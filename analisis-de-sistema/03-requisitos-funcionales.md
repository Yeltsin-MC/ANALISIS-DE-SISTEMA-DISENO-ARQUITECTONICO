# Requisitos Funcionales - Campus UNSCH

## Introducción

Los requisitos funcionales describen las capacidades y comportamientos específicos que el sistema debe implementar. Cada requisito se deriva de las historias de usuario y mantiene trazabilidad con actores y dominios funcionales.

**Estructura de cada requisito:**
- **ID:** Identificador único
- **Nombre:** Título descriptivo del requisito
- **Descripción:** Detalle funcional específico
- **Actor:** Quién interactúa con esta funcionalidad
- **Prioridad:** Crítica / Alta / Media / Baja
- **Historia(s) Relacionada(s):** Trazabilidad con historias de usuario

---

## 1. Identidad y Usuarios

### RF-USR-01: Registro de usuario con validación institucional

**Descripción:**  
El sistema debe permitir el registro de estudiantes mediante la captura de DNI, correo institucional UNSCH, contraseña y datos personales. El registro debe validar automáticamente el DNI contra el Padrón Institucional (ACT-06) y verificar que el correo pertenezca al dominio institucional. El sistema debe impedir registros duplicados por DNI o correo institucional.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-USR-01

---

### RF-USR-02: Activación de cuenta por correo institucional

**Descripción:**  
El sistema debe enviar un correo de activación con token único y tiempo de expiración al correo institucional del estudiante durante el registro. La cuenta permanecerá inactiva hasta que el estudiante confirme su correo haciendo clic en el enlace de activación. El sistema debe validar el token y activar la cuenta si el token es válido y no ha expirado.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-USR-02

---

### RF-USR-03: Autenticación de usuarios

**Descripción:**  
El sistema debe permitir la autenticación de usuarios mediante correo institucional y contraseña. El sistema debe validar las credenciales contra la base de datos, verificar que la cuenta esté activada y generar una sesión autenticada mediante token seguro. El sistema debe implementar protección contra ataques de fuerza bruta.

**Actor:** ACT-01 (Estudiante), ACT-02 (Docente/Tesista), ACT-03 (Moderador), ACT-04 (Autoridad Electoral), ACT-05 (Administrador)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-USR-03

---

### RF-USR-04: Consulta de perfil de usuario

**Descripción:**  
El sistema debe permitir que usuarios autenticados consulten su información de perfil, incluyendo DNI, nombre, correo institucional, facultad, escuela, estado de cuenta, fecha de registro y último acceso. La información sensible como contraseña no debe ser visible.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-USR-04

---

### RF-USR-05: Actualización de perfil de usuario

**Descripción:**  
El sistema debe permitir que usuarios autenticados actualicen su información personal no institucional. El sistema NO debe permitir la modificación de DNI, código de estudiante o correo institucional. Todas las actualizaciones deben quedar registradas en auditoría.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media  
**Historias Relacionadas:** HU-USR-05

---

### RF-USR-06: Cambio de contraseña

**Descripción:**  
El sistema debe permitir que usuarios autenticados cambien su contraseña proporcionando la contraseña actual para validación. La nueva contraseña debe cumplir políticas de seguridad definidas (longitud mínima, complejidad) y ser diferente a la actual. El sistema debe almacenar la contraseña con hash seguro y notificar el cambio al correo institucional.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-USR-06

---

### RF-USR-07: Recuperación de contraseña

**Descripción:**  
El sistema debe permitir la recuperación de contraseña mediante el envío de un enlace de recuperación con token único al correo institucional registrado. El token debe tener tiempo de expiración y permitir al usuario establecer una nueva contraseña. El token debe invalidarse automáticamente después de ser utilizado.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-USR-07

---

### RF-USR-08: Cierre de sesión y expiración automática

**Descripción:**  
El sistema debe permitir que usuarios autenticados cierren su sesión manualmente, invalidando el token de sesión actual. El sistema debe implementar expiración automática de sesiones después de un periodo de inactividad configurable. El cierre de sesión debe quedar registrado con fecha y hora.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-USR-08

---

### RF-USR-09: Gestión de roles y permisos

**Descripción:**  
El sistema debe permitir que administradores asignen y modifiquen roles de usuarios. Los roles soportados son: Estudiante, Docente/Tesista, Moderador, Autoridad Electoral y Administrador. Los cambios de roles deben aplicarse inmediatamente y quedar registrados en auditoría. Solo usuarios con rol Administrador pueden gestionar roles.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-USR-09

