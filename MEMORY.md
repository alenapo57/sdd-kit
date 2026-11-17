# MEMORY.md — lo que este proyecto aprendió

> Se actualiza **al cerrar cada tarea** (lo exige el flujo). Es el historial que lee una sesión
> nueva y el revisor: qué se hizo, **qué se decidió y por qué**, y qué trampas no hay que repetir.
> Entradas cortas, en orden cronológico. Lo de una sola sesión va al archivo de tarea, no acá.

## Estado de las features

<!-- Una línea por feature: NNN, estado (en curso / validada / cerrada), rama o PR. -->

- (ninguna todavía)

## Entradas

<!-- Formato:
- <fecha> · NNN-Tn: <qué se hizo>. Decisión: <qué y por qué>. Trampa: <si hubo>.
  Gates: <N tests unit, N e2e, cobertura x% / y%>.

Ejemplo:
- 16/11 · 001-T2: validación de montos en el DTO (RF-3). Decisión: rechazar montos con más de
  2 decimales en vez de redondear, porque redondear escondía errores de carga (caso 4).
  Trampa: el parser de JSON convierte "10.10" en 10.1 — el test compara números, no textos.
  Gates: 42 unit, 18 e2e, cobertura 88% / 81%.
-->
