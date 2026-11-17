# Clarify 001 — Informe QA de la spec (rol revisor)

> Solo detecciones, sin soluciones. Revisión contra `spec.md` (aprobada el 10/03) y
> `docs/constitution.md`. No hay specs anteriores ni código previo.

## Ronda 1 — 10/03

### 1. Ambigüedades

- **A1.** RF-4 rechaza fechas "posteriores al día de hoy", pero no dice **hoy dónde**. Un
  servidor en UTC y un usuario en Argentina tienen fechas distintas entre las 21:00 y las 23:59:
  un gasto de "hoy" a la noche podría rechazarse, o uno de "mañana" aceptarse.
- **A2.** RF-3 lista las categorías en minúsculas, pero no dice qué pasa con `Comida` o `COMIDA`.

### 2. Contradicciones

- Ninguna.

### 3. Casos límite no cubiertos

- **C1.** No hay monto máximo: un `10000000` por un cero de más se acepta sin aviso.
- **C2.** No se dice qué pasa si el pedido trae campos que no corresponden (un `id` enviado por el
  cliente, por ejemplo) ni un límite para la descripción.
- **C3.** RF-5 ordena por fecha, pero no dice el orden de dos gastos del mismo día.

### 4. Reglas no comprobables

- **T1.** RNF-2 pedía "el resumen tiene que ser rápido": sin un número ni un entorno de medición
  no se puede verificar con un test.

### 5. RF sin criterio de aceptación

- **R1.** HU-3 no tiene caso límite: ¿qué devuelve el resumen de un mes sin gastos?, ¿y la suma de
  montos con centavos?

**VEREDICTO: CAMBIOS NECESARIOS** (A1, A2, C1, C2, C3, T1, R1)

<!-- En el chat, el agente propuso una resolución por detección; Ana aprobó todas y ajustó el monto
     máximo (propuesto: 10.000.000; aprobado: 1.000.000). -->

## Ronda 2 — 10/03

Revisión de la spec corregida (resoluciones aprobadas por el usuario).

| Detección | Dónde quedó resuelta | Estado |
|---|---|---|
| A1 | RF-4 ("en la zona horaria configurada"), RNF-3, casos 7 y 8 | Resuelta |
| A2 | RF-3 (sin distinguir mayúsculas, se guarda en minúsculas), caso 5 | Resuelta |
| C1 | RF-2 (máximo 1.000.000), caso 4 | Resuelta |
| C2 | RF-9 (campos desconocidos y descripción ≤ 200), casos 9 y 10 | Resuelta |
| C3 | RF-5 (a igual fecha, por orden de registro) | Resuelta |
| T1 | RNF-2 (10.000 gastos, < 500 ms, medido en el CI) | Resuelta |
| R1 | RNF-1 (exacto al centavo), caso 11, HU-3 | Resuelta |

Verificaciones de la ronda 2: las correcciones no generan contradicciones nuevas (RF-9 usa el mismo
formato de error que RF-8); cada RF tiene caso o método de verificación; no quedan dudas abiertas.

**VEREDICTO: APROBADA**
