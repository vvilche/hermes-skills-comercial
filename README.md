# Manual de Instalación — Hermes Agent para Equipo Comercial TTung/CONECTA

> **Versión:** 1.0 — Julio 2026
> **Perfil:** Comercial (investigación, propuestas, inteligencia de negocio)
> **Skills incluidos:** SUPcon, NovaTech, CONECTA, investigación, diseño

---

## 1. REQUISITOS MÍNIMOS

| Componente | Mínimo | Recomendado |
|---|---|---|
| Sistema operativo | macOS 12+ / Windows 10 (64-bit) / Ubuntu 20+ | macOS 14+ o Ubuntu 22+ |
| RAM | 8 GB | 16 GB |
| Disco | 2 GB libre | 5 GB libre |
| Python | 3.10+ | 3.11 o 3.12 |
| Git | Sí | Sí |
| API Key | DeepSeek (la entrega Victor) | — |

---

## 2. INSTALACIÓN (5 MINUTOS)

### macOS / Linux

```bash
# 1. Instalar Hermes Agent
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# 2. Agregar al PATH (cierra y abre terminal después)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# 3. Verificar instalación
hermes --version
# Debe mostrar: hermes 0.14.0 o superior
```

### Windows

```powershell
# 1. Abrir PowerShell como Administrador

# 2. Instalar Hermes Agent
irm https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.ps1 | iex

# 3. Verificar instalación
hermes --version
```

---

## 3. CONFIGURACIÓN DE API KEY (DEEPSEEK)

Victor entrega la API key de DeepSeek. Una vez que la tengas:

```bash
# Crear archivo .env
echo 'DEEPSEEK_API_KEY=sk-tu-key-aqui' > ~/.hermes/.env

# Configurar DeepSeek como provider
hermes config set model.provider deepseek
hermes config set model.default deepseek-v4-pro
```

---

## 4. CREAR PERFIL COMERCIAL

```bash
# Crear perfil "comercial" con las skills base
hermes profile create comercial --clone

# Verificar que se creó
hermes profile list
# Debe mostrar: default, comercial
```

---

## 5. SKILLS COMERCIALES ESENCIALES

Estos skills se cargan automáticamente en el perfil comercial. Copia cada carpeta desde el repositorio de Victor.

### Skills Core (OBLIGATORIOS)

| Skill | Carpeta | Para qué |
|---|---|---|
| victor-rules | `victor-rules/` | Reglas de comunicación y estilo |
| ttung-projects | `ttung-projects/` | Contexto de todos los proyectos activos |

### Skills de Investigación e Inteligencia

| Skill | Carpeta | Para qué |
|---|---|---|
| conecta-business-intel | `research/conecta-business-intel/` | Escanear competidores, licitaciones, oportunidades |
| aisa-deep-research | `aisa-deep-research/` | Investigación profunda de proyectos mineros |
| aisa-marketpulse | `aisa-marketpulse/` | Precios cobre, litio, flujos de inversión |
| aisa-multisearch | `aisa-multisearch/` | Búsqueda simultánea en múltiples fuentes |
| aisa-rumor-scanner | `aisa-rumor-scanner/` | Detecta proyectos antes de prensa |
| aisa-x-intel | `aisa-x-intel/` | Monitoreo Twitter/X minería |
| web-research | `research/web-research/` | Búsqueda general en internet |

### Skills de Ventas y Propuestas

| Skill | Carpeta | Para qué |
|---|---|---|
| industrial-digitalization-kickoff | `productivity/industrial-digitalization-kickoff/` | Propuestas de digitalización industrial |
| industrial-tier0-pilot | `productivity/industrial-tier0-pilot/` | Metodología pilotos Tier0 brownfield |
| partnership-plans | `productivity/partnership-plans/` | Planes de partnership CONECTA+SUPcon+externos |
| industrial-research-workflows | `ttung-projects/industrial-research-workflows/` | Workflows de investigación industrial |

### Skills de Documentos y Diseño

