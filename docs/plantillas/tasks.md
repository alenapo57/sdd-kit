# Tareas NNN — <Título>

Estado: **borrador** · Plan: `specs/NNN-nombre/plan.md`

Reglas: una tarea por vez, tests primero (en rojo), gates en verde, un commit `NNN-Tn: …` en la
rama `NNN-nombre`, `[x]` y `MEMORY.md`. Push con OK; entra a `main` por PR.

<!--
  - Máximo 10 tareas (si no entran, la spec es demasiado grande: partila).
  - 15–45 minutos cada una, en orden de dependencia.
  - "Hecho cuando:" verificable con un COMANDO concreto y lo que tiene que mostrar.
-->

- [ ] **T1. <Título>.** RF-…, RNF-… · casos …
      <Qué se hace, en 1–3 líneas: archivos y piezas principales.>
      Hecho cuando: `<comando>` pasa <qué tests / qué resultado>.
      Estimación: <n> min · depende de: —

- [ ] **T2. <Título>.** RF-…
      <…>
      Hecho cuando: `<comando>` …
      Estimación: <n> min · depende de: T1

Después de Tn: `validate.md`, push de la rama con OK, PR y merge del usuario.
