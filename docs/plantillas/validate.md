VEREDICTO: APROBADO  <!-- o: VEREDICTO: CAMBIOS NECESARIOS (siempre en la PRIMERA línea) -->

# Validación NNN — <Título>

Fecha: <fecha> · Rol: revisor · Rama `NNN-nombre`
Insumos: `spec.md`, `clarify.md`, `plan.md`, `tasks.md`, `MEMORY.md`, `docs/constitution.md`,
el código y `git log`.

<!-- Si durante la validación apareció un hallazgo que se convirtió en tarea nueva, va acá arriba:
> **Hallazgo durante la validación (ya corregido):** <qué no cumplía, qué tarea se agregó
> con aprobación del usuario, commits>. -->

## Gates (ejecutados en esta validación)

| Gate | Resultado |
|---|---|
| <lint> | <0 errores, 0 avisos> |
| <tests> | <N en verde> |

## Requisitos → tests → resultado

<!-- Un RF sin test que lo cubra NO está implementado. -->

| RF | Test / evidencia | Resultado |
|---|---|---|
| RF-1 <resumen> | `<archivo de test>` "<nombre del test>" | OK |

## Requisitos no funcionales

| RNF | Evidencia | Resultado |
|---|---|---|
| RNF-1 | <medición, test o revisión> | OK |

## Casos límite

| # | Test |
|---|---|
| 1 | `<archivo>` "<test>" |

## Pruebas de mutación (si hubo tests que caracterizan algo existente)

- <qué se rompió a propósito, qué test falló, y que se revirtió>

## Constitución

<!-- Cada principio: cumplido / cómo se verificó. Tests modificados (no borrados) y por qué. -->

## Proceso

Spec + clarify (`<commit>`) → plan y tareas (`<commit>`) → T1 `<commit>` · T2 `<commit>` …
`MEMORY.md` actualizado en cada tarea.

## Observaciones (no bloquean)

1. <desvíos registrados, correcciones de plan/spec, deuda técnica, pendientes para otra feature>
