---
name: sdd-plan
description: 'SDD · Escribe el plan (cómo se construye, con decisiones justificadas, hallazgos verificados, tests y riesgos) y propone las tareas, y para a esperar su aprobación.'
argument-hint: "<NNN-nombre>"
disable-model-invocation: true
---

Feature: `$ARGUMENTS` (si está vacío, usá la feature en curso según `.claude/tareas/` y `specs/`).

Seguí **al pie de la letra** `.claude/skills/sdd/fases/plan.md` (y `fases/tasks.md` para el
borrador de tareas), con las reglas de `.claude/skills/sdd/SKILL.md`. Requisito: spec aprobada y
clarify en ronda 2 `APROBADA`; si no, pará. **No escribas código de la feature.**
