# 8. FASE G: GOBERNANZA DE LA IMPLEMENTACIÓN
## Marco de Gobernanza Organizacional, Contratos de Arquitectura, Supervisión de Conformidad, Puntos de Control (Quality Gates), Gestión de Dispensas y Transición Operativa para Yanbal Perú (Unique S.A.)

> **Ubicación oficial del documento:** `08_Fase_G_Gobierno_de_Implementacion/08_Fase_G_Gobernanza_de_la_Implementacion.md`  
> **Diagrama asociado:** `DIAG-10` (*Marco de Gobernanza de la Implementación, Contratos y Control de Conformidad Arquitectónica*, disponible en código PlantUML editable en `08_Fase_G_Gobierno_de_Implementacion/diagramas/DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml`)  
> **Marco metodológico:** TOGAF Standard (The Open Group Architecture Framework) — Fase G: Gobernanza de la Implementación (*Implementation Governance*)  
> **Documentos de referencia y gobernanza:** Fase Preliminar, Fase A (Visión de Arquitectura), Fase B (Arquitectura de Negocio), Fase C (Arquitectura de Sistemas de Información), Fase D (Arquitectura Tecnológica), Fase E (Oportunidades y Soluciones), Fase F (Planificación de la Migración), Matriz Maestra de Requerimientos, Catálogo Maestro de Actores y Sistemas, Glosario Canónico y Convenciones de Nomenclatura, y Registro Maestro de Supuestos, Incertidumbres y Decisiones de Arquitectura.  
> **Principio de gobernanza:** Todos los documentos precedentes del repositorio constituyen la línea base formal de verdad arquitectónica. La Fase G no reimplementa el software ni redefine las arquitecturas de transición (AT-1 y AT-2) ni la arquitectura objetivo (TO-BE), sino que establece la estructura institucional, los acuerdos formales vinculantes (*Architecture Contracts*), los criterios objetivos de conformidad y los mecanismos de control mediante los cuales Yanbal Perú garantizará que la solución construida se apegue con fidelidad a las definiciones arquitectónicas aprobadas.

---

## 8.1. Objetivos, alcance y principios de gobernanza de implementación

### 8.1.1. Propósito y necesidad de la gobernanza arquitectónica en Yanbal Perú

La **Fase G de TOGAF ADM (*Implementation Governance*)** proporciona la supervisión arquitectónica requerida para cerrar la brecha entre la concepción de la arquitectura y su construcción física y despliegue operativo. En el contexto de Yanbal Perú (Unique S.A.), donde confluyen sistemas corporativos legados de misión crítica (`SAP R3` - `SIS-01`), múltiples empresas de transporte tercerizadas (`ACT-07`), plataformas satélites de distribución (`SPY` - `SIS-04`) y portales comerciales de fuerza de ventas (`CELLFORCE` - `SIS-09`), la ausencia de una gobernanza formal provocaría desviaciones críticas respecto a los principios de diseño, comprometería la seguridad transaccional del Centro de Distribución de Lurín y degeneraría en integraciones punto a punto frágiles y de difícil mantenimiento.

El propósito fundamental de la gobernanza de implementación en este proyecto consiste en:
1. **Asegurar la conformidad de la implementación:** Garantizar que los equipos de desarrollo interno, las fábricas de software subcontratadas y los socios logísticos implementen con fidelidad técnica los componentes, interfaces, flujos de eventos y estándares definidos en las Fases B, C, D y E.
2. **Salvaguardar las restricciones arquitectónicas innegociables:** Hacer cumplir con rigor vinculante la prohibición de escrituras directas sobre el ERP (`REQ-RES-002`, `PAE-04`), el aislamiento del Centro de Distribución de Lurín (`ACT-03`), el alcance B2B estricto (`REQ-RES-001`, `DEC-04`) y la ausencia de imposición de hardware GPS propietario sobre los transportistas (`REQ-RES-004`, `ASM-05`).
3. **Controlar la evolución hacia los estados objetivo:** Supervisar que la migración gradual transcurra ordenadamente a través de las dos Arquitecturas de Transición formalizadas en la Fase F (AT-1 en Semana S12 y AT-2 en Semana S18), condicionando el avance a la superación de criterios de aceptación verificables (*Quality Gates*).
4. **Gestionar la deuda técnica y las excepciones:** Formalizar un procedimiento de solicitud, evaluación, aprobación temporal y liquidación obligatoria de dispensas arquitectónicas (*Architecture Waivers*), impidiendo la proliferación de excepciones no documentadas.
5. **Alinear la entrega técnica con los beneficios de negocio:** Medir sistemáticamente el aporte de las capacidades implementadas frente a los objetivos estratégicos (`OBJ-01` a `OBJ-08`), factores críticos de éxito (`FCE-01` a `FCE-06`) y métricas de desempeño (`KPI-01` a `KPI-08`).

### 8.1.2. Alcance de la gobernanza de implementación

El marco de gobernanza cubre la totalidad del ciclo de vida de ejecución de los cinco Paquetes de Trabajo canónicos (`PKG-01` a `PKG-05`) a lo largo del horizonte referencial de 24 semanas (S1 a S24) definido en la Fase F, así como la etapa de estabilización operativa y transferencia (*Handover to Run*):

```
+----------------------------------------------------------------------------------------------------+
|                       ALCANCE OPERATIVO DE LA GOBERNANZA DE IMPLEMENTACIÓN                         |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [PAQUETES DE TRABAJO GOBERNADOS]                                                                  |
|  * PKG-01: Gobernanza de Datos, Modelado Canónico y Contratos de Integración.                      |
|  * PKG-02: Plataforma Core MTD y Microservicios de Eventos de Despacho.                           |
|  * PKG-03: Adaptadores de Integración de Sistemas Internos (SAP R3, SPY, CRM).                     |
|  * PKG-04: Onboarding y Pasarela Segura para Operadores Logísticos Tercerizados.                   |
|  * PKG-05: Monitoreo Operacional, Telemetría y Dashboards de Control de SLAs.                     |
|                                                                                                    |
|  [HITOS Y TRANSICIONES AUDITADAS]                                                                  |
|  * GATE 1 (Semana S12): Auditoría y certificación de la Arquitectura de Transición 1 (AT-1).       |
|  * GATE 2 (Semana S18): Auditoría y certificación de la Arquitectura de Transición 2 (AT-2).       |
|  * GATE 3 (Semana S24): Auditoría de conformidad final para pase a producción nacional (TO-BE).   |
|  * POST-IMPLEMENTATION REVIEW (Semanas +4 y +12): Evaluación post-despliegue y pase a Fase H.      |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

El alcance se circunscribe con precisión al flujo de distribución nacional B2B (Lurín hacia distribuidores departamentales), excluyendo de forma terminante cualquier intromisión en la logística de última milla B2C hacia consultoras independientes (`REQ-RES-001`, `DEC-04`).

### 8.1.3. Principios rectores de la gobernanza de implementación

Las actividades de gobernanza de la Fase G se rigen por seis principios fundamentales derivados de los principios corporativos (`PAE-01` a `PAE-08`) y las decisiones arquitectónicas clave (`DEC-01` a `DEC-07`):

| ID Principio | Nombre del Principio | Declaración y Fundamento Arquitectónico | Implicancia Operativa en la Implementación |
| :---: | :--- | :--- | :--- |
| **`PG-01`** | **Primacía del Modelo Arquitectónico (`PAE-01`)** | Las decisiones técnicas de implementación deben supeditarse a los modelos aprobados en las Fases B, C, D y E. Ningún equipo puede alterar contratos, patrones o esquemas sin autorización formal. | Los contratos de datos JSON Schema y las especificaciones OpenAPI 3.0 son vinculantes. El código fuente debe superar validaciones automáticas de adherencia a esquemas. |
| **`PG-02`** | **Inviolabilidad y Soberanía del ERP (`PAE-04`, `REQ-RES-002`)** | `SAP R3` (`SIS-01`) es el sistema de registro de inventarios y pedidos. Queda estrictamente vedado cualquier acceso de escritura directa a sus tablas base por parte de componentes externos al área de Lurín. | La integración se realiza exclusivamente mediante vistas de solo lectura y tablas intermedias de staging gestionadas por `ADP-SAP`. `ACT-03` ejerce el veto técnico absoluto. |
| **`PG-03`** | **Evidencia Objetiva en Puertas de Calidad (`PAE-02`, `PAE-08`)** | La transición entre arquitecturas (AT-1 $\rightarrow$ AT-2 $\rightarrow$ TO-BE) requiere la verificación empírica de métricas de calidad, seguridad y rendimiento, descartando aprobaciones basadas en presunciones. | Para certificar cada *Quality Gate* se exige la ejecución de pruebas automatizadas, reportes SAST/DAST y métricas de latencia auditadas en entornos de réplica idéntica. |
| **`PG-04`** | **Corresponsabilidad Formal con Socios Externos (`PAE-03`, `PAE-05`)** | La trazabilidad de ruta depende de la cooperación técnica de los transportistas (`ACT-07`). Dicha relación se rige por especificaciones abiertas y contratos canónicos, no por exigencias de infraestructura propietaria. | Se proporcionan especificaciones abiertas (CloudEvents v1.0, REST), portal web de contingencia y entornos de homologación (Sandbox). No se exige GPS a bordo (`REQ-RES-004`). |
| **`PG-05`** | **Transparencia y Cero Deuda Técnica No Documentada** | Toda desviación temporal de la arquitectura objetivo debe formalizarse mediante una Solicitud de Dispensa (*Architecture Waiver*) que cuantifique la deuda técnica y fije su fecha perentoria de liquidación. | No se toleran atajos técnicos provisionales sin registro en el libro formal de waivers. Ninguna dispensa puede sobrevivir al paso a producción de la Arquitectura Objetivo (TO-BE). |
| **`PG-06`** | **Continuidad Operacional Ininterrumpida (`PAE-07`)** | Los despliegues de nuevos componentes de integración, actualizaciones o tareas de migración no deben degradar ni interrumpir los procesos de picking, empaque y despacho físico del Centro de Distribución de Lurín. | Los despliegues productivos se realizan bajo esquemas *Blue/Green*, con desacoplamiento asíncrono en colas de eventos (RabbitMQ/Kafka) y planes de rollback de reversión inmediata. |

---

## 8.2. Modelo organizacional de gobernanza arquitectónica

### 8.2.1. Estructura y órganos de decisión

Para administrar la gobernanza de la implementación sin burocracia paralela, se establece un modelo organizacional jerárquico y colegiado, liderado por el **Consejo de Arquitectura (*Architecture Board*)**, apoyado por comités operativos de aseguramiento y enlaces de interoperabilidad:

```
+----------------------------------------------------------------------------------------------------+
|                     ESTRUCTURA ORGANIZACIONAL DE GOBERNANZA ARQUITECTÓNICA                         |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|                     +--------------------------------------------------------+                     |
|                     |                  ARCHITECTURE BOARD                    |                     |
|                     |        (Consejo de Arquitectura de Yanbal Perú)        |                     |
|                     |  * Sponsor Ejecutivo: Gerencia de TI (ACT-13)          |                     |
|                     |  * Dirección Técnica: Arquitecto Líder (ACT-12)        |                     |
|                     |  * Seguridad: Oficial de Seguridad CISO (ACT-11)       |                     |
|                     |  * Logística: Jefatura de Operaciones Lurín (ACT-06)   |                     |
|                     |  * Custodia ERP: Jefatura de Almacén Lurín (ACT-03)    |                     |
|                     +--------------------------------------------------------+                     |
|                                                  |                                                 |
|                   +------------------------------+------------------------------+                  |
|                   |                                                             |                  |
|   +--------------------------------+                           +--------------------------------+  |
|   |   OFICINA DE GESTIÓN Y QA      |                           |     COMITÉ DE INTEGRACIÓN      |  |
|   |         (PMO & QA)             |                           |        Y SOCIOS B2B            |  |
|   | * Gestor de Proyecto (ACT-15)  |                           | * Arquitecto de Solución       |  |
|   | * Líder de Pruebas QA (ACT-14) |                           | * Enlace Operativo (ACT-06)    |  |
|   | * Auditoría de Estándares      |                           | * Socios de Transporte (ACT-07)|  |
|   | * Seguimiento de Cronograma    |                           | * Líderes TI Socios (ACT-16)   |  |
|   +--------------------------------+                           +--------------------------------+  |
|                   |                                                             |                  |
|   +-----------------------------------------------------------------------------+---------------+  |
|   |                                                                                             |  |
|   |  [EQUIPOS EJECUTORES DE IMPLEMENTACIÓN]                                                     |  |
|   |  * Equipo de Datos e Integración (PKG-01)                                                   |  |
|   |  * Fábrica de Software / Equipo Core MTD (PKG-02)                                           |  |
|   |  * Equipo SAP e Interfaces Internas (PKG-03)                                                |  |
|   |  * Equipo de Enlace B2B y Portales Externos (PKG-04)                                        |  |
|   |  * Equipo de Infraestructura, SRE y Observabilidad (PKG-05)                                 |  |
|   +---------------------------------------------------------------------------------------------+  |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

