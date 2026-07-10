---
name: aisa-marketpulse
description: "Monitor mining market trends: copper, lithium prices, investment flows, sector health."
version: 0.1.0
platforms: [linux, macos]
metadata:
  hermes:
    tags: [AISA, intelligence, sales, mining]
---

# aisa-marketpulse

Uses AISA API (marketpulse) for automated intelligence gathering.

## API Configuration
- Provider: custom_aisa
- Base URL: https://api.aisa.one/v1
- API Key: configured in config.yaml

## Usage
Load this skill when doing automated business intelligence, competitive research, 
or sales prospecting for CONECTA Ingeniería in the Chilean mining/industrial market.

## Model
Use `gpt-5-search-api` for live web search capabilities.
Other models (gpt-5.2, claude) do NOT have live search.

## Pitfalls
- Do NOT pass `temperature` parameter to gpt-5-search-api
- Request explicit Spanish-language responses
- Set timeout to 180s minimum for deep searches
- Always request source URLs in output
- **API key redaction**: Hermes redacts the AISA key in tool output (even via grep/config reads). Workaround: write a Python script to file that reads config.yaml + calls the API, then execute it via terminal. The key stays unredacted inside the execution sandbox. See `references/api-key-workaround.md`.
- **Timeouts in cron**: cuando se corre como cron job, `execute_code` con `urllib` funciona mejor que `terminal` para llamadas HTTP largas. Cada query tarda 60-120s — usar timeout 240s por llamada, 600s total para el script.
- **Parallel queries OK, parallel API calls NO**: lanzar las 4 queries secuencialmente (con sleep 2s entre ellas). La API de AISA no soporta bien múltiples requests simultáneos desde la misma key.

## Output Format (cron delivery)
Cuando el resultado se entrega por WhatsApp/cron a Victor:
- **5-7 bullets** en español chileno directo, cada uno con 2-4 líneas max
- Cada bullet: dato duro + contexto breve + **URL de fuente** (sin markdown, link directo)
- Usar emojis 🔸 como bullet markers (se ven bien en WhatsApp)
- Cerrar con línea de fuente: `*Fuente: AISA gpt-5-search-api. Datos extraídos [fecha].*`
- Ver `references/query-templates.md` para el patrón de 4 queries estándar
