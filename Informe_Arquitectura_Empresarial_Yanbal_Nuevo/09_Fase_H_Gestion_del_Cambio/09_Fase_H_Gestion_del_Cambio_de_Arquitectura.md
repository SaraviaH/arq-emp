# 9. FASE H: GESTIÓN DEL CAMBIO DE ARQUITECTURA
## Marco Metodológico, Monitoreo Continuo de Desempeño, Clasificación de RFCs, Evaluación de Impacto Multidimensional, Evolución del Repositorio EA y Activación de Nuevos Ciclos TOGAF ADM para Yanbal Perú (Unique S.A.)

> **Ubicación oficial del documento:** `09_Fase_H_Gestion_del_Cambio/09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md`  
> **Diagrama asociado:** `DIAG-11` (*Marco de Gestión Continua del Cambio de Arquitectura y Ciclo Evolutivo ADM*, disponible en código PlantUML editable en `09_Fase_H_Gestion_del_Cambio/diagramas/DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml`)  
> **Marco metodológico:** TOGAF Standard (The Open Group Architecture Framework) — Fase H: Gestión del Cambio de Arquitectura (*Architecture Change Management*)  
> **Documentos de referencia y gobernanza:** Fase Preliminar, Fase A (Visión de Arquitectura), Fase B (Arquitectura de Negocio), Fase C (Arquitectura de Sistemas de Información), Fase D (Arquitectura Tecnológica), Fase E (Oportunidades y Soluciones), Fase F (Planificación de la Migración), Fase G (Gobernanza de la Implementación), Matriz Maestra de Requerimientos, Catálogo Maestro de Actores y Sistemas, Glosario Canónico y Convenciones de Nomenclatura, y Registro Maestro de Supuestos, Incertidumbres y Decisiones de Arquitectura.  
> **Principio de gobernanza:** Todos los documentos precedentes del repositorio constituyen la línea base formal de verdad arquitectónica. La Fase H no rediseña la arquitectura objetivo ni reimplementa componentes, sino que establece los procedimientos formales, órganos colegiados, umbrales métricos y mecanismos de trazabilidad mediante los cuales Unique S.A. (Yanbal Perú) administrará los cambios continuos que afecten a la plataforma de trazabilidad de despachos B2B (MTD), garantizando su alineación estratégica a largo plazo y determinando cuándo corresponde activar una nueva iteración de TOGAF ADM.

---

## 9.1. Objetivos, propósito y alcance de la Gestión del Cambio de Arquitectura

### 9.1.1. Propósito y necesidad de la Fase H en Yanbal Perú

La culminación de la Fase G transfiere la plataforma de trazabilidad del despacho (MTD) a un régimen de operación productiva continua. Sin embargo, en un entorno empresarial dinámico como el de Yanbal Perú (Unique S.A.), caracterizado por la continua rotación o incorporación de empresas de transporte tercerizadas (`ACT-07`), la evolución de los sistemas transaccionales comerciales (`Maya` / `ZapCommerce` - `SIS-03`/`SIS-04`), el envejecimiento tecnológico de plataformas satélites y la eventual modernización del núcleo ERP (`SAP R3` - `SIS-01`), la arquitectura no puede permanecer estática.

La **Fase H de TOGAF ADM (*Architecture Change Management*)** tiene como propósito fundamental garantizar que la arquitectura empresarial conserve su valor estratégico y su integridad operativa a lo largo del tiempo. Sus objetivos centrales son:
1. **Mantener la alineación estratégica del negocio:** Verificar de forma permanente que las capacidades tecnológicas del MTD continúen respondiendo a los objetivos estratégicos de la compañía (`OBJ-01` a `OBJ-08`), asegurando que las presiones operativas cotidianas no degraden el diseño arquitectónico aprobado.
2. **Salvaguardar las restricciones arquitectónicas innegociables:** Impedir que modificaciones menores o requerimientos emergentes violen los principios rectores del proyecto:
   * Prohibición absoluta de escrituras directas sobre la base de datos de `SAP R3` (`REQ-RES-002`, `PAE-04`).
   * Delimitación estricta al ámbito B2B de distribución nacional, excluyendo la logística hacia consumidoras finales (`REQ-RES-001`, `DEC-04`).
   * Interoperabilidad con transportistas tercerizados sin exigencia de hardware telemático GPS propietario (`REQ-RES-004`, `ASM-05`).
   * Preservación del mecanismo de almacenamiento local y reintento (*Store-and-Forward*) ante contingencias de red (`REQ-RNF-001`, `ASM-16`).
3. **Controlar la evolución de los componentes y contratos:** Establecer un canal formal para canalizar, evaluar y dictaminar cualquier Solicitud de Cambio (*Request for Architecture Change - RFC*), categorizando su impacto en tres niveles rigurosos (Tipo 1, Tipo 2 y Tipo 3).
4. **Preservar la vigencia del Repositorio de Arquitectura Empresarial (EAR):** Garantizar que cualquier cambio aprobado en producción se refleje de manera inmediata y sincrónica en los modelos UML, especificaciones de interfaces, catálogos maestros y matrices de requerimientos, impidiendo la divergencia entre la documentación y la realidad operativa (*Shadow Architecture*).
5. **Identificar disparadores de re-arquitectura:** Establecer criterios objetivos para reconocer cuándo una modificación supera la capacidad adaptativa de la solución vigente y exige formalmente la apertura de una nueva iteración del ciclo ADM.

### 9.1.2. Alcance de la gestión del cambio arquitectónico

El alcance de la Fase H abarca la totalidad del ciclo de vida operativo de la solución TO-BE desplegada en producción a nivel nacional (cubriendo las rutas de Lurín hacia los 24 departamentos del Perú), supervisando las interacciones entre los componentes del Core MTD (`CMP-ING-01` a `CMP-NOT-06`), los adaptadores corporativos (`ADP-SAP`, `ADP-SPY`, `ADP-CRM`, `ADP-LOG`), las interfaces con los 10 sistemas del catálogo (`SIS-01` a `SIS-10`) y los 16 actores institucionales (`ACT-01` a `ACT-16`).

