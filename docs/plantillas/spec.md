# Spec NNN — <Título corto de la feature>

Estado: **borrador** · Se apoya en: <specs anteriores aprobadas, o "—">

<!--
  La spec dice QUÉ y POR QUÉ, nunca CÓMO.
  NO va acá: framework, librerías, arquitectura, nombres de archivos o clases, diseño de tablas,
  algoritmos, estimaciones de tiempo, ni adjetivos sin número ("rápido", "escalable").
  Todo eso es del plan.
-->

## Contexto y objetivo

<!-- 1–2 párrafos: qué problema hay hoy y qué cambia cuando esto exista. -->

## Usuarios

| Rol | Qué hace |
|---|---|
| <rol> | <qué necesita hacer> |

## Historias de usuario

- **HU-1.** Como <rol>, quiero <acción>, para <beneficio>.
  *Aceptación:* <resultado observable> (RF-…).

## Requisitos funcionales (en EARS)

<!-- Agrupalos por tema. Cada RF con condición y resultado observables (ver EARS en la skill sdd).
     Incluí los códigos/estados/mensajes exactos cuando importen. -->

**<Grupo>**

- **RF-1:** CUANDO <evento>, EL SISTEMA <resultado observable>.
- **RF-2:** SI <condición no deseada>, ENTONCES EL SISTEMA <respuesta>.

## Requisitos no funcionales

<!-- Medibles: un número, un límite, una regla verificable. -->

- **RNF-1:** <requisito con número o regla comprobable>.

## Casos límite

| # | Situación | Esperado |
|---|---|---|
| 1 | <entrada vacía / inexistente / duplicada / concurrente / sin permiso…> | <resultado exacto> |

## Fuera de alcance

- <lo que explícitamente NO se hace en esta feature>

## Criterios de finalización

- Cada RF y cada caso límite con test automatizado.
- Gates en verde (los de `docs/constitution.md`).
- <otros: documentación, PR mergeado, etc.>

## Decisiones de la entrevista (<fecha>)

1. **<Tema>:** <respuesta del usuario>.

## Dudas abiertas

<!-- Una spec NO se aprueba con dudas abiertas. -->

- Ninguna.  <!-- o: [NECESITA ACLARACIÓN] <pregunta> -->
