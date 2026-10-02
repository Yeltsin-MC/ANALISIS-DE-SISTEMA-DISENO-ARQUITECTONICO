# Historias de Usuario - Campus UNSCH

## Introducción

Las historias de usuario representan las necesidades funcionales del sistema desde la perspectiva de los actores. Cada historia sigue el formato estándar:

**Como** [actor]  
**quiero** [acción]  
**para** [beneficio]

Las historias están organizadas por dominios funcionales y mantienen trazabilidad con los actores identificados.

---

## 1. Identidad y Usuarios

### HU-USR-01: Registro de estudiante

**Como** estudiante de la UNSCH  
**quiero** registrarme en la plataforma Campus UNSCH  
**para** acceder a los servicios de votación, encuestas, eventos y comunidad estudiantil.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema solicita DNI, correo institucional UNSCH, contraseña y datos personales
- El sistema valida el DNI contra el Padrón Institucional (ACT-06)
- El sistema valida que el correo pertenezca al dominio institucional
- El sistema verifica que el estudiante se encuentre habilitado según padrón
- El sistema evita registros duplicados por DNI o correo
- El sistema envía correo de activación al correo institucional proporcionado
- La cuenta permanece inactiva hasta confirmar el correo

---

### HU-USR-02: Activación de cuenta

**Como** estudiante registrado  
**quiero** activar mi cuenta mediante el enlace enviado a mi correo institucional  
**para** poder autenticarme y acceder a la plataforma.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema genera un token único de activación con tiempo de expiración
- El estudiante recibe el enlace de activación en su correo institucional
- Al hacer clic en el enlace, el sistema valida el token
- Si el token es válido, la cuenta se activa exitosamente
- Si el token expiró, el sistema permite solicitar reenvío
- Una cuenta activada permite autenticación

---

### HU-USR-03: Inicio de sesión

**Como** estudiante habilitado  
**quiero** iniciar sesión con mi correo y contraseña  
**para** acceder a los servicios de Campus UNSCH.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema solicita correo institucional y contraseña
- El sistema valida las credenciales contra la base de datos
- El sistema verifica que la cuenta esté activada
- Si las credenciales son correctas, el sistema genera sesión autenticada
- El sistema registra fecha y hora del último acceso
- Si las credenciales son incorrectas, se muestra mensaje de error sin revelar si el correo existe
- El sistema implementa protección contra ataques de fuerza bruta

---

### HU-USR-04: Consulta de perfil

**Como** estudiante autenticado  
**quiero** consultar mi información de perfil  
**para** verificar mis datos personales e institucionales.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema muestra DNI, nombre, correo institucional, facultad, escuela
- El sistema muestra estado de cuenta (activa, suspendida)
- El sistema muestra fecha de registro y último acceso
- La información sensible como contraseña no es visible

---

### HU-USR-05: Actualización de perfil

**Como** estudiante autenticado  
**quiero** actualizar mi información permitida de perfil  
**para** mantener mis datos personales actualizados.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema permite actualizar datos personales no institucionales
- El sistema NO permite modificar DNI, código de estudiante o correo institucional
- El sistema valida los formatos de los datos ingresados
- El sistema registra auditoría de cambios de perfil
- El sistema confirma la actualización exitosa

---

### HU-USR-06: Cambio de contraseña

**Como** estudiante autenticado  
**quiero** cambiar mi contraseña  
**para** mantener la seguridad de mi cuenta.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema solicita contraseña actual para validar identidad
- El sistema solicita nueva contraseña y confirmación
- El sistema valida que la nueva contraseña cumpla políticas de seguridad
- El sistema valida que la nueva contraseña sea diferente a la actual
- El sistema actualiza la contraseña con hash seguro
- El sistema envía notificación de cambio de contraseña al correo institucional

---

### HU-USR-07: Recuperación de contraseña

**Como** estudiante que olvidó su contraseña  
**quiero** recuperar el acceso a mi cuenta  
**para** volver a utilizar la plataforma.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema solicita correo institucional registrado
- El sistema genera token de recuperación con tiempo de expiración
- El sistema envía enlace de recuperación al correo institucional
- Al acceder al enlace, el sistema valida el token
- El sistema permite establecer nueva contraseña
- El token se invalida después de ser utilizado
- El sistema notifica el cambio exitoso

---

### HU-USR-08: Cierre de sesión

