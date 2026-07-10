---
name: aisa-deep-research
description: "Deep research on mining projects, competitors, regulations. Daily cron intel for CONECTA."
version: 0.1.0
platforms: [linux, macos]
metadata:
  hermes:
    tags: [AISA, intelligence, sales, mining]
---

# aisa-deep-research

Automated business intelligence and competitive research for CONECTA Ingeniería
in the Chilean mining/industrial market.

## Two Execution Modes

### Mode A: Native web_search + web_extract (CRON / AUTONOMOUS)
For scheduled jobs and autonomous runs where no user is present. Uses Hermes
native tools — faster, no API call overhead, no timeout issues.

### Mode B: AISA API (INTERACTIVE / DEEP)
For deep research with LLM synthesis. Use when Victor is present and asks for
analysis, not just data gathering.

- Provider: custom_aisa
- Base URL: https://api.aisa.one/v1
- Model: `gpt-5-search-api` (has live search; gpt-5.2 and claude do NOT)
- Do NOT pass `temperature` parameter to gpt-5-search-api
- Timeout: 180s minimum
- Always request source URLs in output

## Daily Mining Intel (CRON Workflow)

Trigger: cron job for daily mining intel report. No user present — execute fully
autonomously, make reasonable decisions, deliver report as final response.

### Companies to Track
CODELCO, BHP, SQM, Anglo American, AMSA (Antofagasta Minerals), Capstone, ENAMI.

### Workflow (5 phases, ~4-5 min total)

**Phase 1: Parallel Web Searches (4 queries simultaneously)**
1. "CODELCO BHP Anglo American nuevos proyectos mineros Chile 2026 inversiones"
2. "SQM AMSA Capstone ENAMI minería Chile proyectos noticias 2026"
3. "licitaciones mineras Chile 2026 contratos adjudicados minería"
4. "competidores instrumentación control industrial minería Chile 2026 contratos"

### Phase 2: Extract Key Articles
Pick the 4-6 most data-dense results from Phase 1. Extract with web_extract.
**Always include codelco.com/licitaciones-en-proceso in extraction** — CODELCO publishes time-sensitive automation/instrumentation tenders here (e.g., WS2134054164 "ADQUISICION EQ. AUTOMATIZACION, CONTROL E INSTRUMENTACION" published 16/04/2026). These are direct sales leads for CONECTA.

**Phase 3: Fill Gaps**
Targeted searches for companies with thin results. SQM litio, BHP Escondida/Spence,
AMSA Centinela, ENAMI fundición, CODELCO licitaciones instrumentación.

**Phase 4: Extract Deep Content**
Extract the gap-filling articles. Focus on: investment amounts, timelines,
awarded contracts, competitor names.

**Phase 5: Compile Report**
Use the template in `references/reporte-diario-template.md` (view with
`skill_view(name='aisa-deep-research', file_path='references/reporte-diario-template.md')`).
1. CODELCO + Anglo American
2. BHP
3. SQM + CODELCO (Litio)
4. AMSA
5. Capstone + ENAMI
6. ENAMI (fundición)
7. Licitaciones Relevantes
8. Competidores
9. Resumen Oportunidades para CONECTA

### Report Rules
- Spanish only
- Every number must have a source URL cited in the same section
- Include "Próxima actualización: [fecha mañana]"
- Table format for key data (Dato | Detalle)
- If no relevant news for a company, say "Sin novedades relevantes esta semana"
- Deliver as final response (cron handles delivery)

## Competitor Deep-Dive Workflow

When Victor or a cron job triggers competitor-specific research (not general mining
intel), use this focused workflow. Targets: Emerson, Yokogawa, Honeywell, ABB,
Siemens, Schneider Electric.

### Phase 1: Parallel Broad Searches (4 queries simultaneous)
Run these patterns, replacing `[COMPETIDOR]` with the target name:
1. "[COMPETIDOR] contrato adjudicado Chile minería automatización 2025 2026"
2. "[COMPETIDOR] Chile contrato DCS sistema control CODELCO BHP 2024 2025 2026"
3. "[COMPETIDOR] proyecto Chile minería instrumentación control 2025 2026"
4. "licitación instrumentación control industrial Chile minería 2025 2026 adjudicada Emerson Yokogawa Honeywell" (all-competitor sweep)

