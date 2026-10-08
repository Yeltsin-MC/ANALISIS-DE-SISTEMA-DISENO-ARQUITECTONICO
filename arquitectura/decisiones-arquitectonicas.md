# Decisiones Arquitectónicas - Campus UNSCH

## Introducción

Este documento registra las decisiones arquitectónicas principales del proyecto Campus UNSCH. Cada decisión está vinculada a uno o más drivers arquitectónicos identificados y justifica la solución adoptada sobre las alternativas consideradas.

Las decisiones documentadas responden a los 6 drivers arquitectónicos críticos del sistema: alta concurrencia, consistencia, escalabilidad, seguridad/privacidad, disponibilidad y modularidad.

---

## Decisiones Arquitectónicas

| ID | Decisión Arquitectónica | Driver Relacionado | Justificación | Resultado |
|----|-------------------------|-------------------|---------------|-----------|
| **DA-01** | **Monolito Modular con Límites Claros** | DA-06: Modularidad y Evolución Arquitectónica | Se adopta un monolito modular en lugar de microservicios desde el inicio para reducir complejidad operacional prematura, facilitar transacciones ACID nativas y simplificar el desarrollo en contexto académico. Los 9 módulos funcionales (Identidad, Electoral, Encuestas, Eventos, Comunidad, Incentivos, Notificaciones, Auditoría, Administración) mantienen límites claros con interfaces bien definidas y bajo acoplamiento. | Arquitectura más simple de operar y desarrollar, con capacidad de evolución futura hacia microservicios si se justifica. Transacciones ACID nativas garantizan consistencia sin coordinación distribuida. El módulo Electoral queda identificado como candidato principal para extracción futura. |
| **DA-02** | **Backend Stateless con Escalado Horizontal** | DA-01: Alta Concurrencia y Rendimiento<br/>DA-03: Escalabilidad y Elasticidad | El backend se diseña completamente stateless, almacenando sesiones y estado en Redis. Esto permite desplegar múltiples instancias (2-10 réplicas según carga) sin afinidad de sesión, distribuyendo tráfico mediante Nginx/Ingress. Kubernetes HPA gestiona escalado automático basado en CPU (70%) y memoria (80%). Se implementa pre-escalado manual antes de procesos electorales programados. | Capacidad de manejar el escenario objetivo de 2,500 usuarios concurrentes durante picos mediante escalado horizontal. Reducción automática de recursos cuando la demanda disminuye (elasticidad). Respuesta rápida ante incrementos de carga mediante HPA. |
| **DA-03** | **Transacciones ACID con Restricciones de Unicidad** | DA-02: Consistencia en Operaciones Críticas | PostgreSQL con transacciones ACID y nivel de aislamiento READ COMMITTED garantiza consistencia en operaciones críticas. Se implementan restricciones de unicidad en base de datos: `UNIQUE (estudiante_id, proceso_id)` para votos, `UNIQUE (estudiante_id, evento_id)` para inscripciones. Las operaciones de puntos utilizan claves de idempotencia. Se aplica validación en doble capa: aplicación y base de datos. | Voto único garantizado arquitectónicamente: imposible votar dos veces en el mismo proceso incluso bajo alta concurrencia. Control de aforo sin sobrepaso: las restricciones de BD previenen inscripciones que excedan el límite. Asignación de puntos sin duplicados. Auditoría de intentos de operaciones inválidas. |
| **DA-04** | **Separación Arquitectónica de Voto y Participación** | DA-04: Seguridad y Privacidad | Para garantizar privacidad del voto, se diseñan dos tablas completamente independientes: (1) `participacion` registra QUE el estudiante votó (con estudiante_id), (2) `votos` registra QUE opción recibió voto (SIN estudiante_id). No existe join ni relación directa entre ambas tablas. Las credenciales se protegen con bcrypt (cost factor ≥10). Rate limiting con Redis previene fuerza bruta (5 intentos/10 min). ORM y consultas parametrizadas previenen inyección SQL. | Imposibilidad arquitectónica de correlacionar estudiante con voto emitido: ni administradores ni atacantes pueden determinar por quién votó un estudiante específico. Protección robusta de credenciales. Prevención de ataques comunes (inyección SQL, XSS, fuerza bruta). Auditoría electoral completa sin comprometer privacidad individual. |
| **DA-05** | **Caché Distribuido y Procesamiento Asíncrono** | DA-01: Alta Concurrencia y Rendimiento<br/>DA-05: Disponibilidad y Resiliencia | Redis actúa como caché distribuido compartido entre instancias del backend para: sesiones de usuario, rate limiting, consultas frecuentes (listado de eventos, padrón habilitado), locks distribuidos. RabbitMQ desacopla tareas asíncronas: envío de correos, generación de certificados PDF, procesamiento de reportes. Workers procesan colas en background con reintentos automáticos. | Reducción significativa de carga en PostgreSQL mediante caché (hit ratio objetivo ≥80%). Latencia de acceso a caché ≤10ms vs ~100ms en BD. Respuesta inmediata al usuario en operaciones asíncronas (correos, certificados). Resiliencia: si un worker falla, la tarea se reintenta automáticamente. Escalado independiente de workers según profundidad de colas. |

---

## Decisiones Complementarias

### Stack Tecnológico