---

### RF-USR-10: Suspensión y eliminación de cuentas

**Descripción:**  
El sistema debe permitir que administradores suspendan temporalmente o eliminen cuentas de usuario. La suspensión debe impedir el acceso del usuario sin eliminar sus datos. La eliminación debe requerir confirmación explícita. Todas las acciones administrativas deben quedar registradas en auditoría.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-ADM-02

---

## 2. Votación Electoral

### RF-VOT-01: Creación de proceso electoral

**Descripción:**  
El sistema debe permitir que autoridades electorales creen procesos electorales definiendo nombre, descripción, tipo, fecha y hora de inicio y cierre, padrón de estudiantes habilitados, cargos y listas/opciones de voto. El sistema debe validar que la fecha de cierre sea posterior a la de inicio y asignar estado inicial "Configuración". El sistema debe registrar auditoría de creación con usuario responsable.

**Actor:** ACT-04 (Autoridad Electoral)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-VOT-01

---

### RF-VOT-02: Apertura de proceso electoral

**Descripción:**  
El sistema debe permitir que autoridades electorales abran procesos electorales previamente configurados. El sistema debe validar que el padrón, cargos y listas estén completos antes de permitir la apertura. El sistema debe cambiar el estado del proceso a "Abierto" y registrar fecha, hora y usuario que realizó la apertura. Los resultados deben permanecer ocultos durante la votación.

**Actor:** ACT-04 (Autoridad Electoral)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-VOT-02

---

### RF-VOT-03: Emisión de voto único

**Descripción:**  
El sistema debe permitir que estudiantes habilitados emitan su voto en procesos electorales abiertos. El sistema debe verificar que el estudiante esté autenticado, en el padrón habilitado y que NO haya votado previamente en el proceso. El sistema debe registrar el voto mediante transacción con aislamiento adecuado para garantizar consistencia. El sistema debe impedir votos duplicados mediante restricciones de unicidad en base de datos (estudiante + proceso). El voto no puede ser modificado ni eliminado una vez emitido.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-VOT-03, HU-VOT-08

---

### RF-VOT-04: Registro de participación sin revelar elección

**Descripción:**  
El sistema debe registrar la participación electoral del estudiante (que votó) sin exponer el contenido de su voto (por quién votó). El sistema debe mantener separadas la tabla de participación y la tabla de votos para garantizar privacidad. El sistema debe permitir que el estudiante verifique su participación sin revelar su elección.

**Actor:** Sistema (lógica interna)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-VOT-03, HU-VOT-06

---

### RF-VOT-05: Cierre de proceso electoral

**Descripción:**  
El sistema debe permitir que autoridades electorales cierren procesos electorales activos. El sistema debe cambiar el estado del proceso a "Cerrado", registrar fecha, hora y usuario responsable, e impedir la emisión de nuevos votos después del cierre. El sistema debe calcular y consolidar resultados finales.

**Actor:** ACT-04 (Autoridad Electoral)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-VOT-04

---

### RF-VOT-06: Consulta de resultados electorales

**Descripción:**  
El sistema debe permitir la consulta de resultados de procesos electorales cerrados cuando estén habilitados para consulta pública. El sistema debe mostrar votos totales por opción/lista, porcentaje de participación y estadísticas agregadas sin revelar el voto individual de ningún estudiante.

**Actor:** ACT-01 (Estudiante), ACT-04 (Autoridad Electoral)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-VOT-05

---

### RF-VOT-07: Auditoría de proceso electoral

**Descripción:**  
El sistema debe mantener auditoría completa de procesos electorales, incluyendo eventos de creación, apertura, cierre, cambios de configuración, accesos administrativos y total de votos por periodo de tiempo. El sistema debe permitir exportar auditoría para revisión externa sin revelar votos individuales.

**Actor:** ACT-04 (Autoridad Electoral), ACT-05 (Administrador)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-VOT-07

---

### RF-VOT-08: Manejo de concurrencia en emisión de votos

**Descripción:**  
El sistema debe manejar correctamente solicitudes concurrentes de emisión de voto utilizando transacciones con nivel de aislamiento adecuado (READ COMMITTED o superior). El sistema debe garantizar que un estudiante no pueda votar dos veces incluso si envía múltiples solicitudes simultáneas. El sistema debe responder con error controlado ante intentos de voto duplicado.

**Actor:** Sistema (lógica interna)  
**Prioridad:** Crítica  
**Historias Relacionadas:** HU-VOT-08

