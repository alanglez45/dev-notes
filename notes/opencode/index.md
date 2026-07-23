# Guia Rapida de OpenCode

## Indice

- [Instalacion](#instalacion)
- [Primeros pasos](#primeros-pasos)
- [Tips de uso](#tips-de-uso)
- [Comandos utiles](#comandos-utiles)
- [Sesiones](#sesiones)
- [Agentes](#agentes)
- [Modelos recomendados](#modelos-recomendados)
- [Skills](#skills)
- [Reglas (Rules)](#reglas-rules)
- [MCP Servers](#mcp-servers-tools-externas)
- [Tips rapidos](#tips-rapidos)

## Instalacion

```bash
# Script oficial
curl -fsSL https://opencode.ai/install | bash

# Con npm
npm install -g opencode-ai

# Con Homebrew
brew install anomalyco/tap/opencode
```

## Primeros pasos

```bash
cd /ruta/de/tu/proyecto
opencode
```

- `/connect` — configura un proveedor de IA (recomendado: OpenCode Zen)
- `/init` — analiza el proyecto y crea `AGENTS.md` (se committea a Git)

> `AGENTS.md` es un archivo de instrucciones que OpenCode lee automaticamente. Contiene la estructura, tecnologias y convenciones de tu proyecto para que el agente entienda mejor el codigo. Se genera con `/init` y se recomienda versionarlo en Git.

## Tips de uso

- **`@archivo`** — escribe `@` y OpenCode busca archivos del proyecto
- **`!comando`** — ejecuta un comando del sistema (ej: `!git status`, `!ls`) y vuelves directo a OpenCode
- **Usa Plan mode (Tab)** para analizar antes de construir
- **Arrastra imagenes al terminal** para darlas como contexto al agente
- **`/timeline`** — muestra el historial de la sesion actual

## Comandos utiles

| Comando | Que hace |
|---------|----------|
| `/undo` | Revertir ultimo cambio |
| `/redo` | Re-aplicar cambio revertido |
| `/share` | Compartir sesion (genera un link) |
| `/themes` | Cambiar tema visual |

## Sesiones

- **Tab** — cambia entre Build mode (escribe codigo) y Plan mode (solo analiza)
- **`@nombre`** — invoca un subagente (ej: `@explore`, `@general`, `@scout`)
- **Navegacion padre/hijo**: `→` (siguiente hijo), `←` (hijo anterior), `↑` (volver al padre)
- **`/new`** (`ctrl+x n`) — crea nueva sesion, reemplaza la actual
- **`/sessions`** (`ctrl+x l`) — lista y cambia entre sesiones abiertas
- **`ctrl+p`** → "switch session" — lista sesiones abiertas y permite cambiar entre ellas
- **Multiples terminales** — ejecuta `opencode` en distintas ventanas sobre el mismo proyecto
- **Desde CLI**: `opencode run "tu prompt"` ejecuta una sesion sin abrir la TUI
- **Retomar sesion**: `opencode -c` (continuar ultima), `opencode -s <id>` (sesion especifica), `opencode --fork` (forkearla)

## Agentes

Los **agentes** son instancias de IA que OpenCode ejecuta para realizar tareas. Piensa en ellos como asistentes especializados: cada uno tiene su propio **modelo** (ej: Claude Sonnet, Qwen), **temperatura** (que tan creativo es), **system prompt** (instrucciones de comportamiento) y un conjunto de **herramientas permitidas** (leer archivos, editar, ejecutar bash, etc.).

Cuando trabajas con OpenCode, siempre tienes un agente activo. Puedes cambiar entre ellos, crear los tuyos propios o delegar tareas a subagentes segun lo que necesites.

OpenCode tiene dos tipos de agentes: **primarios** (interactuas directamente, cambias con Tab) y **subagentes** (invocados con `@nombre` o automaticamente).

### Built-in

| Agente | Modo | Descripcion |
|--------|------|-------------|
| **Build** | primario | Default. Todas las herramientas habilitadas. |
| **Plan** | primario | Solo analisis y planificacion. Edits y bash en `ask`. |
| **General** | subagente | Multi-step tasks, acceso completo (excepto write). |
| **Explore** | subagente | Solo lectura, rapido, para explorar codebase. |
| **Scout** | subagente | Solo lectura, para docs externos y dependencias. |

### Configurar Agentes

En `opencode.json`:

```json
{
  "agent": {
    "code-reviewer": {
      "description": "Reviews code for best practices",
      "mode": "subagent",
      "model": "anthropic/claude-sonnet-4-5",
      "temperature": 0.1,
      "prompt": "{file:./prompts/code-review.txt}",
      "permission": {
        "edit": "deny",
        "bash": "deny"
      }
    }
  }
}
```

### Crear Agentes desde CLI

Alternativa a la configuracion manual en `opencode.json`:

```bash
# Interactivo — guia paso a paso
opencode agent create

# No interactivo (con todos los flags, no pregunta nada)
opencode agent create \
  --description "Experto en TypeScript" \
  --mode subagent \
  --tools "bash,read,edit,glob,grep" \
  --model anthropic/claude-sonnet-4-5
```

Crea un archivo `.md` en `.opencode/agent/` (proyecto) o `~/.config/opencode/agent/` (global). Se invoca con `@nombre` o `opencode --agent nombre`.

## Modelos recomendados

- **Minimax m2.7** — muy bueno para frontend
- **Qwen 3.6** — bueno para codigo general

## Skills

Las **skills** son archivos de instrucciones reutilizables (`.opencode/skills/<nombre>/SKILL.md`) que el agente carga **bajo demanda** para tareas especificas. A diferencia de las reglas (`AGENTS.md`) que siempre estan presentes, las skills solo se activan cuando se las pides o cuando el agente detecta que son relevantes.

Cada skill tiene un **frontmatter YAML** con `name` y `description` que OpenCode usa para identificarla sin leer todo el archivo:

```yaml
---
name: test-utils
description: Guia para escribir tests con Vitest
---
```

### Como pedirle a OpenCode que cree Skills

Solo dile lo que quieres en la conversacion. Ejemplos:

> *"Crea una skill llamada `test-utils` que describa como escribir tests con Vitest. Ponla en `.opencode/skills/test-utils/SKILL.md`"*

> *"Haz una skill global para revisar codigo React. Ponla en `~/.config/opencode/skills/review-frontend/SKILL.md`"*

OpenCode creara la carpeta y el archivo automaticamente con el frontmatter y las instrucciones.

### Buscar skills

- **Buscar skills**: [skills.sh](https://skills.sh) — repositorio de skills comunitarias

### Autoskills

Herramienta de [midudev](https://midu.dev) que detecta automaticamente tu stack tecnologico e instala las skills mas relevantes.

```bash
npx autoskills
```

**Como funciona:**

1. Escanea tu proyecto (`package.json`, configs, etc.)
2. Detecta tecnologias (React, Next.js, Prisma, Tailwind, etc.)
3. Muestra un selector interactivo con las skills recomendadas
4. Descarga las skills desde [skills.sh](https://skills.sh) (registro auditado)
5. Las instala en `.opencode/skills/`

**Seguridad:** las skills se sincronizan en un registro, se auditan contra riesgos de prompt-injection y se verifican con SHA-256 antes de instalarse. Genera un `skills-lock.json`.

> **Nota:** Solo se ejecuta una vez por proyecto. Las skills se guardan como archivos estaticos en `.opencode/skills/` y no expiran. Vuelve a ejecutarlo solo si agregas tecnologias nuevas al stack o quieres actualizar las skills.

## Reglas (Rules)

Son instrucciones en lenguaje natural que OpenCode inyecta en el prompt del agente para guiar su comportamiento en tu proyecto.

**`AGENTS.md`** es el archivo principal de reglas. Siempre se carga automaticamente.

> **Nota:** `AGENTS.md` no se actualiza solo. Si cambia el stack, comandos o convenciones del proyecto, debes editarlo manualmente o ejecutar `/init` para que OpenCode lo reanalice (revisa los cambios, puede sobrescribir personalizaciones).

### Multiples archivos de reglas

Desde `opencode.json` puedes referenciar archivos adicionales con `instructions`:

```json
{
  "instructions": [
    "docs/convenciones.md",
    "docs/arquitectura.md",
    ".cursor/rules/*.md"
  ]
}
```

OpenCode los inyecta junto con `AGENTS.md`. Util para separar reglas por area (UI, testing, API, etc.).

### Jerarquia

| Prioridad | Ubicacion | Ambito |
|-----------|-----------|--------|
| 1 (maxima) | `./AGENTS.md` | Proyecto actual |
| 2 | `./CLAUDE.md` | Fallback si no hay `AGENTS.md` |
| 3 | `~/.config/opencode/AGENTS.md` | Global (todos los proyectos) |

### Diferencia con Skills

| Reglas (AGENTS.md) | Skills (SKILL.md) |
|-------------------|-------------------|
| Se inyectan **siempre** en el prompt | Se cargan **bajo demanda** |
| Una o pocas por proyecto | Muchas, reutilizables entre proyectos |
| Sin frontmatter | Requieren YAML frontmatter |

## MCP Servers (tools externas)

**MCP (Model Context Protocol)** es un estandar abierto que permite conectar el agente con herramientas externas (APIs, bases de datos, sistemas de archivos, etc.). Los MCP Servers las exponen y OpenCode las llama automaticamente segun la tarea.

```json
// opencode.json — Local
{
  "mcp": {
    "mi-mcp": {
      "type": "local",
      "command": ["npx", "-y", "mi-paquete-mcp"]
    }
  }
}
```

```json
// opencode.json — Remoto
{
  "mcp": {
    "mi-mcp": {
      "type": "remote",
      "url": "https://mi-servidor.com/mcp",
      "headers": { "Authorization": "Bearer {env:API_KEY}" }
    }
  }
}
```

Comandos MCP: `opencode mcp auth <nombre>`, `opencode mcp list`, `opencode mcp logout <nombre>`.

## Tips rapidos

- Describe las tareas como si hablaras con un dev junior
- Despues de crear una skill, dile a OpenCode que la use: *"Usa la skill test-utils para esto"*
- Skills no se activan solas — debes pedirlas explicitamente en la conversacion (ej: *"Usa la skill test-utils para esto"*)
- Skills y config se descubren automaticamente, no necesitas registrar nada
- Commitea `AGENTS.md` — ayuda a OpenCode a entender tu proyecto
- Hablar en **ingles** gasta menos tokens que espanol
