# Guía del flujo SDD (empezá por acá)

Esta guía explica **cómo trabajar con un agente de IA de forma ordenada** y, sobre todo, **por qué**
cada paso existe. Leela una vez antes de tu primera feature (15 minutos) y volvé cuando tengas
dudas. Al lado tenés el [ejemplo completo](ejemplo/001-gastos/) de una feature hecha de punta a
punta.

---

## 1. La idea en una página

Un agente de IA escribe código muy rápido, pero cuando algo no está claro **adivina**, y adivina
con mucha seguridad. El resultado típico: código que compila, tests que pasan, y un comportamiento
que no era el que querías, o un caso que nadie pensó y explota en producción.

**Spec-Driven Development (SDD)** ataca eso con una regla simple: **antes de escribir código, se
escribe qué tiene que hacer el sistema, se revisa buscando huecos y se acuerda.** Recién después
se decide cómo, se parte en tareas chicas y se implementa de a una, con los tests primero.

```
/sdd-init (una vez)
   │
   ▼
spec ──► clarify ──► plan ──► tasks ──► implement T1 … Tn ──► validate ──► PR ──► merge
  qué y     buscar     cómo      tareas     test rojo → código     evidencia     vos
 por qué    huecos               chicas     → gates → commit       RF por RF    mergeás
```

En **cada flecha** el agente para y espera tu aprobación. Eso no es burocracia: es el momento en
que **vos** decidís, con la información a la vista, en lugar de enterarte después.

> **La IA ejecuta, pero el responsable del proyecto sos vos.**

## 2. Quién hace qué

| Rol | Hace | No hace |
|---|---|---|
| **Vos** | Decidís qué se construye, respondés las preguntas, aprobás cada fase, mergeás | Escribir todo a mano (podés, pero no hace falta) |
| **El agente** | Pregunta, escribe specs, planes, tests y código, corre los gates, explica sus decisiones, commitea | Pasar de fase sin tu OK, pushear sin tu OK, arreglar cosas que no le pediste |
| **El revisor** | Busca problemas en la spec (clarify) y en la feature terminada (validate) | Arreglarlos: solo los detecta, y vos decidís |

El revisor puede ser el mismo agente cambiando de rol, o el subagente `sdd-reviewer` si querés una
mirada que no participó de escribir el código ("hacé la validación con el revisor independiente").

## 3. Antes de empezar: `/sdd-init`

El agente te entrevista sobre tu proyecto y configura todo. Lo más importante que vas a decidir son
los **gates**: los comandos que tienen que dar verde para que una tarea se considere terminada
(lint, build o tipos, tests unitarios, tests de integración y, si querés, cobertura con un umbral).

¿Por qué importan tanto? Porque son la **definición objetiva de "terminado"**. Sin gates, "ya
está" significa "el agente cree que está". Con gates, significa "estos comandos pasaron".

También vas a decidir si el agente puede **commitear solo** al cerrar cada fase (recomendado: el
historial queda prolijo, una tarea por commit) y si querés **CI** en GitHub con `main` protegido.

## 4. Las fases, una por una

### Spec: qué y por qué (`/sdd-spec 001-nombre "idea"`)

**Produce** `specs/001-nombre/spec.md`: contexto, usuarios, historias de usuario, requisitos
funcionales en **EARS**, requisitos no funcionales medibles, casos límite numerados y lo que queda
fuera de alcance.

**Por qué existe:** separa la conversación sobre *qué querés* de la conversación sobre *cómo
hacerlo*. Si mezclás las dos, la tecnología termina decidiendo el producto.

**EARS** es una forma de escribir requisitos que se pueden testear:
*"CUANDO se registra un gasto válido, EL SISTEMA responde 201 con el gasto y su id"*. Si un
requisito no tiene una condición y un resultado que se puedan observar, no se puede testear, y lo
que no se puede testear no se puede dar por cumplido.

**Antes de aprobar, revisá:**
- ¿Es lo que querés? Leé los requisitos, no solo el resumen.
- ¿Los casos límite cubren lo raro de tu dominio (vacíos, duplicados, dos usuarios a la vez, sin
  permiso, algo que falla a mitad de camino)?
- ¿"Fuera de alcance" deja afuera lo que querías afuera?
- ¿Se coló alguna palabra técnica (framework, base de datos)? En la spec no va.

### Clarify: buscar los huecos (`/sdd-clarify`)

