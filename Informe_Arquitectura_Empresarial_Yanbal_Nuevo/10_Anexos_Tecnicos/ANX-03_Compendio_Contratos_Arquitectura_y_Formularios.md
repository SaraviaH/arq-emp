# ANEXO 3: COMPENDIO MAESTRO DE CONTRATOS DE ARQUITECTURA, DISPENSAS Y GESTIÓN DEL CAMBIO

> **Ubicación oficial:** `10_Anexos_Tecnicos/ANX-03_Compendio_Contratos_Arquitectura_y_Formularios.md`  
> **Documento maestro articulado:** [`ANEXOS_GENERALES_PROYECTO.md`](ANEXOS_GENERALES_PROYECTO.md)  
> **Fuentes de verdad arquitectónica:** Fases G y H de TOGAF ADM.  
> **Propósito:** Compilar los cinco Contratos de Arquitectura vinculantes (`AC-PKG-01` a `AC-PKG-05`) y los instrumentos formales de gestión de excepciones (`WVR-XXXX`) y solicitudes de cambio (`RFC-ARCH-XXXX`), estableciendo sus reglas de operación, derechos de veto y compromisos ejecutables.

---

## 1. Compendio de Contratos de Arquitectura (*Architecture Contracts*)

En el marco TOGAF ADM, los Contratos de Arquitectura son los acuerdos formales mediante los cuales el Consejo de Arquitectura (*Architecture Board*) de Yanbal Perú delega la construcción, integración y puesta en marcha de los componentes del sistema a los equipos ejecutores internos y a los socios externos.

