# Antes de la estación 7: tu rebanada lista

**Clase:** lunes 2 de noviembre, 7 PM Bogotá. **Tiempo estimado:** de 2 a 3 horas durante el fin de semana. **Ayuda:** tutoría del jueves 29 de octubre y el grupo de la cohorte.

El lunes vamos a implementar y mergear en clase la primera tarea de tu producto. Para eso necesitas llegar con una rebanada definida, evaluada y planificada.

**Una rebanada** es un solo flujo de tu producto que un usuario puede completar de punta a punta. Por ejemplo: «el reclutador crea una campaña y recibe el link para compartir». Es lo que vas a implementar, probar, desplegar y mostrar en el Demo Day. Chica y funcionando vale más que grande y a medias.

## 1. Elige tu rebanada (15 minutos)

Mira tu Vision Board o tu PRD y elige el flujo que mejor muestra el valor de tu producto. Si dudas entre dos, elige el más chico.

## 2. Escribe la spec mínima (30 minutos)

Copia la [plantilla de spec mínima](../plantillas/spec-minima.md) a tu repo como `docs/spec-rebanada.md` y complétala: problema, usuario, flujo paso a paso, 3 criterios de aceptación, qué queda fuera y decisiones técnicas tomadas.

Si ya tienes artefactos de Inception o Construction, enlázalos. Si no los tienes, esta página alcanza.

## 3. Evalúala (20 minutos)

En tu repo, pide a Claude Code:

```text
Lee y sigue skills/evaluate-spec/SKILL.md del repo del curso:
https://github.com/30x-llc/ai-for-devs-cohorte-5. Evalúa docs/spec-rebanada.md como una sola
ola. Escribe docs/spec-review.md con la nota A–D y los vacíos más graves.
No modifiques la spec.
```

Corrige los vacíos y vuelve a evaluar hasta llegar a **A o B**.

## 4. Deja listo tu entorno (30 minutos)

- **Repo:** creado en GitHub, clonado en tu máquina y con `docs/spec-rebanada.md` subido.
- **Claude Code:** instalado y autenticado. Comprueba con `claude --version`, abre `claude` dentro del repo y pídele que te diga qué contiene, sin modificar nada.
- **Git:** `git status` funciona dentro del repo.
- **Linear:** puedes entrar al espacio del curso y crear una tarea. No hace falta configurar API ni MCP.

Si después de 10 minutos un paso de instalación sigue fallando, anota el sistema operativo, el comando y el error exacto, y llévalo a la tutoría del jueves o al grupo.

## 5. Genera el plan (30 minutos)

Pide a Claude Code:

```text
Lee y sigue skills/create-implementation-plan/SKILL.md del mismo
repo. Prepara el plan SOLO para la rebanada de docs/spec-rebanada.md:
entre 3 y 6 tareas. La primera debe dejar el proyecto creado con un test
que corra. Cada tarea con alcance, criterios de aceptación y plan de pruebas.
No implementes nada todavía.
```

Revisa que la primera tarea se pueda hacer en una hora y que las tareas no tengan ciclos. Si puedes, súbelas a Linear; si no, el lunes lo hacemos juntos.

## Llega con esto

- [ ] `docs/spec-rebanada.md` en tu repo.
- [ ] `docs/spec-review.md` con nota A o B.
- [ ] Plan de 3 a 6 tareas en `docs/tasks/` (y en Linear si pudiste).
- [ ] Claude Code abriendo tu repo.
- [ ] Una pregunta concreta o un bloqueo escrito.

## Qué vamos a hacer el lunes

| Min | Bloque |
|---|---|
| 0–10 | Dónde estamos y el plan hasta el Demo Day |
| 10–40 | Vía rápida para quien no trajo spec o plan |
| 40–50 | Demo en vivo: una tarea pasa por agente, PR, review y merge |
| 50–95 | Laboratorio: implementas tu primera tarea y abres el PR |
| 95–110 | Review cruzado en parejas y merge |
| 110–120 | Enlace del PR al roadmap y tarea para E8 |

Si llegas con todo listo, usas el bloque de vía rápida para empezar a implementar antes.
