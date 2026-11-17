# Guía de GitHub para el flujo SDD

Cómo se conecta el flujo con GitHub: ramas por feature, pull requests, CI como puerta y `main`
protegido. Todo esto lo hace el **usuario** en la web; el agente lo explica y lo verifica.

## 1. Crear el repo desde la plantilla

1. En el repo del kit: **"Use this template" → "Create a new repository"**.
2. Cloná tu repo nuevo, abrí Claude Code adentro y corré `/sdd-init`.
3. Si `/sdd-init` creó `.github/workflows/ci.yml`, el primer push ya dispara la corrida `CI`.

> Para quien mantiene el kit: el repo del kit se marca como plantilla en **Settings → General →
> "Template repository"**.

## 2. Ramas y pull requests

- Cada feature en su rama: `NNN-nombre` (la crea `/sdd-spec`).
- Los commits los hace el agente en los momentos que autoriza `CLAUDE.md`; el **push** siempre con
  tu OK (o con una autorización explícita para una rama y un período, que queda en el archivo de
  tarea).
- Al validar: push de la rama → abrís el PR (link `https://github.com/<vos>/<repo>/pull/new/NNN-nombre`)
  → el CI corre → si está verde, **mergeás vos** → el agente sincroniza `main` y verifica la
  corrida en `main`.

## 3. Proteger `main` (para que el CI sea una puerta de verdad)

Sin protección, el CI solo **avisa**: se puede mergear con rojo o pushear directo a `main`. Con
protección, un PR con `gates` en rojo **no se puede mergear**, ni siquiera siendo administrador.

1. Hacé al menos **una corrida** del CI (así GitHub conoce el check `gates`).
2. **Settings → Rules → Rulesets → New branch ruleset** (o **Settings → Branches → Add branch
   protection rule**, la forma clásica):
   - Target: la rama `main`. Enforcement: **Active**.
   - **Require a pull request before merging**.
   - **Require status checks to pass** → agregá **`gates`**.
   - **Sin excepciones**: no agregues a nadie en "Bypass list" (en la forma clásica: tildá
     "Do not allow bypassing the above settings").
   - Opcional: **Block force pushes**.
3. Probalo: abrí un PR con un test que falle a propósito y verificá que el botón de merge queda
   bloqueado. Cerralo sin mergear. Es la mejor evidencia de que la puerta funciona.

> **Límite del plan gratuito:** la protección de ramas **solo se aplica en repos públicos** con el
> plan Free (personal u organización). En un repo **privado** la regla queda guardada pero "not
> enforced" hasta tener un plan pago. Si hacés privado un repo público, la protección deja de
> aplicarse.

## 4. Secretos

- Nunca en el código ni en el repo: van en **Settings → Secrets and variables → Actions** y el
  workflow los lee con `${{ secrets.NOMBRE }}` (GitHub los oculta en los logs).
- `GITHUB_TOKEN` lo crea GitHub en cada corrida y vence al terminar; dale el **mínimo** de permisos
  con `permissions:` (por defecto `contents: read`; `packages: write` solo en el job que publica).
- Para los tests en el CI, generá los valores en la corrida (por ejemplo, `openssl rand -hex 32`)
  en lugar de guardarlos.

## 5. Leer una corrida que falló

- En el PR, **"Details"** al lado del check abre la corrida: el paso en rojo es el que falló y los
  siguientes no corren.
- **Los logs completos** se descargan desde la web (menú de la corrida → "Download log archive").
  La API de GitHub solo los entrega a administradores del repo, así que si el agente no puede
  leerlos, bajalos vos y pasáselos.
- La API pública sin token tiene un límite de **60 consultas por hora**: si el agente sigue muchas
  corridas, puede quedarse sin consultas un rato (la página web de la corrida sigue respondiendo).

## 6. Paquetes e imágenes (si el proyecto publica en `ghcr.io`)

- Un paquete creado por una corrida queda vinculado al repo si la imagen tiene la etiqueta
  `org.opencontainers.image.source` con la URL del repo.
- **En organizaciones, los paquetes suelen crearse privados** y la organización puede prohibir los
  públicos (lo ves en *Package settings → Change visibility*). Para bajarlos hace falta
  `docker login ghcr.io` con un token `read:packages`. Documentá siempre un camino que no necesite
  credenciales (por ejemplo, construir desde el código).