```
+----------------------------------------------------------------------------------------------------+
|                         MAPA DE CONTRATOS DE ARQUITECTURA (AC-PKG-01..05)                          |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ARCHITECTURE BOARD (ACT-13, ACT-12, ACT-11, ACT-06, ACT-03)]                                     |
|                                |                                                                   |
|       +------------------------+------------------------+------------------------+                 |
|       |                        |                        |                        |                 |
|       v                        v                        v                        v                 |
|  [AC-PKG-01]              [AC-PKG-02]              [AC-PKG-03]              [AC-PKG-04]            |
|  Datos & Integración      Core MTD Platform        Adaptadores Internos     Onboarding B2B         |
|  - Esquemas CloudEvents   - CMP-ING-01..PUB-05     - ADP-SAP (Staging)      - ADP-LOG / Gateway    |
|  - Diccionario Canónico   - Sello inmutable T1     - ADP-SPY, ADP-CRM       - Portal Contingencia  |
|  - JSON Schema draft-07   - Idempotencia UUIDv4    - Blindaje SAP R3        - Store-and-Forward    |
|                                                                                                    |
|                                         +----------------------------------------+                 |
|                                         |                                                          |
|                                         v                                                          |
|                                    [AC-PKG-05]                                                     |
|                                    Monitoreo, Telemetría & SLAs                                    |
|                                    - OpenTelemetry / Grafana                                       |
|                                    - Alertas SLA <= 30 min                                         |
|                                    - Notificación Retorno (CMP-NOT-06)                             |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

### 1.1. Ficha Maestra: Contrato `AC-PKG-01` (Gobernanza de Datos y Contratos de Integración)
* **Código del Contrato:** `AC-PKG-01-DATOS-INTEGRACION`
* **Paquete Asociado:** `PKG-01` (Gobernanza de Datos, Modelado Canónico y Contratos de Integración).
* **Partes Firmantes:** Architecture Board (Emisor) y Equipo de Arquitectura de Datos e Integración (Ejecutor).
* **Objetivo:** Definir y estandarizar la semántica de datos de despacho y transporte B2B para todos los sistemas.
* **Componentes y Entregables:** Esquemas JSON Schema draft-07; especificación CloudEvents v1.0; catálogo canónico de eventos (`00_1`); colecciones Postman de prueba sintáctica.
* **Requerimientos Vinculantes:** `REQ-ARQ-001`, `REQ-ARQ-002`, `REQ-ARQ-003`, `REQ-INT-003`, `REQ-INT-004` (y criterios técnicos de transición `CRT-TRA-01`, `CRT-TRA-02` y seguridad `CRT-SEG-01`).
* **Salvaguardas Ineludibles:** Prohibición absoluta de campos propietarios no documentados; obligatoriedad de marcas temporales duales ISO 8601 UTC-5 ($T_0$ y $T_1$).
* **Criterios de Aceptación:** $100\%$ de los esquemas validados con linter automatizado; aprobación colegiada del Architecture Board en Semana S4.

---

### 1.2. Ficha Maestra: Contrato `AC-PKG-02` (Plataforma Core MTD y Microservicios de Eventos)
* **Código del Contrato:** `AC-PKG-02-CORE-MTD-MICROSERVICIOS`
* **Paquete Asociado:** `PKG-02` (Plataforma Core MTD y Microservicios de Eventos de Despacho).
* **Partes Firmantes:** Architecture Board (Emisor) y Fábrica de Software / Equipo Core MTD (Ejecutor).
* **Objetivo:** Construir el motor central de ingesta, correlación, validación y estampado temporal de eventos.
* **Componentes y Entregables:** Microservicios `CMP-ING-01`, `CMP-COR-02`, `CMP-VAL-03`, `CMP-AUD-04`, `CMP-PUB-05` y runtime de excepciones de `CMP-NOT-06`; imágenes Docker endurecidas; charts de Helm.
* **Requerimientos Vinculantes:** `REQ-FUN-001` a `REQ-FUN-004`, `REQ-RNF-001` a `REQ-RNF-003`, `REQ-ARQ-001`, `REQ-ARQ-002`.
* **Salvaguardas Ineludibles:**
  1. *Idempotencia Obligatoria:* Descarte de eventos duplicados mediante cabecera `UUIDv4` sin alterar estado.
  2. *Auditoría Temporal Inmutable:* Estampación de $T_1$ en `CMP-AUD-04` inmediatamente tras superar la validación sintáctica.
  3. *Persistencia Aislada:* Persistencia exclusiva en la base de datos de eventos (`SIS-03`); cero conexiones con `SAP R3`.
* **Criterios de Aceptación:** Cobertura de pruebas unitarias $\ge 85\%$; tiempo de procesamiento $\le 500\text{ ms}$ a 100 req/s; cero vulnerabilidades críticas en análisis SAST/DAST. Certificado en **GATE 1**.

---

### 1.3. Ficha Maestra: Contrato `AC-PKG-03` (Adaptadores de Integración de Sistemas Internos)
* **Código del Contrato:** `AC-PKG-03-ADAPTADORES-INTERNOS`
* **Paquete Asociado:** `PKG-03` (Adaptadores de Integración de Sistemas Internos).
* **Partes Firmantes:** Architecture Board (Emisor) y Equipo de Middleware / Integraciones SAP (Ejecutor).
* **Objetivo:** Extraer pedidos y hitos de preparación de Lurín desacoplando el core ERP de la plataforma MTD.
* **Componentes y Entregables:** Conector `ADP-SAP` (solo lectura / staging `ZTRK_DESPACHO_STG`), conector `ADP-SPY`, conector `ADP-CRM` (hacia `CELLFORCE` `SIS-09`), colas intermedias de mensajería.
* **Requerimientos Vinculantes:** `REQ-INT-001`, `REQ-INT-002`, `REQ-INT-004`, `REQ-RES-002`, `REQ-RES-003` (y criterio técnico de aislamiento `CRT-SEG-02`).
* **Salvaguardas Ineludibles:**
  1. *Aislamiento Total de Escritura en SAP (`REQ-RES-002`):* Prohibición absoluta de sentencias `INSERT`, `UPDATE` o `DELETE` sobre `SAP R3`. Veto vinculante de `ACT-03` (Jefe de Almacén Lurín).
  2. *Lectura No Bloqueante:* Consultas de solo lectura sobre vistas indexadas o staging; cero contención transaccional durante el picking en Lurín.
  3. *Tolerancia a Desconexión:* Retención en colas locales ante mantenimientos de `SAP R3`.
* **Criterios de Aceptación:** Auditoría de base de datos que certifique usuario de servicio con privilegios exclusivos `SELECT`; sincronización en Lurín con latencia $\le 15\text{ minutos}$ (`DEC-05`). Certificado en **GATE 1**.

---

### 1.4. Ficha Maestra: Contrato `AC-PKG-04` (Onboarding y Pasarela de Operadores Logísticos)
* **Código del Contrato:** `AC-PKG-04-ONBOARDING-PASARELA-B2B`
* **Paquete Asociado:** `PKG-04` (Onboarding y Pasarela Segura para Operadores Logísticos Tercerizados).
* **Partes Firmantes:** Architecture Board (Emisor), Equipo B2B y Empresas de Transporte Tercerizado (`ACT-07` / `ACT-16`).
* **Objetivo:** Habilitar la captura estandarizada y segura de eventos en ruta desde los operadores externos.
* **Componentes y Entregables:** Adaptador Logístico `ADP-LOG`, API Gateway Perimetral B2B con WAF y mTLS, Portal Web de Contingencia para carga manual, entorno Sandbox de homologación.
* **Requerimientos Vinculantes:** `REQ-INT-003`, `REQ-INT-004`, `REQ-RES-004`, `REQ-RNF-001`, `REQ-RNF-004` (y criterios derivados `CRT-TRA-02`, `CRT-TRA-03`, `CRT-TRA-04` y `CRT-SEG-01`).
* **Salvaguardas Ineludibles:**
  1. *Sin Hardware GPS Propietario (`REQ-RES-004`, `ASM-05`):* Interoperabilidad basada exclusivamente en REST/JSON o web; no se impone equipamiento de a bordo a transportistas.
  2. *Cifrado y Seguridad Perimetral:* TLS 1.3 mandatorio; autenticación mediante mTLS o tokens JWT (RS256); Rate Limiting activo en Gateway.
  3. *Store-and-Forward Mandatorio:* Los sistemas y apps de los transportistas deben retener eventos localmente ante pérdidas de señal celular (`ASM-16`), retransmitiendo automáticamente sin duplicar transacciones.
* **Criterios de Aceptación:** Homologación en Sandbox con tasa de éxito $\ge 99.5\%$; cumplimiento del umbral de tolerancia en piloto $\ge 95\%$ en ventana $\le 30\text{ min}$ (meta analítica $\ge 98\%$, `KPI-02`, `DEC-05`). Certificado en **GATE 2**.

---

### 1.5. Ficha Maestra: Contrato `AC-PKG-05` (Monitoreo Operacional, Telemetría y Dashboards de SLA)
* **Código del Contrato:** `AC-PKG-05-MONITOREO-TELEMETRIA-SLA`
* **Paquete Asociado:** `PKG-05` (Monitoreo Operacional, Telemetría y Dashboards de Control de SLAs).
* **Partes Firmantes:** Architecture Board (Emisor) y Equipo de Infraestructura, SRE y Observabilidad (Ejecutor).
* **Objetivo:** Proporcionar observabilidad de extremo a extremo, alertas tempranas de desvío y soporte de logística inversa.
* **Componentes y Entregables:** Agente OpenTelemetry v1.20, exportadores Prometheus, servidores Grafana con dashboards de SLAs en tiempo real, módulo de alerta temprana ($> 25\text{ min}$), consola de notificación de retorno para Almacén Lurín (`CMP-NOT-06` / `REQ-FUN-005`).
* **Requerimientos Vinculantes:** `REQ-FUN-004`, `REQ-FUN-005`, `REQ-RNF-001`, `REQ-RNF-002`, `REQ-RNF-003` (y criterio técnico de inmutabilidad `CRT-SEG-03`).
* **Salvaguardas Ineludibles:**
  1. *Sobrecarga de Monitoreo Mínima:* Impacto en rendimiento de microservicios transaccionales $\le 2\%$.
  2. *Canal de Retorno Seguro:* Notificación estructurada de entrega fallida hacia Almacén Lurín (`ACT-03`) sin escrituras directas en `SAP R3`.
  3. *Pistas de Auditoría Inmutables:* Conservación de logs de eventos durante al menos 12 meses.
* **Criterios de Aceptación:** Refresco de tableros en tiempo real $\le 5\text{ segundos}$; cálculo automático de `KPI-01` y `KPI-02`; cobertura nacional de los 24 departamentos verificada. Certificado en **GATE 3**.

---

## 2. Formularios Oficiales de Gobernanza de Arquitectura

---

### 2.1. Formulario Canónico de Solicitud de Dispensa de Arquitectura (`WVR-XXXX`)

```markdown
====================================================================================================
FORMATO OFICIAL: SOLICITUD DE DISPENSA DE ARQUITECTURA (ARCHITECTURE WAIVER)
Código Canónico: WVR-202X-XXXX
====================================================================================================

