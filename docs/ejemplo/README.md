# Ejemplo completo: `001-gastos`

Una feature chica hecha de punta a punta con el flujo, para ver **cómo quedan los artefactos de
verdad**. Es ficticia (los hashes de commit son ilustrativos), pero los problemas que aparecen son
los típicos y están verificados.

**Leelos en este orden:**

1. [`spec.md`](001-gastos/spec.md): qué y por qué. Fijate en los requisitos EARS y los casos
   límite numerados.
2. [`clarify.md`](001-gastos/clarify.md): la ronda 1 encontró 7 problemas en una spec que parecía
   completa; la ronda 2 verifica que quedaron resueltos.
3. [`plan.md`](001-gastos/plan.md): mirá los **hallazgos verificados**: comprobar el
   comportamiento de los números antes de diseñar evitó un bug con montos como `0.29`.
4. [`tasks.md`](001-gastos/tasks.md): 4 tareas planificadas y una quinta agregada por la
   validación.
5. [`validate.md`](001-gastos/validate.md): la tabla requisito → test y un **hallazgo** (la zona
   horaria) que se convirtió en tarea nueva en vez de arreglarse en silencio.

El stack del ejemplo (Node + TypeScript + Express + Vitest) es solo para que el plan sea concreto:
el flujo es el mismo con cualquier lenguaje.
