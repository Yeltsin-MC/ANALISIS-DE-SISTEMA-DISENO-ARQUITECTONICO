# Actores del Sistema Campus UNSCH

## Introducción

Los actores representan entidades que interactúan con el sistema Campus UNSCH. Se clasifican en actores humanos y sistemas externos. Este documento identifica y caracteriza cada actor según su rol, responsabilidades e interacciones con la plataforma.

---

## A. Actores Humanos

### ACT-01: Estudiante

**Tipo:** Actor Humano  
**Descripción:** Usuario principal del sistema. Estudiante habilitado de la UNSCH que accede a la plataforma para participar en votaciones, responder encuestas, inscribirse a eventos, interactuar en la comunidad y consultar sus incentivos.

**Responsabilidades:**
- Registrarse y autenticarse en la plataforma
- Mantener actualizada su información de perfil
- Emitir voto en procesos electorales habilitados
- Responder encuestas académicas y de investigación
- Inscribirse y asistir a eventos universitarios
- Participar en la comunidad estudiantil
- Consultar y canjear puntos acumulados
- Reportar contenido inapropiado

**Módulos con los que interactúa:**
- Identidad y Usuarios
- Votación Electoral
- Encuestas
- Eventos
- Comunidad
- Incentivos

---

### ACT-02: Docente/Tesista

**Tipo:** Actor Humano  
**Descripción:** Personal académico o estudiante de posgrado autorizado que utiliza la plataforma para crear y administrar encuestas dirigidas a la población estudiantil con fines académicos o de investigación.

**Responsabilidades:**
- Autenticarse en la plataforma con credenciales institucionales
- Crear encuestas académicas y de investigación
- Definir segmentos objetivo de estudiantes
- Configurar preguntas, validaciones y reglas de elegibilidad
- Consultar resultados y análisis de respuestas
- Exportar datos para investigación

**Módulos con los que interactúa:**
- Identidad y Usuarios
- Encuestas
- Reportes

---

### ACT-03: Moderador

**Tipo:** Actor Humano  
**Descripción:** Usuario con permisos especiales para supervisar y moderar el contenido de la comunidad estudiantil, garantizando el cumplimiento de las normas de convivencia institucional.

**Responsabilidades:**
- Revisar reportes de contenido inapropiado
- Ocultar o eliminar publicaciones que incumplan normas
- Aplicar medidas correctivas según políticas definidas
- Mantener trazabilidad de acciones de moderación
- Generar reportes de actividad de moderación

**Módulos con los que interactúa:**
- Identidad y Usuarios
- Comunidad
- Auditoría
- Administración

---

### ACT-04: Autoridad Electoral

**Tipo:** Actor Humano  
**Descripción:** Personal institucional autorizado para gestionar procesos electorales estudiantiles. Configura, supervisa y cierra procesos electorales garantizando transparencia y auditoría.

**Responsabilidades:**
- Crear y configurar procesos electorales
- Definir padrón habilitado, cargos y listas
- Establecer fecha y hora de inicio y cierre de votación
- Supervisar el desarrollo del proceso electoral
- Cerrar votación y habilitar resultados
- Consultar auditoría y trazabilidad electoral
- Generar reportes de participación

**Módulos con los que interactúa:**
- Identidad y Usuarios
- Votación Electoral
- Auditoría
- Reportes

---

### ACT-05: Administrador

**Tipo:** Actor Humano  
**Descripción:** Usuario con máximos privilegios en el sistema. Responsable de la gestión general de la plataforma, usuarios, roles, configuraciones y supervisión de operaciones críticas.

**Responsabilidades:**
- Gestionar usuarios y asignación de roles
- Configurar parámetros del sistema
- Supervisar encuestas, eventos y procesos
- Administrar reglas de incentivos y catálogo de recompensas
- Configurar límites y políticas del sistema
- Consultar trazabilidad y auditoría completa
- Acceder a panel administrativo y dashboards
- Gestionar respaldos y mantenimiento del sistema

**Módulos con los que interactúa:**
- Identidad y Usuarios
- Votación Electoral
- Encuestas
- Eventos
- Comunidad
- Incentivos
- Auditoría
- Administración
- Reportes

---

## B. Sistemas Externos

_(A completar en el siguiente commit)_