1. INFORMACIÓN GENERAL DE LA SOLICITUD
   * Fecha de Solicitud: [AAAA-MM-DD / Semana Relativa S1..S24]
   * Paquete de Trabajo / Contrato: [PKG-01..05 / AC-PKG-XX]
   * Equipo Solicitante / Entidad: [Equipo Interno / Empresa de Transporte ACT-07]
   * Responsable Técnico Solicitante: [Nombre, Cargo, Correo Electrónico]

2. DESCRIPCIÓN DE LA DESVIACIÓN ARQUITECTÓNICA
   * Componente o Interfaz Afectada: [e.g., ADP-LOG / Endpoint API Gateway / CMP-COR-02]
   * Estándar, Principio o Restricción Infringida: [e.g., Autenticación básica temporal por falta de soporte mTLS en transportista]
   * Justificación Técnica / Operativa: [Fundamentación detallada de la causa raíz que impide cumplir el estándar]

3. EVALUACIÓN DE CRITICIDAD Y RIESGO
   * Nivel de Desviación:
     [ ] Nivel 1: Menor (Impacto cosmético interno; resuelto por Arquitecto Líder ACT-12)
     [X] Nivel 2: Mayor (Desviación temporal de estándares; requiere evaluación formal del Board)
     [ ] Nivel 3: Crítica / Inadmisible (Intento de tocar SAP R3 o desvío a B2C - VETO AUTOMÁTICO)
   * Impacto en Seguridad y Privacidad: [Dictamen previo del CISO ACT-11]
   * Impacto en Disponibilidad y SLAs: [Evaluación de degradación de latencia o disponibilidad]
   * Riesgo sobre Aislamiento de SAP R3: [ ] Nulo (Confirmado)   [ ] Existente (Causal de Rechazo)