| Componente | Tecnología Seleccionada | Justificación |
|------------|-------------------------|---------------|
| Frontend | React / Next.js | UI moderna, responsive, component-based. TypeScript end-to-end con el backend. |
| Backend | Node.js / NestJS | Arquitectura modular nativa, TypeScript, decoradores, soporte enterprise. |
| Base de Datos | PostgreSQL | ACID robusto, transacciones complejas, restricciones de integridad, rendimiento adecuado. |
| Caché/Sesiones | Redis | Baja latencia, estructuras de datos ricas, locks distribuidos, persistencia opcional. |
| Mensajería | RabbitMQ | Colas confiables, múltiples patrones de mensajería, reintentos automáticos. |
| Contenedores | Docker | Portabilidad, consistencia entre entornos de desarrollo y producción. |
| Orquestación | Kubernetes | HPA para escalado automático, health checks, self-healing, gestión de réplicas. |
| Monitoreo | Prometheus + Grafana | Métricas estándar de industria, alertas configurables, dashboards personalizables. |

---

## Alternativas Consideradas y Descartadas

### Microservicios desde el Inicio
**Descartado.** Complejidad operacional prematura para proyecto académico. Coordinación distribuida complica garantía de consistencia. Overhead de comunicación entre servicios. Límites de dominio aún en evolución. Se reserva para evolución futura si se justifica.

### MongoDB como Base de Datos Principal
**Descartado.** Transacciones ACID menos robustas que PostgreSQL. Mayor complejidad para garantizar consistencia en votos únicos. Integridad referencial requiere gestión manual. PostgreSQL ofrece mejor soporte para el caso de uso transaccional del proyecto.

### Cifrado de Votos con Clave del Estudiante
**Descartado** para privacidad electoral. Separación arquitectónica (tablas independientes sin relación) es más robusta que cifrado: elimina la posibilidad de correlación incluso si se comprometen claves. Cifrado podría romperse; separación arquitectónica es permanente.

### Caché en Memoria del Proceso Backend
**Descartado.** Cada instancia tendría caché independiente (ineficiente). Sesiones no compartidas entre instancias (requeriría sticky sessions). Locks distribuidos imposibles. Redis permite compartir estado entre todas las réplicas.

---

## Riesgos y Mitigaciones

| Riesgo Arquitectónico | Mitigación Adoptada |
|-----------------------|---------------------|
| Fallo de base de datos principal | Respaldos automáticos cada 6 horas; procedimiento de restauración documentado; considerar réplica de lectura en producción. |
| Saturación de conexiones a PostgreSQL | Pool de conexiones configurado y monitoreado; alertas cuando uso > 80%; posibilidad de escalar verticalmente la BD. |
| Inconsistencia en votos por concurrencia extrema | Restricciones de unicidad en BD (última línea de defensa); transacciones ACID; pruebas de carga exhaustivas con k6 simulando 2,500 concurrentes. |
| Compromiso de privacidad del voto | Separación arquitectónica permanente; auditoría de código del módulo Electoral; logs que NO registran contenido del voto. |
| Crecimiento del monolito | Límites modulares claros facilitan extracción futura; módulo Electoral identificado como candidato prioritario a microservicio si crece la complejidad. |

---

## Evolución Arquitectónica Prevista

### Fase Actual: Monolito Modular
- **Estado:** Arquitectura inicial adoptada
- **Capacidad:** Soporta escenario objetivo de 2,500 usuarios concurrentes
- **Ventajas:** Simplicidad operacional, transacciones nativas, menor complejidad

### Fase Futura (Si se justifica): Extracción Selectiva
- **Candidato prioritario:** Módulo Electoral como microservicio independiente
- **Justificación potencial:** Requisitos regulatorios de aislamiento, escalado independiente crítico, equipo dedicado a seguridad electoral
- **Preparación actual:** Límites claros, APIs bien definidas, mínima dependencia de otros módulos

### Fase Posterior (Largo plazo): Arquitectura Híbrida
- **Visión:** Módulos críticos como microservicios, módulos estables en monolito
- **Requisitos:** Madurez organizacional, justificación técnica clara, service mesh para comunicación

---

## Trazabilidad: Drivers → Decisiones

| Driver Arquitectónico | Decisión(es) que lo Atiende(n) |
|-----------------------|---------------------------------|
| DA-01: Alta Concurrencia y Rendimiento | DA-02 (Backend Stateless), DA-05 (Caché Distribuido) |
| DA-02: Consistencia en Operaciones Críticas | DA-03 (Transacciones ACID) |
| DA-03: Escalabilidad y Elasticidad | DA-02 (Backend Stateless con HPA) |
| DA-04: Seguridad y Privacidad | DA-04 (Separación Voto/Participación) |
| DA-05: Disponibilidad y Resiliencia | DA-02 (Múltiples Réplicas), DA-05 (Procesamiento Asíncrono) |
| DA-06: Modularidad y Evolución | DA-01 (Monolito Modular) |

---

## Conclusión

Las decisiones arquitectónicas adoptadas para Campus UNSCH responden directamente a los drivers identificados, priorizando:

1. **Simplicidad inicial** mediante monolito modular
2. **Escalabilidad horizontal** para manejar 2,500 usuarios concurrentes
3. **Consistencia garantizada** mediante transacciones ACID y restricciones de BD
4. **Privacidad robusta** mediante separación arquitectónica permanente
5. **Rendimiento optimizado** mediante caché distribuido y procesamiento asíncrono

La arquitectura establece una base sólida que puede evolucionar hacia microservicios selectivos cuando la complejidad o necesidades operacionales lo justifiquen.