```
+----------------------------------------------------------------------------------------------------+
|                         ALCANCE OPERATIVO DE LA GESTIÓN DEL CAMBIO (FASE H)                        |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [CAPAS ARQUITECTÓNICAS BAJO SUPERVISIÓN CONTINUA]                                                 |
|  * Capa de Negocio: Procesos B2B de Lurín, SLAs de entrega departamental y gestión de excepciones. |
|  * Capa de Datos: Inmutabilidad de marcas T0/T1, esquemas CloudEvents v1.0 y diccionario canónico. |
|  * Capa de Aplicaciones: Microservicios MTD, colas asíncronas y conectores hacia sistemas legacy.  |
|  * Capa Tecnológica: Topología Kubernetes, pasarelas API Gateway, mTLS, cifrado TLS 1.3 y redes.   |
|                                                                                                    |
|  [FRONTERAS INFRANQUEABLES DE LA FASE H]                                                           |
|  * Queda estrictamente excluida cualquier solicitud que pretenda habilitar escrituras en SAP R3.   |
|  * Queda excluida la distribución secundaria de última milla hacia consultoras/consumidoras B2C.   |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

### 9.1.3. Principios rectores de la gestión del cambio arquitectónico

La Fase H se rige por cinco principios de gobernanza continua:

| ID Principio | Nombre del Principio | Declaración y Justificación Arquitectónica | Regla de Aplicación en Fase H |
| :---: | :--- | :--- | :--- |
| **`PH-01`** | **Inviolabilidad de las Restricciones Fundacionales (`PAE-04`)** | Ninguna solicitud de cambio, independientemente de su urgencia comercial u operativa, puede vulnerar las restricciones `REQ-RES-001` a `004` sin pasar por una re-evaluación estratégica formal. | Toda RFC que plantee escrituras en `SAP R3` o desvíos al ámbito B2C es vetada y rechazada de plano en la mesa de entrada. |
| **`PH-02`** | **Trazabilidad Integral y Cero Cambios Ocultos** | Todo cambio introducido en producción debe originarse en una RFC aprobada, contar con análisis de impacto documentado y reflejarse en el Repositorio de Arquitectura. | Se prohíben parches de emergencia permanentes sin RFC de regularización en un plazo máximo de 72 horas hábiles. |
| **`PH-03`** | **Decisión Basada en Evidencia y Telemetría Objetiva (`PAE-02`)** | Las decisiones de cambio, ampliación de infraestructura o calibración de SLAs deben sustentarse en datos empíricos de monitoreo continuo, no en percepciones subjetivas. | La aprobación de cambios de capacidad o escalamiento se supedita a métricas auditadas de Prometheus y logs de `CMP-AUD-04`. |
| **`PH-04`** | **Retrocompatibilidad y Protección de Ecosistema (`PAE-03`)** | Las modificaciones en las APIs y contratos de eventos B2B no deben romper la integración con los transportistas asociados (`ACT-07`), garantizando periodos de transición ordenados. | Los cambios de contrato deben mantener soporte de versiones anteriores durante al menos 6 meses mediante políticas de deprecación. |
| **`PH-05`** | **Disciplina de Versionado del Repositorio EA** | La documentación arquitectónica es el gemelo digital de la operación técnica. Un cambio en producción que no actualice el repositorio se considera formalmente inconcluso. | Ninguna RFC Tipo 2 o Tipo 3 se da por cerrada sin la publicación de la versión actualizada del Repositorio de Arquitectura. |

---

## 9.2. Modelo de monitoreo continuo de la arquitectura empresarial

### 9.2.1. Arquitectura de supervisión proactiva multinivel

Para detectar degradaciones antes de que se transformen en fallas operativas o quiebres de servicio en la cadena logística, Yanbal Perú implementa un modelo de monitoreo continuo estructurado en cuatro capas de observabilidad:

```
+----------------------------------------------------------------------------------------------------+
|                         MODELO DE MONITOREO CONTINUO DE ARQUITECTURA (FASE H)                      |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [1. CAPA DE NEGOCIO Y OPERACIONES LOGÍSTICAS]                                                     |
|  * Sensores: Conciliación SAP vs. MTD, tickets en CELLFORCE (SIS-09), volumen por departamento.   |
|  * Métricas: Lead times B2B, tasa de incidencias de transporte, quejas de clientes por desinformación. |
|                                                                                                    |
|  [2. CAPA DE APLICACIONES Y EVENTOS (CORE MTD)]                                                    |
|  * Sensores: CMP-AUD-04 (Latencia ΔT), CMP-VAL-03 (Consistencia causal), CMP-ING-01 (Duplicados).  |
|  * Métricas: Desfase de reporte (<= 30 min), eventos procesados/seg, rechazos sintácticos/semánticos. |
|                                                                                                    |
|  [3. CAPA DE INTEGRACIÓN Y ADAPTADORES CORPORATIVOS]                                              |
|  * Sensores: ADP-SAP (Lecturas staging), ADP-LOG (API Gateway B2B), colas RabbitMQ/Kafka.          |
|  * Métricas: Retardo de staging Lurín (<= 15 min), mensajes encolados, reintentos Store-and-Forward.|
|                                                                                                    |
|  [4. CAPA DE INFRAESTRUCTURA, PLATAFORMA Y SEGURIDAD]                                              |
|  * Sensores: Kubernetes HPA, WAF perimetral, escaneo de certificados TLS 1.3 / mTLS, DB Audit.    |
|  * Métricas: Uptime de endpoints (>= 99.5%), latencia p95 de APIs, intentos de intrusión bloqueados.|
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

### 9.2.2. Sensores tecnológicos e instrumentación de la observabilidad

1. **Telemetría de Latencia en Tiempo Real (`CMP-AUD-04`):**
   * El componente de auditoría temporal computa determinísticamente la diferencia $\Delta T = T_1 - T_0$ para cada evento recibido.
   * Si $\Delta T > 25\text{ minutos}$, se emite una alerta preventiva (*Warning*) a la consola operativa de `ACT-04` y `ACT-06`, anticipando un posible incumplimiento del SLA de 30 minutos (`DEC-05`).
   * Si $\Delta T > 30\text{ minutos}$, se registra un evento de quiebre de SLA en la serie temporal de Prometheus, generando un ticket de investigación operativa automática.
2. **Auditoría Continua de Aislamiento del ERP (`ACT-03` / `ACT-11`):**
   * Agentes de auditoría nativos en la base de datos de `SAP R3` monitorean las trazas de SQL.
   * Cualquier intento de ejecución de sentencias `INSERT`, `UPDATE` o `DELETE` provenientes de credenciales del MTD o IPs externas dispara una alarma crítica de severidad 1, revocando el acceso del conector y notificando de inmediato al CISO (`ACT-11`) y al Jefe de Almacén (`ACT-03`).
3. **Control Causal de Secuencia de Estados (`CMP-VAL-03`):**
   * Inspección algorítmica del grafo de estados del despacho.
   * Si un transportista remite un evento inconsistente (e.g., "Entregado" para un pedido en estado "En Preparación" en Lurín), el componente rechaza la transición, almacena el payload en la cola de excepciones (Dead Letter Queue - DLQ) y genera una notificación al Gestor de Incidencias (`CMP-NOT-06`).