4. PLAN DE MITIGACIÓN Y COMPENSACIÓN TEMPORAL
   * Controles Compensatorios Propuestos: [e.g., Restricción de acceso por IP Whitelisting estricta y monitoreo dedicado de logs]
   * Condiciones Operativas Provisionales: [Medidas de contingencia aplicables]

5. PLAN DE REMEDIACIÓN Y LIQUIDACIÓN DE DEUDA TÉCNICA
   * Acciones Técnicas Definitivas: [Cronograma y adecuaciones para volver al estándar oficial]
   * Fecha Límite Perentoria de Cierre: [Obligatoriamente <= 6 semanas y antes del siguiente Quality Gate]
   * Presupuesto y Recursos Garantizados: [Responsable de la ejecución]

6. RESOLUCIÓN DEL CONSEJO DE ARQUITECTURA (ARCHITECTURE BOARD)
   * Dictamen Oficial: [ ] APROBADO CONDICIONADO    [ ] RECHAZADO    [ ] DEVUELTO PARA AJUSTES
   * Justificación del Dictamen: [Fundamento técnico-operativo de la resolución colegiada]
   * Registro en el Libro de Excepciones: Folio N.° _________
   * Firmas de Autorización Obligatorias:
     - Sponsor Ejecutivo / Gerente de TI (ACT-13): ________________________________________
     - Arquitecto Líder de Solución (ACT-12): ____________________________________________
     - Oficial de Seguridad de la Información (ACT-11): __________________________________
     - Jefatura de Almacén Lurín (ACT-03 - Si aplica a datos de stock/ERP): _______________
====================================================================================================
```

---

### 2.2. Formulario Canónico de Solicitud de Cambio de Arquitectura (`RFC-ARCH-XXXX`)

```markdown
====================================================================================================
FORMATO OFICIAL: SOLICITUD DE CAMBIO DE ARQUITECTURA (REQUEST FOR ARCHITECTURE CHANGE)
Código Canónico: RFC-ARCH-202X-XXXX
====================================================================================================

1. DATOS DE IDENTIFICACIÓN
   * Fecha de Emisión: [AAAA-MM-DD / Semana Relativa]
   * Actor / Área Solicitante: [ACT-06 (Distribución) / ACT-07 (Transportista) / ACT-12 (TI)]
   * Responsable de la Solicitud: [Nombre, Cargo y Correo Institucional]
   * Título Descriptivo: [Breve título identificador del cambio solicitado]

