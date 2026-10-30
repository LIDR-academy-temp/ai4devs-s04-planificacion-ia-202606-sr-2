# Prompt: extracción de user stories del PRD de FlowSync (MVP)

> Copia todo lo que hay debajo de la línea `---` y pégalo en la IA, sustituyendo `[PEGAR AQUÍ EL CONTENIDO DE docs/PRD.md]` por el PRD completo.

---

## Rol

Eres un **Product Owner senior** con experiencia en aplicaciones SaaS B2C y B2B. Has definido backlogs para productos web de gestión de tareas e integraciones con terceros. Tu trabajo es traducir un PRD a user stories claras, verificables y listas para refinamiento con el equipo de ingeniería y QA.

## Contexto

**FlowSync** es una aplicación web de gestión de tareas personales que mantiene sincronizadas las tareas del usuario con su **Google Calendar**. Está pensada para **profesionales del conocimiento de 25 a 45 años** que ya usan Google Calendar a diario y gestionan sus pendientes en otra herramienta. Su propuesta diferenciadora es que las tareas con fecha límite aparezcan como eventos en el calendario, sin copia manual.

El **alcance de este ejercicio es exclusivamente el MVP**, es decir, la sección **«Qué incluye el MVP»** del PRD y los requisitos funcionales de la sección **«3. Requisitos funcionales»** (subsecciones 3.1 a 3.5). El MVP incluye:

- Registro y autenticación de usuarios.
- CRUD de tareas con estados (`pending`, `completed`, `archived`).
- Filtrado y organización básica.
- Exportación de tareas a CSV.
- Sincronización con Google Calendar (lectura y escritura), con la dirección principal FlowSync → Google Calendar.

Las **secciones 4 (requisitos no funcionales), 6 (criterios de éxito) y 7 (riesgos)** son contexto. Úsalas para afinar criterios de aceptación, pero no generes stories a partir de ellas.

Lo que **no** forma parte del MVP y **no debe aparecer como story**: equipos o tareas compartidas, calendarios distintos de Google (Outlook, iCal), notificaciones push o por email, app móvil nativa, etiquetas, proyectos, subtareas y recordatorios configurables.

## Restricciones

Cumple estas restricciones sin excepción:

1. **No inventes features.** Cada story debe poder rastrearse a un requisito concreto del PRD. Si una funcionalidad no está escrita en el documento, no la añadas, aunque parezca lógica o habitual en este tipo de producto.
2. **No estimes tiempos ni puntos** (ni horas, ni story points, ni tallas de camiseta).
3. **No propongas arquitectura, stack, endpoints, esquemas de base de datos ni diseños de UI.** El PRD indica que el «cómo» se define en las specs posteriores. Las stories describen el «qué» y el «para qué».
4. **No reescribas los requisitos como especificación técnica.** Usa el lenguaje de usuario, no el de desarrollador.
5. **Respeta los matices del PRD.** Por ejemplo, la sincronización inversa (Google → FlowSync) no está comprometida en el MVP: si la mencionas, debe ser solo como duda abierta, nunca como story.

## Formato de salida

### Formato de cada story

Cada story debe seguir **exactamente** esta frase, con los tres huecos rellenos:

`Como [rol], quiero [acción], para [beneficio]`

- **Rol**: usa `visitante` (antes de registrarse) o `usuario` (usuario registrado). No inventes otros roles.
- **Acción**: concreta y verificable, sin ambigüedad.
- **Beneficio**: el valor para el usuario, tomado del PRD (por ejemplo, el *job to be done* de la sección 2).

### Criterios de aceptación

Cada story debe tener **entre 3 y 5 criterios de aceptación**, escritos en formato **Given / When / Then**:

- **Given**: el estado inicial o precondición.
- **When**: la acción concreta del usuario o del sistema.
- **Then**: el resultado observable y verificable.

Los criterios deben ser **específicos y comprobables**. Evita frases genéricas como «el sistema funciona correctamente», «la interfaz es intuitiva» o «los datos se guardan bien». Un buen criterio permite que QA escriba un test a partir de él sin preguntar nada más.

Incluye, cuando el PRD lo indique, los casos límite y los errores: email ya registrado, contraseña de menos de 8 caracteres, tarea sin fecha límite, Google API no disponible, usuario sin tareas, etc.

### Agrupación

Agrupa las stories por **épica**, usando estos módulos del PRD como épicas (puedes renombrarlas si mejora la claridad, pero no añadas épicas que no tengan respaldo en el documento):

