# Constitución — [COMPLETAR: nombre del proyecto]

Principios innegociables del proyecto. **Toda spec, plan y tarea tiene que cumplirlos.**
Este archivo no se modifica sin acuerdo explícito del responsable del proyecto.

<!-- /sdd-init completa los [COMPLETAR] con las respuestas de la entrevista. Los principios 1, 2,
     5 y 7 son del flujo y conviene no tocarlos; el 3, 4 y 6 dependen del stack. -->

1. **La spec manda.** Nada se implementa si no está en una spec aprobada. Un requisito nuevo o
   cambiado pasa primero por la spec (`/sdd-change`), después por el código.

2. **Los tests son la puerta.** Ninguna tarea se da por hecha con un gate en rojo. Los tests se
   escriben primero y se ven fallar. Prohibido borrar, comentar o debilitar un test para que la
   suite pase.

3. **Arquitectura.** [COMPLETAR: cómo se organiza el código. Ej.: "capas: la entrada (HTTP/CLI/UI)
   solo traduce; la lógica vive en servicios; el acceso a datos, en repositorios".]

4. **Tipos y calidad.** [COMPLETAR: reglas del lenguaje. Ej. TypeScript: "sin `any`, sin
   `@ts-ignore`, sin casts forzados"; Python: "type hints y mypy sin errores".]

5. **La seguridad no se negocia.** Secretos solo en `.env` o en el gestor de secretos (nunca en el
   repo ni en una imagen). Ningún dato sensible en respuestas ni en logs. [COMPLETAR si aplica:
   contraseñas hasheadas, endpoints protegidos por defecto…]

6. **Idioma.** [COMPLETAR. Ej.: "código e identificadores en inglés; documentación, comentarios y
   textos visibles al usuario en español".]

7. **Dependencias.** Ninguna dependencia nueva sin que el plan la liste y el usuario la apruebe.

---

## Gates de verificación

<!-- Un comando por fila. Una tarea se cierra solo con TODOS en verde. -->

| Gate | Comando | Cuándo |
|---|---|---|
| [COMPLETAR] Lint | `[COMPLETAR]` | Cada tarea |
| [COMPLETAR] Build / tipos | `[COMPLETAR]` | Cada tarea |
| [COMPLETAR] Tests unitarios | `[COMPLETAR]` | Cada tarea (tests primero) |
| [COMPLETAR] Tests de integración / e2e | `[COMPLETAR]` | Cada tarea que toque una interfaz externa |
| [COMPLETAR, opcional] Cobertura | `[COMPLETAR]` (umbral: [COMPLETAR] %) | Cada tarea |
