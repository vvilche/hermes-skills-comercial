# 🚀 Manual de Instalación — Hermes Agent para el Equipo TTung/CONECTA

> **Para:** Equipo Comercial TTung  
> **Última actualización:** Julio 2026  
> **Tiempo de instalación:** 15-20 minutos  
> **Dificultad:** Baja (no necesitas saber programar)

---

## 📖 ¿QUÉ ES HERMES AGENT?

Hermes Agent es un asistente de inteligencia artificial que instalas en tu computador. Funciona como ChatGPT pero:
- Está entrenado específicamente para ventas industriales (minería, energía, agua)
- Conoce todos los productos de SUPcon, NovaTech y CONECTA
- Puede investigar clientes, escribir correos, crear PPTs, analizar licitaciones
- Corre en tu computador, no en la nube de otra empresa

**Lo usas desde la terminal** (una ventana negra de comandos). Escribes lo que necesitas y él responde. Como un WhatsApp con un vendedor experto.

---

## 📋 REQUISITOS PREVIOS

Antes de instalar Hermes, necesitas tener 3 cosas:

### 1. Python instalado (lenguaje de programación)

**¿Cómo saber si ya lo tienes?**  
Abre la terminal y escribe:
```bash
python3 --version
```
Si sale algo como `Python 3.10.x` o superior, ya lo tienes. Si no, instálalo:

- **Mac:** https://www.python.org/downloads/macos/ → Descarga el instalador y ejecútalo
- **Windows:** https://www.python.org/downloads/windows/ → Descarga el instalador, **IMPORTANTE: marca "Add Python to PATH"** durante la instalación
- **Linux:** Ya viene instalado en Ubuntu 20+

### 2. Git instalado (control de versiones)

**¿Cómo saber si ya lo tienes?**
```bash
git --version
```

Si no lo tienes:
- **Mac:** https://git-scm.com/download/mac
- **Windows:** https://git-scm.com/download/win
- **Linux:** `sudo apt install git -y`

### 3. API Key de DeepSeek (la "llave" para usar inteligencia artificial)

DeepSeek es el motor de inteligencia artificial que usa Hermes. Es más barato y mejor que ChatGPT para tareas técnicas.

**💰 Costo:** Aproximadamente $2.000 - $5.000 CLP al mes por persona (depende del uso).

**🔑 Cómo obtener la API Key en 2 minutos:**

1. Entra a https://platform.deepseek.com
2. Haz clic en "Sign Up" (arriba a la derecha)
3. Regístrate con tu correo y una contraseña
4. Una vez dentro, ve a "API Keys" en el menú izquierdo
5. Haz clic en "Create new API key"
6. Ponle nombre "Hermes-TTung" y haz clic en "Create"
7. **¡COPIA LA KEY AHORA!** Solo se muestra una vez. Empieza con `sk-`
8. Ves a https://platform.deepseek.com/top_up y carga saldo (mínimo $5 USD, unos $5.000 CLP). Puedes pagar con tarjeta de crédito/débito internacional.

> **⚠️ IMPORTANTE:** Guarda tu API key en un lugar seguro. Es como la contraseña de tu correo. No la compartas con nadie fuera de TTung.

---

## 💻 INSTALACIÓN DE HERMES AGENT

### En Mac (macOS 12 o superior)

Abre la aplicación **Terminal** (está en Aplicaciones > Utilidades > Terminal) y copia y pega estos comandos UNO POR UNO:

```bash
# Paso 1: Instalar Hermes Agent
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```
> Espera 1-2 minutos. Verás texto pasando por la pantalla. Es normal.

```bash
# Paso 2: Agregar Hermes al sistema
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

```bash
# Paso 3: Verificar que quedó instalado
hermes --version
```
> Debe responder algo como `hermes 0.14.0`. Si dice "command not found", cierra la terminal, ábrela de nuevo y prueba otra vez.

### En Windows 10/11 (64-bit)

Abre **PowerShell como Administrador** (botón derecho en el menú inicio > "Windows PowerShell (Admin)") y copia y pega:

```powershell
# Paso 1: Instalar Hermes Agent
irm https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.ps1 | iex
```
> Espera 1-2 minutos. Si pregunta algo, responde "S" (sí).

```powershell
# Paso 2: Verificar instalación
hermes --version
```
> Debe responder algo como `hermes 0.14.0`.

### En Linux (Ubuntu 20.04 o superior)

Abre la terminal y ejecuta:

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
hermes --version
```

---

## 🔑 CONFIGURAR DEEPSEEK (LA API KEY)

Ahora dile a Hermes que use DeepSeek. Copia y pega estos comandos EN ORDEN:

```bash
# Paso 1: Crear carpeta de configuración
mkdir -p ~/.hermes

# Paso 2: Guardar tu API key (REEMPLAZA "sk-tu-key-aqui" con tu key real)
echo 'DEEPSEEK_API_KEY=sk-tu-key-aqui' > ~/.hermes/.env
```

