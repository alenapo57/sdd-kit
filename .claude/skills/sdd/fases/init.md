# Fase: configurar el proyecto (`/sdd-init`)

**Objetivo:** dejar el repo listo para trabajar con SDD, adaptado al proyecto del usuario:
constitución, gates, `CLAUDE.md`, permisos y (si lo quiere) CI. Se hace **una vez**; se puede
volver a correr para cambiar algo.

## 0. Antes de preguntar nada

- Si `docs/constitution.md` ya no tiene `[COMPLETAR]`, el proyecto ya está configurado: mostrá
  un resumen de la configuración actual y preguntá qué quiere cambiar.
- Mirá qué hay en el repo. Si ya hay código, leé lo necesario para **proponer** respuestas en vez
  de preguntar a ciegas: manifiestos (`package.json`, `pyproject.toml`, `requirements.txt`,
  `pom.xml`, `build.gradle`, `go.mod`, `Cargo.toml`, `*.csproj`…), configuración de tests y de
  lint, README, carpeta de tests, workflows existentes.
- Creá el archivo de tarea `.claude/tareas/000-init.md` (plantilla `docs/plantillas/tarea.md`).

## 1. Entrevista

Preguntá **solo lo que no se puede deducir**, con opciones concretas y **la recomendada primero**
(con su porqué en una línea). Si tenés la herramienta de preguntas, agrupá hasta 4 por vez.

1. **El proyecto**: qué es, para quién, y si arranca de cero o ya hay código.
2. **Lenguaje y framework**: si arranca de cero, recomendá uno adecuado al objetivo y a lo que el
   usuario ya sabe, y explicá por qué.
3. **Herramientas**: gestor de paquetes / build, framework de tests (unitarios y de
   integración/e2e), linter y formateador. Proponé los estándar del ecosistema.
4. **Gates**: la lista final de comandos que tienen que dar verde para cerrar una tarea (lint,
   build o chequeo de tipos, tests unitarios, tests de integración/e2e y, opcional, cobertura con
   un umbral). Explicá qué protege cada uno.
5. **Idioma**: de la documentación y las respuestas del agente; del código e identificadores.
6. **Arquitectura y calidad** (principios 3 y 4 de la constitución): proponé lo habitual del
   stack (capas, tipado estricto, sin atajos del compilador…).
7. **GitHub**: ¿usa GitHub? ¿quiere CI con los gates en cada PR? ¿quiere proteger `main` (PR
   obligatorio + check `gates` en verde)? Avisá el límite: en una **organización con plan Free** la
   protección de ramas solo se aplica en **repos públicos** (en un repo personal también funciona
   con el repo público; para privado hace falta un plan pago).
8. **Commits automáticos por fase**: ¿autoriza al agente a commitear al cerrar cada fase o tarea
   sin pedir permiso cada vez? Explicá que el **push siempre** pide OK.

## 2. Proponer y PARAR

Mostrá una tabla con la configuración resultante (stack, cada gate con su comando, idioma, reglas
de arquitectura y calidad, CI sí/no, protección de `main`, commits automáticos sí/no) y
**esperá la aprobación**.

## 3. Aplicar (solo con la aprobación)

1. **`docs/constitution.md`**: reemplazá cada `[COMPLETAR]` (nombre, principios 3, 4, 5 y 6,
   tabla de gates con los comandos reales). No toques los principios 1, 2 y 7 sin pedirlo.
2. **`CLAUDE.md`**:
   - "Proyecto": qué es, stack, estructura de carpetas, convenciones.
   - "Comandos y gates": un comando por gate y cómo levantar la app.
   - "Commits, ramas y push": si **no** autorizó commits automáticos, reemplazá esa parte por
     "Pedí permiso antes de cada commit" y dejá el resto (ramas, push con OK).
   - Idioma, si no es español.
3. **`.claude/settings.json`**: agregá a `permissions.allow` los comandos de los gates (forma
   `"Bash(<comando> *)"` o exacta). Si no autorizó commits automáticos, mové `"Bash(git commit *)"`
   de `allow` a `ask`.
4. **`.gitignore`**: agregá lo propio del stack (dependencias, builds, cobertura, entornos
   virtuales, bases de datos locales…).
5. **CI** (si lo eligió): copiá `docs/plantillas/ci.yml` a `.github/workflows/ci.yml` y completá:
   - el bloque de preparación con las **acciones oficiales** del stack (`actions/setup-node`,
     `actions/setup-python`, `actions/setup-java`, `actions/setup-go`…) con versión fija y caché;
   - la instalación de dependencias **con el lockfile congelado** (`npm ci`, `pnpm install
     --frozen-lockfile`, `pip install -r requirements.txt`, `uv sync --locked`…);
   - **un paso por gate**, con el mismo comando que en la constitución.
   El job se llama `gates`: es el nombre del check que se va a exigir en `main`.
6. **Base del proyecto** (si arranca de cero y el usuario quiere): generá el esqueleto con el
   **generador oficial** del framework, agregá **un test mínimo** para que los gates tengan algo
   que correr y verificá que todos den verde. Si el usuario prefiere construir la base con el
   flujo completo, dejala como primera feature (`001-base`).

## 4. Verificar

- Si hay código: corré **cada gate** y mostrá el resultado real. Si un comando no existe o falla,
  corregí la configuración (no la constitución) y avisá.
- Si creaste el CI: validá que el YAML parsea y que los comandos coinciden con la constitución.
- Releé `CLAUDE.md` y `docs/constitution.md`: no puede quedar ningún `[COMPLETAR]`.

## 5. Cerrar

1. `MEMORY.md`: entrada `000: configuración` con las decisiones y su porqué.
2. Commit `000: configuración del flujo SDD` (si autorizó commits; si no, pedí permiso).
3. Si usa GitHub: pedí OK para el push y explicá los pasos para **proteger `main`** según
   `docs/guia-github.md` (los hace el usuario en la web).
4. Contale cómo seguir: `/sdd-spec 001-nombre "idea"` y que lea `docs/GUIA.md` si no lo hizo.
