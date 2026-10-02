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

_(A completar en el siguiente commit)_
