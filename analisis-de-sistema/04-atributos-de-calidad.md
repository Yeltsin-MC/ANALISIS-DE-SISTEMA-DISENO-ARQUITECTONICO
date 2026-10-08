# Atributos de Calidad - Campus UNSCH

## Introducción

Los atributos de calidad representan características no funcionales que el sistema debe satisfacer para cumplir con sus objetivos arquitectónicos. Este documento define atributos de calidad mediante escenarios estructurados que incluyen fuente del estímulo, estímulo, entorno, artefacto afectado, respuesta del sistema y medida de respuesta.

**Nota importante sobre concurrencia:**  
La propuesta del proyecto Campus UNSCH establece como **escenario objetivo de dimensionamiento y prueba** el manejo de hasta **2,500 usuarios concurrentes** durante procesos masivos. Este valor representa un objetivo arquitectónico que debe validarse mediante pruebas de carga (k6), NO una medición actual. La UNSCH cuenta con aproximadamente 15,000 estudiantes, pero el uso normal estimado es de 200 a 500 usuarios diarios, con picos de hasta 2,500 concurrentes durante elecciones. Los atributos de calidad definidos a continuación consideran este escenario objetivo como referencia para diseño y validación.

---

## 1. Rendimiento

### AC-REN-01: Tiempo de respuesta en autenticación

**Fuente del estímulo:** Estudiante autenticado  
**Estímulo:** Solicitud de inicio de sesión con credenciales válidas  
**Entorno:** Operación normal del sistema con carga moderada  
**Artefacto:** Módulo de Identidad y Autenticación  
**Respuesta:** El sistema valida credenciales, genera sesión y responde al usuario  
**Medida de respuesta:** Tiempo de respuesta ≤ 500 ms en el percentil 95

**Prioridad:** Alta  
**Requisitos relacionados:** RF-USR-03

---

### AC-REN-02: Latencia en emisión de voto

**Fuente del estímulo:** Estudiante habilitado  
**Estímulo:** Solicitud de emisión de voto durante proceso electoral abierto  
**Entorno:** Alta concurrencia durante pico electoral (escenario objetivo: 2,500 usuarios concurrentes)  
**Artefacto:** Módulo Electoral  
**Respuesta:** El sistema valida elegibilidad, registra voto mediante transacción y confirma  
**Medida de respuesta:** Tiempo de respuesta ≤ 2 segundos en el percentil 95 bajo escenario de alta concurrencia

**Prioridad:** Crítica  
**Requisitos relacionados:** RF-VOT-03, RF-VOT-08

---

### AC-REN-03: Rendimiento en consulta de eventos disponibles

**Fuente del estímulo:** Estudiante autenticado  
**Estímulo:** Solicitud de listado de eventos disponibles con cupos  
**Entorno:** Operación normal con múltiples eventos activos  
**Artefacto:** Módulo de Eventos  
**Respuesta:** El sistema consulta eventos, verifica cupos disponibles y responde  
**Medida de respuesta:** Tiempo de respuesta ≤ 300 ms en el percentil 95

**Prioridad:** Alta  
**Requisitos relacionados:** RF-EVE-02

---

### AC-REN-04: Throughput de solicitudes HTTP

**Fuente del estímulo:** Múltiples usuarios simultáneos  
**Estímulo:** Solicitudes HTTP concurrentes a diferentes endpoints  
**Entorno:** Escenario objetivo de alta concurrencia (2,500 usuarios concurrentes)  
**Artefacto:** Backend (Node.js/NestJS), Balanceador de carga  
**Respuesta:** El sistema procesa solicitudes distribuidas entre instancias  
**Medida de respuesta:** Throughput ≥ 250 solicitudes/segundo con múltiples instancias del backend

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura general

---

## 2. Escalabilidad

### AC-ESC-01: Escalado horizontal del backend

**Fuente del estímulo:** Administrador del sistema o sistema de orquestación  
**Estímulo:** Incremento de carga detectado mediante métricas  
**Entorno:** Proceso electoral o evento masivo con incremento sostenido de usuarios  
**Artefacto:** Backend stateless, Kubernetes/Orquestador  
**Respuesta:** El sistema despliega instancias adicionales del backend y distribuye carga  
**Medida de respuesta:** Tiempo de escalado ≤ 2 minutos; nuevas instancias operativas y recibiendo tráfico en menos de 3 minutos

