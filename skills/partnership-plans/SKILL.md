---
name: partnership-plans
title: Partnership & Collaboration Plans — TTung Ecosystem
description: Build structured partnership/collaboration plans between CONECTA + SUPcon + external partners (RSM, etc.) for the Chilean industrial market. Includes synergy mapping, project targeting, tone calibration, and visual one-pager generation.
trigger: User asks to create a collaboration plan, partnership proposal, alliance document, or joint offering with an external firm in the TTung/CONECTA/SUPcon ecosystem.
domain: Spanish, Chilean industrial context, business development
---

# Partnership & Collaboration Plans

## When to Use
This skill covers creating **business collaboration / alliance plans** between CONECTA + SUPcon and external partners (engineering firms, EPCs, suppliers) for the Chilean market. Typical triggers:
- User says "crea un plan de colaboración con [empresa]"
- User says "mejora el plan de colaboración [X]-CONECTA-SUPcon"
- User needs a "mapa de sinergias", "oferta conjunta", or "proyectos objetivo" for a partnership

## Core Workflow (6-Step Method)

### Step 1: Extract Partner Profile
Read all available documentation about the partner:
- Brochures (PDF → extract text with PyPDF2 or pdftotext)
- Website / LinkedIn if available
- Previous projects mentioned by the user

Build a structured profile covering:
```
- Company name, type (consultora / EPC / fabricante)
- Core services (in bullet list)
- Key projects (client, size, description, $ ticket)
- Key clients (relationships)
- Geographical presence
- Any overlap with CONECTA/SUPcon projects
```

### Step 2: Map Synergies
Create a visual or structured matrix showing:
```
┌─────────────┬──────────┬───────────┬──────────┐
│ Project Type │ RSM does │ CONECTA   │ SUPcon   │
│              │          │ does      │ does     │
├─────────────┼──────────┼───────────┼──────────┤
│ Desaladora  │ Captación │ SCADA/CEN │ DCS/APC  │
│ ...         │ ...      │ ...       │ ...      │
└─────────────┴──────────┴───────────┴──────────┘
```
Focus on where the partner's services **complement** rather than compete with CONECTA/SUPcon.

### Step 3: Find Project Opportunities
Cross-reference the partner's client list and expertise against:
- **MIV Intelligence Engine** (`run_intelligence_engine` tool) — 934+ Chilean projects
- **Project CSV** (`Listado de Proyectos - Process(Lista ).csv`) — live pipeline
- **Memory** — previous TTung project context (ENAPAC, IOC Minero, JAFURAH)

Filter for projects where:
- Partner already has a relationship (warm lead)
- Combined capabilities solve a real problem the partner couldn't solve alone
- Ticket size is worth the coordination overhead ($500K+ recommended)

### Step 4: Set the Tone
**IMPORTANT — Tone is Relationship-Dependent:**

| Relationship | Tone | Meeting Format | Example Opening |
|---|---|---|---|
| **Friend / Known** | Coloquial, cálido, "oye wey" | Café o bar, 45 min + cerveza | "Oye, ¿cómo estai? Te quiero mostrar una weá interesante..." |
| **Cold / Professional** | Formal, estructurado | Oficina, 60 min con deck formal | "Estimado [nombre], hemos identificado una oportunidad de sinergia..." |
| **Warm Introduction** | Semi-formal, referencial | Oficina o videollamada, 45 min | "[Referente] nos sugirió conversar con ustedes..." |

Store the relationship status in memory so future sessions don't default to the wrong tone.

### Step 5: Structure the Plan Document
Always produce a structured markdown document with these sections:

```
# Plan de Colaboración — [Partner] + CONECTA + SUPcon

1. Mapa de Sinergias — Quién Hace Qué
2. Propuesta de Valor Integrada — La Oferta Conjunta
3. Proyectos Objetivo — Dónde Ganar YA (4 projects minimum)
4. OnePager Visual — Referencia de Diseño (layout diagram)
5. Agenda Primera Reunión — Tone-appropriate
6. Sugerencias de Slides para la Presentación (9 slides min)
7. Próximos Pasos Inmediatos (con responsables y fechas)
```

Each project in section 3 must include:
- **Client** name
- **Partner already there?** (yes/no with evidence)
- **What each party contributes** (specific scope)
- **Ticket estimate** in USD
- **Entry angle** (how to approach the client using partner's relationship)

### Step 6: Generate Visual Output
Always produce two files:
1. **Markdown plan** (`[PARTNER]_CONECTA_SUPcon_Plan_Colaboracion.md`) — full document
2. **HTML OnePager** (`[PARTNER]_CONECTA_SUPcon_OnePager.html`) — visual, printable, A4 format

OnePager HTML specs:
- Load the **`ttung-design-system`** skill (`creative/ttung-design-system`) for the full color palette, typography scale, card specs, and layout patterns
- Key palette: Navy `#0A1F3F`, Blue `#0088CC`, Teal `#00967A`, Cyan `#00B8D9`, BG `#F4F5F8`, Text `#1A1A2E`, Border `#D0D4D8`
- Font: Inter (Google Fonts)
- A4 page size (210mm x 297mm) with print CSS
- Components: header with 3 logos, hero section, 3 capability cards, project grid, KPI row, footer
- Each partner card gets a colored accent bar matching their brand
- Project cards show client name, entry angle, and ticket estimate

## Pitfalls to Avoid
- ❌ Using formal "Estimado" tone when partner is a known contact — feels cold, kills the vibe
- ❌ Listing projects where the partner *competes* with CONECTA/SUPcon — makes them suspicious
- ❌ Suggesting partner be a "subcontractor" — propose "aliados" or "socios" model
- ❌ Giving only abstract ideas — each project must have a dollar figure
- ❌ Skipping the one-pager — the user expects visual collateral to bring to meetings
- ❌ Writing the opening line in English when working in Spanish context

## Verification
- [ ] Partner profile extracted and accurate
- [ ] Synergies mapped (complementary, not competitive)
- [ ] At least 4 projects with concrete entry angles
- [ ] Ticket estimates for each project
- [ ] Tone matches relationship (coloquial for friends, formal for cold)
- [ ] Markdown plan saved
- [ ] HTML OnePager generated
- [ ] Meeting agenda with venue suggestion
- [ ] Slide structure for presentation deck
