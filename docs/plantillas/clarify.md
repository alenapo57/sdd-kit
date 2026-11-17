# Clarify NNN — Informe QA de la spec (rol revisor)

> Solo detecciones, sin soluciones. Revisión contra `spec.md` (aprobada el <fecha>),
> `docs/constitution.md`, las specs anteriores y <el código actual / la consigna, si aplica>.

## Ronda 1 — <fecha>

<!-- Si hay una consigna o un documento de requisitos externo, primero una tabla que verifique
     que cada exigencia está en la spec. -->

### 1. Ambigüedades

- **A1.** <requisito que se puede interpretar de más de una forma, y por qué importa>

### 2. Contradicciones

- **K1.** <dos requisitos que chocan, o uno que choca con la constitución o con otra spec>

### 3. Casos límite no cubiertos

- **C1.** <situación sin requisito: vacíos, inexistentes, duplicados, concurrencia, sin permiso,
  fallas a mitad de camino, límites numéricos…>

### 4. Conflictos con la constitución / reglas no comprobables

- **T1.** <requisito que no se puede verificar con un test tal como está escrito>

### 5. RF sin criterio de aceptación

- **R1.** <RF que no aparece en ninguna historia ni caso, o sin resultado observable>

**VEREDICTO: CAMBIOS NECESARIOS** (A1, K1, C1, T1, R1)  <!-- o APROBADA si no hay nada -->

<!--
  Entre la ronda 1 y la 2: el agente propone una resolución por detección (en el chat, no acá),
  el usuario las aprueba o ajusta, y se incorporan a la spec ("Correcciones aprobadas tras
  clarify ronda 1" al final de la spec).
-->

## Ronda 2 — <fecha>

Revisión de la spec corregida (resoluciones aprobadas por el usuario).

| Detección | Dónde quedó resuelta | Estado |
|---|---|---|
| A1 | RF-… / caso … | Resuelta |

Verificaciones de la ronda 2: <que las correcciones no crearon contradicciones nuevas, que cada
RF tiene caso o método de verificación, que no quedan dudas abiertas>.

**VEREDICTO: APROBADA**
