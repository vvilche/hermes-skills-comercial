---
name: industrial-tier0-pilot
description: "Methodology for industrial Tier0/UNS digitalization pilots — minimal viable approach, read-only architecture, no DCS intervention, progressive phased roadmap. Includes stakeholder management (skeptical IT), commercial pitch, training course design, and demo lab setup."
version: 1.1.0
author: Hermes Agent (TTung/CONECTA/SUPcon)
tags: [industrial, digitalization, tier0, uns, pilot, iiot, CONECTA, SUPcon]
---

# Industrial Tier0 Pilot — Methodology

## When to Use

Use this skill when the user asks to:
- Start a digitalization pilot at an industrial plant (power plant, refinery, mine, etc.)
- Build a Tier0/UNS dashboard without touching the DCS
- Design a minimal viable approach for industrial data visibility
- Prepare a pitch for a skeptical IT/OT audience
- Design training courses for Tier0, supOS, PRIDE, ECS-700
- Set up a demo lab with SUPCON hardware (ECS-700, instruments, servers)

## BRANDING CRITICO: Tier0 es CONECTA, no SUPcon

**Tier0/UNS es metodologia de CONECTA.** supOS, PRIDE, ECS-700 son productos de SUPcon. No son lo mismo, no se mezclan en el pitch.

- Tier0 = approach de CONECTA: arquitectura, consultoria, ciberseguridad, visibilidad
- supOS/PRIDE = tecnologia de SUPcon: plataforma, monitoreo, AI, DCS

Cuando vendes Tier0, vendes la metodologia CONECTA. La tecnologia SUPcon viene despues si el cliente quiere escalar. No digas "el Tier0 de SUPcon" — no existe. Tier0 es de CONECTA, punto.

**En el pitch:**
- "Nosotros (CONECTA) diseñamos e implementamos la arquitectura Tier0"
- "SUPcon nos provee la plataforma tecnologica (supOS, PRIDE, ECS-700) cuando se necesita"
- "Primero la visibilidad (CONECTA), despues la inteligencia (SUPcon)"

## Demo Deployment on VPS

Las demos se despliegan en el VPS (76.13.239.113) para que el cliente las vea por internet:

```bash
cd /root/ttung/demos/
python3 demo_X.py <port>
```

Reglas:
- Cada demo en su propio puerto (8080, 8081, 8082...)
- Bind a 0.0.0.0 (no 127.0.0.1)
- Sin firewall que bloquee (ufw inactive)
- URL: http://76.13.239.113:<puerto>
- Scripts auto-contenidos (stdlib Python, sin dependencias externas)
- Matar proceso anterior si el puerto esta ocupado: `fuser -k <puerto>/tcp`

## Tier0-Edge Data Integration (UNS Ingest)

**Lección crítica de esta sesión:** La integración de datos en Tier0-Edge requiere entender las DOS capas de flujo: el pipeline MQTT directo (backend consumidor) y el Source Flow (Node-RED). Actúan independientemente.

### Pipeline MQTT Directo (Backend Consumer)

El backend corre un `UnsMessageConsumer` que se suscribe a `$share/uns/#` y recibe TODOS los mensajes MQTT cuyo topic comience con `uns/`. El flujo es:

```
Publisher → EMQX (topic: uns/Guacolda/Metric/unit_load) → UnsMessageConsumer.OnMsg
  → GetDefinitionByPath(topic)      ← busca el file UNS por path exacto
  → parseJsonList(payload)          ← espera JSON: {"value":99.2,"quality":0,"timeStamp":...}
  → procDataAndSendWs(def, data)    ← si Save2Db=true, encola para persistencia
  → UnsQueueDataSinkService.Sink    ← escribe a disco queue
  → persistence()                   ← lee de queue y llama a IPersistentService
  → TimescaleDB                     ← almacena si def.Save2Db=true
```

**Cada paso DEBE funcionar para que los datos lleguen a TimescaleDB.** Si alguno falla, los datos aparecen en `getLastMsg`/websocket pero NO se persisten.

### Formato de payload esperado

El `parseJsonList` espera:
```json
{"value": 99.2, "quality": 0, "timeStamp": 1747184800000}
```

Formato plano — todas las claves se convierten a `map[string]string`. El campo `value` es obligatorio (se mapea al field `value` del modelo UNS).

### Cache negativo (pitfall importante)

`GetDefinitionByPath` cachea los resultados con **ccache** (en memoria, TTL 10-13 min). Si un lookup falla (path no encontrado), guarda un placeholder negativo `int64(-1)` por **2 minutos**. Durante esos 2 minutos, cualquier lookup del mismo path retorna nil sin consultar la DB.