#### Roles y Atribuciones de los Órganos de Gobierno:

1. **Architecture Board (Consejo de Arquitectura):**
   * **Composición:** Presidido por la Gerencia de TI (`ACT-13`), con la secretaría técnica del Arquitecto Líder de Solución (`ACT-12`), el Oficial de Seguridad de la Información (`ACT-11`), la Jefatura de Operaciones Logísticas Lurín (`ACT-06`) y la Jefatura de Almacén Lurín (`ACT-03`).
   * **Atribuciones:** Máxima instancia de arbitraje técnico; aprobación y firma de los Contratos de Arquitectura (`AC-PKG-01` a `AC-PKG-05`); evaluación y dictamen vinculante sobre los tres *Quality Gates* (AT-1, AT-2, TO-BE); resolución de conflictos de diseño; y aprobación o denegación de solicitudes de dispensa (*Architecture Waivers*).
   * **Frecuencia:** Sesiones ordinarias quincenales y sesiones extraordinarias por convocatoria urgente (solicitudes de waiver crítico o incidentes de gate).
2. **Oficina de Gestión de Proyectos y Aseguramiento de Calidad (PMO & QA):**
   * **Composición:** Conducido por el Gestor de Proyecto / Scrum Master (`ACT-15`) y el Líder de QA y Pruebas Arquitectónicas (`ACT-14`).
   * **Atribuciones:** Fiscalizar el cumplimiento de las fechas relativas (Semanas S1 a S24); auditar la ejecución de pruebas unitarias, de integración, de carga y de seguridad estática (SAST); custodiar los libros de actas de conformidad y administrar el inventario de hallazgos técnicos.
3. **Comité de Integración y Socios B2B:**
   * **Composición:** Conducido por el Arquitecto de Solución (`ACT-12`) y el Representante de Operaciones Logísticas (`ACT-06`), en coordinación directa con los responsables operativos de las Empresas de Transporte Tercerizado (`ACT-07`) y sus enlaces técnicos (`ACT-16`).
   * **Atribuciones:** Gestionar el onboarding técnico de los transportistas externos; certificar la interoperabilidad de las APIs mediante el ambiente de pruebas (Sandbox); homologar los esquemas de eventos; y consensuar los planes de contingencia y contingencias de conectividad.

### 8.2.2. Matriz RACI Integral de Gobernanza de Implementación

A continuación se establece la distribución formal de responsabilidades entre los 16 actores canónicos de la arquitectura (`ACT-01` a `ACT-16`), conforme a la convención internacional RACI:
* **R (Responsible):** Quien realiza la actividad ejecutiva de diseño, configuración, desarrollo o verificación.
* **A (Accountable):** Quien aprueba formalmente y asume la responsabilidad final sobre el resultado del artefacto.
* **C (Consulted):** Quien proporciona información clave, criterios operativos, técnicos o restricciones mandatorias.
* **I (Informed):** Quien es notificado formalmente sobre las decisiones, aprobaciones o desviaciones.

| Actividad / Proceso de Gobernanza de Fase G | ACT-01 (Dir. Log) | ACT-02 (Jef. Dist) | ACT-03 (Jef. Alm) | ACT-04 (Sup. Rut) | ACT-05 (Aux. Emb) | ACT-06 (Jef. Oper) | ACT-07 (Emp. Tran) | ACT-08 (Sup. Call) | ACT-09 (Dir. Vent) | ACT-10 (Aud. Int) | ACT-11 (Ofi. Seg) | ACT-12 (Arq. Lid) | ACT-13 (Ger. TI) | ACT-14 (Lid. QA) | ACT-15 (Scrum PM) | ACT-16 (Resp. TI Tran) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1. Definición y Emisión de Contratos de Arquitectura (`AC-01`..`05`)** | I | C | C | I | I | C | C | I | I | I | C | **R** | **A** | C | C | C |
| **2. Revisiones de Diseño Detallado (DDR) y Modelos Lógicos** | I | I | C | I | I | C | I | I | I | I | C | **R** | **A** | C | C | I |
| **3. Auditoría de Aislamiento ERP y Acceso a Base de Datos (`REQ-RES-002`)** | I | I | **A** | I | I | C | I | I | I | C | C | **R** | C | C | I | I |
| **4. Verificación de Seguridad, Cifrado TLS 1.3 y mTLS (`REQ-RNF-003`)** | I | I | I | I | I | I | C | I | I | C | **A** | **R** | I | C | I | C |
| **5. Homologación e Integración de Transportistas en Sandbox** | I | I | I | C | I | C | **R** | I | I | I | C | C | I | **R** | C | **R** |
| **6. Evaluación y Certificación de Puerta GATE 1 (AT-1, S12)** | I | C | **A** | I | I | C | I | I | I | C | C | **R** | **A** | **R** | C | I |
| **7. Evaluación y Certificación de Puerta GATE 2 (AT-2, S18)** | I | C | C | C | I | **A** | C | C | I | C | C | **R** | **A** | **R** | C | C |
| **8. Dictamen de Solicitudes de Dispensa (*Architecture Waivers*)** | I | I | C | I | I | C | I | I | I | C | C | **R** | **A** | C | C | I |
| **9. Autorización de Rollout Nacional y Go-Live GATE 3 (TO-BE, S24)** | **A** | C | C | C | I | **A** | C | C | C | C | C | **R** | **A** | **R** | C | C |
| **10. Ejecución del Post-Implementation Review (PIR, Sem +4/+12)** | C | C | C | C | I | C | C | C | C | C | C | **R** | **A** | C | C | I |
| **11. Traspaso Formal a Gestión del Cambio de Arquitectura (Fase H)** | I | I | C | I | I | C | I | I | I | I | I | **R** | **A** | I | I | I |

