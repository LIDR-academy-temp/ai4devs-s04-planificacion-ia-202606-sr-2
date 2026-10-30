## Épica 1: Autenticación y gestión de cuenta

### AUTH-01 · Registro de cuenta con email y contraseña

**Story:** Como visitante, quiero crear una cuenta con mi email y una contraseña, para tener un espacio propio donde guardar mis tareas.

**Criterios de aceptación:**
- **Given** un visitante sin cuenta, **When** introduce un email con formato válido y una contraseña de 8 o más caracteres y envía el formulario, **Then** se crea la cuenta, queda autenticado y ve la pantalla de bienvenida que le invita a crear su primera tarea.
- **Given** un visitante, **When** introduce una contraseña de 7 caracteres, **Then** no se crea la cuenta y se muestra un mensaje que indica que la contraseña debe tener al menos 8 caracteres.
- **Given** un email que ya está registrado, **When** un visitante intenta registrarse con ese email, **Then** no se crea una cuenta nueva, se indica que el email ya existe y se ofrece un enlace al inicio de sesión.
- **Given** un visitante, **When** introduce un email con formato inválido, **Then** se muestra un mensaje comprensible sobre el email y no se crea la cuenta.

### AUTH-02 · Inicio y cierre de sesión

**Story:** Como usuario, quiero iniciar y cerrar sesión con mi email y contraseña, para acceder a mis tareas y poder salir de mi cuenta cuando quiera.

**Criterios de aceptación:**
- **Given** un usuario registrado, **When** introduce su email y contraseña correctos, **Then** accede a su listado de tareas.
- **Given** un usuario registrado, **When** introduce una contraseña incorrecta, **Then** no accede y ve un mensaje de error que no revela si el email existe. *(asumido: el PRD no especifica el texto del error, solo que debe ser comprensible)*
- **Given** un usuario autenticado, **When** cierra sesión, **Then** vuelve a la pantalla de inicio de sesión y deja de poder ver sus tareas sin autenticarse de nuevo. *(asumido: el token de acceso queda invalidado al cerrar sesión)*

## Épica 2: Gestión de tareas

### TAR-01 · Crear una tarea

**Story:** Como usuario, quiero crear una tarea indicando al menos su título, para tener mis pendientes en FlowSync en lugar de en otra herramienta.

**Criterios de aceptación:**
- **Given** un usuario autenticado, **When** crea una tarea solo con título, **Then** la tarea aparece en su listado con estado `pending`, sin descripción y sin fecha límite.
- **Given** un usuario autenticado, **When** crea una tarea con título, descripción y fecha límite, **Then** los tres campos quedan guardados y la fecha límite se muestra en el listado.
- **Given** un usuario autenticado, **When** intenta crear una tarea con el título vacío, **Then** no se crea la tarea y se muestra un mensaje que indica que el título es obligatorio.

### TAR-02 · Editar una tarea

**Story:** Como usuario, quiero editar cualquier campo de una tarea existente, para mantenerla al día cuando cambien mis planes.

**Criterios de aceptación:**
- **Given** una tarea existente, **When** el usuario cambia su título y guarda, **Then** el listado muestra el nuevo título y la tarea conserva su estado anterior.
- **Given** una tarea con fecha límite, **When** el usuario cambia la fecha y guarda, **Then** la nueva fecha queda guardada y se muestra en el listado.
- **Given** una tarea con fecha límite, **When** el usuario vacía el campo de fecha y guarda, **Then** la tarea queda sin fecha límite. *(asumido: el PRD permite editar cualquier campo y la fecha es opcional, pero no detalla cómo se quita)*
- **Given** una tarea existente, **When** el usuario deja el título vacío y guarda, **Then** los cambios no se guardan y se muestra un mensaje que indica que el título es obligatorio.

### TAR-03 · Borrar una tarea

**Story:** Como usuario, quiero borrar una tarea, para quitar de mi listado lo que ya no necesito.

