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

_(A completar en el siguiente commit)_
