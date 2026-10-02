# Atributos de Calidad - Campus UNSCH

## Introducción

Los atributos de calidad representan características no funcionales que el sistema debe satisfacer para cumplir con sus objetivos arquitectónicos. Este documento define atributos de calidad mediante escenarios estructurados que incluyen fuente del estímulo, estímulo, entorno, artefacto afectado, respuesta del sistema y medida de respuesta.

**Nota importante sobre concurrencia:**  
La propuesta del proyecto Campus UNSCH establece como **escenario objetivo de dimensionamiento y prueba** el manejo de hasta **15,000 usuarios concurrentes** durante procesos masivos. Este valor representa un objetivo arquitectónico que debe validarse mediante pruebas de carga (k6), NO una medición actual. Los atributos de calidad definidos a continuación consideran este escenario objetivo como referencia para diseño y validación.

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
**Entorno:** Alta concurrencia durante pico electoral (escenario objetivo: 15,000 usuarios concurrentes)  
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
**Entorno:** Escenario objetivo de alta concurrencia (15,000 usuarios concurrentes)  
**Artefacto:** Backend (Node.js/NestJS), Balanceador de carga  
**Respuesta:** El sistema procesa solicitudes distribuidas entre instancias  
**Medida de respuesta:** Throughput ≥ 1,500 solicitudes/segundo con múltiples instancias del backend

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
**Entorno:** Escenario objetivo de alta concurrencia (15,000 usuarios)  
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

_(A completar en el siguiente commit)_
