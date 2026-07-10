---
name: conecta-business-intel
description: "Scan competitors, tenders, and opportunities for CONECTA Chile."
version: 0.4.0
author: Hermes
platforms: [linux, macos]
metadata:
  hermes:
    tags: [CONECTA, Competitive, Business, Chile, Tenders, Mining]
---

# CONECTA Business Intelligence Scanner

Radar continuo de oportunidades y competidores. Usa AIsa search API (GPT-5-search) + fuentes directas.

## Search Sources (25+ verified)

### Priority 1 — Early Detection (12-24 meses antes)
| Source | URL | What It Finds |
|---|---|---|
| **SEIA** | sea.gob.cl | DIA/EIA ingresados — primera señal |
| **SIG SEA** | sig.sea.gob.cl | Mapas de proyectos en evaluacion |
| **Portales Compras Mineras** | ver `portales-mineras.md` | Licitaciones directas (CODELCO, SQM, Collahuasi, Gold Fields, Glencore) |
| **Diario Antofagasta** | diarioantofagasta.cl | Anuncios mineros locales |
| **Diario Atacama** | eldiariodeatacama.cl | Proyectos mineria/energia regional |
| **Diario Concepcion** | diarioconcepcion.cl | Celulosa, industria Biobio |

### Priority 2 — Industry News (3-6 meses antes)
| Source | URL | Frequency |
|---|---|---|
| **Reporte Minero** | reporteminero.cl | Daily |
| **Portal Minero** | portalminero.com | Daily |
| **Mundo Minería** | mundomineria.cl | Daily — cobertura EXPONOR, ferias, tecnología minera |
| **ElectroIndustria** | emb.cl/electroindustria | Daily |
| **Redimin** | redimin.cl | Weekly |
| **MCH** | mch.cl | Monthly |
| **NME** | nuevamineria.com | Monthly |
| **Mas Mineria Energia** | masmineriaenergia.cl | Weekly |

### Priority 2b — Energy & Transmission Specialized
| Source | URL | What It Finds |
|---|---|---|
| **Energía Estratégica** | energiaestrategica.com | Licitaciones transmisión, CEN, proyectos utility |
| **Revista Electricidad** | revistaei.cl | Adjudicaciones CEN, subestaciones, marco regulatorio |
| **Guía Chile Energía** | guiachileenergia.cl | Tecnología SE, digitalización, productos (Schneider, ABB) |
| **Rumbo Minero** | rumbominero.com | Cobertura minera LatAm, transacciones corporativas (BHP) |
| **Review Energy** | review-energy.com | Política energética, Ruta Energética, transición |
| **En Línea** | enlalinea.cl | Acuerdos corporate (CODELCO-Schneider, etc.) |

### Priority 3 — Energy & Regulation
| Source | URL |
|---|---|
| **M. Energia Reporte** | energia.gob.cl/panel/reporte-de-proyectos |
| **CNE Normas Tecnicas** | cne.cl/normativas/electrica/normas-tecnicas/ |
| **BCN Ley Chile** | bcn.cl/leychile |
| **CEN Obras Nuevas** | coordinador.cl/desarrollo/documentos/licitaciones/nuevas/ |

### Priority 4 — Bidding & Social (1-3 meses antes)
| Source | URL |
|---|---|
| **ChileCompra** | mercadopublico.cl |
| **LinkedIn mineras** | ver `portales-mineras.md` (12 mineras con LinkedIn y portales de compra) |
| **Twitter/X** | x.com (via AIsa search API) |

### Mining Company Procurement Portals (Direct Sources)
| Minera | Portal | Plataforma |
|---|---|---|
| CODELCO | codelco.com/proveedores/portal-de-compras | Propio |
| SQM | sqm.com/portal-proveedores/ | Propio |
| Collahuasi | proveedores.collahuasi.cl | Propio |
| Gold Fields | goldfields.cl/proveedores-locales/ | Propio |
| Glencore Chile | glencore.cl/abastecimiento-y-proveedores | Propio |
| BHP | SAP Ariba | Ariba |
| Antofagasta Minerals | SAP Ariba | Ariba |

### Early Signal Chain
```
SEIA (DIA/EIA) → Portales Compras Mineras → Diarios Regionales → Revistas → LinkedIn hiring → ChileCompra
  12-24 meses        6-12 meses                6-12 meses        3-6 meses    1-3 meses      licitacion
```

## AIsa Search Queries (for gpt-5-search-api)

Run via: `curl https://api.aisa.one/v1/chat/completions` with model `gpt-5-search-api`

1. "Proyectos de automatizacion DCS SCADA en mineria chilena 2026. Contratos adjudicados."
2. "Proyectos de transmision electrica, subestaciones y BESS en Chile 2026."
3. "Actividad de Emerson, Yokogawa, Honeywell, ABB, Siemens en Chile/LatAm 2026."
4. "Tendencias digitalizacion minera Chile 2026: automatizacion, IA, mantenimiento."

## Mercado Público Tender Scan (ChileCompra)

Quick scan for industrial automation opportunities. Run on a cron or on demand.

### ⚠️ PITFALL: ChileCompra NO indexa licitaciones industriales de automatización

Las búsquedas con `site:mercadopublico.cl` + SCADA, RTU, DCS, protecciones diferenciales, IED, telecontrol arrojan **0 resultados**. Las licitaciones de automatización industrial/eléctrica NO pasan por ChileCompra — van por portales privados:

| Portal | Tipo | Acceso |
|---|---|---|
| **Portal de Compras CODELCO** + SAP Ariba | Automatización, instrumentación | Registro proveedor |
| **Unilink** (Celeo, transmisoras) | Obras de transmisión | Plataforma privada |
| **SAP Ariba** (BHP, AMSA) | Compras mineras | Invitación |
| **Portales propios** (Collahuasi, Gold Fields, Glencore) | Compras directas | Registro proveedor |

