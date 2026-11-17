# Fase: cambiar un requisito (`/sdd-change NNN-nombre "qué cambia"`)

**Objetivo:** que un requisito nuevo o modificado pase **primero por la spec y después por el
código**, sin perder la historia. Sirve también para los hallazgos de la validación.

## 1. Entender el impacto

- ¿En qué fase está la feature? (spec, plan, implementación a medias, validada, cerrada).
- ¿Qué RF, casos, decisiones del plan y tareas toca el cambio? ¿Hay código ya hecho y commiteado
  que se ve afectado?
- **Si el cambio es grande** (nuevos usuarios, nuevo flujo, más de ~3 tareas), recomendá una
  **feature nueva** con su propia spec en lugar de estirar esta.

## 2. Proponer y PARAR

Explicá en simple:
- el cambio en la spec (RF nuevo o reescrito, casos nuevos), marcado como
  `*Cambio <fecha>:* …`;
- el impacto en plan y tareas: tarea nueva `Tn+1`, o una tarea ya hecha que vuelve a `[ ]` con
  una nota;
- qué tests existentes cambian, y por qué no se debilitan.

Cerrá con: **"¿Aprobás el cambio?"**

## 3. Aplicar (con la aprobación)

- Spec actualizada con la nota de cambio. Si el cambio afecta algo ya revisado, hacé una
  **mini-ronda de clarify** sobre lo nuevo (detecciones → resoluciones → ronda 2).
- Plan y `tasks.md` actualizados. **Nunca se borran tareas cerradas ni su historia.**
- Commit `NNN: cambio aprobado — <resumen>`.
- Seguí con `fases/implement.md` para las tareas nuevas o reabiertas.
