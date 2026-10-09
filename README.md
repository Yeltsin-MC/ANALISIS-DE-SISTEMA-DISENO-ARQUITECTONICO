# Diseño de una arquitectura de software escalable y elástica para una plataforma integral de servicios y participación estudiantil en la UNSCH – 2026

## Información del Proyecto

**Universidad:** Universidad Nacional de San Cristóbal de Huamanga  
**Escuela Profesional:** Ingeniería de Sistemas  
**Curso:** Arquitectura de Software  
**Docente:** Ing. Lizbet Jaico Quispe  
**Estudiante:** Yeltsin Wilber Muñoz Corichahua  
**Nombre del Sistema:** Campus UNSCH

---

## Descripción

Campus UNSCH es una plataforma integral diseñada para centralizar la participación estudiantil y los servicios universitarios en la Universidad Nacional de San Cristóbal de Huamanga.

El sistema busca proporcionar un entorno web seguro, escalable y elástico que permita a los estudiantes acceder a votaciones electrónicas, encuestas académicas, eventos universitarios, comunidad estudiantil y mecanismos de incentivos desde una única plataforma.

---

## Problema Abordado

La participación estudiantil en la UNSCH se encuentra fragmentada en múltiples canales y carece de una plataforma unificada que garantice:

- Procesos electorales transparentes y auditables.
- Acceso centralizado a encuestas académicas.
- Gestión eficiente de eventos universitarios.
- Espacios de interacción estudiantil moderados.
- Sistemas de incentivos controlados.

---

## Objetivo

Diseñar una arquitectura de software escalable y elástica que permita soportar hasta **2,500 usuarios concurrentes** durante procesos masivos, garantizando consistencia, seguridad, disponibilidad y auditabilidad en las operaciones críticas del sistema.

La UNSCH cuenta con aproximadamente 15,000 estudiantes, mientras que el uso normal estimado de la plataforma es de 200 a 500 usuarios diarios.

---

## Estructura del Repositorio

```text
.
├── propuesta/
│   └── Propuesta-plataforma integral.pdf
│
├── analisis-de-sistema/
│   ├── 01-actores.md
│   ├── 02-historias-del-usuario.md
│   ├── 03-requisitos-funcionales.md
│   ├── 04-atributos-de-calidad.md
│   ├── 05-restricciones.md
│   └── 06-driver-arquitectonicos.md
│
├── arquitectura/
│   ├── arquitectura-inicial.md
│   ├── decisiones-arquitectonicas.md
│   ├── estilo-arquitectonico.md
│   └── enfoque/
│       └── enfoque-arquitectonico.md
│
└── README.md
```

---

# Análisis del Sistema

## 1. Actores del Sistema

**Archivo:** [`analisis-de-sistema/01-actores.md`](analisis-de-sistema/01-actores.md)

Se identifican los principales actores humanos y sistemas externos que interactúan con Campus UNSCH.

Entre ellos se encuentran:

- Estudiante.
- Docente/Tesista.
- Moderador.
- Autoridad Electoral.
- Administrador.
- Padrón Institucional UNSCH.
- Correo Institucional UNSCH.

---

## 2. Historias de Usuario

**Archivo:** [`analisis-de-sistema/02-historias-del-usuario.md`](analisis-de-sistema/02-historias-del-usuario.md)

Las historias de usuario se organizan según los principales dominios funcionales:

- Identidad y Usuarios.
- Votación Electoral.
- Encuestas e Investigación.
- Eventos.
- Comunidad Estudiantil.
- Incentivos y Puntos.
- Administración.
- Reportes y Análisis.

Cada historia identifica el actor, prioridad y criterios de aceptación.

---

## 3. Requisitos Funcionales

**Archivo:** [`analisis-de-sistema/03-requisitos-funcionales.md`](analisis-de-sistema/03-requisitos-funcionales.md)

Los requisitos funcionales fueron derivados de las historias de usuario y organizados según los módulos principales del sistema.

Entre las operaciones críticas destacan:

- Emisión de voto único.
- Registro de participación electoral.
- Inscripción a eventos con control de aforo.
- Asignación controlada de incentivos.
- Autenticación y gestión de usuarios.
- Gestión de encuestas y comunidad estudiantil.

---

## 4. Atributos de Calidad

