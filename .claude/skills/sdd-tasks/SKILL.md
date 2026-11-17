---
name: sdd-tasks
description: 'SDD · Parte el plan en un máximo de 10 tareas ordenadas, con lo que cubre cada una y un "Hecho cuando" verificable con un comando.'
argument-hint: "<NNN-nombre>"
disable-model-invocation: true
---

Feature: `$ARGUMENTS` (si está vacío, usá la feature en curso según `.claude/tareas/` y `specs/`).

Seguí **al pie de la letra** `.claude/skills/sdd/fases/tasks.md`, con las reglas de
`.claude/skills/sdd/SKILL.md`. Verificá que todos los RF y casos límite queden cubiertos y **pará**
a esperar la aprobación.