**Criterios de aceptación:**
- **Given** una tarea propia, **When** el usuario la borra, **Then** la tarea desaparece del listado.
- **Given** una tarea borrada, **When** el usuario aplica cualquier filtro de estado, **Then** la tarea no aparece en ningún resultado.
- **Given** una tarea de otro usuario, **When** el usuario autenticado intenta acceder a ella directamente desde su enlace, **Then** no puede verla ni borrarla y recibe un error de acceso.

### TAR-04 · Cambiar el estado de una tarea

**Story:** Como usuario, quiero cambiar el estado de una tarea entre `pending`, `completed` y `archived`, para tener a la vista solo lo que sigue pendiente.

**Criterios de aceptación:**
- **Given** una tarea en estado `pending`, **When** el usuario la marca como completada, **Then** su estado pasa a `completed`.
- **Given** una tarea en estado `pending`, **When** el usuario la archiva, **Then** su estado pasa a `archived` y la tarea aparece al aplicar el filtro de archivadas.
- **Given** una tarea en estado `completed`, **When** el usuario la vuelve a marcar como pendiente, **Then** su estado pasa a `pending`. *(asumido: el PRD no dice si se permiten todas las transiciones entre estados)*

## Épica 3: Organización y filtrado

### ORG-01 · Filtrar tareas por estado

**Story:** Como usuario, quiero filtrar mis tareas por estado, para ver solo las pendientes, las completadas o las archivadas.

**Criterios de aceptación:**
- **Given** un usuario con tareas en distintos estados, **When** aplica el filtro «pendientes», **Then** solo aparecen las tareas con estado `pending`.
- **Given** un usuario con tareas en distintos estados, **When** aplica el filtro «completadas», **Then** solo aparecen las tareas con estado `completed`.
- **Given** un usuario con tareas en distintos estados, **When** aplica el filtro «archivadas», **Then** solo aparecen las tareas con estado `archived`.
- **Given** un filtro de estado activo, **When** el usuario lo quita, **Then** vuelven a aparecer todas sus tareas. *(asumido: el PRD no describe cómo se quita un filtro)*

### ORG-02 · Estado vacío con invitación a crear una tarea

**Story:** Como usuario, quiero ver una invitación a crear mi primera tarea cuando no tengo ninguna, para saber cómo empezar a usar FlowSync.

**Criterios de aceptación:**
- **Given** una cuenta recién creada sin tareas, **When** el usuario abre su listado, **Then** ve un estado vacío con una invitación a crear la primera tarea.
- **Given** un usuario cuyas tareas están todas archivadas, **When** abre su listado, **Then** ve el mismo estado vacío con la invitación a crear una tarea. *(asumido: el listado por defecto no muestra las tareas archivadas; ver Dudas abiertas)*
- **Given** un usuario con tareas, **When** abre su listado, **Then** ve sus tareas y no el estado vacío.

## Épica 4: Exportación

### EXP-01 · Exportar tareas a CSV

**Story:** Como usuario, quiero exportar mis tareas a un archivo CSV, para llevarme mis datos cuando quiera.

**Criterios de aceptación:**
- **Given** un usuario con tareas, **When** pulsa exportar, **Then** descarga un archivo CSV con una fila por tarea y las columnas título, descripción, estado y fecha límite. *(asumido: nombres de columnas y formato de fecha pendientes de definir)*
- **Given** un usuario con tareas en distintos estados, **When** exporta, **Then** el CSV incluye todas sus tareas, también las archivadas, con su estado en la columna correspondiente. *(asumido: el PRD dice «exportar sus tareas» sin excluir ningún estado)*
- **Given** una tarea sin descripción ni fecha límite, **When** el usuario exporta, **Then** esas celdas aparecen vacías en la fila de la tarea. *(asumido)*

## Épica 5: Sincronización con Google Calendar

### SYNC-01 · Conectar Google y ver las tareas con fecha como eventos

**Story:** Como usuario, quiero conectar mi cuenta de Google y ver mis tareas con fecha límite como eventos en mi Google Calendar, para saber qué tengo que hacer hoy sin mirar dos sitios distintos.

