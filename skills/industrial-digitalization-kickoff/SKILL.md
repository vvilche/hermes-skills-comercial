---
name: industrial-digitalization-kickoff
title: Industrial Digitalization Project Kickoff
description: Approach workflow for industrial digitalization proposals (DCS replacement, Tier0/UNS, IA overlays) at CONECTA/SUPcon. Emphasizes minimal MVP, data-first analysis, passive integration with existing systems, and layered complexity.
domain: Industrial automation, DCS migration, MIV proposals, Chilean and Peruvian power/mining/water
trigger: User asks to start a new industrial project, analyze plant data, propose architecture, or scope a pilot for a power plant, mine, water treatment, or similar facility.
tags: [industrial, DCS, MIV, proposal, tier0, UNS, MVP, consultancy, conecta, supcon]
related_skills: [architecture-diagram, ttung-design-system]
---

# Industrial Digitalization Project Kickoff

## When to Use

Load this skill when Victor (CONECTA/SUPcon CEO) asks to:
- Start a new industrial project (power plant, mine, aqueduct, refinery)
- Analyze plant data (P&IDs, I/O lists, minutas)
- Propose architecture for DCS replacement / IA overlay / Tier0/UNS
- Scope a pilot for a specific unit or plant area
- Build a demo or proof-of-concept

## Core Principles

### 1. MVP First, Complexity Later
Always start with the absolute minimum viable approach:
- **No new hardware** unless essential
- **No touching existing systems** — read passively
- **Software-only** first (dashboard, analytics)
- Add gateways, sensors, new DCS only in future phases

The user's default is to simplify. If you propose a complex architecture, expect him to strip it down. Save time by starting minimal and offering to add layers.

### 2. Data-First Audit
Before proposing anything:
1. Audit the project folder — ls -laR, check subdirs, extract archives
2. Create INVENTARIO.md listing all found documents
3. Identify: P&IDs, I/O lists, minutas, existing architectures
4. Analyze the I/O list: count by signal type (AI/AO/DI/DO), controller type, P&ID reference
5. Present findings before proposing solutions

### 3. The "No-Touch" Principle
For existing DCS installations (Mitsubishi, Siemens, ABB, etc.):
- **DO NOT propose replacing** hardware in Phase 1
- **DO NOT propose touching** the running control system
- Read data passively via existing historians (PI System, OSIsoft, OPC)
- Layer Tier0/UNS dashboards and IA analytics on top
- Gateways (PRIDE, GW series) and new sensors come in Phase 2
- Full DCS replacement (ECS-700) is Phase 3+ optional

### 4. Three-Phase Roadmap Structure
Structure every proposal as 3 phases:

| Phase | Name | What | Timeline |
|-------|------|------|----------|
| F1 | Tier0 MVP | Software-only, reads data, dashboard, no new hardware | Weeks |
| F2 | IA Overlay | PRIDE gateways, TPT sensors, APC, predictive maintenance | 3-6 months |
| F3 | Scale / Replace | Replicate to other units, optional DCS replacement (ECS-700) | 12-18 months |

### 5. Address Cybersecurity as a Feature, Not a Risk
When the audience includes IT/cybersecurity stakeholders:
- Lead with security in the architecture — frame "no touch" as a security feature, not just convenience
- Dedicate a slide specifically to cybersecurity when presenting
- Key messages:
  - **No connection from Tier0 to the DCS** — reads existing historian (PI System), not the control network
  - **No ports opened** on the DCS network
  - **No cloud dependency** — Tier0 runs 100% local
  - **No data leaves the plant** in Phase 1
  - The isolation gateway (GW033/Diode) is available for Phase 2 if they want additional hardening
- This is your strongest differentiator vs EtaPRO or other solutions that require deeper integration
- AI/cloud features come in Phase 2 when they've already validated trust

### 6. Tier0 Has NO AI — Set Expectations Upfront
Tier0/UNS is purely visualization (KPIs, trends, alarms). It does NOT:
- Run AI/ML models
- Generate apps
- Provide predictive maintenance
- Require cloud credentials

