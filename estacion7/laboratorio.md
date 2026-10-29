# Laboratorio: del plan al primer merge

Guía para usar durante la clase. Todo se hace en **tu repo**, sobre **tu rebanada**.

## Paso 1 · Elige la tarea

Toma la primera tarea de tu plan: la que crea el proyecto y deja un test corriendo. Si no tienes plan, usa la [vía rápida](#vía-rápida).

Crea una rama con el ID de la tarea:

```bash
git checkout -b task-001
```

## Paso 2 · Pide la implementación

En Claude Code, dentro de tu repo:

```text
Toma únicamente la tarea docs/tasks/001-*.md. Léela completa, junto con
docs/spec-rebanada.md. Antes de escribir código, resume en 5 líneas el
alcance y el plan. Después implementa el cambio mínimo, escribe el test
del criterio de aceptación y córrelo. Entrégame: archivos cambiados,
comando de test y resultado. No toques nada fuera del alcance de la tarea.
```

Lee el resumen antes de dejarlo avanzar. Si se va fuera del alcance, detenlo y corrígelo.

## Paso 3 · Comprueba tú

- Corre el test tú mismo. Debe pasar.
- Revisa el diff: `git diff`. ¿Hay algo que no pediste?
- Si falla, pásale al agente el error exacto y vuelve a correr.

## Paso 4 · Abre el PR

```bash
git add -A
git commit -m "TASK-001: <qué hace>"
git push -u origin task-001
gh pr create --fill
```

En la descripción del PR pon: el criterio de aceptación que cumple, el comando de test y el resultado.

## Paso 5 · Review cruzado en parejas

Revisa el PR de tu pareja con esta lista:

- [ ] El cambio hace lo que dice la tarea y nada más.
- [ ] Hay un test del criterio de aceptación y pasa.
- [ ] No hay secretos, claves ni archivos generados de más.
- [ ] Entiendes el código lo suficiente para explicarlo.

Deja al menos un comentario. Si todo está bien, aprueba.

## Paso 6 · Merge y roadmap

Haz merge del PR aprobado y pega su enlace en tu celda de E7 del roadmap del grupo.

## Vía rápida

Si llegaste sin spec o sin plan, haz esto en los primeros 30 minutos:

1. Copia [la plantilla de spec mínima](../plantillas/spec-minima.md) a `docs/spec-rebanada.md` y complétala en 10 minutos.
2. Pide a Claude Code que la evalúe con `evaluate-spec` y corrige el vacío más grave.
3. Pide el plan con este prompt:

```text
Con docs/spec-rebanada.md, propón entre 3 y 6 tareas para implementar esta
rebanada. Escríbelas en docs/tasks/ con alcance, criterio de aceptación y
plan de pruebas. La primera crea el proyecto con un test que corra.
```

Después sigue desde el paso 1.

## Si te trabas

Levanta la mano o escribe en el chat con: qué paso, qué comando y el error exacto. Un bloqueo de más de 10 minutos se resuelve con el instructor o el tutor, no solo.
