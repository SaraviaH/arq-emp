# ANEXOS GENERALES DEL PROYECTO DE ARQUITECTURA EMPRESARIAL
## Compendio Técnico, Matrices de Trazabilidad, Catálogos Oficiales, Contratos, Auditoría y Fuentes Normativas para Yanbal Perú (Unique S.A.)

> **Ubicación oficial del documento:** `10_Anexos_Tecnicos/ANEXOS_GENERALES_PROYECTO.md`  
> **Marco metodológico:** TOGAF Standard 10th Edition (The Open Group) e Ingeniería de Requerimientos ISO/IEC/IEEE 29148.  
> **Organización analizada:** Unique S.A. (Nombre comercial: Yanbal Perú) — Cadena de Suministro B2B.  
> **Principio de gobernanza documental:** Los anexos constituyen la evidencia técnica detallada y los instrumentos operativos que respaldan los nueve capítulos principales del informe de Arquitectura Empresarial. No reimplementan ni duplican información arbitrariamente, sino que consolidan los registros transversales y proporcionan las herramientas de auditoría, trazabilidad y verificación formal. Esta etapa es una actividad complementaria de cierre documental del proyecto y **no constituye la Fase I de TOGAF ADM**.

---

## 1. Estructura y Navegación de los Anexos Generales

El compendio de anexos se organiza en tres niveles documentales:
1. **Fuentes Maestras Canónicas de Referencia:** Documentos independientes centralizados en el repositorio que actúan como base de verdad inmutable.
2. **Anexos Técnicos Específicos Consolidados (`ANX-01` a `ANX-05`):** Fichas técnicas, matrices integrales de extremo a extremo, inventarios y reportes de auditoría.
3. **Secciones de Consolidación Transversal:** Tablas maestras de objetivos, métricas, gobernanza unificada, protocolos de Quality Gates y bibliografía unificada en formato APA 7.ª edición.

