# Spec review — EntreVista AI

Ejemplo de salida de la skill [`evaluate-spec`](../../skills/evaluate-spec/SKILL.md) sobre los artefactos de [estación 5](https://github.com/hardcore-ai/c4/tree/main/estacion5/agentic_interviewer_ai). Se generó el 26 de octubre de 2026 y sirve como referencia para el módulo 2.

Rutas relativas a `estacion5/agentic_interviewer_ai/`.

## Nota por ola

| Ola | Nota | Por qué |
|---|---|---|
| **Unit 6 · auth-lambda** | **B** | Tiene diseño funcional, reglas, NFR, infraestructura y runner. Quedan supuestos menores sobre la política de contraseñas. |
| **Resto del MVP (units 1–5 y 7)** | **C** | Solo tienen Inception. Falta el diseño funcional con reglas de negocio por unidad, y hay una dependencia técnica a verificar. |

**Recomendación:** planificar primero la ola de auth-lambda. Las demás unidades vuelven a Construction (E5) antes de entrar al plan.

## Lente 1 · Producto

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| P1 (*) Problema y usuario | Cumple | `PRD_agentic_interviewer_ai.md` §2 y §3; `aidlc-docs/inception/user-stories/personas.md` | |
| P2 (*) Alcance | Cumple | `PRD…` §8 (MoSCoW); `requirements.md` FR-01 a FR-10 | |
| P3 (*) Fuera de alcance | Cumple | `requirements.md` «Out of Scope (MVP)»; `PRD…` §8 Won't Have | |
| P4 (*) Reglas de negocio | Cumple en auth-lambda · Falta en el resto | `construction/auth-lambda/functional-design/business-rules.md`. Las demás unidades no tienen `business-rules.md`. | Correr functional design de E5 por unidad antes de planificarla. |
| P5 Criterio de éxito | Cumple | `PRD…` §10 y NFR-07 | |
| P6 Decisiones pendientes | Parcial | `PRD…` líneas 40 y 43 dejan ROI y stack como TBD, aunque el stack ya se decidió en `requirements.md`. Línea 179: la adopción de Telegram queda como riesgo sin validar. | Cerrar los TBD resueltos y registrar la validación de Telegram como tarea. |

## Lente 2 · TDD

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| T1 (*) Aceptación observable | Cumple | `user-stories/stories.md`: 34 historias con criterios Gherkin | |
| T2 (*) Casos de prueba | Parcial | Gherkin cubre el camino feliz y algunos errores. Faltan bordes en la política de contraseñas (Unicode, espacios). | Agregar los casos en la tarea; ver `estacion6/ejemplo-y-prompts.md` §2. |
| T3 Runner y comando | Parcial | `construction/plans/auth-lambda-code-generation-plan.md:315` declara `pytest` con `make test`. Las demás unidades no declaran runner. No hay código todavía. | Primera tarea de cada ola: runner mínimo que ejecute una prueba. |
| T4 Declaración TDD | N/A | No se declaró TDD. `requirements.md` NFR-06.5 pide tests unitarios e integración, no TDD. | |

## Lente 3 · Diseño

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| D1 (*) Arquitectura | Parcial | `application-design/components.md`, `services.md` y `unit-of-work.md` la definen. Pero `components.md:295` y `:307` usan una «Anthropic embeddings API»: **verificar**, porque Anthropic no publica una API de embeddings propia en su documentación. | Elegir el proveedor de embeddings y actualizar FR-06.3 y el diagrama de dependencias. |
| D2 Contratos | Parcial | `application-design/component-methods.md` define rutas y métodos (por ejemplo `POST /api/v1/messages`). Faltan esquemas de request y response. | Esquemas por endpoint en el diseño funcional de cada unidad. |
| D3 design.md | Falta | El dashboard (Unit 7) no tiene sistema visual, componentes ni estados de pantalla. | Crear `design.md` con la plantilla antes de planificar Unit 7. |
| D4 Restricciones | Cumple | `requirements.md` NFR-01 a NFR-07 y Security Baseline habilitada | |

## Coherencia entre artefactos

| Hallazgo | Evidencia |
|---|---|
| Las fechas no son coherentes | El PRD dice 2026-06-11 en su encabezado; `requirements.md` cita el PRD con fecha 2026-09-01; `business-rules.md` de auth-lambda dice 2026-03-10, antes del PRD. |
| El estado de AIDLC está desactualizado | `aidlc-state.md` dice «INCEPTION - Units Generation Complete» y Construction sin empezar, pero existe el diseño completo de auth-lambda. |
| Ruta de trabajo de otra máquina | `aidlc-state.md` apunta a una carpeta en la computadora de otra persona. |

Ninguno bloquea la ola de auth-lambda, pero confunden a un agente que lee el repo sin este contexto.

## Los tres vacíos más graves

1. **Reglas de negocio solo para una unidad de siete.** Sin ellas, el plan de las otras seis inventaría reglas.
2. **Proveedor de embeddings sin verificar.** Bloquea el RAG de Unit 4 y todo lo que depende de él.
3. **Sin `design.md` para el dashboard.** Unit 7 se implementaría con criterios visuales inventados.

## Supuestos propuestos (pendientes de aprobación)

- auth-lambda: la longitud mínima de contraseña cuenta caracteres Unicode, no bytes.
- auth-lambda: los espacios al inicio y al final de la contraseña se conservan.
- Todas las olas: si una unidad no declara runner, se usa el del plan de auth-lambda (`pytest`) para Python y se decide para Node.js y React.