**Conclusión:** Para automatización industrial, saltar ChileCompra. Ir directo a portales mineros, Unilink, y CODELCO Portal de Compras. ChileCompra solo sirve para obras civiles pequeñas con componentes eléctricos menores (alumbrado, empalmes <100 KVA).

### Keywords (search in parallel batches — para obras menores, no automatización industrial)
```
BATCH 1 (core): automatizacion, SCADA, "sistema control distribuido" OR DCS, RTU "unidad terminal remota", subestacion electrica
BATCH 2 (extended): PLC "control industrial", instrumentacion, telemetria, "sistema supervision", telecontrol, HMI, "tablero control", "grupo electrogeno", transformador
```

### Procedure (cuando aplique — obras civiles/eléctricas menores)
1. `web_search` with `site:mercadopublico.cl` + each keyword, 5 searches in parallel
2. `web_extract` the 4-5 most promising results (filter for `Publicada` / open status)
3. Identify: ID, title, organismo, cierre date, keywords, status
4. Output table with 4 sections: 🟢 Abiertas, 🟡 Cerradas recientes, 🔴 Desiertas, 📊 Resumen
5. Always include direct URLs to each `DetailsAcquisition.aspx` page
6. Highlight tenders closing within 72 hours with ⚠️

### Output format
Use the template in `references/tender-table-format.md`. Always Spanish. Always include the scan date.

## Weekly Procedure

1. Run 4 AIsa searches (parallel)
2. Run Mercado Público tender scan (keyword batches)
3. Scan ElectroIndustria homepage
4. Classify: Leads / Tendencias / Competidores
5. Compile to ~/.hermes/intel/YYYY-MM-DD.md
6. Regenerate dashboard + push to conecta-intel repo

## Fallback cuando AISA no tiene saldo

Si AISA devuelve `"insufficient wallet balance"`, NO reintentar. Pasar inmediatamente a:

1. **web_search** en 3-4 frentes paralelos con keywords específicas por competidor
2. **web_extract** en los 2-3 resultados más prometedores de cada frente
3. Buscar en fuentes sectoriales chilenas: Reporte Minero, Forbes Chile, MCH, Nueva Minería
4. Verificar EXPONOR (exponor.cl) — feria bienal donde todos los competidores tienen presencia
5. Incluir en el reporte: "Nota: API AISA sin saldo — búsquedas vía web_search tradicional"

## Company Deep-Dive (Pre-Contact Research)

When Victor gets a contact from a company and needs to prepare a response, run this 5-phase research BEFORE drafting anything:

### Phase 1: Company Basics (2-3 parallel searches)
1. "[Empresa] RUT razón social Chile" — legal identity
2. "[Empresa] proyecto inversión etapa 2026" — what they're building
3. "[Empresa] dueños accionistas controlador" — who really owns it

Key sources: vlex.cl (RUT), codelco.com/filiales (if CODELCO-linked), bnamericas.com, guiaminera.cl

### Phase 2: Executives & Org Chart
Search: "[Empresa] gerente CEO LinkedIn equipo"
Find: CEO/Gerente General, head of the contact's department, procurement/contracts head

### Phase 3: Project Status
Search: "[Proyecto] DIA EIA SEIA construcción contratistas"
Determine: environmental permits approved?, construction started?, who are the EPC/contractors?

### Phase 4: Opportunity Mapping
Based on the contact's role, map to CONECTA services:
- Hidrogeología → pozos, bombeo, SCADA, instrumentación campo
- Mantención → monitoreo condición, predictivo, Tier0
- Ingeniería → DCS, protecciones, PMU, SCADA
- Abastecimiento → cotizaciones, licitaciones en curso

### Phase 5: Relationship Map
- Is the company a CODELCO filial? (check codelco.com/transparencia/filiales)
- Any existing CONECTA contacts there?
- Any competitors already positioned? (search "[Empresa] Emerson Yokogawa Siemens ABB contrato")

Ver `references/competitor-profiles-chile.md` para el mapa actualizado de los 3 grandes (Emerson, Yokogawa, Honeywell) en minería chilena — centros de excelencia, JVs, EXPONOR presencia, y ventanas de oportunidad para CONECTA/SUPcon.

## SE Retrofit & Modernización Research

Ver `references/se-retrofit-research.md` para la metodología de 4 frentes (proyectos, licitaciones, competidores, normativa) aplicada a modernización de subestaciones eléctricas. Incluye queries específicos, fuentes clave, y template de reporte.

### Queries de competitor monitoring (para cron jobs)

Correr 3 queries en paralelo (AISA o web_search fallback):

```
1. "Emerson Automation Solutions Chile minería contrato DCS DeltaV 2026"
2. "Yokogawa Chile minería CENTUM contrato automatización 2026 Exponor"
3. "Honeywell Kairos Mining Codelco Chile automatización 2026 contrato"
```

### Fuentes clave para competitor intel

| Fuente | URL | Qué entrega |
|---|---|---|
| Forbes Chile | forbes.cl | Entrevistas ejecutivos Honeywell/Emerson |
| Reporte Minero | reporteminero.cl | Noticias diarias minería + proveedores |
| MCH | mch.cl | Anuncios centros I+D, CORFO, innovación |
| Nueva Minería | nuevamineria.com | Cobertura Yokogawa, centros excelencia |
| EXPONOR | exponor.cl | Presencia en feria (todos compiten aquí) |
| México Business | mexicobusiness.news | Entrevistas Honeywell LatAm mining |
