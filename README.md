# Diseño de una arquitectura de software escalable y elástica para una plataforma integral de servicios y participación estudiantil en la UNSCH – 2026

## Información del Proyecto

**Universidad:** Universidad Nacional de San Cristóbal de Huamanga  
**Escuela Profesional:** Ingeniería de Sistemas  
**Curso:** Arquitectura de Software  
**Docente:** Ing. Lizbet Jaico Quispe

**Estudiante:** Yeltsin Wilber Muñoz Corichahua  

**Nombre del Sistema:** Campus UNSCH

## Descripción

Campus UNSCH es una plataforma integral diseñada para centralizar la participación estudiantil y los servicios universitarios en la Universidad Nacional de San Cristóbal de Huamanga. El sistema busca proporcionar un entorno web seguro, escalable y elástico que permita a los estudiantes acceder a votaciones electrónicas, encuestas académicas, eventos universitarios, comunidad estudiantil y mecanismos de incentivos desde una única plataforma.

## Problema Abordado

La participación estudiantil en la UNSCH se encuentra fragmentada en múltiples canales y carece de una plataforma unificada que garantice:
- Procesos electorales transparentes y auditables
- Acceso centralizado a encuestas académicas
- Gestión eficiente de eventos universitarios
- Espacios de interacción estudiantil moderados
- Sistemas de incentivos controlados

## Objetivo

Diseñar y construir una arquitectura de software escalable y elástica que permita soportar hasta 2,500 usuarios concurrentes durante procesos masivos, garantizando consistencia, seguridad, disponibilidad y auditabilidad en todas las operaciones críticas del sistema.

## Estructura del Repositorio

```
.
├── propuesta/                    # Propuesta oficial del proyecto (PDF)
├── analisis-de-sistema/          # Documentación de análisis
│   ├── 01-actores.md
│   ├── 02-historias-del-usuario.md
│   ├── 03-requisitos-funcionales.md
│   ├── 04-atributos-de-calidad.md
│   ├── 05-restricciones.md
│   └── 06-driver-arquitectonicos.md
├── arquitectura/                 # Documentación arquitectónica
│   └── arquitectura-inicial.md
└── README.md                     # Este archivo
```

---

## Documentación del Análisis de Sistema

### 1. Actores del Sistema
**Archivo:** [`analisis-de-sistema/01-actores.md`](analisis-de-sistema/01-actores.md)

Identifica y caracteriza los 7 actores del sistema:
- **Actores humanos:** Estudiante, Docente/Tesista, Moderador, Autoridad Electoral, Administrador
- **Sistemas externos:** Padrón Institucional UNSCH, Correo Institucional UNSCH

Incluye diagrama de contexto en Mermaid mostrando las interacciones.

---

### 2. Historias de Usuario
**Archivo:** [`analisis-de-sistema/02-historias-del-usuario.md`](analisis-de-sistema/02-historias-del-usuario.md)

Documenta **47 historias de usuario** organizadas en 8 dominios funcionales:
- Identidad y Usuarios (9 historias)
- Votación Electoral (8 historias)
- Encuestas e Investigación (5 historias)
- Eventos (5 historias)
- Comunidad Estudiantil (5 historias)
- Incentivos y Puntos (5 historias)
- Administración (5 historias)
- Reportes y Análisis (5 historias)

Cada historia incluye actor, prioridad y criterios de aceptación detallados.

---

### 3. Requisitos Funcionales
**Archivo:** [`analisis-de-sistema/03-requisitos-funcionales.md`](analisis-de-sistema/03-requisitos-funcionales.md)

Define **48 requisitos funcionales** derivados de las historias de usuario, organizados por dominio. Incluye matriz de trazabilidad Historia → Requisito.

**Requisitos críticos destacados:**
- RF-VOT-03: Emisión de voto único (con control de concurrencia)
- RF-VOT-04: Registro de participación sin revelar elección
- RF-EVE-02: Inscripción con control de aforo concurrente
- RF-INC-01: Asignación idempotente de puntos

---

### 4. Atributos de Calidad
**Archivo:** [`analisis-de-sistema/04-atributos-de-calidad.md`](analisis-de-sistema/04-atributos-de-calidad.md)

