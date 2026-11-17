# Fase: estado (`/sdd-status`)

**Objetivo:** responder "¿dónde estamos y qué sigue?" **sin modificar nada**.

## Juntar

- `docs/constitution.md`: ¿tiene `[COMPLETAR]`? → el proyecto no está configurado (`/sdd-init`).
- Para cada carpeta de `specs/`: qué artefactos existen y su línea de `Estado`, cuántas tareas
  `[x]` / `[ ]` hay en `tasks.md` y el veredicto de `validate.md` si existe.
- `.claude/tareas/*.md`: la sección "Estado" y el "Siguiente" de cada uno.
- `git status`, rama actual, `git log --oneline -5` y si hay commits sin pushear
  (`git status -sb`).
- `MEMORY.md`: la sección "Estado de las features".

## Responder

Una tabla por feature: `NNN-nombre | fase actual | tareas x/n | último commit | qué sigue`, y
abajo:
- **Qué sigue**, concreto (el comando o la frase para continuar).
- Cambios sin commitear o commits sin pushear, si los hay.
- Inconsistencias encontradas (por ejemplo, una tarea `[x]` sin su commit, o un archivo de tarea
  que dice otra cosa que `tasks.md`): se informan, no se corrigen solas.
