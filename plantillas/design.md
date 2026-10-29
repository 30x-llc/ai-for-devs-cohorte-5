---
name: <nombre del producto>
colors:
  primary: "#______"
  text: "#______"
  background: "#______"
typography:
  heading: ______
  body: ______
spacing:
  base: __px
radius:
  base: __px
---

# design.md

El bloque de arriba (frontmatter) guarda los tokens de diseño en un formato que
el agente puede leer y comparar. El resto del archivo explica el porqué. Es la
misma idea de DESIGN.md que usan las skills de diseño del curso.

Plantilla para el repositorio de tu proyecto. Cópiala como `design.md` en la
raíz y complétala. El agente la lee antes de cualquier tarea que toque la
interfaz o los contratos entre componentes.

La spec dice **qué** debe hacer el producto. Este archivo dice **cómo se ve y
cómo se construye**. Si una decisión ya está aquí, ninguna tarea la repite: la
enlaza.

## 1. Principios

- Tres o cuatro frases sobre la experiencia que buscamos.
- Ejemplo: «Primero el dato que el reclutador necesita para decidir».

## 2. Sistema visual

Explica cuándo se usa cada token del frontmatter. Ejemplo: «`primary` solo
para la acción principal de cada pantalla».

Si usas una librería de componentes, nómbrala con su versión.

## 3. Componentes y patrones

| Componente | Cuándo se usa | Cuándo no |
|---|---|---|
| ______ | ______ | ______ |

Estados obligatorios para cada pantalla: carga, vacío, error y éxito.

## 4. Arquitectura y contratos

- Componentes principales y cómo se comunican.
- Contratos que otras tareas consumen (endpoints, eventos, esquemas). Enlaza
  el archivo donde vive cada uno.

## 5. Decisiones

| Fecha | Decisión | Por qué | Enlace |
|---|---|---|---|
| ______ | ______ | ______ | ADR o issue |

## 6. Qué no hacer

- Lista corta de patrones prohibidos en este proyecto.

## 7. Cómo se verifica

- Cómo comprobar que una tarea respeta este diseño (capturas, tests visuales,
  revisión humana).