4. **Supervisión de Salud de Conectividad en Ruta (`ASM-16`):**
   * Módulo de monitoreo en el API Gateway B2B que fiscaliza los reportes acumulados por el mecanismo *Store-and-Forward*.
   * Detecta si un lote diferido de eventos presenta marcas temporales $T_0$ cohérentes con la ruta o si existen anomalías de reloj en los dispositivos de captura de los transportistas asociados (`ACT-07`).

---

## 9.3. Evaluación continua del desempeño arquitectónico y beneficios de negocio

### 9.3.1. Cuadro de mando integral de indicadores de arquitectura (`KPI-01` a `KPI-08`)

La evaluación del desempeño no se efectúa de manera aislada, sino a través del seguimiento continuo de los ocho indicadores canónicos frente a los objetivos estratégicos (`OBJ-01` a `OBJ-08`) y los factores críticos de éxito (`FCE-01` a `FCE-06`):

| Código KPI | Métrica Arquitectónica Oficial | Meta de Diseño TO-BE (Compromiso Arquitectónico) | Umbral de Degradación Crítica (Disparador RFC) | Frecuencia de Medición | Rol Responsable del Análisis |
| :---: | :--- | :--- | :--- | :---: | :---: |
| **`KPI-01`** | **Latencia Media de Actualización ($\Delta T = T_1 - T_0$)** | $\le 30\text{ minutos}$ en ruta B2B (`DEC-05`) | Promedio móvil semanal $> 40\text{ min}$ | Tiempo real / Reporte Semanal | Arquitecto de Solución (`ACT-12`) |
| **`KPI-02`** | **Cumplimiento del SLA de Eventos en Ventana** | $\ge 98\%$ de eventos integrados en $\le 30\text{ min}$ | Cumplimiento mensual $< 92\%$ | Diario / Consolidado Mensual | Jefatura de Distribución (`ACT-06`) |
| **`KPI-03`** | **Integridad Causal y Orden de Estados** | $100\%$ de eventos en orden secuencial válido | $> 0.1\%$ de eventos con orden alterado | Tiempo real (alertas automáticas) | Líder de Calidad / QA (`ACT-14`) |
| **`KPI-04`** | **Cobertura de Despachos Monitoreados** | $100\%$ de pedidos B2B hacia los 24 departamentos | Cobertura $< 97\%$ en alguna región | Diario / Cierre de Despacho | Jefatura de Operaciones (`ACT-06`) |
| **`KPI-05`** | **Reducción de Incidentes en Call Center** | Reducción $\ge 40\%$ de tickets de reclamo en CRM | Reducción acumulada $< 20\%$ | Mensual | Supervisión de Call Center (`ACT-08`) |
| **`KPI-06`** | **Disponibilidad Mensual de Plataforma MTD** | $\ge 99.5\%$ Uptime de servicios y APIs | Uptime mensual $< 99.0\%$ | Continuo (Sondeos sintéticos) | Gerencia de TI / SRE (`ACT-13`) |
| **`KPI-07`** | **Tasa de Descarte por Duplicidad / Malformación** | $\le 1\%$ de eventos descartados por duplicado | Descarte sostenido $> 3\%$ | Semanal | Especialista de Integración (`ACT-12`) |
| **`KPI-08`** | **Tiempo Medio de Resolución de Fallas (MTTR)** | $MTTR \le 2\text{ horas}$ para incidencias técnicas | $MTTR > 4\text{ horas}$ en incidentes P1/P2 | Mensual | Soporte de TI y Middleware (`ACT-12`) |

> **Principio de Claridad Epistémica sobre Metas de Diseño:**  
> En cumplimiento estricto de las directrices metodológicas, se ratifica que los valores comprometidos en la tabla anterior constituyen **metas de diseño arquitectónico aprobadas para la operación**, y no mediciones empíricas históricas consumadas. Los valores basales reales continúan sujetos a las campañas periódicas de medición y calibración en producción (`ASM-03`, `ASM-13`, `DEC-05`).

### 9.3.2. Detección temprana de obsolescencia y degradación funcional

El Architecture Board realiza una **Revisión de Salud Arquitectónica Semestral**, donde se evalúan los siguientes síntomas de degradación estructural:
1. **Fatiga de Conectores Legacy:** Incremento en los tiempos de espera de extracción en las vistas de staging de `ADP-SAP` o problemas de concurrencia en `SPY` (`SIS-02`) durante picos de preparación en Lurín.
2. **Obsolescencia Tecnológica Externa:** Notificaciones de fin de soporte o cambios de API por parte de proveedores de software satélite (`NSDG` - `SIS-06`, `DRIVING` - `SIS-08`, o transportistas tercerizados `ACT-07`).
3. **Erosión de la Calidad de Datos:** Aumento en la tasa de eventos de contingencia cargados manualmente a través del portal web frente a los reportes automatizados vía API REST, lo que denota desidia operativa en los transportistas o problemas de integración en sus aplicativos móviles.

---

## 9.4. Gestión y clasificación de Solicitudes de Cambio (RFC - Request for Architecture Change)

### 9.4.1. Procedimiento formal de tramitación de Solicitudes de Cambio

Todo cambio que afecte procesos, flujos de datos, componentes de software, contratos de interfaces, reglas de negocio o plataformas de infraestructura debe canalizarse obligatoriamente mediante el flujo formal de **Solicitud de Cambio de Arquitectura (*Request for Architecture Change - RFC*)**:

```
+----------------------------------------------------------------------------------------------------+
|                         FLUJO DE VIDA DE UNA SOLICITUD DE CAMBIO (RFC)                             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [1. INICIACIÓN Y REGISTRO]                                                                        |
|  * Emisión del formato oficial con código RFC-ARCH-XXXX.                                           |
|  * Registro formal en la Secretaría Técnica de Arquitectura (ACT-12 / ACT-15).                     |
|                                                                                                    |
|                                     |                                                              |
|                                     v                                                              |
|                                                                                                    |
|  [2. TRIAJE Y CLASIFICACIÓN PRELIMINAR]                                                            |
|  * Clasificación según criticidad e impacto: TIPO 1 | TIPO 2 | TIPO 3.                             |
|  * Filtro de Violación Fundacional: Veto inmediato si afecta aislamiento SAP o alcance B2B.        |
|                                                                                                    |
|                                     |                                                              |
|                                     v                                                              |
|                                                                                                    |
|  [3. ANÁLISIS DE IMPACTO MULTIDIMENSIONAL]                                                         |
|  * Evaluación técnica en Negocio, Datos, Aplicaciones, Tecnología, Seguridad y Contratos.          |
|  * Estimación de costos, esfuerzo, riesgos y dependencias operativas.                              |
|                                                                                                    |
|                                     |                                                              |
|                                     v                                                              |
|                                                                                                    |
|  [4. DICTAMEN Y APROBACIÓN POR ÓRGANO COMPETENTE]                                                  |
|  * TIPO 1: Aprobado por Comité Técnico de Cambios (CAB Técnico).                                   |
|  * TIPO 2: Aprobado por el Consejo de Arquitectura (Architecture Board).                          |
|  * TIPO 3: Aprobado por Dirección Corporativa (ACT-13) + Architecture Board (Nuevo Ciclo ADM).     |
|                                                                                                    |
|                                     |                                                              |
|                                     v                                                              |
|                                                                                                    |
|  [5. IMPLEMENTACIÓN, HOMOLOGACIÓN Y PRUEBAS]                                                       |
|  * Despliegue en Sandbox / Staging; pruebas de regresión y seguridad (SAST/DAST).                 |
|  * Verificación de Contratos de Interfaces y cumplimiento de SLAs.                                 |
|                                                                                                    |
|                                     |                                                              |
|                                     v                                                              |
|                                                                                                    |
|  [6. SINCRONIZACIÓN DEL REPOSITORIO EA Y CIERRE]                                                   |
|  * Actualización mandatoria de modelos UML, catálogos maestros y matrices en el Repositorio.      |
|  * Emisión del Acta de Cierre de Cambio y pase formal a operación regular.                         |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

### 9.4.2. Clasificación formal tripartita de Solicitudes de Cambio

Conforme a las recomendaciones de TOGAF ADM (Fase H), las solicitudes de cambio se categorizan estrictamente en tres tipos:

```
+----------------------------------------------------------------------------------------------------+
|                         CLASIFICACIÓN FORMAL DE SOLICITUDES DE CAMBIO                              |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [RFC TIPO 1: MANTENIMIENTO OPERATIVO Y PARCHES MENORES]                                           |
|  * Criterio: Cambios internos que NO alteran contratos de arquitectura, esquemas de datos         |
|    canónicos, interfaces públicas ni modelos de seguridad.                                         |
|  * Ejemplos:                                                                                       |
|    - Actualización de parches de seguridad en contenedores Docker de microservicios MTD.           |
|    - Optimización de índices en la base de datos operacional PostgreSQL (SIS-03).                 |
|    - Reconfiguración cosmética de tableros de métricas en Grafana o ajuste de alertas en Slack.    |
|    - Refactorización interna de código en CMP-COR-02 preservando la firma de métodos e interfaces. |
|  * Autoridad de Aprobación: Comité Técnico de Cambios (ACT-12, ACT-14, ACT-15).                   |
|  * Ruta de Ejecución: Sprint regular de mantenimiento; despliegue mediante pipeline CI/CD.         |
|                                                                                                    |
|  [RFC TIPO 2: MODIFICACIÓN INCREMENTAL DE LA ARQUITECTURA]                                         |
|  * Criterio: Cambios que modifican capacidades existentes, añaden nuevos actores o alteran        |
|    contratos de interfaz de forma retrocompatible sin quebrar los principios de diseño.           |
|  * Ejemplos:                                                                                       |
|    - Homologación e incorporación de una nueva empresa de transporte tercerizada (ACT-07).        |
|    - Adición de un nuevo campo opcional retrocompatible en el esquema CloudEvents v1.0.            |
|    - Creación de un nuevo endpoint REST en el API Gateway B2B para soporte de dispositivos móviles.|
|    - Actualización de versión del middleware de colas de eventos (e.g. RabbitMQ a Kafka).          |
|    - Ajuste en los mecanismos de polling de solo lectura del adaptador ADP-SAP ante nuevas vistas.|
|  * Autoridad de Aprobación: Architecture Board pleno (ACT-13, ACT-12, ACT-11, ACT-06, ACT-03).    |
|  * Ruta de Ejecución: Emisión de enmienda a Contratos de Arquitectura (AC-PKG-XX), homologación    |
|    obligatoria en Sandbox, pase a producción y actualización de la línea base v1.x del EAR.        |
|                                                                                                    |
|  [RFC TIPO 3: CAMBIO ARQUITECTÓNICO FUNDAMENTAL (RE-ARQUITECTURA)]                                 |
|  * Criterio: Transformaciones estructurales que rompen los supuestos fundacionales del proyecto,   |
|    alteran los límites del sistema o requieren una reevaluación total del ciclo de arquitectura.  |
|  * Ejemplos:                                                                                       |
|    - Migración corporativa del ERP SAP R3 (SIS-01) hacia SAP S/4HANA Cloud.                        |
|    - Decisión estratégica de ampliar la trazabilidad hacia la última milla B2C (consultoras).     |
|    - Adquisición de flota propia de transporte con instalación de hardware telemático GPS masivo. |
|    - Sustitución completa de la arquitectura de eventos por una plataforma de datos en streaming.  |
|  * Autoridad de Aprobación: Dirección Corporativa / Sponsor Ejecutivo (ACT-13) + Architecture Board|
|  * Ruta de Ejecución: Disparo formal de una NUEVA ITERACIÓN TOGAF ADM mediante la emisión de una   |
|    nueva Solicitud de Trabajo de Arquitectura (Request for Architecture Work - RAW).               |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

### 9.4.3. Formato canónico de Solicitud de Cambio de Arquitectura (`RFC-ARCH-XXXX`)