**Prioridad:** Crítica  
**Requisitos relacionados:** Arquitectura general

---

### AC-ESC-02: Escalado de base de datos (conexiones)

**Fuente del estímulo:** Múltiples instancias del backend  
**Estímulo:** Incremento de solicitudes concurrentes a base de datos  
**Entorno:** Escenario objetivo de alta concurrencia (2,500 usuarios)  
**Artefacto:** Pool de conexiones PostgreSQL  
**Respuesta:** El sistema gestiona pool de conexiones eficientemente entre instancias  
**Medida de respuesta:** El sistema mantiene latencia de consultas ≤ 100 ms en percentil 95 con pool adecuadamente dimensionado

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura de datos

---

### AC-ESC-03: Escalabilidad de caché distribuido

**Fuente del estímulo:** Múltiples instancias del backend  
**Estímulo:** Consultas frecuentes de datos temporales y sesiones  
**Entorno:** Operación con múltiples instancias backend activas  
**Artefacto:** Redis (caché distribuido)  
**Respuesta:** El sistema comparte caché entre instancias para consultas de alta frecuencia  
**Medida de respuesta:** Hit ratio de caché ≥ 80% para datos frecuentes; latencia de acceso a caché ≤ 10 ms

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura de caché

---

### AC-ESC-04: Capacidad de crecimiento de datos

**Fuente del estímulo:** Operación continua del sistema  
**Estímulo:** Acumulación de datos históricos (votos, encuestas, eventos, comunidad)  
**Entorno:** Operación de varios semestres o años  
**Artefacto:** Base de datos PostgreSQL  
**Respuesta:** El sistema mantiene rendimiento adecuado mediante índices y particionamiento  
**Medida de respuesta:** Tiempo de consultas críticas se mantiene estable (≤ 10% de degradación) con crecimiento de hasta 10x en volumen de datos

**Prioridad:** Media  
**Requisitos relacionados:** Arquitectura de datos

---

## 3. Elasticidad

### AC-ELA-01: Escalado automático ante carga variable

**Fuente del estímulo:** Sistema de monitoreo (Prometheus/Kubernetes HPA)  
**Estímulo:** Aumento de uso de CPU > 70% o solicitudes/segundo > umbral configurado  
**Entorno:** Proceso electoral programado o evento masivo con carga creciente  
**Artefacto:** Backend, Orquestador (Kubernetes)  
**Respuesta:** El sistema despliega réplicas adicionales automáticamente  
**Medida de respuesta:** Escalado automático en ≤ 2 minutos cuando se supera umbral configurado

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura general, AC-ESC-01

---

### AC-ELA-02: Reducción de recursos tras finalización de pico

**Fuente del estímulo:** Sistema de monitoreo  
**Estímulo:** Reducción sostenida de carga (CPU < 30%, solicitudes por debajo de umbral)  
**Entorno:** Finalización de proceso electoral o evento masivo  
**Artefacto:** Backend, Orquestador  
**Respuesta:** El sistema reduce número de réplicas de backend  
**Medida de respuesta:** Reducción de réplicas en ≤ 10 minutos después de detectar carga baja sostenida

**Prioridad:** Media  
**Requisitos relacionados:** Arquitectura general

---

### AC-ELA-03: Pre-escalado para eventos programados

**Fuente del estímulo:** Administrador o sistema de programación  
**Estímulo:** Configuración de pre-escalado para proceso electoral programado  
**Entorno:** Horas o minutos antes del inicio de proceso crítico  
**Artefacto:** Backend, Orquestador  
**Respuesta:** El sistema incrementa réplicas preventivamente antes del evento  
**Medida de respuesta:** Réplicas adicionales desplegadas y operativas 15 minutos antes del inicio configurado

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura general

---

## 4. Seguridad

### AC-SEG-01: Protección de credenciales

**Fuente del estímulo:** Estudiante o atacante  
**Estímulo:** Intento de autenticación con credenciales  
**Entorno:** Operación normal o intento de ataque  
**Artefacto:** Módulo de autenticación, Base de datos  
**Respuesta:** El sistema almacena contraseñas con hash seguro (bcrypt), valida sin exponer información sensible  
**Medida de respuesta:** Todas las contraseñas almacenadas con algoritmo bcrypt (cost factor ≥ 10); mensajes de error no revelan si el correo existe

