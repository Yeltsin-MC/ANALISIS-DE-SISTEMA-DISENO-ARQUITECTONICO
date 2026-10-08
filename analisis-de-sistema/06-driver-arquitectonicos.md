# Drivers Arquitectónicos - Campus UNSCH

## Introducción

Los drivers arquitectónicos son aquellos requisitos, atributos de calidad o restricciones que tienen un impacto significativo en la arquitectura del sistema y que condicionan decisiones arquitectónicas importantes. NO todos los requisitos son drivers; solo aquellos que realmente determinan la forma, estructura o tecnologías clave del sistema.

Este documento identifica los drivers arquitectónicos principales de Campus UNSCH y establece su trazabilidad hacia requisitos, atributos de calidad y decisiones arquitectónicas.

---

## DA-01: Alta Concurrencia y Rendimiento

### Descripción

El sistema debe soportar un **escenario objetivo de hasta 2,500 usuarios concurrentes** durante procesos masivos como votaciones electorales o inscripciones a eventos populares, manteniendo tiempos de respuesta aceptables (≤ 2 segundos en percentil 95 para operaciones críticas).

### Origen

- **Propuesta del proyecto:** Escenario objetivo de 2,500 usuarios concurrentes
- **Contexto real:** Población estudiantil de UNSCH de aproximadamente 15,000 estudiantes; uso normal de 200-500 usuarios diarios con picos de hasta 2,500 concurrentes durante procesos críticos

### Importancia

**Crítica**. Sin capacidad de manejar alta concurrencia, el sistema colapsaría durante procesos electorales o eventos masivos, generando pérdida de confianza institucional y fracaso del proyecto.

### Requisitos Relacionados

- RF-VOT-03: Emisión de voto
- RF-EVE-02: Inscripción con control de aforo concurrente
- RF-ENC-03: Respuesta de encuestas

### Atributos de Calidad Relacionados

- AC-REN-01: Tiempo de respuesta en autenticación
- AC-REN-02: Latencia en emisión de voto
- AC-REN-04: Throughput de solicitudes HTTP
- AC-ESC-01: Escalado horizontal del backend
- AC-ESC-02: Escalado de base de datos

### Consecuencias Arquitectónicas

1. **Backend stateless:** Necesidad de múltiples instancias sin afinidad de sesión
2. **Balanceo de carga:** Nginx/Ingress para distribuir tráfico entre instancias
3. **Caché distribuido:** Redis para reducir carga en base de datos
4. **Pool de conexiones optimizado:** Gestión eficiente de conexiones a PostgreSQL
5. **Índices de base de datos:** Optimización de consultas críticas
6. **Pruebas de carga obligatorias:** Validación con k6 del escenario objetivo

---

## DA-02: Consistencia en Operaciones Críticas

### Descripción

El sistema debe garantizar consistencia absoluta en operaciones críticas:
- **Voto único:** Un estudiante solo puede votar una vez por proceso electoral
- **Control de aforo:** No superar el aforo máximo de eventos
- **Asignación de puntos:** No duplicar asignación de incentivos por la misma actividad

### Origen

- **Requerimientos del proyecto:** Integridad de procesos electorales y transaccionales
- **Atributos de calidad:** AC-CON-01, AC-CON-02, AC-CON-03

### Importancia

**Crítica**. La inconsistencia en estas operaciones comprometería la integridad del sistema, generando votos duplicados, sobrecupos en eventos o fraude en incentivos.

### Requisitos Relacionados

- RF-VOT-03: Emisión de voto único
- RF-VOT-08: Manejo de concurrencia en emisión de votos
- RF-EVE-02: Inscripción con control de aforo concurrente
- RF-INC-01: Asignación idempotente de puntos

### Atributos de Calidad Relacionados

- AC-CON-01: Voto único garantizado
- AC-CON-02: Control de aforo sin sobrepaso
- AC-CON-03: Asignación idempotente de puntos

### Consecuencias Arquitectónicas

1. **Transacciones ACID:** PostgreSQL con nivel de aislamiento adecuado (READ COMMITTED o superior)
2. **Restricciones de unicidad:** Constraints en base de datos (estudiante + proceso para votos)
3. **Operaciones idempotentes:** Claves de idempotencia para asignación de puntos
4. **Bloqueos optimistas o pesimistas:** Control de concurrencia en aforo de eventos
5. **Validaciones en capa de aplicación y base de datos:** Doble capa de protección
6. **Auditoría de inconsistencias:** Registro de intentos de operaciones duplicadas