Esto pasa cuando:
1. Se crean files UNS por API mientras el backend está corriendo
2. Un lookup previo ya cacheó "not found" para ese path
3. Se publican datos al MQTT — el consumer no encuentra el file → descarta los datos
4. **Solución:** reiniciar el container `uns` para limpiar el cache, luego publicar un dato fresco

### EMQX REST API

Para publicar datos MQTT por API HTTP en lugar de por cliente MQTT:

```bash
curl -X POST "http://localhost:18083/api/v5/publish" \
  -u "<api_key>:<api_secret>" \
  -H "Content-Type: application/json" \
  -d '{"topic":"uns/Guacolda/Metric/unit_load","payload":"{\"value\":75.5,\"quality\":0,\"timeStamp\":1747184800000}","qos":1}'
```

**Nota:** El `payload` debe ser un string JSON escapado (binary), no un objeto JSON. De lo contrario EMQX responde `"expected: binary()"`.

La API key se configura en `/opt/emqx/etc/default_api_key.conf` del container.

### with_flags bitmask para persistencia

Los files UNS en `supos.uns_namespace` tienen un campo `with_flags` (bitmask):
- `UnsFlagWithFlow = 1` — agregar data acquisition flow
- `UnsFlagWithDashboard = 2` — agregar dashboard
- `UnsFlagWithSave2DB = 4` — **persistir a base de datos**
- `UnsFlagRetainTableWhenDelInstance = 8` — retener tabla al borrar instancia

`with_flags = 4` (solo Save2DB) es el mínimo para persistencia de time-series. Sin esta flag, `Save2Db` es false y los datos se descartan en `procDataAndSendWs`.

### CreateSourceMockFlow — Injector Node-RED (NO es Source Flow)

**Importante:** `CreateSourceMockFlow` NO activa la ingesta de datos desde dispositivos. Crea un **generador de datos mock** que inyecta datos sintéticos a MQTT cada 10s mediante un flow Node-RED (template `relational-emqx.json.tpl`).

```
POST /inter-api/supos/flow/create
Cookie: supos_community_token=...
Body: {"unsAlias": "uns_Guacolda_Metric", "path": "uns/Guacolda/Metric/unit_load"}
```

Requiere que el **modelo** (folder, path_type=0) tenga `fields` definidos en la DB con al menos un campo `value` (DOUBLE). Sin fields, el flow no puede generar el payload. El flow se deployea automáticamente (flowStatus: RUNNING).

### Source Flow REAL (activación manual)

La activación de la ingesta real de datos desde dispositivos requiere hacer click en la UI:
```
UNS > Namespace > seleccionar TOPIC > pestaña Topology > click "Source Flow"
```
No hay endpoint API documentado. El endpoint `/open-api/uns/import` ejecuta un `SourceFlowService` durante el import pero con `spendMills:0` (no crea el flow).

### Lo que SÍ funciona por API

| Operación | Endpoint | Auth |
|-----------|----------|------|
| Login | POST /inter-api/supos/auth/login (body: username/password) | No (devuelve cookie) |
| Import estructura UNS | POST /inter-api/supos/uns/importExport/import (multipart file) | Cookie |
| Crear folder | POST /open-api/uns/folder | No |
| Crear file/tag | POST /open-api/uns/file | No |
| Batch delete | DELETE /open-api/uns/batch/alias | No |
| Publicar MQTT | POST http://localhost:18083/api/v5/publish (auth básica) | API Key |

### Auth para endpoints internos

El login devuelve una cookie `supos_community_token` que se usa para endpoints bajo `/inter-api/supos/`. NO es JWT/Bearer, es cookie de sesión. Ejemplo:

```bash
# Login
curl -c cookies.txt -X POST http://localhost:18998/inter-api/supos/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"username":"tier0","password":"tier0"}'

# Usar cookie
curl -b cookies.txt http://localhost:18998/inter-api/supos/... 
```

### Estructura UNS correcta para time-series

El namespace debe tener raíz `uns/` para que el topic MQTT matchee la suscripción del backend (`$share/uns/#`):

```
uns/ (PATH)
  └── Guacolda/ (PATH)
       └── Metric/ (PATH, topicType: METRIC)
            ├── unit_load (TOPIC, fields: [{name:"value", type:"DOUBLE"}], enableHistory: "TRUE")
            ├── power (TOPIC, ...)
            └── ...
```

El topic MQTT es `uns/Guacolda/Metric/unit_load` y DEBE coincidir exactamente con el path autogenerado del file.

### Import de estructura vía API

