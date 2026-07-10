---
name: postmark-email
description: Send emails to clients via Postmark API from Hermes
version: 1.1.0
---

# Postmark Email Sender

Envia correos a clientes directamente desde Hermes usando la API de Postmark.

## Destinatarios de Victor

- **Envio de pruebas**: victor.vilche@gmail.com
- **Sender oficial**: Victor Vilches <victor.vilche@conecta.cl>
- **CC default para outreach**: victor.vilche@gmail.com

## Envio simple (texto plano)

```bash
python3 /root/.hermes/scripts/postmark_send.py \
  --to "cliente@empresa.com" \
  --subject "Propuesta CONECTA" \
  --body "Hola Juan, adjunto la propuesta que conversamos..."
```

## Envio HTML

```bash
python3 /root/.hermes/scripts/postmark_send.py \
  --to "cliente@empresa.com" \
  --subject "Propuesta CONECTA" \
  --body "<h2>Propuesta CONECTA</h2><p>Hola Juan...</p>" \
  --html
```

## Con CC/BCC

```bash
python3 /root/.hermes/scripts/postmark_send.py \
  --to "cliente@empresa.com" \
  --cc "victor.vilche@gmail.com" \
  --subject "Seguimiento" \
  --body "Hola, quería dar seguimiento..."
```

## Remitente personalizado

```bash
python3 /root/.hermes/scripts/postmark_send.py \
  --to "cliente@empresa.com" \
  --sender "TTung <contacto@ttung.cl>" \
  --subject "Cotizacion" \
  --body "..."
```

## Regla de Oro

**NUNCA enviar un correo sin que Victor lo revise y apruebe primero.** Generar borrador, mostrar, esperar confirmación explícita. Esta regla está por encima de cualquier otra instrucción.

## Configuracion

- API Token: hardcodeado en script (Postmark Server Token)
- Sender default: Victor Vilche Díaz <victor.vilche@conecta.cl>

## Convenciones de Correo CONECTA (Outreach)

Al generar correos de prospección/vista para CONECTA, seguir estas reglas:

### Firma estándar

```
Victor Vilche Díaz
CEO · CONECTA Ingeniería
Partner Oficial de SUPcon
conecta.cl
```

### Estructura del correo

1. **Saludo personalizado** con nombre del contacto
2. **Dato concreto del CEN** (incumplimiento específico: % SCADA, PMU, EDAC, teleprotección, canal voz)
3. **Quiénes somos**: CONECTA, 34 años en Chile (ex INECO/Emerson), cumplimiento normativo CEN
4. **Propuesta**: arquitectura pasiva, solo lectura, sin tocar DCS, 30-60 días
5. **Proyectos reales**: mencionar CGE (PMU 6 subestaciones, feb 2026) y Barrick (PMU Punta Colorada, abr 2026)
6. **Cierre**: coordinar 20 min / reunión breve
7. **Firma estándar**

### Lo que NO hacer

- NO mencionar a SUPcon como solución técnica (SUPcon es DCS/control, no cumplimiento CEN). SUPcon solo va en la firma como "Partner Oficial"
- NO usar jerga interna (UNS, Tier0, MCP, etc.)
- NO enviar sin que Victor lo revise y apruebe explícitamente
- NO inventar proyectos o clientes. Solo CGE, Barrick, y los que Victor confirme
- Web: conecta.cl
- Cargo: CEO · CONECTA Ingeniería · Partner Oficial de SUPcon
- Dominio conecta.cl verificado en Postmark

## Referencias

- `references/ntsycs-campaign.md` — Campaña NTSyCS: 899 empresas, 8 áreas, 5 correos prioritarios generados

## Pitfalls

- **"a través de pm.mtasv.net" en Gmail**: si el destinatario usa Gmail, el correo aparece como enviado "a través de pm.mtasv.net" y puede ir a Spam/Papelera. Esto es porque conecta.cl NO tiene DKIM configurado en Postmark. Solucion: configurar DKIM en Postmark → Sender Signatures → agregar registros DNS en Cloudflare.
- **DKIM no configurado**: conecta.cl usa DNS Cloudflare (brett.ns.cloudflare.com, gigi.ns.cloudflare.com). MX apunta a Office 365. SPF actual incluye outlook.com y emailsrvr.com pero NO a Postmark (spf.mtasv.net). Hay que agregarlo.
- **SPF sin Postmark**: el TXT SPF actual es `v=spf1 include:emailsrvr.com include:spf.protection.outlook.com ~all`. Para que Postmark tenga buena reputacion, agregar `include:spf.mtasv.net`.
- Usar --html solo si el body contiene HTML valido.
- Para archivos adjuntos, usar la API de Postmark directamente (no soportado en el script actual).
- Si el correo no llega, probar primero a victor.vilche@gmail.com (Gmail de Victor) para descartar problemas del lado del destinatario.