| Skill | Carpeta | Para qué |
|---|---|---|
| powerpoint | `productivity/powerpoint/` | Crear y editar PPTs |
| postmark-email | `productivity/postmark-email/` | Enviar correos a clientes |
| excalidraw | `creative/excalidraw/` | Diagramas de arquitectura y flujo |
| ttung-design-system | `creative/ttung-design-system/` | Reglas visuales de entregables TTung |

### Skills de Producto (SUPcon + NovaTech)

| Skill | Carpeta | Para qué |
|---|---|---|
| tier0-edge | `tier0-edge/` | Arquitectura Tier0 completa |
| tier0-populate-namespace | `tier0-edge/tier0-populate-namespace/` | Crear estructuras UNS |
| tier0-populate-grafana | `tier0-edge/tier0-populate-grafana/` | Dashboards Grafana |
| industrial-dashboard-generator | `devops/industrial-dashboard-generator/` | Dashboards HTML standalone |

---

## 6. COPIAR SKILLS DESDE EL REPOSITORIO

Victor mantiene todas las skills en GitHub. Para instalarlas:

```bash
# 1. Clonar el repo de skills (Victor te da la URL)
git clone https://github.com/vvilche/ttung-skills.git /tmp/ttung-skills

# 2. Copiar skills al perfil comercial
cp -r /tmp/ttung-skills/skills/* ~/.hermes/profiles/comercial/skills/

# 3. Recargar skills
hermes --profile comercial skills list

# Debes ver 20+ skills listadas
```

---

## 7. SOUL DEL PERFIL COMERCIAL

Crear `~/.hermes/profiles/comercial/SOUL.md`:

```markdown
# Soul — Comercial TTung/CONECTA

Eres un ejecutivo comercial senior de TTung Engineering. Conoces a fondo:
- SUPcon: Field Instrumentation, Software (supOS, PRIDE, X-AI), Control Systems (DCS)
- NovaTech: Orion RTUs, Kronos satellite clocks, Argus fleet management
- CONECTA: SCADA ELIPSE, PMU Vizimax, Monitoreo calidad energía
- Tier0/UNS: Plataforma de digitalización industrial

## Reglas
- Español chileno natural, sin jerga interna
- Cada número con fuente real. No inventar precios
- Antes de enviar cualquier cosa a un cliente, revisar 2 veces
- Si no sabes algo, preguntar a Victor antes de responder
- Propuestas siempre con: problema → solución → precio → ROI → próximo paso
```

---

## 8. VERIFICACIÓN FINAL

```bash
# 1. Cambiar al perfil comercial
hermes --profile comercial

# 2. Verificar skills cargados
/skills

# 3. Probar con una consulta simple
"Investiga quién es el gerente de mantención de Minera Los Pelambres"
```

---

## 9. USO DIARIO

### Iniciar sesión comercial
```bash
hermes --profile comercial
```

### Consultas típicas
```
"Escríbeme un correo para agendar reunión con [cliente]"
"Investiga qué proyectos tiene [minera] para 2026-2027"
"Prepárame una propuesta de Tier0 para [planta]"
"Escanea licitaciones de instrumentación esta semana"
"Arma un PPT de 5 slides sobre digitalización para [cliente]"
```

### Slash commands útiles
```
/skill nombre       — Cargar un skill específico
/goal "objetivo"    — Perseguir objetivo hasta cumplirlo
/new                — Sesión limpia
/model              — Cambiar modelo
```

---

## 10. SOLUCIÓN DE PROBLEMAS

| Problema | Solución |
|---|---|
| "hermes: command not found" | Agregar `~/.local/bin` al PATH |
| "DEEPSEEK_API_KEY not set" | Verificar `~/.hermes/.env` |
| Skills no aparecen | Ejecutar `/reload-skills` |
| Respuestas en inglés | Decir "respóndeme en español" |

---

## 11. SOPORTE

- **Victor Vilche**: +56 9 9978 47438 (WhatsApp)
- **Documentación oficial**: https://hermes-agent.nousresearch.com/docs/
- **Repo de skills**: https://github.com/vvilche/ttung-skills

---

*Manual preparado por Hermes Agent — Julio 2026*
*TTung Engineering SpA — CONECTA + SUPcon*