Documenta **29 atributos de calidad** mediante escenarios estructurados en 9 categorías:
- **Rendimiento:** Latencia ≤ 2s en emisión de voto (p95), throughput ≥ 1,500 req/s
- **Escalabilidad:** Escalado horizontal, pool de conexiones optimizado
- **Elasticidad:** Escalado automático ante carga, pre-escalado programado
- **Seguridad:** Hash bcrypt, rate limiting, privacidad del voto
- **Consistencia:** Voto único garantizado, control de aforo, idempotencia
- **Disponibilidad:** ≥ 99.5% mensual
- **Resiliencia:** Recuperación automática ante fallos
- **Auditabilidad:** Trazabilidad completa sin exponer votos individuales
- **Observabilidad:** Métricas con Prometheus/Grafana
- **Modificabilidad:** Separación modular, evolución arquitectónica

**Nota importante:** El escenario de 2,500 usuarios concurrentes es un **objetivo de dimensionamiento y prueba** para procesos masivos (como elecciones), NO una medición actual. La UNSCH cuenta con aproximadamente 15,000 estudiantes, pero el uso normal estimado es de 200-500 usuarios diarios.

---

### 5. Restricciones
**Archivo:** [`analisis-de-sistema/05-restricciones.md`](analisis-de-sistema/05-restricciones.md)

Identifica **24 restricciones** en 6 categorías:
- **Alcance:** Plataforma web responsive, NO reemplazo de sistemas académicos
- **Institucionales:** Validación con Padrón UNSCH, correo institucional obligatorio
- **Tecnológicas obligatorias:** Docker, Kubernetes, Prometheus/Grafana, k6
- **Decisiones tecnológicas propuestas:** React/Next.js, Node.js/NestJS, PostgreSQL, Redis, RabbitMQ
- **Seguridad:** HTTPS, bcrypt, protección de datos personales
- **Académicas:** Proyecto académico, requiere validación institucional para producción

---

### 6. Drivers Arquitectónicos
**Archivo:** [`analisis-de-sistema/06-driver-arquitectonicos.md`](analisis-de-sistema/06-driver-arquitectonicos.md)

Identifica **6 drivers arquitectónicos** que condicionan decisiones importantes:

| Driver | Prioridad | Impacto Arquitectónico |
|--------|-----------|------------------------|
| DA-01: Alta Concurrencia | Crítica | Backend stateless, balanceo, caché distribuido |
| DA-02: Consistencia | Crítica | Transacciones ACID, restricciones de unicidad, idempotencia |
| DA-03: Escalabilidad | Alta | Kubernetes HPA, Docker, múltiples réplicas |
| DA-04: Seguridad | Crítica | Hash bcrypt, separación voto/participación, rate limiting |
| DA-05: Disponibilidad | Alta | Múltiples réplicas, health checks, respaldos automáticos |
| DA-06: Modularidad | Alta | Monolito modular, límites claros, evolución futura |

Incluye matriz de trazabilidad: Driver → Requisitos → Atributos → Decisiones Arquitectónicas.

---

## Arquitectura del Sistema

### Arquitectura Inicial
**Archivo:** [`arquitectura/arquitectura-inicial.md`](arquitectura/arquitectura-inicial.md)

**Decisión arquitectónica principal:** MONOLITO MODULAR ESCALABLE

#### Justificación

**NO se inicia con microservicios porque:**
- Complejidad operacional prematura para proyecto académico
- Transacciones ACID nativas simplifican garantía de consistencia
- Menor overhead de comunicación y latencia
- Límites de dominio aún en evolución

**Se adopta monolito MODULAR porque:**
- Separación lógica clara entre 9 módulos funcionales
- Escalamiento horizontal posible (backend stateless)
- Menor complejidad operacional
- Evolución futura hacia microservicios si es necesario

#### Módulos Funcionales

1. **Identidad y Usuarios** - Autenticación, roles, perfiles
2. **Electoral** - Procesos electorales, emisión de votos *(candidato a microservicio futuro)*
3. **Encuestas** - Creación, publicación, respuestas
4. **Eventos** - Publicación, inscripción, asistencia, certificados
5. **Comunidad** - Publicaciones, comentarios, moderación
6. **Incentivos** - Asignación, consulta y canje de puntos
7. **Notificaciones** - Correos y WebSocket
8. **Auditoría** - Registro inmutable de eventos críticos
9. **Administración** - Panel admin, configuración, respaldos

