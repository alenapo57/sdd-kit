# AGENTS.md — flujo SDD para agentes de IA

Este repositorio trabaja con **Spec-Driven Development (SDD)**: primero se especifica qué hay que
construir y por qué; recién después, cómo; y recién después, el código. **La spec manda.**

Está armado para **Claude Code**: las instrucciones completas están en `CLAUDE.md` y los comandos
`/sdd-*` en `.claude/skills/`. Si usás otra herramienta (Codex, Cursor, OpenCode, …), estas son
las mismas reglas en versión resumida, y los procedimientos de cada fase se pueden seguir a mano.

## Fases (cada una para y espera aprobación explícita)

| Fase | Produce | Procedimiento |
|---|---|---|
| Configurar el proyecto (una vez) | `docs/constitution.md`, gates, CI | `.claude/skills/sdd/fases/init.md` |
| Spec (qué y por qué) | `specs/NNN-nombre/spec.md` | `.claude/skills/sdd/fases/spec.md` |
| Clarify (revisión QA, 2 rondas) | `specs/NNN-nombre/clarify.md` | `.claude/skills/sdd/fases/clarify.md` |
| Plan (cómo) | `specs/NNN-nombre/plan.md` | `.claude/skills/sdd/fases/plan.md` |
| Tareas | `specs/NNN-nombre/tasks.md` | `.claude/skills/sdd/fases/tasks.md` |
| Implementar UNA tarea | código + tests + commit | `.claude/skills/sdd/fases/implement.md` |
| Validar | `specs/NNN-nombre/validate.md` | `.claude/skills/sdd/fases/validate.md` |
| Cambiar un requisito | spec actualizada | `.claude/skills/sdd/fases/change.md` |

## Reglas que no se negocian

- Antes de trabajar: leé el archivo de tarea en `.claude/tareas/` (si existe), `MEMORY.md` y
  `docs/constitution.md`.
- Tests primero (en rojo), después el código, después los gates en verde. Nunca borrar ni comentar
  un test para que pase; nunca marcar `[x]` con un gate en rojo.
- No modificar código que no leíste; nada de cambios fuera de lo pedido (anotalos y avisá).
- Preguntar cuando algo ambiguo cambia lo que vas a hacer; si hay un default razonable, elegirlo y
  avisar.
- Verificar de verdad (tests, build, la app); decir explícitamente lo que no se pudo verificar.
- Secretos solo en `.env` (nunca en el repo). Push solo con OK del usuario.
- Explicar las decisiones importantes en simple.

Comandos del proyecto y convenciones: ver las secciones "Proyecto" y "Comandos y gates" de
`CLAUDE.md` (las completa la configuración inicial).