```bash
curl -X POST "http://localhost:18998/inter-api/supos/uns/importExport/import" \
  -H "Cookie: supos_community_token=..." \
  -F "file=@estructura.json"
```

El JSON sigue el formato de `uns-structure` skill: PATH/TOPIC hierarchy con topicType METRIC, enableHistory "TRUE", fields array. Ver referencia `tier0-edge-data-ingest.md` para ejemplo completo.

### Lo que NO se puede hacer por API

- **Activar Source Flow:** Solo desde la UI: UNS > Namespace > seleccionar TOPIC > Topology > click "Source Flow". No hay endpoint API documentado. SourceFlowService se ejecuta durante el import pero con spendMills:0 (no hace nada).
- **Conectar Node-RED automáticamente:** El Source Flow generado automáticamente requiere el click en la UI.
- **Hacer que los datos aparezcan en el frontend sin ese click:** No se ha encontrado forma. 9 intentos distintos fallaron.

### Servicios Docker

| Container | Host Port |
|-----------|-----------|
| kong | 8088 (gateway) |
| frontend | 3010 |
| uns (backend) | 18998 |
| emqx (MQTT) | 1883, 18083 (dashboard) |
| nodered | 1880 |
| eventflow | 1889 |
| postgresql | 5432 |
| tsdb (TimescaleDB) | 2345 |
| portainer | 9443 |

## Core Principles

1. **No DCS intervention** — read-only via existing systems (PI System, OPC, CSV exports). Zero risk, zero downtime.
2. **Minimum viable first** — start with a single PC + script + dashboard HTML. Add gateways, servers, AI only after validation.
3. **Fases progresivas** — F1: software only (weeks), F2: PRIDE+supOS+IA (months), F3: escalamiento (year+).
4. **Darle la razon al TI** — ciberseguridad es el argumento principal. Lectura pasiva, aislada, sin modificar la red OT.
5. **El CSV es tu mejor amigo** — para la primera demo, un export de 20-30 tags del PI System es suficiente. El cliente lo consigue en 5 minutos.

## Arquitectura Tipica (F1 — MVP)

```
PI System (existente)  ← DCS (no se toca)
    ↓ API/ODBC/OPC
PC Industrial (Edge)
    ↓ Python script → JSON REST API
Dashboard HTML (Tier0/UNS)
    ↓ (futuro)
supOS + PRIDE (Fase 2)
```

## Workflow para un Piloto

### Paso 1: Entender la planta
- Identificar fuentes de datos disponibles (PI System, OPC server, historicos, CSV exports)
- Identificar stakeholder clave: operaciones, mantenimiento, TI
- Especial atencion al **experto TI** — preparar argumento de ciberseguridad
- Revisar si hay P&IDs, I/O lists, o documentacion previa (usar INVENTARIO.md)

### Paso 2: Definir alcance minimo
- Elegir UNA unidad/un equipo/un area como piloto
- Elegir 20-30 tags criticos (carga, presion, temp, eficiencia, vibracion, emisiones)
- Definir KPIs: carga, potencia, eficiencia, alarmas, tendencias

### Paso 3: Construir MVP en 90 min
- Script Python (ver referencia `guacolda-tier0-case.md`)
- Dashboard HTML con Chart.js
- Modo demo (datos simulados) + modo real (lectura CSV/PI)
- Verificar que el dashboard se vea bien y responda en <2s

### Paso 4: Presentar al cliente
- Mostrar primero en modo demo (datos simulados)
- Despues con datos reales (cargar CSV)
- Enfatizar: "Sin tocar su DCS, sin comprar nada, sin instalar nada"
- Proximo paso: "Mandeme un CSV con sus tags y en 5 min lo vemos"

### Paso 5: Roadmap
- Definir F1 (Tier0), F2 (PRIDE+supOS+IA), F3 (escalamiento)
- Tiempos tipicos: F1=semanas, F2=3-6 meses, F3=12-18 meses

## Manejo de Stakeholders

### Experto TI — Reticente a ciberseguridad
- Darle la razon: "Tiene toda la razon, por eso NO tocamos el DCS"
- Arquitectura: solo lectura, datos del PI System (que ya existe en red IT)
- Sin puertos abiertos, sin software en OT, sin riesgo
- Demostrar con el diagrama de arquitectura antes de prender el computador

### Operaciones — Quieren ver resultados
- Demo en vivo con datos reales
- Dashboard visual, KPIS claros, trends
- "Esto es lo que siempre han querido ver y nadie les ha podido mostrar"

### Gerencia — Quieren entender el valor
- Enfocar en: bajo riesgo, bajo costo, resultado inmediato
- Roadmap claro hacia IA sin comprometer la operacion
- Pitch: "En medio dia tiene un dashboard funcionando. Que pierde?"