**Archivo:** [`analisis-de-sistema/04-atributos-de-calidad.md`](analisis-de-sistema/04-atributos-de-calidad.md)

Los principales atributos de calidad considerados son:

- Rendimiento.
- Escalabilidad.
- Elasticidad.
- Seguridad.
- Consistencia.
- Disponibilidad.
- Resiliencia.
- Auditabilidad.
- Observabilidad.
- Modificabilidad.

El escenario objetivo considera hasta **2,500 usuarios concurrentes** durante procesos masivos como elecciones.

---

## 5. Restricciones

**Archivo:** [`analisis-de-sistema/05-restricciones.md`](analisis-de-sistema/05-restricciones.md)

Las restricciones consideran aspectos:

- Institucionales.
- Tecnológicos.
- Académicos.
- De seguridad.
- De alcance.

Entre las tecnologías consideradas en la propuesta se encuentran Docker, Kubernetes, PostgreSQL, Redis, RabbitMQ y herramientas de observabilidad y pruebas de carga.

---

## 6. Drivers Arquitectónicos

**Archivo:** [`analisis-de-sistema/06-driver-arquitectonicos.md`](analisis-de-sistema/06-driver-arquitectonicos.md)

Se identificaron seis drivers arquitectónicos principales:

| Driver | Prioridad | Influencia principal |
|---|---|---|
| DA-01: Alta Concurrencia y Rendimiento | Crítica | Backend stateless, balanceo y caché distribuido |
| DA-02: Consistencia en Operaciones Críticas | Crítica | Transacciones ACID, unicidad e idempotencia |
| DA-03: Escalabilidad y Elasticidad | Alta | Escalamiento horizontal y automático |
| DA-04: Seguridad y Privacidad | Crítica | Protección de credenciales, voto y datos |
| DA-05: Disponibilidad y Resiliencia | Alta | Réplicas, health checks y recuperación |
| DA-06: Modularidad y Evolución | Alta | Monolito modular y evolución futura |

Los drivers permiten justificar posteriormente las decisiones arquitectónicas del proyecto.

---

# Arquitectura del Sistema

## 7. Arquitectura Inicial

**Archivo:** [`arquitectura/arquitectura-inicial.md`](arquitectura/arquitectura-inicial.md)

La propuesta inicial establece una arquitectura basada en un **Monolito Modular Escalable**.

El sistema se organiza mediante módulos funcionales con responsabilidades claramente separadas.

### Módulos principales

1. Identidad y Usuarios.
2. Electoral.
3. Encuestas.
4. Eventos.
5. Comunidad.
6. Incentivos.
7. Notificaciones.
8. Auditoría.
9. Administración.

### Características principales

- Backend stateless.
- Escalamiento horizontal.
- Transacciones ACID.
- Caché distribuido.
- Procesamiento asíncrono.
- Comunicación en tiempo real.
- Balanceo de carga.
- Observabilidad.
- Posibilidad de evolución arquitectónica.

El módulo Electoral es considerado especialmente crítico debido a sus requisitos de consistencia, privacidad, auditoría y alta concurrencia.

---

## 8. Decisiones Arquitectónicas

**Archivo:** [`arquitectura/decisiones-arquitectonicas.md`](arquitectura/decisiones-arquitectonicas.md)

Las decisiones arquitectónicas se derivan de los drivers identificados para Campus UNSCH.

Estas decisiones buscan responder principalmente a:

- Alta concurrencia.
- Escalabilidad y elasticidad.
- Consistencia en operaciones críticas.
- Seguridad y privacidad.
- Disponibilidad.
- Mantenibilidad y evolución.

Las decisiones permiten justificar la forma en que se organizará y desplegará el sistema, evitando seleccionar tecnologías o estilos sin relación con las necesidades reales del proyecto.

---

## 9. Estilo Arquitectónico

**Archivo:** [`arquitectura/estilo-arquitectonico.md`](arquitectura/estilo-arquitectonico.md)

### Estilo seleccionado: Monolito Modular Escalable

Campus UNSCH se plantea inicialmente como una sola aplicación desplegable, pero organizada internamente mediante módulos funcionales independientes.

Este estilo permite:

- Mantener una menor complejidad operacional.
- Separar claramente las responsabilidades.
- Facilitar el mantenimiento.
- Utilizar transacciones consistentes.
- Escalar horizontalmente el backend.
- Evolucionar determinados módulos cuando sea necesario.

