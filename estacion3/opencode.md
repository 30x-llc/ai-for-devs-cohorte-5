# Estación 3 · OpenCode

[← Volver al índice de la estación 3](README.md)

1. [OpenCode: Agente Open Source para Modelos Locales o Cloud](#1-opencode-agente-open-source-para-modelos-locales-o-cloud)
2. [Modelos locales o cloud](#2-modelos-locales-o-cloud)
3. [AGENTS.md y flujo de trabajo](#3-agentsmd-y-flujo-de-trabajo)
4. [MCP en OpenCode](#4-mcp-en-opencode)
5. [Skills en OpenCode](#5-skills-en-opencode)

---

## 1. OpenCode: Agente Open Source para Modelos Locales o Cloud

_Duración: 8 minutos_

**Objetivos**

- Entender dónde encaja OpenCode en el stack agéntico
- Instalar y abrir una sesión en el proyecto
- Diferenciar TUI, Desktop, IDE y modo servidor

> **¿Qué es OpenCode?**
>
> **OpenCode** es un agente de codificación open source que puede correr en terminal, app de escritorio, IDE o modo cliente/servidor. Su valor para este taller es que permite practicar flujos agénticos con proveedores cloud y también con modelos locales.

### Instalación Rápida

```bash
# Instalación con npm (o con: curl -fsSL https://opencode.ai/install | bash)
npm install -g opencode-ai

# Abrir OpenCode en el proyecto actual
opencode

# Ver opciones disponibles
opencode --help
```

> **Versión**
>
> Estas lecciones siguen OpenCode 1, que es lo que instalan `npm`, el script `curl` y `brew install anomalyco/tap/opencode`. La fórmula de Homebrew `brew install opencode` instala OpenCode 2, que puede diferir de lo que ves aquí.

### Superficies de Uso

- **Terminal TUI:** Interfaz principal para trabajar desde el repositorio, revisar cambios y aprobar acciones.
- **Desktop:** Cliente visual para sesiones locales, útil cuando quieres una experiencia más cercana a un IDE.
- **IDE:** Integración con editor para usar OpenCode mientras navegas archivos y cambios.
- **Client/Server:** Modo servidor para separar el agente de la interfaz y conectarte desde otros clientes.

### Documentación Base

| Recurso | Uso en el taller |
|---|---|
| [Introducción OpenCode](https://opencode.ai/docs/es) | Instalación, configuración inicial y formas de ejecutar el agente. |
| [MCP Servers](https://opencode.ai/docs/es/mcp-servers/) | Conexión de herramientas externas en `opencode.json`. |
| [Skills](https://opencode.ai/docs/skills/) | Creación de habilidades reutilizables en `.opencode/skills/`. |

---

## 2. Modelos locales o cloud

_Duración: 7 minutos_

**Objetivos**

- Configurar proveedores en OpenCode
- Comparar privacidad, costo y latencia
- Elegir modelo según la tarea

### Configuración de Proveedor

OpenCode usa un archivo `opencode.json`para declarar configuración del proyecto. En esta clase lo usaremos para fijar proveedor, modelo y herramientas compartidas. `opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "openai/gpt-oss-120b",
  "provider": {
    "openai": {
      "apiKey": "$OPENAI_API_KEY"
    }
  }
}
```

### Ejemplo con Modelo Local

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "ollama/qwen2.5-coder:14b",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      }
    }
  }
}
```

| Opción | Cuándo usarla | Trade-off |
|---|---|---|
| **Cloud** | Features complejas, refactors grandes, revisión de arquitectura. | Mejor calidad y contexto, mayor costo y dependencia de red. |
| **Local** | Exploración, privacidad, tareas repetitivas o repos con datos sensibles. | Menor costo marginal, pero más límites de capacidad y velocidad. |

---

## 3. AGENTS.md y flujo de trabajo

_Duración: 7 minutos_

**Objetivos**

- Definir reglas del repositorio
- Guiar al agente con contexto estable
- Practicar prompts orientados a artifacts

`AGENTS.md`funciona como memoria de proyecto: convenciones, comandos, rutas críticas, criterios de calidad y estilo de respuesta. En OpenCode lo usamos antes de MCP y Skills para que el agente tenga un contrato claro. `AGENTS.md` ```markdown
# Reglas del proyecto

## Comandos
- Instalar: npm install
- Desarrollo: npm run dev
- Verificación: npm run lint && npm test

## Convenciones
- Mantén cambios pequeños y revisables.
- Antes de editar, explica el plan en máximo 5 bullets.
- Después de editar, ejecuta la verificación aplicable.
- Nunca incluyas secretos ni credenciales en archivos del repo.

## Criterios de entrega
- Resumen de cambios
- Comandos ejecutados
- Riesgos o trabajo pendiente
```

### Prompt de práctica

**💬 Prompt — OpenCode con contexto**

```text
Lee AGENTS.md y revisa la estructura del proyecto. Luego propón un plan para agregar un endpoint de búsqueda al CRM, indicando archivos a tocar, riesgos y pruebas necesarias. No edites todavía.
```

---

## 4. MCP en OpenCode

_Duración: 18 minutos_

**Objetivos**

- Configurar MCP locales y remotos en opencode.json
- Descubrir marketplaces y repositorios útiles
- Instalar un top de MCPs para flujos reales

> **MCP = herramientas externas bajo contrato**
>
> En OpenCode, MCP se declara dentro de `opencode.json` bajo la clave `mcp`. Cada servidor tiene un nombre estable, un tipo de conexión (`local` o `remote`) y puede habilitarse, deshabilitarse o limitarse por agente.

### Modelo mental: local vs remoto

| Tipo | Cómo corre | Cuándo usarlo | Riesgo principal |
|---|---|---|---|
| **Local** | OpenCode levanta un comando como `npx`, `bunx` o un binario local. | Filesystem, Postgres local, herramientas internas o servidores que necesitan credenciales del entorno. | Puede ejecutar código en tu máquina; revisa el paquete antes de instalarlo. |
| **Remoto** | OpenCode llama una URL MCP y, si aplica, maneja OAuth o headers. | Servicios SaaS como Sentry, Context7, GitHub, Vercel o gateways empresariales. | Puede sumar muchos tokens al contexto; habilita solo lo necesario. |

### Ejemplo completo: MCP local y remoto

Este ejemplo combina servidores MCP remotos y locales. La idea es copiarlo y llevarlo al archivo `opencode.json`, reemplazando los tokens de ejemplo por credenciales reales cuando aplique. `opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp",
      "enabled": true
    },
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev/mcp",
      "oauth": {},
      "enabled": true
    },
    "Neon_db": {
      "type": "remote",
      "url": "https://mcp.neon.tech/mcp",
      "enabled": true,
      "headers": {
        "Authorization": "Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      }
    },
    "github": {
      "type": "local",
      "command": [
        "npx",
        "-y",
        "@modelcontextprotocol/server-github",
        "--GITHUB_PERSONAL_ACCESS_TOKEN",
        "xxxxxxxxxxxxxxxxxxxxxxxxx"
      ],
      "enabled": true
    },
    "sequential_thinking": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-sequential-thinking"],
      "enabled": true
    }
  }
}
```

### Gestión de MCPs en OpenCode

```bash
# Listar servidores MCP y estado de autenticación
opencode mcp list

