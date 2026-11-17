# Fase: plan (`/sdd-plan NNN-nombre`)

**Objetivo:** decidir **cómo** se construye lo que dice la spec, con cada decisión justificada.
Requisito: spec aprobada y clarify con ronda 2 `APROBADA`. Si no, **PARÁ**.

## 1. Investigar antes de diseñar

- Leé la spec, el clarify, `docs/constitution.md`, `MEMORY.md` y el código existente que se parece
  a lo que hay que hacer (convenciones, módulos vecinos, cómo se testea hoy).
- **Verificá lo que el diseño supone**, no lo recuerdes: leé la documentación actual o el código
  fuente de la librería, o hacé un experimento chico. Lo que se compruebe va a la sección
  "Hallazgos que definen el diseño (verificados el <fecha>)". Ejemplos de cosas que vale la pena
  verificar: cómo maneja la librería las transacciones o la concurrencia, qué hace el framework al
  fallar el arranque, cómo se pasan los parámetros a la base, qué incluye la imagen base.

## 2. Escribir `plan.md`

Plantilla `docs/plantillas/plan.md`:

- **Dependencias nuevas**: con versión y porqué. Si se puede sin dependencias, preferilo y decilo.
  **Cada dependencia nueva requiere aprobación explícita.**
- **Estructura**: árbol de archivos nuevos o modificados, para qué sirve cada uno y qué RF cubre.
- **Decisiones (y por qué)**: numeradas; para cada una, qué se descartó y cómo cumple la
  constitución. Escribilas para que alguien que está aprendiendo entienda el razonamiento.
- **Estrategia de tests**: qué RF y casos cubre cada tipo de test. Concurrencia **sin sleeps**
  (barreras controladas por el test). Servicios externos con **dobles controlables** inyectados,
  nunca con flags de producción.
- **Riesgos**: qué puede salir mal y cómo se va a detectar.
- **Tareas**: lista tentativa (se detalla en `tasks.md`).

## 3. Recomendado: plan y tareas juntos

Escribí también `tasks.md` (ver `fases/tasks.md`) en borrador y pedí la aprobación de **ambos
juntos**: ahorra una vuelta y el usuario ve el plan ya partido en trabajo concreto.

## 4. Presentar y PARAR

Resumen en simple de las decisiones principales, las dependencias nuevas (si hay) y los riesgos.
Si hay decisiones que son del usuario (una dependencia, un trade-off, un permiso especial como
pushear una rama varias veces), preguntalas explícitamente. Cerrá con: **"¿Aprobás el plan (y las
tareas)?"**

## 5. Con la aprobación

`Estado: **aprobado** (<fecha>, <quién>)` en plan y tareas, commit `NNN: plan y tareas aprobados`,
archivo de tarea actualizado (con las autorizaciones especiales que se hayan dado).