**Prioridad:** Crítica  
**Requisitos relacionados:** RF-USR-03, RF-USR-06

---

### AC-SEG-02: Protección contra fuerza bruta

**Fuente del estímulo:** Atacante  
**Estímulo:** Múltiples intentos fallidos de autenticación desde una IP  
**Entorno:** Intento de ataque de fuerza bruta  
**Artefacto:** Módulo de autenticación, Rate limiting (Redis)  
**Respuesta:** El sistema bloquea temporalmente intentos adicionales desde la IP  
**Medida de respuesta:** Bloqueo de IP después de 5 intentos fallidos en 10 minutos; bloqueo de 15 minutos

**Prioridad:** Alta  
**Requisitos relacionados:** RF-USR-03

---

### AC-SEG-03: Privacidad del voto

**Fuente del estímulo:** Administrador, autoridad electoral o atacante  
**Estímulo:** Intento de consultar por quién votó un estudiante específico  
**Entorno:** Durante o después de proceso electoral  
**Artefacto:** Módulo Electoral, Base de datos  
**Respuesta:** El sistema mantiene separadas participación y voto; NO permite trazabilidad individual  
**Medida de respuesta:** Imposibilidad arquitectónica de relacionar estudiante con voto emitido; auditorías externas confirman privacidad

**Prioridad:** Crítica  
**Requisitos relacionados:** RF-VOT-04

---

### AC-SEG-04: Validación de entrada y prevención de inyección

**Fuente del estímulo:** Usuario malicioso  
**Estímulo:** Envío de datos con código SQL, scripts o comandos maliciosos  
**Entorno:** Formularios, APIs, entradas de usuario  
**Artefacto:** Backend, Capa de servicios  
**Respuesta:** El sistema valida y sanitiza todas las entradas; utiliza consultas parametrizadas  
**Medida de respuesta:** 100% de consultas SQL utilizan ORM o consultas preparadas; validación de entrada en todas las APIs

**Prioridad:** Crítica  
**Requisitos relacionados:** Todos los módulos

---

## 5. Consistencia

### AC-CON-01: Voto único garantizado

**Fuente del estímulo:** Estudiante o múltiples solicitudes concurrentes del mismo estudiante  
**Estímulo:** Intentos de emitir voto múltiples veces en el mismo proceso  
**Entorno:** Alta concurrencia durante proceso electoral  
**Artefacto:** Módulo Electoral, Base de datos (PostgreSQL)  
**Respuesta:** El sistema utiliza transacciones y restricción de unicidad (estudiante + proceso) para impedir votos duplicados  
**Medida de respuesta:** 0 votos duplicados registrados; transacciones con nivel de aislamiento READ COMMITTED o superior

**Prioridad:** Crítica  
**Requisitos relacionados:** RF-VOT-03, RF-VOT-08

---

### AC-CON-02: Control de aforo sin sobrepaso

**Fuente del estímulo:** Múltiples estudiantes simultáneos  
**Estímulo:** Inscripciones concurrentes al último cupo disponible de un evento  
**Entorno:** Alta concurrencia en inscripción a evento popular  
**Artefacto:** Módulo de Eventos, Base de datos  
**Respuesta:** El sistema utiliza transacciones y bloqueos para garantizar que no se supere el aforo máximo  
**Medida de respuesta:** 0 inscripciones por encima del aforo configurado; control mediante transacciones y restricciones

**Prioridad:** Alta  
**Requisitos relacionados:** RF-EVE-02

---

### AC-CON-03: Asignación idempotente de puntos

**Fuente del estímulo:** Sistema o solicitudes duplicadas  
**Estímulo:** Múltiples intentos de asignar puntos por la misma actividad  
**Entorno:** Procesamiento asíncrono de incentivos  
**Artefacto:** Módulo de Incentivos, Base de datos  
**Respuesta:** El sistema utiliza claves de idempotencia para evitar asignaciones duplicadas  
**Medida de respuesta:** 0 asignaciones duplicadas de puntos por la misma actividad; operaciones idempotentes implementadas