# Autenticar un servidor remoto con OAuth
opencode mcp auth sentry

# Ver estado OAuth de servidores remotos
opencode mcp auth list

# Depurar conexión y flujo OAuth
opencode mcp debug sentry

# Cerrar sesión de un servidor
opencode mcp logout sentry
```

### Limitar herramientas MCP por agente

Los MCPs agregan herramientas al contexto. En repos grandes conviene declararlos, pero deshabilitarlos globalmente y activarlos solo en agentes especializados.

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp",
      "enabled": true
    }
  },
  "tools": {
    "context7_*": false
  },
  "agent": {
    "plan": {
      "tools": {
        "context7_*": true
      }
    }
  }
}
```

### Prompts con MCP

**💬 Prompt — MCP + documentación**

```text
Usa Context7 para consultar la documentación actual de Next.js App Router y dime qué patrón conviene para agregar paginación server-side.
```

**💬 Prompt — MCP + base de datos**

```text
Usa el MCP postgres para revisar el esquema de Customer. Verifica si existe índice para email y propón la migración mínima si falta.
```

**💬 Prompt — MCP + producción**

```text
Usa Sentry para encontrar los errores abiertos más recientes del proyecto, agrúpalos por causa probable y genera un plan de fix con archivos candidatos.
```

### Marketplaces y Repositorios MCP para OpenCode