The AI capabilities come from **supOS** (SUPCON's industrial OS platform), which connects in Phase 2:
- supOS X-Collector reads data from PI System (ODBC/OPC)
- supOS DataLake stores and processes
- supOS X-AI runs ML models
- InPlant APC provides advanced process control
- PRIDE provides equipment monitoring
- REST API feeds AI results back to Tier0 dashboard

### 7. CONECTA vs SUPcon — Branding Distinction

**Tier0/UNS is CONECTA's methodology, NOT a SUPcon product.** supOS, PRIDE, ECS-700, TPT are SUPcon products. Do NOT present Tier0 as "SUPcon's Tier0" or mix the brands.

When presenting to clients:

| Concept | Who owns it | What it is |
|---------|-------------|------------|
| Tier0/UNS | **CONECTA** | Methodology, architecture, approach to ciberseguridad industrial y visibilidad de datos |
| supOS | **SUPcon** | Platforma IIoT, DataLake, X-AI, InPlant APC |
| PRIDE | **SUPcon** | Monitoreo de equipos rotantes, gateways GW series |
| ECS-700 | **SUPcon** | DCS con APL, SIL3, E-BUS |
| MIV | **CONECTA + SUPcon** | Modelo de contratación integrado |

**Reglas de branding:**
- Tier0 se vende como "metodologia CONECTA" — es el approach, la consultoría, el know-how de arquitectura industrial
- supOS/PRIDE/ECS-700 se venden como "productos SUPcon" — son la plataforma tecnológica
- La propuesta de valor es CONECTA (método) + SUPcon (tecnología) = solución completa
- NO decir "SUPcon Tier0" — suena a producto SUPcon, y no lo es
- NO decir "Tier0 incluye supOS" — Tier0 es solo visualización, supOS viene en Fase 2
- **En el pitch verbal**: "Nosotros (CONECTA) diseñamos la arquitectura Tier0, y SUPcon pone la plataforma tecnológica (supOS/PRIDE/ECS-700) cuando se requiere escalar."

**Esto es crítico para no confundir al cliente y para posicionar a CONECTA como consultora, no como revendedora de SUPcon.**

### 8. Build Working Demos on VPS

When proposing a dashboard or Tier0 solution:
- Build it immediately as a working HTML file
- Use simulated data if real data isn't available yet
- The demo should be openable in a browser with zero dependencies
- Use the dark TTung palette (see ttung-design-system skill)
- **Deploy on the VPS** (not localhost) so the client can access it via internet
- Each demo on its own port: 8080, 8081, 8082, etc.
- Bind to 0.0.0.0, not 127.0.0.1, so it's reachable from outside
- Verify firewall isn't blocking: `ufw status` or `iptables -L INPUT`
Provide the public URL with the VPS IP and port.

Demos live in `/root/ttung/demos/` on the Hostinger VPS (76.13.239.113).
Scripts are self-contained Python (stdlib only, no dependencies).

### 9. Peru Partnership Model

Victor is now targeting Peru. The business model is a **three-way partnership**:

| Role | Company Type | What they do | Example |
|------|-------------|-------------|---------|
| Engineering | Pure engineering firm (no construction) | Design, studies, specs | Noatec-style |
| Technology | CONECTA/SUPcon | Instrumentation, control, automation, digitalization | We do this |
| Construction | Local contractor (small/medium, not electrical-focused) | Civil works, installation, montaje | To be found |

**Key insight:** The engineering firm can't do construction. The contractor can't do engineering. Neither can provide the technology layer alone. CONECTA/SUPcon is the technology & integration bridge.

**Target projects:** Chiquinta, Tambo Real, JAFURAH — stations, pumping, water infrastructure in Peru.

**Action:** When Victor mentions a new Peruvian project or company:
1. Identify which role they fill (engineering, tech, or construction)
2. Propose the partnership triangle
3. Search for the missing piece if needed

### 10. Voice Cloning for Demos & Presentations

Victor wants to clone the voice of **Guillermo Velarde** (a Peruvian contact) using ElevenLabs voice cloning.

**Workflow:**
1. Get a 30s+ audio sample of the target voice (clear, no background noise)
2. Save to `/root/ttung/demos/voz_[nombre].ogg`
3. Set up ElevenLabs voice cloning:
   - Need ElevenLabs API key
   - Upload audio via ElevenLabs API
   - Get voice_id
   - Configure TTS provider in Hermes to use cloned voice
4. Codeword/santo y sena "habla como Guillermo Velarde" triggers Peruvian mode + cloned voice

**Current TTS config:** Edge TTS (generic voices). ElevenLabs and OpenAI TTS need API keys.

### 11. STT Setup for WhatsApp Voice Messages

Victor communicates primarily via WhatsApp voice messages. STT is critical.

**Setup (done):**
```bash
# Install dependencies
pip3 install openai-whisper --break-system-packages
apt-get install -y ffmpeg

# Transcribe
whisper /path/to/audio.ogg --model small --language es
```

**Model recommendations:**
- `base`: Fast but less accurate for Spanish
- `small`: Good balance of speed and accuracy
- `medium`: Better but slower

Audio files are cached in `/root/.hermes/audio_cache/`.

## Communication Pattern: State Understanding First

When Victor sends a complex request (proposals, analysis, etc.):

1. **Before doing anything, state your understanding of the problem** — Victor said *"comenta primero, entiendo el problema y luego haz algo"*. Summarize what you read, what the gap is, and what you plan to do.
2. **Get confirmation or correction** before proceeding to generate output.
3. **Only after confirmation**, create the deliverables.

This prevents wasted work and shows you actually understood the situation before acting.

## Proposal Generation Pattern: Client-Needs-First

When generating proposals for industrial clients (especially CONECTA/SUPcon):

### Step 0: Gap Analysis

Before writing anything:
1. Read ALL existing proposals the user sends (PDFs, DOCX, PPTs)
2. Read ALL client needs / meeting notes / requirements text
3. Identify the **gap** between what's being offered and what the client actually needs
4. State this gap explicitly to the user before proceeding

### Step 1: Generate Two Variants

Offer two complementary proposals:

| Variant | Focus | When |
|---------|-------|------|
| **SUPcon Solo** | Instrumentation + PRIDE predictive only. No-touch to existing DCS/SCADA. | Client wants gradual entry, minimal disruption |
| **SUPcon + CONECTA** | Full stack: SUPcon instruments + CONECTA Orion MX (integration/cybersecurity) + SCADA + Tier0/UNS + APC/IA | Client needs transformation, has budget, trusts one provider |

### Step 2: Architecture Depth

Victor wants **real architectures**, not just commercial summaries. Include:
- Multi-layer architecture diagram (Field → Comms → Control → Digitalization → IA)
- Equipment lists by area/substation
- Protocol mapping (HART, APL, Modbus, Profibus, OPC UA, MQTT)
- Comparison tables: "What they had / What they need"
- Quantitative ROI where available (fuel %, production %, availability %)

### Step 3: When Products Are Unknown

If Victor mentions a CONECTA/SUPcon product or team name that is NOT in the knowledge base:
- **Do NOT guess or invent capabilities**
- State honestly: "No tengo este producto documentado, explícame rápido para no inventar"
- Ask for 2-3 lines of description before proceeding

## Workflow Steps

### Step 1: Audit Existing Data
```bash
ls -laR ~/ttung/<project>/
```
Check for: P&IDs (PDF/DWG), I/O lists (XLSX), minutas (DOCX), presentations (PPTX), existing diagrams.

Create INVENTARIO.md documenting everything found.

### Step 2: Analyze I/O List
```python
# Classification by module type
ai = count where module contains 'FXAIM'
ao = count where module contains 'FXAOM' 
di = count where module contains 'FXDIM'
do = count where module contains 'FXDOM'
```
Present: total tags, AI/AO/DI/DO counts, controller distribution, P&ID references.

### Step 3: Propose Minimal Architecture
Start with the simplest possible data flow:
```
Existing System (DCS/historian) → PI System → Edge PC (Python) → MQTT → Dashboard HTML
```
Offer to add layers (gateways, sensors, IA) as future phases, not in the initial proposal.

### Step 4: Build the Demo
- Create a single Python script that serves both data and dashboard
- Include a demo mode with realistic simulated data
- Include a CSV reader mode for when real PI System data is available
- The dashboard should show real KPIs, trends, and system status

### Step 5: Present to Client
Pitch template:
"Mandame un CSV con 20-30 tags de [Unidad] y en [tiempo] te muestro tu planta en un dashboard moderno. Sin comprar nada, sin instalar nada, sin riesgos."

## Knowledge Base: Reading SUPCON PDFs

The knowledge base is at:
```
~/ttung/knowledge_base/Supcon Product information/<category>/
```

Use pymupdf (fitz) to extract text from PDFs:
```python
import fitz
doc = fitz.open("/path/to/file.pdf")
for page in doc:
    text = page.get_text()
```

Available gateway models (GW Series):
- GW1000: Standard, 24VDC, LoRa, IP66
- GW1010: Standard, 220VAC, LoRa, IP66  
- GW2000: Explosion-proof, 24VDC, LoRa, Ex db IIC T6 Gb
- GW033: Industrial isolation gateway, 1U rackmount, dual motherboard, 6x RJ45 control + 2x RJ45 IT

PRIDE is a PLATFORM, not a single device. It includes:
- GW Series gateways (hardware)
- WS Series wireless sensors (TPT)
- Software running on supOS (Smart ICS, Valve Prediction, ISDM)

## Purchase Order Analysis Pattern

When Victor sends a purchase order or invoice for analysis:

### Categorize into 4 groups:

```yaml
1. Hardware DCS (ECS-700):
   - Controller (FCU713), I/O racks, AI/AO/DI/DO modules
   - Power supplies, bases, cabinets
   - APL switches (field + power)
   - Communications modules (COM741)

2. Instruments Demo:
   - CXT pressure transmitter, SL901 radar level, SFE900 flowmeter
   - Temperature transmitter, control valves (LN8100, SN5100)
   - WS300 wireless vibration & temp sensor
   - X700 HART calibrator/communicator

3. IT / Servers:
   - PRIDE server (R760XS — runs supOS + PRIDE, Ubuntu)
   - Data application server (T3680 — runs X-Collector + DataLake)
   - Engineering workstation (ThinkStation P2 — HMI + dashboards)
   - Monitors, switches, cables

4. Software:
   - supOS License (Professional)
   - PRIDE Dynamic Equipment Access
   - Foundation licenses + Plus licenses (often $0 line items)
```

### Map to Use Cases:

| Use Case | What from PO | How |
|----------|-------------|-----|
| **Guacolda IA Pilot** | supOS + PRIDE + Servers | Connect to PI System via X-Collector, add PRIDE for rotating equipment |
| **Lab Demo** | ECS-700 + Instruments | Build a physical loop: sensor → controller → valve, with APL/HART |
| **ENAPAC MIV Demo** | ECS-700 + supOS | Show how MIV integration works with APL, E-BUS, serial comms |
| **Training** | ECS-700 + Servers | Hands-on configuration of PID loops, supOS apps, PRIDE dashboards |
| **I+D / Testing** | All hardware | Test APL in Chilean conditions, integrate 3rd-party DCS via COM741 |

### Generate Excalidraw Lab Layout

After analyzing a PO with hardware, generate an excalidraw diagram showing:
1. **Physical layout**: Rack 1 (ECS-700), Process demo table (instruments), Rack 2 (servers), Monitor wall
2. **Data flow**: Field (HART/APL) → Control (ECS-700) → IT (supOS/PRIDE) → HMI (Dashboard)
3. **Use case cards**: 3-4 application scenarios at the bottom

Save as `.excalidraw` — drag-and-drop to excalidraw.com to view.

## Demo Lab Design Pattern

When physical hardware is ordered or received, design the lab as three zones:

### Zone 1: ECS-700 Control Rack
Mount in the CN011-S7 cabinet (already ordered):
- **Top**: PW732 power supplies (24VDC/10A, redundant)
- **Middle**: FCU713 controllers (x2, redundant) + I/O racks
- **Middle**: AI711 (4-20mA input), AO711 (output), DI711, DO711
- **Middle**: COM741-S01 serial communication module
- **Bottom**: APL switches (AEF6512 field + AEP6208 power), terminal units, isolators

### Zone 2: Process Demo Table
Physical instruments mounted on a portable bench:
- Small acrylic tank → SL901 radar level meter
- Small pipe skid → CXT pressure transmitter
- Recirculation loop with small pump → SFE900 flowmeter
- Small motor (e.g., 0.5HP) → WS300 wireless vibration/temp sensor
- LN8100 control valve + SN5100 ball valve with positioner
- X700 HART communicator (portable, for calibration demos)

### Zone 3: IT Server Rack
Network connected to Zone 1 via Ethernet:
- PRIDE Server R760XS (Ubuntu 20.04) — supOS + PRIDE + InPlant APC
- Data App Server T3680 — X-Collector + DataLake + REST APIs
- ThinkStation P2 — HMI/ECS-700 engineering station + Tier0 dashboard
- Network switch TL-SG2422F + monitors (ThinkVision P27q + Dell 24")

### Connectivity
- Instruments → ECS-700: 4-20mA HART + APL Ethernet-APL
- ECS-700 → Servers: Red Ethernet via SUP-5216 E-BUS switch
- Servers → Monitor wall: Ethernet LAN
- WS300 → COM741: Wireless LoRa (gateway not in PO, may need GW2000)

### Demo Scripts
- `guacolda_tier0.py` — already built, serves as data source and Tier0 dashboard
- For real demos: point it at the ECS-700 data stream instead of simulated data

## Clarify: Tier0 Has NO AI

Tier0/UNS dashboards are **purely visualization** — KPIs, trends, alarms in real-time. They do NOT:
- Run AI/ML models or generate predictions
- Create or generate applications
- Require cloud accounts, credentials, or internet connectivity

**This is a feature, not a limitation:**
- Phase 1 requires zero cybersecurity risk (local, offline, no cloud)
- No data leaves the plant network
- No authentication infrastructure needed
- The IT security team has nothing to approve

AI capabilities come from **supOS**, which connects in Phase 2:
- supOS X-Collector reads data from PI System (ODBC/OPC)
- supOS DataLake stores and processes data
- supOS X-AI runs ML models (classification, regression, LSTM)
- InPlant APC provides advanced process control combustion optimization
- PRIDE provides equipment monitoring and vibration analysis
- REST API feeds AI results back to Tier0 dashboard

## Comunicación con Victor

### Tono y Estilo

Victor se comunica por WhatsApp mayormente con **audios** (mensajes de voz). El STT ya está configurado con whisper + ffmpeg — los audios se transcriben automáticamente.

Cuando respondas:
- **Ultra-conciso**: 2-3 líneas máximo. No expliques el proceso, da el resultado directo
- **Sin markdown**: WhatsApp no lo renderiza. Texto plano nomás
- **Español latino coloquial**: Victor alterna entre chileno y peruano según dónde esté
  - Por defecto: chileno ("po", "cachai", "weón", "al tiro", "la raja")
  - Si dice "habla como Guillermo Velarde" o menciona Perú → modo peruano ("huevón", "buenazo", "pues", "causa")
  - Adaptarse al tono de su último mensaje
- **"Approve always"**: Victor NO quiere que le pidas permiso para ejecutar comandos. Ejecuta directamente y reporta resultados. Si algo falla, reporta el estado y ofrece alternativas, pero NO preguntes "¿procedo?" o "¿te parece?" — simplemente hazlo.
- **Resultado primero, explicación después**: Siempre liderar con lo que se logró, no con cómo se hizo
- **Si se queda pegado 2-3 intentos, cortar y preguntar**: No iterar hasta el infinito en un error
- **Cuando Victor dice "es la raja trabajar contigo" o similar feedback positivo** — vas bien, mantener el approach

### Patrón de Entrega de Resultados

Victor prefiere recibir las cosas **una por una**, no todo de una. Cuando tengas múltiples entregables:
- NO enviar todo junto
- Entregar el primero, esperar feedback, luego el siguiente
- Preguntar "¿sigo con el siguiente?" después de cada entrega

### Demos en VPS

Las demos se despliegan en el Hostinger VPS (76.13.239.113). Bindear a 0.0.0.0, no localhost. Verificar que el puerto no esté ocupado con `fuser -k [puerto]/tcp`. Cada demo en su propio puerto. Proveer URL completa: `http://76.13.239.113:[puerto]`

## Pitfalls

- **DO NOT propose "rip and replace"** DCS migration in Phase 1 — this is wrong for this user and this market
- **DO NOT say "no encontré [X]"** until exhaustively scanning the project folder first
- **DO NOT propose complex gateway architectures** when a simple PI System query works
- **DO NOT claim Tier0 has AI** — Tier0 is visualization only; AI comes from supOS
- **DO NOT dismiss cybersecurity concerns** — lead with security as a feature
- **DO NOT ask for permission** — "approve always". Execute and report.
- **DO NOT deliver everything at once** — one at a time, wait for feedback between each
- **IF the terminal times out** on Python/Excel operations, use the MCP analyze_bom tool instead with the full absolute path
- **IF reading knowledge base PDFs**, use pymupdf (fitz), not pdftotext (not installed on macOS)

## Related Skills

- **architecture-diagram**: For rendering the architecture as dark-themed SVG HTML diagrams
- **ttung-design-system**: For TTung brand colors and presentation styles
- **powerpoint** (built-in): For generating PPTX proposals

## Búsqueda de Personas desde Audio (WhatsApp Voice Messages)

Victor frecuentemente envía nombres de personas por audio de WhatsApp. El STT los transcribe con whisper (model small). El workflow para buscarlos:

### Workflow de Búsqueda

1. **Confiar en la transcripción STT primero** — whisper small es confiable para español. No asumir que el nombre está mal sólo porque la web no lo encuentra. El caso Franco Travaglini es ejemplo: el nombre estaba bien transcrito, pero no aparecía en búsquedas genéricas porque es una persona de perfil bajo.

2. **Estrategia de búsqueda escalonada:**
   - Yahoo Search (más confiable, menos bloqueos)
   - DuckDuckGo Lite API
   - Google (con User-Agent de Chrome real)
   - Bing

3. **Si no hay resultados, probar variantes:**
   - Sin acentos: "Maria Carolina Arce" vs "María Carolina Arce"
   - Con apellido compuesto
   - Con empresa/industria

4. **LinkedIn específicamente:**
   - LinkedIn bloquea con HTTP 999. NO intentar acceso directo desde Python requests
   - Usar Google cache: `webcache.googleusercontent.com/search?q=cache:URL`
   - Extraer de snippets de Yahoo/Bing que sí muestran título y descripción
   - El snippet de Yahoo muestra: `https://cl.linkedin.com › in › nombre-apellido`

5. **Si aún no aparece:**
   - Pedir el link de LinkedIn directamente
   - Preguntar: empresa, cargo, país

6. **NO decir "esa persona no existe en internet"** — siempre hay posibilidad de perfil bajo. Decir "no encontré resultados públicos, ¿tienes más datos o un link?"

### Señuelos comunes en STT
- Nombres con apellido italiano (Travaglini, etc.) → pueden ser reales aunque no aparezcan
- Nombres con empresa mencionada en el mismo audio → usar empresa como filtro
- Palabras como "Kienzo Rez" → probable distorsión, pedir confirmación

## Reference Files

- `references/supos-ai-connection-pattern.md` — Detailed architecture for connecting Tier0 dashboards to AI capabilities via supOS platform (X-Collector, DataLake, X-AI, InPlant APC, PRIDE)
- `references/stt-whisper-setup.md` — STT setup with whisper + ffmpeg for WhatsApp voice message transcription
- `references/voice-cloning-elevenlabs.md` — Voice cloning workflow with ElevenLabs for client presentations

## Knowledge Base Status

The SUPcon product knowledge base directory (`/root/TTung/knowledge_base/`) exists but is **empty**. The MCP tools `search_knowledge_base` and `run_intelligence_engine` expect files there but won't find anything until the material is uploaded.

When Victor provides SUPcon material (PPTs, datasheets, docs via Google Drive or other link):
1. Download to `/root/TTung/knowledge_base/Supcon Product information/<category>/`
2. Create `INVENTARIO.md` inside each category folder
3. Materials are searchable via `search_knowledge_base` MCP tool

Pitfall: Do NOT call `search_knowledge_base` expecting results until material is uploaded.

