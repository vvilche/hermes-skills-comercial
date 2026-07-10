---
name: ttung-projects
description: Resumen de todos los proyectos activos de TTung
---

# TTung Projects

## ⚠️ Preferencias de Victor (CRÍTICAS — leer siempre antes de trabajar)

### Planificación estratégica para Victor (CEO)
- Cuando Victor pide "estrategia", **NO analices ni critiques planes que él ya tiene**. Él ya los conoce. Construye desde cero con datos operacionales concretos.
- **NO des teoría conceptual** ("tesis de empresa", "visión transformacional", "análisis de gaps"). Victor corrigió: "na na.. es mas profundo", "creo que aun no vamos por buen camino".
- **Lo que Victor QUIERE en una estrategia:**
  - Headcount año por año (cuántas personas, en qué ciudades)
  - Revenue por región (Santiago, Antofagasta, Concepción, Magallanes, Perú)
  - Expansión geográfica con año exacto y costo de apertura
  - Línea de tiempo año por año durante 10 años
  - Modelo de ventas con % de conversión y tickets promedios
  - Gestión de capacidad (proyectos por técnico, umbrales de saturación)
  - Checklist próximos 90 días con acciones concretas
  - Riesgos con planes B
- **Formato final: HTML auto-contenido** con timeline visual, tablas, métricas — no solo markdown. Victor confirmó con "aja" cuando se le entregó este formato.
- El plan estratégico existente de CONECTA está en `/root/TTung/plan_crecimiento_2026_2031/`. Contiene TAE Comercial, Operaciones, Roles, NeuroNegociación. No repetir — construir sobre eso.
- Datos de mercado disponibles: `/root/TTung/Mapla_2026/` (tendencias Mapla-Mantemin), `/root/TTung/Paul_Constancio_GeoMarketing/` (geomarketing).
- Roadmap generado: `http://76.13.239.113:8005/roadmap_conecta_2026_2036.html`

### Contenido para ingenieros ("cuadrados")
- Cuando el público objetivo son **ingenieros técnicos**, el contenido debe ser **paso a paso exacto**, no conceptual.
- Nada de "lógica del diseño" o "separación lógica". Dar instrucciones precisas: "Conecta el PLC X al Node-RED Y en el puerto Z usando protocolo W, publica en topic V".
- Victor corrigió esto explícitamente: "recuerda que son cuadrados" — necesitan comandos, puertos, protocolos, no teoría.

### Capturas de pantalla / Screenshots
- Cuando Victor pide "fotos" o "manda fots", tomar screenshots de **lo que SÍ funciona**, no de lo que está roto.
- Si hay problema, mostrar primero el working state (el overview con tráfico, el árbol expandido, el dashboard con datos) y luego explicar qué falta.
- Victor se frustra si se muestran pantallas vacías o rotas primero — quiere confirmación visual de que el sistema opera antes de ver lo que falta.
- Usar Playwright para screenshots automáticos: login completo + árbol expandido + dashboard + secciones relevantes.
- Script base en `scripts/capturar_pantallas_tier0.py`.

### Diseño de HTML/Presentaciones
- **SIEMPRE modo claro (fondo blanco/color claro)**. Victor dijo "el diseño está muy re oscuro no veo una mierda" y hubo que convertir todo de dark mode a light mode.
- Paleta light: fondo `#f5f5f8`, texto `#1a1a2e`, header degradado `#00b0f0 → #6c5ce7`, nav `#e8e8f0`, tablas con th `#e8e8f4`, code `#f0f0f6` con texto `#c0392b`.
- HTML auto-contenido sin dependencias externas.
- Secciones expandibles por defecto.

### Capas del stack SUPcon — Aclaración lógica vs física
- Victor cuestionó por qué Node-RED (SourceFlow) está en Capa 1 y no en Tier 0 si físicamente corre en el mismo servidor.
- **Explicación correcta:** La separación por capas es **conceptual** (Tier 0 = plataforma digital, Capa 1 = conectividad/traducción de protocolos). Físicamente pueden correr en el mismo servidor (escenario SMB) o en edge gateways separados (escenario grande).
- Si Victor o sus ingenieros se confunden, **fusionar las capas 0-1** en la documentación como "Plataforma Unificada de Infraestructura + Integración".

## SUPcon Software — Guías Generadas (Mayo 2026)

### Archivos disponibles (Mayo 2026)

| Archivo | Descripción | Link servido |
|---------|-------------|--------------|
| `02_Presentaciones/supcon_software_completo.html` | 25 softwares SUPcon en detalle: qué es, capacidades, venta, sectores | http://76.13.239.113/supcon_software_completo.html |
| `02_Presentaciones/supcon_integracion_tecnica.html` | Integración técnica: topología, data flow, protocolos, puertos, queries SQL, configs Docker | http://76.13.239.113/supcon_integracion_tecnica.html |
| `02_Presentaciones/supcon_software_deep_dive.md` | Markdown completo de los 25 softwares (741 líneas) | Archivo local |
| `02_Presentaciones/supcon_stack_completo.md` | Mapa de capas versión markdown | Archivo local |
| `02_Presentaciones/supcon_software_stack_map.html` | Mapa interactivo de capas (versión anterior) | Archivo local |
| `02_Presentaciones/tier0_instalacion_a_prueba_de_balas.html` | Manual de instalación bulletproof — paso a paso, parches, problemas conocidos, checklist | http://76.13.239.113/tier0_instalacion_a_prueba_de_balas.html |
| `tier0_agent_install.py` | Tier0 Installer Agent — script Python 5 capas (pre-flight/setup/install/health/report) | http://76.13.239.113/tier0_agent_install.py |
| `tier0_test_agent.py` | Tier0 Test Agent Nivel 1 — 7 tests infraestructura (login/namespace/mqtt/tsdb/grafana/export) — resultado 7/7 | http://76.13.239.113/tier0_test_agent.py |
| `tier0_test_agent_level2.py` | Tier0 Test Agent Nivel 2 — 4 tests plataforma (Chat IA/dashboards/SourceFlow real/EventFlow) — resultado 3/4 (Chat IA necesita API key) | http://76.13.239.113/tier0_test_agent_level2.py |
| `test_report.html` | Reporte de prueba Nivel 1 — 7/7 tests pasados | http://76.13.239.113/test_report.html |
| `test_report_level2.html` | Reporte de prueba Nivel 2 — 3/4 tests (Chat IA necesita key) | http://76.13.239.113/test_report_level2.html |