```
+----------------------------------------------------------------------------------------------------+
|                         MAPA DE NAVEGACIÓN DE ANEXOS DEL PROYECTO                                  |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [FUENTES MAESTRAS CENTRALIZADAS]                                                                  |
|  * 00_1: Glosario Canónico y Nomenclatura Oficial del Proyecto.                                    |
|  * 00_2: Catálogo Maestro de Actores (ACT-01..16), Sistemas (SIS-01..10) y Procesos.               |
|  * 00_3: Registro Maestro de Supuestos (ASM-01..16), Incertidumbres y Decisiones (DEC-01..07).    |
|  * 01_5: Matriz Maestra de Requerimientos Canónicos (REQ-01 a REQ-24 x 15 campos).                 |
|                                                                                                    |
|  [ANEXOS TÉCNICOS ESPECÍFICOS CONSOLIDADOS]                                                        |
|  * ANX-01: Matriz Consolidada de Trazabilidad Integral Extremo a Extremo (REQ x Fases A-H).        |
|  * ANX-02: Catálogo Consolidado de Riesgos y Mitigaciones (14 Riesgos RSK-XX x Severidad PxI).     |
|  * ANX-03: Compendio Maestro de Contratos de Arquitectura (AC-PKG-01..05) y Formularios WVR / RFC. |
|  * ANX-04: Inventario Maestro y Catálogo Técnico de Diagramas Canónicos (DIAG-01 a DIAG-11).       |
|  * ANX-05: Informe Final de Auditoría de Integridad, Consistencia y Trazabilidad Documental.       |
|                                                                                                    |
|  [ENTREGABLES DE AUDITORÍA Y REMEDIACIÓN DOCUMENTAL CONTROLADA]                                    |
|  * AUD_10: Informe de Remediación Documental Controlada (Resolución H-01 a H-04).                  |
|  * AUD_11: Matriz de Control de Cambios de Auditoría (Registro exhaustivo CHG-REM-01..12).         |
|  * AUD_12: Informe de Verificación Post-Remediación y Dictamen Final (Trazabilidad 100%, CMMI-3). |
|                                                                                                    |
|  [SECCIONES TRANSVERSALES EN ESTE DOCUMENTO]                                                       |
|  * Sección 2: Matriz de Correspondencia entre Anexos y Capítulos Principales.                     |
|  * Sección 3: Matriz Consolidada de Objetivos (OBJ), FCE y Métricas de Rendimiento (KPI).         |
|  * Sección 4: Modelo de Gobernanza Unificado y Matriz RACI Maestra de los 16 Actores Canónicos.   |
|  * Sección 5: Protocolo Integral de Puntos de Control (Quality Gates) y Criterios de Aceptación.   |
|  * Sección 6: Bibliografía General Unificada del Proyecto (Normas APA 7.ª edición).                |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 2. Matriz de Correspondencia entre Anexos y Capítulos Principales

A continuación se establece el mapeo bidireccional entre los capítulos principales del informe de Arquitectura Empresarial (Fase Preliminar y Fases A a H) y los anexos que respaldan técnica y metodológicamente cada sección:

| Capítulo / Fase del Proyecto | Documento Oficial en Repositorio | Fuentes Maestras Aplicables | Anexos Técnicos de Respaldo | Diagramas Canónicos Vinculados |
| :--- | :--- | :--- | :--- | :---: |
| **1. Fase Preliminar** | [`01_Fase_Preliminar_Completa.md`](../01_Fase_Preliminar/01_Fase_Preliminar_Completa.md) | `00_1`, `00_2`, `00_3`, `01_5` | `ANX-01`, `ANX-05` | Marco Preliminar |
| **2. Fase A: Visión de Arquitectura** | [`02_Fase_A_Vision_Arquitectura_Completa.md`](../02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md) | `00_2`, `00_3`, `01_5` | `ANX-01`, `ANX-02`, `ANX-04` | `DIAG-01` |
| **3. Fase B: Arquitectura de Negocio** | [`03_Fase_B_Arquitectura_Negocio_Completa.md`](../03_Fase_B_Arquitectura_de_Negocio/03_Fase_B_Arquitectura_Negocio_Completa.md) | `00_1`, `00_2` | `ANX-01`, `ANX-04` | `DIAG-02` a `DIAG-06` |
| **4. Fase C: Sistemas de Información** | [`04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md`](../04_Fase_C_Arquitectura_de_Sistemas_de_Informacion/04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md) | `00_1`, `00_2`, `01_5` | `ANX-01`, `ANX-03` | Figuras C-1 y C-2 |
| **5. Fase D: Arquitectura Tecnológica** | [`05_Fase_D_Arquitectura_Tecnologica_Completa.md`](../05_Fase_D_Arquitectura_Tecnologica/05_Fase_D_Arquitectura_Tecnologica_Completa.md) | `00_1`, `00_3` | `ANX-01`, `ANX-04` | `DIAG-07` |
| **6. Fase E: Oportunidades y Soluciones** | [`06_Fase_E_Oportunidades_y_Soluciones_Completa.md`](../06_Fase_E_Oportunidades_y_Soluciones/06_Fase_E_Oportunidades_y_Soluciones_Completa.md) | `01_5`, `00_2` | `ANX-01`, `ANX-02`, `ANX-04` | `DIAG-08` |
| **7. Fase F: Planificación de Migración** | [`07_Fase_F_Planificacion_de_la_Migracion.md`](../07_Fase_F_Planificacion_de_Migracion/07_Fase_F_Planificacion_de_la_Migracion.md) | `00_3`, `01_5` | `ANX-01`, `ANX-02`, `ANX-04` | `DIAG-09` |
| **8. Fase G: Gobernanza de Implementación**| [`08_Fase_G_Gobernanza_de_la_Implementacion.md`](../08_Fase_G_Gobierno_de_Implementacion/08_Fase_G_Gobernanza_de_la_Implementacion.md) | `00_2`, `01_5` | `ANX-01`, `ANX-03`, `ANX-04` | `DIAG-10` |
| **9. Fase H: Gestión del Cambio** | [`09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md`](../09_Fase_H_Gestion_del_Cambio/09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md) | `00_1`, `00_2`, `00_3` | `ANX-01`, `ANX-02`, `ANX-03`, `ANX-05` | `DIAG-11` |

---

## 3. Matriz Consolidada de Objetivos, Factores Críticos de Éxito y KPIs

A continuación se articula la relación entre los ocho Objetivos Estratégicos (`OBJ-01` a `OBJ-08`), los seis Factores Críticos de Éxito (`FCE-01` a `FCE-06`) y los ocho Indicadores de Desempeño (`KPI-01` a `KPI-08`), detallando su meta de diseño y el estado de validación corporativa:

| Código OBJ | Descripción del Objetivo Estratégico SMART | Factores Críticos (FCE) | Indicador Oficial Vinculado (KPI) | Línea Base AS-IS | Meta de Diseño TO-BE | Estado de Validación |
| :---: | :--- | :---: | :---: | :--- | :--- | :---: |
| **`OBJ-01`** | Reducir el margen de desfase de actualización de estados de despacho en ruta a un máximo de 30 minutos. | `FCE-01`<br>`FCE-02` | **`KPI-01`**: Latencia Media de Actualización ($\Delta T$) | Desfase de hasta ~2 h en seguimiento nacional (`[TO]`) | $\Delta T \le 30\text{ minutos}$ en ruta B2B (`DEC-05`) | Meta de Diseño propuesta; línea base sujeta a medición (`ASM-03`) |
| **`OBJ-02`** | Estandarizar la captura y sincronización de eventos de tránsito con marcas temporales auditables. | `FCE-02`<br>`FCE-03` | **`KPI-02`**: Cumplimiento de SLA en ventana $\le 30\text{ min}$ | Reportes batch heterogéneos y desalineados | $\ge 98\%$ eventos en ventana (Piloto $\ge 95\%$) | Meta analítica de diseño (`DEC-05`); piloto según `ASM-16` |
| **`OBJ-03`** | Asegurar la integridad causal y consistencia en la secuencia de estados de cada pedido. | `FCE-03`<br>`FCE-04` | **`KPI-03`**: Integridad Causal de Estados | Saltos periódicos de estados por desorden | $100\%$ de eventos en secuencia lógica válida | Criterio innegociable de aceptación en Quality Gates |
| **`OBJ-04`** | Lograr cobertura total de trazabilidad en los 24 departamentos hacia distribuidoras y sedes B2B. | `FCE-04`<br>`FCE-05` | **`KPI-04`**: Cobertura Territorial de Despachos | Visibilidad limitada a Lurín y cabeceras principales | $100\%$ despachos monitoreados en 24 departamentos | Sujeto a homologación de transportistas (`REQ-RES-001`) |
| **`OBJ-05`** | Reducir las consultas y reclamos reiterativos en el Call Center por falta de información de ruta. | `FCE-05` | **`KPI-05`**: Reducción de Incidentes en Call Center | Alto volumen de llamadas por incertidumbre de pedido | Reducción $\ge 40\%$ de tickets por desinformación | Medición condicionada a datos de `CELLFORCE` (`ASM-06`) |
| **`OBJ-06`** | Establecer un canal formal de trazabilidad y alerta para retornos por logística inversa hacia Lurín. | `FCE-01`<br>`FCE-03` | **`REQ-NEG-004`** / **`REQ-FUN-005`** (Logística Inversa) | Registro manual diferido; aviso informal de bultos devueltos | $100\%$ incidentes notificados a Lurín sin tocar SAP | Blindado por salvaguarda `REQ-RES-002` con `ACT-03` |
| **`OBJ-07`** | Garantizar la alta disponibilidad y resiliencia de la plataforma de eventos de despacho. | `FCE-06` | **`KPI-06`**: Disponibilidad de la Plataforma MTD | Inexistente (sistemas aislados no integrados) | Disponibilidad mensual $\ge 99.5\%$ | Validado en pruebas de estrés de infraestructura |
| **`OBJ-08`** | Preservar las inversiones legadas e interoperar mediante estándares abiertos sin hardware propietario. | `FCE-02`<br>`FCE-04` | **`KPI-07`**: Tasa de Descarte por Duplicidad / Error | Conexiones punto a punto rígidas de terceros | $\le 1\%$ de descartes; $MTTR \le 2\text{ h}$ (`KPI-08`) | Regido por `REQ-RES-004` y `ASM-05` |

---

## 4. Modelo de Gobernanza Unificado y Matriz RACI Maestra

Para consolidar la gobernanza institucional del proyecto, se articula la matriz de responsabilidades integral entre los dieciséis actores canónicos (`ACT-01` a `ACT-16`) frente a los rituales clave de diseño, migración, control de calidad y cambio continuo:

```
+----------------------------------------------------------------------------------------------------+
|                         CONSEJO DE ARQUITECTURA (ARCHITECTURE BOARD)                               |
+----------------------------------------------------------------------------------------------------+
|  * Sponsor Ejecutivo: Gerente de TI (ACT-13) - Presidencia y Aprobación de Presupuesto.           |
|  * Dirección Técnica: Arquitecto Líder (ACT-12) - Secretaría Técnica y Dictamen de Solución.       |
|  * Ciberseguridad: Oficial de Seguridad CISO (ACT-11) - Veto Vinculante de Seguridad Perimetral.   |
|  * Operaciones Logísticas: Jefatura de Distribución y Rutas (ACT-06) - Propietario del Proceso.     |
|  * Integridad ERP: Jefatura de Almacén Lurín (ACT-03) - Veto Vinculante de Aislamiento de SAP R3.  |
+----------------------------------------------------------------------------------------------------+
```

### Matriz RACI Maestra del Ciclo Completo (16 Actores Canónicos):

* **R (Responsible):** Quien ejecuta o lidera técnicamente el artefacto.
* **A (Accountable):** Quien aprueba formalmente y asume la responsabilidad final.
* **C (Consulted):** Quien provee insumos, criterios operativos o restricciones.
* **I (Informed):** Quien es notificado formalmente sobre los acuerdos y avances.

| Proceso Clave de la Arquitectura Empresarial | ACT-01 (Manuf) | ACT-02 (Envas) | ACT-03 (Alm) | ACT-04 (Pick) | ACT-05 (Desp) | ACT-06 (Dist) | ACT-07 (Trans) | ACT-08 (Call) | ACT-09 (Dest) | ACT-10 (Recep) | ACT-11 (CISO) | ACT-12 (Arq) | ACT-13 (Dir TI) | ACT-14 (QA) | ACT-15 (PMO) | ACT-16 (TI Ext) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Definición de Principios y Requerimientos (Fase Prel/A)** | I | I | C | C | C | **A** | C | C | I | I | C | **R** | **A** | C | C | I |
| **Modelado de Procesos AS-IS y TO-BE (Fase B)** | C | C | C | C | C | **A** | C | I | I | I | I | **R** | I | C | I | I |
| **Diseño Lógico y Contratos de Datos (Fase C)** | I | I | C | I | I | C | C | I | I | I | C | **R** | **A** | C | I | C |
| **Topología y Seguridad Tecnológica (Fase D)** | I | I | I | I | I | I | C | I | I | I | **A** | **R** | **A** | C | I | C |
| **Estructuración de Paquetes PKG-01..05 (Fase E)** | I | I | C | I | I | C | I | I | I | I | I | **R** | **A** | C | C | I |
| **Roadmap y Arquitecturas de Transición (Fase F)** | I | I | C | I | I | C | C | I | I | I | C | **R** | **A** | C | **R** | I |
| **Emisión de Contratos de Arquitectura (Fase G)** | I | I | **A** | I | I | C | C | I | I | I | C | **R** | **A** | C | C | C |
| **Certificación de Quality Gates 1, 2 y 3 (Fase G)** | I | I | **A** | I | I | **A** | C | I | I | I | **A** | **R** | **A** | **R** | C | C |
| **Dictamen de Solicitudes de Dispensa (Waivers - Fase G)**| I | I | C | I | I | C | I | I | I | I | C | **R** | **A** | C | C | I |
| **Gestión Continua de Cambios RFC Tipo 1/2/3 (Fase H)** | I | I | **A** | I | I | **A** | C | I | I | I | **A** | **R** | **A** | C | C | C |
| **Activación de Nuevo Ciclo TOGAF ADM (Fase H)** | I | I | C | I | I | C | I | I | I | I | C | **R** | **A** | I | C | I |

---

## 5. Protocolo Integral de Puntos de Control (*Quality Gates*) y Aceptación

El aseguramiento de la conformidad arquitectónica se ejecuta a través de tres compuertas de calidad secuenciales e innegociables:

```
+----------------------------------------------------------------------------------------------------+
|                         MAPA DE COMPUERTAS DE CALIDAD (QUALITY GATES)                              |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [GATE 1: CERTIFICACIÓN AT-1] --------> [GATE 2: CERTIFICACIÓN AT-2] --------> [GATE 3: TO-BE]     |
|  * Semana S12                           * Semana S18                           * Semana S24        |
|  * Paquetes: PKG-01, 02, 03             * Paquetes: PKG-04 (Piloto B2B)        * Paquete: PKG-05   |
|  * Cero escrituras en SAP R3            * Secuencia causal 100% (KPI-03)       * Cobertura 24 Dptos|
|  * Sello inmutable T1 en CMP-AUD-04     * SLA piloto >= 95% (ventana 30 min)   * Telemetría activa |
|  * Extracción Lurín <= 15 min           * Store-and-Forward comprobado         * Cero waivers pend.|
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