---

## 3. Encuestas e Investigación

### RF-ENC-01: Creación y configuración de encuestas

**Descripción:**  
El sistema debe permitir que docentes/tesistas autorizados creen encuestas académicas definiendo título, descripción, objetivo, preguntas (selección, escala, respuesta abierta), validaciones, segmento objetivo, fechas de inicio y fin, límite de respuestas e incentivos. El sistema debe validar que el usuario tenga rol Docente/Tesista.

**Actor:** ACT-02 (Docente/Tesista)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-ENC-01

---

### RF-ENC-02: Publicación de encuestas

**Descripción:**  
El sistema debe permitir que docentes/tesistas publiquen encuestas configuradas. El sistema debe validar que la encuesta tenga al menos una pregunta y fechas configuradas correctamente antes de permitir la publicación. El sistema debe cambiar el estado a "Publicada" y hacer visible la encuesta a estudiantes del segmento objetivo.

**Actor:** ACT-02 (Docente/Tesista)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-ENC-02

---

### RF-ENC-03: Respuesta de encuestas con validaciones

**Descripción:**  
El sistema debe permitir que estudiantes respondan encuestas publicadas dentro del periodo habilitado. El sistema debe aplicar validaciones configuradas por pregunta, verificar límites de respuestas por estudiante, aplicar controles de atención y reglas antifraude, y registrar respuestas con marca de tiempo. El sistema debe impedir respuestas duplicadas cuando esté configurado.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-ENC-03, HU-ENC-05

---

### RF-ENC-04: Consulta y exportación de resultados

**Descripción:**  
El sistema debe permitir que docentes/tesistas consulten resultados agregados de sus encuestas, incluyendo gráficos para preguntas de selección y escala, respuestas de texto agrupadas, total de respuestas y tasa de participación. El sistema debe permitir exportar resultados en formatos CSV o Excel.

**Actor:** ACT-02 (Docente/Tesista)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-ENC-04

---

### RF-ENC-05: Control de respuestas duplicadas y antifraude

**Descripción:**  
El sistema debe aplicar límites configurables por usuario y encuesta, validar respuestas duplicadas, aplicar controles de atención para detectar respuestas automáticas, y asignar incentivos solo después de validaciones exitosas. El sistema debe registrar intentos de respuestas no permitidas en auditoría.

**Actor:** Sistema (lógica interna)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-ENC-05

---

## 4. Eventos

### RF-EVE-01: Publicación y configuración de eventos

**Descripción:**  
El sistema debe permitir que administradores publiquen eventos universitarios (charlas, talleres, congresos) definiendo título, descripción, fecha, hora, lugar, aforo máximo, imagen, configuración de certificado y fecha límite de inscripción. El sistema debe asignar estado "Publicado" al evento.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-EVE-01

---

### RF-EVE-02: Inscripción con control de aforo concurrente

**Descripción:**  
El sistema debe permitir que estudiantes autenticados se inscriban a eventos con cupos disponibles. El sistema debe controlar concurrencia mediante transacciones para evitar sobrepaso de aforo cuando múltiples estudiantes intenten ocupar los últimos cupos simultáneamente. El sistema debe reducir el contador de cupos disponibles y enviar confirmación.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-EVE-02

---

### RF-EVE-03: Generación de código QR para asistencia

**Descripción:**  
El sistema debe generar código QR único por inscripción que permita validar la asistencia del estudiante al evento. El código debe contener información cifrada que identifique al estudiante y al evento. El sistema debe permitir que estudiantes consulten su código QR desde el perfil.

**Actor:** Sistema (lógica interna), ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-EVE-03, HU-EVE-04

---

### RF-EVE-04: Validación de asistencia mediante QR

**Descripción:**  
El sistema debe permitir que administradores escaneen códigos QR de estudiantes en el evento para validar asistencia. El sistema debe validar que el código pertenezca al evento correspondiente, marcar la asistencia como "Asistió", registrar fecha y hora de validación, e impedir validaciones duplicadas.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-EVE-04

---

### RF-EVE-05: Generación de certificados digitales

**Descripción:**  
El sistema debe generar certificados digitales en formato PDF para estudiantes que asistieron a eventos configurados para otorgar certificado. El certificado debe incluir nombre del estudiante, evento, fecha y código de verificación único. El sistema debe permitir descargar el certificado desde el perfil y registrar la descarga.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media  
**Historias Relacionadas:** HU-EVE-05

---