---

## DA-03: Escalabilidad y Elasticidad

### Descripción

El sistema debe escalar horizontalmente para responder a incrementos de carga y reducir recursos cuando la demanda disminuye. La arquitectura debe permitir agregar/quitar instancias de backend sin interrumpir el servicio.

### Origen

- **Propuesta del proyecto:** Arquitectura escalable y elástica
- **Escenario objetivo:** 2,500 usuarios concurrentes durante picos; carga moderada en operación normal (200-500 usuarios diarios)

### Importancia

**Crítica**. La capacidad de escalar determina si el sistema puede manejar picos de demanda sin colapsar y si puede optimizar costos reduciendo recursos cuando no son necesarios.

### Requisitos Relacionados

- Arquitectura general del sistema
- RF-REP-05: Indicadores de rendimiento

### Atributos de Calidad Relacionados

- AC-ESC-01: Escalado horizontal del backend
- AC-ESC-03: Escalabilidad de caché distribuido
- AC-ELA-01: Escalado automático ante carga variable
- AC-ELA-02: Reducción de recursos tras finalización de pico
- AC-ELA-03: Pre-escalado para eventos programados

### Consecuencias Arquitectónicas

1. **Backend stateless:** Sin afinidad de sesión; estado en Redis/BD
2. **Contenedorización:** Docker para empaquetado consistente
3. **Orquestación con Kubernetes:** HPA (Horizontal Pod Autoscaler) para escalado automático
4. **Balanceador de carga:** Distribución dinámica de tráfico
5. **Métricas de observabilidad:** Prometheus para decisiones de escalado
6. **Configuración de políticas de escalado:** Umbrales de CPU, memoria y solicitudes/segundo

---

## DA-04: Seguridad y Privacidad

### Descripción

El sistema debe proteger la privacidad del voto estudiantil, almacenar credenciales de forma segura, prevenir ataques comunes (inyección SQL, XSS, fuerza bruta) y cumplir con normativa de protección de datos personales.

### Origen

- **Requerimientos del proyecto:** Privacidad electoral, seguridad de autenticación
- **Restricciones:** RES-21, RES-22, RES-23
- **Marco legal:** Protección de datos personales

### Importancia

**Crítica**. Comprometer la seguridad o privacidad del voto invalidaría el proceso electoral. La exposición de datos personales generaría responsabilidad legal e institucional.

### Requisitos Relacionados

- RF-USR-03: Autenticación de usuarios
- RF-USR-06: Cambio de contraseña
- RF-VOT-03: Emisión de voto
- RF-VOT-04: Registro de participación sin revelar elección

### Atributos de Calidad Relacionados

- AC-SEG-01: Protección de credenciales
- AC-SEG-02: Protección contra fuerza bruta
- AC-SEG-03: Privacidad del voto
- AC-SEG-04: Validación de entrada y prevención de inyección

### Consecuencias Arquitectónicas

1. **Hash seguro de contraseñas:** bcrypt con cost factor ≥ 10
2. **Separación de participación y voto:** Tablas independientes sin relación directa
3. **Rate limiting:** Redis para control de intentos de autenticación
4. **Validación y sanitización:** Validación en todas las entradas de usuario
5. **ORM y consultas parametrizadas:** Prevención de inyección SQL
6. **HTTPS obligatorio:** Comunicación cifrada en producción
7. **Auditoría sin exposición de votos:** Logs que NO revelan contenido del voto

---

## DA-05: Disponibilidad y Resiliencia

### Descripción

El sistema debe mantener disponibilidad ≥ 99.5% mensual y recuperarse automáticamente de fallos de instancias backend. Durante procesos críticos (votaciones), la disponibilidad es aún más crítica.

### Origen

- **Requerimientos del proyecto:** Tolerancia a fallos, recuperación automática
- **Atributos de calidad:** AC-DIS-01, AC-RES-01, AC-RES-02, AC-RES-03

### Importancia

**Alta**. La indisponibilidad durante un proceso electoral impediría la participación estudiantil y comprometería la legitimidad del proceso.

### Requisitos Relacionados

- Arquitectura general del sistema
- RF-ADM-05: Gestión de respaldos