> Ejemplo: si tu key es `sk-abc123def456`, el comando queda:  
> `echo 'DEEPSEEK_API_KEY=sk-abc123def456' > ~/.hermes/.env`

```bash
# Paso 3: Configurar DeepSeek como proveedor
hermes config set model.provider deepseek
hermes config set model.default deepseek-v4-pro
```

```bash
# Paso 4: Probar que funciona
hermes chat -q "Responde en español: ¿Cuál es la capital de Chile?"
```
> Si responde "Santiago", ¡está funcionando! 🎉

---

## 📦 INSTALAR LAS SKILLS COMERCIALES

Las "skills" son módulos de conocimiento que le enseñan a Hermes sobre nuestros productos y clientes. Son como "cursos" que ya tomó.

```bash
# Paso 1: Descargar las skills desde GitHub
cd /tmp
git clone https://github.com/vvilche/hermes-skills-comercial.git

# Paso 2: Copiarlas a Hermes
cp -r hermes-skills-comercial/skills/* ~/.hermes/skills/

# Paso 3: Recargar las skills
hermes chat -q "/reload-skills"
```

> Si no tienes Git instalado o falla el clone, ve al paso alternativo abajo ↓

### 🌐 Alternativa: Descarga manual (si Git falla)

1. Entra a https://github.com/vvilche/hermes-skills-comercial
2. Haz clic en el botón verde "<> Code" → "Download ZIP"
3. Descomprime el ZIP en tu carpeta de Descargas
4. Copia la carpeta `skills/` a `~/.hermes/skills/`:
   - **Mac/Linux:** `cp -r ~/Downloads/hermes-skills-comercial-main/skills/* ~/.hermes/skills/`
   - **Windows:** Copia manualmente el contenido de `skills/` a `C:\Users\TU_USUARIO\.hermes\skills\`

---

## 📚 QUÉ PUEDES HACER CON HERMES (EJEMPLOS REALES)

Una vez instalado, abre la terminal y escribe `hermes`. Verás una pantalla de bienvenida. Escribe cualquiera de estas cosas:

### Investigar clientes
```
Investiga quién es el gerente de mantención de Minera Los Pelambres y qué proyectos tienen para 2026
```

### Preparar correos
```
Escríbeme un correo para Cristián Schälchli de Minera Salar Blanco presentando CONECTA
```

### Analizar licitaciones
```
Busca licitaciones de instrumentación industrial en ChileCompra de esta semana
```

### Crear presentaciones
```
Hazme un PPT de 5 slides sobre digitalización de subestaciones eléctricas para presentar a Transelec
```

### Cotizar proyectos
```
Arma una cotización para un sistema SCADA de 10 subestaciones con precios referenciales
```

---

## 🛠️ COMANDOS ÚTILES DENTRO DE HERMES

Cuando estás dentro de Hermes (después de escribir `hermes`), puedes usar estos comandos con `/`:

| Comando | Qué hace |
|---|---|
| `/help` | Muestra todos los comandos disponibles |
| `/skill nombre` | Carga un skill específico |
| `/goal "tarea"` | Le das una misión y no para hasta completarla |
| `/new` | Empieza una conversación nueva desde cero |
| `/model` | Cambia el modelo de IA (no necesario para uso normal) |
| `/quit` | Sale de Hermes |

---

## ❓ SOLUCIÓN DE PROBLEMAS COMUNES

| Problema | Causa probable | Solución |
|---|---|---|
| "hermes: command not found" | No se agregó al PATH | Cierra la terminal y ábrela de nuevo |
| "DEEPSEEK_API_KEY not set" | La key no se guardó | Revisa que `~/.hermes/.env` tenga tu key |
| "Insufficient balance" | Sin saldo en DeepSeek | Entra a https://platform.deepseek.com/top_up y recarga |
| "Connection error" | Sin internet | Revisa tu conexión |
| Hermes responde en inglés | No configuraste español | Dile "respóndeme siempre en español" |
| Skills no se cargan | No se copiaron bien | Ejecuta `/reload-skills` dentro de Hermes |

---

## 📞 SOPORTE

Si algo no funciona después de seguir esta guía:

1. **WhatsApp a Victor:** +56 9 9978 47438
2. **Repo de skills:** https://github.com/vvilche/hermes-skills-comercial
3. **Documentación oficial de Hermes:** https://hermes-agent.nousresearch.com/docs/

---

## ✅ CHECKLIST DE INSTALACIÓN

Antes de decir "estoy listo", verifica:

- [ ] Python instalado (`python3 --version`)
- [ ] Git instalado (`git --version`)
- [ ] API Key de DeepSeek creada y con saldo
- [ ] Hermes Agent instalado (`hermes --version`)
- [ ] `.env` configurado con la API key
- [ ] DeepSeek configurado como provider
- [ ] Probé `hermes chat -q "Hola"` y respondió
- [ ] Skills comerciales copiadas de GitHub
- [ ] Ejecuté `/reload-skills` dentro de Hermes

---

*Manual preparado por Hermes Agent — Julio 2026*  
*TTung Engineering SpA — CONECTA + SUPcon*