## 5. Comunidad Estudiantil

### RF-COM-01: Publicación de contenido en comunidad

**Descripción:**  
El sistema debe permitir que estudiantes verificados publiquen contenido (título, texto, imágenes opcionales) en la comunidad estudiantil. El sistema debe permitir publicación anónima frente a otros estudiantes cuando esté habilitado, manteniendo trazabilidad administrativa para moderación. El sistema debe aplicar filtros básicos de contenido inapropiado y registrar fecha y hora de publicación.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-COM-01

---

### RF-COM-02: Comentarios en publicaciones

**Descripción:**  
El sistema debe permitir que estudiantes verificados comenten publicaciones existentes. El sistema debe permitir comentarios anónimos cuando esté habilitado manteniendo trazabilidad administrativa. Los comentarios deben mostrarse ordenados cronológicamente.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-COM-02

---

### RF-COM-03: Sistema de reportes de contenido

**Descripción:**  
El sistema debe permitir que estudiantes reporten publicaciones y comentarios inapropiados indicando motivo (acoso, contenido ofensivo, spam, etc.) y descripción adicional. El sistema debe registrar el reporte con fecha, hora y usuario reportante, notificar a moderadores e impedir reportes duplicados del mismo usuario sobre el mismo contenido.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-COM-03

---

### RF-COM-04: Moderación y aplicación de medidas

**Descripción:**  
El sistema debe permitir que moderadores revisen reportes pendientes, visualicen contenido reportado con contexto, oculten o eliminen contenido, apliquen medidas al usuario infractor y rechacen reportes que no procedan. El sistema debe registrar auditoría de todas las acciones de moderación y notificar al usuario afectado.

**Actor:** ACT-03 (Moderador)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-COM-04

---

### RF-COM-05: Gestión de publicaciones propias

**Descripción:**  
El sistema debe permitir que estudiantes consulten sus publicaciones y comentarios, editen publicaciones propias dentro de un periodo configurable, y eliminen publicaciones propias que no tengan comentarios.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media  
**Historias Relacionadas:** HU-COM-05

---

## 6. Incentivos y Puntos

### RF-INC-01: Asignación idempotente de puntos

**Descripción:**  
El sistema debe asignar puntos por actividades autorizadas (responder encuestas, asistir a eventos, participar en votaciones) utilizando operaciones idempotentes para evitar asignaciones duplicadas. El sistema debe aplicar límites diarios y reglas de elegibilidad, registrar auditoría de cada asignación y actualizar el saldo del estudiante. La asignación de puntos por participación electoral NO debe depender de la opción elegida ni requerir revelar el contenido del voto.

**Actor:** Sistema (lógica interna)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-INC-01

---

### RF-INC-02: Consulta de saldo y historial de puntos

**Descripción:**  
El sistema debe permitir que estudiantes consulten su saldo actual de puntos, historial de puntos ganados con detalle de actividad, puntos canjeados y fecha de cada movimiento.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media  
**Historias Relacionadas:** HU-INC-02

---

### RF-INC-03: Canje de puntos por recompensas

**Descripción:**  
El sistema debe permitir que estudiantes canjeen puntos por recompensas del catálogo disponible. El sistema debe validar que el estudiante tenga puntos suficientes, registrar el canje mediante transacción, deducir puntos del saldo, generar comprobante o código de canje y notificar el canje exitoso.

**Actor:** ACT-01 (Estudiante)  
**Prioridad:** Media  
**Historias Relacionadas:** HU-INC-03

---

### RF-INC-04: Configuración de reglas de incentivos

**Descripción:**  
El sistema debe permitir que administradores configuren puntos por tipo de actividad, límites diarios por usuario, reglas de elegibilidad y habiliten/deshabiliten tipos de incentivos. El sistema debe registrar auditoría de cambios de configuración y aplicar cambios a partir de la fecha de configuración.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Alta  
**Historias Relacionadas:** HU-INC-04

---

### RF-INC-05: Gestión de catálogo de recompensas

**Descripción:**  
El sistema debe permitir que administradores agreguen, editen y eliminen recompensas del catálogo, definiendo nombre, descripción, puntos requeridos, imagen, cantidad disponible y estado (habilitado/deshabilitado). El sistema debe actualizar disponibilidad después de cada canje.

**Actor:** ACT-05 (Administrador)  
**Prioridad:** Media  
**Historias Relacionadas:** HU-INC-05

---

## 7. Administración y Reportes

_(A completar en el siguiente commit)_
