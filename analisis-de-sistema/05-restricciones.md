# Restricciones - Campus UNSCH

## Introducción

Las restricciones representan limitaciones o condiciones obligatorias que el sistema debe respetar durante su diseño e implementación. A diferencia de los atributos de calidad (que son cualidades deseables medibles), las restricciones son condiciones impuestas que NO son negociables.

Este documento diferencia claramente:
- **Atributos de calidad:** Características medibles que el sistema debe lograr (ej: disponibilidad ≥ 99.5%)
- **Restricciones:** Condiciones fijas que el sistema DEBE cumplir (ej: debe ser web responsive)

---

## 1. Restricciones de Alcance

### RES-01: Plataforma web responsive

**Descripción:**  
El sistema DEBE ser íntegramente web y responsive, permitiendo su uso desde computadoras y dispositivos móviles mediante navegadores web modernos. NO se desarrollará aplicación móvil nativa para Android o iOS en la primera versión.

**Origen:** Propuesta del proyecto  
**Impacto:** Decisión tecnológica de frontend; necesidad de diseño responsive

---

### RES-02: NO reemplazo de sistemas académicos oficiales

**Descripción:**  
El sistema NO reemplazará los sistemas oficiales existentes de matrícula, notas, pagos, biblioteca u otros sistemas académicos institucionales. Campus UNSCH se enfoca exclusivamente en participación estudiantil y servicios complementarios.

**Origen:** Propuesta del proyecto (Fuera de Alcance)  
**Impacto:** Definición clara de límites funcionales; integración limitada con sistemas académicos

---

### RES-03: NO sistema de pagos ni billetera digital propia

**Descripción:**  
El sistema NO implementará una entidad financiera, billetera digital ni sistema propio de pagos. El sistema de incentivos se limita a puntos canjeables por recompensas no monetarias o beneficios institucionales autorizados.

**Origen:** Propuesta del proyecto (Fuera de Alcance)  
**Impacto:** Restricción del alcance del módulo de incentivos

---

### RES-04: NO sorteos ni recompensas reales sin autorización institucional

**Descripción:**  
El sistema NO otorgará recompensas monetarias reales ni ejecutará sorteos reales sin autorización institucional previa y revisión legal correspondiente.

**Origen:** Propuesta del proyecto (Fuera de Alcance), Consideraciones legales  
**Impacto:** Configuración del catálogo de recompensas; validación institucional requerida

---

### RES-05: NO elección oficial vinculante sin aprobación institucional

**Descripción:**  
El sistema NO podrá realizar una elección oficial vinculante sin aprobación institucional, normativa correspondiente y auditoría de autoridades competentes.

**Origen:** Propuesta del proyecto (Fuera de Alcance), Marco institucional  
**Impacto:** Clasificación de procesos electorales; validación institucional necesaria

---

## 2. Restricciones Institucionales

### RES-06: Validación mediante Padrón Institucional UNSCH

**Descripción:**  
El sistema DEBE validar la identidad y condición de estudiante habilitado mediante consulta al Padrón Institucional oficial de la UNSCH durante el registro. NO se permitirá el registro sin validación institucional.

**Origen:** Requerimientos del proyecto  
**Impacto:** Dependencia de sistema externo (ACT-06); necesidad de integración con padrón

---

### RES-07: Uso obligatorio de correo institucional UNSCH

**Descripción:**  
El sistema DEBE utilizar exclusivamente correos electrónicos del dominio institucional UNSCH para registro, activación de cuenta y notificaciones oficiales.

**Origen:** Requerimientos del proyecto  
**Impacto:** Validación de dominio en registro; dependencia del servicio de correo institucional (ACT-07)

---

### RES-08: Respeto de normas de convivencia institucional

**Descripción:**  
El contenido de la comunidad estudiantil DEBE respetar las normas de convivencia y reglamentos institucionales de la UNSCH. El sistema DEBE implementar moderación y mecanismos de reporte.

**Origen:** Marco normativo institucional  
**Impacto:** Necesidad de módulo de moderación; políticas de contenido

---

## 3. Restricciones Tecnológicas

### RES-09: Compatibilidad con navegadores web modernos

**Descripción:**  
El sistema DEBE ser compatible con las últimas dos versiones de Chrome, Firefox, Safari y Edge. NO se garantiza soporte para navegadores obsoletos o discontinuados.

**Origen:** Propuesta del proyecto  
**Impacto:** Selección de tecnologías frontend; pruebas de compatibilidad

---

### RES-10: Contenedorización con Docker

**Descripción:**  
Los componentes del sistema DEBEN empaquetarse en contenedores Docker para facilitar despliegue, portabilidad y consistencia entre entornos.

**Origen:** Propuesta del proyecto  
**Impacto:** Estrategia de despliegue; configuración de entornos

---

### RES-11: Orquestación con Kubernetes

**Descripción:**  
El sistema DEBE utilizar Kubernetes como orquestador de contenedores para gestionar escalado, health checks, reinicio automático de instancias fallidas y administración de réplicas.

**Origen:** Propuesta del proyecto  
**Impacto:** Estrategia de despliegue y escalado; configuración de infraestructura

---

### RES-12: Monitoreo con Prometheus y Grafana

**Descripción:**  
El sistema DEBE implementar monitoreo de métricas y observabilidad utilizando Prometheus para recolección de métricas y Grafana para visualización de dashboards.

**Origen:** Propuesta del proyecto  
**Impacto:** Arquitectura de observabilidad; exposición de métricas

---

### RES-13: Pruebas de carga con k6

**Descripción:**  
El sistema DEBE ser validado mediante pruebas de carga progresivas utilizando k6, simulando el escenario objetivo de hasta 2,500 usuarios concurrentes para validar rendimiento y escalabilidad.