**Como** estudiante autenticado  
**quiero** cerrar mi sesión  
**para** proteger mi cuenta cuando termine de usar la plataforma.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema invalida el token de sesión actual
- El sistema redirige al usuario a la página de inicio de sesión
- El sistema registra la fecha y hora del cierre de sesión
- Las sesiones expiran automáticamente después de un periodo de inactividad

---

### HU-USR-09: Gestión de roles

**Como** administrador del sistema  
**quiero** asignar y modificar roles de usuarios  
**para** controlar los permisos de acceso en la plataforma.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema permite asignar roles: Estudiante, Docente/Tesista, Moderador, Autoridad Electoral, Administrador
- El sistema permite modificar roles de usuarios existentes
- El sistema valida que solo administradores puedan gestionar roles
- El sistema registra auditoría de cambios de roles
- Los cambios de permisos se aplican inmediatamente en la siguiente acción del usuario

---

## 2. Votación Electoral

### HU-VOT-01: Creación de proceso electoral

**Como** autoridad electoral  
**quiero** crear un proceso electoral con fecha de inicio, cierre, padrón y listas  
**para** permitir que los estudiantes habilitados participen en elecciones estudiantiles.

**Actor:** ACT-04 (Autoridad Electoral)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema permite definir nombre, descripción y tipo de proceso electoral
- El sistema permite establecer fecha y hora de inicio y cierre del proceso
- El sistema permite cargar o definir el padrón de estudiantes habilitados
- El sistema permite registrar cargos y listas/opciones de voto
- El sistema valida que la fecha de cierre sea posterior a la de inicio
- El sistema asigna un estado inicial "Configuración" al proceso
- El sistema registra auditoría de creación con usuario responsable

---

### HU-VOT-02: Apertura de proceso electoral

**Como** autoridad electoral  
**quiero** abrir un proceso electoral configurado  
**para** permitir que los estudiantes comiencen a emitir su voto.

**Actor:** ACT-04 (Autoridad Electoral)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema valida que el proceso esté en estado "Configuración"
- El sistema valida que el padrón, cargos y listas estén completos
- El sistema cambia el estado del proceso a "Abierto"
- El sistema registra fecha, hora y usuario que abrió el proceso
- Los estudiantes habilitados pueden emitir voto a partir de la apertura
- Los resultados permanecen ocultos durante la votación

---

### HU-VOT-03: Emisión de voto

**Como** estudiante habilitado  
**quiero** emitir mi voto en un proceso electoral abierto  
**para** participar en la elección estudiantil.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema verifica que el estudiante esté autenticado
- El sistema verifica que el estudiante esté en el padrón habilitado del proceso
- El sistema verifica que el proceso electoral esté en estado "Abierto"
- El sistema verifica que el estudiante NO haya votado previamente en este proceso
- El sistema permite seleccionar la opción de voto (lista o candidato)
- El sistema registra el voto mediante transacción que garantiza consistencia
- El sistema registra la participación del estudiante sin exponer su elección
- El sistema confirma la emisión exitosa del voto
- El sistema impide votos duplicados mediante restricciones de base de datos
- El voto NO puede ser modificado ni eliminado una vez emitido

---

### HU-VOT-04: Cierre de proceso electoral

**Como** autoridad electoral  
**quiero** cerrar un proceso electoral activo  
**para** finalizar la votación y proceder al conteo oficial.

**Actor:** ACT-04 (Autoridad Electoral)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema valida que el proceso esté en estado "Abierto"
- El sistema cambia el estado del proceso a "Cerrado"
- El sistema registra fecha, hora y usuario que cerró el proceso
- El sistema impide la emisión de nuevos votos después del cierre
- El sistema calcula y consolida resultados
- Los resultados pueden habilitarse según configuración del proceso

---

### HU-VOT-05: Consulta de resultados

**Como** estudiante  
**quiero** consultar los resultados de un proceso electoral cerrado  
**para** conocer el resultado de la elección.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema valida que el proceso esté cerrado
- El sistema valida que los resultados estén habilitados para consulta pública
- El sistema muestra votos totales por opción/lista
- El sistema muestra porcentaje de participación
- El sistema muestra estadísticas agregadas (total de votantes, abstenciones)
- El sistema NO revela el voto individual de ningún estudiante

---

### HU-VOT-06: Consulta de participación electoral personal

