---
name: conecta-productos
description: Catálogo completo de productos y servicios de CONECTA + SUPcon para el equipo comercial
---

# CONECTA + SUPcon — Catálogo Comercial

## CONECTA Ingeniería (34 años en Chile)

CONECTA es el nexus de software y conectividad industrial. No vendemos hardware — vendemos sistemas que conectan, monitorean y protegen infraestructura crítica.

### SCADA ELIPSE (Brasil)
- Sistema de supervisión y control para subestaciones eléctricas
- Protocolos: IEC 61850, DNP3, Modbus, IEC 60870-5-101/104
- Clientes típicos: Transelec, CGE, Enel, Colbún, AES Andes, Metro
- Ticket: $80K-350K USD por proyecto
- Ciclo de venta: 3-8 meses (licitación)

### PMU Vizimax (Canadá)
- Unidades de medición fasorial para el Coordinador Eléctrico Nacional (CEN)
- Obligatorio por norma técnica para subestaciones de transmisión
- Ticket: $18-19K USD por subestación (~400 UF)
- Incluye: equipo PMU + GPS + materiales + ingeniería + montaje + pruebas
- Ciclo de venta: 6-12 meses (regulatorio)
- Clientes: Transelec, CGE, Coordinador Eléctrico, Barrick

### NovaTech Automation (distribución exclusiva CONECTA)

**Orion:** RTUs robustas para subestaciones
- Familia: OrionMX, OrionLX+, OrionSX, OrionVX
- 50+ protocolos: DNP3, IEC 61850, Modbus, SEL, Conitel
- Certificación NERC CIP para ciberseguridad
- OrionOS común en toda la familia
- 10 años de garantía

**Kronos:** Relojes satelitales GPS
- Precisión: 60 nanosegundos
- Multi-constelación: GPS + GLONASS + Galileo + BeiDou
- Sincronización crítica para PMU y SCADA

**Argus:** Software de gestión de flota
- Monitoreo centralizado de Orion + Kronos
- Dashboard en tiempo real
- Alertas y diagnóstico remoto

### Monitoreo de Calidad de Energía
- Power Quality: armónicos, flicker, desbalances
- Compliance Reporting: informes para SEC/CEN
- Analytics: tendencias, predicción de fallas
- Ticket: $30K-120K USD

---

## SUPcon (representación en Chile)

SUPcon es el mayor fabricante de sistemas de control de China, con presencia en 50+ países. TTung representa sus 3 líneas en Chile.

### 1. Field Instrumentation (Instrumentación de Campo)
- Transmisores de presión, temperatura, nivel, flujo
- Caudalímetros electromagnéticos, Coriolis, vortex
- Analizadores de pH, conductividad, oxígeno disuelto
- Posicionadores de válvula inteligentes
- Radar de nivel guiado y no guiado
- **Precio:** $75K-130K USD por planta (típico 11 plantas como Cementos Bio Bio)
- **Competidores:** Emerson, Endress+Hauser, Yokogawa, ABB
- **Diferenciador:** 30-50% más barato que Emerson con especificaciones equivalentes

### 2. Software y Digitalización
- **supOS:** Plataforma operativa industrial (equivalente a OSIsoft PI + dashboard)
- **PRIDE:** Detección de anomalías con IA (machine learning sobre datos de proceso)
- **X-AI:** Agentes de inteligencia artificial para operaciones
- **InPlant PID:** P&ID inteligente conectado a datos en tiempo real
- **InPlant LIMS:** Gestión de laboratorio
- **InPlant EAM:** Gestión de activos empresariales
- **InPlant OMS:** Gestión de operaciones
- **APC:** Control avanzado de procesos
- **Precio:** Consultar (varía por módulo y escala)

### 3. Control Systems (Sistemas de Control)
- **DCS:** Sistema de control distribuido (equivalente a DeltaV, CENTUM)
- **SIS:** Sistema instrumentado de seguridad
- **PLC:** Controladores lógicos programables
- **Precio:** Consultar (depende de cantidad de E/S y arquitectura)

---

## Tier0/UNS — Plataforma de Digitalización

Producto propio de TTung/CONECTA. Capa de datos unificada que conecta cualquier PLC/DCS/RTU a dashboards en tiempo real sin tocar los sistemas existentes.

- **Arquitectura:** MQTT → TimescaleDB → Grafana → SPA web
- **Protocolos:** OPC UA, Modbus, MQTT, IEC 61850, DNP3
- **Aplicaciones:** monitoreo de activos, mantenimiento predictivo, dashboards ejecutivos
- **Precio:** $25K USD/año (licencia plataforma + soporte)
- **Diferenciador:** Se instala en 2 días sin detener la planta. Read-only, no interfiere con el DCS.

---

## Argumentos de Venta

### Contra Emerson/Yokogawa (instrumentación)
"SUPcon ofrece la misma precisión y durabilidad que Emerson, a la mitad del precio. Con soporte local en Chile, no en Houston o Singapur."

### Contra Siemens/ABB (subestaciones)
"CONECTA tiene 34 años instalando SCADA en subestaciones chilenas. Conocemos la norma técnica mejor que nadie. Siemens te vende la caja; nosotros te entregamos el sistema funcionando con la norma CEN."

### Contra la competencia (Tier0/digitalización)
"No necesitas cambiar tu DCS. Tier0 se conecta en modo lectura a lo que ya tienes y en 48 horas estás viendo dashboards que antes tomaban 6 meses."

---

## Clientes de Referencia (para mencionar en propuestas)

- **Transelec:** SCADA + PMU en múltiples subestaciones
- **CGE:** PMU en 6 subestaciones (2026)
- **Barrick:** PMU Punta Colorada (2026)
- **Enel, Colbún, AES Andes:** SCADA y monitoreo
- **Metro de Santiago:** Sistemas de supervisión

---

## Notas para el Vendedor

1. Nunca menciones "Caje", "Rendimiento", "Reanual" — son términos internos. Usa "Field Instrumentation", "Software", "Control Systems"
2. Siempre decir "Consultar" si no tienes precio exacto. No inventar
3. SUPcon es chino pero con estándares internacionales (IEC, ISO, SIL). Mencionar certificaciones
4. CONECTA es el diferenciador: 34 años en Chile, soporte local, conocen la norma
5. NovaTech Orion compite contra SEL y GE en RTUs — Orion gana en NERC CIP y garantía de 10 años