**Origen:** Propuesta del proyecto  
**Impacto:** Estrategia de pruebas; validación de atributos de calidad

---

## 4. Decisiones Tecnológicas Propuestas

**NOTA IMPORTANTE:** Las siguientes tecnologías fueron presentadas como "sugeridas" en la propuesta del proyecto. Se clasifican aquí como decisiones tecnológicas adoptadas para el diseño inicial, NO como restricciones institucionales obligatorias. Estas decisiones pueden revisarse durante la implementación si el análisis técnico lo justifica.

### RES-14: Frontend con React / Next.js

**Descripción:**  
Se PROPONE utilizar React o Next.js como framework de frontend para construir la aplicación web responsive. Esta decisión responde a la necesidad de una UI moderna, component-based y con buen ecosistema.

**Origen:** Propuesta del proyecto (tecnología sugerida)  
**Tipo:** Decisión tecnológica propuesta  
**Impacto:** Stack de frontend; arquitectura de componentes

---

### RES-15: Backend con Node.js / NestJS

**Descripción:**  
Se PROPONE utilizar Node.js con el framework NestJS para implementar el backend del sistema. NestJS proporciona estructura modular, TypeScript, decoradores y soporte para arquitectura enterprise.

**Origen:** Propuesta del proyecto (tecnología sugerida)  
**Tipo:** Decisión tecnológica propuesta  
**Impacto:** Stack de backend; arquitectura modular

---

### RES-16: Base de datos PostgreSQL

**Descripción:**  
Se PROPONE utilizar PostgreSQL como base de datos relacional principal para almacenamiento transaccional. PostgreSQL ofrece ACID, transacciones robustas, integridad referencial y rendimiento adecuado.

**Origen:** Propuesta del proyecto (tecnología sugerida)  
**Tipo:** Decisión tecnológica propuesta  
**Impacto:** Modelo de datos; diseño de esquema

---

### RES-17: Caché con Redis

**Descripción:**  
Se PROPONE utilizar Redis como sistema de caché distribuido para sesiones, datos temporales, rate limiting y consultas frecuentes. Redis proporciona baja latencia y operaciones atómicas.

**Origen:** Propuesta del proyecto (tecnología sugerida)  
**Tipo:** Decisión tecnológica propuesta  
**Impacto:** Arquitectura de caché; gestión de sesiones

---

### RES-18: Cola de mensajes con RabbitMQ

**Descripción:**  
Se PROPONE utilizar RabbitMQ como sistema de mensajería para procesamiento asíncrono de notificaciones, certificados y tareas diferidas. RabbitMQ proporciona colas confiables y patrones de mensajería.

**Origen:** Propuesta del proyecto (tecnología sugerida)  
**Tipo:** Decisión tecnológica propuesta  
**Impacto:** Arquitectura de mensajería; procesamiento asíncrono

---

### RES-19: Comunicación en tiempo real con WebSockets

**Descripción:**  
Se PROPONE utilizar WebSockets para comunicación bidireccional en tiempo real, permitiendo notificaciones push y actualizaciones de estado sin polling.

**Origen:** Propuesta del proyecto (tecnología sugerida)  
**Tipo:** Decisión tecnológica propuesta  
**Impacto:** Arquitectura de notificaciones en tiempo real

---

### RES-20: Balanceo de carga con Nginx / Ingress

**Descripción:**  
Se PROPONE utilizar Nginx o Kubernetes Ingress como balanceador de carga para distribuir solicitudes HTTP entre instancias del backend.

**Origen:** Propuesta del proyecto (tecnología sugerida)  
**Tipo:** Decisión tecnológica propuesta  
**Impacto:** Arquitectura de distribución de carga

---

## 5. Restricciones de Seguridad

### RES-21: Almacenamiento seguro de contraseñas

**Descripción:**  
El sistema DEBE almacenar contraseñas utilizando algoritmos de hash seguros (bcrypt con cost factor ≥ 10). NO se permite almacenamiento de contraseñas en texto plano ni con algoritmos obsoletos (MD5, SHA1 sin salt).

**Origen:** Mejores prácticas de seguridad  
**Impacto:** Implementación de autenticación; selección de librerías

---

### RES-22: Comunicación HTTPS

**Descripción:**  
El sistema DEBE utilizar HTTPS para todas las comunicaciones entre cliente y servidor en entornos de producción. NO se permite tráfico HTTP sin cifrado para datos sensibles.

**Origen:** Mejores prácticas de seguridad  
**Impacto:** Configuración de infraestructura; certificados SSL/TLS

---

### RES-23: Protección de datos personales

**Descripción:**  
El sistema DEBE proteger datos personales de estudiantes según legislación aplicable y políticas institucionales. NO se permite compartir información personal sin consentimiento ni revelar el voto individual de estudiantes.

**Origen:** Marco legal y normativo  
**Impacto:** Arquitectura de privacidad; diseño de base de datos electoral

---

## 6. Restricciones Académicas

### RES-24: Proyecto académico sin garantía de producción inmediata

**Descripción:**  
Este proyecto constituye un trabajo académico del curso de Arquitectura de Software. Su implementación en producción real requiere validación institucional, pruebas exhaustivas, auditoría de seguridad y aprobación de autoridades competentes.

**Origen:** Contexto académico  
**Impacto:** Alcance del proyecto; expectativas de despliegue

---

## Resumen de Restricciones

| Categoría | Cantidad de Restricciones |
|-----------|---------------------------|
| Alcance | 5 |
| Institucionales | 3 |
| Tecnológicas (Obligatorias) | 5 |
| Decisiones Tecnológicas Propuestas | 7 |
| Seguridad | 3 |
| Académicas | 1 |
| **Total** | **24** |