- **MCP Registry Oficial:** Registro oficial en preview para descubrir servidores públicos, metadata e instrucciones estandarizadas. [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io/)
- **MCP Docs:** Documentación del protocolo, registry y guías de implementación. [modelcontextprotocol.io](https://modelcontextprotocol.io)
- **modelcontextprotocol/servers:** Servidores de referencia: filesystem, git, memory, fetch, time y sequential thinking. [GitHub Repo](https://github.com/modelcontextprotocol/servers)
- **Glama:** Directorio amplio con verificación, health checks y herramientas para probar MCPs en navegador. [glama.ai](https://glama.ai/)
- **Smithery:** Marketplace comunitario para buscar servidores MCP por categoría y revisar opciones de instalación. [smithery.ai](https://smithery.ai)
- **MCP Atlas:** Agregador que cruza Registry, Smithery, Glama, PulseMCP y GitHub para explorar el ecosistema. [mcp-atlas.com](https://www.mcp-atlas.com/)

### Top 6 MCPs para OpenCode

> **Recomendación práctica**
>
> Instala pocos MCPs, pero que abran capacidades reales: documentación actual, repos, errores de producción, base de datos, búsqueda de ejemplos y razonamiento estructurado.

| # | MCP Server | Uso en OpenCode | Instalación sugerida |
|---|---|---|---|
| **1** | **Context7** | Traer documentación actualizada de librerías antes de editar código. | `remote` o `npx ctx7 setup --opencode` |
| **2** | **GitHub** | Issues, PRs, revisiones, Actions y trazabilidad de cambios. | `remote` con OAuth si tu organización lo permite |
| **3** | **PostgreSQL / Neon** | Inspeccionar esquemas, índices y datos de desarrollo sin salir del agente. | `local` con connection string por variable de entorno |
| **4** | **Sentry** | Convertir errores reales de producción en planes de fix verificables. | `remote` con `opencode mcp auth sentry` |
| **5** | **Grep by Vercel** | Buscar ejemplos de código en GitHub cuando la documentación no alcanza. | `remote` con `https://mcp.grep.app` |
| **6** | **Sequential Thinking** | Descomponer debugging, arquitectura y migraciones con razonamiento explícito. | `local` con `@modelcontextprotocol/server-sequential-thinking` |

### Cómo instalar MCP en OpenCode

#### 1. Context7 remoto

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp",
      "enabled": true
    }
  }
}
```

#### 2. Context7 setup automático

```bash
npx ctx7 setup --opencode
```

#### 3. GitHub remoto

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "github": {
      "type": "remote",
      "url": "https://api.githubcopilot.com/mcp/",
      "enabled": true
    }
  }
}
```

#### 4. PostgreSQL / Neon

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "neon_pg": {
      "type": "local",
      "command": [
        "npx",
        "-y",
        "@modelcontextprotocol/server-postgres",
        "{env:DATABASE_URL}"
      ],
      "enabled": true
    }
  }
}
```

#### 5. Sentry remoto con OAuth

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev/mcp",
      "oauth": {},
      "enabled": true
    }
  }
}
``` ```bash
opencode mcp auth sentry
```

#### 6. Grep by Vercel

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "gh_grep": {
      "type": "remote",
      "url": "https://mcp.grep.app",
      "enabled": true
    }
  }
}
```

#### 7. Sequential Thinking local

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "sequential_thinking": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-sequential-thinking"],
      "enabled": true
    }
  }
}
```

> **Regla de seguridad**
>
> No pegues tokens ni connection strings reales en `opencode.json`. Usa variables de entorno como `{env:DATABASE_URL}` y versiona solo configuración que el equipo pueda compartir.

---

## 5. Skills en OpenCode

_Duración: 20 minutos_

**Objetivos**