## Estructura de Curso Tier0 (4 horas)

| Modulo | Duracion | Contenido |
|--------|----------|-----------|
| M1: Que es Tier0/UNS | 45 min | Modelo UNS, casos de uso, ciberseguridad, costo/beneficio |
| M2: Arquitectura | 60 min | Pipeline datos, protocolos, diagrama en vivo |
| M3: Hands-On | 90 min | 7 pasos: entorno → demo → API → customizar → CSV → analisis → presentar |
| M4: Datos Reales + Next | 45 min | PI System, roadmap F1→F2→F3 |

Cada alumno se lleva un dashboard funcionando y un plan de implementacion.

## Plan de Estudio SUPcon = Skills System

Victor confirmó que el plan de estudio de SUPcon **son los skills mismos**. Cada skill es un módulo de aprendizaje auto-contenido:

| Skill | Tipo | Contenido |
|-------|------|-----------|
| `industrial-mqtt-simulator` | Técnico | Simulación MQTT Python, multi-dispositivo, UNS |
| `industrial-dashboard-generator` | Técnico | HTML dashboard, MQTT WebSocket, REST fallback |
| `industrial-digitalization-kickoff` | Estrategia | Metodología propuestas, branding CONECTA vs SUPcon |
| `industrial-tier0-pilot` (este) | Metodología | Curso Tier0, stakeholders, pitch |
| `cement-plant-simulator` | Técnico | Simulación cemento: horno, molino, enfriador |
| `enapac-plant-simulator` | Técnico | Simulación ENAPAC: 8 estaciones, KPIs, APEX+TPT |
| `guacolda-tier0-simulator` | Técnico | Planta térmica Tier0, REST + dashboard |
| `pipeline-hydraulics-sim` | Ingeniería | Darcy-Weisbach, bombas, water hammer |
| `ttung-design-system` | Diseño | Paleta, tipografía, cards, slides |
| `powerpoint` | Presentaciones | PPTs, HTML slides, propuestas visuales |
| `partnership-plans` | Estrategia | Alianzas Noatec, RSM, partners Perú |

**No hay un documento separado de "plan de estudio". El plan de estudio ES el sistema de skills.**

## Curso Siguiente Sugerido
Despues de Tier0: **supOS — Plataforma Industrial IoT** (16h, 2 dias). Conectar X-Collector, modelar planta en objetos, DataLake, apps low-code.

## Diseno de Laboratorio Demo

