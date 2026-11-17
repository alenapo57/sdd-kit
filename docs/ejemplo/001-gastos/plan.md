# Plan 001 — Registro de gastos personales

Estado: **aprobado** (11/03, Ana) · Spec: `specs/001-gastos/spec.md` (aprobada el 10/03, clarify
ronda 2)

## Dependencias nuevas

- **Ninguna.** Express, Vitest y supertest ya los instaló `/sdd-init`. La validación se escribe a
  mano (son 4 campos; una librería de esquemas no se justifica todavía).

## Hallazgos que definen el diseño (verificados el 11/03)

1. **Validar decimales multiplicando por 100 falla.** En JavaScript `0.29 * 100` da
   `28.999999999999996`, así que `Number.isInteger(monto * 100)` **rechazaría 0,29**, un monto
   válido (caso 3).
   → Los decimales se validan sobre el texto del número: `/^\d+(\.\d{1,2})?$/.test(String(monto))`.
   Comprobado: acepta `0.29` y `10.5`, rechaza `10.555` y `1e+21`.
2. **Sumar decimales acumula error.** `0.1 + 0.2` da `0.30000000000000004` (RNF-1).
   → Los montos se guardan en **centavos enteros** (`Math.round(monto * 100)`: 0,29 → 29), se suman
   como enteros y se convierten a pesos solo al responder.
3. **`new Date()` del servidor está en UTC.** Para "hoy" en la zona configurada se usa
   `Intl.DateTimeFormat('en-CA', { timeZone })`, que devuelve `AAAA-MM-DD` en esa zona (RNF-3).

## Estructura

```
src/gastos/
  gasto.ts                 tipo Gasto y CATEGORIAS (RF-3)
  validar-gasto.ts         validación de campos → errores que nombran el campo (RF-2–RF-4, RF-9)
  gastos.repository.ts     almacenamiento en memoria, en centavos; orden de registro (RF-1, RF-5)
  gastos.service.ts        registrar, listar del mes, resumen con las 5 categorías (RF-5, RF-7)
  gastos.router.ts         POST /gastos, GET /gastos?mes=, GET /gastos/resumen?mes= (RF-1, RF-6)
  reloj.ts                 Reloj inyectable con la zona horaria (RF-4, RNF-3)
src/errores.ts             middleware: todo error → {"detalle"} (RF-8)
test/gastos.e2e.test.ts    la API completa con supertest
```

## Decisiones (y por qué)

1. **Centavos enteros por dentro, pesos por fuera.** Ver hallazgo 2: es la forma estándar de
   evitar errores de redondeo. Se descartó usar una librería de decimales: es una dependencia para
   un problema que los enteros resuelven.
2. **Un `Reloj` inyectable.** Los tests de fecha (casos 7 y 8) necesitan fijar "ahora" y la zona;
   depender de la hora real haría los tests frágiles. El reloj se pasa al servicio, no es global.
3. **Capas** (constitución, principio 3): el router solo traduce HTTP; la validación y las reglas
   viven en el servicio; los datos, en el repositorio.
4. **Campos desconocidos → 400** (RF-9) antes de validar el resto: así el error nombra el campo que
   sobra y no confunde.

## Estrategia de tests

| Qué | Dónde |
|---|---|
| Validación de cada campo (casos 1–6, 9, 10) | unitarios de `validar-gasto.ts` + e2e de un caso por campo |
| Fechas y zona horaria (casos 7 y 8) | unitarios con un `Reloj` fijo |
| Orden y filtro por mes (RF-5, RF-6, caso 12) | e2e |
| Resumen y centavos (RF-7, RNF-1, caso 11) | unitarios del servicio + e2e |
| RNF-2 (10.000 gastos < 500 ms) | e2e que carga 10.000 gastos en el repositorio y mide |

## Riesgos

- **Zona horaria:** fácil de testear mal si el reloj del test se fija en un horario "cómodo".
  Mitigación: los casos 7 y 8 usan horarios cercanos a la medianoche.
- **RNF-2 en el CI:** una máquina lenta podría dar falsos rojos. Mitigación: margen amplio (500 ms
  contra ~20 ms medidos localmente).

## Tareas (detalle en `tasks.md`)

1. Registrar un gasto válido y formato de errores.
2. Validaciones de todos los campos.
3. Listar por mes.
4. Resumen por categoría y rendimiento.