1. Autenticación y gestión de cuenta (PRD 3.1)
2. Gestión de tareas (PRD 3.2)
3. Organización y filtrado (PRD 3.3)
4. Exportación (PRD 3.4)
5. Sincronización con Google Calendar (PRD 3.5)

Dentro de cada épica, numera las stories con el prefijo de la épica: `AUTH-01`, `TAR-01`, `ORG-01`, `EXP-01`, `SYNC-01`, etc.

### Cantidad

Genera **entre 8 y 12 stories** en total, repartidas entre las épicas según el peso que tengan en el PRD. La épica de sincronización con Google Calendar es la más compleja: no la dejes sin stories, pero no la sobredimensiones.

### Estructura de la respuesta

Responde únicamente con esta estructura, en Markdown:

```
## Épica 1: [nombre]

### [ID] · [título corto de la story]

**Story:** Como [rol], quiero [acción], para [beneficio].

**Criterios de aceptación:**
- **Given** … **When** … **Then** …
- **Given** … **When** … **Then** …
- **Given** … **When** … **Then** …

(… repetir para cada story …)

## Épica 2: …
```

Al final, añade una sección:

```
## Dudas abiertas

- [Lista corta de puntos que el PRD deja sin cerrar y que impiden escribir una story completa, por ejemplo: criterio exacto de ordenación del listado, alcance de la sincronización inversa, zona horaria de los eventos.]
```

No añadas introducciones, conclusiones ni comentarios fuera de esta estructura.

## Transparencia

Si una story o un criterio de aceptación contiene **cualquier información que no esté literal en el PRD** (un supuesto, una deducción, un valor por defecto, un comportamiento inferido), márcalo con la etiqueta **`(asumido)`** justo al final de la frase afectada.

Ejemplos de lo que debes marcar:

- Un valor que el PRD no concreta (por ejemplo, la hora a la que se crea un evento de una tarea con solo fecha).
- Un comportamiento deducido de una frase general (por ejemplo, que al completar una tarea su evento se elimine en lugar de marcarse, si el PRD lo deja a criterio).
- Un caso límite que no aparece en el documento pero que se desprende lógicamente de él.

No marques lo que el PRD dice literalmente. La etiqueta sirve para que el equipo sepa qué debe validar.

## Ejemplos de referencia

Estas dos stories están escritas a mano y muestran el nivel de detalle esperado. **No las copies**: úsalas solo como modelo de formato y de calidad.

---

## Épica 2: Gestión de tareas

### TAR-01 · Marcar una tarea como completada

**Story:** Como usuario, quiero marcar una tarea como completada, para dejar de tener pendientes las cosas que ya he hecho.

**Criterios de aceptación:**
- **Given** una tarea en estado `pending`, **When** el usuario la marca como completada, **Then** su estado pasa a `completed`.
- **Given** una tarea en estado `completed`, **When** el usuario aplica el filtro «completadas», **Then** la tarea aparece en el listado.
- **Given** una tarea en estado `pending`, **When** el usuario aplica el filtro «pendientes», **Then** la tarea ya no aparece en el listado. *(asumido: el filtro por estado se aplica sobre el listado completo, según PRD 3.3)*

---

## Épica 5: Sincronización con Google Calendar

### SYNC-01 · Ver las tareas con fecha límite como eventos en Google Calendar

**Story:** Como usuario, quiero que mis tareas con fecha límite aparezcan como eventos en mi Google Calendar, para saber qué tengo que hacer hoy sin mirar dos sitios distintos.

**Criterios de aceptación:**
- **Given** una cuenta de Google conectada a FlowSync, **When** el usuario guarda una tarea con fecha límite, **Then** aparece un evento en su Google Calendar en la fecha de esa fecha límite. *(asumido: la hora del evento se define en la spec de zonas horarias; ver Dudas abiertas)*
- **Given** una cuenta de Google conectada, **When** el usuario guarda una tarea sin fecha límite, **Then** no se crea ningún evento en su Google Calendar.
- **Given** una tarea con evento ya sincronizado, **When** el usuario cambia su fecha límite en FlowSync, **Then** el evento correspondiente en Google Calendar se actualiza a la nueva fecha.
- **Given** la API de Google no disponible, **When** el usuario guarda una tarea con fecha límite, **Then** la tarea queda guardada en FlowSync y la sincronización se reintenta más tarde.

---

## Tarea

Ahora, a partir del PRD que está en docs/PRD.md, genera las user stories siguiendo todas las reglas anteriores.

**PRD de FlowSync:**

[PEGAR AQUÍ EL CONTENIDO DE docs/PRD.md]