### Knowledge Base original (624 archivos)
- **NO están en el VPS.** El directorio `/root/TTung/knowledge_base/` está VACÍO.
- Los archivos están en el Mac de Victor (donde se configuró el MCP server).
- Para usar `search_knowledge_base` hay que copiarlos via SCP.
- Mientras tanto, usar los documentos locales en `ESTRATEGIA_SUPCON/` y `02_Presentaciones/`.

### Documentos de estrategia disponibles
| Archivo | Contenido |
|---------|-----------|
| `ESTRATEGIA_SUPCON/supcon_software_suite_guide.md` | Stack completo: TPT → PRIDE → APC → Tier0 |
| `ESTRATEGIA_SUPCON/supcon_tier_zero_software_guide.md` | supOS, InPlant PID/LIMS/EAM/OMS |
| `ESTRATEGIA_SUPCON/supcon_knowledge_map.md` | 6 pilares: DCS/SIS, Instrumentación, Software Avanzado, Verticales, Comercial, Robótica |
| `ESTRATEGIA_SUPCON/supcon_vertical_playbook.md` | Estrategia por industria |
| `ESTRATEGIA_SUPCON/supcon_roi_and_experiences.md` | Casos de éxito y ROI |

### Para servir HTML
```bash
cd /root/ttung/02_Presentaciones && python3 -m http.server 80
```
(Puerto 80 ya está abierto en UFW. Los archivos se sirven en http://76.13.239.113/<filename>)

---

## Enfoque Venta Tier0 (Brownfield vs Greenfield)
- **Brownfield (plantas existentes, ej: Guacolda)**: datos ya estandarizados en DCS/PI, Tier0 suma poco valor incremental. Vender directo PRIDE/TPT (capas superiores: deteccion anomalias, predictivo).
- **Greenfield (proyectos nuevos)**: Tier0 es ahorro enorme — namespace definido desde ingenieria basica, evita N integraciones inseguras. Vender desde la arquitectura.

## Guacolda
- Central termica 5 unidades, DCS Mitsubishi, PI Historian
- Arquitectura 5 capas (menos→mas invasivo): Tier0→PRIDE→TPT→OMS→APC
- Demo Tier0 en VPS, datos simulados MQTT → TimescaleDB
- Grafana dashboard: 8 paneles, puerto 3001
- Publicador MQTT: ~12000+ publicaciones, fluyendo continuo
- Directorio: ~/TTung/Guacolda/

## CONECTA Operaciones IA
- Sistema multiagente para automatizar traspaso Odoo→proyectos
- Orquestador + 4 subagentes: Admin, Ingenieria, Prevencion, Logistica
- 7 proyectos reales en ~/TTung/MaterialIAProyectos/
| `Tier0 Intro.pdf` | Introducción Tier0 | — |
- `references/plan-estudios-hermes-principiantes.md` — Plan de estudios Hermes Agent para principiantes (4 semanas)
- `references/whatsapp-gateway-troubleshooting.md` — Diagnóstico y reparación del enlace WhatsApp Bridge↔Gateway: doble capa autorización, systemd override, LID mapping
| `references/estudio-mercado-supcon-3-lineas.md` | SUPcon: Field Instrumentation · Software · Control Systems — pricing real de proyectos | — |
| `references/estudio-conecta-nexus-scada-cen-monitoreo.md` | CONECTA: SCADA · CEN/PMU · Monitoreo — precios reales CGE y Barrick | — |
| `references/plan-marketing-digital-landing-pages.md` | Marketing Digital: landing pages + SEO + leads para SUPcon+CONECTA | — |

## Estudios Generados (Junio 2026) — Formato de referencia

⚠️ **Victor usa numeros como shorthand:** "la 5" = los 5 Agentes IA, "canvas" = diagrama Excalidraw, "interfaz web" = dashboard Kanban de Hermes. Siempre aclarar antes de asumir.

### AGENT-SCADA — Prototipo Funcional
- Código: `/root/ttung/01_Demos/agent_scada_proto.py`
- Se conecta a EMQX (MQTT localhost:1883), se suscribe a `v1/Guacolda/+/Metric/+`
- Detecta anomalías por umbrales configurables (Presion, Temperatura, Vibracion, RPM)
- Expone estado vía `get_status()` (métrica counts, alarmas recientes, conexión)
- Dependencias: `paho-mqtt` (ya instalado en el venv)
- Para correr: `python3 /root/ttung/01_Demos/agent_scada_proto.py`
- Ver `references/agent-scada-prototype.md` para detalle técnico

### Kanban Board — Tareas del Proyecto
- Proyecto cargado en kanban de Hermes: `hermes kanban list`
- Tarea principal: `t_5bc448c1` "Canvas 5 Agentes IA para CONECTA" (ready)
- 5 sub-tareas (uno por agente, todos en estado `todo`)
- 4 sub-tareas de roadmap (F1-F4, todos en estado `todo`)
- Dashboard web: `http://76.13.239.113:9119/` (requiere `ufw allow 9119` + `pip install fastapi uvicorn`)
- Comando de lanzamiento: `hermes dashboard --port 9119 --host 0.0.0.0 --insecure --no-open`
- Flujo de trabajo: `hermes kanban create "titulo" --assignee hermes --parent <id>` para crear sub-tareas
- Sistema multi-agente para potenciar servicios de CONECTA con inteligencia embebida
- NO reemplaza nada — se monta sobre infraestructura existente (SCADA, protecciones, RTUs)
- Stack: LangGraph + Hermes Agent + DeepSeek/Claude on-prem + Tier0 (MQTT/TimescaleDB/Grafana)
- Canvas Excalidraw: `/root/ttung/01_Demos/canvas_5_agentes_conecta.excalidraw`
  - Link: https://excalidraw.com/#json=tGKXqWF2cGYRlEjfD9tHA,gRcpy3NLZaS8V-ypNux6vQ
- Canvas HTML (fallback visual): `/root/ttung/01_Demos/canvas_5_agentes_conecta.html`
- Ver `references/conecta-5-agentes-ia.md` para detalle completo de propuesta
- Ver `references/conecta-estrategia-2026-2036.md` para roadmap CONECTA 10 años (headcount, regiones, revenue, modelo ventas, capacidad)

### Los 5 Agentes:

| # | Agente | Color | Función | Tecnología |
|---|--------|-------|---------|------------|
| 1 | AGENT-SCADA | Cyan | Monitoreo RT + diagnóstico | MQTT, TimescaleDB, Grafana |
| 2 | AGENT-PROTEC | Verde | Protecciones inteligentes | IEC 61850, DNP3, SEL |
| 3 | AGENT-CIBER | Rojo | Ciberseguridad OT | Orion RTU NERC CIP, Syslog |
| 4 | AGENT-ACTIVOS | Violeta | Mantenimiento predictivo + ML | TimescaleDB, Scikit-learn |
| 5 | AGENT-NORMATIVA | Amarillo | Compliance regulatorio automático | RAG, Vector DB, DeepSeek |

### Roadmap:
- F1 (S1-6): AGENT-SCADA piloto en cliente real ($15-20K)
- F2 (S7-10): AGENT-PROTEC + AGENT-NORMATIVA ($20-30K)
- F3 (S11-16): AGENT-CIBER + AGENT-ACTIVOS + ML predictivo ($25-35K)
- F4 (S17-20): Orquestador completo, dashboard unificado ($10-15K)
- Total setup: $70-100K por cliente · Recurrencia: $3-8K/mes · ROI: 2-3 meses

## JAFURAH
- Detalles pendientes

## IOC Minero
- Detalles pendientes

## Tier0 API Testing Patterns
Archivo: `references/tier0-api-testing-patterns.md`

Descubrimientos al construir los test agents Nivel 1 y Nivel 2:
- Grafana API: solo accesible via `docker exec` (root_url bloquea acceso directo)
- `wget` funciona mejor que `curl` dentro de containers Grafana
- `curl -m 5` necesario para evitar timeouts en docker exec
- UNS pathType: 0=PATH, 1=TOPIC; topicType: 0=STATE, 1=ACTION, 2=METRIC
- EMQX CLI usa args posicionales (`publish topic payload`), no flags
- SourceFlow/EventFlow APIs requieren token Bearer de IAM
- Dashboard creation + bind UNS via Grafana API + UNS API funciona end-to-end

Ver el archivo para comandos exactos de cada API.

## Dify Self-Hosted
- 6 containers en VPS, misma red edge_network que Tier0
- PostgreSQL compartida (database `dify` con pgvector)
- Web UI en puerto 3002, API en 5001
- Ver `references/dify-selfhosted-setup.md` para setup completo

## Cementos Melón
- Cementera chilena, 12 subestaciones (4 críticas), La Calera + Ventanas
- Situación: sin SCADA en SE críticas, SITR con 6% pérdidas en 4 meses, dispositivos heterogéneos
- **3 propuestas generadas** en ~/TTung/Cementos_Melon/:
  1. `Propuesta1_SUPcon_Solo.md/pdf` — Instrumentación SUPcon + PRIDE (no invasivo)
  2. `Propuesta2_SUPcon_MAS_CONECTA.md/pdf` — SUPcon + CONECTA full stack
  3. `Propuesta_CONECTA_Orion_Kronos_Argus.md/pdf` — Solo CONECTA con NovaTech
- **NovaTech products** (CONECTA distribuye en Chile):
  - **Orion**: RTU familia OrionMX/LX+/SX/VX, 50+ protocolos, NERC CIP, OrionOS común, 10yr warranty
  - **Kronos**: Relojes satelitales GPS multi-constelación (GPS+GLONASS+Galileo+BeiDou), 60ns precisión
  - **Argus**: Fleet management (lanzado Feb 2026), monitoreo Orion+Kronos+Hermes en tiempo real
- Ver `references/novatech-orion-kronos-argus.md` para especificaciones técnicas completas

## EXPORNOR Leads DB (Junio 2026)
Mini CRM para capturar oportunidades en EXPONOR 2026. FastAPI + SQLite + web app mobile con fotos de tarjetas.
- **URL:** http://76.13.239.113:8020/
- **API:** `/api/leads` (GET/POST), `/api/upload` (POST fotos), `/api/add?nombre=...` (quick-add)
- **DB:** SQLite `/root/exponor_db/leads.db`, fotos en `/root/exponor_db/fotos/`
- **WhatsApp flow:** Foto de tarjeta + "EXPORNOR" → Hermes lee con visión y guarda
- **Health check:** Cronjob cada 15 min (ID: 517a1072e781)
- **Skill:** `exponor-leads` (instrucciones detalladas)

## Scripts
- `scripts/capturar_pantallas_tier0.py` — Toma screenshots automatizados del SPA, login y Grafana. Usa Playwright. Previo: `playwright install chromium`.
- `/root/TTung/context_compressor.py` — Comprime specs, PDFs y BOMs al 7-50% del original conservando números técnicos. Usa DeepSeek vía litellm + caché SHA256. Skill: `industrial-context-compression`.
- `tier0_agent_install.py` (/root/) — Tier0 Installer Agent autónomo. 5 capas, auto-detecta path, 9 patrones de error.
- `tier0_test_agent.py` (/root/) — Tier0 Test Agent Nivel 1 autónomo. 7 pruebas con fallback methods. Resultado 7/7 verificado.
- `tier0_test_agent_level2.py` (/root/) — Tier0 Test Agent Nivel 2 autónomo. 4 pruebas plataforma (dashboards, flows, chat IA). Resultado 3/4.
- Ver `references/tier0-testing-patterns.md` para comandos exactos de cada prueba.
- `tier0_agent_install.py` (/root/) — Tier0 Installer Agent autónomo. 5 capas, auto-detecta path, 9 patrones de error.
- `tier0_test_agent.py` (/root/) — Tier0 Test Agent Nivel 1 autónomo. 7 pruebas con fallback methods. Resultado 7/7 verificado.
- `tier0_test_agent_level2.py` (/root/) — Tier0 Test Agent Nivel 2 autónomo. 4 pruebas plataforma (dashboards, flows, chat IA). Resultado 3/4.

## Referencias
- `references/10-open-source-alternatives-2026.md` — Repos open-source que Victor está evaluando
- `references/17-autonomous-agent-prompts.md` — Los 17 prompts de agentes autónomos analizados y mapeados a industria (Jun 2026)
- `references/hermes-local-mac-mini.md` — Guía para correr Hermes Agent + Gemma 4 local en Mac Mini (planeado)
- `references/arquitectura-local-first-jun2026.md` — Visión de Victor para stack local-first: Butterbase+MCP, LLM Wiki, Hermes+Obsidian, Dynamic Workflows, VoxCPM2 (sesión Junio 2026)
- `references/conecta-5-agentes-ia.md` — propuesta completa 5 agentes IA para CONECTA
- `references/dify-selfhosted-setup.md` — setup Dify junto a Tier0
- `references/hermes-tts-chile.md` — configuración TTS para español chileno (voice cloning)
- `references/novatech-orion-kronos-argus.md` — especificaciones técnicas NovaTech Automation (Orion RTUs, Kronos satellite clocks, Argus fleet management)
- `references/supcon-software-stack.md` — guía completa del stack SUPcon (25 softwares, integración técnica)
- `references/tier0-testing-patterns.md` — patrones de prueba verificados (Grafana docker exec, EMQX sin mosquitto, TSDB fallbacks, UNS API, etc.)
- `references/butterbase-selfhosted-deployment.md` — instalación y configuración de Butterbase self-hosted coexistiendo con Tier0, puertos, MCP endpoint, problemas conocidos
- `references/arquitectura-local-first-jun2026.md` — Stack de 6 herramientas para TTung local-first (visión Junio 2026: VoxCPM2, Butterbase, Dynamic Workflows, Mac Mini+Gemma4, Hermes+Obsidian, LLM Wiki)
- `references/competitor-ai-agent-pricing-2026.md` — Precios de mercado IA agents 2026 para decisiones de pricing en landing pages
- `references/estudio-mercado-agentes-ia-ingenieria-2026.md` — Estudio de mercado completo 2026: tamaños, competidores, gaps, pricing, FODA, plan 12 meses. HTML servido en :8006
- `references/skills-hub-audit.md` — Metodología para auditar/cross-referenciar el Skills Hub de Hermes contra skills instaladas.
- `references/soulmd-hermes-bible-best-practices.md` — SOUL.md best practices from Hermes Bible: prompt stack, token economics, qué va y qué no, estructura 50-80 líneas

## ChileCompra Scraper Cron Job

### Archivo
- **Scraper:** `/root/miv/chilecompra.js` (Node.js Playwright)
- **Output:** `/root/miv/chilecompra.json`
- **Cron:** Diario, ejecuta búsqueda de licitaciones instrumentación/SCADA/digitalización

### ⚠️ Problema conocido (Jun 2026)
El scraper actual está **roto** — devuelve 0 resultados porque las URLs que usa redirigen al homepage. La búsqueda real funciona en `https://www.mercadopublico.cl/BuscarLicitacion` con interacción directa en el DOM. Ver `references/chilecompra-scraper.md` para el fix completo (URL correcta, selectores, flujo Playwright, filtros UTM).

## Hermes Agent
- Versión actual: **0.14.0**
- Provider: DeepSeek v4 Flash, gateway WhatsApp activo
- Se ejecuta en el VPS Hostinger (srv1352721, 76.13.239.113)

### WhatsApp Gateway — Infraestructura (⚠️ CRÍTICO — leer antes de tocar)

**Dos procesos independientes** que deben estar sincronizados:

| Capa | Proceso | Puerto | Sesión |
|------|---------|--------|--------|
| Bridge (Baileys) | `bridge.js --port 3000 --mode self-chat` | **:3000** | `/root/.hermes/whatsapp-victor` |
| Bridge (Baileys) | `bridge.js --port 3002 --mode self-chat` | :3002 | `/root/.hermes/whatsapp-fran` |
| Gateway (Hermes) | `hermes_cli.main gateway run` via systemd | — | N/A |

**⚠️ DOBLE CAPA DE AUTORIZACIÓN:** El bridge tiene su allowlist (`WHATSAPP_ALLOWED_USERS`) Y el gateway tiene la suya propia (`_is_user_authorized()` en `gateway/run.py`). **Ambas necesitan `WHATSAPP_ALLOWED_USERS=56997847438`.** Si solo una capa lo tiene, los mensajes se rechazan como "Unauthorized".

**⚠️ Systemd NO lee `.env`:** El gateway arranca via systemd. El archivo `/root/.hermes/.env` NO es leído automáticamente. Las variables deben ir en `/etc/systemd/system/hermes-gateway.service.d/override.conf` como `Environment=`.

**Mapeo Phone↔LID:** Victor = `56997847438` (phone) ↔ `92170031751414` (LID interno WhatsApp). Archivos de mapeo en `/root/.hermes/whatsapp-victor/lid-mapping-*.json`.

**Watchdog:** Script `whatsapp-watchdog.sh` (cada 5 min, no_agent). Reinicia el bridge si se muere. Cron job: `ee132d2f10d8`.

Ver `references/whatsapp-gateway-troubleshooting.md` para guía completa de diagnóstico y reparación.

### Estrategia Local AI / Mac Mini (NUEVO Junio 2026)
Victor está considerando migrar a un setup **100% local** para eliminar dependencia de APIs de pago:

**Concepto:** Mac Mini M4 16GB ($700) corriendo Hermes Agent + Gemma 4 26B (cuantizado IQ4_XS) via Ollama — $0/mes en APIs.

**Referencias clave:**
- **NVIDIA Blog:** https://blogs.nvidia.com/blog/rtx-ai-garage-hermes-agent-dgx-spark/ — Hermes en RTX/DGX Spark, Qwen 3.6, self-improving agents
- **Lushbinary Guide:** https://lushbinary.com/blog/hermes-agent-gemma-4-qwen-3-5-local-ai-guide/ — Setup completo Gemma 4 + Qwen 3.5 via Ollama
- **Reddit (macmini):** https://www.reddit.com/r/macmini/comments/1tb3p3d/ — M4 16GB con Gemma-4-26b IQ4_XS.gguf
- **Twitter @atrekovas:** https://x.com/atrekovas/status/2062203236191191244 — Hermes Agent + Gemma 4 en Mac Mini

**Setup local (para cuando Victor decida ejecutar):**
```bash
# 1. Instalar Ollama
brew install ollama  # macOS
curl -fsSL https://ollama.com/install.sh | sh  # Linux

# 2. Pull modelo
ollama pull gemma4:26b
ollama run gemma4:26b --ctx-size 65536  # Hermes necesita min 64K

# 3. Instalar Hermes Agent
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# 4. Configurar Ollama como provider
hermes model → "More providers..." → "Custom endpoint"
API base: http://127.0.0.1:11434/v1
API key: (blank)
Model: gemma4:26b
```

**Implicaciones para TTung:**
- Elimina costo mensual DeepSeek API
- Datos nunca salen del escritorio de Victor
- Rendimiento depende de M4 16GB (el 26B cuantizado cabe, pero el debate en Reddit dice que 16GB puede quedar justo)
- Alternativa: DGX Spark (128GB, 1 petaflop, $?) o NVIDIA RTX

### Kanban Dashboard (Web UI)
- Comando: `hermes dashboard --port 9119 --host 0.0.0.0 --insecure --no-open`
- URL: `http://76.13.239.113:9119/`
- Prerrequisitos: `pip install fastapi uvicorn` + `ufw allow 9119`
- **Importante**: Siempre verificar `ufw status` antes de indicar a Victor una URL. Puertos abiertos conocidos: 8088 (Kong), 3010, 4000, 3001 (Grafana), 9443 (Portainer). Puertos como 8080, 8642, 8888 están BLOQUEADOS por UFW.
- Ver `references/hermes-kanban-dashboard.md` para guía de lanzamiento y resolución de problemas

## Perfiles Hermes del Equipo
4 perfiles creados via `hermes profile create <name> --clone` en /root/.hermes/profiles/:

| Perfil | Comando | SOUL.md | Función |
|--------|---------|---------|---------|
| **comercial** | `comercial chat` | Ejecutivo comercial senior | PPTs, propuestas, análisis competitivo |
| **cotizador** | `cotizador chat` | Ingeniero cotizaciones | BOMs, precios, ROI |
| **ingenieria** | `ingenieria chat` | Ingeniero sistemas automatización | Arquitecturas, Tier0, specs técnicas |
| **pm** | `pm chat` | Project Manager | Tareas, plazos, seguimiento |

Cada perfil comparte skills base pero tiene SOUL.md personalizado. Usan el mismo modelo/proveedor que default.
Los wrappers están en `~/.local/bin/<nombre>`. Se necesita `export PATH="$HOME/.local/bin:$PATH"` en bashrc.
Ver `references/hermes-profiles-equipo.md` para guía de uso completa.

## Tier0 Frontend — Zod Fix (services-express)
**Problema:** services-express crashea con `ERR_PACKAGE_PATH_NOT_EXPORTED` porque dependencias importan `./v3`/`./v4` de zod pero zod en pnpm store no tiene exports.

**Fix:** Script `/root/Tier0-Edge/deploy/fix-zod.sh` montado como volumen en frontend, ejecutado antes de node services-express. Busca recursivamente todos los `zod/package.json` bajo `.pnpm` y agrega exports.

**docker-compose.yml:** Comando del frontend tiene `sh /fix-zod.sh` antes de `concurrently`, y volumen `- /root/Tier0-Edge/deploy/fix-zod.sh:/fix-zod.sh`.

**Causa raíz:** Imagen `tier0/tier0-frontend:1.0.5-R1` con zod sin exports. Versión 1.0.6+ debería traer zod@4 o MCP SDK corregido.

## Tier0 Status — APAGADO (12-Jun-2026)
⚠️ Tier0 stack completamente down. Containers no existen. ENAPAC eliminado por orden de Victor. No levantar sin orden explícita.

### Servicios (cuando activo, referencia histórica)
| Servicio | Puerto host | Login | Nota |
|----------|-------------|-------|------|
| Kong (proxy) | **:8088** | — | Entry point unificado |
| SPA Frontend | via :8088 | tier0/tier0 | Login en /tier0-login |
| Grafana | **:3000** | admin/supos | GF_SERVER_ROOT_URL=/grafana/home/ |
| EMQX (MQTT) | :1883 | — | No tiene mosquitto_pub |
| PostgreSQL | :5432 | postgres/postgres | Base principal del stack |
| TimescaleDB | :2345 | postgres/postgres | DB puede ser "postgres" no "uns" |
| Node-RED | :1880 | — | SourceFlow |
| UNS backend | :18998 | — | API via Kong |
| EventFlow | :1889 | — | Similar a Node-RED |
| Portainer | :9443 | admin/adminpassword | Gestión Docker |

**Notas importantes (leer antes de operar):**
- **Grafana NO responde en :3001 — está en :3000.** Tiene `GF_SERVER_ROOT_URL=http://IP/grafana/home/` configurado, lo que significa que desde el host las APIs directas (`localhost:3000/api/health`) devuelven HTML error. Para probar APIs de Grafana desde fuera: usar `docker exec grafana curl -m 5 -s localhost:3000/api/...` con `-u admin:supos` — esto funciona. `wget` dentro del container se cuelga, usar `curl -m 5` siempre.
- **docker compose requiere `--profile grafana`** para activar Grafana (no arranca con `up -d` normal).
- **Contenedores zombies (Exited):** kong-migrations, init-db — init containers, comportamiento normal.
- **Contenedores viejos compiten con compose nuevos:** Si hubo un compose anterior y se detuvo, los containers viejos siguen existiendo con el mismo nombre. `docker compose down` NO los elimina — hay que hacer `docker rm -f` de los containers viejos antes de `docker compose up -d`. El error típico es "Conflict. The container name /X is already in use".
- **ENV vars que aparecen como "not set"** (non-blocking warnings): BASE_URL, UNS_DB_URL, SINK_PG_URL, SINK_TSDB_URL, OS_LLM_TYPE. Son generadas por `bin/util/set-temp-env.sh` cuando se ejecuta el `install.sh` oficial, pero NO se generan con `docker compose up -d` directo.
- **LLM_API_KEY vacío:** services-express se saltea, no bloquea el resto del stack.
- **ENTRANCE_DOMAIN:** Debe ser la IP real del servidor (76.13.239.113), no 100.94.162.88 (IP de ejemplo).

## Tier0 Bitácora de Problemas — Hallazgos Verificados (Mayo 2026)
- **Límite de ~20 tags/nodos: NO EXISTE** como restricción hardcodeada. Se escarbó backend Go (980 archivos), frontend React (200+ archivos), DTOs y queries SQL. No hay `if len(fields) > 20` ni `validate:"max=20"`. La paginación default es 20 items por tabla, pero no hay tope de creación. Descartado como problema real a menos que se confirme en la práctica.
- **Fix Zod** ya documentado en sección anterior.
- **GRPC_POOL_CORE_SIZE=8** recomendado en .env para evitar timeouts gRPC.
- **ENTRANCE_DOMAIN** debe apuntar a la IP real del servidor, no a 100.94.162.88 (IP de ejemplo del repo).
- **Grafana password:** `admin/supos` (no admin/admin). El password está en la config del container, no en .env.
- **Contenedores zombie (Exited):** Son init-containers (kong-migrations, init-db) — comportamiento normal.
- **LLM_API_KEY vacío:** Saltea services-express automáticamente, no bloquea el resto.
- **Grafana root_url:** `GF_SERVER_ROOT_URL=http://IP/grafana/home/` impide acceso directo a APIs REST desde el host. Usar `docker exec grafana curl -m 5` para interactuar.
- **EMQX sin mosquitto_pub:** El container EMQX no incluye herramientas mosquitto. Usar paho-mqtt desde host o `emqx_ctl status` para verificar.
- **ENV vars "not set"** (BASE_URL, UNS_DB_URL, etc.): Son generadas por `set-temp-env.sh` durante `install.sh`. Con `docker compose up -d` directo aparecen como warnings no bloqueantes.
- **Container stale conflict:** `docker compose down` no elimina containers viejos. Hacer `docker rm -f <nombres>` antes de `docker compose up -d` si hay conflictos de nombre.
- **⚠️ 0 tags en uns_tag = SPA vacío (Jun 2026):** Aunque el namespace tree tenga 310 nodos y tsdb tenga 2.4M registros, si `uns_tag` tiene 0 filas, el SPA no muestra datos. Las tags son el mapping namespace↔tsdb. Sin tags, el árbol está vacío de datos. Diagnóstico: `docker exec postgresql psql -U postgres -d postgres -c "SELECT COUNT(*) FROM uns_tag;"`

## Tier0 Installer Agent — IMPLEMENTADO (Mayo 2026)
- Script autónomo en **`/root/tier0_agent_install.py`** (servido en http://76.13.239.113/tier0_agent_install.py)
- **Arquitectura 5 capas probada (dry-run + --help funcionan):**
  1. **Pre-flight**: Verifica root, SO, puertos (8088/1883/5432/3001), disco (>50GB), Docker, instalación previa
  2. **Setup**: Clona repo oficial, configura .env con IP real, aplica fix-zod.sh + GRPC_POOL_CORE_SIZE=8
  3. **Install**: Ejecuta `install.sh` oficial con 2 reintentos + matching de 9 patrones de error
  4. **Health Gates**: 6 pruebas (Containers UP, Kong :8088, Login SPA, MQTT/EMQX, TimescaleDB, Grafana)
  5. **Report**: HTML con resumen, URL acceso, configuración, log completo
- **Modos:** `--dry-run`, `--non-interactive`, `--repair`, `--ip <IP>`, `--llm-key <KEY>`
- **Error handlers:** ERR_PACKAGE_PATH_NOT_EXPORTED → fix-zod, connection refused → reintento, OOM/137 → hint RAM, port conflict → ss -tlnp, Kong unhealthy → restart
- **Documento complementario:** `tier0_instalacion_a_prueba_de_balas.html` — manual paso a paso con checklist 14 ítems
- **Hallazgo de debugging:** Límite ~20 tags DESCARTADO — se verificó código fuente completo (980 Go + 200+ TS/TSX), no existe restricción hardcodeada. Solo paginación default 20.

## Tier0 Test Agent — IMPLEMENTADO (Mayo 2026)
- Script autónomo en **`/root/tier0_test_agent.py`** (servido en http://76.13.239.113/tier0_test_agent.py)
- **7 pruebas end-to-end del stack Tier0:**
  1. **Login**: Autenticación tier0/tier0 via API REST + verificación login page
  2. **Namespace**: Creación de estructura PATH > TOPIC con fields (Testing_Agent/Metric/temp_test)
  3. **SourceFlow**: Verificación Node-RED responde via Kong y directo
  4. **MQTT**: Publicación de datos con paho-mqtt (fallback a EMQX HTTP API o EMQX CLI)
  5. **TimescaleDB**: Consulta de registros en uns_timeserial en postgres/uns/tsdb
  6. **Grafana**: Health check en puerto 3000/3001 + fallback via docker exec
  7. **Export/Import**: Descarga de template JSON
- **Fallback methods para cada test** — si el método principal falla, intenta alternativas
- **Resultado verificado:** 7/7 tests pasaron en VPS real con 6,129 registros en TSDB
- **Modos:** `--quick` (solo login + namespace), `--non-interactive`, `--dry-run`

## Tier0 Test Agent Level 2 — IMPLEMENTADO (Mayo 2026)
- Script autónomo en **`/root/tier0_test_agent_level2.py`** (servido en http://76.13.239.113/tier0_test_agent_level2.py)
- **4 pruebas profundas de plataforma:**
  1. **Chat IA**: Verifica LLM_API_KEY configurada, services-express corriendo, endpoints CopilotKit. **Falla si no hay API key** (no bloqueante).
  2. **Dashboards Grafana**: Crea datasource TimescaleDB via docker exec `curl -m 5 -u admin:supos`, crea dashboard con panel querying TSDB, bind a UNS via API. **Requiere usar `docker exec grafana curl -m 5`** por el root_url configurado.
  3. **SourceFlow real**: Crea flow Node-RED via `POST /inter-api/supos/flow`, bind a UNS namespace. Verifica creación y listado.
  4. **EventFlow**: Crea flow de eventos via `POST /inter-api/supos/event/flow`, lista flows para confirmar.
- **Técnicas descubiertas:**
  - **Grafana API**: No accesible desde host por `root_url=/grafana/home/`. Usar `docker exec grafana curl -m 5 -u admin:supos localhost:3000/api/...` — `curl -m 5` es crítico (sin timeout se cuelga). `wget` también se cuelga en POST.
  - **Node-RED**: API responde en :1880 directo y via Kong. Flow creation API funciona.
  - **EventFlow**: Misma interfaz que SourceFlow pero bajo `/event/flow/`.
  - **EMQX**: No tiene `mosquitto_pub` ni `mosquitto_sub`. Usar `emqx_ctl status` para verificar, o instalar paho-mqtt en host. `curl -u admin:public` a `http://localhost:18083/api/v5/publish` como fallback.
  - **TimescaleDB**: La database puede llamarse "postgres" en vez de "uns". Verificar con `\l` primero.
- **Resultado en VPS real (26 May):** 3/4 — Chat IA requiere LLM_API_KEY. Dashboard (uid: efn7rm8a7zwu8d), SourceFlow, EventFlow OK.
- Ver `references/tier0-testing-patterns.md` para patrones detallados de prueba.

## ⚠️ REGLA CRÍTICA: Cada número con fuente verificada

Victor corrigió explícitamente: **"hoy inventamos el número uno, cada número con una fuente, nunca más nos mandan un número sin verificar"**.

Esto aplica a TODOS los estudios, análisis, informes y PPTs. Siempre que se presente un número:

1. **Si tiene fuente en documentos locales:** citar el archivo exacto (PDF, XLSX, propuesta real, carta de adjudicación)
2. **Si es de proyecto real:** decir "Fuente: Propuesta OT-XXXX" o "Fuente: Carta Adjudicación CGE"
3. **Si es estimación/benchmark sin fuente documental:** decir EXPLÍCITAMENTE "Estimado basado en información disponible — sin precio público de SUPcon" o "Benchmark de industria — sin fuente documental propia"
4. **Si no hay fuente disponible en los documentos:** preguntar a Victor antes de publicarlo
5. **NUNCA inventar números ni usar estimaciones no declaradas** — Victor preguntó "de donde sacaste el numero weon" cuando se usó $4.2B sin fuente

Para extraer precios reales de documentos:
- Usar pymupdf (`fitz`) para leer PDFs de propuestas
- Buscar secciones "DETALLE DE PRECIOS" y "ANEXO Modelo Economico"
- Archivos clave: `MaterialIAProyectos/Traspasos/OT-XXXX/4 Oferta/` (propuestas con pricing) y `6 OC/` (cartas adjudicación)
- Tabla_Productos_Completa.xlsx tiene Tier0 a $25K/año, el resto "Consultar"

## CONECTA Nexus — SCADA · CEN/PMU · Monitoreo (Junio 2026)

CONECTA Ingeniería tiene 34 años en Chile. Su **core de negocio (el "nexus")** son sistemas de software de conectividad para el sector eléctrico. **Sin hardware, sin RTUs, sin instrumentos** — solo software que conecta, norma y monitorea.

### Las 3 Líneas del Nexus

| Línea | Producto | Ticket |Margen | Ciclo Venta | Clientes |
|-------|----------|--------|-------|-------------|----------|
| **SCADA** | SCADA ELIPSE (Brasil) + ingeniería Automalogica | $80K-350K USD | 35-45% | 3-8 meses (licitación) | Transelec, CGE, Enel, Colbún, AES Andes, Metro |
| **CEN/PMU** | Sistema normativo — PMU Vizimax + PDC ELPROS | $120K-500K USD (proyecto) / ~400 UF (~$18K) por subestación | 40-50% | 6-12 meses (regulatorio) | Transelec, CGE, Coordinador Eléctrico, Barrick |
| **Monitoreo** | Power Quality, Compliance Reporting, Analytics | $30K-120K USD | 45-55% | 2-4 meses (post-venta) | Transelec, AES Andes, Colbún |

**Regla clave:** Todas son software puro. Datos entran por IEC 61850, DNP3, Modbus desde equipos del cliente. No se vende hardware.

### Precios Reales Encontrados en Proyectos CONECTA

**Proyecto CGE PMU (Feb 2026) — 6 subestaciones, ADJUDICADO a CONECTA:**
- Total: **2,449.67 UF** (~$110K USD)
- Por subestación: **399-418 UF** (~$18-19K USD) incluye:
  - PMU Vizimax (Canadá): 109 UF (~$4,900)
  - Materiales menores: 113 UF (~$5,100)
  - Ingeniería: 19 UF (~$855)
  - Montaje/alambrado: 91 UF (~$4,100)
  - Configuración/pruebas: 13 UF (~$585)
  - Gastos generales: 53-72 UF (~$2,400-3,240)
- Entrega: 138 días hábiles (~6 meses)
- Pago: 20% ing básica → 25% ing detalle → 20% FAT → 15% montaje → 15% SAT → 5% puesta en servicio
- Fuente: Propuesta 260110 Rev 0, PDF con pymupdf desde `MaterialIAProyectos/Traspasos/OT 5219/`

**Proyecto Barrick PMU (Abr 2026) — 2 PMUs Punta Colorada:**
- Equipos (2 PMU Vizimax + blocks + GPS): **USD 18,476**
- Licencias PDC Corporativo CEN: **USD 5,460**
- Servicios implementación: **501 UF** (~$22,545 USD)
- **Total: ~$46,481 USD**
- Opcional cableado patio: 141 UF (~$6,345)
- Fuente: Propuesta 260303 Rev 0, PDF con pymupdf desde `MaterialIAProyectos/Traspasos/OT 5223/`

### Cómo Extraer Precios Reales de PDFs

Cuando necesites precios reales de CONECTA/SUPcon, NO estimar desde benchmarks. Extraer de los PDFs de propuestas usando pymupdf (biblioteca `fitz`):

```python
import fitz
doc = fitz.open("/ruta/a/propuesta.pdf")
text = ""
for page in doc:
    text += page.get_text()
# Buscar "DETALLE DE PRECIOS", "UF", "USD", tablas
```

Archivos clave donde buscar:
- `MaterialIAProyectos/Traspasos/OT-XXXX/4 Oferta/` — propuestas comerciales con pricing
- `MaterialIAProyectos/Traspasos/OT-XXXX/6 OC/` — cartas de adjudicación con montos
- `MaterialIAProyectos/Traspasos/OT-XXXX/3 Calculo de Oferta/Cotización/` — cotizaciones de proveedores
- `Tabla_Productos_Completa.xlsx` — catálogo SUPcon (Tier0: $25K/año, resto: "Consultar")

⚠️ **Siempre declarar la fuente del pricing cuando presentes datos.** Si es de PDF real, decir "fuente: propuesta OT-XXXX". Si es estimación/benchmark, decir "estimado basado en información disponible — sin precio público de SUPcon". Victor preguntó "de donde sacaste los precios" cuando no se declaró la fuente — evitar que eso se repita.

### Formato de Estudio de Mercado (10 secciones)

Cuando Victor pide análisis de mercado con mismo formato que el estudio de agentes IA, seguir esta estructura exacta:

1. **Resumen Ejecutivo** — KPIs (4 tarjetas color) + insight/conclusión clave
2. **Definición de Líneas/Servicios** — Tarjetas de producto con tickets, márgenes, ciclos, competidores
3. **Tamaño de Mercado** — Barras por línea + tabla captura + crecimiento CAGR
4. **Panorama Competitivo** — Tablas: competidor | producto | precio relativo | presencia | debilidad
5. **Gaps de Mercado** — Tarjetas con severidad (crítico→oportunidad)
6. **FODA por Línea** — SWOT grid para cada línea + acento/codo insight
7. **Acento vs Codo** — Dónde poner inteligencia vs fuerza bruta comercial
8. **Plan de Acción 12 Meses** — Tabla mes a mes con recursos y métricas
9. **Proyección Revenue** — Tabla 3 años por línea + KPIs
10. **Veredicto** — KPIs finales + insight de cierre

Archivos de referencia de este formato:
- `/root/TTung/estudio-mercado-supcon-3-lineas.html` — SUPcon: Field Instrumentation · Software · Control Systems
- `/root/TTung/estudio-conecta-nexus-scada-cen-monitoreo.html` — CONECTA: SCADA · CEN/PMU · Monitoreo
- `/root/agentesnextai/estudio-mercado-agentes-ia.html` — Agentes IA (formato original)

Servir en puerto 3013: `cd /root/TTung && python3 -m http.server 3013`

## Producto "Agentes IA para Empresas de Ingeniería" (Junio 2026)

Landing page para vender sistema de 3 agentes IA a empresas de ingeniería. GENÉRICO (sin CONECTA/SUPcon/TTung).

### Estructura
| Agente | Área | Color |
|--------|------|-------|
| Agente 1 | Administración (finanzas, RRHH, contratos) | Azul #4A90D9 |
| Agente 2 | Operaciones (proyectos, ingeniería, calidad) | Verde #00A86B |
| Agente 3 | Comercial (ventas, cotizaciones, licitaciones) | Naranjo #E67E22 |

### Key Messaging
- **"Lo que ves es lo que haces"** — transparencia total, el usuario ve y aprueba cada acción del agente
- **"Sin miedo a la automatización"** — los agentes potencian a la gente, no la reemplazan. 5 personas con agentes = 15

### Pricing
- Setup: $30K - $50K (diagnóstico + implementación + 3 agentes)
- Recurrencia: $2K - $5K/mes (mantenimiento, monitoreo, actualización)

### Stack Tecnológico
Hermes Agent + WhatsApp/Slack/Teams + modelos on-prem (Gemma 4, DeepSeek, Claude) + ERP/CRM existente + RAG + Dashboards

### Archivos
- Landing: `/root/ttung/landing_ingenieria_agentes.html`
- Servida en VPS: http://76.13.239.113:8005/agentes-ia-ingenieria.html
- Netlify: https://agentes-ia-ingenieria.netlify.app
- GitHub: https://github.com/vvilche/agentesnextai

### Competidores (pricing 2026)
| Empresa | Setup | Recurrencia/mes | Nota |
|---------|-------|-----------------|------|
| ProductCrafters | $30K-$150K | — | Multi-agent, 6-14 sem |
| LeewayHertz | $100K-$500K+ | — | Enterprise, 16-30 sem |
| Itransition | $50K-$250K | — | ERP/CRM, 10-20 sem |
| Neurons Lab | $80K-$300K | — | R&D, 14-24 sem |
| SoftTeco (Sales Agent) | $60K-$120K | $3K-$8K | Más parecido a Agente Comercial |

TTung está BARATO vs mercado. Nadie apunta a ingeniería — nicho vacío.

### Arquitectura (3 capas, Excalidraw)
Excalidraw: `https://excalidraw.com/#json=NbE-Rz-TI-THjFKkP5zON,MKYL5XTaK8dQ0gQDhAzjNg`

```
┌─────────────────────────────────────────────────────┐
│  TU EQUIPO (Personas)                                │
│  Admin (azul)  |  Ops (verde)  |  Comercial (naranja)│
│  ── via WhatsApp / Slack / Teams ──                   │
├──────────────────────────────────────────────────────┤
│  CAPA DE AGENTES (Hermes Agent)                       │
│  Plugin Admin | Plugin Ops | Plugin Comercial        │
│  Skills auto-mejora | Modelos On-Prem                │
│  Gemma 4 · DeepSeek · Qwen 3.6 · LM Studio/Ollama   │
│  $0/mes APIs · Datos nunca salen                     │
├──────────────────────────────────────────────────────┤
│  SISTEMAS EXISTENTES (Integraciones)                  │
│  ERP/CRM | Base Conocimiento | Datos | Comms         │
│  (Odoo/SAP) | (Documentos)   | (MQTT/TSDB)           │
└──────────────────────────────────────────────────────┘
```

Flujo: Equipo pide via WhatsApp/Slack → Hermes orquesta al plugin correcto → plugins acceden a sistemas existentes → modelos on-prem procesan sin enviar datos.

### Value Prop (5 puntos — pitch ejecutivo)
1. **No necesitas más gente** — 5 personas con agentes rinden como 15
2. **Cero riesgo de data leakage** — modelos on-prem, datos nunca salen
3. **Modular** — partes con 1 agente ($15K, 3 sem), escalas a 3
4. **Auto-mejora** — 20-50 workflows aprendidos en 1 mes específicos de tu empresa
5. **3x más barato que competencia** — $30-50K setup vs $100-500K

### Cronjobs Activos (Junio 2026)

| Nombre | ID | Schedule (UTC) | Hora Chile | Skills | Delivery | Estado |
|--------|----|----------------|-----------|--------|----------|--------|
| **brief-7am** ☀️ | cf71448715d5 | 11:00 UTC | 07:00 (inv) / 08:00 (ver) | ttung-projects, vps-deployment | origin (WhatsApp) | ❌ ERROR — bloqueado por patrón ssh_backdoor (12-Jun) |
| **nightly-review** 🔍 | b2976bf04cdf | 02:00 UTC | 23:00 (inv) / 22:00 (ver) | — | origin (WhatsApp) | ✅ Nuevo |
| **vps-watchdog** 🤖 | 84ddb5f50e30 | c/4h (0,4,8,12,16,20) | — | — | origin (WhatsApp) | ✅ Nuevo (no_agent) |
| Revisión de Prensa - Agentes IA | ae4989827149 | Lun 9:00 | — | web-research | origin | ✅ Activo |
| miv-chilecompra | 9aeeb3d83851 | Lun-Vie 7:00 | — | — | origin | ✅ Activo |
| hermes-auto-update | 3c76e4c49e12 | Cada 4320m (3 días) | — | — | origin | ✅ Activo |
| VPS Cleanup Semanal | d78c469fc09d | Dom 3:00 | — | clean_vps.sh (no_agent) | origin | ✅ Activo |
| EXPORNOR HealthCheck | 517a1072e781 | cada 15m | — | exponor_health.sh (no_agent) | local | ✅ Activo |
| Competitor Watch | 132cec2de952 | Lun-Vie 12:00 | — | — | origin | ✅ Activo |
| **Morning Brief (antiguo)** | 2b7183a9c150 | Pausado | — | — | — | ⏸️ Pausado (reemplazado por brief-7am) |
| **whatsapp-watchdog** 🔁 | ee132d2f10d8 | */5 * * * * (c/5min) | — | whatsapp-watchdog.sh (no_agent) | origin | ✅ Nuevo (09-Jun) |

**Cambios recientes (Junio 2026):**
- Pulso Guacolda: delivery reparado (entregaba a la nada por deliver=origin sin WhatsApp conectado)
- OpenClaw HealthCheck: eliminado (OpenClaw ya no corre en este VPS)

### SOUL.md Template (Referencia para configurar Hermes Agent autónomo)
Ver `references/soulmd-template.md` para el template viral de 6 secciones que convierte agentes de chatbot a operadores autónomos. Publicado por senior dev en Reddit/X, nunca compartido antes.
