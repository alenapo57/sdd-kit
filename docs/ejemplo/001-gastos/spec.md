# Spec 001 — Registro de gastos personales

Estado: **aprobada** (10/03, Ana) · Se apoya en: —

## Contexto y objetivo

Hoy los gastos se anotan en una planilla y a fin de mes nadie sabe cuánto se fue en cada cosa.
Esta feature permite **registrar gastos** y ver, para un mes, **la lista y un resumen por
categoría**, para decidir dónde recortar.

## Usuarios

| Rol | Qué hace |
|---|---|
| Usuario | Registra sus gastos y consulta la lista y el resumen de un mes |

## Historias de usuario

- **HU-1.** Como usuario, quiero registrar un gasto con su monto, categoría y fecha, para no
  depender de la planilla. *Aceptación:* un gasto válido queda guardado y uno inválido se rechaza
  diciendo qué campo está mal (RF-1 a RF-4, RF-9).
- **HU-2.** Como usuario, quiero ver los gastos de un mes, para revisar en qué gasté.
  *Aceptación:* lista del mes pedido, en orden de fecha (RF-5, RF-6).
- **HU-3.** Como usuario, quiero el total del mes por categoría, para saber dónde recortar.
  *Aceptación:* total y total por cada categoría, exactos al centavo (RF-7, RNF-1).

## Requisitos funcionales (en EARS)

**Registrar**

- **RF-1:** CUANDO se registra un gasto con monto, categoría, fecha y una descripción opcional
  válidos, EL SISTEMA lo guarda y responde 201 con el gasto completo y su `id`.
- **RF-2:** SI el monto no es un número mayor que 0 y menor o igual que 1.000.000 con hasta 2
  decimales, ENTONCES EL SISTEMA responde 400 con un `detalle` que nombra `monto`.
- **RF-3:** SI la categoría no es una de `comida`, `transporte`, `casa`, `ocio` u `otros`
  (sin distinguir mayúsculas), ENTONCES EL SISTEMA responde 400 que nombra `categoria`. Una
  categoría válida se guarda en minúsculas.
- **RF-4:** SI la fecha no es una fecha real con formato `AAAA-MM-DD`, o es posterior al día de hoy
  en la zona horaria configurada, ENTONCES EL SISTEMA responde 400 que nombra `fecha`.
- **RF-9:** SI la descripción tiene más de 200 caracteres, o el pedido trae un campo que no es
  `monto`, `categoria`, `fecha` ni `descripcion`, ENTONCES EL SISTEMA responde 400 que nombra el
  campo.

**Consultar**

- **RF-5:** CUANDO se piden los gastos de un mes (`mes=AAAA-MM`), EL SISTEMA responde 200 con los
  gastos de ese mes ordenados por fecha y, a igual fecha, por orden de registro.
- **RF-6:** SI `mes` falta o no tiene el formato `AAAA-MM` con un mes del 01 al 12, ENTONCES EL
  SISTEMA responde 400 que nombra `mes`.
- **RF-7:** CUANDO se pide el resumen de un mes, EL SISTEMA responde 200 con el total del mes y el
  total de **cada una** de las cinco categorías (0 para las que no tienen gastos).

**Errores**

- **RF-8:** EL SISTEMA DEBE responder todo error con el cuerpo `{"detalle": "<mensaje>"}`.

## Requisitos no funcionales

- **RNF-1:** Los totales son exactos al centavo: tres gastos de 0,10 + 0,20 + 0,29 suman
  exactamente 0,59.
- **RNF-2:** El resumen de un mes con 10.000 gastos responde en menos de 500 ms en el CI.
- **RNF-3:** La zona horaria para "hoy" se configura por entorno; por defecto,
  `America/Argentina/Buenos_Aires`.

## Casos límite

| # | Situación | Esperado |
|---|---|---|
| 1 | Monto `0` o negativo | 400 `monto` |
| 2 | Monto `10.555` (3 decimales) | 400 `monto` |
| 3 | Monto `0.29` | 201 (es válido; ver plan, hallazgo 1) |
| 4 | Monto `1000000.01` | 400 `monto` |
| 5 | Categoría `Comida` | 201, guardada como `comida` |
| 6 | Fecha `2026-02-30` | 400 `fecha` |
| 7 | Fecha de mañana | 400 `fecha` |
| 8 | Fecha de hoy a las 23:30 en la zona configurada | 201 |
| 9 | Campo desconocido (`id`, `usuario`…) | 400 que nombra el campo |
| 10 | Descripción de 201 caracteres | 400 `descripcion` |
| 11 | Mes sin gastos | lista vacía; resumen con total 0 y las cinco categorías en 0 |
| 12 | `mes=2026-13` o `mes=marzo` | 400 `mes` |

## Fuera de alcance

- Usuarios y login (todos los gastos son de un único usuario).
- Editar o borrar gastos; categorías personalizadas; monedas distintas del peso.
- Guardar en una base de datos (en esta feature, en memoria; la persistencia es la 002).

## Criterios de finalización

- Cada RF y cada caso límite con test automatizado.
- Gates en verde (`docs/constitution.md`) y PR mergeado.

## Decisiones de la entrevista (10/03)

1. **Categorías:** cinco fijas (`comida`, `transporte`, `casa`, `ocio`, `otros`).
2. **Monto máximo:** 1.000.000 por gasto (evita errores de tipeo con ceros de más).
3. **Fechas futuras:** no se permiten (un gasto es algo que ya pasó).
4. Por defecto (avisado en la entrevista): un solo usuario, sin login; errores con `detalle`;
   descripción opcional de hasta 200 caracteres.

Correcciones aprobadas tras clarify ronda 1 (A1, A2, C1, C2, C3, T1, R1): zona horaria configurable
para "hoy" (RNF-3, casos 7 y 8); categorías sin distinguir mayúsculas (RF-3, caso 5); monto máximo
y caso 4; campos desconocidos y descripción (RF-9, casos 9 y 10); orden a igual fecha (RF-5); RNF-2
medible en el CI con 500 ms; HU-3 con RNF-1 y caso 11.

## Dudas abiertas

- Ninguna.
