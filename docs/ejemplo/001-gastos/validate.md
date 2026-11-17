VEREDICTO: APROBADO

# Validación 001 — Registro de gastos personales

Fecha: 12/03 · Rol: revisor · Rama `001-gastos`
Insumos: `spec.md`, `clarify.md`, `plan.md`, `tasks.md`, `MEMORY.md`, `docs/constitution.md`, el
código y `git log`.

> **Hallazgo durante la validación (ya corregido):**
> - **El problema:** al recorrer RF-4, el chequeo de "fecha futura" comparaba con
>   `new Date().toISOString()`, es decir, la fecha en **UTC**.
>   - Entre las 21:00 y las 23:59 de Buenos Aires, en UTC ya es el día siguiente: un gasto de
>     "mañana" se aceptaba.
>   - Los tests de T2 no lo veían porque fijaban el reloj al mediodía.
> - **La solución:** con aprobación de Ana se agregó la **T5** (`b7e21c4`), con dos tests a las
>   23:30 de Buenos Aires. Los dos estuvieron en rojo antes del cambio y pasan después de comparar
>   con la fecha en la zona configurada (plan, hallazgo 3).

## Gates (ejecutados en esta validación)

| Gate | Resultado |
|---|---|
| `npm run lint` | 0 errores, 0 avisos |
| `npm run typecheck` | OK |
| `npm test` | 41 en verde (28 unitarios, 13 e2e) |
| `npm run test:cov` | líneas 94% · ramas 89% (umbral 80/80) |

## Requisitos → tests → resultado

| RF | Test | Resultado |
|---|---|---|
| RF-1 registrar | e2e "registra un gasto válido → 201 con id" | OK |
| RF-2 monto | unit `validar-gasto` casos 1–4; e2e "monto inválido → 400 monto" | OK |
| RF-3 categoría | unit caso 5 y "categoría inexistente"; e2e | OK |
| RF-4 fecha | unit casos 6–8 (T5: 23:30 en Buenos Aires); e2e | OK |
| RF-5 listar | e2e "lista del mes ordenada por fecha y registro" | OK |
| RF-6 mes | e2e "mes inválido → 400 mes" (caso 12) | OK |
| RF-7 resumen | unit "las 5 categorías, con 0 si no hay gastos"; e2e | OK |
| RF-8 errores | e2e "un error inesperado responde 500 con detalle"; todos los 400 verifican `detalle` | OK |
| RF-9 campos | unit casos 9 y 10; e2e "campo id → 400 id" | OK |

## Requisitos no funcionales

| RNF | Evidencia | Resultado |
|---|---|---|
| RNF-1 exacto al centavo | unit "0,10 + 0,20 + 0,29 = 0,59 exacto" (con decimales daría 0,5900000000000001) | OK |
| RNF-2 < 500 ms | e2e con 10.000 gastos: 23 ms local, 61 ms en el CI | OK |
| RNF-3 zona configurable | unit con `America/Argentina/Buenos_Aires` y con `UTC` | OK (tras T5) |

## Casos límite

| # | Test | # | Test |
|---|---|---|---|
| 1 | unit monto 0 y −5 | 7 | unit T5 "mañana a las 23:30" |
| 2 | unit 10.555 | 8 | unit T5 "hoy a las 23:30" |
| 3 | unit 0.29 (plan, hallazgo 1) | 9 | unit + e2e campo `id` |
| 4 | unit 1000000.01 | 10 | unit descripción de 201 |
| 5 | unit `Comida` → `comida` | 11 | e2e lista vacía + resumen en 0 |
| 6 | unit 2026-02-30 | 12 | e2e `2026-13` y `marzo` |

## Pruebas de mutación

- Al reemplazar la regex de decimales por `Number.isInteger(monto * 100)`, el test del caso 3
  **falla** (rechaza 0,29). Revertido. Confirma que el hallazgo 1 del plan quedó protegido.

## Constitución

Capas respetadas (el router no valida ni calcula), sin `any` ni casts forzados, ningún test borrado,
sin dependencias nuevas, sin secretos.

## Proceso

Spec + clarify (`3f1a9d0`; ronda 1 con 7 detecciones, ronda 2 APROBADA) → plan y tareas (`8c04e2b`)
→ T1 `1d9a7f3` · T2 `5e88b10` · T3 `a43c2d7` · T4 `c19f6e5` → hallazgo de la validación → T5
`b7e21c4`. `MEMORY.md` actualizado en cada tarea.

## Observaciones (no bloquean)

1. **Lección para `MEMORY.md`:** los tests con reloj fijo tienen que incluir horarios cerca de la
   medianoche; el mediodía esconde los errores de zona horaria.
2. La persistencia en memoria se pierde al reiniciar: es lo esperado (fuera de alcance) y es la
   feature 002.
