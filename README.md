# SDD Kit — Spec-Driven Development con Claude Code

Plantilla para trabajar con IA **de forma ordenada**: primero se escribe *qué* hay que construir
y *por qué* (la spec), alguien la revisa buscando huecos, después se decide *cómo* (el plan), se
parte en tareas chicas y recién ahí se escribe código, **una tarea a la vez, con los tests
primero**. Al final, un veredicto con evidencia.

> **La IA ejecuta, pero el responsable del proyecto sos vos.** Cada fase termina con la IA
> esperando tu aprobación. Nada se implementa si no está en una spec que aprobaste.

Sirve para cualquier lenguaje o framework: al arrancar, el agente te entrevista sobre tu
proyecto y configura todo (reglas, comandos de verificación, CI).

---

## Empezar en 5 minutos

**Necesitás:** [Claude Code](https://code.claude.com/docs/en/overview), Git y una cuenta de
GitHub.

1. En GitHub, tocá **"Use this template" → "Create a new repository"** y cloná tu repo nuevo.
2. Abrí Claude Code en la carpeta del repo.
3. Escribí:
   ```
   /sdd-init
   ```
   El agente te va a hacer preguntas de a una: qué proyecto es, qué lenguaje y framework,
   cómo se testea, si querés CI, si lo autorizás a commitear solo… Con eso completa
   `docs/constitution.md`, `CLAUDE.md`, el workflow de CI y los permisos. Revisá y aprobá.
4. Tu primera feature:
   ```
   /sdd-spec 001-nombre-corto "la idea en una frase"
   ```
   Respondé la entrevista (máximo 5 preguntas), leé la spec y aprobala. Después seguí con
   `/sdd-clarify`, `/sdd-plan`, `/sdd-tasks`, `/sdd-implement`, `/sdd-validate`.

**Antes de la primera feature, leé [`docs/GUIA.md`](docs/GUIA.md)** (15 minutos): explica cada
fase, por qué existe, qué revisar antes de aprobar y los errores más comunes. Y mirá el
[ejemplo completo](docs/ejemplo/001-gastos/) de una feature hecha de punta a punta.

---

## Comandos

| Comando | Qué hace | Para y espera |
|---|---|---|
| `/sdd-init` | Entrevista sobre el proyecto y configura constitución, gates, CI y permisos | tu aprobación |
| `/sdd-spec NNN-nombre "idea"` | Entrevista (máx. 5 preguntas) y escribe la spec: qué y por qué | aprobación de la spec |
| `/sdd-clarify NNN-nombre` | Revisa la spec como QA: ambigüedades, contradicciones, casos sin cubrir | aprobación de las resoluciones |
| `/sdd-plan NNN-nombre` | Escribe el plan: cómo, con qué archivos, decisiones y riesgos | aprobación del plan |
| `/sdd-tasks NNN-nombre` | Parte el plan en ≤ 10 tareas con su "Hecho cuando" | aprobación de las tareas |
| `/sdd-implement NNN-nombre T3` | Implementa **una** tarea: test en rojo → código → gates → commit | tu OK para la siguiente |
| `/sdd-validate NNN-nombre` | Revisa todo requisito por requisito y da un veredicto con evidencia | OK para push y PR |
| `/sdd-change NNN-nombre "qué cambia"` | Cambia un requisito: primero la spec, después el código | aprobación del cambio |
| `/sdd-status` | Dónde está cada feature y qué sigue | — |

También podés hablar normal ("seguí con T4", "escribí el validate"): el agente sigue el mismo
procedimiento que el comando.

---

## Qué hay en el repo

```
CLAUDE.md                 Reglas de trabajo que Claude Code lee en cada sesión
AGENTS.md                 Las mismas reglas, resumidas, para otras herramientas de IA
MEMORY.md                 Lo que el proyecto va aprendiendo (decisiones y lecciones por tarea)
docs/
  GUIA.md                 Tutorial del flujo: empezá por acá
  constitution.md         Reglas innegociables del proyecto (las completa /sdd-init)
  guia-github.md          Ramas, pull requests, main protegido y secretos
  plantillas/             Plantillas de cada artefacto (spec, clarify, plan, tasks, validate…)
  ejemplo/001-gastos/     Una feature completa, de la spec al veredicto
specs/                    Acá van las specs de tu proyecto (001-…, 002-…)
.claude/
  settings.json           Permisos: qué puede hacer el agente sin preguntar
  skills/sdd/             El manual del flujo: reglas, EARS y el procedimiento de cada fase
  skills/sdd-*/           Los comandos /sdd-init, /sdd-spec, …
  agents/sdd-reviewer.md  Revisor independiente de solo lectura (opcional)
.github/
  pull_request_template.md  Checklist SDD para cada PR
```

`/sdd-init` además crea `.github/workflows/ci.yml` con los comandos de tu stack, si elegís usar CI.

---

## Cómo se ve una sesión típica

```
vos:    /sdd-spec 002-login "que los usuarios inicien sesión con email y contraseña"
agente: (pregunta 1 de 5, con la opción recomendada primero) …
vos:    (respondés)
agente: escribe specs/002-login/spec.md → "¿Aprobás la spec?"
vos:    Aprobada, seguí con el clarify
agente: ronda 1: 9 detecciones → propone resoluciones → "¿Las aprobás?"
vos:    Sí
agente: incorpora, ronda 2 APROBADA, commit "002: spec aprobada y clarify"
…
vos:    seguí con T3
agente: test en rojo → código → gates en verde → [x] → MEMORY.md → commit → "¿Sigo con T4?"
```

## Retomar otro día

El agente lleva el estado en `.claude/tareas/<tarea>.md` (local, no se sube) y lo duradero en
`MEMORY.md`. En una sesión nueva alcanza con decir *"retomemos la tarea X"* o usar `/sdd-status`.

## Personalizar el kit

Todo es Markdown y se puede adaptar:

| Querés cambiar… | Editá… |
|---|---|
| una regla de trabajo (commits, idioma, cómo explicar) | `CLAUDE.md` |
| cómo se hace una fase (por ejemplo, más preguntas en la entrevista) | `.claude/skills/sdd/fases/<fase>.md` |
| el formato de un artefacto | `docs/plantillas/<artefacto>.md` |
| qué puede hacer el agente sin preguntar | `.claude/settings.json` (lo personal, en `.claude/settings.local.json`) |
| las reglas innegociables del proyecto | `docs/constitution.md` (con acuerdo explícito) |

Cada fase vive en **un solo archivo**: los comandos `/sdd-*` solo apuntan ahí, así que un cambio
vale tanto para el comando como para cuando le hablás en lenguaje natural.
