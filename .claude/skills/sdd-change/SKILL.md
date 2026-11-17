---
name: sdd-change
description: 'SDD · Cambia o agrega un requisito a una feature: primero la spec (con su impacto en plan y tareas), con aprobación, y recién después el código.'
argument-hint: "<NNN-nombre> \"<qué cambia>\""
disable-model-invocation: true
---

Argumentos: `$ARGUMENTS`
(la feature `NNN-nombre` y la descripción del cambio. Si falta alguno, preguntalo.)

Seguí **al pie de la letra** `.claude/skills/sdd/fases/change.md`, con las reglas de
`.claude/skills/sdd/SKILL.md`. **No toques código** hasta que el cambio de la spec esté aprobado.
