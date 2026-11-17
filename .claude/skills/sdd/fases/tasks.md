# Fase: tareas (`/sdd-tasks NNN-nombre`)

**Objetivo:** partir el plan en tareas chicas, ordenadas y verificables. Requisito: plan escrito
(y aprobado, si se aprueba por separado).

## Reglas de las tareas

Plantilla `docs/plantillas/tasks.md`.

- **Máximo 10.** Si no entran, la feature es demasiado grande: proponé partir la spec.
- **15–45 minutos** cada una, en **orden de dependencia**.
- Cada tarea dice qué RF, RNF y casos límite cubre. **Todos** los RF y casos de la spec tienen que
  quedar cubiertos por alguna tarea: verificalo antes de presentar.
- **"Hecho cuando:"** con un **comando concreto** y lo que tiene que mostrar ("`<comando de
  tests>` pasa `archivo.test` con los casos 3, 4 y 7").
- Los tests van **en la misma tarea** que el código que prueban (escritos antes).
- Si una tarea **solo se puede verificar en CI** (por ejemplo, no hay Docker local), decilo en su
  "Hecho cuando" y avisá que va a necesitar push de la rama.
- Estimación y dependencias en cada una.

## Presentar y PARAR

Lista corta de las tareas con lo que cubre cada una. Si se presenta junto con el plan, una sola
pregunta para los dos. Con la aprobación: `Estado: **aprobado**`, commit
`NNN: plan y tareas aprobados`, archivo de tarea.
