# Fase: clarify (`/sdd-clarify NNN-nombre`)

**Objetivo:** revisar la spec **como un QA que busca problemas**, antes de que se conviertan en
código. Es la fase que más errores ahorra: un hueco en la spec cuesta una línea acá y una tarde
de debugging después.

## 1. Ronda 1: solo detecciones

Leé la spec aprobada, `docs/constitution.md`, las specs anteriores (¿contradice alguna?), el
código actual que la spec asume y, si existe, la consigna o el documento de requisitos externo.

Escribí `specs/NNN-nombre/clarify.md` (plantilla `docs/plantillas/clarify.md`). **Solo
detecciones, sin proponer soluciones en el archivo:**

| Prefijo | Categoría | Qué buscar |
|---|---|---|
| **A** | Ambigüedades | requisitos que admiten más de una lectura; palabras como "rápido", "válido", "correcto" sin definir |
| **K** | Contradicciones | entre requisitos, con la constitución, con specs anteriores, con el formato de errores del proyecto |
| **C** | Casos no cubiertos | vacíos, inexistentes, duplicados, límites, concurrencia, permisos, falla a mitad de camino, reintentos, apagado |
| **T** | No comprobables | requisitos que no se pueden verificar con un test tal como están escritos; dependencias de tiempos ("sleep") |
| **R** | Sin aceptación | RF que no aparecen en ninguna historia ni caso |

- Si hay consigna: primero una tabla "exigencia → ¿está en la spec?".
- Veredicto: `CAMBIOS NECESARIOS` con la lista de IDs, o `APROBADA` si no hay nada.
- **Una ronda 1 sin hallazgos es sospechosa**: revisá de nuevo concurrencia, permisos, errores a
  mitad de camino, valores límite e idempotencia antes de aprobar.
- **Revisión independiente (opcional):** si el usuario lo pide o la feature es crítica, delegá la
  ronda 1 al subagente `sdd-reviewer`, que no participó de la spec y no puede modificar archivos.

## 2. Resoluciones (en el chat) y PARAR

Proponé **una resolución por detección**, en simple, agrupadas por categoría, explicando el porqué
de las que no sean obvias. Cerrá con: **"¿Aprobás estas resoluciones para incorporarlas a la spec y
cerrar el clarify con la ronda 2?"** El usuario puede aprobar todo, ajustar alguna o descartarla.

## 3. Incorporar

- Modificá la spec según lo aprobado: requisitos nuevos o reescritos, casos límite nuevos,
  secciones nuevas (por ejemplo, "Adaptaciones" si algo de la consigna no aplica tal cual).
- Al final de la spec: "Correcciones aprobadas tras clarify ronda 1 (IDs): <resumen>".
- La spec sigue `aprobada`, con la fecha de las correcciones.

## 4. Ronda 2

En `clarify.md`: tabla **detección → dónde quedó resuelta → estado**, verificaciones (que las
correcciones no crearon contradicciones nuevas, que cada RF tiene caso o método de verificación,
que no quedan dudas abiertas) y `**VEREDICTO: APROBADA**`. Si algo quedó sin resolver,
`CAMBIOS NECESARIOS` y se repite el paso 2.

## 5. Cerrar

- `MEMORY.md`: la feature pasa a "en curso" con las decisiones principales.
- Commit `NNN: spec aprobada y clarify` (spec + clarify + MEMORY) si está autorizado.
- Archivo de tarea actualizado. Preguntá si sigue con el plan.
