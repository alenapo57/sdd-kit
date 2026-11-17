---
name: sdd-validate
description: 'SDD · Valida la feature requisito por requisito con evidencia (gates, tests, CI), escribe validate.md con el veredicto en la primera línea y guía el cierre (push, PR, merge y verificación en main).'
argument-hint: "<NNN-nombre>"
disable-model-invocation: true
---

Feature: `$ARGUMENTS` (si está vacío, usá la feature en curso según `.claude/tareas/` y `specs/`).

Seguí **al pie de la letra** `.claude/skills/sdd/fases/validate.md`, con las reglas de
`.claude/skills/sdd/SKILL.md`. Un hallazgo **no se arregla en silencio**: se propone como tarea
nueva y se espera la aprobación. El push siempre con OK.