**Prioridad:** Alta  
**Requisitos relacionados:** RF-INC-01

---

## 6. Disponibilidad y Resiliencia

### AC-DIS-01: Disponibilidad del sistema

**Fuente del estímulo:** Usuarios (estudiantes, docentes, administradores)  
**Estímulo:** Solicitudes de acceso al sistema  
**Entorno:** Operación continua durante 24/7  
**Artefacto:** Sistema completo (backend, BD, caché)  
**Respuesta:** El sistema está disponible y respondiendo correctamente  
**Medida de respuesta:** Disponibilidad ≥ 99.5% mensual (tiempo de inactividad ≤ 3.6 horas/mes)

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura general

---

### AC-RES-01: Recuperación ante fallo de instancia backend

**Fuente del estímulo:** Fallo de hardware, error de aplicación o proceso terminado  
**Estímulo:** Una instancia del backend falla  
**Entorno:** Operación con múltiples réplicas activas  
**Artefacto:** Backend, Balanceador de carga, Kubernetes  
**Respuesta:** El balanceador redirige tráfico a instancias saludables; Kubernetes reinicia instancia fallida  
**Medida de respuesta:** Detección de fallo en ≤ 30 segundos; tráfico redirigido sin pérdida de solicitudes activas; nueva instancia operativa en ≤ 2 minutos

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura general

---

### AC-RES-02: Recuperación ante fallo de base de datos

**Fuente del estímulo:** Fallo de hardware, corrupción de datos o error crítico  
**Estímulo:** Fallo de la instancia principal de PostgreSQL  
**Entorno:** Operación con configuración de respaldo o réplica  
**Artefacto:** Base de datos PostgreSQL  
**Respuesta:** El sistema detecta fallo y promueve réplica secundaria (si existe) o restaura desde respaldo  
**Medida de respuesta:** Detección en ≤ 1 minuto; promoción de réplica en ≤ 5 minutos; restauración desde respaldo ≤ 1 hora con pérdida de datos ≤ 15 minutos

**Prioridad:** Crítica  
**Requisitos relacionados:** RF-ADM-05

---

### AC-RES-03: Manejo de errores transitorios

**Fuente del estímulo:** Fallo temporal de red, timeout de BD o error transitorio  
**Estímulo:** Error transitorio en operación no crítica  
**Entorno:** Operación normal con fallos transitorios ocasionales  
**Artefacto:** Backend, Cliente de BD, Colas de mensajes  
**Respuesta:** El sistema reintenta operación con backoff exponencial  
**Medida de respuesta:** Máximo 3 reintentos con intervalos crecientes (1s, 2s, 4s); operación marcada como fallida después de reintentos

**Prioridad:** Media  
**Requisitos relacionados:** Arquitectura general

---

## 7. Auditabilidad

### AC-AUD-01: Trazabilidad de proceso electoral

**Fuente del estímulo:** Autoridad electoral o auditor externo  
**Estímulo:** Solicitud de auditoría de proceso electoral  
**Entorno:** Durante o después de proceso electoral  
**Artefacto:** Módulo Electoral, Base de datos de auditoría  
**Respuesta:** El sistema proporciona log completo de eventos del proceso (creación, apertura, votos emitidos por periodo, cierre, accesos administrativos)  
**Medida de respuesta:** 100% de eventos críticos registrados con timestamp, usuario responsable y acción realizada; logs inmutables

**Prioridad:** Crítica  
**Requisitos relacionados:** RF-VOT-07

---

### AC-AUD-02: Registro de cambios administrativos

**Fuente del estímulo:** Administrador  
**Estímulo:** Acción administrativa (cambio de rol, suspensión de cuenta, modificación de configuración)  
**Entorno:** Operación administrativa  
**Artefacto:** Módulo de Administración, Auditoría  
**Respuesta:** El sistema registra quién, qué, cuándo y desde dónde se realizó la acción  
**Medida de respuesta:** 100% de acciones administrativas auditadas; logs almacenados de forma inmutable; retención mínima de 2 años

**Prioridad:** Alta  
**Requisitos relacionados:** RF-ADM-03

---

### AC-AUD-03: Trazabilidad de asignación de incentivos