```markdown
====================================================================================================
FORMATO OFICIAL: SOLICITUD DE CAMBIO DE ARQUITECTURA (REQUEST FOR ARCHITECTURE CHANGE)
====================================================================================================
1. DATOS DE IDENTIFICACIÓN
   * Código de Solicitud: RFC-ARCH-2026-XXXX
   * Fecha de Emisión: [AAAA-MM-DD / Semana Relativa]
   * Área / Actor Solicitante: [e.g., ACT-06 (Distribución) / ACT-07 (Transportista) / ACT-12 (TI)]
   * Responsable de la Solicitud: [Nombre, Cargo y Correo Institucional]
   * Título Descriptivo: [Breve descripción conceptual del cambio solicitado]

2. CLASIFICACIÓN DE LA SOLICITUD
   * Tipo de Cambio: [ ] TIPO 1 (Mantenimiento Operativo)
                     [ ] TIPO 2 (Modificación Incremental)
                     [ ] TIPO 3 (Cambio Fundamental / Re-arquitectura)
   * Prioridad de Negocio: [ ] Alta   [ ] Media   [ ] Baja
   * Justificación / Origen: [e.g., Degradación de KPI, nuevo transportista, requerimiento legal]

3. DESCRIPCIÓN DETALLADA DEL CAMBIO
   * Estado Actual (Línea Base Afectada): [Detalle técnico del componente/proceso actual]
   * Estado Propuesto: [Detalle del cambio arquitectónico requerido]
   * Componentes Afectados: [CMP-XX-YY / ADP-XX / SIS-XX]
   * Interfaces y Contratos Impactados: [OpenAPI / CloudEvents / JSON Schema / AC-PKG-XX]

4. EVALUACIÓN PRELIMINAR DE RESTRICCIONES VINCULANTES
   * ¿Afecta de alguna manera el aislamiento de SAP R3? [ ] SÍ (VETO AUTOMÁTICO)  [X] NO
   * ¿Pretende extender el alcance hacia distribución B2C? [ ] SÍ (VETO / TIPO 3)  [X] NO
   * ¿Impone hardware GPS propietario a transportistas?    [ ] SÍ (RECHAZO)         [X] NO
   * ¿Mantiene el mecanismo de contingencia Store-and-Forward? [X] SÍ               [ ] NO

5. ANÁLISIS DE IMPACTO MULTIDIMENSIONAL (Dictamen de Secretaría Técnica)
   * Impacto en Negocio (Procesos, Lead Times, Costos): [Evaluación cualitativa/cuantitativa]
   * Impacto en Datos y Aplicaciones (Esquemas, Idempotencia): [Evaluación técnica]
   * Impacto en Tecnología, Seguridad y SLAs (<= 30 min): [Evaluación por ACT-11 y ACT-12]
   * Estimación de Riesgos y Plan de Rollback: [Procedimiento de reversión técnica]

6. RESOLUCIÓN Y DICTAMEN DEL ÓRGANO DE GOBIERNO
   * Decisión: [ ] APROBADO   [ ] APROBADO CON CONDICIONES   [ ] RECHAZADO   [ ] ELEVADO A RAW
   * Enmiendas Requeridas al Repositorio de Arquitectura: [Modelos UML, Catálogos, Matrices]
   * Firmas de Autorización:
     - Sponsor Ejecutivo / Dirección TI (ACT-13): ________________________________
     - Arquitecto Líder de Solución (ACT-12): ____________________________________
     - Oficial de Seguridad de la Información (ACT-11): __________________________
     - Jefatura de Operaciones Logísticas Lurín (ACT-06): ________________________
     - Jefatura de Almacén Lurín (ACT-03 - Si aplica): ___________________________
====================================================================================================
```

---

## 9.5. Evaluación del impacto arquitectónico de los cambios

### 9.5.1. Matriz multidimensional de evaluación de impacto

Para evitar análisis superficiales que omitan consecuencias colaterales en la cadena de distribución, toda RFC Tipo 2 o Tipo 3 debe ser evaluada por la Secretaría Técnica de Arquitectura (`ACT-12`) a través de seis dimensiones críticas:

| Dimensión de Impacto | Criterios de Evaluación Obligatoria | Preguntas de Control Técnico | Artefactos Afectados a Inspeccionar |
| :--- | :--- | :--- | :--- |
| **1. Arquitectura de Negocio** | Afectación de procesos de picking, empaque, despacho o transporte interprovincial. | ¿Altera el flujo de zonificación de `ACT-05`? ¿Modifica las responsabilidades operativas en Lurín? ¿Afecta los acuerdos de servicio con distribuidores (`ACT-09`)? | `03_Fase_B_Arquitectura_Negocio_Completa.md`, `DIAG-03`, `DIAG-04`. |
| **2. Datos y Contratos Canónicos** | Integridad del modelo de datos, esquemas de eventos y diccionario corporativo. | ¿Requiere mutar entidades lógicas (`ENT-XX`)? ¿Introduce campos que puedan ser nulos o malformados? ¿Preserva las marcas duales $T_0/T_1$ en ISO 8601 UTC-5? | `04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md`, `JSON Schemas`. |
| **3. Arquitectura de Aplicaciones** | Acoplamiento, idempotencia y flujo de microservicios MTD. | ¿Afecta la lógica de correlación de `CMP-COR-02`? ¿Modifica la validación causal de `CMP-VAL-03`? ¿Introduce llamadas síncronas bloqueantes en el bus? | `04_Fase_C`, Diagramas de Componentes, Contratos `AC-PKG-02`. |
| **4. Arquitectura Tecnológica y Redes** | Cómputo, almacenamiento, consumo de red y topología perimetral. | ¿Exige escalar los clústeres de Kubernetes? ¿Aumenta el tráfico de red en la VPC corporativa? ¿Requiere apertura de puertos en firewalls perimetrales? | `05_Fase_D_Arquitectura_Tecnologica_Completa.md`, `DIAG-07`. |
| **5. Seguridad y Aislamiento ERP** | Cifrado, autenticación, autorización y fronteras inquebrantables. | ¿Debilita el cifrado TLS 1.3 o mTLS? ¿Modifica las políticas de tokens JWT? ¿Existe algún riesgo, directo o indirecto, de acceso de escritura en `SAP R3`? | Políticas de Seguridad, Dictamen CISO (`ACT-11`), `REQ-RES-002`. |
| **6. SLAs y Beneficios Estratégicos** | Impacto en latencia de eventos y métricas de desempeño corporativo. | ¿El cambio pone en riesgo la meta de actualización $\le 30\text{ min}$ (`KPI-01`, `DEC-05`)? ¿Altera la disponibilidad global $\ge 99.5\%$ (`KPI-06`)? | Matriz Maestra de Requerimientos (`01_5`), `KPI-01` a `KPI-08`. |

---

### 9.5.2. Auditoría especial de gobernanza sobre componentes con dependencias cruzadas

Durante la auditoría documental previa se identificaron asignaciones funcionales cruzadas entre fases precedentes respecto a los componentes `CMP-NOT-06` y `CMP-AUD-04`. Para resolver cualquier ambigüedad en la gestión de cambios, se formaliza la siguiente regla arquitectónica:

#### Regla de Gobernanza de Cambios sobre `CMP-NOT-06` (Gestión de Incidencias y Retorno de Logística Inversa):
* **Origen de la Discrepancia Documental:** En la Fase C, `CMP-NOT-06` fue concebido como el microservicio del núcleo encargado de procesar eventos de excepción y generar avisos hacia Almacén Lurín (`ACT-03`). En la Fase F, su construcción y despliegue se programó dentro de `PKG-05` (actividad `ACT-P5-01`, Semanas S17 a S19), debido a que la orquestación de retornos requería la existencia previa de la pasarela de transportistas (`PKG-04`). En la Fase G, la ficha `AC-PKG-02` lo incluyó en el catálogo de microservicios del contenedor Core.
* **Criterio de Evaluación de Impacto en Fase H:**
  1. Si una RFC propone modificar la **lógica interna de parsing, validación de causales o reglas de negocio de excepciones**, el cambio se evalúa como una modificación al Core MTD (`PKG-02`).
  2. Si una RFC propone modificar el **canal de notificación hacia Almacén Lurín (`ACT-03`), los formatos de integración con la consola de recepción o las reglas de enlace de logística inversa con los transportistas externos (`ACT-07`)**, el cambio se evalúa como una modificación a la capa de integración y telemetría (`PKG-05`).

#### Regla de Gobernanza de Cambios sobre `CMP-AUD-04` (Auditoría Temporal y Monitoreo de SLAs):
* **Origen de la Discrepancia Documental:** `CMP-AUD-04` interviene tanto en el sellado inmutable unitario de la marca $T_1$ en el momento de la ingesta (Fase C, `PKG-02`, Gate 1) como en el motor de telemetría, tableros telemáticos y alertas automáticas de SLAs en tiempo real (Fase F, `PKG-05`, Gate 3).
* **Criterio de Evaluación de Impacto en Fase H:**
  1. Cualquier RFC que intente alterar el **algoritmo de estampación de $T_1$ o la fórmula $\Delta T = T_1 - T_0$** se clasifica automáticamente como **RFC Tipo 2 de Alto Impacto**, requiriendo pruebas de no regresión y la certificación obligatoria del CISO (`ACT-11`) para garantizar la inmutabilidad de las pistas de auditoría (`REQ-RNF-003` / criterio `CRT-SEG-03`).
  2. Cualquier RFC que proponga modificar los **umbrales de visualización en Grafana, las reglas de ruteo de alertas preventivas (e.g., alertar a los 20 min en lugar de los 25 min) o los reportes analíticos agregados**, se clasifica como **RFC Tipo 1**, ejecutándose de forma ágil sin perturbar el microservicio transaccional.

---

## 9.6. Gobierno, aprobación y ejecución de cambios arquitectónicos

### 9.6.1. Órganos de decisión y autoridades colegiadas

El gobierno del cambio se sustenta en dos instancias institucionales complementarias:
1. **Comité Técnico de Cambios (CAB Técnico):**
   * **Integrantes:** Arquitecto Líder (`ACT-12`), Líder de Aseguramiento de Calidad / QA (`ACT-14`) y Gestor de Proyecto / Scrum Master (`ACT-15`).
   * **Atribuciones:** Evaluar, autorizar y calendarizar RFCs Tipo 1; supervisar la ejecución de pruebas unitarias y de integración en pipelines de CI/CD; y remitir informes mensuales de cambios operativos al Architecture Board.
2. **Consejo de Arquitectura (Architecture Board de Yanbal Perú):**
   * **Integrantes:** Gerente de TI / Sponsor (`ACT-13`, Presidente), Arquitecto Líder (`ACT-12`, Secretario Técnico), Oficial de Seguridad de la Información / CISO (`ACT-11`), Jefatura de Operaciones Logísticas Lurín (`ACT-06`) y Jefatura de Almacén Lurín (`ACT-03`).
   * **Atribuciones:** Dictamen exclusivo y vinculante sobre RFCs Tipo 2; emisión de enmiendas formales a Contratos de Arquitectura (`AC-PKG-01` a `AC-PKG-05`); evaluación de justificaciones estratégicas para elevar propuestas a RFC Tipo 3; y salvaguarda final de las restricciones corporativas.

#### Reglas de Veto Técnico Innegociable:
* **Veto de Integridad del ERP:** El representante del Área de Almacén Lurín (`ACT-03`) posee facultad de veto unilateral y absoluto sobre cualquier cambio que pretenda introducir escrituras, triggers o dependencias directas sobre la base de datos de `SAP R3` (`REQ-RES-002`, `PAE-04`).
* **Veto de Ciberseguridad y Perímetro:** El Oficial de Seguridad (`ACT-11`) posee facultad de veto unilateral sobre cualquier cambio que debilite el cifrado de comunicaciones (TLS 1.3 / mTLS), altere los mecanismos de autenticación JWT o exponga interfaces internas a redes públicas sin protección WAF.

---

### 9.6.2. Matriz RACI Integral para la Gestión del Cambio (Fase H)

A continuación se detalla la asignación de responsabilidades entre los 16 actores canónicos de la arquitectura (`ACT-01` a `ACT-16`) frente a los procesos del ciclo de cambio arquitectónico:

| Proceso / Actividad de la Fase H | ACT-01 (Manuf) | ACT-02 (Envas) | ACT-03 (Alm) | ACT-04 (Pick) | ACT-05 (Desp) | ACT-06 (Dist) | ACT-07 (Trans) | ACT-08 (Call) | ACT-09 (Dest) | ACT-10 (Recep) | ACT-11 (CISO) | ACT-12 (Arq) | ACT-13 (Dir TI) | ACT-14 (QA) | ACT-15 (PMO) | ACT-16 (TI Ext) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1. Detección y Registro de Necesidad de Cambio (RFC)** | I | I | C | C | C | **R** | C | C | I | I | C | **R** | I | C | **R** | C |
| **2. Triaje y Clasificación Preliminar (Tipo 1 / 2 / 3)** | I | I | C | I | I | C | I | I | I | I | C | **R** | I | C | **A** | I |
| **3. Análisis de Impacto en Aislamiento ERP (SAP R3)** | I | I | **A** | I | I | C | I | I | I | I | C | **R** | I | C | I | I |
| **4. Análisis de Impacto en Seguridad y Cifrado TLS 1.3** | I | I | I | I | I | I | C | I | I | I | **A** | **R** | I | C | I | C |
| **5. Aprobación de RFC Tipo 1 (CAB Técnico)** | I | I | I | I | I | I | I | I | I | I | I | **A** | I | **R** | **R** | I |
| **6. Dictamen y Aprobación de RFC Tipo 2 (Architecture Board)** | I | I | **A** | I | I | **A** | C | I | I | I | **A** | **R** | **A** | C | C | C |
| **7. Homologación de Transportista en Sandbox B2B** | I | I | I | I | I | C | **R** | I | I | I | C | C | I | **R** | C | **R** |
| **8. Evaluación y Elevación de RFC Tipo 3 (Re-arquitectura)** | C | I | C | I | I | C | C | C | C | I | C | **R** | **A** | I | C | C |
| **9. Actualización del Repositorio de Arquitectura (EAR)** | I | I | I | I | I | I | I | I | I | I | I | **R** | **A** | C | I | I |
| **10. Activación de Nueva Iteración TOGAF ADM (RAW)** | I | I | C | I | I | C | I | I | I | I | C | **R** | **A** | I | C | I |