---

## 8.3. Contratos de Arquitectura (*Architecture Contracts*)

### 8.3.1. Concepto y valor normativo según TOGAF ADM

En el estándar TOGAF, un **Contrato de Arquitectura (*Architecture Contract*)** es un acuerdo conjunto formal y documentado entre los patrocinadores del negocio, los líderes de la arquitectura empresarial y los equipos ejecutores (fábricas de desarrollo internas, consultoras externas de integración y proveedores logísticos asociados).

El contrato de arquitectura cumple tres funciones vitales:
1. **Fijar compromisos ejecutables:** Define con precisión matemática los entregables técnicos, contratos de interfaces de datos (JSON Schemas), niveles de servicio de latencia ($\le 30\text{ min}$) y protocolos criptográficos que los ejecutores deben acatar.
2. **Establecer salvaguardas inviolables:** Documenta explícitamente las prohibiciones operativas (e.g., veto de escrituras en `SAP R3`, impedimento de desvíos B2C) para evitar que los programadores implementen atajos convenientes pero destructivos.
3. **Servir de base para la aceptación formal:** Cada paquete de trabajo (`PKG-01` a `PKG-05`) se liquida técnica y administrativamente únicamente cuando el Architecture Board certifica el cumplimiento del contrato respectivo.

### 8.3.2. Estructura estándar de los Contratos de Arquitectura

Cada contrato emitido por el Architecture Board de Yanbal Perú adopta una estructura canónica de nueve secciones:
* **Sección 1: Metadatos y Partes Firmantes:** Identificador del contrato, paquete asociado, responsables y patrocinador.
* **Sección 2: Objetivos de Arquitectura y Beneficios de Negocio:** Alineación con `OBJ-XX` y `KPI-XX`.
* **Sección 3: Componentes y Entregables del Paquete:** Mapeo a componentes canónicos (`CMP-XX-YY`, `ADP-XX`).
* **Sección 4: Requerimientos y Restricciones Vinculantes:** Trazabilidad a `REQ-XX` y salvaguardas arquitectónicas.
* **Sección 5: Especificaciones Técnicas y Estándares de Interoperabilidad:** Protocolos, formatos y normas.
* **Sección 6: Métricas de Calidad y Criterios de Aceptación:** Umbrales numéricos de aprobación.
* **Sección 7: Plan de Pruebas y Evidencias Requeridas:** Reportes de homologación y certificaciones.
* **Sección 8: Procedimiento ante Desviaciones y Penalidades:** Reglas para tramitar waivers o rechazos.
* **Sección 9: Firmas y Dictamen de Aprobación:** Visto bueno colegiado del Architecture Board.

---

### 8.3.3. Fichas de Contratos de Arquitectura para los Paquetes Canónicos (`PKG-01` a `PKG-05`)

#### Ficha 1: Contrato de Arquitectura `AC-PKG-01`
* **Identificador del Contrato:** `AC-PKG-01-DATOS-INTEGRACION`
* **Paquete de Trabajo Asociado:** `PKG-01` — Gobernanza de Datos, Modelado Canónico y Contratos de Integración.
* **Partes Interesadas y Ejecutores:**
  * *Emisor / Aprobador:* Architecture Board (Presidido por `ACT-13`; Dirección Técnica por `ACT-12`).
  * *Equipo Ejecutor:* Equipo de Arquitectura de Datos y Especialistas de Integración de Yanbal.
  * *Contraparte Operativa:* Jefatura de Almacén Lurín (`ACT-03`) y Enlaces Técnicos de Transportistas (`ACT-16`).
* **Objetivos y Requerimientos Asociados:** Asegurar la consistencia semántica en los intercambios de información de despacho B2B; implementar los requerimientos canónicos `REQ-ARQ-001`, `REQ-ARQ-002`, `REQ-ARQ-003`, `REQ-INT-003` y `REQ-INT-004`, cumpliendo las condiciones técnicas de transición (`CRT-TRA-01` a `CRT-TRA-05`) y los controles de seguridad perimetral (`CRT-SEG-01` derivado de `REQ-RNF-003`).
* **Componentes de Software Asignados:** Definición y versionado del catálogo canónico de esquemas JSON Schema; especificación CloudEvents v1.0; diccionario de datos unificado.
* **Restricciones Ineludibles:** Prohibición estricta de introducir identificadores de estado propietarios no homologados en el glosario canónico; obligatoriedad de marcas temporales duales ISO 8601 UTC-5 ($T_0$ de captura en campo y $T_1$ de ingesta en plataforma).
* **Criterios de Aceptación y Conformidad:**
  1. Especificación OpenAPI 3.0 y JSON Schema aprobada al 100% por el Architecture Board en Semana S4.
  2. Matriz de validación sintáctica que verifique el rechazo automático de payloads incompletos o maliciosos.
  3. Diccionario de datos canónico sincronizado sin ambigüedades entre los 10 sistemas (`SIS-01` a `SIS-10`).
* **Mecanismo de Verificación:** Auditoría de schemas en repositorio Git corporativo mediante linter automatizado y certificación de no regresión en entorno de Staging.

---

#### Ficha 2: Contrato de Arquitectura `AC-PKG-02`
* **Identificador del Contrato:** `AC-PKG-02-CORE-MTD-MICROSERVICIOS`
* **Paquete de Trabajo Asociado:** `PKG-02` — Plataforma Core MTD y Microservicios de Eventos de Despacho.
* **Partes Interesadas y Ejecutores:**
  * *Emisor / Aprobador:* Architecture Board (Validación técnica por `ACT-12` y CISO `ACT-11`).
  * *Equipo Ejecutor:* Fábrica de Software / Equipo de Desarrollo de Microservicios Core.
  * *Contraparte Operativa:* Líder de Aseguramiento de Calidad (`ACT-14`).
* **Objetivos y Requerimientos Asociados:** Desplegar el motor central de procesamiento reactivo de eventos; implementar los requerimientos canónicos `REQ-FUN-001`, `REQ-FUN-002`, `REQ-FUN-003`, `REQ-FUN-004`, `REQ-RNF-001`, `REQ-RNF-002`, `REQ-RNF-003`, `REQ-ARQ-001` y `REQ-ARQ-002`, satisfaciendo los criterios técnicos de seguridad y auditoría (`CRT-SEG-01`, `CRT-SEG-02` y `CRT-SEG-03` derivados de `REQ-RNF-003`).
* **Componentes de Software Asignados:** `CMP-ING-01` (Ingesta y Autenticación), `CMP-COR-02` (Procesamiento de Eventos y Estado), `CMP-VAL-03` (Validación Semántica y Causal), `CMP-AUD-04` (Auditoría Temporal y SLA), `CMP-PUB-05` (Publicación y Notificación) y `CMP-NOT-06` (Gestor de Incidentes y Desvíos).
* **Restricciones Ineludibles:**
  1. *Idempotencia Obligatoria:* Cada evento debe procesarse mediante clave única `UUIDv4`; los eventos duplicados deben descartarse sin mutar el estado transaccional.
  2. *Auditoría Temporal en `CMP-AUD-04`:* El microservicio debe estampar de forma inmutable la marca $T_1$ inmediatamente después de superar la validación sintáctica en `CMP-ING-01`, computando la latencia de tránsito $\Delta T = T_1 - T_0$.
  3. *Persistencia Aislada:* Los microservicios persisten exclusivamente en su propia base de datos operacional de eventos (`SIS-03`), sin ninguna conexión directa a `SAP R3`.
* **Criterios de Aceptación y Conformidad:**
  1. Cobertura de pruebas unitarias $\ge 85\%$ en todos los microservicios.
  2. Procesamiento de validación y estampado temporal en $\le 500\text{ ms}$ por evento a 100 req/s.
  3. Cero vulnerabilidades críticas o altas en reporte de análisis estático de seguridad (SAST).
* **Mecanismo de Verificación:** Batería de pruebas de integración y carga en entorno réplica supervisada por `ACT-14`.

---

#### Ficha 3: Contrato de Arquitectura `AC-PKG-03`
* **Identificador del Contrato:** `AC-PKG-03-ADAPTADORES-INTERNOS`
* **Paquete de Trabajo Asociado:** `PKG-03` — Adaptadores de Integración de Sistemas Internos (`ADP-SAP`, `ADP-SPY`, `ADP-CRM`).
* **Partes Interesadas y Ejecutores:**
  * *Emisor / Aprobador:* Architecture Board (Visto bueno obligatorio de `ACT-03` para ERP y `ACT-11` para seguridad).
  * *Equipo Ejecutor:* Equipo Especialista en Integraciones SAP / Middleware Corporativo.
  * *Contraparte Operativa:* Jefatura de Almacén Lurín (`ACT-03`) y Operaciones Logísticas (`ACT-06`).
