# Fase: validar (`/sdd-validate NNN-nombre`)

**Objetivo:** demostrar, requisito por requisito, que la feature cumple la spec, con evidencia que
otra persona pueda comprobar. Después, cerrarla: push, PR, merge y verificación en `main`.
Requisito: todas las tareas `[x]`. Si no, **PARÁ** y decí cuáles faltan.

**Revisión independiente (opcional):** si el usuario lo pide, delegá la revisión al subagente
`sdd-reviewer` (no participó de la implementación y no puede modificar archivos), y escribí el
`validate.md` con lo que reporte.

## 1. Juntar la evidencia (ahora, no de memoria)

- Leé spec, clarify, plan, tasks, `MEMORY.md` y el historial de la rama.
- **Corré todos los gates ahora** y copiá los números.
- Recorré la spec **RF por RF, RNF por RNF y caso por caso**: qué test lo cubre (archivo y nombre
  del test) o qué evidencia lo demuestra (corrida de CI, medición, prueba con la app real).
  **Un RF sin test que lo cubra no está implementado.**
- Chequeos que los tests no hacen solos: buscá con `grep` lo que la spec o la constitución
  prohíben (por ejemplo, `sleep` en tests de concurrencia, secretos en el repo, `console.log`,
  valores concatenados en SQL); revisá la constitución principio por principio; listá los tests
  que se **modificaron** (nunca borrados) y por qué.
- Si hay consigna: tabla "exigencia → evidencia", con las adaptaciones declaradas.

## 2. Si aparece un hallazgo

**No lo arregles por tu cuenta.** Explicá el problema en simple y proponé:
- una tarea nueva `Tn+1` en `tasks.md` (con su "Hecho cuando" y su test en rojo), y
- si hace falta, la precisión del requisito en la spec ("Corrección/Precisión tras la validación").

Con la aprobación: commit de la tarea nueva, implementala con `fases/implement.md` y volvé a
validar. En el validate queda arriba de todo: "Hallazgo durante la validación (ya corregido)".

## 3. Escribir `validate.md`

Plantilla `docs/plantillas/validate.md`. La **primera línea** es el veredicto:
`VEREDICTO: APROBADO` o `VEREDICTO: CAMBIOS NECESARIOS`. Si algo solo se puede confirmar después del
merge (una publicación o un despliegue que corre en `main`), decilo en la misma línea.
Observaciones al final: desvíos, correcciones registradas, deuda, pendientes para otra feature.

Commit `NNN: validate — VEREDICTO: …` y `MEMORY.md` (la feature pasa a "validada").

## 4. Cerrar la feature

1. Pedí **OK para el push** de la rama (salvo autorización vigente para esa rama).
2. El usuario abre el PR (dale el link `…/pull/new/NNN-nombre` y una descripción lista para pegar).
3. Seguí las corridas de CI del PR hasta el verde. Si fallan, investigá con los logs.
4. El usuario mergea. Sincronizá `main` local (`git fetch` + `git merge --ff-only origin/main`).
5. **Verificá la corrida en `main`** (lo que solo corre ahí: publicaciones, despliegues).
6. Si algo de lo validado cambió o se confirmó después del merge, actualizá el `validate.md` con una
   nota "Actualización tras el merge" (si `main` está protegido, por un PR chico).
7. `MEMORY.md` y archivo de tarea: feature **cerrada**.
8. **Proponé una sesión nueva** para lo próximo. Una sesión larga arrastra todo su contexto en cada
   turno y, si se resume sola, pierde detalles; la feature cerrada ya está documentada.
   Preguntá: **"La NNN quedó cerrada. ¿Abrimos una sesión nueva para seguir con la próxima spec?"**
   Si acepta, escribí el **pase** en `.claude/tareas/siguiente.md`:

   ```markdown
   # Pase a la próxima sesión (<fecha>)

   ## Se cerró
   NNN-nombre: <una línea>. PR #<n> mergeado; corrida de main <verde / link>.

   ## Siguiente
   <una de estas: "Feature NNN+1-nombre: <idea>" · "Elegir la próxima feature (ideas
   anotadas: …)" · "Retomar NNN-nombre en Tn" · "<pendiente concreto>">

   ## Para tener en cuenta
   - <decisiones, pendientes u observaciones del validate que la próxima feature necesita>
   ```

   Y decile exactamente: **"Escribí `/clear` y después `/sdd-siguiente`."** (`/clear` empieza una
   conversación limpia en la misma ventana; una conversación nueva también sirve.) Si no acepta,
   seguí en la misma sesión.