**Produce** `clarify.md` en **dos rondas**:
1. **Ronda 1:** el agente hace de QA y lista **solo problemas**, sin soluciones: ambigüedades,
   contradicciones, casos sin cubrir, requisitos que no se pueden testear.
2. En el chat te propone **una resolución por problema**. Vos aprobás, ajustás o descartás.
3. Incorpora lo aprobado a la spec y hace la **ronda 2**: verifica que cada problema quedó resuelto.

**Por qué existe:** un hueco en la spec cuesta una línea ahora y una tarde de debugging después.
Y se separan detección y solución a propósito: si el revisor propone arreglos mientras busca, deja
de buscar.

**Señal de alarma:** una ronda 1 sin ningún hallazgo. Casi siempre hay algo (en nuestras features
reales, entre 9 y 14 detecciones por spec).

### Plan: cómo (`/sdd-plan`)

**Produce** `plan.md`: dependencias nuevas, **hallazgos verificados**, estructura de archivos,
decisiones con su porqué, estrategia de tests y riesgos. Normalmente viene junto con las tareas.

**Por qué existe:** las decisiones técnicas tienen consecuencias, y escribirlas con su razón
permite discutirlas antes de que estén enterradas en el código.

**Lo más valioso del plan son los hallazgos verificados**: cosas que el agente comprobó leyendo la
documentación o el código de una librería, en lugar de suponerlas. Ejemplo real: validar "hasta 2
decimales" multiplicando por 100 parece obvio, pero en JavaScript `0.29 * 100` da
`28.999999999999996`, y esa validación rechazaría un monto válido. Comprobarlo antes de diseñar
evitó un bug que los tests "típicos" no habrían encontrado.

**Antes de aprobar, revisá:** las dependencias nuevas (¿hacen falta?), las decisiones que no
entiendas (pedí que te las explique) y los riesgos.

### Tareas (`/sdd-tasks`)

**Produce** `tasks.md`: hasta **10 tareas** de 15–45 minutos, en orden, cada una con los
requisitos que cubre y un **"Hecho cuando:"** con un comando concreto.

**Por qué existe:** una tarea chica se puede revisar, testear y deshacer. Una tarea de "implementar
todo" no. Si no entran en 10, la feature es demasiado grande: se parte.

### Implementar UNA tarea (`/sdd-implement 001-nombre T3` o "seguí con T3")

El ciclo, siempre igual:

1. **Test en rojo:** primero el test, y verlo fallar *por el motivo correcto*.
2. **Código mínimo** para que pase.
3. **Gates en verde**: todos.
4. `[x]` en la tarea, entrada en `MEMORY.md`, **commit** `001-T3: …`.
5. **Para** y te cuenta qué hizo, qué decidió y qué no pudo verificar.

**¿Por qué el test primero?** Un test escrito después del código tiende a describir lo que el
código hace, no lo que debería hacer. Verlo fallar primero demuestra que el test realmente prueba
algo.

**¿Y si el test pasa de entrada?** Pasa cuando el test caracteriza algo que ya existía. Ahí se usa
una **mutación**: se rompe el código a propósito y se verifica que el test lo detecte. Ejemplo
real: en un sistema de reservas, un test de concurrencia pasaba en verde. Al sacar a propósito el
candado que serializa las reservas, el mismo test mostró **12 entradas vendidas de un cupo de 5**.
Eso prueba que el test sirve y que el candado es necesario.

**Cuándo no alcanza con los tests:** si la tarea toca el arranque, la configuración o los logs, el
agente levanta **la app real** y la prueba. En un caso real, así se descubrió que el framework, al
fallar el arranque, abortaba el proceso sin escribir ningún error; ningún test lo veía.

### Validar (`/sdd-validate`)

**Produce** `validate.md`, con el **veredicto en la primera línea** y la evidencia: los gates
corridos en ese momento, y una tabla **requisito → test que lo cubre → resultado**.

**Un requisito sin test que lo cubra no está implementado**, aunque el código exista.

**Si la validación encuentra un problema, no se arregla en silencio**: se propone como **tarea
nueva**, vos la aprobás y se implementa con su test en rojo. Ejemplo real: una validación encontró
que unas consultas escribían los ids directamente en el texto del SQL en lugar de pasarlos como
parámetros, contra lo que pedía la spec. Se agregó una tarea con un test que registraba el SQL
ejecutado y fallaba si había un número literal: 11 consultas en rojo, después todas en verde.

Después del veredicto: push (con tu OK), PR, CI en verde, **vos mergeás**, y el agente verifica la
corrida en `main`.

