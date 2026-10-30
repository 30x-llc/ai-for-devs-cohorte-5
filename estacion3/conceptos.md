# Estación 3 · Conceptos base

[← Volver al índice de la estación 3](README.md)

1. [Bienvenida: El Estado del Arte en 2026](#1-bienvenida-el-estado-del-arte-en-2026)
2. [El Bucle Agéntico (ReAct Loop)](#2-el-bucle-agéntico-react-loop)
3. [Del "Commit" al "Artifact"](#3-del-commit-al-artifact)
4. [Context Engineering vs Prompt Engineering](#4-context-engineering-vs-prompt-engineering)
5. [Requisitos Técnicos](#5-requisitos-técnicos)

---

## 1. Bienvenida: El Estado del Arte en 2026

_Duración: 8 minutos_

**Objetivos**

- Entender la evolución del stack de desarrollo
- Diferenciar Copiloto vs Agente Autónomo
- Conocer las herramientas del ecosistema 2026

### De LLM Wrappers a AI-Native IDEs

En 2023, las herramientas de IA para desarrollo eran **"wrappers"**: interfaces de chat pegadas a un editor de código. En 2026, hemos llegado a los **AI-Native IDEs** — entornos donde la IA no es un complemento, sino el motor principal de la ingeniería.

- **2023: LLM Wrapper:** Chatbot integrado en el editor. Genera snippets de código. El humano copia, pega y adapta manualmente.
- **2024: Copiloto:** Autocompletado contextual. Sugiere código en tiempo real. Aún requiere guía constante del desarrollador.
- **2026: Agente Autónomo:** Lee tu codebase, ejecuta comandos, edita archivos, crea PRs. Piensa, actúa, observa y corrige de forma iterativa.

> **Concepto Clave**
>
> **Ingeniería de Software Agéntica** = Pasar de la *generación de código* (Code Gen) a la *orquestación de sistemas* (System Orchestration). Ya no le dices al agente QUÉ escribir, le diseñas el CONTEXTO para que tome buenas decisiones.

### El Ecosistema de Herramientas

| Herramienta | Tipo | Fortaleza Principal |
|---|---|---|
| **Claude Code** | CLI / Terminal Agéntico | Ejecución táctica directa en tu filesystem para la primera parte del taller |
| **OpenCode** | TUI / Desktop / IDE Open Source | Agente extensible para modelos locales o cloud, con MCP y Skills versionables |
| **Antigravity** | Manager Surface (Web) | Orquestación asíncrona y paralela |
| **Codex** | CLI / Agente de Código | Implementación y edición guiada desde terminal |
| **Cursor** | AI-Native IDE | Edición asistida con contexto visual |
| **Copilot CLI** | CLI / Asistente | Asistencia contextual en terminal y flujos de desarrollo |
| **Gemini CLI** | CLI / Asistente | Ejecución de tareas y automatización desde consola |

#### Prompt de Práctica (Copiar y Pegar)

```text
Prompt: Create a single-page app in a single HTML file with the following requirements:
- Name: Typing Rain
- Goal: Type falling words before they reach the bottom.
- Features: Increasing difficulty, accuracy tracker, score.
- The UI should be the city background with animated raindrop words.
```

---

## 2. El Bucle Agéntico (ReAct Loop)

_Duración: 5 minutos_

**Objetivos**

- Comprender el ciclo Pensar → Actuar → Observar
- Conocer las herramientas del agente

El corazón de toda herramienta de código agéntica es el **Bucle Agéntico** (también conocido como *ReAct Loop*). Es un ciclo iterativo que se repite hasta completar la tarea:

**🧠 Pensar → 📋 Planificar → 🔧 Ejecutar Herramienta → 👁 Observar Resultado → 🔄 Corregir / Repetir**

### Herramientas del Agente

Claude Code tiene acceso a estas herramientas que le permiten interactuar con tu proyecto:

| Herramienta | Descripción | Ejemplo de Uso |
|---|---|---|
| `Read` | Lee contenido de archivos | Leer un archivo de configuración |
| `Edit` | Edita archivos existentes | Corregir un bug en una función |
| `Write` | Crea archivos nuevos | Crear un nuevo componente |
| `Bash` | Ejecuta comandos en terminal | `npm test`, `git diff` |
| `Glob` | Busca archivos por patrón | Encontrar todos los `*.test.ts` |
| `Grep` | Busca contenido dentro de archivos | Buscar dónde se usa una función |
| `Task` | Delega a un subagente | Investigar en paralelo |
| `WebFetch` | Obtiene contenido de la web | Consultar documentación |

> **Para la clase**
>
> Abre una terminal, ejecuta `claude` y pide *"dame un overview de este proyecto"*. Observa en tiempo real cómo Claude usa `Glob` para encontrar archivos, `Read` para leerlos, y construye un resumen.

---

## 3. Del "Commit" al "Artifact"

_Duración: 5 minutos_

La nueva unidad de trabajo ya no es solo el **commit**. Con agentes, trabajamos con **Artifacts**: paquetes completos de trabajo que incluyen:

- **Planes de Implementación:** El agente primero investiga, analiza y propone un plan antes de escribir código.
- **Diffs y Cambios:** Ediciones verificables con contexto de por qué se hizo cada cambio.
- **Verificaciones:** Tests ejecutados, screenshots de validación, pruebas de que funciona.

> **Máxima del Módulo**
>
> *"No escribas código que puedas generar, pero nunca generes código que no sepas verificar."*

### El Problema de la Alucinación

Los agentes pueden generar código que *parece* correcto pero tiene errores sutiles. Por eso, la verificación automática mediante **tests** es más importante que nunca. El **TDD (Test-Driven Development)** se convierte en una necesidad, no solo una buena práctica.

---

## 4. Context Engineering vs Prompt Engineering

_Duración: 5 minutos_

> **Diferencia Clave**
>
> **Prompt Engineering** = Escribir el mensaje correcto al chat.  
>  **Context Engineering** = Diseñar tu sistema de archivos, reglas y configuración para que el agente entienda el contexto sin necesidad de explicárselo en cada mensaje.

En la ingeniería agéntica moderna, la calidad del output depende de:

1. **Contexto** — Qué información tiene el agente sobre tu proyecto (CLAUDE.md, rules, memoria)
2. **Restricciones** — Qué puede y no puede hacer (permisos, settings, hooks)
3. **Skills** — Habilidades reutilizables empaquetadas (scripts versionados)
4. **Protocolos** — Estándares de comunicación (MCP para conectar herramientas externas)

### Checklist: "Agent-Ready Repo"

Tu repositorio está listo para la IA cuando tiene:

- ✅ **Contexto**: `CLAUDE.md` o `AGENTS.md` con instrucciones del proyecto
- ✅ **Restricciones**: `.claude/settings.json` con permisos configurados
- ✅ **Linting**: Herramientas de calidad de código configuradas
- ✅ **Tests**: Suite de tests automatizados que el agente pueda ejecutar
- ✅ **Documentación**: README actualizado con comandos y arquitectura

---

## 5. Requisitos Técnicos

_Duración: 2 minutos_

### Software Necesario

| Requisito | Detalle |
|---|---|
| **Terminal** | macOS Terminal, Linux, o WSL2 en Windows |
| **Node.js** | Versión 22 LTS o superior (el CRM usa Next.js 16 y Prisma 7) |
| **Git** | Para control de versiones |
| **VS Code** | Editor de código (o Cursor) |
| **Claude Code** | Se instala con el comando de la lección 1 de Claude Code |

### Cuentas y Accesos

- Cuenta Claude ([Pro, Max, Teams o Enterprise](https://claude.com/pricing)) **o** cuenta en [Claude Console](https://platform.claude.com)
- Cuenta de GitHub (para demos de PRs y GitHub Actions)

### 🚀 Proyecto Base: CRM

Durante todo el taller trabajaremos sobre un **CRM real** construido con Next.js, Prisma y Tailwind CSS. Clona el repositorio antes de empezar:

`Clonar el proyecto`

```bash
git clone https://github.com/30x-llc/basic_crm_30x.git
cd basic_crm_30x
npm install
```

> **Repositorio del Proyecto**
>
> Este CRM incluye gestión de clientes, interacciones y un dashboard. Todas las prácticas del taller (prompts, flujos, Hooks, Subagentes, Agent Teams, MCPs y Skills) se aplican sobre este proyecto.   
> [🔗 github.com/30x-llc/basic_crm_30x](https://github.com/30x-llc/basic_crm_30x)

### Nivel Requerido

Intermedio: conocimiento de Git, línea de comandos (CLI), y ciclo de vida de desarrollo de software.

> **Cita de Cierre**
>
> *"En 2026, la calidad de tu software ya no depende de tu capacidad para escribir sintaxis, sino de tu capacidad para especificar, restringir y verificar el trabajo de sistemas inteligentes."*
