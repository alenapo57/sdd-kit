---
name: sdd-clarify
description: 'SDD · Revisa la spec aprobada como QA en dos rondas (detecciones → resoluciones aprobadas → ronda 2) y la deja lista para planificar.'
argument-hint: "<NNN-nombre>"
disable-model-invocation: true
---

Feature: `$ARGUMENTS` (si está vacío, usá la feature en curso según `.claude/tareas/` y `specs/`).

Seguí **al pie de la letra** `.claude/skills/sdd/fases/clarify.md`, con las reglas de
`.claude/skills/sdd/SKILL.md`. En la ronda 1, **solo detecciones**; las resoluciones se proponen
en el chat y **se esperan aprobadas** antes de tocar la spec.
