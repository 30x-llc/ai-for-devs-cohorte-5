# Estación 3 · Claude Code

[← Volver al índice de la estación 3](README.md)

1. [Instalación y Primeros Pasos](#1-instalación-y-primeros-pasos)
2. [Modo Interactivo: Atajos y Permisos](#2-modo-interactivo-atajos-y-permisos)
3. [Settings y Permisos](#3-settings-y-permisos)
4. [Memoria: CLAUDE.md y Auto Memory](#4-memoria-claudemd-y-auto-memory)
5. [Flujos de Trabajo Reales](#5-flujos-de-trabajo-reales)
6. [Hooks — Automatización por Eventos](#6-hooks--automatización-por-eventos)
7. [Subagentes — Delegación Inteligente](#7-subagentes--delegación-inteligente)
8. [Headless, Costos y CI/CD](#8-headless-costos-y-cicd)
9. [Agent Teams — Equipos de Agentes en Paralelo](#9-agent-teams--equipos-de-agentes-en-paralelo)

---

## 1. Instalación y Primeros Pasos

_Duración: 5 minutos_

**Objetivos**

- Instalar Claude Code CLI
- Completar autenticación
- Ejecutar primera exploración

### ¿Qué es Claude Code?

Claude Code es una herramienta de código agéntica que **lee tu codebase, edita archivos y ejecuta comandos**. Funciona en tu terminal, IDE, navegador y como app de escritorio.

### Instalación

`macOS / Linux /
                WSL`

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

`Homebrew`

```bash
brew install --cask claude-code
```

`Windows
                PowerShell`

```powershell
irm https://claude.ai/install.ps1 | iex
```

### Primera Sesión

```bash
cd tu-proyecto
claude
# Se te pedirá iniciar sesión la primera vez
# Usa /login si necesitas cambiar de cuenta
```

### Modelos Disponibles

| Modelo | Caso de Uso | Costo |
|---|---|---|
| **Opus 4.7** | Razonamiento complejo, arquitectura | $$$ |
| **Sonnet 4.6** | Tareas diarias, balance costo/capacidad | $$ |
| **Haiku** | Tareas simples, búsquedas rápidas | $ |

### Primer Prompt: Explorar tu Proyecto

**💬 Prompt**

```text
dame un overview de este CRM, su arquitectura en Next.js y esquema de datos en
              Prisma
```

> **Observa el Bucle**
>
> Claude usará `Glob` para encontrar archivos, `Read` para leerlos, y construirá un resumen arquitectónico. Observa las herramientas que usa en la terminal.

---

## 2. Modo Interactivo: Atajos y Permisos

_Duración: 5 minutos_

**Objetivos**

- Dominar atajos de teclado
- Entender los 3 modos de permisos
- Usar menciones con @

### Los 3 Modos de Permisos

Alterna entre modos con Shift+Tab:

- **Plan Mode:** Solo lee y analiza. **Nunca modifica** archivos. Ideal para exploración segura y diagnóstico.
- **Normal Mode:** Claude pide **aprobación** antes de cada acción que modifica archivos o ejecuta comandos.
- **Auto-Accept:** Claude ejecuta **sin preguntar**. Usar con precaución — ideal para tareas de confianza.

### Atajos Esenciales

| Atajo | Función |
|---|---|
| Ctrl+C | Cancelar acción actual |
| Ctrl+R | Buscar en historial de prompts |
| Ctrl+O | Ver pensamiento extendido (verbose) |
| Ctrl+L | Limpiar pantalla |
| Shift+Enter | Prompt multilínea |
| Esc+Esc | Rewind — revertir al checkpoint anterior |
| Shift+Tab | Cambiar modo de permisos |

### Menciones con @

Da contexto directo incluyendo archivos o directorios en tu prompt:

**💬 Prompt — @ Mención en el CRM**

```text
@src/app/api/customers/route.ts ¿hay alguna vulnerabilidad de seguridad en esta
              ruta de API? ¿Se valida el input antes de hacer queries a Prisma?
```

### Comandos Integrados

| Comando | Descripción |
|---|---|
| `/help` | Mostrar ayuda y comandos disponibles |
| `/clear` | Limpiar historial de conversación |
| `/usage` | Ver costo acumulado de la sesión |
| `/context` | Ver uso de la ventana de contexto |
| `/compact` | Compactar contexto preservando lo esencial |
| `/model sonnet` | Cambiar modelo mid-session |
| `/resume` | Reanudar conversación anterior |
| `/rename` | Nombrar la sesión actual |
| `!comando` | Ejecutar comando bash directamente |

---

## 3. Settings y Permisos

_Duración: 3 minutos_

**Objetivos**

- Entender la jerarquía de scopes
- Configurar reglas de permisos
- Proteger archivos sensibles

### Jerarquía de Scopes (Precedencia)

Las configuraciones se aplican de mayor a menor prioridad:

| Scope | Ubicación | ¿Compartido? | Uso |
|---|---|---|---|
| **Managed** | Sistema (`/etc/claude-code/`) | Sí (IT) | Políticas corporativas |
| **User** | `~/.claude/settings.json` | No | Preferencias personales |
| **Project** | `.claude/settings.json` | Sí (Git) | Reglas del equipo |
| **Local** | `.claude/settings.local.json` | No (gitignored) | Overrides personales |

### Ejemplo: settings.json del Proyecto

`.claude/settings.json`

```json
{
  "permissions": {
    "deny": [
      "Read(.env)",
      "Read(no_leer.txt)",
      "Read(**/*.pem)",
      "Edit(.env)",
      "Bash(rm -rf *)",
      "Bash(DROP TABLE *)"
    ],
    "allow": [
      "Bash(npm run *)",
      "Bash(npx prisma *)",
      "Bash(git *)",
      "Read",
      "Edit",
      "Write"
    ]
  }
}
```

> **Seguridad**
>
> Las reglas `deny` siempre tienen prioridad sobre `allow`. Si un archivo está en deny, Claude NO podrá leerlo ni editarlo, sin importar las reglas de allow.

### Ejercicio Práctico

1. Crea el archivo `.claude/settings.json` con las reglas de arriba
2. Abre `claude` e intenta: *"lee el archivo secrets.env"*
3. Verifica que Claude bloquea el acceso
4. Usa `/config` para explorar la configuración interactiva

---

## 4. Memoria: CLAUDE.md y Auto Memory

_Duración: 5 minutos_

**Objetivos**

- Escribir un CLAUDE.md efectivo
- Entender Auto Memory
- Usar reglas modulares

> **Dos Tipos de Memoria**
>
> **CLAUDE.md** = Instrucciones que TÚ escribes para Claude (reglas, stack, convenciones)  
>  **Auto Memory** = Notas que CLAUDE escribe para sí mismo (patrones aprendidos, insights)

### Jerarquía de Memoria

| Archivo | Propósito | Compartido |
|---|---|---|
| `~/.claude/CLAUDE.md` | Preferencias personales globales | No |
| `./CLAUDE.md` | Instrucciones del proyecto | Sí (Git) |
| `./CLAUDE.local.md` | Preferencias por proyecto (gitignored) | No |
| `.claude/rules/*.md` | Reglas modulares por tema | Sí (Git) |
| `~/.claude/projects/<proyecto>/memory/` | Auto Memory (notas de Claude) | No |

### Template: CLAUDE.md Efectivo

`CLAUDE.md`

```markdown
# CRM Proyecto - Instrucciones para Claude

## Stack
- Runtime: Node.js 20+ con TypeScript y React 19
- Framework: Next.js 16 (App Router)
- Estilos: Tailwind CSS v4
- ORM: Prisma con PostgreSQL (Neon DB)
- Pruebas: Vitest

## Comandos
- Dev server: `npm run dev`
- Build: `npm run build`
- DB generate: `npx prisma generate`
- Linter: `npm run lint`

## Convenciones
- Componentes de UI van en `src/components/` (usamos Lucide React para íconos)
- Lógica de backend y endpoints en `src/app/api/`
- Páginas de la app van en `src/app/`
- Componentes de servidor (RSC) por defecto. Usa `"use client"` solo si necesitas interactividad o hooks de React
- Las mutaciones a la base de datos se hacen usando Server Actions o en rutas de API

## Seguridad
- NUNCA commitear archivos `.env.local` ni secrets
- NUNCA retornar objetos Prisma completos al frontend, filtra datos sensibles
- Valida entradas de usuario antes de tocar la DB
```

### Reglas Modulares

`.claude/rules/testing.md`

```markdown
# Convenciones de Testing

- Framework: Vitest + Supertest para tests de integración
- Cada test debe cubrir: happy path, errores, edge cases
- Nombrar tests descriptivamente:
  "should return 404 when product not found"
- Usar factories para datos de prueba, no hardcodear
```

### Auto Memory

Puedes decirle a Claude que recuerde algo específico:

**💬 Prompt**

```text
recuerda que siempre usamos componentes de servidor (RSC) en Next.js por defecto
```

Usa `/memory` para revisar y editar lo que Claude ha guardado.

### Generar CLAUDE.md Automáticamente

```bash
# Dentro de una sesión de Claude Code:
/init
# Claude analizará tu proyecto y generará un CLAUDE.md base
```

---

## 5. Flujos de Trabajo Reales

_Duración: 7 minutos_

**Objetivos**

- Debug agéntico con stack traces
- Refactorización incremental
- Commits y PRs conversacionales

### Debug Agéntico

Pega el error directamente en Claude y deja que trace la causa raíz:

**💬 Prompt — Debug del CRM**

```text
Tengo este error en el CRM:
              TypeError: Cannot read properties of undefined (reading 'name')
              at GET (src/app/api/customers/route.ts:15)

              Diagnostica la causa raíz en la ruta de clientes y corrígelo. Verifica con npm run build
```

### Refactorización Incremental

> **Regla de Oro**
>
> Pide cambios en **pasos pequeños y verificables**. Siempre incluye un comando de verificación.

**💬 Prompt — Refactorización del CRM**

```text
refactoriza src/components/Sidebar.tsx para usar iconos de Lucide React en lugar de
              emojis o SVG, manteniendo el mismo
              comportamiento visual. Agrega hover effects con Tailwind. Después de cada cambio, corre npm run lint
```

### Checkpoints y Rewind

Claude crea **checkpoints automáticos** antes de cada cambio. Si algo sale mal:

- Esc+Esc — Revertir al estado anterior (Rewind)
- **Restore**: Revierte código al checkpoint
- **Summarize**: Mantiene los cambios pero limpia el contexto

### Git y Pull Requests

**💬 Prompt — Code Review del CRM**

```text
revisa todos mis cambios contra main buscando problemas de seguridad en las rutas
              de API del CRM y rendimiento en las queries de Prisma
```

**💬 Prompt**

```text
commitea los bug fixes en un commit y la refactorización en otro, con mensajes
              descriptivos
```

**💬 Prompt — PR del CRM**

```text
crea un PR con título descriptivo, descripción detallada de los cambios al CRM de
              clientes, y un test plan que incluya los endpoints de /api/customers y /api/interactions
```

### Gestión de Sesiones

```bash
# Nombrar la sesión actual
/rename bugfix-authentication

# Continuar la sesión más reciente
claude -c

# Seleccionar sesión para reanudar
claude -r

# Reanudar por nombre
claude --resume bugfix-authentication
```

---

## 6. Hooks — Automatización por Eventos

_Duración: 5 minutos_

**Objetivos**

- Configurar hooks en settings.json
- Usar matchers para filtrar herramientas
- Auto-formatear y bloquear archivos

> **Skills vs Hooks**
>
> **Skills** = Claude decide cuándo usarlas (o tú las invocas con `/nombre`).  
>  **Hooks** = Comandos deterministas que **siempre** se ejecutan en eventos específicos.

### ¿Qué hace un Hook y cómo se estructura?

Un hook ejecuta lógica automática en puntos concretos del ciclo de Claude Code. La estructura oficial es: **evento → matcher → hooks[]**. En hooks de comando, Claude envía JSON por stdin con el contexto del evento, y tu script decide si permite, bloquea o solo registra.

`Estructura mínima en settings.json`

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-risky-commands.sh"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/format.sh"
          }
        ]
      }
    ]
  }
}
```

`.claude/hooks/block-risky-commands.sh`

```bash
#!/usr/bin/env bash
INPUT="$(cat)"
CMD="$(echo "$INPUT" | jq -r '.tool_input.command // empty')"

if echo "$CMD" | grep -Eq 'rm -rf|sudo rm'; then
  jq -n '{
    hookSpecificOutput: {
      hookEventName: "PreToolUse",
      permissionDecision: "deny",
      permissionDecisionReason: "Comando destructivo bloqueado por policy"
    }
  }'
  exit 0
fi

exit 0
```

### Tipos de Hook Handler

- `type: "command"`: corre scripts/comandos shell (determinista)
- `type: "prompt"`: validación de una condición con modelo (single-turn)
- `type: "agent"`: delega validación a subagente con herramientas

### Eventos del Ciclo de Vida

| Evento | Cuándo se ejecuta | Uso típico |
|---|---|---|
| `PreToolUse` | Antes de usar una herramienta | Bloquear ediciones a archivos protegidos |
| `PostToolUse` | Después de usar una herramienta | Auto-formatear código |
| `Stop` | Cuando Claude termina de responder | Validación final |
| `Notification` | Cuando Claude espera input | Notificaciones de escritorio |
| `SessionStart` | Al iniciar sesión | Inyectar variables de entorno |
| `PreCompact` | Antes de compactar contexto | Re-inyectar info crítica |

### Ejemplo Completo de Hooks

`.claude/settings.json (sección hooks)`

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "file=$(jq -r '.tool_input.file_path // empty'); [ -n \"$file\" ] && npx prettier --write \"$file\" 2>/dev/null || true"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "file=$(jq -r '.tool_input.file_path // empty'); if echo \"$file\" | grep -q 'prisma/schema.prisma'; then echo 'Blocked: No editar prisma/schema.prisma directamente.' >&2; exit 2; fi"
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "notify-send 'Claude Code' 'Necesita tu atención' 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

#### Ejemplos para probar

1. Crea un archivo `test.js` mal formateado.
2. Edita `prisma/schema.prisma` agregando un comentario.

> **Tip**
>
> Usa `/hooks` dentro de Claude Code para crear y gestionar hooks visualmente sin editar JSON manualmente.

### 🏪 Marketplaces y Repositorios de Hooks

- **GitHub: Claude Code Hooks Examples:** Ejemplos oficiales de Hooks para distintos flujos de trabajo. [GitHub Repo](https://github.com/anthropics/claude-code)
- **Anthropic Docs: Hooks:** Guía oficial de configuración y eventos disponibles. [Documentación](https://docs.anthropic.com/en/docs/claude-code/hooks)
- **Community Hook Recipes:** Recetas de la comunidad para auto-formateo, linting, notificaciones y protección de archivos. [Discord #claude-code](https://www.anthropic.com/discord)
- **AITMPL:** Plantillas de automatización que puedes adaptar como Hooks de validación y flujo. [aitmpl.com](https://www.aitmpl.com/)

---

## 7. Subagentes — Delegación Inteligente

_Duración: 3 minutos_

**Objetivos**

- Conocer los subagentes built-in
- Crear subagentes personalizados
- Decidir cuándo delegar

### Subagentes Integrados

- **Explore:** Modelo Haiku, solo lectura. Optimizado para búsqueda rápida en el codebase. Económico.
- **Plan:** Hereda modelo, solo lectura. Investiga para construir planes detallados de implementación.
- **General-purpose:** Hereda modelo, todas las herramientas. Para tareas complejas multi-paso.

### Crear un Subagente Personalizado

`.claude/agents/test-writer.md`

```markdown
---
name: "test-writer"
description: Genera tests unitarios e integración para componentes React y rutas API
model: sonnet
color: red
tools:
  - Read
  - Edit
  - Write
  - Bash
  - Glob
  - Grep
---

# Test Writer Agent

Especialista en escribir tests completos y confiables con Vitest + React Testing Library.

## Stack de Testing

- **Framework**: Vitest (con `vitest.config.ts`)
- **Testing Library**: React Testing Library para componentes
- **Assertions**: expect() de Vitest
- **HTTP Testing**: Supertest para rutas API
- **Cobertura mínima**: 80% de líneas

## Reglas de Testing

### Componentes React (`src/components/`)

- Renderizar sin errores (render + expect)
- Testear interacciones: clics, inputs, cambios de estado
- Usar `screen.getByRole()`, `getByTestId()` solo si necesario
- Mockear `next/navigation`: `useRouter`, `usePathname`, `useSearchParams`
- Mockear llamadas a APIs: fetch, POST, DELETE
- Evitar mockear la lógica interna (test del comportamiento, no la implementación)

### Rutas API (`src/app/api/`)

- Testear status codes (200, 404, 400, 500)
- Validar payload de request/response
- Mockear Prisma: `prismock` o manual `jest.mock()`
- Probar casos de error: registros no encontrados, validación fallida
- Usar Supertest: `request(app).get('/api/...')`

### Estructura de Archivos

```
src/
  components/
    Sidebar.tsx
  __tests__/
    components/
      Sidebar.test.tsx
    api/
      customers.test.ts
```

### Nombres Descriptivos

- ✅ "debe renderizar la tabla con filas de clientes"
- ✅ "retorna 404 cuando cliente no existe"
- ❌ "test 1", "works", "basic test"

## Proceso

1. **Leer el código**: Entiende el componente/ruta
   - Props, estado, efectos secundarios
   - Dependencias externas (APIs, librerías)

2. **Identificar escenarios**:
   - Happy path (caso exitoso)
   - Casos de error (validación, no encontrado)
   - Edge cases (entrada vacía, valores extremos)

3. **Escribir tests**:
   - Describe: agrupar tests por funcionalidad
   - It/test: un comportamiento por test
   - Arrange-Act-Assert pattern

4. **Mockear dependencias**:
   - Prisma: crear mock del cliente
   - APIs externas: mockear fetch/axios
   - Next.js: mockear `next/navigation`

5. **Ejecutar y validar**:
   - `npm run test` para ejecutar todos
   - `npm run test:watch` durante desarrollo
   - Verificar cobertura: `npm run test:coverage`

## Ejemplo: Componente

```tsx
// src/components/CustomerTable.test.tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { CustomerTable } from "./CustomerTable";

describe("CustomerTable", () => {
  it("debe renderizar tabla con clientes", () => {
    const customers = [{ id: "1", name: "Juan", email: "juan@example.com" }];
    render();
    expect(screen.getByText("Juan")).toBeInTheDocument();
  });

  it("debe llamar onDelete cuando se hace clic en eliminar", async () => {
    const onDelete = vi.fn();
    render(
      ,
    );
    await userEvent.click(screen.getByRole("button", { name: /eliminar/i }));
    expect(onDelete).toHaveBeenCalledWith("1");
  });
});
```

## Ejemplo: Ruta API

```ts
// src/app/api/customers/[id]/route.test.ts
import { describe, it, expect, beforeEach, vi } from "vitest";
import { GET } from "./route";

describe("GET /api/customers/[id]", () => {
  it("debe retornar cliente cuando existe", async () => {
    const response = await GET(
      new Request("http://localhost:3000/api/customers/123"),
      { params: { id: "123" } },
    );
    expect(response.status).toBe(200);
  });

  it("debe retornar 404 cuando cliente no existe", async () => {
    const response = await GET(
      new Request("http://localhost:3000/api/customers/999"),
      { params: { id: "999" } },
    );
    expect(response.status).toBe(404);
  });
});
```
```

### Otro Ejemplo: Code Reviewer

`.claude/agents/code-reviewer.md`

```markdown
---
name: "code-reviewer"
description: Revisa código buscando bugs, seguridad, performance y code smells
model: haiku
tools:
  - Read
  - Grep
  - Glob
  - Bash
color: green
---

# Code Reviewer Agent

Especialista en auditoría de código con enfoque en seguridad, calidad y performance.

## Áreas de Revisión

### 🔴 CRITICAL (Bloquea merge)

- **Seguridad**: SQL injection, XSS, CSRF, autenticación ausente
- **Datos sensibles**: secrets en código, logs de contraseñas
- **Crashes**: null dereferences, tipos incorrectos, missing null checks
- **DB**: N+1 queries, migraciones sin rollback, deadlocks

### 🟠 HIGH (Revisar antes de merge)

- **Bugs lógicos**: race conditions, estado inconsistente, edge cases
- **Performance**: queries sin índices, renders innecesarios, memory leaks
- **Code smells**: funciones >50 líneas, complejidad ciclomática >10
- **Testing**: rutas críticas sin tests, cobertura <60%

### 🟡 MEDIUM (Nice to have)

- **Duplicación**: código repetido, DRY violations
- **Naming**: variables confusas, funciones genéricas
- **Documentación**: APIs sin JSDoc, lógica no documentada
- **Imports**: unused, circular dependencies

### 🔵 LOW (Cosmético)

- **Estilo**: formatting, trailing spaces, imports no ordenados
- **Logging**: verbosidad, nivel de detalle
- **Comments**: obvios, desactualizados

## Proceso

1. **Recibir archivos/ruta**: Usuario indica qué revisar.
2. **Analizar línea por línea**: Buscar vulnerabilidades, performance issues, y code smells.
3. **Documentar hallazgos**: Generar un reporte claro por severidades.
```

> **¿Cuándo delegar?**
>
> Usa subagentes cuando quieras: **preservar contexto principal**, **controlar costos** (Haiku para tareas simples), y **especializar comportamiento**.

---

## 8. Headless, Costos y CI/CD

_Duración: 2 minutos_

**Objetivos**

- Usar modo headless (-p) en scripts
- Monitorear y optimizar costos
- Configurar GitHub Actions con Claude

### Modo Headless (-p)

Ejecuta Claude en modo no-interactivo para integrarlo en scripts y pipelines:

`Scripts de
                ejemplo`

```bash
# Query simple
claude -p "explica qué hace la función processPayment"

# Piping: analizar logs
cat error.log | claude -p "explica este error y sugiere la solución"

# Output JSON estructurado
claude -p "lista los endpoints de la API" --output-format json

# Limitar presupuesto
claude -p "revisa seguridad del proyecto" --max-budget-usd 2.00

# Linter semántico con Claude
git diff main --name-only -- '*.ts' | while read file; do
  cat "$file" | claude -p \
    "Busca bugs lógicos y vulnerabilidades. Formato: ARCHIVO:LINEA - PROBLEMA" \
    --output-format text --max-budget-usd 0.50
done
```

### Monitoreo de Costos

| Comando | Información |
|---|---|
| `/cost` | Costo total de la sesión, duración, cambios |
| `/context` | Qué consume espacio en la ventana de contexto |
| `/compact` | Compactar contexto (preservar lo esencial) |
| `/model sonnet` | Cambiar a modelo más económico |

> **Optimización de costos**
>
> **opusplan**: Opus planifica → Sonnet ejecuta. Mejor balance calidad/costo.  
>  **/clear** entre tareas no relacionadas.  
>  Mover instrucciones largas de CLAUDE.md a skills (se cargan bajo demanda).

### GitHub Actions — Claude como Revisor de PRs

`.github/workflows/claude-review.yml`

```yaml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]
  issue_comment:
    types: [created]

jobs:
  claude-review:
    if: |
      (github.event_name == 'pull_request') ||
      (github.event_name == 'issue_comment' &&
       contains(github.event.comment.body, '@claude'))
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      issues: write
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          model: sonnet
```

Con esta configuración, puedes escribir `@claude revisa este código` en cualquier PR y Claude lo revisará automáticamente según las reglas del `CLAUDE.md` del repo.

---

## 9. Agent Teams — Equipos de Agentes en Paralelo

_Duración: 8 minutos_

**Objetivos**

- Entender cuándo usar Agent Teams vs Subagentes
- Configurar y lanzar un equipo de agentes
- Controlar, comunicar y coordinar teammates
- Aplicar mejores prácticas en proyectos reales

> **¿Qué son los Agent Teams?**
>
> **Agent Teams** te permiten orquestar **múltiples sesiones de Claude Code trabajando en paralelo** como un equipo coordinado. Un agente *líder* gestiona a varios *teammates*, cada uno con su propio contexto y tareas asignadas, comunicándose entre sí mediante mensajes.

### ¿Cuándo Usar Agent Teams?

Agent Teams es ideal para tareas que se benefician del **trabajo paralelo y coordinado**:

- **Investigación y Revisión:** Múltiples teammates investigan diferentes aspectos de un problema simultáneamente, comparten hallazgos y desafían las conclusiones del otro.
- **Nuevos Módulos o Features:** Cada teammate trabaja en una pieza independiente sin interferir con los demás. Ej: frontend, backend y tests en paralelo.
- **Debugging con Hipótesis Competidoras:** Teammates prueban diferentes teorías en paralelo y convergen en la respuesta más rápido.
- **Coordinación Cross-Layer:** Cambios que abarcan frontend, backend y tests — cada uno gestionado por un teammate diferente.

### Agent Teams vs Subagentes

| Característica | Subagentes | Agent Teams |
|---|---|---|
| **Contexto** | Comparten la sesión del padre | Cada uno tiene su propia ventana de contexto |
| **Comunicación** | Retornan resultado al padre | Mensajes bidireccionales entre teammates |
| **Paralelismo** | Limitado (dentro de la sesión) | Verdadero paralelismo (sesiones separadas) |
| **Mejor para** | Tareas ligeras de investigación o verificación | Trabajo complejo que requiere coordinación |
| **Costos** | Compartidos con la sesión principal | Cada teammate consume tokens independientemente |

### Habilitar Agent Teams

Agent Teams es una feature experimental. Actívala con la variable de entorno o en settings:

`Variable de
                entorno`

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

`.claude/settings.json`

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### Lanzar tu Primer Equipo

Simplemente describe la tarea y pide un equipo. Claude creará los teammates automáticamente:

**💬 Prompt — Agent Team para el CRM**

```text
Necesito agregar un módulo de reportes al CRM. Crea un agent team con: un teammate
              para diseñar el esquema de datos en Prisma, otro para crear las rutas de API en Next.js, y otro para
              construir los componentes de UI con Tailwind. Coordinen entre sí para asegurar consistencia.
```

> **¿Qué pasa al crear un equipo?**
>
> Claude analiza tu petición, crea una **lista de tareas compartida**, y spawna los teammates necesarios. Cada teammate recibe su contexto inicial y comienza a trabajar de forma autónoma. El líder coordina y tú puedes monitorear el progreso.

### Controlar tu Equipo

#### Modos de Visualización

- **In-Process:** Todos los teammates corren dentro de tu terminal principal. Usa Shift+Down para navegar entre ellos. Funciona en cualquier terminal.
- **Split Panes (tmux):** Cada teammate tiene su propio panel. Ves el output de todos simultáneamente. Requiere `tmux` o iTerm2.

`.claude/settings.json`

```json
// Forzar modo in-process (sin tmux)
{ "teammateMode": "in-process" }

// O desde la línea de comandos:
// claude --teammate-mode in-process
```

#### Comunicarte con Teammates

| Acción | In-Process | Split Panes |
|---|---|---|
| **Navegar teammates** | Shift+Down para ciclar | Click en el panel del teammate |
| **Enviar mensaje** | Navega al teammate y escribe | Click en su panel y escribe |
| **Ver sesión** | Enter para ver, Escape para interrumpir | Visible directamente en el panel |
| **Ver lista de tareas** | Ctrl+T | Ctrl+T |

#### Especificar Modelos y Pedir Aprobación

**💬 Prompt — Equipo con modelos específicos**

```text
Crea un equipo con 3 teammates para refactorizar estos módulos en paralelo. Usa
              Sonnet para cada teammate.
```

**💬 Prompt — Requerir aprobación de plan**

```text
Crea un teammate arquitecto para refactorizar el módulo de autenticación. Requiere
              aprobación del plan antes de hacer cambios.
```

#### Gestionar el Equipo

`Comandos de
                gestión`

```bash
# Pedir a un teammate que se detenga
"Pide al teammate investigador que se detenga"

# Limpiar el equipo completo cuando termines
"Limpia el equipo"

# Esperar a que terminen antes de continuar
"Espera a que tus teammates completen sus tareas antes de continuar"
```

### Hooks para Quality Gates

Usa hooks específicos de Agent Teams para controlar calidad:

| Hook | Cuándo se ejecuta | Uso |
|---|---|---|
| `TeammateIdle` | Cuando un teammate va a quedar inactivo | Exit code 2 → envía feedback y mantiene al teammate trabajando |
| `TaskCompleted` | Cuando una tarea se marca como completada | Exit code 2 → previene completar y envía feedback |

### Arquitectura Interna

> **¿Cómo funciona por dentro?**
>
> El **Lead** (tu sesión principal) spawna teammates como sesiones independientes de Claude Code. Todos comparten un directorio de configuración del equipo y una lista de tareas compartida:
>
> - `~/.claude/teams/{team-name}/config.json` — Configuración del equipo
> - `~/.claude/tasks/{team-name}/` — Lista de tareas compartida

#### Comunicación y Contexto

- 📨 **Entrega automática** — Cuando un teammate envía un mensaje, se entrega automáticamente al destinatario
- 🔔 **Notificaciones idle** — Cuando un teammate termina, notifica automáticamente al líder
- 📋 **Lista de tareas compartida** — Todos los agentes ven el estado y pueden reclamar trabajo disponible
- 💬 **message** — Envía a un teammate específico
- 📣 **broadcast** — Envía a todos simultáneamente (usar con moderación por costos)

### Casos de Uso Prácticos

#### Code Review en Paralelo

**💬 Prompt — Review paralelo del CRM**

```text
Crea un agent team para revisar el PR #42 del CRM. Spawn tres revisores:
              - Uno enfocado en implicaciones de seguridad de las rutas de API
              - Uno verificando impacto en rendimiento de las queries de Prisma
              - Uno validando cobertura de tests
              Que cada uno revise y reporte sus hallazgos.
```

#### Debugging con Hipótesis Competidoras

**💬 Prompt — Debugging paralelo**

```text
Los usuarios reportan que el dashboard del CRM tarda más de 10 segundos en cargar.
              Spawn 3 teammates para investigar diferentes hipótesis:
              - Teammate 1: queries N+1 en Prisma y problemas de índices
              - Teammate 2: renderizado excesivo en componentes React
              - Teammate 3: tamaño del bundle y carga de assets
              Que hablen entre sí para refutar las teorías del otro y converjan en un diagnóstico.
```

#### Feature Cross-Layer para el CRM

**💬 Prompt — Feature completa con team**

```text
Necesito agregar un módulo de "Notas" al CRM (cada cliente puede tener múltiples
              notas). Crea un agent team:
              - Teammate "schema": agrega el modelo Note en Prisma y corre la migración
              - Teammate "api": crea las rutas CRUD en src/app/api/notes/
              - Teammate "ui": crea el componente NotesPanel en src/components/
              - Teammate "tests": escribe tests para la API y componentes
              Coordinen para usar los mismos nombres de campos y tipos.
```

### Mejores Prácticas

- **Da Contexto Suficiente:** Los teammates no comparten la ventana de contexto del líder. Al spawnearlos, incluye paths relevantes, convenciones del proyecto y reglas.
- **Tamaño Apropiado del Equipo:** 2-5 teammates es ideal. Los costos escalan linealmente y la coordinación se complica con equipos grandes. Más no siempre es mejor.
- **Dimensiona las Tareas:** Ni muy pequeñas (el overhead no vale la pena) ni muy grandes (mucho tiempo sin check-ins). Unidades autocontenidas: una función, un archivo de test, una review.
- **Evita Conflictos de Archivos:** Asigna archivos diferentes a cada teammate. Si dos teammates editan el mismo archivo, habrá conflictos de merge.
- **Empieza con Research:** Los equipos funcionan mejor cuando empiezan con investigación y luego pasan a implementación, en lugar de saltar directo a escribir código.
- **Monitorea y Dirige:** Revisa el progreso periódicamente. Envía mensajes para redirigir si algún teammate va por mal camino.

### Troubleshooting

| Problema | Solución |
|---|---|
| **Teammates no aparecen** | En in-process, presiona Shift+Down. Si usas split panes, verifica que `tmux` esté instalado: `which tmux` |
| **Demasiados prompts de permisos** | Configura permisos adecuados en `.claude/settings.json` con reglas de `allow` |
| **Teammates se detienen por errores** | Envíales instrucciones adicionales directamente, o spawna un reemplazo |
| **El líder termina antes que el equipo** | Pide al líder: *"Espera a que tus teammates completen sus tareas"* |
| **Sesiones tmux huérfanas** | `tmux ls` para listar y `tmux kill-session -t <nombre>` para limpiar |

### Limitaciones Actuales

> **Ten en cuenta**
>
> - **Sin `/resume` para teammates in-process** — Al reanudar sesión, los teammates ya no existen. Spawna nuevos si es necesario.
> - **Un equipo por sesión** — Limpia el equipo actual antes de crear uno nuevo.
> - **Sin equipos anidados** — Solo el líder puede crear y gestionar el equipo.
> - **Líder fijo** — La sesión que crea el equipo es el líder de por vida.
> - **Permisos al spawn** — Todos los teammates inician con el modo de permisos del líder.
> - **Split panes requiere tmux/iTerm2** — No soportado en terminal integrada de VS Code ni Windows Terminal.

> **Próximos Pasos**
>
> Ahora que conoces Agent Teams, combínalos con **Subagentes** para delegación ligera, **Git Worktrees** para sesiones paralelas manuales, y **Hooks** para quality gates automáticos. Documentación completa: [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams)