* **Objetivos y Requerimientos Asociados:** Desacoplar los sistemas corporativos legados del flujo de eventos en tiempo real; garantizar la integridad de `SAP R3`; implementar los requerimientos canónicos `REQ-INT-001`, `REQ-INT-002`, `REQ-INT-004`, `REQ-RES-002` y `REQ-RES-003`, asegurando el criterio de aislamiento y staging (`CRT-SEG-02` derivado de `REQ-RNF-003`).
* **Componentes de Software Asignados:** Adaptador SAP de solo lectura/staging (`ADP-SAP`), Adaptador SPY (`ADP-SPY`) y Adaptador CRM/Comercial (`ADP-CRM`).
* **Restricciones Ineludibles:**
  1. *Aislamiento Total de Escritura en SAP (`REQ-RES-002`):* Queda terminantemente prohibido el uso de sentencias `INSERT`, `UPDATE` o `DELETE` directas sobre tablas base de `SAP R3`.
  2. *Lectura Segura y No Bloqueante:* La sincronización de pedidos y entregas preparadas se realiza mediante consultas de solo lectura contra tablas intermedias de staging (`ZTRK_DESPACHO_STG`) o vistas indexadas no transaccionales, programadas para evitar contención de bloqueos durante el picking en Lurín.
  3. *Tolerancia a Desconexión:* Si `SAP R3` entra en mantenimiento nocturno o pierde enlace, `ADP-SAP` debe retener los mensajes en su cola local sin provocar caídas de servicio.
* **Criterios de Aceptación y Conformidad:**
  1. Verificación en auditoría de base de datos de que el usuario de servicio de `ADP-SAP` solo posee privilegios `SELECT` sobre tablas autorizadas.
  2. Pruebas de resiliencia ante parada simulada del ERP, verificando cero pérdidas de transacciones mediante colas intermedias de mensajería.
  3. Latencia de sincronización interna desde Lurín hacia el MTD $\le 15\text{ minutos}$ (`DEC-05`).
* **Mecanismo de Verificación:** Auditoría de permisos de base de datos y certificación formal emitida por el Oficial de Seguridad (`ACT-11`) y el Jefe de Almacén (`ACT-03`).

---

#### Ficha 4: Contrato de Arquitectura `AC-PKG-04`
* **Identificador del Contrato:** `AC-PKG-04-ONBOARDING-PASARELA-B2B`
* **Paquete de Trabajo Asociado:** `PKG-04` — Onboarding y Pasarela Segura para Operadores Logísticos Tercerizados.
* **Partes Interesadas y Ejecutores:**
  * *Emisor / Aprobador:* Architecture Board (Dirección de Operaciones `ACT-06` y Arquitecto `ACT-12`).
  * *Equipo Ejecutor:* Equipo de Integraciones B2B y Empresas de Transporte Tercerizado (`ACT-07` / `ACT-16`).
  * *Contraparte Operativa:* Jefatura de Distribución y Transporte (`ACT-02`) y Supervisión de Rutas (`ACT-04`).
* **Objetivos y Requerimientos Asociados:** Estandarizar la captura de eventos en ruta desde transportistas; implementar los requerimientos canónicos `REQ-INT-003`, `REQ-INT-004`, `REQ-RES-004`, `REQ-RNF-001`, `REQ-RNF-004` y `REQ-ARQ-003`, satisfaciendo los criterios de homologación en Sandbox, tolerancia Store-and-Forward y portal de contingencia (`CRT-TRA-02`, `CRT-TRA-03`, `CRT-TRA-04` y `CRT-SEG-01` derivados de `REQ-RES-004` y `REQ-RNF-003`).
* **Componentes de Software Asignados:** Adaptador Logístico (`ADP-LOG`), API Gateway Perimetral B2B (`WAF/Reverse Proxy`), Portal Web de Contingencia para Carga Manual de Eventos.
* **Restricciones Ineludibles:**
  1. *Sin Hardware Propietario (`REQ-RES-004`, `ASM-05`):* No se exigirá la instalación de computadoras de a bordo ni dispositivos telemáticos propietarios a las empresas de transporte. La interoperabilidad se logra vía REST/JSON o carga web manual.
  2. *Autenticación Segura y Cifrado:* Toda comunicación externa exige canal cifrado TLS 1.3 y autenticación mediante tokens JWT (RS256) con llaves rotativas o mTLS para integraciones sistema a sistema.
  3. *Mecanismo Store-and-Forward Obligatorio:* Los socios logísticos que consuman la API o utilicen aplicativos móviles deben implementar retención local de eventos para retransmitir automáticamente al restablecerse la cobertura celular en rutas interprovinciales.
* **Criterios de Aceptación y Conformidad:**
  1. Homologación en ambiente Sandbox de los transportistas asociados (`ACT-07`), con una tasa de éxito de ingesta $\ge 99.5\%$.
  2. Disponibilidad del Portal Web de Contingencia con tiempo de respuesta $\le 2\text{ segundos}$ para despacho de incidencias.
  3. Verificación empírica de latencia de reporte en piloto: cumplimiento del umbral de tolerancia técnica operativa $\ge 95\%$ de eventos transmitidos dentro de la ventana objetivo de 30 minutos (orientado a la meta analítica de diseño $\ge 98\%$, `DEC-05`, `KPI-02`).
* **Mecanismo de Verificación:** Ensayos en rutas piloto reales (e.g., Lima - Trujillo y Lima - Arequipa) durante cuatro semanas continuas en AT-2.

---

#### Ficha 5: Contrato de Arquitectura `AC-PKG-05`
* **Identificador del Contrato:** `AC-PKG-05-MONITOREO-TELEMETRIA-SLA`
* **Paquete de Trabajo Asociado:** `PKG-05` — Monitoreo Operacional, Telemetría y Dashboards de Control de SLAs.
* **Partes Interesadas y Ejecutores:**
  * *Emisor / Aprobador:* Architecture Board (Jefatura de Operaciones `ACT-06` y Gerencia de TI `ACT-13`).
  * *Equipo Ejecutor:* Equipo de Infraestructura, SRE y Observabilidad Tecnológica.
  * *Contraparte Operativa:* Supervisión de Call Center (`ACT-08`) y Jefatura de Distribución (`ACT-02`).
* **Objetivos y Requerimientos Asociados:** Proporcionar visibilidad centralizada y proactiva del desempeño logístico y tecnológico; implementar los requerimientos canónicos `REQ-FUN-004`, `REQ-FUN-005`, `REQ-RNF-001`, `REQ-RNF-002` y `REQ-RNF-003` (incluyendo la especificación técnica de observabilidad y auditoría `CRT-SEG-03`).
* **Componentes de Software Asignados:** Módulo de Telemetría Distribuida (OpenTelemetry), Servidor de Métricas y Alertas (Prometheus/Grafana), Paneles Telemáticos de SLA en tiempo real y componentes de auditoría operativa de `CMP-AUD-04`.
* **Restricciones Ineludibles:**
  1. *Desacoplamiento Operacional:* El monitoreo debe instrumentarse sin sobrecargar los microservicios transaccionales (sobrecarga de procesamiento $\le 2\%$).
  2. *Regla de Alerta Temprana de SLA:* El sistema debe disparar alertas automáticas a `ACT-04` y `ACT-06` tan pronto como un despacho exceda la cota de 25 minutos sin actualización de estado en ruta, previniendo el quiebre del SLA objetivo de 30 minutos.
* **Criterios de Aceptación y Conformidad:**
  1. Tableros operacionales desplegados y operativos con refresco de datos en tiempo real ($\le 5\text{ segundos}$).
  2. Cálculo automatizado y auditoría histórica del indicador `KPI-01` (latencia media) y `KPI-02` (cumplimiento SLA $\le 30\text{ min}$).
  3. Pistas de auditoría inmutables preservadas durante al menos 12 meses conforme al requerimiento canónico de seguridad y auditoría corporativa (`REQ-RNF-003` / criterio `CRT-SEG-03`).
* **Mecanismo de Verificación:** Certificación de simulacro de alertas e informe de auditoría técnica emitido por el Líder de QA (`ACT-14`).

---

## 8.4. Supervisión y control de conformidad arquitectónica

### 8.4.1. Mecanismos y rituales de revisión técnica

Para certificar que los componentes construidos respeten las directrices de arquitectura sin esperar al final del ciclo de entrega, el marco de gobernanza instituye cuatro mecanismos de supervisión continua:

```
+----------------------------------------------------------------------------------------------------+
|                         RITUALES DE SUPERVISIÓN Y CONTROL DE CONFORMIDAD                           |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [NIVEL 1: DISEÑO DETALLADO]                                                                       |
|  * Revisiones de Diseño de Detalle (Detailed Design Reviews - DDR).                                |
|  * Aprobación de diagramas de secuencia técnica, esquemas OpenAPI 3.0 y modelos DDL de bases.     |
|                                                                                                    |
|  [NIVEL 2: CONSTRUCCIÓN Y CÓDIGO]                                                                  |
|  * Análisis Estático de Seguridad y Calidad (SAST/SCA en CI/CD pipeline).                          |
|  * Inspección de SonarQube: cobertura unitaria >= 85%, deuda técnica <= nivel A.                  |
|  * Verificación de inmutabilidad de esquemas y reglas de serialización JSON.                       |
|                                                                                                    |
|  [NIVEL 3: INFRAESTRUCTURA Y DESPLIEGUE]                                                           |
|  * Auditoría de Infraestructura como Código (IaC - Terraform/Helm charts).                         |
|  * Comprobación de reglas de firewall, TLS 1.3, mTLS y aislamiento perimetral de red.              |
|                                                                                                    |
|  [NIVEL 4: INTEROPERABILIDAD Y PRUEBAS]                                                            |
|  * Certificación en ambiente Sandbox con socios de transporte tercerizados.                        |
|  * Ensayos de inyección de fallos, caos testing y contingencias Store-and-Forward.                 |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

### 8.4.2. Verificación de adhesión a estándares tecnológicos y principios

La siguiente matriz establece las pruebas de conformidad técnica que deben superar los componentes frente a los estándares definidos en la Fase D y los principios rectores de la arquitectura:

| Estándar / Directriz | Especificación Técnica Adoptada | Mecanismo de Verificación de Conformidad | Responsable de Auditoría | Evidencia Documental |
| :--- | :--- | :--- | :---: | :--- |
| **API RESTful** | OpenAPI 3.0 / Nivel 2 de Richardson (HTTP verbos, códigos semánticos) | Linter automático en pipeline (Spectral) y pruebas DAST | `ACT-12` / `ACT-14` | Reporte de análisis de contrato API |
| **Formato de Eventos** | CloudEvents v1.0 (Estructura canónica JSON) | Validación sintáctica en microservicio `CMP-ING-01` | `ACT-12` | Schema Validator Log |
| **Marcas Temporales** | ISO 8601 UTC-5 dual ($T_0$ captura, $T_1$ ingesta corporativa) | Inspección de payloads en `CMP-AUD-04` | `ACT-14` | Trazas de eventos en `SIS-03` |
| **Cifrado en Tránsito** | TLS 1.3 obligatorio; suites criptográficas modernas | Escaneo perimetral con Qualys SSL Labs (Calificación A+) | `ACT-11` | Certificado de Seguridad Perimetral |
| **Identificación / Idempotencia** | Clave única `UUIDv4` por evento en cabecera HTTP | Batería de pruebas de repetición intencional de eventos | `ACT-14` | Log de descarte de duplicados |
| **Aislamiento ERP** | Solo lectura en SAP; staging desacoplado | Auditoría de privilegios de usuario y trazas de red SQL | `ACT-03` / `ACT-11` | Dictamen de Aislamiento de Base de Datos |
| **Monitoreo y Métricas** | OpenTelemetry v1.20 / Prometheus exporter | Verificación de traces distribuidos de extremo a extremo | `ACT-12` / `ACT-13` | Dashboard de Observabilidad |

---

### 8.4.3. Listas de Verificación de Cumplimiento (*Compliance Checklists*)

A continuación se presentan las listas de verificación obligatorias que deben ser completadas y suscritas por los responsables de QA y Arquitectura previo a cada presentación de Quality Gate:

#### Lista de Verificación 1: Seguridad Perimetral e Interoperabilidad B2B (`PKG-01`, `PKG-04`)
* [ ] ¿Todos los endpoints de la API perimetral B2B exigen autenticación criptográfica vía JWT (RS256) o mTLS?
* [ ] ¿Se encuentra activo el módulo de limitación de tasa (*Rate Limiting*) en el API Gateway para mitigar ataques DDoS?
* [ ] ¿El Web Application Firewall (WAF) bloquea payloads con inyecciones SQL, scripts maliciosos o JSON deformes?
* [ ] ¿El mecanismo *Store-and-Forward* retiene eventos en caso de corte de red sin duplicar transacciones al reconectar?
* [ ] ¿El Portal Web de Contingencia cuenta con doble factor de autenticación (2FA) para los operadores de transporte?

#### Lista de Verificación 2: Microservicios Core y Auditoría Temporal (`PKG-02`)
* [ ] ¿El microservicio `CMP-ING-01` valida sintáctica y semánticamente el payload antes de remitirlo a la cola de eventos?
* [ ] ¿El componente `CMP-AUD-04` estampa la marca temporal $T_1$ de forma determinística e inmutable en UTC-5?
* [ ] ¿Se calcula y persiste correctamente la latencia de tránsito $\Delta T = T_1 - T_0$ en la base de datos operacional?
* [ ] ¿El componente `CMP-VAL-03` rechaza transiciones de estado ilógicas (e.g., "Entregado" antes de "En Tránsito")?
* [ ] ¿La persistencia operacional de eventos (`SIS-03`) está completamente aislada de los sistemas transaccionales legacy?

#### Lista de Verificación 3: Aislamiento del ERP y Adaptadores Internos (`PKG-03`)
* [ ] ¿Se comprobó que el usuario técnico de `ADP-SAP` no posee permisos de inserción, actualización ni borrado en `SAP R3`?
* [ ] ¿Las consultas de extracción de pedidos preparados leen exclusivamente sobre tablas de staging o vistas autorizadas?
* [ ] ¿Se verificó que el proceso de sincronización interna no genera bloqueos transaccionales en el Centro de Lurín?
* [ ] ¿El adaptador `ADP-SPY` procesa las confirmaciones de empaque sin dependencias de red síncronas bloqueantes?

---

## 8.5. Evaluación de conformidad de las implementaciones: Puntos de Control (*Quality Gates*)

### 8.5.1. Marco de evaluación escalonada de calidad por transiciones

La progresión hacia la arquitectura objetivo no es continua ni automática; se estructura mediante tres **Puntos de Control de Conformidad (*Quality Gates*)** vinculantes, ubicados en los hitos de cierre de las Arquitecturas de Transición definidas en la Fase F:

```
+----------------------------------------------------------------------------------------------------+
|                         PUNTOS DE CONTROL DE CONFORMIDAD (QUALITY GATES)                           |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [SEMANA S12]                      [SEMANA S18]                      [SEMANA S24]                  |
|  +---------------+                 +---------------+                 +---------------+             |
|  |    GATE 1     |                 |    GATE 2     |                 |    GATE 3     |             |
|  |  (Cierre AT-1)|                 |  (Cierre AT-2)|                 | (Cierre TO-BE)|             |
|  +-------+-------+                 +-------+-------+                 +-------+-------+             |
|          |                                 |                                 |                     |
|          v                                 v                                 v                     |
|  * Base tecnológica y core.        * Pasarela y pilotos B2B.         * Cobertura nacional 100%.    |
|  * Aislamiento SAP certificado.    * Consistencia secuencia (100%).  * Telemetría SLA activa.      |
|  * Sello temporal T1 operativo.    * Latencia piloto (>= 95%).       * Cumplimiento REQ-01..24.    |
|  * Aprobación para habilitar B2B.  * Aprobación para Rollout Nac.    * Handover a Operaciones.     |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

### 8.5.2. GATE 1: Certificación de la Arquitectura de Transición 1 (AT-1 — Semana S12)

El **Gate 1** valida la solidez de la plataforma interna, el despliegue del bus corporativo y el blindaje absoluto del ERP antes de abrir las comunicaciones hacia el exterior.

* **Criterios de Entrada:**
  1. Conclusión y entrega técnica de los paquetes `PKG-01`, `PKG-02` y `PKG-03`.
  2. Disponibilidad del entorno de Preproducción / Staging configurado con réplica de datos de prueba.
  3. Informes preliminares de pruebas unitarias y de integración interna completados.
* **Aspectos Auditados y Criterios de Aceptación:**
  1. *Aislamiento Estricto de `SAP R3` (`REQ-RES-002`):* Auditoría de seguridad de base de datos que certifique cero sentencias de modificación de datos y cero bloqueos en tablas maestras. Dictamen suscrito por `ACT-03` y `ACT-11`.
  2. *Auditoría Temporal y Sellado $T_1$ en `CMP-AUD-04`:* Verificación de que el microservicio de auditoría estampa $T_1$ con precisión de milisegundos y calcula el diferencial respecto al evento de empaque interno.
  3. *Validación Sintáctica de Esquemas:* Verificación de que el API Gateway interno y `CMP-ING-01` rechazan el 100% de payloads no conformes con los esquemas JSON Schema aprobados.
  4. *Rendimiento de Integración Interna:* Sincronización de eventos de salida de almacén Lurín hacia el MTD con latencia $\le 15\text{ minutos}$ (`DEC-05`).
* **Criterios de Salida y Dictamen:**
  * Emisión del **Acta de Certificación de Arquitectura de Transición 1 (AT-1)** suscrita por el Architecture Board.
  * Habilitación formal de la infraestructura de red perimetral para el inicio del onboarding del paquete `PKG-04`.

---

### 8.5.3. GATE 2: Certificación de la Arquitectura de Transición 2 (AT-2 — Semana S18)

El **Gate 2** evalúa los resultados de la integración con los primeros transportistas tercerizados en rutas piloto reales, certificando la estabilidad de la captura de eventos en ruta y los protocolos de contingencia.

* **Criterios de Entrada:**
  1. Certificación previa y formal del Gate 1.
  2. Despliegue completado del paquete `PKG-04` y del portal web de contingencia.
  3. Ensayos piloto completados durante al menos cuatro semanas en rutas representativas (Lima - Trujillo y Lima - Arequipa) con socios logísticos participantes (`ACT-07`).