**Criterios de aceptación:**
- **Given** un usuario registrado sin cuenta de Google conectada, **When** inicia la conexión y autoriza FlowSync en Google mediante OAuth, **Then** la cuenta queda conectada y FlowSync lo indica en la interfaz.
- **Given** una cuenta de Google conectada, **When** el usuario guarda una tarea con fecha límite, **Then** aparece en su Google Calendar un evento vinculado a esa tarea en la fecha de su fecha límite. *(asumido: la hora y la duración del evento no están definidas en el PRD; ver Dudas abiertas)*
- **Given** una cuenta de Google conectada, **When** el usuario guarda una tarea sin fecha límite, **Then** no se crea ningún evento en su Google Calendar.
- **Given** una cuenta de Google conectada, **When** el usuario guarda una tarea con fecha límite y la API de Google no está disponible, **Then** la tarea queda guardada en FlowSync y se muestra en su listado, y la sincronización se reintenta más tarde. *(asumido: número, intervalo de los reintentos y mensaje al usuario por el fallo, no definidos en el PRD)*

### SYNC-02 · Reflejar en Google Calendar los cambios de fecha, completado y borrado

**Story:** Como usuario, quiero que los cambios de fecha, completado o borrado de una tarea se reflejen en su evento de Google Calendar, para que mi calendario siempre coincida con mis tareas.

**Criterios de aceptación:**
- **Given** una tarea con evento sincronizado en Google Calendar, **When** el usuario cambia su fecha límite en FlowSync, **Then** el evento se actualiza a la nueva fecha.
- **Given** una tarea con evento sincronizado, **When** el usuario la marca como completada, **Then** el evento se elimina o se marca según corresponda, y deja de aparecer como pendiente en Google Calendar. *(asumido: el PRD deja abierto si se elimina o se marca el evento)*
- **Given** una tarea con evento sincronizado, **When** el usuario la borra en FlowSync, **Then** su evento desaparece de Google Calendar.
- **Given** una tarea con evento sincronizado, **When** el usuario cambia su fecha y la API de Google devuelve un error, **Then** la nueva fecha queda guardada en FlowSync y la sincronización se reintenta más tarde.

### SYNC-03 · Desconectar la cuenta de Google sin perder tareas

**Story:** Como usuario, quiero desconectar mi cuenta de Google en cualquier momento, para dejar de sincronizar sin perder mis tareas.

**Criterios de aceptación:**
- **Given** una cuenta de Google conectada, **When** el usuario la desconecta, **Then** FlowSync deja de crear, actualizar o eliminar eventos en su Google Calendar a partir de ese momento.
- **Given** una cuenta desconectada con tareas existentes, **When** el usuario abre su listado, **Then** todas sus tareas siguen disponibles, incluidas las que tenían evento sincronizado.
- **Given** una cuenta desconectada, **When** el usuario guarda una tarea con fecha límite, **Then** no se crea ningún evento en su Google Calendar.

---

## Dudas abiertas

- Criterio exacto de ordenación del listado «relevante para hoy» (PRD 3.3): impide cerrar cualquier criterio de orden.
- ¿Las tareas archivadas aparecen en el listado por defecto o solo con el filtro correspondiente? Afecta a ORG-02 y a TAR-04.
- Hora y duración de los eventos de tareas que solo tienen fecha, y zona horaria de referencia (PRD 7). Afecta a SYNC-01.
- Al completar una tarea, ¿el evento se elimina o se marca? (PRD 3.5 lo deja «según corresponda»). Afecta a SYNC-02.
- Número de reintentos, intervalo y mensaje al usuario cuando la sincronización falla de forma definitiva. Afecta a SYNC-01 y SYNC-02.
- Qué ocurre con los eventos ya creados en Google Calendar al desconectar la cuenta o al borrar una tarea sin conexión. Afecta a SYNC-02 y SYNC-03.
- ¿Se permiten todas las transiciones de estado (por ejemplo, de `completed` a `pending`)? Afecta a TAR-04.
- Formato del CSV: nombres de columnas, formato de fecha y codificación. Afecta a EXP-01.
- Cómo se quita un filtro de estado y si existe una vista «todas». Afecta a ORG-01.
- Alcance de la sincronización inversa (Google → FlowSync): queda fuera de estas stories hasta que el spike técnico lo valide (PRD 3.5).