### Cambiar un requisito (`/sdd-change`)

Querés algo distinto a lo aprobado: **primero se cambia la spec** (con su impacto en el plan y las
tareas), lo aprobás, y recién después el código. Nunca se borra la historia: las tareas cerradas
quedan, y el cambio queda anotado con su fecha. Si el cambio es grande, conviene una feature nueva.

## 5. Cómo hablarle al agente

| Querés… | Decí… |
|---|---|
| aprobar y seguir | "Aprobada, seguí con el clarify" · "dale, seguí con T4" |
| aprobar con un ajuste | "Aprobado, pero el límite es 500 y no 200" |
| entender una decisión | "¿Por qué elegiste X y no Y? Explicámelo simple" |
| saber qué quedó flojo | "¿Qué no pudiste verificar?" |
| retomar otro día | "Retomemos la 002" · `/sdd-status` |
| cortar algo que se está desviando | "Pará. Eso no está en la spec" |

**Pedí siempre el porqué.** El agente está configurado para explicarlo, y es lo que te permite
aprobar con criterio y aprender en el camino.

## 6. Retomar en otra sesión

El agente lleva dos registros:
- **`.claude/tareas/<feature>.md`** (local): el "dónde quedé" de la tarea en curso, con los
  commits, las decisiones y **qué sigue**. Una sesión nueva lo lee y continúa sin volver a
  preguntar.
- **`MEMORY.md`** (se commitea): lo que el proyecto aprendió, una entrada por tarea. Decisiones,
  trampas, números.

Las conversaciones largas se resumen solas y se pierden detalles; estos archivos no.

## 7. Git y GitHub en el flujo

Cada feature en su **rama** (`001-nombre`) → un **commit por tarea** → push con tu OK → **PR** con
el CI → **vos mergeás**. Con `main` protegido, un PR con los gates en rojo no se puede mergear.
Detalles en [`guia-github.md`](guia-github.md).

## 8. Correcciones: equivocarse está bien, esconderlo no

Las suposiciones fallan. Cuando el agente descubre que algo del plan o la spec era falso, lo
corrige **por escrito** ("Corrección en T2: …"). Ejemplo real: un plan decía que un componente
perdía el contexto de cada pedido y que había que reordenar el código para evitarlo. Una mutación
mostró que no era así: se corrigió el plan, se simplificó el código y quedó un test que lo protege.
Un historial con correcciones explícitas vale más que uno que parece perfecto.

## 9. Errores comunes

| Error | Qué pasa | Cómo evitarlo |
|---|---|---|
| Aprobar sin leer | Se construye otra cosa | Leé los requisitos y los casos límite, no solo el resumen |
| Tareas enormes | No se pueden revisar ni deshacer | Máximo 10 tareas de 15–45 min; si no entra, partí la spec |
| Saltear el clarify | Los huecos aparecen como bugs | Siempre clarify; desconfiá de una ronda 1 vacía |
| Test escrito después | El test confirma el código en vez de la spec | Rojo primero, o mutación |
| "Ya está" sin gates | Algo se rompió en otro lado | Todos los gates, siempre, con los números reales |
| Arreglar "de paso" | Cambios que nadie aprobó ni testeó | Anotarlo y avisar; si importa, tarea nueva |
| Borrar un test que molesta | La puerta deja de cuidar | Prohibido. Si el requisito cambió, `/sdd-change` |
| Secretos en el repo | Quedan para siempre en el historial | Solo `.env` (ignorado) y secrets de GitHub |
| Perder el hilo entre sesiones | Se repiten preguntas y errores | Archivo de tarea + `MEMORY.md` + `/sdd-status` |

## 10. Glosario

- **Gate**: un comando que tiene que dar verde para cerrar una tarea (lint, tests…).
- **Rojo / verde**: test fallando / pasando.
- **Mutación**: romper el código a propósito para comprobar que un test lo detecta.
- **EARS**: forma de escribir requisitos con condición y resultado observables.
- **Caso límite**: situación en el borde (vacío, máximo, duplicado, simultáneo, sin permiso…).
- **Hallazgo**: problema que aparece durante la validación; se convierte en tarea nueva.
- **Constitución**: reglas del proyecto que no se negocian (`docs/constitution.md`).
- **PR / check requerido**: pedido de merge / verificación de CI que tiene que estar en verde para
  mergear.
- **Idempotente**: hacerlo dos veces deja el mismo resultado que una (por ejemplo, cancelar algo ya
  cancelado).
