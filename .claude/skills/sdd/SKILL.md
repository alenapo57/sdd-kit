---
name: sdd
description: 'Manual del flujo Spec-Driven Development de este repo (init → spec → clarify → plan → tasks → implement → validate, y change). Usala siempre que haya que escribir o revisar una spec, un plan o una lista de tareas, implementar o cerrar una tarea, validar una feature o cambiar un requisito, y cuando el usuario diga cosas como "seguí con T3", "aprobado, seguí", "escribí el validate" o "retomemos".'
user-invocable: false
---

# SDD — Spec-Driven Development

> La IA ejecuta, pero el responsable del proyecto es el usuario.
> La spec manda: nada se implementa si no está en una spec aprobada.

## El flujo y sus puertas

```
init (una vez)
spec ─PARA─ clarify (ronda 1 ─PARA─ ronda 2) ─PARA─ plan ─PARA─ tasks ─PARA─
implement T1 ─PARA─ T2 ─PARA─ … ─PARA─ validate ─PARA─ push · PR · merge · verificación en main
```

**PARA** significa: terminá la fase, mostrá el resultado con un resumen en simple y **esperá la
aprobación explícita** del usuario. "Dale", "aprobado", "seguí con X" cuentan como aprobación de lo
que se presentó; una pregunta del usuario **no** es una aprobación.

## Qué procedimiento seguir

Leé el archivo de la fase **antes** de empezarla, aunque creas recordarlo:

| Si el usuario… | Seguí |
|---|---|
| usa `/sdd-init` o el proyecto no está configurado | [fases/init.md](fases/init.md) |
| pide una feature nueva, `/sdd-spec` | [fases/spec.md](fases/spec.md) |
| aprueba la spec / pide revisarla, `/sdd-clarify` | [fases/clarify.md](fases/clarify.md) |
| aprueba el clarify / pide el plan, `/sdd-plan` | [fases/plan.md](fases/plan.md) |
| aprueba el plan / pide las tareas, `/sdd-tasks` | [fases/tasks.md](fases/tasks.md) |
| dice "seguí con Tn", `/sdd-implement` | [fases/implement.md](fases/implement.md) |
| pide validar / cerrar la feature, `/sdd-validate` | [fases/validate.md](fases/validate.md) |
| cambia o agrega un requisito, `/sdd-change` | [fases/change.md](fases/change.md) |
| pregunta dónde quedamos, `/sdd-status` | [fases/status.md](fases/status.md) |
| abre una sesión nueva para seguir, `/sdd-siguiente` | [fases/siguiente.md](fases/siguiente.md) |

Las plantillas de cada artefacto están en `docs/plantillas/`. Un ejemplo completo y real de cómo
quedan está en `docs/ejemplo/001-gastos/`.

## Reglas que valen en todas las fases

1. **Leé primero el contexto**: archivo de tarea en `.claude/tareas/`, `MEMORY.md`,
   `docs/constitution.md` y los artefactos previos de la feature. Si falta una fase previa
   aprobada (por ejemplo, no hay `tasks.md` aprobado), **PARÁ** y decilo.
2. **Línea de estado** arriba de cada artefacto: `Estado: **borrador**` → `Estado: **aprobada**`
   (spec) / `**aprobado**` (plan, tareas). Solo la cambia una aprobación del usuario, y se anota la
   fecha y quién aprobó.
3. **Preguntas**: cuando una decisión es del usuario, preguntá con opciones concretas, **la
   recomendada primero** y su porqué en una línea. Si tenés la herramienta de preguntas
   (AskUserQuestion), usala. Si hay un default razonable, elegilo y avisá en vez de preguntar.
4. **Explicá en simple** las decisiones importantes: el usuario está aprendiendo.
5. **Archivo de tarea** (`.claude/tareas/<feature>.md`): actualizalo al final de cada paso con
   commits, estado y qué sigue.
6. **Commits** solo en los momentos que autoriza `CLAUDE.md`; **push** solo con OK.
7. **Honestidad**: lo que no se pudo verificar se dice. Una suposición que resultó falsa se corrige
   por escrito ("Corrección en Tn: …"), no en silencio.

## EARS: requisitos que se pueden testear

Cada requisito funcional (`RF-n`) se escribe con una de estas formas. Si no tiene una condición y
un resultado **observables**, no se puede testear: se reescribe.

| Forma | Cuándo | Ejemplo |
|---|---|---|
| CUANDO … EL SISTEMA … | evento | CUANDO se pide `GET /gastos`, EL SISTEMA responde 200 con la lista |
| MIENTRAS … EL SISTEMA … | estado | MIENTRAS la cuenta está bloqueada, EL SISTEMA rechaza el login con 403 |
| SI … ENTONCES … | condición no deseada | SI el monto es negativo, ENTONCES EL SISTEMA responde 400 que nombra el campo |
| A MENOS QUE … | excepción | EL SISTEMA oculta el detalle A MENOS QUE el usuario sea admin |
| EL SISTEMA DEBE … | regla fija o no funcional | EL SISTEMA DEBE responder en < 200 ms con 1000 registros |

## Lecciones que ya costaron caro (aplicalas sin que te las pidan)

- **Test en rojo primero, y verlo fallar por el motivo correcto** (no por un import roto que tape
  el problema). Si por algún motivo no se pudo correr el rojo, registralo como desvío en
  `MEMORY.md` y en el validate.
- **Un test que caracteriza algo que ya existía pasa en verde de entrada**: probalo con una
  **mutación** (romper el código a propósito, ver que el test falla, revertir). Una mutación que el
  test no detecta puede ser una mala mutación: analizala antes de concluir.
- **Verificá con la app real** cuando la tarea toca arranque, configuración o integración: los
  tests no ven todo.
- **Todo módulo nuevo necesita tests unitarios**, no solo de integración: si no, el gate de
  cobertura (si existe) corta.
- **Hallazgo en la validación → tarea nueva** con aprobación del usuario (y su test en rojo). Nunca
  un arreglo silencioso.
- **Tareas que solo se pueden verificar en CI** (por ejemplo, sin Docker local): commit sin `[x]`,
  push con OK, y el cierre con `[x]` recién después de la corrida en verde, con el link.
- **Los conteos y números del documento se verifican**, no se recuerdan (cantidad de tests,
  cobertura, operaciones de la API): corré el comando y copiá el resultado.
- **Nada de dependencias nuevas** que el plan aprobado no liste.
