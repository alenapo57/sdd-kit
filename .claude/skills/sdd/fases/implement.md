# Fase: implementar UNA tarea (`/sdd-implement NNN-nombre Tn` o "seguí con Tn")

**Objetivo:** cerrar **una sola tarea** con los tests primero, los gates en verde y su commit.
**No empieces la siguiente**: al terminar, PARÁ.

## 0. Chequeos previos (si alguno falla, PARÁ y decilo)

- `tasks.md` está `aprobado`. Si no dijeron qué tarea, es la primera `[ ]` cuyas dependencias
  estén `[x]`.
- La tarea está `[ ]` y sus dependencias `[x]`.
- Estás en la rama de la feature y `git status` no tiene cambios ajenos a la tarea (si los hay,
  entendé qué son antes de seguir; nunca los descartes sin preguntar).

## 1. Leer

`tasks.md` (la tarea), `plan.md` (las decisiones que la afectan), `spec.md` (sus RF y casos),
`MEMORY.md` (lecciones) y **todo el código que vas a tocar**.

## 2. Tests primero, y verlos fallar

- Escribí los tests que prueban los RF y casos de la tarea.
- Corrélos y **verlos fallar por el motivo correcto**. "Cannot find module" sirve como rojo inicial,
  pero si la tarea modifica algo existente, el rojo tiene que ser el comportamiento, no un import.
- Si el test **caracteriza algo que ya existía** (pasa en verde de entrada), demostrá que sirve con
  una **mutación**: rompé el código a propósito, mirá que el test falle, revertí. Si una mutación
  no se detecta, analizá si la mutación era mala antes de concluir que el test es malo.
- Si no se pudo ver el rojo, registralo como **desvío** en `MEMORY.md`.

## 3. Código mínimo

Lo justo para que pasen los tests, siguiendo el plan y la constitución. No modifiques los tests
para que pasen; si un test estaba mal, corregilo **y explicá por qué**.

## 4. Gates

Corré **todos** los gates de `docs/constitution.md` y **copiá los números reales** (tests que
pasan, cobertura). Si un gate falla por algo que no es de esta tarea, no lo arregles por tu cuenta:
avisá.

## 5. Verificación extra (cuando corresponda)

- La tarea toca arranque, configuración, logging o integración → probala con **la app real**
  (levantala, hacele pedidos, mirá los logs). Para procesos en segundo plano, controlá solo el
  proceso que lanzaste (nunca mates procesos por nombre).
- Si una suposición del plan o la spec resultó falsa → **"Corrección en Tn: …"** en el documento.

## 6. Cerrar

1. `[x]` en `tasks.md`.
2. Entrada en `MEMORY.md`: qué se hizo, decisiones y porqué, trampas, números de los gates.
3. Archivo de tarea: commit y "Siguiente".
4. Commit `NNN-Tn: <descripción>` con un mensaje que explique qué y por qué y los gates. Revisá
   `git show --stat HEAD`: solo tienen que entrar los archivos de la tarea.

**Tareas que solo se verifican en CI:** commit **sin** `[x]` (se dice en el mensaje), push con OK,
seguí la corrida y recién con el verde hacé el commit de cierre con `[x]` y el link de la corrida.

## 7. Reportar y PARAR

En simple: qué se hizo, las decisiones importantes y su porqué, qué no se pudo verificar, los
gates. Cerrá con **"¿Sigo con Tn+1?"**.