2. CLASIFICACIÓN DE LA SOLICITUD
   * Tipo de Cambio Arquitectónico:
     [ ] TIPO 1: Mantenimiento Operativo y Correcciones Menores (No altera contratos ni esquemas)
     [ ] TIPO 2: Modificación Incremental de Arquitectura (Nuevos socios, campos opcionales, adaptadores)
     [ ] TIPO 3: Cambio Fundamental / Re-arquitectura (Sustitución de SAP, paso a B2C, flota propia IoT)
   * Prioridad de Negocio: [ ] Alta   [ ] Media   [ ] Baja
   * Origen del Cambio: [ ] Degradación de KPI   [ ] Nuevo Transportista   [ ] Requerimiento Legal   [ ] Mejora Operativa

3. DESCRIPCIÓN TÉCNICA DEL CAMBIO
   * Situación Actual (Línea Base Afectada): [Detalle de los componentes e interfaces actuales]
   * Situación Propuesta: [Detalle técnico de la modificación arquitectónica requerida]
   * Componentes Impactados: [CMP-ING-01..NOT-06 / ADP-SAP / ADP-SPY / ADP-LOG / ADP-CRM / SIS-XX]
   * Contratos de Arquitectura Afectados: [AC-PKG-01..05 / OpenAPI 3.0 / CloudEvents v1.0]

4. FILTRO DE SALVAGUARDAS Y RESTRICCIONES FUNDACIONALES
   * ¿Pretende habilitar accesos de escritura sobre SAP R3? [ ] SÍ (VETO AUTOMÁTICO)  [X] NO
   * ¿Propone extender la trazabilidad hacia consumidoras B2C? [ ] SÍ (VETO / TIPO 3)  [X] NO
   * ¿Impone hardware GPS propietario a empresas de transporte? [ ] SÍ (RECHAZO)         [X] NO
   * ¿Mantiene el mecanismo Store-and-Forward ante fallas de red? [X] SÍ               [ ] NO

5. EVALUACIÓN DE IMPACTO MULTIDIMENSIONAL (Secretaría Técnica ACT-12)
   * Impacto en Procesos de Negocio B2B y Lead Times: [Evaluación cualitativa y cuantitativa]
   * Impacto en Datos, Esquemas JSON y Modelos Lógicos: [Evaluación de retrocompatibilidad]
   * Impacto en Aplicaciones, Microservicios e Idempotencia: [Análisis de acoplamiento y colas]
   * Impacto en Infraestructura, Topología de Red y Cómputo: [Consumo de recursos Kubernetes / VPC]
   * Impacto en Seguridad de Comunicaciones y Cifrado TLS 1.3: [Evaluación por CISO ACT-11]
   * Impacto en SLAs (<= 30 min) y Objetivos Estratégicos: [Riesgo sobre KPI-01 a KPI-08]

6. PLAN DE TRANSICIÓN, PRUEBAS Y ROLLBACK
   * Estrategia de Homologación: [Pruebas en Sandbox / Contract Testing / Staging]
   * Plan de Rollback Inmediato: [Procedimiento de reversión en menos de 60 segundos ante incidentes]

7. DICTAMEN Y APROBACIÓN DEL ÓRGANO DE GOBIERNO
   * Órgano Competente:
     [ ] Comité Técnico de Cambios - CAB Técnico (Para RFC Tipo 1)
     [ ] Consejo de Arquitectura - Architecture Board (Para RFC Tipo 2)
     [ ] Dirección Corporativa + Architecture Board (Para RFC Tipo 3 / Emisión de RAW)
   * Dictamen Oficial: [ ] APROBADO   [ ] APROBADO CON CONDICIONES   [ ] RECHAZADO   [ ] ELEVADO A RAW
   * Enmiendas Obligatorias al Repositorio de Arquitectura (EAR):
     - Modelos PlantUML a actualizar: [DIAG-XX]
     - Catálogos / Matrices modificadas: [00_1, 00_2, 00_3, 01_5]
     - Nueva Versión Semántica del EAR: [v1.Y.Z]
   * Firmas de Autorización:
     - Gerente de TI / Sponsor Ejecutivo (ACT-13): ________________________________________
     - Arquitecto Líder de Solución (ACT-12): ____________________________________________
     - Oficial de Seguridad de la Información (ACT-11): __________________________________
     - Jefatura de Operaciones Logísticas Lurín (ACT-06): ________________________________
     - Jefatura de Almacén Lurín (ACT-03 - Si aplica a inventarios/SAP): __________________
====================================================================================================
```