* **Aspectos Auditados y Criterios de Aceptación:**
  1. *Consistencia de Secuencia de Estados (`KPI-03`):* $100\%$ de los despachos procesados en el piloto deben reflejar un orden causal riguroso y coherente (e.g., no reportar "En Distribución" sin "En Tránsito Interprovincial").
  2. *Cumplimiento de Latencia en Ruta y SLA:* Verificación del umbral de tolerancia técnica operativa $\ge 95\%$ de eventos de ruta reportados e integrados dentro de la ventana objetivo de 30 minutos (`DEC-05`), avanzando hacia la meta analítica de diseño de $\ge 98\%$ establecida en `KPI-02` (`ASM-16`).
  3. *Operatividad del Mecanismo Store-and-Forward:* Ensayos documentados de corte intencional de red en tramos de carretera sin cobertura celular, verificando que los eventos retenidos localmente se transmiten sin duplicidad ni alteración de marcas temporales al recuperar conectividad.
  4. *Sincronización hacia `CELLFORCE` (`SIS-09`):* Visibilidad de estados actualizada en el portal de fuerza de ventas para pedidos de la zona piloto.
* **Criterios de Salida y Dictamen:**
  * Emisión del **Acta de Certificación de Arquitectura de Transición 2 (AT-2)** suscrita por el Architecture Board.
  * Autorización para iniciar el despliegue nacional masivo (*Rollout*) hacia los 24 departamentos del Perú.

> **Resolución de la Inconsistencia Auditada sobre Umbrales de Aceptación:**  
> Se formaliza explícitamente en el marco de gobernanza la distinción rigurosa entre métricas:
> * **$100\%$:** Es el criterio absoluto e innegociable de consistencia y validez de la secuencia de estados de despacho (`KPI-03`). Ningún evento desordenado puede ser admitido.
> * **$\ge 98\%$:** Constituye la **meta analítica de diseño arquitectónico** comprometida en `DEC-05` y `KPI-02` para el régimen operacional pleno TO-BE.
> * **$\ge 95\%$:** Se establece formalmente como el **umbral de tolerancia técnica de prueba operativa** aceptable durante la fase piloto en AT-2 (`ASM-16`), reconociendo las fricciones iniciales de adopción técnica por parte de los conductores de los transportistas asociados.

---

### 8.5.4. GATE 3: Certificación de la Arquitectura Objetivo (TO-BE — Semana S24)

El **Gate 3** constituye el hito final de aceptación arquitectónica y autorización para el pase a régimen operacional y productivo continuo.

* **Criterios de Entrada:**
  1. Certificación previa y formal del Gate 2.
  2. Conclusión del despliegue del paquete `PKG-05` (Monitoreo, telemetría y dashboards de SLA).
  3. Rollout nacional completado hacia la totalidad de empresas de transporte tercerizadas y rutas de los 24 departamentos.
  4. Cero solicitudes de dispensa (*Waivers*) activas o pendientes de remediación.
* **Aspectos Auditados y Criterios de Aceptación:**
  1. *Cobertura B2B Total (`KPI-04`):* Monitoreo activo del $100\%$ de los pedidos B2B despachados desde Lurín hacia destinos nacionales.
  2. *Telemetría y Alertas de SLA en Tiempo Real:* Dashboards operativos funcionando con latencia de refresco $\le 5\text{ segundos}$ y disparo automatizado de alertas ante desviaciones de 25 minutos en ruta.
  3. *Verificación de Rendimiento y Disponibilidad (`KPI-06`):* Disponibilidad del servicio MTD $\ge 99.5\%$ durante el periodo de pruebas de estrés y precorte.
  4. *Cumplimiento Integral de Requerimientos:* Matriz de trazabilidad que demuestre la cobertura satisfactoria y verificada de los 24 Requerimientos Canónicos (`REQ-01` a `REQ-24`).
* **Criterios de Salida y Dictamen:**
  * Emisión y suscripción del **Acta de Aceptación de Arquitectura Empresarial TO-BE**.
  * Firma formal de Transferencia Operacional (*Handover to Run*) hacia Operaciones de TI (`ACT-13`) y Logística (`ACT-06`), dando inicio a la etapa de soporte *Hypercare*.

---

### 8.5.5. Matriz Consolidada de Puntos de Control y Reglas de Decisión

| Quality Gate | Hito / Momento | Paquetes Auditados | Condición Crítica de Aprobación | Responsable de Dictamen | Regla ante Incumplimiento |
| :---: | :---: | :---: | :--- | :---: | :--- |
| **GATE 1** | Cierre AT-1 (Semana S12) | `PKG-01`, `PKG-02`, `PKG-03` | Cero escrituras en SAP; estampación $T_1$ en `CMP-AUD-04` operativa; latencia interna $\le 15\text{ min}$. | Architecture Board (`ACT-13`, `ACT-12`, `ACT-03`) | Bloqueo de interfaces externas; prohibido iniciar onboarding de transportistas. |
| **GATE 2** | Cierre AT-2 (Semana S18) | `PKG-04`, Piloto B2B | Secuencia causal 100%; SLA piloto $\ge 95\%$ en ventana $\le 30\text{ min}$; *Store-and-Forward* validado. | Architecture Board (`ACT-13`, `ACT-12`, `ACT-06`) | Extensión de fase piloto por 2 semanas; veto al despliegue en los 24 departamentos. |
| **GATE 3** | Cierre TO-BE (Semana S24) | `PKG-05`, Rollout Nacional | Cobertura 100% rutas B2B; observabilidad de SLA activa; 24 requerimientos cubiertos; cero waivers. | Architecture Board y Sponsor General | Retención del pase a producción; activación de plan de estabilización extendido. |

---

## 8.6. Gestión de desviaciones, excepciones y solicitudes de dispensa (*Architecture Waivers*)

### 8.6.1. Marco conceptual y clasificación de desviaciones arquitectónicas

En proyectos de alta complejidad e integración interempresarial, es previsible que surjan restricciones técnicas imprevistas, retrasos en la adecuación de software de terceros o incompatibilidades tecnológicas transitorias. El estándar TOGAF establece el mecanismo de **Dispensas de Arquitectura (*Architecture Waivers*)** para gestionar estas excepciones con disciplina, evitando atajos informales.

Las desviaciones se clasifican formalmente en tres niveles de criticidad:

```
+----------------------------------------------------------------------------------------------------+
|                         CLASIFICACIÓN DE DESVIACIONES Y DISPENSAS                                  |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [NIVEL 1: DESVIACIÓN MENOR]                                                                       |
|  * Alcance: Ajustes cosméticos internos, bibliotecas secundarias, optimizaciones locales.          |
|  * Impacto: Cero impacto en interfaces públicas, seguridad ni contratos JSON.                      |
|  * Resolución: Dictaminada por el Arquitecto Líder (ACT-12) y registrada en bitácora de QA.        |
|                                                                                                    |
|  [NIVEL 2: DESVIACIÓN MAYOR (DISPENSA / WAIVER FORMAL)]                                            |
|  * Alcance: Incumplimiento temporal de estándares, demoras en onboarding de transportistas,        |
|    uso provisional del portal web de contingencia en lugar de API REST.                             |
|  * Impacto: No compromete la seguridad perimetral ni el aislamiento de SAP R3.                     |
|  * Resolución: Requiere evaluación formal del Architecture Board. Aprobación máxima de 6 semanas.  |
|                                                                                                    |
|  [NIVEL 3: DESVIACIÓN CRÍTICA / INADMISIBLE (NO DISPENSABLE)]                                      |
|  * Alcance: Intentos de escritura directa en SAP R3, desactivación de cifrado TLS 1.3,             |
|    alteración de marcas temporales T1, o desvíos del alcance hacia distribución B2C.               |
|  * Impacto: Riesgo severo para la continuidad del negocio, la seguridad o la integridad de datos.  |
|  * Resolución: RECHAZO AUTOMÁTICO E INNEGOCIABLE. Veto vinculante de ACT-03, ACT-11 o ACT-12.     |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

### 8.6.2. Procedimiento formal de solicitud y ciclo de vida de un Waiver

El ciclo de tramitación de una dispensa arquitectónica se ejecuta rigurosamente a través de seis etapas:

1. **Detección y Registro:** El equipo ejecutor identifica una incompatibilidad insalvable dentro de los plazos o especificaciones del contrato y completa el formulario oficial de Solicitud de Dispensa (*Architecture Waiver Request*).
2. **Análisis de Impacto Técnico y Financiero:** La Secretaría Técnica de Arquitectura (`ACT-12`) evalúa el impacto en la arquitectura de destino, cuantifica la deuda técnica generada y revisa el plan de mitigación propuesto.
3. **Revisión de Seguridad y ERP:** Si la solicitud roza aspectos de infraestructura o inventarios, se somete a revisión del CISO (`ACT-11`) y del Jefe de Almacén (`ACT-03`). Cualquier riesgo sobre la integridad de `SAP R3` conlleva el rechazo inmediato.
4. **Deliberación y Dictamen del Architecture Board:** El consejo sesiona, analiza el caso y emite una resolución formal:
   * **Aprobada con Condiciones:** Se autoriza de forma transitoria, fijando un plan de remediación financiado y una fecha límite de caducidad perentoria ($\le 6\text{ semanas}$ y siempre antes del siguiente Quality Gate).
   * **Rechazada:** La solución propuesta no es aceptable; el ejecutor debe implementar la solución conforme a contrato asumiendo los ajustes requeridos.
5. **Seguimiento de Remediación:** La PMO (`ACT-15`) y QA (`ACT-14`) monitorean semanalmente el avance del plan de remediación de la deuda técnica.
6. **Certificación de Cierre:** El Architecture Board verifica la liquidación técnica de la excepción y procede a la clausura formal del waiver en el registro corporativo.

---

### 8.6.3. Formato canónico de Solicitud de Dispensa Arquitectónica

```markdown
====================================================================================================
FORMATO OFICIAL: SOLICITUD DE DISPENSA DE ARQUITECTURA (ARCHITECTURE WAIVER)
====================================================================================================
1. DATOS GENERALES
   * Código de Dispensa: WVR-2026-XXXX
   * Fecha de Solicitud: [Semana Relativa de Proyecto / Fecha]
   * Paquete de Trabajo / Contrato: [PKG-01..05 / AC-PKG-XX]
   * Equipo Solicitante / Empresa: [Equipo Interno / Empresa de Transporte ACT-07]
   * Responsable Técnico Solicitante: [Nombre y Cargo]

