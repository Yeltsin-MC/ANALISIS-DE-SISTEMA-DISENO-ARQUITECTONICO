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

_(A completar en el siguiente commit)_