Usar hardware existente (ej: PO #P01309):

**Rack ECS-700:** Controller redundante, I/O, APL switches, serial comm
**Mesa Demo:** Tanque (radar nivel), skid (presion), loop (flujo), motor (vibracion inalambrica), valvulas, calibrador HART
**Rack IT:** PRIDE Server, Data App Server, Workstation, Monitores

Ver referencia `guacolda-tier0-case.md` para detalle de PO y hardware.

## Tier0-Edge AI/LLM Integration

El frontend de Tier0-Edge incluye un chatbot AI y el App GUI (generación de apps con lenguaje natural). Esto se maneja mediante **services-express**, un servicio Node.js que corre DENTRO del container `frontend`.

### AI Bridge: Container vs Host

Hay DOS formas de conectar el chatbot AI al LLM:

**Opcion A — Nativa (services-express en container frontend):**
- `LLM_API_KEY` en deploy/.env → container `frontend` arranca services-express
- Funciona si la key es valida y el container `frontend` tiene el startup completo
- **Problema común:** La key se pierde al recrear containers (queda como `***` o placeholder)

**Opcion B — Host bridge (recomendado para debug/demos):**
- Script Python en host (`ai_bridge_host.py`) que proxy HTTP a DeepSeek/Ollama
- Corre en puerto 4001 directamente en el VPS
- No requiere Docker, no tiene problemas de networking
- Solo necesita la key en el archivo, no en variables de entorno
- Ver referencia `references/ai-bridge-deployment.md`

**Cuando la opcion A falla**, pasar directamente a opcion B. No debuggear el container services-express — el startup del container `frontend` es complicado (concurrently con serve + node) y los errores de Node.js (zod/v3, CopilotKit) son dificiles de resolver sin rebuildear la imagen.

### Configuración

El container `frontend` recibe estas variables de entorno para activar el AI:

```yaml
LLM_API_KEY=<api_key_real>   # Si está vacío o es "***", el AI NO arranca
LLM_TYPE=deepseek            # deepseek | openai | ollama, etc.
LLM_MODEL=deepseek-chat
```

El startup del container verifica:
```bash
if [ -z "$LLM_API_KEY" ]; then
    echo "LLM_API_KEY vacío, salta servicios AI"
    serve -s /app/web-dist -l 3000    # solo frontend
else
    echo "LLM_API_KEY configurado, arranca todo"
    concurrently "serve -s /app/web-dist -l 3000" "node /app/services-express-dist/index.js"
fi
```

### Arquitectura

```
Frontend (React SPA)
  ↓ solicita AI asistente
services-express (Node.js, puerto interno 3001)
  ↓ HTTP a LLM API externa
DeepSeek / OpenAI / Ollama (según LLM_TYPE)
```

El endpoint `/api/chat` dentro del container frontend recibe las consultas del chatbot UI y las reenvía al LLM configurado. No hay un puerto expuesto público para services-express — solo el frontend React lo consume internamente.

### WhatsApp/External Bridge (concepto)

Para conectar el AI de Tier0-Edge a WhatsApp, la arquitectura sería:

```
WhatsApp → Hermes Gateway → script bridge → Tier0 API (/api/chat) → services-express → DeepSeek
                                                                              ↓ respuesta
WhatsApp ← Hermes Gateway ← script bridge ← Tier0 API ← services-express ← DeepSeek
```

Esto requiere configurar `LLM_API_KEY` con una clave válida (no `***`) y reiniciar el container.

### Verificación

```bash
docker exec frontend ps aux | grep services-express
docker logs frontend | grep -i "llm\\|chat\\|ai"
```

Para deploy del bridge en host, ver `templates/ai_bridge_host.py` y `references/ai-bridge-deployment.md` en este skill.

## Grafana Data Visualization (TTung deployment)

Los archivos de provisionamiento de Grafana están en `/root/ttung/grafana/`:

| Archivo | Propósito |
|---------|-----------|
| `datasources/datasource.yaml` | Datasource TimescaleDB (host: tsdb:5432, user: postgres, sslmode: disable, timescaledb: true) |
| `dashboards/guacolda_overview.json` | Dashboard "Guacolda U3 - Overview" con 8 paneles de time-series |

Puerto: **3001** (3000 ocupado por services-express).

## Pitfalls

- **NO preguntar, solo hacer.** Cuando Victor dice "hazlo", "yapo", "resuelve" o se queja de que preguntas mucho — la respuesta es ejecutar, no preguntar más. No pedir confirmación, no proponer opciones, no explicar alternativas. Las preguntas solo cuando algo TÉCNICAMENTE no se puede resolver sin más info. La preferencia de estilo es: acción inmediata, sin rodeos, sin pasos intermedios que requieran su aprobación.
- **No iterar demasiado** — si algo no funciona en 2-3 intentos, parar y preguntar. No debuggear hasta el infinito.
- **NO hacer prueba y error** — antes de tocar cualquier configuración de la plataforma, leer la documentación completa: README.md, skills/ relevantes, docs online. Victor es explícito: "estudia antes de hacer webadas", "lee la documentacion". Cada intento fallido sin leer primero es tiempo perdido y frustración. Si no hay documentación, preguntar. No hay excepción.
- **No pedirle a Victor que haga clicks en la UI** — "para que diantres estas tu". Si una configuración solo se puede hacer desde la interfaz gráfica, buscar la API equivalente o automatizarla. Mandarlo a hacer clicks manuales no es opción.
- **No planificar semanas de trabajo** — Victor prefiere resultados inmediatos. Si propones un plan de 2 semanas, te va a decir "me voy a demorar semanas?" Hacer las cosas una por una, mostrar cada demo funcionando antes de pasar a la siguiente.
- **Entregar una por una** — cuando hay múltiples demos o entregables, no hacer todo junto. Hacer la primera, mostrar que funciona, preguntar "sigo con la siguiente?"
- **Terminal timeout frecuente** — comandos con `&` background, `curl`, `sleep`, o scripts largos suelen timeout. Usar `execute_code` para comandos Python simples, o lanzar procesos background con `background=true` y monitorear con `process(action='wait')`.
- **No ofrecer arquitectura compleja primero** — siempre empezar por lo minimo: PC + script + CSV
- **Excalidraw a veces falla** — HTML es mas confiable para compartir diagramas; el JSON de excalidraw puede tener problemas de formato
- **No decir "no encontre [X]"** — primero hacer `ls -laR` completo de la carpeta del proyecto y crear INVENTARIO.md
- **No prometer PPTX y entregar HTML sin preguntar** — preguntar primero: "¿Lo quieres en HTML para verlo ya, o en PPTX para editar?"
- **AI Bridge container puede fallar silenciosamente:** container aparece "Up X minutes" pero no responde HTTP ni acepta `docker exec` commands (timeout). Sin logs, sin errores. **No debuggear el container** — pasar a ejecucion directa en host con `python3 /root/ttung/ai_bridge_host.py <puerto>`. El script host es Python stdlib puro, sin dependencias, y funciona consistentemente.\n- **Cache negativo de GetDefinitionByPath (Tier0-Edge):** Si el backend no encuentra un file UNS por path, cachea `int64(-1)` por 2 minutos en memoria. Durante ese lapso, mensajes MQTT entrantes se descartan del pipeline de persistencia (van solo a websocket). No asumas que porque el file existe en DB, el backend lo encuentra — el cache puede estar bloqueando. **Solución:** reiniciar container `uns` y publicar 1 dato fresco inmediatamente. Ver `references/tier0-edge-cache-debug.md`.
- **CreateSourceMockFlow genera datos, NO consume:** El endpoint `POST /inter-api/supos/flow/create` crea un flow Node-RED que inyecta datos sintéticos cada 10s a MQTT usando el template `relational-emqx.json.tpl`. No es un Source Flow de ingesta real. El modelo UNS (folder) debe tener `fields` definidos o el flow falla. No confundir con la activación manual de Source Flow en la UI.
- **Payload MQTT por REST API debe ser string escapado:** La API de EMQX espera payload como string JSON escapado, no como objeto. Si pasas objeto, responde error.
- **SINK_TSDB_URL vacio: datos no persisten:** Tier0-Edge usa DOS bases: postgresql (metadata) y tsdb (time-series). La env var SINK_TSDB_URL debe estar en deploy/.env o el TsdbPersistentService no se inicializa. Datos aparecen en getLastMsg pero tabla vacia. Ver references/tier0-edge-two-db-arch.md.
- **sslmode=disable requerido:** tsdb no soporta TLS. Connection string DEBE incluir `?sslmode=disable`.
- **Docker run para uns necesita -p 18998:8080:** Si recreas uns manualmente, mapea el puerto o Kong no conecta.

## Referencias

- `references/ai-bridge-deployment.md` — Deployment patterns for AI bridge in Tier0 (container vs host), pitfalls with key management and Docker networking.
- `references/tier0-edge-cache-debug.md` — Diagnóstico detallado del cache negativo de `GetDefinitionByPath`, flujo completo de persistencia de datos MQTT→TimescaleDB, y comandos de verificación. Referencia técnica primaria para debug de ingesta.
- `references/tier0-edge-two-db-arch.md` — Arquitectura de dos bases de datos PostgreSQL/TimescaleDB, configuración de `SINK_TSDB_URL`, lazy init del `TsdbPersistentService`, y solución para falla de persistencia sin errores visibles.
- `references/guacolda-tier0-case.md` — Caso completo Guacolda: PO #P01309, arquitectura, scripts, dashboard, y lecciones aprendidas. Usar como plantilla para nuevos proyectos.
- `references/tier0-edge-data-ingest.md` — Formato de import de estructura UNS vía API (JSON de PATH/TOPIC hierarchy).
- `references/piloto-estructura.md` — Plantilla de estructura para curso Tier0

## Tier0-Edge Language Fix (Chinese → English)

Cuando el frontend de Tier0-Edge se ve en chino (idioma por defecto), hay dos ubicaciones de archivos de traducción en el container `uns`:

| Archivo original (chino) | Archivo inglés | Ubicación en container |
|---|---|---|
| zh_CN.json | en_US.json | `/app/etc/i18n/` |
| zh-CN.json | en-US.json | `/app/go-edge/system/resource/supos/` |

**Fix:**
```bash
docker exec uns cp /app/etc/i18n/zh_CN.json /app/etc/i18n/zh_CN.json.bak
docker exec uns cp /app/etc/i18n/en_US.json /app/etc/i18n/zh_CN.json
docker exec uns cp /app/go-edge/system/resource/supos/zh-CN.json /app/go-edge/system/resource/supos/zh-CN.json.bak
docker exec uns cp /app/go-edge/system/resource/supos/en-US.json /app/go-edge/system/resource/supos/zh-CN.json
docker restart uns
```

El frontend pide `zh-CN.json` basado en configuración de idioma (el frontend tiene `REACT_APP_OS_LANG=en-US` pero carga chino por el archivo). Haciendo que `zh-CN.json` contenga traducciones en inglés, el sistema se ve en inglés sin modificar el código.

## Tier0-Edge i18n — Full Spanish Translation Workflow

Cuando se necesita traducir TODO el frontend de Tier0-Edge del chino/inglés al español, hay que traducir los archivos JSON de i18n en el container `uns`. La plataforma tiene DOS archivos separados en ubicaciones distintas:

| Archivo | Keys | Propósito | Ubicación en container |
|---------|------|-----------|----------------------|
| `en-US.json` | ~800 | Strings del sistema/backend | `/app/go-edge/system/resource/supos/` |
| `en_US.json` | ~1900 | Strings principales de la UI | `/app/etc/i18n/` |

**Workflow comprobado (no usar diccionario Python solo ni subagentes con JSON grande):**

### Paso 1: Extraer archivos originales en inglés
```bash
docker exec uns cat /app/go-edge/system/resource/supos/en-US.json > /tmp/en_supos.json
docker exec uns cat /app/etc/i18n/en_US.json > /tmp/en_etc.json
```

### Paso 2: Crear archivo plano con TODOS los values a traducir
```python
import json
# Extraer todos los values que no son placeholders
vals = []
for d_en in [supos_en, etc_en]:
    for k, v in d_en.items():
        if isinstance(v, str) and len(v) > 4:
            vals.append(v)
with open('/tmp/to_translate.txt', 'w') as f:
    for v in vals:
        f.write(v + '\n')
```

### Paso 3: Subagente traduce archivo plano (línea por línea)
Usar `delegate_task()` con el archivo plano (~1500-2000 líneas, ~50KB). Cada línea se traduce individualmente manteniendo el orden. **NO pasar el JSON directamente** — los subagentes hacen timeout con archivos JSON de más de 500 keys.

```
Prompt: "Traduce CADA línea al español (es-CL / chileno). 
         Mantén orden exacto. NO traduzcas términos técnicos: 
         MQTT, JSON, URL, UUID, SQL, GraphQL, Node-RED, API, etc.
         Placeholders {variable}, %s, %v intactos."
```

El subagente procesa ~20-25KB por llamada API. Para 2000 líneas (~50KB), tarda ~200s y ~10 llamadas API.

### Paso 4: Mapear traducciones de vuelta al JSON
```python
# untra_keys = lista ordenada de (prefix, key) que estaban sin traducir
# translated_lines = output del subagente (mismo orden)
for (prefix, key), translation in zip(untra_keys, translated_lines):
    if prefix == "supos":
        supos_es[key] = translation
    else:
        etc_es[key] = translation
```

### Paso 5: Copiar al container y reiniciar
```bash
# Backup de originales
docker exec uns cp /app/etc/i18n/zh_CN.json /app/etc/i18n/zh_CN.json.bak
docker exec uns cp /app/go-edge/system/resource/supos/zh-CN.json /app/go-edge/system/resource/supos/zh-CN.json.bak

# Copiar español a TODAS las 4 ubicaciones
docker cp /tmp/es_supos_full.json uns:/app/go-edge/system/resource/supos/en-US.json
docker cp /tmp/es_supos_full.json uns:/app/go-edge/system/resource/supos/zh-CN.json
docker cp /tmp/es_etc_full.json uns:/app/etc/i18n/en_US.json
docker cp /tmp/es_etc_full.json uns:/app/etc/i18n/zh_CN.json

# Reiniciar backend Y frontend para refrescar cache
docker restart uns frontend
```

**Importante:** Tanto `en-US` como `zh-CN` deben contener el mismo contenido en español. El frontend pide `zh-CN.json` pero también usa `en-US.json`. Cubriendo ambos archivos se asegura que cualquier configuración de idioma muestre español.

### Pitfalls
- **No usar diccionario Python solo para traducir:** Se necesitan ~2000+ entradas para cubrir todas las strings específicas de SUPcon (namespaces, tooltips, errores de Portainer, etc.). El diccionario solo funciona para términos genéricos.
- **No pasar JSON al subagente:** El orden de las keys en un JSON al serializar/deserializar puede cambiar. Usar archivo plano con un valor por línea para preservar orden exacto.
- **Timeout de subagentes con JSON >20KB:** Dividir en partes de ~300-400 keys si es necesario. El archivo plano funciona hasta ~2000 líneas (~50KB).
- **Frontend cachea traducciones:** React SPA cachea en memoria. Reiniciar el container `frontend` es obligatorio. Solo reiniciar `uns` no es suficiente.
- **Posible desalineación:** Si alguna línea se salta en la traducción, las traducciones posteriores quedan desalineadas. Verificar con `len(translated) == len(keys_to_update)` y revisar una muestra aleatoria.

## Direct TSDB Inserter (Persistence Bypass)

Cuando el backend de Tier0-Edge recibe datos via MQTT (getLastMsg funciona) pero NO los persiste en TimescaleDB a pesar de tener `SINK_TSDB_URL` configurada y `TsdbPersistentService` inicializado, un workaround directo es insertar los datos via script Python que escribe directamente a la tabla time-series:

```python
import json, time, paho.mqtt.client as mqtt
import psycopg2

# Connection
conn = psycopg2.connect("postgresql://postgres:supcon2024@tsdb:5432/postgres?sslmode=disable")
cur = conn.cursor()

TAGS = {
    2054734405385064457: "unit_load",
    # ... mapear tag IDs a nombres
}

def on_message(client, userdata, msg):
    topic = msg.topic
    payload = json.loads(msg.payload)
    ts = payload.get("timeStamp", int(time.time() * 1000))
    val = payload.get("value", 0)
    tag_id = payload.get("tagId", extract_tag_from_topic(topic))
    
    cur.execute(
        "INSERT INTO supos_timeserial_double (tag, \"timeStamp\", quality, value) VALUES (%s, to_timestamp(%s/1000.0), %s, %s)",
        (tag_id, ts, 192, val)
    )
    conn.commit()

client = mqtt.Client()
client.on_message = on_message
client.connect("emqx", 1883, 60)
client.subscribe("uns/#")
client.loop_forever()
```

**Tablas disponibles en tsdb:**
- `supos_timeserial_double`: tabla primaria PostgreSQL, columnas: `tag` (bigint), `timeStamp` (timestamptz), `quality` (bigint), `value` (double)
- `uns_timeserial`: tabla alterna creada para pruebas, misma estructura pero nombre diferente que el código espera en algunos paths

**Nota:** La columna `timeStamp` tiene `S` mayúscula en PostgreSQL, necesita comillas dobles en SQL.

## Publisher Loop Pattern (Long-Running)

Para mantener datos fluyendo de forma continua por horas/días:

```bash
# Script publisher que corre en background:
python3 /tmp/publisher_loop.py
```

El patrón comprobado publica 29 tags cada 5s via MQTT a `uns/Guacolda/Metric/{tag_name}` con payload `{"value": X, "quality": 0, "timeStamp": timestamp_ms}`. Ha estado corriendo por 10+ horas sin fallo.

## Grafana Data Visualization

Para visualizar datos de TimescaleDB, levantar Grafana con provisionamiento automático:

```bash
# Configuración de datasource (datasources.yaml)
docker run -d --name grafana --network tier0_edge_network -p 3001:3000 \
  -e GF_SECURITY_ADMIN_USER=admin \
  -e GF_SECURITY_ADMIN_PASSWORD=admin \
  -e GF_AUTH_ANONYMOUS_ENABLED=true \
  -v /root/ttung/grafana/datasources:/etc/grafana/provisioning/datasources \
  -v /root/ttung/grafana/dashboards:/var/lib/grafana/dashboards \
  -v /root/ttung/grafana/dashboards:/etc/grafana/provisioning/dashboards \
  grafana/grafana:latest
```

**Datasource provisioning YAML:**
```yaml
apiVersion: 1
datasources:
  - name: TimescaleDB
    type: postgres
    url: tsdb:5432
    user: postgres
    secureJsonData:
      password: supcon2024
    jsonData:
      database: postgres
      sslmode: disable
      postgresVersion: 1600
      timescaledb: true
```

**Dashboard provisioning JSON:** Dashboard con paneles timeseries, raw SQL queries con `$__timeFilter("timeStamp")` para el rango de tiempo. Los tags se identifican por su ID bigint en la tabla `supos_timeserial_double`.

Para subir dashboards via API tras el provisionamiento:
```bash
curl -X POST http://admin:admin@localhost:3001/api/dashboards/db \
  -H "Content-Type: application/json" \
  -d '{"dashboard": <dashboard_json>, "overwrite": true, "folderUid": "<folder_uid>"}'
```

**Nota:** Si el usuario admin no tiene permisos (error "Access denied" con anonymous enabled), reiniciar el container carga la configuración de provisionamiento automáticamente.

## Related Files in User's Project
Para implementar un piloto Tier0 desde cero:
1. Crear `guacolda_tier0.py` — script servidor + datos (Python stdlib, sin dependencias)
2. Crear `dashboard-[planta]-u[N].html` — dashboard con Chart.js, 8 pestanas, 7 KPIs, 6 graficos
3. Crear `ejemplo_export_pi.csv` — CSV de ejemplo con 29 tags
4. Crear `curso_tier0_presentacion.html` — slides del curso en HTML
5. Crear `laboratorio_demo_[proyecto].html` — layout del laboratorio demo