### Atributos de Calidad Relacionados

- AC-DIS-01: Disponibilidad del sistema
- AC-RES-01: Recuperación ante fallo de instancia backend
- AC-RES-02: Recuperación ante fallo de base de datos
- AC-RES-03: Manejo de errores transitorios

### Consecuencias Arquitectónicas

1. **Múltiples réplicas del backend:** ≥ 2 instancias en producción
2. **Health checks:** Kubernetes detecta y reinicia instancias fallidas
3. **Reintentos con backoff exponencial:** Manejo de errores transitorios
4. **Respaldos automáticos de base de datos:** Programación periódica
5. **Réplicas de lectura (opcional):** Para alta disponibilidad de PostgreSQL
6. **Monitoreo y alertas:** Prometheus/Grafana para detección proactiva de fallos

---

## DA-06: Modularidad y Evolución Arquitectónica

### Descripción

El sistema debe separar claramente los dominios funcionales (Identidad, Electoral, Encuestas, Eventos, Comunidad, Incentivos, Notificaciones, Auditoría, Administración) para facilitar mantenimiento, evolución y eventual extracción de módulos como microservicios si la complejidad o necesidades de escalado lo justifican.

### Origen

- **Propuesta del proyecto:** Arquitectura modular
- **Decisión arquitectónica inicial:** Monolito modular con límites claros

### Importancia

**Alta**. La modularidad determina la mantenibilidad a largo plazo y la capacidad de evolucionar la arquitectura sin reescrituras completas.

### Requisitos Relacionados

- Arquitectura general del sistema
- Todos los módulos funcionales

### Atributos de Calidad Relacionados

- AC-MOD-01: Separación modular de dominios
- AC-MOD-02: Extracción de módulo Electoral como microservicio

### Consecuencias Arquitectónicas

1. **Monolito modular como arquitectura inicial:** Separación lógica con límites claros
2. **Interfaces bien definidas entre módulos:** Contratos claros
3. **Bajo acoplamiento:** Módulos NO acceden directamente a datos de otros módulos
4. **Alta cohesión:** Funcionalidad relacionada agrupada en el mismo módulo
5. **Posibilidad de extracción futura:** Módulo Electoral como candidato a microservicio
6. **Arquitectura hexagonal/limpia (opcional):** Separación de lógica de negocio e infraestructura

---

## Matriz de Trazabilidad Arquitectónica

| Driver | Requisitos Clave | Atributos de Calidad | Decisiones Arquitectónicas |
|--------|------------------|----------------------|----------------------------|
| DA-01: Alta Concurrencia | RF-VOT-03, RF-EVE-02 | AC-REN-02, AC-REN-04, AC-ESC-01 | Backend stateless, Balanceo, Caché distribuido |
| DA-02: Consistencia | RF-VOT-03, RF-VOT-08, RF-EVE-02 | AC-CON-01, AC-CON-02, AC-CON-03 | Transacciones ACID, Restricciones de unicidad, Idempotencia |
| DA-03: Escalabilidad | Arquitectura general | AC-ESC-01, AC-ELA-01, AC-ELA-02 | Kubernetes HPA, Docker, Métricas |
| DA-04: Seguridad | RF-USR-03, RF-VOT-04 | AC-SEG-01, AC-SEG-03, AC-SEG-04 | Hash bcrypt, Separación voto/participación, Rate limiting |
| DA-05: Disponibilidad | Arquitectura general, RF-ADM-05 | AC-DIS-01, AC-RES-01, AC-RES-02 | Múltiples réplicas, Health checks, Respaldos |
| DA-06: Modularidad | Todos los módulos | AC-MOD-01, AC-MOD-02 | Monolito modular, Límites claros, Evolución futura |

---

## Priorización de Drivers

| Driver | Prioridad | Justificación |
|--------|-----------|---------------|
| DA-01 | Crítica | Sin manejo de concurrencia, el sistema falla en su escenario objetivo |
| DA-02 | Crítica | Inconsistencia compromete integridad de votos y eventos |
| DA-04 | Crítica | Seguridad y privacidad son requisitos no negociables |
| DA-03 | Alta | Escalabilidad permite cumplir con escenario objetivo |
| DA-05 | Alta | Disponibilidad es clave durante procesos críticos |
| DA-06 | Alta | Modularidad facilita mantenimiento y evolución |