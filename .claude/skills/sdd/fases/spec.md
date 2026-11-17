# Fase: spec (`/sdd-spec NNN-nombre "idea"`)

**Objetivo:** escribir **qué** hay que construir y **por qué**, sin decir cómo. **No escribas
código en esta fase.**

## 1. Preparar

- Carpeta: `specs/NNN-nombre/`. Si el usuario no dio número, proponé el siguiente libre (mirá
  `specs/`) y un nombre corto en kebab-case.
- Leé `docs/constitution.md`, `MEMORY.md`, las specs anteriores relacionadas y, si ya hay código,
  lo que la feature va a tocar: así no preguntás lo que ya está decidido y detectás de entrada lo
  que la feature asume.
- Creá el archivo de tarea `.claude/tareas/NNN-nombre.md`.
- Si el repo trabaja con ramas: creá la rama `NNN-nombre` desde `main` actualizado (antes,
  `git status` limpio; si hay cambios sin commitear, preguntá qué hacer con ellos).

## 2. Entrevista (máximo 5 preguntas)

- Solo las preguntas cuya respuesta **cambia la spec**. Lo que tiene un default razonable no se
  pregunta: se elige y se avisa.
- Cada pregunta con opciones concretas, **la recomendada primero** y su porqué en una línea.
- Al final avisá qué asumiste por defecto: va a la spec como "Por defecto (avisado en la
  entrevista)" dentro de "Decisiones de la entrevista".

## 3. Escribir `spec.md`

Usá `docs/plantillas/spec.md`. Reglas:

- **Solo qué y por qué.** Nada de framework, librerías, archivos, tablas ni algoritmos.
- Requisitos funcionales en **EARS** (ver la skill `sdd`), agrupados por tema, con los resultados
  exactos cuando importan (códigos, mensajes, formatos).
- Requisitos no funcionales **medibles**.
- Casos límite **numerados** con su resultado esperado: vacíos, inexistentes, duplicados, límites
  numéricos, concurrencia, sin permiso, fallas a mitad de camino, reintentos.
- `Estado: **borrador**`.

**Autochequeo antes de mostrarla:**
- ¿Cada RF tiene condición y resultado observables?
- ¿Cada historia de usuario apunta a RF?
- ¿Se coló alguna palabra de stack?
- ¿Quedan dudas sin resolver?

## 4. Presentar y PARAR

Resumen en simple: qué va a hacer el sistema, las decisiones clave de la entrevista y lo que quedó
fuera de alcance. Link al archivo. Cerrá con: **"¿Aprobás la spec para pasar al clarify?"**

## 5. Con la aprobación

- `Estado: **aprobada** (<fecha>, <quién>)`.
- Actualizá el archivo de tarea. **Todavía no se commitea**: la spec entra junto con el clarify.
- Si el usuario pidió seguir ("aprobada, seguí con el clarify"), pasá a `fases/clarify.md`.