- Crear skills versionables para el repo
- Controlar permisos e invocación
- Combinar Skills + MCP para flujos repetibles

Las Skills de OpenCode siguen el estándar abierto de [Agent Skills](https://agentskills.io/home): carpetas versionables que contienen instrucciones, metadatos y recursos para que un agente ejecute una tarea especializada de forma consistente. OpenCode las descubre en el repo o en tu home y las carga bajo demanda con la herramienta nativa `skill`. Para la clase usaremos `.opencode/skills/<nombre>/SKILL.md` porque esa ruta se versiona con el proyecto.

### Qué es una Skill

> **Procedimiento reusable, no prompt suelto**
>
> Una skill empaqueta conocimiento operativo: cuándo usarla, qué pasos seguir, qué archivos consultar, qué scripts ejecutar y cómo verificar el resultado. El agente solo ve `name` y `description` al inicio; cuando la tarea coincide, carga el `SKILL.md` completo y los recursos necesarios. Esto mantiene bajo el contexto base y vuelve repetibles los flujos del equipo.

### Ubicaciones que OpenCode puede descubrir

| Ubicación | Uso recomendado |
|---|---|
| `.opencode/skills/<name>/SKILL.md` | Skills del proyecto, revisadas por el equipo y compartidas por Git. |
| `~/.config/opencode/skills/<name>/SKILL.md` | Skills personales para todos tus repos. |
| `.claude/skills/<name>/SKILL.md` | Compatibilidad cuando el repo ya trae skills de Claude Code. |
| `.agents/skills/<name>/SKILL.md` | Compatibilidad con repos que estandarizan instrucciones de agentes. |

### Estructura oficial de una Skill

OpenCode reconoce frontmatter YAML con `name` y `description`como campos requeridos. El nombre debe coincidir con la carpeta, estar en minúsculas y usar guiones simples. `estructura` ```text
my-skill/
├── SKILL.md       # requerido: metadata + instrucciones
├── scripts/       # opcional: comandos o utilidades ejecutables
├── references/    # opcional: documentación, guías, specs
├── assets/        # opcional: plantillas, ejemplos, recursos
└── ...            # opcional: archivos adicionales del workflow
``` `.opencode/skills/review-pr/SKILL.md` ```markdown
---
name: review-pr
description: Revisa un PR y reporta riesgos, regresiones y faltantes de test.
license: MIT
compatibility: opencode
metadata:
  audience: developers
  workflow: code-review
---

# Review PR

Revisa el PR indicado por el usuario.

1. Resume los cambios clave.
2. Identifica bugs, regresiones y riesgos de seguridad.
3. Señala gaps de testing.
4. Entrega hallazgos por severidad con archivos afectados.
5. No modifiques archivos durante la revisión.
```

### Permisos por agente

`opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "plan": {
      "permission": {
        "skill": {
          "docs-check": "allow",
          "db-migration": "ask"
        }
      }
    }
  }
}
```

### Instalar Skills útiles en OpenCode

Reinicia OpenCode después de instalar skills: el catálogo se carga al iniciar la sesión. Luego verifica con `list installed skills` o pidiendo explícitamente usar la herramienta `skill`. `frontend-design` ```bash
# Buscar skills disponibles por palabra clave
npx skills search frontend
npx skills search design

# Instalar la skill de UI/UX y frontend de Anthropic
npx skills add https://github.com/anthropics/skills --skill frontend-design

# Verificar dentro de OpenCode
opencode
# Prompt: list installed skills
# Prompt: usa la skill frontend-design para revisar esta interfaz
``` `opencode.json` ```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": [
    "superpowers@git+https://github.com/obra/superpowers.git"
  ]
}
```

> **Superpowers se instala como plugin**
>
> En OpenCode, Superpowers recomienda agregar el plugin a `opencode.json`, reiniciar OpenCode y verificar con el prompt `Tell me about your superpowers`. Si ya usas Superpowers en Claude Code o Codex, instálalo aparte para OpenCode porque cada harness gestiona sus plugins y skills por separado.

### Marketplaces y Repositorios de Skills para OpenCode

- **OpenCode Docs: Agent Skills:** Referencia de ubicación, frontmatter, permisos y troubleshooting de skills. [Documentación](https://opencode.ai/docs/skills/)
- **Agent Skills:** Estándar abierto para `SKILL.md`, estructura de carpetas y progressive disclosure. [Ver estándar](https://agentskills.io/home)
- **skills CLI:** Instalador práctico para buscar con `npx skills search`, traer skills desde repos públicos y detectarlas en agentes compatibles. [Guía OpenCode School](https://opencode.school/lessons/skills/)
- **Repos con .opencode/skills:** Busca en GitHub ejemplos reales de `.opencode/skills/*/SKILL.md` para adaptar workflows de equipos. [Buscar ejemplos](https://github.com/search?q=%22.opencode%2Fskills%22+%22SKILL.md%22&type=code)
- **Compatibilidad .agents/skills:** Útil si tu organización comparte skills entre diferentes agentes CLI. [Buscar en GitHub](https://github.com/search?q=%22.agents%2Fskills%22+%22SKILL.md%22&type=code)
- **Compatibilidad .claude/skills:** OpenCode puede leer skills Claude-compatible, útil para migrar repos existentes sin duplicar contenido. [Buscar en GitHub](https://github.com/search?q=%22.claude%2Fskills%22+%22SKILL.md%22&type=code)
- **Tu repo como marketplace interno:** La mejor fuente para equipos suele ser un directorio versionado con reviews, owners y changelog.

### Top 5 Skills para OpenCode

> **Skills que sí cambian el resultado**
>
> Prioriza skills que codifican criterios del equipo, no prompts genéricos. Cada una debe mejorar consistencia, trazabilidad o seguridad.

| # | Skill | ¿Para qué la uso? | Referencia |
|---|---|---|---|
| **1** | **Superpowers** | Metodología completa para agentes: brainstorming, specs, TDD, ejecución por planes, subagentes y verificación antes de cerrar trabajo. | [obra/superpowers](https://github.com/obra/superpowers) |
| **2** | **frontend-design** | Diseñar y revisar interfaces reales: layout, UX, accesibilidad, estados, responsive y polish visual antes de entregar UI. | [frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) |
| **3** | **mcp-builder** | Construir servidores MCP propios para exponer APIs, datos internos y herramientas del equipo con un contrato reusable. | [mcp-builder](https://github.com/anthropics/skills/tree/main/skills/mcp-builder) |
| **4** | **skill-creator** | Crear nuevas skills bien estructuradas: objetivo claro, frontmatter correcto, recursos de apoyo y verificación de uso. | [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) |
| **5** | **document-toolkit** | Manejar documentos de negocio y clase: leer, transformar, resumir, auditar y generar archivos Word, PDF, PowerPoint y Excel. | [docx](https://github.com/anthropics/skills/tree/main/skills/docx) · [pdf](https://github.com/anthropics/skills/tree/main/skills/pdf) · [pptx](https://github.com/anthropics/skills/tree/main/skills/pptx) · [xlsx](https://github.com/anthropics/skills/tree/main/skills/xlsx) |

### Buenas prácticas

- **Una skill, una intención:** revisión, documentación, migración, incidente o release.
- **Description específica:** explica cuándo usarla; eso mejora el descubrimiento automático.
- **Nombre estable:** usa `kebab-case`, máximo 64 caracteres y sin guiones dobles.
- **Versiona las skills del equipo:** usa `.opencode/skills/` dentro del repo.
- **Controla permisos:** configura `permission.skill` en `opencode.json`.
- **Combina con MCP:** una skill puede exigir documentación, issues, errores o DB antes de decidir.
- **Evita efectos secundarios ocultos:** si la skill revisa, no edita; si edita, verifica.

### Taller práctico: comparar con y sin Skill

**💬 Prompt sin Skill**

```text
Revisa este PR y dime si está bien.
```

**💬 Prompt con Skill**

```text
Usa la skill review-pr para revisar el PR 42. Prioriza bugs, regresiones, seguridad y gaps de testing. No edites archivos.
```

> **Resultado esperado**
>
> La respuesta con skill debe ser más consistente: hallazgos ordenados por severidad, referencias a archivos, riesgos explícitos y pruebas faltantes. Ese es el valor de convertir prompts repetidos en infraestructura.
