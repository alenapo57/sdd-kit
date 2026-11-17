# Tareas 001 — Registro de gastos personales

Estado: **aprobado** (11/03, Ana) · Plan: `specs/001-gastos/plan.md`

Reglas: una tarea por vez, tests primero (en rojo), gates en verde, un commit `001-Tn: …` en la
rama `001-gastos`, `[x]` y `MEMORY.md`. Push con OK; entra a `main` por PR.

- [x] **T1. Registrar un gasto válido y formato de errores.** RF-1, RF-8
      `gasto.ts`, `gastos.repository.ts` (en centavos), `gastos.service.ts` (registrar),
      `gastos.router.ts` (POST), `errores.ts`.
      Hecho cuando: `npm test` pasa `gastos.e2e.test.ts` "registra un gasto válido → 201 con id" y
      "un error inesperado responde 500 con detalle".
      Estimación: 30 min · depende de: —

- [x] **T2. Validaciones de todos los campos.** RF-2, RF-3, RF-4, RF-9, RNF-3 · casos 1–10
      `validar-gasto.ts` con regex sobre `String(monto)` (hallazgo 1), `reloj.ts` con zona horaria.
      Hecho cuando: `npm test` pasa los unitarios de `validar-gasto` (casos 1–10) y un e2e por campo.
      Estimación: 40 min · depende de: T1

- [x] **T3. Listar por mes.** RF-5, RF-6 · casos 11 (lista), 12
      Hecho cuando: `npm test` pasa "lista del mes ordenada por fecha y registro", "mes sin gastos →
      []" y "mes inválido → 400 mes".
      Estimación: 25 min · depende de: T1

- [x] **T4. Resumen por categoría y rendimiento.** RF-7, RNF-1, RNF-2 · caso 11 (resumen)
      Hecho cuando: `npm test` pasa "0,10 + 0,20 + 0,29 = 0,59 exacto", "las 5 categorías aparecen,
      con 0 si no hay gastos" y "10.000 gastos → resumen en < 500 ms".
      Estimación: 30 min · depende de: T3

- [x] **T5. "Hoy" en la zona configurada (hallazgo de la validación).** RF-4, RNF-3 · casos 7, 8
      La validación encontró que el chequeo de fecha futura comparaba con la fecha en UTC: los tests
      fijaban el reloj al mediodía y no lo veían. Se compara con la fecha en la zona configurada y
      se agregan tests cerca de la medianoche. Agregada con aprobación de Ana (12/03).
      Hecho cuando: `npm test` pasa "23:30 en Buenos Aires: un gasto de hoy es válido" y "23:30 en
      Buenos Aires: un gasto de mañana se rechaza" (los dos en rojo antes del cambio).
      Estimación: 20 min · depende de: T2

Después de T5: `validate.md`, push de la rama con OK, PR y merge del usuario.