### Resumen de Criterios y Entregables de Salida por Gate:

1. **GATE 1 (Cierre de AT-1 — Semana S12):**
   * *Evidencias Requeridas:* Reporte de auditoría de permisos de base de datos de `SAP R3` (solo `SELECT` en staging); trazas de ejecución de `CMP-AUD-04` estampando $T_1$; reporte SonarQube con cobertura $\ge 85\%$ y cero fallas SAST críticas; certificación de cifrado TLS 1.3.
   * *Entregable:* Acta de Certificación de la Arquitectura de Transición 1 suscrita por `ACT-13`, `ACT-12`, `ACT-11` y `ACT-03`. Habilita el inicio de onboarding externo (`PKG-04`).
2. **GATE 2 (Cierre de AT-2 — Semana S18):**
   * *Evidencias Requeridas:* Reporte de pruebas piloto en rutas reales (Lima - Trujillo / Arequipa) durante 4 semanas; comprobación de $100\%$ de consistencia causal de estados (`KPI-03`); verificación empírica de latencia en piloto con cumplimiento $\ge 95\%$ en ventana $\le 30\text{ min}$ (orientado a la meta $\ge 98\%$, `KPI-02`); informe de ensayos de desconexión y retransmisión *Store-and-Forward*.
   * *Entregable:* Acta de Certificación de la Arquitectura de Transición 2 suscrita por `ACT-13`, `ACT-12` y `ACT-06`. Autoriza el despliegue masivo (*Rollout*) hacia los 24 departamentos.