2. DESCRIPCIÓN DE LA DESVIACIÓN
   * Componente o Interfaz Afectada: [e.g., ADP-LOG / Endpoint API Gateway]
   * Estándar o Principio Infringido: [e.g., Imposibilidad de autenticación mTLS por limitación legacy]
   * Causa Raíz de la Desviación: [Explicación técnica fundamentada]

3. EVALUACIÓN DE IMPACTO Y RIESGO
   * Clasificación de Criticidad: [ ] Menor (Nivel 1)   [X] Mayor (Nivel 2)   [ ] Crítica (Nivel 3)
   * Impacto en Disponibilidad / Desempeño: [Estimación cuantitativa]
   * Impacto en Seguridad y Privacidad: [Evaluación de riesgo por CISO ACT-11]
   * Impacto en Aislamiento ERP (SAP R3): [ ] Nulo   [ ] Riesgo Existente (Causal de Veto Inmediato)

4. PLAN DE CONTINGENCIA Y MITIGACIÓN TEMPORAL
   * Solución Temporal Propuesta: [e.g., Autenticación vía JWT por HTTPS con IP Whitelisting temporal]
   * Medidas de Monitoreo Reforzado: [Trazabilidad y alertas específicas]

5. PLAN DE REMEDIACIÓN Y LIQUIDACIÓN DE DEUDA TÉCNICA
   * Acciones Técnicas Definitivas: [Detalle de adecuaciones para volver al estándar]
   * Fecha Límite Perentoria de Cierre: [No mayor a 6 semanas / Previo a Quality Gate subsiguiente]
   * Recursos y Presupuesto Asignado: [Compromiso de ejecución]

6. DICTAMEN DEL CONSEJO DE ARQUITECTURA (ARCHITECTURE BOARD)
   * Resolución: [ ] APROBADA CONDICIONADA    [ ] RECHAZADA    [ ] DEVUELTA PARA SUBSANACIÓN
   * Justificación del Dictamen: [Fundamento de la resolución]
   * Firmas Obligatorias:
     - Gerente de TI (ACT-13) - Sponsor Ejecutivo: ___________________________
     - Arquitecto Líder (ACT-12) - Dirección Técnica: _______________________
     - Oficial de Seguridad (ACT-11) - Visto Bueno: _________________________
     - Jefe de Almacén Lurín (ACT-03) - Si aplica ERP: ______________________
====================================================================================================
```

---

## 8.7. Gobernanza de despliegue y transición operativa

### 8.7.1. Política de liberaciones y control de cambios en despliegue (*Release Management*)

Para garantizar que el despliegue del MTD no perturbe las operaciones comerciales y de distribución de Unique S.A. (`PAE-07`), la gobernanza de implementación define una política de liberaciones estricta sustentada en la segregación de entornos y estrategias modernas de pase a producción:

```
+----------------------------------------------------------------------------------------------------+
|                         POLÍTICA DE ENTORNOS Y CONTROL DE LIBERACIONES                             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ENTORNO DE DESARROLLO (DEV)]                                                                     |
|  * Despliegue continuo (CI) con mock services y generadores sintéticos de eventos.                |
|  * Pruebas unitarias automatizadas y análisis de calidad de código con SonarQube.                  |
|                                                                                                    |
|  [ENTORNO DE HOMOLOGACIÓN / SANDBOX (STAGING)]                                                     |
|  * Réplica idéntica de la topología productiva (Kubernetes, colas de eventos, WAF).                |
|  * Ambiente abierto y seguro para pruebas de integración con socios de transporte (ACT-07).        |
|  * Pruebas de estrés, simulación de cortes de red y validación de Puntos de Control (Gates).       |
|                                                                                                    |
|  [ENTORNO DE PRODUCCIÓN (PROD)]                                                                    |
|  * Despliegue bajo esquema Blue/Green para los microservicios del Core MTD (PKG-02).               |
|  * Despliegue gradual tipo Canary para la activación de transportistas en la pasarela (PKG-04).    |
|  * Monitoreo telemático continuo y control formal de cambios (RFC) supervisado por PMO y TI.      |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

#### Reglas de Despliegue:
1. **Despliegues sin Caída de Servicio (*Zero-Downtime Deployment*):** Todos los microservicios de la plataforma MTD se despliegan utilizando la estrategia *Blue/Green*. La versión nueva se prueba en producción sin tráfico real; una vez certificada, el enrutador del API Gateway conmuta el tráfico instantáneamente.
2. **Onboarding Canario para Transportistas:** La incorporación de operadores de transporte en AT-2 y TO-BE se efectúa de manera gradual (primero rutas piloto, luego regiones norte y sur, finalmente oriente y cobertura nacional completa), asegurando que cualquier anomalía de integración afecte únicamente a una fracción mínima del tráfico.

### 8.7.2. Continuidad operacional, procedimientos de Rollback y tolerancia a fallos

Ante cualquier contingencia grave durante un despliegue, la prioridad institucional absoluta es preservar la operatividad física del Centro de Distribución de Lurín.

#### Disparadores Automáticos de Aborto y Rollback (*Rollback Triggers*):
El Líder de QA (`ACT-14`) y el Arquitecto Líder (`ACT-12`) tienen la orden mandatoria de abortar inmediatamente el pase a producción y ejecutar el *Rollback* técnico si durante los primeros 30 minutos posteriores al despliegue se detecta cualquiera de las siguientes condiciones:
* Cualquier error transaccional o degradación del tiempo de respuesta en `SAP R3` atribuible al conector `ADP-SAP`.
* Tasa de error HTTP $5xx$ en el API Gateway perimetral superior al $1\%$.
* Pérdida de mensajes o acumulación descontrolada en las colas de eventos sin procesamiento por más de 5 minutos.
* Fallas en la autenticación que bloqueen la recepción de eventos desde más de un operador logístico.

#### Protocolo de Rollback Arquitectónico:
1. **Conmutación Inmediata de Tráfico:** El API Gateway revierte el enrutador a la versión previa estable (*Blue Environment*) en un lapso inferior a 60 segundos.
2. **Preservación de Trazabilidad:** Los eventos que hayan ingresado durante la ventana de incidente se retienen de forma segura en las colas de mensajería (Dead Letter Queue - DLQ) para su reprocesamiento ordenado posterior, garantizando cero pérdida de información.
3. **Desacoplamiento Operativo:** Si el fallo radicara en el adaptador `ADP-SAP`, este se apaga de forma preventiva; el Centro de Distribución de Lurín continúa operando normalmente en sus pantallas locales de `SAP R3`, acumulando los registros de despacho en la tabla intermedia de staging hasta la estabilización del conector.

### 8.7.3. Criterios de corte (*Cutover*) y transferencia formal a operaciones (*Handover to Run*)

La transición desde el proyecto de implementación hacia la operación cotidiana se rige por un protocolo formal de corte y estabilización:

1. **Plan de Corte (*Cutover Plan*):** Secuencia milimétricamente temporalizada de actividades ejecutadas durante una ventana técnica de bajo volumen operativo (fin de semana o noche), coordinando el congelamiento de versiones de prueba, la migración de configuraciones maestras y la activación de los certificados productivos.
2. **Periodo de Estabilización Asistida (*Hypercare*):** Durante las primeras dos semanas posteriores al *Go-Live* de cada transición, se conforma un equipo de soporte extendido de guardia 24/7 integrado por desarrolladores del Core, especialistas de integración y enlaces de transporte para resolver incidencias de forma prioritaria con un $MTTR \le 30\text{ minutos}$.
3. **Acta de Entrega y Transferencia Operacional (*Handover to Run*):** La transferencia definitiva de la solución hacia la Gerencia de TI (`ACT-13`) y la Jefatura de Operaciones Logísticas (`ACT-06`) se formaliza únicamente tras cumplir los siguientes requisitos:
   * Entrega de la documentación técnica completa: arquitectura como fue construida (*As-Built*), manuales de operación, procedimientos de contingencia y runbooks de administración.
   * Catálogo de métricas y alertas de monitoreo completamente configurado en Grafana/Prometheus.
   * Capacitación certificada al personal de monitoreo de TI, mesa de ayuda y supervisores de transporte (`ACT-04`).
   * Cierre formal de todas las no conformidades y entrega de la fianza técnica de garantía del software.