**Como** estudiante  
**quiero** consultar si he participado en un proceso electoral  
**para** verificar mi participación sin revelar mi voto.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema muestra listado de procesos electorales disponibles
- El sistema indica si el estudiante ha votado o no en cada proceso
- El sistema muestra fecha y hora de participación
- El sistema NO revela por quién votó el estudiante
- El sistema NO permite modificar el voto emitido

---

### HU-VOT-07: Auditoría de proceso electoral

**Como** autoridad electoral  
**quiero** consultar la auditoría completa de un proceso electoral  
**para** verificar la integridad y transparencia del proceso.

**Actor:** ACT-04 (Autoridad Electoral)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema muestra eventos de creación, apertura y cierre del proceso
- El sistema muestra total de votos emitidos por periodo de tiempo
- El sistema muestra accesos administrativos al proceso
- El sistema muestra cambios de configuración realizados
- El sistema muestra padrón habilitado utilizado
- El sistema NO revela el voto individual de los estudiantes
- El sistema permite exportar auditoría para revisión externa

---

### HU-VOT-08: Verificación de voto único

**Como** sistema  
**quiero** garantizar que cada estudiante vote una sola vez por proceso electoral  
**para** mantener la integridad del proceso democrático.

**Actor:** Sistema (lógica interna)  
**Prioridad:** Crítica

**Criterios de aceptación:**
- El sistema utiliza restricciones de unicidad en base de datos (estudiante + proceso)
- El sistema valida voto único antes de registrar el voto
- El sistema maneja correctamente solicitudes concurrentes de voto del mismo estudiante
- El sistema utiliza transacciones con nivel de aislamiento adecuado
- El sistema registra intentos de voto duplicado en auditoría
- El sistema responde con error controlado ante intento de voto duplicado

---

## 3. Encuestas e Investigación

### HU-ENC-01: Creación de encuesta

**Como** docente/tesista autorizado  
**quiero** crear una encuesta académica dirigida a estudiantes  
**para** recopilar información para investigación o fines académicos.

**Actor:** ACT-02 (Docente/Tesista)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema valida que el usuario tenga rol Docente/Tesista
- El sistema permite definir título, descripción y objetivo de la encuesta
- El sistema permite agregar preguntas de selección, escala y respuesta abierta
- El sistema permite definir validaciones básicas por pregunta
- El sistema permite definir segmento objetivo de estudiantes (facultad, escuela, ciclo)
- El sistema permite establecer fecha de inicio y fin de la encuesta
- El sistema permite configurar límite de respuestas por estudiante
- El sistema permite habilitar incentivos por respuesta
- El sistema asigna estado inicial "Borrador" a la encuesta

---

### HU-ENC-02: Publicación de encuesta

**Como** docente/tesista  
**quiero** publicar una encuesta configurada  
**para** que los estudiantes objetivo puedan responderla.

**Actor:** ACT-02 (Docente/Tesista)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema valida que la encuesta tenga al menos una pregunta
- El sistema valida que las fechas estén configuradas correctamente
- El sistema cambia el estado de la encuesta a "Publicada"
- Los estudiantes del segmento objetivo pueden visualizar la encuesta
- El sistema registra fecha y hora de publicación

---

### HU-ENC-03: Respuesta de encuesta

**Como** estudiante  
**quiero** responder encuestas disponibles  
**para** contribuir a la investigación académica y obtener incentivos.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema muestra encuestas disponibles según el segmento del estudiante
- El sistema valida que la encuesta esté publicada y dentro del periodo habilitado
- El sistema valida que el estudiante no haya superado el límite de respuestas
- El sistema aplica validaciones configuradas en las preguntas
- El sistema registra las respuestas con marca de tiempo
- El sistema aplica controles de atención y reglas antifraude cuando corresponda
- El sistema asigna puntos si la encuesta tiene incentivos configurados
- El sistema impide respuestas duplicadas cuando esté configurado

---

### HU-ENC-04: Consulta de resultados de encuesta

**Como** docente/tesista  
**quiero** consultar los resultados agregados de mi encuesta  
**para** analizar la información recopilada.

**Actor:** ACT-02 (Docente/Tesista)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema muestra resultados agregados por pregunta
- El sistema muestra gráficos para preguntas de selección y escala
- El sistema muestra respuestas de texto agrupadas
- El sistema muestra total de respuestas y tasa de participación
- El sistema permite filtrar resultados por segmento
- El sistema permite exportar resultados en formato CSV o Excel

