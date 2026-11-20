# Fase: continuar en una sesión nueva (`/sdd-siguiente`)

**Objetivo:** que una sesión recién abierta (después de `/clear` o en una conversación nueva)
retome el trabajo **sin preguntar lo que ya se decidió y sin que el usuario copie nada**. Se apoya
en el "pase" que deja la sesión anterior al cerrar una feature (ver `fases/validate.md`, paso 8).

## 1. Leer el pase

- Leé `.claude/tareas/siguiente.md`. Si **no existe**, no inventes: seguí `fases/status.md`
  para mostrar dónde está cada feature y preguntá con qué seguir.
- Leé `MEMORY.md` y `docs/constitution.md`. Si el pase nombra archivos (la feature cerrada, una
  idea anotada, un pendiente), leelos también.
- Chequeá `git status` y la rama: si hay cambios sin commitear o la rama no es `main` actualizado,
  avisalo antes de arrancar.

## 2. Resumir en dos o tres líneas

Por ejemplo: *"Venimos de cerrar la 002 (login, PR mergeado). Lo próximo es la 003: exportar los
gastos a CSV. Quedó pendiente revisar el límite de filas."* Nada más: el detalle está en los
archivos.

## 3. Arrancar directo

Según lo que diga el pase en "Siguiente":

| El pase dice | Hacé |
|---|---|
| una feature nueva con número e idea | seguí `fases/spec.md` con esos datos (arranca la entrevista) |
| "elegir la próxima feature" | preguntá cuál, con 2–3 opciones si el pase o `MEMORY.md` mencionan ideas, la recomendada primero; después `fases/spec.md` |
| retomar una tarea a medias | seguí la fase correspondiente (por ejemplo `fases/implement.md` con esa `Tn`) |
| un pendiente concreto (un PR por mergear, una corrida por verificar) | resolvelo primero y después seguí con lo próximo |

## 4. Consumir el pase

Cuando el trabajo nuevo ya arrancó (por ejemplo, ya existe el archivo de tarea de la feature
nueva), **borrá `.claude/tareas/siguiente.md`**: el pase sirve una sola vez, y uno viejo confundiría
a una sesión futura. Desde ahí, el estado vive en el archivo de tarea de la feature nueva.