#### Stack Tecnológico Propuesto

| Componente | Tecnología | Justificación |
|------------|------------|---------------|
| Frontend | React / Next.js | UI moderna, responsive, component-based |
| Backend | Node.js / NestJS | TypeScript, arquitectura modular nativa |
| Base de Datos | PostgreSQL | ACID robusto, transacciones complejas |
| Caché | Redis | Baja latencia, estructuras ricas, locks distribuidos |
| Mensajería | RabbitMQ | Colas confiables, procesamiento asíncrono |
| Contenedores | Docker | Portabilidad, consistencia entre entornos |
| Orquestación | Kubernetes | Escalado automático, health checks, resiliencia |
| Balanceo | Nginx / Ingress | Distribución de carga, enrutamiento |
| Monitoreo | Prometheus / Grafana | Métricas, dashboards, alertas |
| Pruebas de Carga | k6 | Validación del escenario objetivo (2,500 concurrentes) |

#### Diagramas Arquitectónicos

El documento de arquitectura incluye 4 diagramas Mermaid:
1. **Diagrama de Módulos Funcionales** - Dependencias entre módulos
2. **Diagrama de Arquitectura Lógica** - Capas y flujo de comunicación
3. **Diagrama de Despliegue Escalable** - Kubernetes, pods, services
4. **Secuencia de Emisión de Voto** - Flujo crítico con transacciones

#### Características Clave

- **Backend stateless:** Sesiones en Redis, escalado horizontal sin afinidad
- **Transacciones ACID:** Consistencia garantizada en votos y aforo
- **Caché distribuido:** Reducción de carga en BD, rate limiting
- **Procesamiento asíncrono:** RabbitMQ para correos, certificados
- **WebSockets:** Notificaciones en tiempo real
- **HPA (Horizontal Pod Autoscaler):** 2-10 réplicas según carga
- **Observabilidad:** Prometheus + Grafana con alertas configuradas

#### Módulo Electoral: Consideraciones Especiales

- **Privacidad arquitectónica:** Tablas separadas para votos y participación (sin correlación directa)
- **Voto único:** Restricción de unicidad (estudiante + proceso)
- **Auditoría completa:** Sin revelar votos individuales
- **Candidato a extracción:** Puede evolucionar a microservicio si se justifica

---

## Resumen Cuantitativo

| Elemento | Cantidad |
|----------|----------|
| Actores | 7 (5 humanos + 2 sistemas externos) |
| Historias de Usuario | 47 |
| Requisitos Funcionales | 48 |
| Atributos de Calidad | 29 |
| Restricciones | 24 |
| Drivers Arquitectónicos | 6 |
| Módulos Funcionales | 9 |
| Diagramas Mermaid | 5 |

---

## Decisiones Arquitectónicas Principales

1. **Monolito Modular Stateless** como arquitectura inicial
2. **Backend Node.js + NestJS** para aprovechar TypeScript y modularidad
3. **PostgreSQL** para transacciones ACID robustas
4. **Redis** para caché distribuido y sesiones
5. **Separación arquitectónica de voto y participación** para privacidad
6. **Kubernetes con HPA** para escalado automático

---

## Escenario Objetivo de Validación

- **2,500 usuarios concurrentes** durante procesos masivos
- **Latencia p95 ≤ 2 segundos** en operaciones críticas
- **Throughput ≥ 250 solicitudes/segundo**
- **Disponibilidad ≥ 99.5%** mensual

Estos objetivos deben validarse mediante pruebas de carga con k6.

---

## Navegación Rápida

- [Propuesta del Proyecto (PDF)](propuesta/Propuesta-plataforma%20integral.pdf)
- [Actores](analisis-de-sistema/01-actores.md)
- [Historias de Usuario](analisis-de-sistema/02-historias-del-usuario.md)
- [Requisitos Funcionales](analisis-de-sistema/03-requisitos-funcionales.md)
- [Atributos de Calidad](analisis-de-sistema/04-atributos-de-calidad.md)
- [Restricciones](analisis-de-sistema/05-restricciones.md)
- [Drivers Arquitectónicos](analisis-de-sistema/06-driver-arquitectonicos.md)
- [Arquitectura Inicial](arquitectura/arquitectura-inicial.md)

---

**Entregable 02:** Análisis de Caso - Arquitectura

**Fecha:** Octubre 2026