3. **GATE 3 (Cierre de TO-BE — Semana S24):**
   * *Evidencias Requeridas:* Cobertura nacional del $100\%$ de pedidos B2B despachados desde Lurín; tableros de telemetría de SLAs operativos en tiempo real en Grafana con refresco $\le 5\text{ seg}$; canal de retorno de logística inversa hacia Almacén Lurín (`CMP-NOT-06`) operando; matriz de trazabilidad con $100\%$ de los 24 requerimientos satisfechos; cero dispensas (*Waivers*) activas.
   * *Entregable:* Acta de Aceptación de Arquitectura Empresarial TO-BE y firma del *Handover to Run* hacia Operaciones de TI y Logística.

---

## 6. Bibliografía General Unificada del Proyecto (Normas APA 7.ª edición)

* Bass, L., Clements, P., & Kazman, R. (2021). *Software architecture in practice* (4.ª ed.). Addison-Wesley Professional.
* Cloud Native Computing Foundation. (2022). *CloudEvents specification v1.0.2*. Linux Foundation. https://cloudevents.io/
* Drivin. (2023). *Caso de éxito Yanbal: Optimización y trazabilidad logística en Latinoamérica*. Drivin TMS Case Studies. https://driv.in/
* Fowler, M. (2018). *Patterns of enterprise application architecture*. Addison-Wesley.
* International Organization for Standardization. (2018). *Systems and software engineering — Architecture description* (ISO/IEC/IEEE Standard N.° 42010:2018). ISO.
* International Organization for Standardization. (2019). *Systems and software engineering — Life cycle processes — Requirements engineering* (ISO/IEC/IEEE Standard N.° 29148:2018). ISO.
* Newman, S. (2021). *Building microservices: Designing fine-grained systems* (2.ª ed.). O'Reilly Media.
* Object Management Group. (2017). *Unified Modeling Language (OMG UML) specification version 2.5.1*. Object Management Group. https://www.omg.org/spec/UML/2.5.1/
* OpenTelemetry Authors. (2023). *OpenTelemetry: High-quality, ubiquitous, and portable telemetry to enable effective observability*. Cloud Native Computing Foundation. https://opentelemetry.io/
* Project Management Institute. (2021). *A guide to the project management body of knowledge (PMBOK guide)* (7.ª ed.). Project Management Institute.
* Richardson, L., & Ruby, S. (2007). *RESTful web services*. O'Reilly Media.
* The Open Group. (2022a). *The TOGAF® Standard, 10th Edition: Introduction and core concepts*. The Open Group Standard (G220). https://pubs.opengroup.org/architecture/togaf10-adm/
* The Open Group. (2022b). *The TOGAF® Standard, 10th Edition: Architecture Development Method (ADM)*. The Open Group Standard (G220).
* The Open Group. (2022c). *The TOGAF® Standard, 10th Edition: Implementation Governance (Phase G)*. The Open Group Guide (G222).
* The Open Group. (2022d). *The TOGAF® Standard, 10th Edition: Architecture Change Management (Phase H)*. The Open Group Guide (G223).
* Yanbal Perú. (2024). *Portal corporativo y comercial de Unique S.A. (Yanbal Perú)*. Yanbal International. https://www.yanbal.com/pe

