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

_(A completar en el siguiente commit)_