---

## 9.7. Mantenimiento y evolución del repositorio de arquitectura

### 9.7.1. Estructura y sincronización del Repositorio de Arquitectura Empresarial (EAR)

El **Repositorio de Arquitectura Empresarial (*Enterprise Architecture Repository - EAR*)** de Yanbal Perú constituye el inventario único de la verdad técnica y de negocio. Para impedir que el repositorio quede desactualizado tras la implementación de cambios, se instaura un protocolo estricto de sincronización mandatoria:

```
+----------------------------------------------------------------------------------------------------+
|                   SINCRONIZACIÓN DEL REPOSITORIO DE ARQUITECTURA EMPRESARIAL (EAR)                 |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ARTEFACTOS DE GOBERNANZA MAESTRA]                                                                |
|  * 00_1: Glosario Canónico, prefijos de eventos y convención de nombres.                           |
|  * 00_2: Catálogo Maestro de Actores (ACT-01..16), Sistemas (SIS-01..10) y Procesos.               |
|  * 00_3: Registro Maestro de Supuestos (ASM-01..16), Decisiones (DEC-01..07) y Riesgos.           |
|                                                                                                    |
|  [ARTEFACTOS DE ESPECIFICACIÓN Y REQUERIMIENTOS]                                                   |
|  * 01_5: Matriz Maestra de Requerimientos (REQ-01 a REQ-24) y matrices de trazabilidad cruzada.   |
|  * Esquemas Técnicos: Contratos OpenAPI 3.0, JSON Schema y eventos CloudEvents v1.0.               |
|                                                                                                    |
|  [MODELOS Y DIAGRAMAS TEXTUALES PLANTUML]                                                          |
|  * DIAG-01 a DIAG-10: Modelos de Procesos, Componentes, Despliegue, Roadmap y Gobernanza.          |
|  * DIAG-11: Modelo de Gestión del Cambio y Ciclo Evolutivo ADM.                                    |
|                                                                                                    |
|  [CONTRATOS DE ARQUITECTURA Y AUDITORÍA]                                                           |
|  * AC-PKG-01 a AC-PKG-05: Fichas formales de contratos y adendas de enmienda aprobadas.            |
|  * Bitácora de RFCs: Historial auditable de solicitudes, análisis de impacto y actas de cierre.   |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

### 9.7.2. Política de versionado de la Arquitectura Empresarial

Para gestionar el historial de cambios con rigor, la arquitectura de Yanbal Perú adopta el esquema de versionado semántico arquitectónico `vX.Y.Z`:
* **Versión Mayor (`vX.0.0`):** Se incrementa exclusivamente tras la culminación de una **RFC Tipo 3**, correspondiente a la ejecución de un nuevo ciclo completo TOGAF ADM (e.g., paso de la arquitectura `v1.0.0` a `v2.0.0` tras la migración de `SAP R3` a SAP S/4HANA o la incorporación de última milla B2C).
* **Versión Menor (`v1.Y.0`):** Se incrementa tras la aprobación y despliegue exitoso de una **RFC Tipo 2** (e.g., paso a `v1.1.0` al homologar dos nuevos socios logísticos `ACT-07` o al añadir un endpoint REST retrocompatible). Requiere actualización obligatoria de diagramas PlantUML y contratos `AC-PKG-XX`.
* **Versión de Parche (`v1.1.Z`):** Se incrementa tras la resolución de una **RFC Tipo 1** (e.g., paso a `v1.1.1` tras la optimización de un índice de base de datos o corrección de una alerta en Grafana). Se registra en la bitácora técnica sin necesidad de redibujar modelos conceptuales.

---

## 9.8. Gestión de riesgos, comunicación y transición de cambios

### 9.8.1. Matriz de riesgos operacionales del cambio arquitectónico

La introducción de modificaciones en un sistema logístico interprovincial en vivo conlleva riesgos operacionales significativos, los cuales se administran mediante la siguiente matriz preventiva:

| Código Riesgo | Descripción del Riesgo Operacional | Causa Técnica Potencial | Impacto en la Cadena Logística | Medida Preventiva y Plan de Mitigación |
| :---: | :--- | :--- | :--- | :--- |
| **`RSK-CHG-01`** | **Ruptura de Interoperabilidad con Transportistas (`ACT-07`)** | Modificación no coordinada de campos en esquemas CloudEvents o cabeceras HTTP. | Pérdida de visibilidad de eventos en ruta; eventos derivados masivamente a la cola DLQ. | Pruebas de contrato automatizadas (*Contract Testing* con Pact); periodo de deprecación de 6 meses; ambiente Sandbox permanente. |
| **`RSK-CHG-02`** | **Contención de Recursos en Base de Datos de Lurín** | Ajuste ineficiente en consultas de extracción de `ADP-SAP` sobre tablas de staging. | Degradación del tiempo de respuesta durante el proceso de picking en el Centro de Distribución. | Pruebas obligatorias de carga en réplica de Staging; indexación de tablas intermedias; veto técnico de `ACT-03`. |
| **`RSK-CHG-03`** | **Fatiga Operativa y Rechazo de Nuevos Transportistas** | Exigencias técnicas desmedidas o interfaces móviles complejas durante el onboarding. | Transportistas omiten el reporte en ruta o acumulan eventos sin sincronizar oportunamente. | Disponibilidad del Portal Web de Contingencia para carga simplificada; capacitación operativa conducida por `ACT-06`. |
| **`RSK-CHG-04`** | **Divergencia entre Documentación y Producción (*Shadow Architecture*)** | Aplicación de parches de emergencia no regularizados mediante RFC formal. | Pérdida de control del diseño; diagnósticos fallidos en futuras incidencias de trazabilidad. | Auditorías semestrales de configuración de clúster contra repositorio Git (GitOps con ArgoCD); congelamiento de accesos directos. |

### 9.8.2. Plan de comunicación y gestión del cambio organizacional

Ningún cambio técnico debe sorprender a los operadores de la cadena. La PMO (`ACT-15`) y la Jefatura de Distribución (`ACT-06`) conducen el plan de comunicación institucional:
1. **Comunicaciones Externas a Empresas de Transporte (`ACT-07` / `ACT-16`):** Notificación formal mediante boletines técnicos con 30 días calendario de anticipación ante cualquier actualización menor de APIs, detallando especificaciones, URLs del Sandbox y fecha límite de adecuación.
2. **Comunicaciones Internas a Operadores de Lurín (`ACT-03`, `ACT-04`, `ACT-05`):** Talleres breves de socialización ante cualquier modificación en pantallas de picking, lectores RF o procedimientos de despacho, garantizando cero perturbación de la jornada productiva.
3. **Coordinación con Servicio al Cliente (`ACT-08`):** Actualización de manuales de consulta en `CELLFORCE` (`SIS-09`) ante nuevas tipologías de eventos o cambios en las estimaciones de promesa de entrega.

---

## 9.9. Activación de nuevos ciclos de TOGAF ADM y cierre de la Fase H

### 9.9.1. Disparadores objetivos para el reinicio del ciclo TOGAF ADM

En concordancia con las directrices de TOGAF ADM, la Fase H no concluye el ciclo de arquitectura de forma definitiva, sino que actúa como el centinela vigilante que determina cuándo la organización ha acumulado suficientes cambios como para exigir una nueva iteración arquitectónica completa.

Se establecen formalmente **cuatro disparadores estratégicos y operacionales** que obligan al Architecture Board a emitir una nueva **Solicitud de Trabajo de Arquitectura (*Request for Architecture Work - RAW*)** y reabrir el ciclo ADM desde la Fase Preliminar / Fase A:

```
+----------------------------------------------------------------------------------------------------+
|                         DISPARADORES DE REINICIO DEL CICLO TOGAF ADM                               |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [DISPARADOR 1: MIGRACIÓN ESTRUCTURAL DEL ERP CORPORATIVO]                                         |
|  * Condición: Yanbal inicia el proyecto de reemplazo de SAP R3 (SIS-01) por SAP S/4HANA.           |
|  * Justificación: Implica rediseñar por completo el conector ADP-SAP, redefinir el modelo de      |
|    datos transaccional de inventarios y reevaluar la frontera soberana de Almacén Lurín (ACT-03).  |
|                                                                                                    |
|  [DISPARADOR 2: EXPANSIÓN DEL ALCANCE A DISTRIBUCIÓN B2C / ÚLTIMA MILLA]                           |
|  * Condición: La Dirección aprueba extender la trazabilidad hacia consultoras y consumidoras finales.|
|  * Justificación: Rompe la restricción REQ-RES-001 y la decisión DEC-04; multiplica el volumen de |
|    eventos por un factor > 50x; requiere aplicaciones móviles masivas y geolocalización continua. |
|                                                                                                    |
|  [DISPARADOR 3: CAMBIO DE ESTRATEGIA LOGÍSTICA A FLOTA PROPIA CON IOT/GPS]                         |
|  * Condición: Unique S.A. adquiere flota de transporte propia para distribución departamental.    |
|  * Justificación: Supera el supuesto ASM-05 y la restricción REQ-RES-004; exige diseñar una       |
|    arquitectura de telemetría IoT de streaming en tiempo real con hardware a bordo de vehículos.   |
|                                                                                                    |
|  [DISPARADOR 4: QUIEBRE ESTRUCTURAL Y SOSTENIDO DEL SLA OBJETIVO DE 30 MINUTOS]                    |
|  * Condición: El cumplimiento de KPI-02 cae por debajo del 90% durante tres meses consecutivos     |
|    debido a la inviabilidad operativa de captura en ruta por parte de terceros (RSK-02).           |
|  * Justificación: Evidencia que el modelo arquitectónico de eventos discretos asíncronos ha sido    |
|    superado por la realidad de las carreteras nacionales, exigiendo un nuevo enfoque de solución.  |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

