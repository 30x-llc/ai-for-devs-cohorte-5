# Estación 3 · Antigravity

[← Volver al índice de la estación 3](README.md)

1. [¿Qué es Google Antigravity?](#1-qué-es-google-antigravity)
2. [Instalación y Setup](#2-instalación-y-setup)
3. [Navegación Básica](#3-navegación-básica)
4. [Agent Manager](#4-agent-manager)
5. [Editor y Worktrees](#5-editor-y-worktrees)
6. [Artefactos y Paralelismo](#6-artefactos-y-paralelismo)
7. [FAQ, Planes y Soporte](#7-faq-planes-y-soporte)
8. [Taller Práctico: Tu Primera Sesión en Antigravity](#8-taller-práctico-tu-primera-sesión-en-antigravity)

---

## 1. ¿Qué es Google Antigravity?

_Duración: 5 minutos_

**Objetivos**

- Entender el cambio de paradigma
- Diferenciar síncrono vs asíncrono
- Conocer la "Manager Surface"

### De "Pair Programming" a "Delegación de Tareas"

Google Antigravity es el **AI-Native IDE** de Google — una herramienta de desarrollo donde la IA no es un complemento, sino el motor principal. Su lema es *"Build the new way"*.

- **Antes: Pair Programming (Síncrono):** Tú y el agente trabajan juntos en tiempo real. Tú escribes, el agente sugiere. Requiere tu atención constante.
- **Ahora: Delegación (Asíncrono):** Defines la tarea, el agente trabaja de forma autónoma. Tú revisas los artefactos resultantes cuando termina.

### Antigravity como "Mission Control"

Antigravity es la superficie de gestión (**Manager Surface**) de Google para orquestar agentes de código de forma asíncrona. Piénsalo como un **"Mission Control"** donde:

- 🚀 **Lanzas tareas** — Define qué quieres que el agente haga en lenguaje natural
- 📊 **Monitorizas progreso** — Ves artefactos en tiempo real sin leer logs de chat
- ✅ **Revisas resultados** — Apruebas planes de implementación, diffs visuales, PRs
- 🔄 **Iteras** — Das feedback y el agente ajusta automáticamente

### Dos Componentes Principales

- **Agent Manager:** La vista de gestión donde creas workspaces, lanzas agentes, monitoreas sesiones y revisas resultados. Es tu "centro de mando".
- **Editor:** Editor de código integrado basado en Monaco (VS Code). Edita archivos, navega el proyecto y trabaja con el agente en contexto visual.

> **Concepto Clave**
>
> Dejamos de **leer logs de chat** y empezamos a **revisar Artifacts**: planes de implementación, diffs visuales, screenshots de validación. El output del agente es un documento estructurado, no una conversación.

---

## 2. Instalación y Setup

_Duración: 3 minutos_

**Objetivos**

- Descargar e instalar Antigravity
- Autenticarse con cuenta Google
- Conocer requisitos del sistema

### Descarga

Visita [antigravity.google/download](https://antigravity.google/download) para descargar Google Antigravity.

### Requisitos del Sistema

| Plataforma | Requisito Mínimo |
|---|---|
| **macOS** | Versión 12 (Monterey) o posterior. **Solo Apple Silicon** (ARM). x86 no soportado. |
| **Windows** | Windows 10 (64 bit) o posterior |
| **Linux** | glibc ≥ 2.28, glibcxx ≥ 3.4.25 (Ubuntu 20+, Debian 10+, Fedora 36+, RHEL 8+) |

### Autenticación

Antigravity requiere una **cuenta personal de Google** (@gmail.com). Las cuentas de Workspace pueden tener restricciones según la organización.

> **Requisitos de Autenticación**
>
> - Debes ser **mayor de 18 años** — si tu edad no está verificada, hazlo en [myaccount.google.com/age-verification](https://myaccount.google.com/age-verification)
> - Tu cuenta debe estar en un **país aprobado** (la mayoría de América, Europa, África, Asia y Oceanía)
> - Si tienes problemas con cuentas Workspace, prueba con una cuenta @gmail.com personal

### Actualizaciones

La aplicación muestra una notificación cuando hay actualizaciones disponibles. Solo necesitas hacer clic en **"Restart to Update"** para aplicar la última versión.

### Primer Inicio

1. Descarga e instala Antigravity para tu plataforma
2. Abre la aplicación — se abrirá el **Agent Manager**
3. Inicia sesión con tu cuenta de Google
4. Crea tu primer **Workspace** conectando un repositorio

---

## 3. Navegación Básica

_Duración: 3 minutos_

**Objetivos**

- Navegar entre Agent Manager y Editor
- Dominar atajos de teclado esenciales
- Entender la relación Workspace → Sesión

### Dos Vistas Principales

Antigravity tiene dos vistas que se complementan:

- **Agent Manager:** Vista de gestión. Aquí creas workspaces, lanzas agentes, monitorizas sesiones y revisas artefactos. Es tu vista "macro".
- **Editor:** Vista de código. Editor Monaco con terminal integrada, diffs, y chat lateral con el agente. Es tu vista "micro".

### Atajos de Navegación

| Atajo | Acción |
|---|---|
| Cmd+E | Alternar entre Agent Manager ↔ Editor |
| Cmd+K | Abrir Command Palette (buscar cualquier acción) |
| Cmd+P | Buscar archivos rápidamente (Quick Open) |
| Cmd+Shift+P | Paleta de comandos del Editor |
| Cmd+J | Abrir/cerrar terminal integrada |

> **Tip: Flujo recomendado**
>
> Usa el **Agent Manager** para lanzar y monitorear tareas. Cuando necesites ver los cambios en detalle o editar manualmente, cambia al **Editor** con Cmd+E. Puedes abrir el Editor desde cualquier workspace haciendo clic en *"Focus Editor"* en el menú dropdown del workspace.

### Flujo de Navegación

**📋 Agent Manager → ✏️ Editor → 🖥 Terminal**

Desde el **Agent Manager**, selecciona un workspace y haz clic en *"Open Editor"* — o usa Cmd+E. Desde el **Editor**, el botón en la barra superior o Cmd+E te regresa al Agent Manager.

---

## 4. Agent Manager

_Duración: 5 minutos_

**Objetivos**

- Crear y configurar Workspaces
- Lanzar sesiones de agente
- Entender checkpoints y revisión

### ¿Qué es un Workspace?

Un **Workspace** es la unidad de trabajo en Antigravity. Conecta un repositorio Git y proporciona el contexto donde el agente opera.

### Crear un Workspace

1. Abre el **Agent Manager**
2. Haz clic en **"New Workspace"**
3. Conecta un repositorio: GitHub, GitLab, o repositorio local
4. El agente clonará e indexará el repositorio automáticamente

### Lanzar una Sesión

Una vez creado el workspace, describe tu tarea en lenguaje natural:

**💬 Tarea CRM — Agent Manager**

```text
Analiza el schema de Prisma del CRM (modelos Customer e Interaction) y agrega
              validación de email y teléfono al endpoint POST /api/customers. Incluye mensajes de error claros y tests.
```

El agente seguirá un flujo estructurado:

**🧠 Analiza el codebase → 📋 Crea un plan → ✅ Espera aprobación → ⚙️ Implementa cambios → 🧪 Verifica y entrega**

### Gestión de Sesiones

| Acción | Descripción |
|---|---|
| **Detener** | Pausa la sesión del agente. Puedes reanudarla después. |
| **Checkpoint** | El agente crea snapshots automáticos antes de cada cambio mayor. |
| **Rewind** | Revierte al checkpoint anterior si los cambios no son satisfactorios. |
| **Branching** | Cada sesión trabaja en una rama separada para no afectar `main`. |

> **Múltiples Sesiones**
>
> Puedes lanzar **múltiples sesiones simultáneas** en el mismo workspace o en diferentes workspaces. Cada sesión tiene su propia rama y contexto independiente — esto es lo que permite el paralelismo real.

---

## 5. Editor y Worktrees

_Duración: 3 minutos_

**Objetivos**

- Usar el editor integrado
- Entender el chat lateral del agente
- Conocer las limitaciones actuales

### El Editor de Antigravity

El editor está basado en **Monaco** (el mismo motor de VS Code), lo que significa que ya conoces la mayoría de los atajos:

| Característica | Descripción |
|---|---|
| **Sintaxis highlighting** | Soporte para todos los lenguajes populares |
| **Terminal integrada** | Ejecuta comandos directamente desde el editor |
| **Diffs visuales** | Visualiza los cambios del agente como diffs lado a lado |
| **Chat lateral** | Comunícate con el agente mientras ves el código |
| **Quick Open** | Cmd+P para navegar archivos rápidamente |

### Chat del Agente en el Editor

En el panel lateral del editor puedes interactuar con el agente en contexto:

**💬 Tarea CRM — Editor**

```text
@src/components/CustomerTable.tsx la tabla de clientes no muestra el status con
              colores diferenciados. Agrega badges con colores para LEAD (amarillo), ACTIVE (verde) y CHURNED (rojo)
              usando clases de Tailwind.
```

### Revisando Cambios del Agente

Cuando el agente hace cambios, los verás como:

- 📝 **Diffs inline** — Cambios resaltados directamente en el archivo
- ✅ **Botón "Accept"** — Para aceptar los cambios propuestos
- ↩️ **Botón "Revert"** — Para rechazar y volver al estado anterior
- 💬 **Comentar** — Pide ajustes sin aceptar ni rechazar

> **Limitación Actual**
>
> Google Antigravity **no soporta Git worktrees** en este momento. Cada workspace opera con un solo working directory. Si necesitas trabajar en múltiples ramas simultáneamente, crea workspaces separados.

### Sleep y Sesiones Activas

Si un agente está ejecutándose, Antigravity **evitará que tu computadora entre en modo sleep** para no interrumpir la tarea. Esto sucede automáticamente mientras haya una sesión activa.

---

## 6. Artefactos y Paralelismo

_Duración: 5 minutos_

**Objetivos**

- Entender los tipos de artefactos
- Lanzar agentes en paralelo
- Resolver silos de contexto

### Tipos de Artefactos

En la orquestación asíncrona, el agente produce **artefactos** — documentos estructurados que contienen su trabajo:

- **Plan de Implementación:** Análisis del problema, propuesta de cambios organizados por componente, plan de verificación. Tú lo apruebas antes de que el agente ejecute.
- **Diffs Visuales:** Cambios de código mostrados como diffs con contexto: qué se cambió, por qué, y cómo afecta al sistema.
- **Screenshots de Validación:** Capturas que demuestran que los cambios funcionan. El agente puede tomar screenshots del UI o de resultados de tests.
- **Walkthrough:** Resumen final de qué se hizo, qué se probó, y resultados de validación. Documentación del trabajo completado.

### Flujo de Trabajo con Artefactos

**📝 Defines Tarea → 📋 Agente Planifica → ✅ Apruebas Plan → ⚙️ Agente Ejecuta → 📊 Revisas Artefactos**

### Paralelismo de Agentes

Una de las ventajas más poderosas de Antigravity es lanzar **múltiples agentes en paralelo**:

- **Agente 1: Fix Bug #42:** Investiga y corrige el bug reportado en el issue de GitHub.
- **Agente 2: Escribir Tests:** Genera tests para el módulo de pagos.
- **Agente 3: Documentación:** Actualiza la documentación de la API.

Cada agente trabaja de forma independiente con su propia rama y ventana de contexto. Los resultados se entregan como PRs independientes que puedes revisar y aprobar.

### El Problema del "Silo de Contexto"

Cuando cada desarrollador tiene su propio historial de chat, la IA no aprende del equipo. La solución:

- **Project Knowledge (Harness):** Fuentes de verdad compartidas (`.claude/rules`, `CLAUDE.md` a nivel de organización)
- **Prompts como Infraestructura:** Versionado de Skills en el repositorio para que todo el equipo use los mismos estándares
- **Artefactos Compartidos:** El equipo colabora sobre el output generado (comentarios, iteraciones, hand-off humano-máquina)

> **Para la Clase**
>
> La diferencia clave con un chat es que los artefactos son **documentos colaborativos**: puedes comentar, pedir cambios, y el agente los actualiza — similar a Google Docs pero para código.

---

## 7. FAQ, Planes y Soporte

_Duración: 3 minutos_

**Objetivos**

- Conocer los planes disponibles
- Resolver problemas comunes
- Saber sobre límites de uso y privacidad

### Planes de Acceso

Google Antigravity está vinculado al ecosistema de Google AI. Existen varias formas de acceder:

| Plan | Acceso | Límites |
|---|---|---|
| **Google One AI Premium** | Cuenta personal con suscripción | Cuota estándar de uso |
| **Google Workspace AI Ultra** | Cuentas empresariales con add-on | Cuotas incrementadas para equipos |
| **GCP Project** | Solo por invitación a equipos seleccionados | Configuración personalizada |

Para más detalles sobre rate limits específicos, consulta la documentación de [Planes](https://antigravity.google/docs/plans).

### Preguntas Frecuentes

#### ¿Por qué no puedo autenticarme?

- Antigravity está disponible para **cuentas personales de Google** en geografías aprobadas
- Si tienes problemas con cuentas Workspace, prueba con un @gmail.com
- Debes ser mayor de 18 años. [Verifica tu edad aquí](https://myaccount.google.com/age-verification)

#### ¿Puedo usar Antigravity con herramientas de terceros?

> **Importante**
>
> Usar software de terceros (como Claude Code, OpenClaw, OpenCode) con tu login de Antigravity es una **violación de los Términos de Servicio** y puede resultar en la suspensión de tu cuenta. Si quieres usar un agente de terceros con Gemini, usa una API key de Vertex o AI Studio.

#### Privacidad y Datos

Puedes **optar por no compartir datos** en cualquier momento desde el panel de Settings. Consulta los [Términos de Servicio](https://antigravity.google/terms) para detalles completos.

#### ¿Se soportan Git Worktrees?

No por el momento. Cada workspace opera con un solo working directory.

#### ¿Qué pasa cuando mi computadora entra en modo sleep?

Si un agente está activo, Antigravity **previene automáticamente** que tu computadora entre en modo sleep para no interrumpir la ejecución.

### Soporte

Visita la [página de soporte](https://antigravity.google/support) y las comunidades oficiales.

---

## 8. Taller Práctico: Tu Primera Sesión en Antigravity

_Duración: 8 minutos_

**Objetivos**

- Crear tu primer workspace
- Lanzar y monitorear una sesión de agente
- Revisar artefactos y aprobar cambios

### Ejercicio 1: Setup del Workspace

1. Abre **Antigravity** y accede al **Agent Manager**
2. Crea un nuevo workspace conectando un repositorio de GitHub o un proyecto local
3. Espera a que el agente indexe el código base
4. Explora la interfaz: sidebar de archivos, vista de sesiones, panel de artefactos

### Ejercicio 2: Tu Primera Tarea

Lanza una tarea sencilla para ver el flujo completo:

**💬 Tarea CRM — Análisis Inicial**

```text
Analiza la arquitectura del CRM: el schema de Prisma (Customer, Interaction, User),
              las rutas de API en src/app/api/, y los componentes en src/components/. Dame un resumen del stack y
              sugiere 3 mejoras prioritarias.
```

Observa cómo el agente:

- 📂 Escanea la estructura de archivos
- 📖 Lee archivos clave (package.json, configs, entry points)
- 📋 Genera un **Plan de Implementación** como artefacto
- 🧪 Propone cambios verificables

### Ejercicio 3: Tarea con Implementación

**💬 Tarea CRM — Implementación**

```text
Agrega validación de input al endpoint POST /api/interactions del CRM. Debe validar
              que el customerId exista en la base de datos, que el campo 'type' sea uno de EMAIL/CALL/MEETING/NOTE, y
              que 'notes' no esté vacío. Retorna errores 400 descriptivos.
```

Para este ejercicio, practica:

1. **Revisar el plan** — ¿El agente entendió correctamente los requisitos?
2. **Aprobar con ajuste** — Comenta pidiendo un cambio antes de aprobarlo
3. **Revisar diffs** — Cambia al Editor con Cmd+E y revisa los cambios
4. **Verificar tests** — Confirma que los tests fueron creados y pasan

### Ejercicio 4: Paralelismo

Lanza **dos sesiones simultáneas** en el mismo workspace:

- **Sesión A: Dashboard del CRM:** *"Implementa un dashboard en src/app/page.tsx que muestre KPIs: total de clientes, clientes activos vs churned, e interacciones de la última semana. Usa Server Components y queries de Prisma"*
- **Sesión B: Búsqueda de Clientes:** *"Agrega filtrado por status (LEAD/ACTIVE/CHURNED) y búsqueda por nombre/email al endpoint GET /api/customers. Incluye query params y paginación"*

Observa cómo cada sesión trabaja en su propia rama sin interferir con la otra.

### Checklist Final: ¿Completaste el Taller?

- ☐ Workspace creado y repositorio conectado
- ☐ Primera tarea de análisis completada y artefactos revisados
- ☐ Tarea de implementación con plan aprobado y código verificado
- ☐ Dos sesiones paralelas lanzadas exitosamente
- ☐ Navegación fluida entre Agent Manager y Editor (Cmd+E)

> **Entregables del Taller**
>
> 1. Al menos un **artefacto revisado y aprobado** (plan de implementación o walkthrough)  
>  2. Código implementado por el agente con **tests pasando**  
>  3. Experiencia navegando entre **Agent Manager ↔ Editor**

### Recursos Adicionales

| Recurso | Link |
|---|---|
| Documentación Oficial Antigravity | [antigravity.google/docs](https://antigravity.google/docs) |
| Página de Descarga | [antigravity.google/download](https://antigravity.google/download) |
| FAQ y Soporte | [FAQ](https://antigravity.google/docs/faq) |
| Términos de Servicio | [Términos](https://antigravity.google/terms) |
| Planes y Precios | [Planes](https://antigravity.google/docs/plans) |
| Soporte / Comunidades | [Soporte](https://antigravity.google/support) |
| Documentación Oficial Claude Code | [code.claude.com/docs](https://code.claude.com/docs) |
| Discord Anthropic | [Community Discord](https://www.anthropic.com/discord) |