---

## 8.8. Revisión post-implementación (*Post-Implementation Review*), evaluación de resultados y verificación de beneficios

### 8.8.1. Marco de evaluación Post-Implementation Review (PIR)

Conforme a las mejores prácticas de TOGAF ADM, la gobernanza de la implementación culmina con la ejecución del **Examen Posterior a la Implementación (*Post-Implementation Review - PIR*)**. Este proceso colegiado se ejecuta en dos momentos clave del ciclo de vida operativo:
* **PIR Etapa 1 (Semana +4 tras el Go-Live TO-BE):** Evaluación técnica temprana de estabilidad, rendimiento de infraestructura, integridad de datos y resolución de fricciones iniciales de interoperabilidad.
* **PIR Etapa 2 (Semana +12 tras el Go-Live TO-BE):** Evaluación de madurez y verificación integral del valor de negocio, midiendo el impacto tangible en la reducción de tiempos de atención, disminución de quejas en el Call Center y cumplimiento efectivo de los SLAs de despacho B2B.

El PIR es conducido por el Architecture Board con la participación de la Dirección de Logística (`ACT-01`), Dirección de Ventas (`ACT-09`) y Auditoría Interna (`ACT-10`).

---

### 8.8.2. Matriz de verificación de beneficios y alineación con objetivos estratégicos y KPIs

A continuación se presenta el marco formal de evaluación de los ocho indicadores clave de rendimiento (`KPI-01` a `KPI-08`), contrastando la línea base observada con los resultados comprometidos y los mecanismos de medición arquitectónica:

| Código KPI | Nombre del KPI y Métrica Oficial | Línea Base AS-IS (Evidencia Documental) | Meta de Diseño / Compromiso TO-BE | Mecanismo de Verificación en Fase G | Alineación con Objetivos y FCE |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **`KPI-01`** | **Latencia Media de Actualización de Estados** | Desfase de hasta ~2 horas en tramos nacionales (`REQ-NEG-002`, entrevista) | Latencia media $\le 30\text{ minutos}$ en ruta B2B (`DEC-05`) | Muestreo automatizado en `CMP-AUD-04`: $\Delta T = T_1 - T_0$ sobre el 100% de eventos | `OBJ-01`, `OBJ-02` / `FCE-01` |
| **`KPI-02`** | **Cumplimiento del SLA de Reporte de Eventos** | Reportes batch heterogéneos y dispersos sin SLA regulado | $\ge 98\%$ de eventos integrados en $\le 30\text{ min}$ (Piloto AT-2 $\ge 95\%$) | Telemetría en tiempo real en Prometheus; reportes automáticos de desvío | `OBJ-02` / `FCE-02` |
| **`KPI-03`** | **Integridad y Consistencia Causal de Estados** | Inconsistencias periódicas por falta de ordenamiento causal | $100\%$ de eventos procesados en estricto orden de ciclo de vida | Validación semántica en `CMP-VAL-03`; log de eventos rechazados por causales | `OBJ-03` / `FCE-03` |
| **`KPI-04`** | **Cobertura de Despachos Monitoreados** | Visibilidad limitada a Lurín y cabeceras principales | $100\%$ de despachos B2B cubiertos hacia los 24 departamentos | Conciliación diaria entre despachos emitidos por SAP y eventos en MTD | `OBJ-04` / `FCE-04` |
| **`KPI-05`** | **Reducción de Incidentes por Falta de Información** | Alto volumen de llamadas y reclamos en Call Center (`SIS-08`) | Reducción $\ge 40\%$ de tickets de consulta sobre estado de pedido | Métricas de CRM (`SIS-07`) y reportes de atención de `ACT-08` | `OBJ-05` / `FCE-05` |
| **`KPI-06`** | **Disponibilidad de la Plataforma MTD** | Inexistente (sistemas no integrados de trazabilidad) | Disponibilidad mensual $\ge 99.5\%$ (excluyendo mantenimientos) | Monitoreo sintético externo de uptime del API Gateway perimetral | `OBJ-06` / `FCE-06` |
| **`KPI-07`** | **Tasa de Reprocesamiento y Eventos Duplicados** | Inconsistencias de duplicación por retransmisiones manuales | $\le 1\%$ de payloads descartados por duplicidad no justificada | Métricas de control de claves idempotentes `UUIDv4` en `CMP-ING-01` | `OBJ-07` / `FCE-02` |
| **`KPI-08`** | **Tiempo Medio de Resolución de Incidentes (MTTR)** | Tiempos de diagnóstico indeterminados por falta de auditoría | $MTTR \le 2\text{ horas}$ para incidencias técnicas de trazabilidad | Trazabilidad distribuida OpenTelemetry y consola centralizada de logs | `OBJ-08` / `FCE-06` |

---

### 8.8.3. Gestión de lecciones aprendidas y retroalimentación al Repositorio de Arquitectura

El resultado del examen PIR se consolida formalmente en el **Informe de Lecciones Aprendidas de la Implementación**, el cual retroalimenta los activos arquitectónicos corporativos de Yanbal Perú:
1. **Actualización del Catálogo de Bloques de Construcción de Solución (SBB):** Los adaptadores `ADP-SAP`, `ADP-SPY` y la pasarela perimetral B2B se catalogan como activos de software reutilizables para futuros proyectos corporativos de Yanbal en la región andina.
2. **Calibración de Estándares Corporativos de Interoperabilidad:** La experiencia de integración asíncrona mediante CloudEvents v1.0 y mensajería en colas se incorpora como el estándar tecnológico obligatorio para futuras integraciones de la cadena de suministro.
3. **Registro Histórico de Deuda Técnica Subsanada:** Documentación del cierre de todos los waivers aprobados durante el proyecto, acreditando que la solución pasó a producción sin compromisos pendientes de calidad ni vulnerabilidades técnicas.

---

### 8.8.4. Traspaso formal a la Fase H (Gestión del Cambio de Arquitectura)

La culminación exitosa de la Fase G transfiere formalmente la gobernanza del sistema desde el modelo de supervisión de proyectos de desarrollo hacia el **marco continuo de gestión del cambio de arquitectura (Fase H de TOGAF ADM)**:

```
+----------------------------------------------------------------------------------------------------+
|                TRANSICIÓN DE FASE G (GOBERNANZA) A FASE H (GESTIÓN DEL CAMBIO)                     |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [FASE G: GOBERNANZA DE LA IMPLEMENTACIÓN]                                                         |
|  * Enfoque: Proyecto cerrado, construcción, control de calidad, Quality Gates y despliegue.        |
|  * Entregable de cierre: Acta de Aceptación TO-BE y Handover a Operaciones.                        |
|                                                                                                    |
|                                   |                                                                |
|                                   v                                                                |
|                                                                                                    |
|  [FASE H: GESTIÓN DEL CAMBIO DE ARQUITECTURA]                                                      |
|  * Enfoque: Ciclo de vida continuo, operación sostenible y evolución permanente.                   |
|  * Monitoreo del Desempeño: Auditoría constante del SLA de 30 min y salud de los microservicios.  |
|  * Clasificación de Solicitudes de Cambio (RFC):                                                   |
|    - Cambio Tipo 1 (Mantenimiento Operativo): Correcciones menores y parches sin impacto.          |
|    - Cambio Tipo 2 (Modificación Incremental): Nuevos endpoints o incorporación de operadores.     |
|    - Cambio Tipo 3 (Re-arquitectura Fundamental): Sustitución del ERP o migración de nube.         |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

Con este traspaso formal concluye la Fase G de TOGAF ADM, dotando a Yanbal Perú (Unique S.A.) de un esquema de gobernanza riguroso, auditable y plenamente documentado que asegura que la inversión en la plataforma de trazabilidad de despachos B2B entregue el valor estratégico proyectado con total estabilidad operativa.

---

## 8.9. Referencias bibliográficas (Normas APA 7.ª edición)

* Bass, L., Clements, P., & Kazman, R. (2021). *Software architecture in practice* (4.ª ed.). Addison-Wesley Professional.
* Cloud Native Computing Foundation. (2022). *CloudEvents specification v1.0.2*. Linux Foundation. https://cloudevents.io/
* Fowler, M. (2018). *Patterns of enterprise application architecture*. Addison-Wesley.
* International Organization for Standardization. (2018). *Systems and software engineering — Architecture description* (ISO/IEC/IEEE Standard N.° 42010:2018). ISO.
* Newman, S. (2021). *Building microservices: Designing fine-grained systems* (2.ª ed.). O'Reilly Media.
* OpenTelemetry Authors. (2023). *OpenTelemetry: High-quality, ubiquitous, and portable telemetry to enable effective observability*. Cloud Native Computing Foundation. https://opentelemetry.io/
* Project Management Institute. (2021). *A guide to the project management body of knowledge (PMBOK guide)* (7.ª ed.). Project Management Institute.
* Richardson, L., & Ruby, S. (2007). *RESTful web services*. O'Reilly Media.
* The Open Group. (2022). *The TOGAF® Standard, 10th Edition: Architecture Development Method (ADM)*. The Open Group Standard (G220). https://pubs.opengroup.org/architecture/togaf10-adm/
* The Open Group. (2022). *The TOGAF® Standard, 10th Edition: Implementation Governance (Phase G)*. The Open Group Guide (G222).

