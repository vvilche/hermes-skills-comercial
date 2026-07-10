---
name: ttung-design-system
description: Reglas visuales y de diseño para entregables de TTung — landing pages, presentaciones, dashboards, documentos. Basado en preferencias de Victor.
tags: [design, html, css, landing-page, visual, branding]
related_skills: [popular-web-designs, claude-design, victor-rules]
---

# TTung Design System

## Reglas Cardinales (NO violar)

1. **MODO CLARO SIEMPRE.** Victor dijo "está muy re oscuro no veo una mierda". Fondo blanco (#f5f5f8 o #ffffff) por defecto. Nunca dark a menos que él lo pida explícitamente.

2. **SIN branding CONECTA/SUPcon/TTung en landing pages genéricas.** A menos que Victor diga explícitamente que la página es para CONECTA, mantenerlo completamente neutro. Sin logos, sin menciones, sin "Powered by". El producto debe venderse solo.

3. **Genérico, no específico.** Las landing pages para vender agentes/servicios deben posicionarse para CUALQUIER empresa de ingeniería, no solo para CONECTA. Las 3 áreas universales: Administración, Operaciones, Comercial.

4. **Estilo referencia: CONECTA Netlify portfolio.** Victor validó este diseño: https://conectanuevaweb.netlify.app/index_b_portfolio.html
   - Tarjetas blancas con sombra suave + borde
   - Barra de color en borde superior de tarjetas (categorización visual)
   - Tags tecnológicos como badges estilizados
   - Navegación limpia, header con degradado sutil
   - Secciones separadas con fondo alternado (blanco / gris claro #f8f9fa)

5. **Hosting: Netlify.** Victor prefiere Netlify (como CONECTA). Alternativa: VPS en puerto 8005.

---

## Paleta de Colores

| Rol | Color | Hex | Uso |
|-----|-------|-----|-----|
| Fondo página | Blanco | #ffffff | Body principal |
| Fondo alterno | Gris suave | #f8f9fa | Secciones alternadas |
| Texto principal | Azul oscuro | #1a1a2e | Headings, body |
| Texto secundario | Gris | #4a5568 | Descripciones |
| Texto terciario | Gris claro | #718096 | Metadatos |
| Primary | Azul industrial | #1E3A5F | CTAs, botones principales |
| Acento tecnología | Azul claro | #00B0F0 | Links, iconos técnicos |
| Acento IA | Púrpura | #6C5CE7 | Valor "Sin miedo automatización" |
| Admin | Azul medio | #4A90D9 | Agente Administración |
| Operaciones | Verde | #00A86B | Agente Operaciones |
| Comercial | Naranjo | #E67E22 | Agente Comercial |
| OK/Éxito | Verde | #10b981 | Checkmarks |
| Bordes | Gris | #e2e8f0 | Tarjetas, separadores |

## Tipografía

- **Headings:** Inter, bold (700), letter-spacing negativo
- **Body:** Inter, regular (400)
- **Mono:** JetBrains Mono — solo para datos técnicos
- **Fuente:** Google Fonts CDN
  ```html
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
  ```

## Componentes

### Tarjeta estándar (card)
```
background: #ffffff;
border-radius: 10px;
box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
border: 1px solid #e2e8f0;
```
Hover: `box-shadow: 0 4px 12px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.04); transform: translateY(-2px);`

### Tarjeta de agente (con barra de color superior)
```
position: relative;
/* barra: */
.agent-card::before {
  content: '';
  position: absolute; top: 0; left: 0; right: 0; height: 4px;
  border-radius: 10px 10px 0 0;
}
/* colores: admin=#4A90D9, ops=#00A86B, com=#E67E22 */
```

### Botón primario
```
background: #1E3A5F; color: #fff;
padding: 11px 24px; border-radius: 6px;
font-size: 14px; font-weight: 600;
```
Hover: background #2a4a6f, translateY(-1px)

### Botón secundario
```
background: #fff; color: #1a1a2e;
border: 1px solid #e2e8f0;
box-shadow: 0 1px 3px rgba(0,0,0,0.06);
```
Hover: shadow más fuerte, translateY(-1px)

### Tag tecnológico (badge)
```
display: inline-flex;
padding: 6px 16px;
background: #fff;
border: 1px solid #e2e8f0;
border-radius: 6px;
font-size: 13px;
```

### Agent-tag (badge de área)
```
display: inline-block;
font-size: 11px; font-weight: 600;
padding: 2px 10px; border-radius: 999px;
text-transform: uppercase; letter-spacing: 0.3px;
background: rgba(color, 0.1); /* según área */
```

### Formulario
```
input/select: padding 10px 12px;
border: 1px solid #e2e8f0;
border-radius: 6px;
background: #f8f9fa;
font-family: Inter;
focus: border-color #00B0F0, box-shadow 0 0 0 3px rgba(0,176,240,0.1)
```

---

## Estructura de Landing Page Genérica (para vender agentes IA a ingeniería)

1. **Nav** — Links: Agentes, Cómo Funciona, Beneficios, Tecnología, + CTA "Agendar Demo"
2. **Hero** — H1 con azul industrial + acento azul claro. Subtítulo posicionando "Lo que ves es lo que haces" + "Sin miedo a la automatización". 2 CTAs.
3. **Valores clave** — 2 columnas: (a) Transparencia total, (b) Potenciación del equipo
4. **3 Agentes** — Grid de 3 tarjetas con barra de color: Admin (azul), Operaciones (verde), Comercial (naranjo). Cada una con icono + tag + 3-4 bullets
5. **Cómo funciona** — 4 pasos numerados: Conectar → Entrenar → Potenciar → Escalar
6. **Métricas** — 3 tarjetas con números grandes: 80%, 3x, 90%
7. **Para quién** — 3 tarjetas: chicas (5-15), medianas (15-50), dueños que escalan
8. **Tecnología** — Tags en fila: Hermes Agent, WhatsApp/Slack/Teams, On-prem, ERP/CRM, RAG, Dashboards
9. **Pricing** — Setup $30K-$50K + recurrencia $2K-$5K/mes. En tarjeta centrada.
10. **Formulario CTA** — Nombre, Empresa, Correo, Tamaño equipo, Área que duele. Botón "Solicitar Demo".
11. **Footer** — Mínimo: "Hecho en Chile" + correo
12. **Hosting** — Netlify (preferido) o VPS :8005

## Landing Page Copywriting (posicionamiento validado por Victor)

- **Propuesta valor central:** "Tu empresa de ingeniería, multiplicada"
- **3 áreas universales:** Administración, Operaciones, Comercial — NO agentes industriales (SCADA, protecciones, etc.)
- **"Lo que ves es lo que haces"** — transparencia total, no caja negra
- **"Sin miedo a la automatización"** — los agentes potencian, no reemplazan
- **Mensaje clave:** 5 personas con agentes rinden como 15. 50 como 100
- **Pricing referencial:** Setup $30K-$50K + recurrencia $2K-$5K/mes (basado en análisis de mercado 2026)

## Precios de Referencia (Mercado 2026)

Basado en análisis de competidores (ProductCrafters, LeewayHertz, Itransition, etc.):

| Tipo | Nuestro precio | Mercado |
|-----|----------------|---------|
| Setup 3 agentes | $30K-$50K | $80K-$200K (multi-agente) |
| Recurrencia | $2K-$5K/mes | $4K-$12K/mes |
| Competidores generales | — | $25K-$500K+ |

Nuestro precio está en el rango bajo del mercado, lo que es buena posición para ingeniería (sector conservador con presupuestos ajustados).

## Email Drafts con Copy Button

Para correos/drafts que Victor necesita copiar y pegar:

```html
<button onclick="copyEmail()">📋 Copiar todo</button>
<div class="email"><!-- contenido --></div>
<script>
function copyEmail() {
  const text = document.querySelector('.email').innerText;
  navigator.clipboard.writeText(text).then(() => {
    btn.textContent = '✅ Copiado!';
    setTimeout(() => btn.textContent = '📋 Copiar todo', 1500);
  });
}
</script>
```

- **Max-width 680px** centrado, fondo blanco, borde sutil
- **Metadatos** (To/Subject) al inicio con color secundario (#718096)
- **Secciones** con `border-left` de color para cada oportunidad/tema
- **Sin diseño recargado** — es funcional, no landing page
- **Botón "Copy All"** usa `navigator.clipboard.writeText()` — compatible con móvil y desktop
- Servir en puerto 8005 con http.server

### Email Copywriting Rules (Victor)
- **NUNCA usar jerga interna** (Caje, Rendimiento, Reanual) — solo nombres reales SUPcon
- **Ser específico por prospecto** — no templates genéricos
- **Incluir "lo que el cliente pide específicamente"** como checklist
- **Preguntar al destinatario** qué documentación tiene disponible, no asumir
- **Si Victor pide "en inglés"**, traducir TODO (campos, etiquetas, oportunidades, acciones)