### ¿Por qué no iniciar con microservicios?

No se utilizan microservicios desde el inicio debido a que introducirían mayor complejidad operacional, comunicación distribuida y dificultades adicionales de consistencia.

El monolito modular permite comenzar con una arquitectura más controlable sin impedir una futura evolución.

El módulo Electoral podría convertirse posteriormente en un servicio independiente si la carga, criticidad o complejidad lo justifican.

---

## 10. Enfoque Arquitectónico

**Archivo:** [`arquitectura/enfoque/enfoque-arquitectonico.md`](arquitectura/enfoque/enfoque-arquitectonico.md)

### Enfoque seleccionado: Clean Architecture

Clean Architecture se utiliza para organizar internamente las responsabilidades y dependencias del sistema.

Las capas consideradas son:

| Capa | Responsabilidad |
|---|---|
| Presentación | Interacción con los usuarios mediante la aplicación web y API |
| Aplicación | Coordinación de casos de uso |
| Dominio | Entidades y reglas principales del negocio |
| Infraestructura | Base de datos, caché, mensajería y servicios externos |

### Regla de dependencias

Las dependencias deben orientarse hacia las capas internas.

El Dominio contiene las reglas principales del negocio y no debe depender directamente de tecnologías como:

- PostgreSQL.
- Redis.
- RabbitMQ.
- Frameworks de interfaz o backend.

De esta manera, las reglas del negocio permanecen desacopladas de los detalles tecnológicos.

### Ejemplos de casos de uso

- Autenticar estudiante.
- Emitir voto.
- Responder encuesta.
- Inscribirse a un evento.
- Gestionar publicaciones.
- Asignar incentivos.

### Relación entre estilo y enfoque

Campus UNSCH utiliza:

**Estilo arquitectónico:** Monolito Modular Escalable.  
**Enfoque arquitectónico:** Clean Architecture.

No son conceptos contradictorios.

El Monolito Modular define la organización global y el despliegue del sistema, mientras que Clean Architecture organiza las responsabilidades y dependencias internas.

---

# Tecnologías Propuestas

| Componente | Tecnología |
|---|---|
| Frontend | React / Next.js |
| Backend | Node.js / NestJS |
| Base de Datos | PostgreSQL |
| Caché | Redis |
| Mensajería | RabbitMQ |
| Contenedores | Docker |
| Orquestación | Kubernetes |
| Balanceo | Nginx / Ingress |
| Monitoreo | Prometheus / Grafana |
| Pruebas de carga | k6 |

---

# Escenario Objetivo de Validación

La arquitectura deberá considerar como escenario objetivo:

- Hasta **2,500 usuarios concurrentes** durante procesos masivos.
- Latencia adecuada en operaciones críticas.
- Escalamiento horizontal del backend.
- Consistencia en la emisión de votos.
- Disponibilidad del sistema durante procesos críticos.
- Protección de los datos y privacidad de los estudiantes.

Estos valores representan objetivos de diseño y validación, no mediciones actuales del sistema.

---

# Navegación Rápida

## Análisis

- [Actores](analisis-de-sistema/01-actores.md)
- [Historias de Usuario](analisis-de-sistema/02-historias-del-usuario.md)
- [Requisitos Funcionales](analisis-de-sistema/03-requisitos-funcionales.md)
- [Atributos de Calidad](analisis-de-sistema/04-atributos-de-calidad.md)
- [Restricciones](analisis-de-sistema/05-restricciones.md)
- [Drivers Arquitectónicos](analisis-de-sistema/06-driver-arquitectonicos.md)

## Arquitectura

- [Arquitectura Inicial](arquitectura/arquitectura-inicial.md)
- [Decisiones Arquitectónicas](arquitectura/decisiones-arquitectonicas.md)
- [Estilo Arquitectónico](arquitectura/estilo-arquitectonico.md)
- [Enfoque Arquitectónico](arquitectura/enfoque/enfoque-arquitectonico.md)

## Propuesta

- [Propuesta del Proyecto](propuesta/Propuesta-plataforma%20integral.pdf)

---

**Entregable 02:** Análisis de Caso - Arquitectura  
**Entregable 03:** Análisis y Selección de Estilos y Enfoques Arquitectónicos  

**Fecha:** Octubre 2026