### 9.9.2. Protocolo formal de emisión del Request for Architecture Work (RAW)

Al confirmarse cualquiera de los disparadores precedentes, el Gerente de TI (`ACT-13`) y el Arquitecto Líder (`ACT-12`) redactan el documento **RAW-202X-001**, el cual define:
* Patrocinador ejecutivo y presupuesto asignado para la nueva iniciativa.
* Alcance preliminar del nuevo ciclo de transformación.
* Lecciones aprendidas acumuladas durante la vigencia de la arquitectura previa.
* Directiva formal para convocar a la Fase Preliminar y Fase A del nuevo ciclo ADM.

### 9.9.3. Síntesis acumulativa del ciclo inicial TOGAF ADM para Yanbal Perú

Con el desarrollo exhaustivo de la Fase H, culmina de manera formal y rigurosa el ciclo inicial completo del método **TOGAF ADM (Fase Preliminar y Fases A a H)** para el proyecto de mejora arquitectónica de la trazabilidad y visibilidad de estados en el proceso de despacho B2B de Unique S.A. (Yanbal Perú).

El proyecto entrega a la organización una arquitectura empresarial robusta, desacoplada, fundamentada en estándares abiertos de la industria y gobernada por métricas objetivas de negocio, sentando las bases para una cadena logística ágil, confiable y plenamente auditable en todo el territorio peruano.

---

## 9.10. Referencias bibliográficas (Normas APA 7.ª edición)

* Bass, L., Clements, P., & Kazman, R. (2021). *Software architecture in practice* (4.ª ed.). Addison-Wesley Professional.
* Cloud Native Computing Foundation. (2022). *CloudEvents specification v1.0.2*. Linux Foundation. https://cloudevents.io/
* Fowler, M. (2018). *Patterns of enterprise application architecture*. Addison-Wesley.
* International Organization for Standardization. (2018). *Systems and software engineering — Architecture description* (ISO/IEC/IEEE Standard N.° 42010:2018). ISO.
* Newman, S. (2021). *Building microservices: Designing fine-grained systems* (2.ª ed.). O'Reilly Media.
* OpenTelemetry Authors. (2023). *OpenTelemetry: High-quality, ubiquitous, and portable telemetry to enable effective observability*. Cloud Native Computing Foundation. https://opentelemetry.io/
* Project Management Institute. (2021). *A guide to the project management body of knowledge (PMBOK guide)* (7.ª ed.). Project Management Institute.
* Richardson, L., & Ruby, S. (2007). *RESTful web services*. O'Reilly Media.
* The Open Group. (2022). *The TOGAF® Standard, 10th Edition: Architecture Development Method (ADM)*. The Open Group Standard (G220). https://pubs.opengroup.org/architecture/togaf10-adm/
* The Open Group. (2022). *The TOGAF® Standard, 10th Edition: Architecture Change Management (Phase H)*. The Open Group Guide (G223).

