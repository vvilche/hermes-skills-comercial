---
name: industrial-research-workflows
description: Industrial research workflows — market analysis, context compression, partner profiling, and technical scoping for the TTung portfolio. Umbrella skill for all industrial-domain research activities.
category: ttung-projects
tags: [industrial, market-analysis, context-compression, ttung, supcon, conecta]
triggers:
 - Victor pide un "estudio de mercado" o "análisis competitivo"
 - Hay que procesar BOMs, specs técnicas, PDFs industriales o logs
 - Antes de consolidar una knowledge base para DeepSeek/Hermes
 - Compresión de contexto previa a inyección
---

# Industrial Research Workflows

Umbrella skill for industrial-domain research: market analysis, context compression, gap extraction, partner profiling, and technical scoping.

## 1. Context Compression

**Purpose:** Reduce token footprint of industrial documents before injecting into LLMs (DeepSeek, Hermes). Conserves specs, models, standards, and numbers while removing filler.

**Trigger:** Specs, PDFs, BOMs, or logs exceeding ~10K chars that would overflow context windows.

**Tool:**
```bash
python3 /root/TTung/context_compressor.py compress archivo.pdf
```

**Rules:**
- **Preserve:** Numbers, dimensions, model codes (`Vizimax PMU 6020`), standards (`IEC 61850`), manufacturers (`ABB, Siemens`)
- **Discard:** Intros, historical backstories, verbose descriptions
- **Format:** Ultra-dense bullets (`• Model X: 220V, 50Hz, API-6D`)
- **Caching:** Content-keyed (SHA256) — same input → cached output

**Performance:**
- Ver [references/pmu-cge-compression-case.md](references/pmu-cge-compression-case.md): 20K chars → 1.4K (93% savings)

**Pitfalls:**
- Re-check numbers after compression (LLMs sometimes reword tables)
- PDFs that are images → OCR first (tesseract)
- ❌ Avoid double-compression — cache prevents redundant DeepSeek calls

## 2. Market Analysis

**Purpose:** Generate Victor-approved "agentes IA" style HTML reports — 10-section research artifacts covering market size, competitive landscape, gaps, FODA, strategy, and verdict.

**Sections (strict order):**
1. **Resumen Ejecutivo**: 4 KPI cards + insight
2. **Segmentos** (`Field Instrumentation`/`Control Systems`/`Software`): Product-cards grid
3. **Tamaño de Mercado**: Total + CAGR + share projection
4. **Competencia**: Pricing table + advantage assessment
5. **Gaps**: Critical/High/Medium/Opportunity cards
6. **FODA por Segmento**: 2×2 grid (Fortalezas/Debilidades/Oportunidades/Amenazas)
7. **Estrategia (Acento vs Codo)**: Victor’s framework — quality vs volume
8. **Plan de Acción**: 12-month roadmap
9. **Proyección Revenue**: 3-year CLP/USD
10. **Veredicto**: 4 KPI cards + go/no-go

**Visual Rules:**
- Theme: Dark (`#0a0a12` bg, `#1a1a2e` cards)
- Numbers **must** cite source — every KPI/price/estimation  
- Self-contained HTML (all CSS inline)
- ❌ No TTung jargon (`Caje`, `Reanual`) — ALWAYS translate: `Field Instrumentation`, `Control Systems`, `Software`

**Sample Workflow:**
```bash
# Extract CONECTA pricing from PDFs
python3 -m fitz extract OT-5219-00/"4 Oferta"/carta_adjudicacion.pdf | grep "UF"

# Generate analysis
cd /root/TTung/market-analysis-framework && python3 generate.py --template "supcon-3-lineas" --output 2026-Q3-Estudio.html
```

**Pitfalls:**
- ❌ Never omit the "Acento vs Codo" section — Victor’s signature framework
- ❌ Never use light theme — dark is approved for market analysis
- ❌ Never ask "te parece?" — generate, serve, notify

## 3. Gap Analysis & Opportunity Prioritization

**Purpose:** Extract and prioritize market gaps from specs, proposals, and competitor documentation.

**Tool:** Built into `context_compressor.py`:
```bash
python3 /root/TTung/context_compressor.py extract-gaps "especificaciones_PPU_2026.pdf"
```

**Output:** CSV with columns:
| Gap Text | Segment | Severity (1-4) | Evidence |

**Prioritization Matrix:** See [references/gap-analysis-priority-matrix.md](references/gap-analysis-priority-matrix.md)

## 4. Partner Profiling

**Purpose:** Research industrial partners (contractors/integrators) before meetups. Pull from:
- ChileCompra scraping (subcontractors)
- CONECTA OT PDFs (recipient companies)
- Public tenders (`mercado publico`)

**Tool:**
```bash
# Core integrators for CONECTA (Chile-only)
python3 /root/TTung/chilecompra-scraper/scrape_partners.py --contract_type "obra" --region "RM"
```

**Output:** JSON with:
```json
{
  "integrator": {
    "name": "ELECNOR",
    "contracts": 34,
    "last_contract_date": "2026-05-15",
    "specialty": "subestaciones",
    "shared_OTs": ["OT-5219-00"],
    "channel": "whatsapp:+569...
  }
}
```

## 5. Research Slash Commands

Quick-reference guide for combining Hermes slash commands (`/goal`, `/skill`, `/background`, `/cron`, `/busy steer`) with research skills. See [references/slash-command-research-workflows.md](references/slash-command-research-workflows.md).

## 6. Prompt Engineering para Investigación

Guía validada con Victor para escribir prompts de investigación efectivos. Estructura de 4 dimensiones (QUÉ, PARA QUÉ, FORMATO, LÍMITES) + ejemplos para inteligencia competitiva, licitaciones, contenido, y decisiones de negocio. Ver [references/prompt-engineering-investigacion.md](references/prompt-engineering-investigacion.md).

## Pitfalls Across Workflows

- **Numbers Must Cite Source**: Victor’s mantra — “¿de dónde sacaste el número weón?“
  Sources: CONECTA OT PDFs, ChileCompra scraping, SUPcon catalog, industry benchmarks

- **No Jargon**:
  | Banned | Use Instead    |
  |--------|----------------|
  | Caje   | Field Instrumentation |
  | Reanual| Control Systems |
  | Rendimi| Software |

- **Venezuela Context**:
  - Region-specific pricing (`Bs → USD` conversions, inflation-adjusted)
  - Integrator networks (`METROCAR`,`LITEC`) instead of Chilean contractors
  - Electrical grid codes (`CORPOELEC`)
  - See [Conecta América Framework](references/conecta-america-framework.md)

- **Error Handling**:
  - PDF extraction failures → fallback to OCR (`tesseract`)
  - DeepSeek rate limits → retry after 180s

## Template & Script Hub

### `templates/`
- `market-analysis.html`: Victor-approved 10-section template
- `gap-prioritization.csv`: Pre-formatted export
- `partner-profiling.json`: Schema

### `scripts/`
- `context_compressor.py`: Context compression, caching, gap extraction
- `market-generator.sh`: Wrapper for Victor’s pipeline (`extract → compress → generate → serve`)
- `conecta-project-analyzer.py`: BOM/pricing extraction from OT PDFs