---

### HU-ENC-05: Control de respuestas duplicadas

**Como** sistema  
**quiero** controlar respuestas duplicadas según configuración  
**para** garantizar la calidad de los datos recopilados.

**Actor:** Sistema (lógica interna)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema aplica límites configurables por usuario y encuesta
- El sistema valida respuestas duplicadas antes de registrar
- El sistema aplica controles de atención para detectar respuestas automáticas
- El sistema registra intentos de respuestas no permitidas
- El sistema asigna incentivos solo después de validaciones exitosas

---

## 4. Eventos

### HU-EVE-01: Publicación de evento

**Como** administrador  
**quiero** publicar eventos universitarios (charlas, talleres, congresos)  
**para** que los estudiantes puedan inscribirse y asistir.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema permite ingresar título, descripción, fecha, hora y lugar del evento
- El sistema permite establecer aforo máximo del evento
- El sistema permite cargar imagen o banner del evento
- El sistema permite configurar si el evento otorga certificado
- El sistema permite establecer fecha límite de inscripción
- El sistema asigna estado "Publicado" al evento
- Los estudiantes pueden visualizar el evento publicado

---

### HU-EVE-02: Inscripción a evento

**Como** estudiante  
**quiero** inscribirme a un evento disponible  
**para** reservar mi cupo y asistir al evento.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema muestra eventos disponibles con cupos restantes
- El sistema valida que el estudiante esté autenticado
- El sistema valida que exista aforo disponible
- El sistema controla concurrencia para evitar sobrepaso de aforo
- El sistema registra la inscripción mediante transacción
- El sistema reduce el contador de cupos disponibles
- El sistema confirma la inscripción exitosa
- El sistema envía notificación de confirmación

---

### HU-EVE-03: Consulta de mis inscripciones

**Como** estudiante  
**quiero** consultar los eventos a los que me he inscrito  
**para** recordar fecha, hora y lugar de asistencia.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema muestra listado de eventos inscritos
- El sistema muestra estado de cada inscripción (pendiente, asistido, no asistió)
- El sistema muestra fecha y hora del evento
- El sistema permite visualizar código QR o código único de asistencia

---

### HU-EVE-04: Validación de asistencia

**Como** administrador  
**quiero** validar la asistencia de estudiantes mediante código QR  
**para** confirmar su participación en el evento.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema genera código QR único por inscripción
- El sistema permite escanear código QR en el evento
- El sistema valida que el código pertenezca al evento correspondiente
- El sistema marca la asistencia como "Asistió"
- El sistema registra fecha y hora de validación
- El sistema impide validaciones duplicadas

---

### HU-EVE-05: Generación de certificado

**Como** estudiante que asistió a un evento  
**quiero** descargar mi certificado digital  
**para** acreditar mi participación.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema valida que el evento otorgue certificado
- El sistema valida que el estudiante haya asistido al evento
- El sistema genera certificado en formato PDF
- El certificado incluye nombre del estudiante, evento, fecha y código de verificación
- El sistema permite descargar el certificado desde el perfil
- El sistema registra la descarga del certificado

---

## 5. Comunidad Estudiantil

### HU-COM-01: Publicación en comunidad

**Como** estudiante verificado  
**quiero** publicar contenido en la comunidad estudiantil  
**para** compartir información, opiniones o preguntas con otros estudiantes.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema valida que el estudiante esté autenticado y verificado
- El sistema permite ingresar título y contenido de la publicación
- El sistema permite adjuntar imágenes (opcional)
- El sistema permite publicar de forma anónima si está habilitado
- El sistema mantiene trazabilidad administrativa aun en publicaciones anónimas
- El sistema aplica filtros básicos de contenido inapropiado
- El sistema registra fecha y hora de publicación
- La publicación se muestra a otros estudiantes

---

### HU-COM-02: Comentario en publicación

**Como** estudiante verificado  
**quiero** comentar publicaciones de la comunidad  
**para** interactuar con otros estudiantes.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema permite agregar comentarios a publicaciones existentes
- El sistema valida que el estudiante esté autenticado
- El sistema permite comentar de forma anónima si está habilitado
- El sistema mantiene trazabilidad administrativa de comentarios anónimos
- El sistema registra fecha y hora del comentario
- Los comentarios se muestran ordenados cronológicamente

---

### HU-COM-03: Reporte de contenido

