# Instrucciones del proyecto — flujo SDD

> Claude Code lee este archivo al empezar cada sesión. Las secciones marcadas con
> `<!-- sdd-init -->` las completa el comando `/sdd-init` la primera vez que se usa el repo.
> Lo demás es la forma de trabajar: se puede ajustar, pero conviene entenderla antes
> (ver `docs/GUIA.md`).

## Antes de hacer cualquier cosa

1. Si existe un archivo de tarea en `.claude/tareas/` para lo que te piden, **leelo entero**:
   ahí está el estado de la última sesión.
2. Leé `MEMORY.md` (decisiones y lecciones del proyecto) y `docs/constitution.md` (las reglas
   que no se negocian).
3. Si `docs/constitution.md` todavía tiene `[COMPLETAR]`, el proyecto no está configurado:
   proponé correr `/sdd-init` antes de cualquier otra cosa.

## Idioma y forma de explicar

- Respondé en el idioma que fije la constitución (por defecto, español).
- **Explicá las decisiones importantes en simple**: quien usa este repo quiere entender el
  porqué, no solo recibir código. Una decisión sin su razón no sirve para aprender.
- Si cometiste un error o una suposición resultó falsa, decilo claramente y registrá la
  corrección (ver "Correcciones" más abajo). No lo escondas.

## Tamaño de la tarea: decidilo primero

- **Chica** (un archivo, cambio obvio, pregunta puntual): hacela directo, sin ceremonia.
- **Mediana o grande** (varios archivos, algo nuevo, no está claro cómo hacerlo): seguí el
  flujo SDD de abajo, con su archivo de tarea.
- Si no sabés en qué tamaño cae, preguntá.

## El flujo SDD: la spec manda

```
/sdd-init (una vez) → /sdd-spec ─PARA─ → /sdd-clarify ─PARA─ → /sdd-plan ─PARA─ → /sdd-tasks ─PARA─
                    → /sdd-implement T1 ─PARA─ … Tn ─PARA─ → /sdd-validate → PR → merge
```

- Cada fase deja un archivo en `specs/NNN-nombre/` y **para a esperar la aprobación
  explícita** del usuario. Nunca se pasa de fase sin un "sí, aprobado" (o equivalente claro).
- Los procedimientos completos de cada fase están en `.claude/skills/sdd/fases/`. Los comandos
  `/sdd-*` solo los invocan. Si el usuario pide lo mismo en lenguaje natural ("seguí con T3",
  "escribí el validate"), seguí **el mismo procedimiento**, igual que con el comando.
- Nada se implementa si no está en la spec aprobada. Un requisito nuevo o cambiado pasa primero
  por `/sdd-change`, después por el código.
- `/sdd-status` dice en qué fase está cada feature y qué sigue.

## Reglas

- **No modifiques código que no leíste.**
- **Si algo es ambiguo y cambia lo que vas a hacer, preguntá**, con la opción recomendada
  primero. Si tiene una respuesta razonable por defecto, elegila y avisá cuál elegiste.
- **Verificá de verdad**: corré los tests, el build o la app. Si no pudiste verificar algo,
  decilo explícitamente; nunca lo des por hecho.
- **Nada de cambios fuera de lo pedido.** Si ves otro problema, anotalo y avisá; no lo arregles
  por tu cuenta.
- **Nunca**: borrar o comentar un test para que la suite pase; marcar una tarea `[x]` con un gate
  en rojo; tocar `docs/constitution.md` sin acuerdo explícito; commitear `.env` u otros secretos;
  agregar una dependencia que el plan aprobado no incluya.

## Archivo de tarea (para retomar en otra sesión)

En tareas medianas o grandes llevá el estado en `.claude/tareas/<nombre-corto>.md` (plantilla en
`docs/plantillas/tarea.md`): pedido, decisiones, plan, progreso, commits y **qué sigue**.
Actualizalo al cerrar cada paso. Una sesión nueva tiene que poder seguir leyendo solo ese archivo,
sin volver a preguntar lo que ya se decidió. Es local (no se commitea): lo duradero va a `MEMORY.md`.

## MEMORY.md (lo que el proyecto aprendió)

Al cerrar cada tarea agregá una entrada corta: fecha, `NNN-Tn`, qué se hizo, **decisiones con su
porqué**, trampas encontradas y los números de los gates. Sirve para no repetir errores y para
que el revisor entienda el historial.

## Correcciones y hallazgos

- Si una suposición del plan o de la spec resulta falsa durante la implementación, corregí el
  documento con una nota **"Corrección en Tn: …"** (qué se creía, qué se comprobó). No se borra
  lo anterior en silencio.
- Si la validación encuentra que algo no cumple la spec, **no lo arregles por tu cuenta**:
  proponé una tarea nueva (`Tn+1`) con su test en rojo, y esperá la aprobación.

## Commits, ramas y push <!-- sdd-init -->

<!-- /sdd-init deja una de las dos opciones según lo que elija el usuario. -->
**Commits automáticos por fase: ACTIVADOS.** Dentro del flujo SDD estás autorizado a commitear
sin pedir permiso cada vez, en estos momentos:

- `NNN: spec aprobada y clarify` — tras la ronda 2 aprobada.
- `NNN: plan y tareas aprobados` — tras aprobar `tasks.md`.
- `NNN-Tn: <descripción>` — al cerrar cada tarea con los gates en verde, `[x]` y `MEMORY.md`.
- `NNN: validate — VEREDICTO: …` — al cerrar la validación.

Cómo:
- Escribí el mensaje en un archivo temporal y usá `git commit -F <archivo>` (evita problemas con
  acentos y comillas).
- Agregá solo los archivos de la tarea (`git add <rutas>`), nunca `git add -A` a ciegas.
- Nunca `.env`, ni `.claude/tareas/`, ni `.claude/settings.local.json`.

**Push: NO está autorizado de forma permanente.** Pedí OK cada vez (o una autorización explícita
para una rama y un período, que se anota en el archivo de tarea).

**Ramas**: cada feature trabaja en su rama `NNN-nombre` y entra a `main` por pull request, que
mergea el usuario (ver `docs/guia-github.md`). Después del merge, verificá la corrida de CI en
`main` antes de dar la feature por cerrada.

## Proyecto <!-- sdd-init -->

[COMPLETAR con /sdd-init: qué es el proyecto, stack, estructura de carpetas, convenciones.]

## Comandos y gates <!-- sdd-init -->

[COMPLETAR con /sdd-init: un comando por gate (lint, build, tests, …) y cómo levantar la app.]

## Trampas conocidas

<!-- Se agregan a medida que aparecen: problemas del entorno, del framework o de las
     herramientas que costaron tiempo y no deberían repetirse. -->
- (vacío por ahora)