**Fuente del estímulo:** Administrador o auditor  
**Estímulo:** Solicitud de auditoría de sistema de incentivos  
**Entorno:** Revisión de integridad del sistema de puntos  
**Artefacto:** Módulo de Incentivos, Auditoría  
**Respuesta:** El sistema proporciona log de todas las asignaciones y canjes de puntos  
**Medida de respuesta:** 100% de movimientos de puntos auditados con estudiante, actividad, cantidad y timestamp; detección de anomalías

**Prioridad:** Alta  
**Requisitos relacionados:** RF-INC-01

---

## 8. Observabilidad

### AC-OBS-01: Monitoreo de métricas de rendimiento

**Fuente del estímulo:** Sistema de monitoreo (Prometheus)  
**Estímulo:** Recolección continua de métricas  
**Entorno:** Operación continua  
**Artefacto:** Backend, Base de datos, Redis, Sistema completo  
**Respuesta:** El sistema expone métricas de solicitudes/segundo, latencia, errores, uso de CPU, memoria, conexiones  
**Medida de respuesta:** Métricas actualizadas cada 15 segundos; retención de métricas durante 30 días; visualización en Grafana

**Prioridad:** Alta  
**Requisitos relacionados:** RF-REP-05

---

### AC-OBS-02: Alertas ante anomalías

**Fuente del estímulo:** Sistema de monitoreo  
**Estímulo:** Detección de condición anómala (tasa de errores > 5%, latencia > umbral, uso de CPU > 85%)  
**Entorno:** Operación con monitoreo activo  
**Artefacto:** Sistema de alertas (Prometheus Alertmanager)  
**Respuesta:** El sistema notifica a administradores mediante canal configurado  
**Medida de respuesta:** Alerta enviada en ≤ 1 minuto después de detectar condición anómala sostenida durante 2 minutos

**Prioridad:** Alta  
**Requisitos relacionados:** RF-REP-05

---

### AC-OBS-03: Logs estructurados y centralizados

**Fuente del estímulo:** Aplicación (backend, servicios)  
**Estímulo:** Eventos y errores durante operación  
**Entorno:** Operación continua  
**Artefacto:** Sistema de logging  
**Respuesta:** El sistema genera logs estructurados (JSON) con nivel, timestamp, contexto y trazabilidad  
**Medida de respuesta:** Logs centralizados; retención de 90 días; búsqueda y filtrado en ≤ 5 segundos

**Prioridad:** Media  
**Requisitos relacionados:** Arquitectura general

---

## 9. Modificabilidad

### AC-MOD-01: Separación modular de dominios

**Fuente del estímulo:** Desarrollador  
**Estímulo:** Necesidad de modificar lógica del módulo Electoral sin afectar otros módulos  
**Entorno:** Desarrollo o mantenimiento  
**Artefacto:** Código del backend (monolito modular)  
**Respuesta:** El sistema permite modificar módulo específico con impacto limitado  
**Medida de respuesta:** Cambio en un módulo afecta a ≤ 2 módulos relacionados; cambios aislados mediante interfaces y límites claros

**Prioridad:** Alta  
**Requisitos relacionados:** Arquitectura general

---

### AC-MOD-02: Extracción de módulo Electoral como microservicio

**Fuente del estímulo:** Arquitecto o equipo de desarrollo  
**Estímulo:** Decisión de extraer módulo Electoral como microservicio independiente  
**Entorno:** Evolución arquitectónica  
**Artefacto:** Módulo Electoral, Arquitectura general  
**Respuesta:** El sistema permite extraer módulo con refactorización controlada  
**Medida de respuesta:** Extracción posible en ≤ 4 semanas con límites claros ya establecidos; sin reescritura completa

**Prioridad:** Media  
**Requisitos relacionados:** Arquitectura inicial

---

## Resumen de Atributos de Calidad

| Categoría | Cantidad de Atributos |
|-----------|----------------------|
| Rendimiento | 4 |
| Escalabilidad | 4 |
| Elasticidad | 3 |
| Seguridad | 4 |
| Consistencia | 3 |
| Disponibilidad y Resiliencia | 3 |
| Auditabilidad | 3 |
| Observabilidad | 3 |
| Modificabilidad | 2 |
| **Total** | **29** |