**Como** estudiante  
**quiero** reportar contenido inapropiado  
**para** mantener un ambiente de convivencia saludable.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema permite reportar publicaciones y comentarios
- El sistema solicita motivo del reporte (acoso, contenido ofensivo, spam, etc.)
- El sistema permite agregar descripción adicional
- El sistema registra el reporte con fecha, hora y usuario reportante
- El sistema notifica a moderadores sobre el nuevo reporte
- El sistema impide reportes duplicados del mismo usuario sobre el mismo contenido

---

### HU-COM-04: Moderación de contenido

**Como** moderador  
**quiero** revisar reportes de contenido y aplicar medidas  
**para** garantizar el cumplimiento de normas de convivencia.

**Actor:** ACT-03 (Moderador)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema muestra listado de reportes pendientes
- El sistema permite visualizar el contenido reportado y su contexto
- El sistema permite ocultar o eliminar contenido
- El sistema permite aplicar medidas al usuario infractor
- El sistema permite rechazar el reporte si no procede
- El sistema registra auditoría de todas las acciones de moderación
- El sistema notifica al usuario afectado sobre la medida aplicada

---

### HU-COM-05: Consulta de mis publicaciones

**Como** estudiante  
**quiero** consultar mis publicaciones y comentarios  
**para** revisar mi actividad en la comunidad.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema muestra publicaciones del estudiante autenticado
- El sistema muestra comentarios realizados
- El sistema muestra fecha de publicación
- El sistema permite editar publicaciones propias (dentro de un periodo)
- El sistema permite eliminar publicaciones propias sin comentarios

---

## 6. Incentivos y Puntos

### HU-INC-01: Asignación de puntos

**Como** sistema  
**quiero** asignar puntos por actividades autorizadas  
**para** incentivar la participación estudiantil.

**Actor:** Sistema (lógica interna)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema asigna puntos por responder encuestas autorizadas
- El sistema asigna puntos por asistir a eventos
- El sistema asigna puntos por participación en votaciones sin revelar la opción elegida
- El sistema utiliza operaciones idempotentes para evitar asignaciones duplicadas
- El sistema aplica límites diarios y reglas de elegibilidad
- El sistema registra auditoría de cada asignación de puntos
- El sistema actualiza el saldo de puntos del estudiante

---

### HU-INC-02: Consulta de puntos

**Como** estudiante  
**quiero** consultar mis puntos acumulados  
**para** conocer mi saldo y actividades realizadas.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema muestra saldo actual de puntos
- El sistema muestra historial de puntos ganados con detalle de actividad
- El sistema muestra puntos canjeados
- El sistema muestra fecha de cada movimiento
- El sistema muestra puntos disponibles para canje

---

### HU-INC-03: Canje de puntos

**Como** estudiante  
**quiero** canjear mis puntos por recompensas disponibles  
**para** obtener beneficios por mi participación.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema muestra catálogo de recompensas disponibles
- El sistema muestra puntos requeridos por recompensa
- El sistema valida que el estudiante tenga puntos suficientes
- El sistema registra el canje mediante transacción
- El sistema deduce los puntos del saldo del estudiante
- El sistema genera comprobante o código de canje
- El sistema notifica el canje exitoso

---

### HU-INC-04: Configuración de reglas de incentivos

**Como** administrador  
**quiero** configurar reglas de asignación y límites de puntos  
**para** controlar el sistema de incentivos.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Alta

**Criterios de aceptación:**
- El sistema permite definir puntos por tipo de actividad
- El sistema permite establecer límites diarios por usuario
- El sistema permite configurar reglas de elegibilidad
- El sistema permite habilitar o deshabilitar tipos de incentivos
- El sistema registra auditoría de cambios de configuración
- Los cambios se aplican a partir de la fecha de configuración

---

### HU-INC-05: Gestión del catálogo de recompensas

**Como** administrador  
**quiero** gestionar el catálogo de recompensas disponibles  
**para** ofrecer opciones de canje a los estudiantes.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Media

**Criterios de aceptación:**
- El sistema permite agregar, editar y eliminar recompensas
- El sistema permite definir nombre, descripción, puntos requeridos e imagen
- El sistema permite establecer cantidad disponible de cada recompensa
- El sistema permite habilitar o deshabilitar recompensas
- El sistema actualiza disponibilidad después de cada canje

---

## 7. Administración

_(A completar en el siguiente commit)_