### Phase 2: Extract High-Density Sources
Priority extraction targets (in order):
1. **codelco.com/licitaciones-adjudicadas** — direct evidence of competitor contracts. Search page content for competitor names (HONEYWELL CHILE S A, etc.)
2. **yokogawa.com/library/resources/references/** — Yokogawa publishes detailed customer stories with contract scope, tonnage, timeline
3. **reporteminero.cl** — best Chilean mining portal for competitor interviews and event coverage
4. **portalminero.com** — mining events and partnerships
5. **mch.cl** (Minería Chilena) — industry alliances and distribution deals

### Phase 3: Gap-Fill Searches
For any competitor with thin results, run targeted searches:
- "[COMPETIDOR] Chile litio SQM Albemarle automatización 2025 2026"
- "[COMPETIDOR] Kairos Mining CODELCO" (Honeywell-specific JV)
- "[COMPETIDOR] Chile centro excelencia minería"
- "Siemens ABB Schneider contrato Chile minería automatización 2025 2026" (adjacent competitors)

### Phase 4: Compile Threat-Level Report
Deliver as standalone report (not embedded in daily mining intel). Format:
- **🔴 AMENAZA ALTA / 🟠 MEDIA-ALTA / 🟡 MEDIA / ⚪ MONITOREAR** per competitor
- Table: Dato | Detalle for each competitor
- Section: Movimientos clave, Ejecutivos clave, Fuentes
- Section: Contexto mercado (pipeline inversión, JVs activas)
- Section: 🎯 Oportunidades para CONECTA
- Section: ⚠️ Amenazas
- Every fact with linked source URL
- Spanish only

### Competitor-Specific Intel Sources
- **Honeywell**: CODELCO licitaciones page, Kairos Mining JV, Mining Summit events
- **Yokogawa**: yokogawa.com customer stories, MCH.cl alliance articles, Egaflow distribution coverage
- **Emerson**: emerson.com lithium pages, EXPONOR/APRIMIN presence, CORFO I+D center
- **ABB**: new.abb.com/news Chile section, Exponor coverage, Minería Digital congress
- **Siemens/Schneider**: MoU announcements with CODELCO, energy efficiency events

## Pitfalls
- **AISA API key 401**: The API key in config.yaml may be partially redacted (`sk-ais...HsFU`). If AISA API returns 401, fall back IMMEDIATELY to Mode A (native web_search) — don't debug the key, don't retry. Mode A is the correct mode for cron anyway.
- web_extract on some sites (ialicitaciones.com) may return 404 or cookie walls — skip, don't retry
- Instagram/Facebook results have low data density — prefer news portals (Reporte Minero, Mundo Minería, BNamericas, Rumbo Minero, Portal Minero)
- CODELCO licitaciones page has pagination — focus on most recent 3 months
- Some articles are in English — translate key data to Spanish for the report
- Never fabricate investment numbers — if unclear, mark "Monto no informado"
- **AISA API timeout**: If using Mode B and the call hangs, cut at 120s and fall back to Mode A. The 180s recommendation is aspirational; 120s is the practical limit for cron jobs.
- Emerson rarely announces Chilean mining contracts publicly — their presence is more event/brand focused. Don't treat "no contracts found" as "no activity" — look for I+D centers, CORFO partnerships, EXPONOR presence as activity signals
- Honeywell's Kairos Mining JV with CODELCO means they often win contracts through the JV vehicle, not as Honeywell directly — check both names in licitaciones
- Yokogawa customer stories on yokogawa.com are excellent but may be 6-12 months old — supplement with Chilean news portals for recency
- See `references/competidores-chile-2026.md` for detailed profiles of Emerson, Yokogawa, Honeywell, ABB — execs, contracts, structures, and market context
