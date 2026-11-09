# PROPUESTA DE REDACCIÓN Y ESTRUCTURA NORMALIZADA
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Plan de Mejora Arquitectónica para la Trazabilidad y Visibilidad de Estados en el Proceso de Despacho B2B

---

## 1. Criterios Metodológicos de la Estructura Simplificada

El presente documento constituye la entrega actualizada de **REV_02: Propuesta de Redacción y Estructura Normalizada**, desarrollada para establecer la organización definitiva de los capítulos del proyecto bajo una política editorial de **dos niveles de numeración formal (`X.` y `X.Y.`)**, reservando el tercer nivel (`X.Y.Z.`) para desarrollos técnicos independientes de gran relevancia.

### 1.1. Reglas de Desanidamiento y Síntesis
1. **Dos Niveles Numerados por Defecto:** Los capítulos se dividen exclusivamente en apartados de segundo nivel (`X.Y.`).
2. **Absorción de Micro-secciones en Subtítulos en Negrita:** Todo subtema, criterio, condición o tarea operativa que anteriormente poseía numeración `X.Y.Z.` o `X.Y.Z.W.` se transforma en un **subtítulo en negrita dentro del flujo del texto** (ej. `- **Consolidación en Centro de Distribución:** desarrollo conceptual...`).
3. **Preservación Íntegra de Contenidos Técnicos:** La simplificación de títulos no suprime requerimientos (`REQ-*`), decisiones (`DEC-*`), brechas (`GAP-*`), contratos (`AC-PKG-*`) ni parámetros operacionales.
4. **Integración Natural de Figuras:** Los diagramas UML no abren apartados numerados; se ubican como figuras integradas en el apartado que desarrolla su contexto.

---

## 2. Estructura Normalizada Capítulo por Capítulo (Fases Preliminar a H)

A continuación se detalla la estructura normalizada de dos niveles para cada fase del informe principal:

### 2.1. Capítulo 2: Fase Preliminar — Marco de Arquitectura y Principios
- **2.1. Alcance Organizacional, Geográfico y Frontera B2B:** Delimitación de las operaciones en la Planta de Lurín y cobertura de despacho hacia distribuidoras y destinos empresariales en los 24 departamentos (`DEC-04`, `REQ-RES-001`). Veto formal a la distribución minorista B2C.
- **2.2. Principios Rectores de Arquitectura Empresarial:** Presentación estructurada de los seis principios corporativos (`PRN-01` a `PRN-06`), destacando el principio de aislamiento estricto de los sistemas core (`PRN-02`, `REQ-RES-002`, `DEC-01`).
- **2.3. Marco de Gobernanza Preliminar y Requerimientos de Alto Nivel:** Modelo de gobierno inicial, definición del repositorio de arquitectura y articulación de la Matriz Maestra de 24 Requerimientos Canónicos (referenciada formalmente a `ANX-01`).

### 2.2. Capítulo 3: Fase A — Visión de Arquitectura
- **3.1. Evaluación de Partes Interesadas y Matriz de Preocupaciones:** Análisis de los actores clave (`ACT-01` a `ACT-16`) y sus expectativas de negocio, servicio y control.
- **3.2. Diagnóstico del Problema de Despacho y Trazabilidad:** Identificación de la fragmentación de información entre SAP R3, el BWMS SPY y los operadores de transporte tercerizados.
- **3.3. Objetivos del Negocio, Factores Críticos y Rendimiento:** Mapeo formal de los 8 objetivos estratégicos (`OBJ-01..08`), 6 factores de éxito (`FCE-01..06`) y 8 indicadores clave (`KPI-01..08`).
- **3.4. Visión de la Capacidad MTD (Monitor de Trazabilidad del Despacho):** Definición de la capacidad arquitectónica propuesta (`DEC-02`) como plataforma orientada a eventos desacoplada.  
  *(Aquí se integra la **Figura 3.1: Diagrama Conceptual de la Visión de la Solución Propuesta (DIAG-01)**, acompañada de su contextualización y análisis de flujos).*
- **3.5. Evaluación de Riesgos Iniciales y Factores de Éxito:** Registro de los riesgos de contexto `RSK-01` a `RSK-06` y criterios de viabilidad del proyecto.

### 2.3. Capítulo 4: Fase B — Arquitectura de Negocio
- **4.1. Modelo Funcional de Casos de Uso del Negocio B2B:** Delimitación de las interacciones entre los actores del Centro de Distribución de Lurín, los transportistas nacionales y las Consultoras.  
  *(Aquí se integra la **Figura 4.1: Casos de Uso del Negocio para Despacho y Trazabilidad B2B (DIAG-02)**, con la especificación de `CUN-01` a `CUN-06`).*
- **4.2. Proceso Operativo Actual (Línea Base AS-IS):** Flujo de preparación, despacho y transporte multimodal sin telemetría continua.  
  - **Subproceso de Picking y Cajas:** Apertura de cajas master UA y empaque en los 8 tipos de caja (`ACT-04`).  
  - **Subproceso de Zonificación:** Clasificación hacia 24 departamentos (`ACT-05`).  
  - **Transporte y Puntos de Quiebre:** Desfase observado de $\sim 2\text{ horas}$ por reportes por lotes (`ASM-03`).  
  *(Aquí se integra la **Figura 4.2: Flujo de Actividades del Proceso de Despacho Actual AS-IS (DIAG-03)**).*
- **4.3. Proceso Operativo Propuesto (Arquitectura TO-BE):** Flujo optimizado con captura reactiva de eventos y notificación oportuna.  
  - **Captura en Origen ($T_0$):** Emisión de marcas temporales en SPY y despacho.  
  - **Transporte y Resiliencia:** Sincronización offline *Store-and-Forward* ante pérdida de red en ruta (`ASM-16`, `RSK-06`).  
  - **Notificación Temprana ($T_1$):** Alerta proactiva a la Consultora con latencia $\le 30\text{ minutos}$ (`DEC-03`, `DEC-05`).  
  *(Aquí se integra la **Figura 4.3: Flujo de Actividades del Proceso de Despacho Propuesto TO-BE (DIAG-04)**).*
- **4.4. Comparativa Operacional y Análisis de Brechas del Negocio:** Evaluación de rendimiento AS-IS vs. TO-BE y matriz de brechas del negocio (`GAP-01` a `GAP-08`).

### 2.4. Capítulo 5: Fase C — Arquitectura de Sistemas de Información
- **5.1. Arquitectura de Datos y Entidades Canónicas de Despacho:** Modelo canónico de datos compuesto por las 10 entidades analíticas (`ENT-01` a `ENT-10`), definiciones de campos maestros, estados y ciclo de vida de la información.
- **5.2. Arquitectura de Aplicaciones y Plataforma MTD:** Catálogo de microservicios canónicos (`CMP-ING-01` Ingesta, `CMP-COR-02` Motor, `CMP-VAL-03` Validación, `CMP-AUD-04` Telemetría $\Delta T$, `CMP-PUB-05` Exposición y `CMP-NOT-06` Notificaciones) y capa de adaptadores (`ADP-SAP`, `ADP-SPY`, `ADP-NSDG`, `ADP-LOG`, `ADP-CRM`).
- **5.3. Interacción Dinámica y Cronológica (AS-IS vs. TO-BE):** Análisis de latencia temporal en la línea base frente a la coreografía asíncrona de eventos.  
  *(Aquí se integran la **Figura 5.1: Secuencia e Interacción Temporal en la Línea Base AS-IS (DIAG-05)** y la **Figura 5.2: Secuencia Reactiva de Eventos y Notificación TO-BE (DIAG-06)**).*
- **5.4. Análisis de Brechas de Sistemas de Información:** Cobertura de requerimientos de integración (`REQ-INT-001..004`) y datos.

### 2.5. Capítulo 6: Fase D — Arquitectura Tecnológica
- **6.1. Principios de Infraestructura y Topología Híbrida:** Definición del entorno híbrido que articula los servidores on-premise de Lurín con la plataforma de mensajería e integración corporativa sobre el Bus `SIS-05`.
- **6.2. Especificación de Despliegue Físico y Nodos de Ejecución:** Detalle de los nodos On-Premise (`SAP R3`, `SPY`, `NODE-MSG-BUS`, `NODE-APP-MTD`, `DB-EVENTSTORE`), DMZ perimetral (API Gateway, Portal Contingencia) y terminales móviles en ruta.  
  *(Aquí se integra la **Figura 6.1: Despliegue de la Arquitectura Tecnológica Objetivo TO-BE (DIAG-07)**).*
- **6.3. Dimensionamiento, Disponibilidad y Seguridad de Comunicaciones:** Requerimientos de alta disponibilidad (99.5%), cifrado perimetral (TLS 1.3, mTLS) y protección de datos conforme a ley (`REQ-RNF-001..004`).

### 2.6. Capítulo 7: Fase E — Oportunidades y Soluciones
- **7.1. Consolidación de Brechas de Arquitectura (GAP-01 a GAP-08):** Matriz integral de brechas y criterios de resolución mediante alternativas de ingeniería.
- **7.2. Definición Canónica de Paquetes de Trabajo (PKG-01 a PKG-05):**  
  - **PKG-01:** Gobernanza de Datos y Contratos de Integración (`INT-DAT-01..08`, JSON Schema).  
  - **PKG-02:** Plataforma Core MTD y Microservicios de Eventos (`CMP-ING-01..NOT-06`, `DB-EVENTSTORE`).  
  - **PKG-03:** Adaptadores de Integración de Sistemas Internos (`ADP-SAP`, `ADP-SPY`, `ADP-CRM`).  
  - **PKG-04:** Onboarding y Pasarela de Operadores Logísticos (`ADP-LOG`, Portal Web B2B).  
  - **PKG-05:** Monitoreo Operacional, Telemetría y Dashboards (OpenTelemetry, Prometheus/Grafana).
- **7.3. Arquitectura de Solución Integrada y Mapeo Modular:** Asignación contractual de componentes a paquetes de trabajo y relaciones de interfaz.  
  *(Aquí se integra la **Figura 7.1: Arquitectura de Solución y Mapa de Paquetes de Trabajo (DIAG-08)**).*
- **7.4. Criterios de Coexistencia e Interoperabilidad:** Mecanismos de compatibilidad durante la migración progresiva.

### 2.7. Capítulo 8: Fase F — Planificación de la Migración
- **7.1. Estrategia y Enfoque de Transición Gradual:** Enfoque por etapas evitando migraciones disruptivas ("Big Bang") mediante dos arquitecturas de transición (AT-1 y AT-2) y tres puertas de calidad (`DEC-06`).
- **7.2. Definición de Arquitecturas de Transición e Hitos (Horizonte S1 a S24):**  
  - **Transición AT-1 (Semanas S1 a S12):** Habilitación del núcleo interno MTD, adaptadores internos (`ADP-SAP`, `ADP-SPY`) y certificación en **Gate 1 (Semana S12)**.  
  - **Transición AT-2 (Semanas S11 a S18):** Onboarding de transportistas (`ADP-LOG`), piloto interprovincial, *Store-and-Forward* y certificación en **Gate 2 (Semana S18)**.  
  - **Arquitectura Objetivo TO-BE (Semanas S19 a S24):** Despliegue nacional a 24 departamentos, telemetría continua y certificación final en **Gate 3 (Semana S24)**.  
  *(Aquí se integra la **Figura 8.1: Roadmap Estratégico de Implementación y Arquitecturas de Transición (DIAG-09)**).*
- **7.3. Priorización Multicriterio de Proyectos y Dependencias:** Matriz de prelación técnica y ruta crítica del cronograma.
- **7.4. Presupuesto Referencial, Recursos y Retorno de Inversión:** Asignación de equipos de trabajo y análisis de costo-beneficio operacional.
- **7.5. Plan de Gestión y Mitigación de Riesgos de Migración:** Controles preventivos y planes de contingencia para `RSK-07` a `RSK-10`.

### 2.8. Capítulo 9: Fase G — Gobernanza de la Implementación
- **8.1. Marco Organizacional y Órganos de Control:** Composición y facultades vinculantes del Architecture Board (`ACT-11..13`) y la Oficina de Aseguramiento PMO/QA (`ACT-14..15`).
- **8.2. Proceso de Puertas de Calidad Arquitectónica (Quality Gates 1, 2 y 3):** Criterios rigurosos de entrada, artefactos obligatorios de salida y listas de chequeo para **Gate 1 (S12)**, **Gate 2 (S18)** y **Gate 3 (S24)**.
- **8.3. Contratos de Arquitectura y Procedimiento de Excepciones:** Especificación de los cinco contratos vinculantes (**`AC-PKG-01` a `AC-PKG-05`**) y circuito formal de dispensas arquitectónicas (`WVR-*`).  
  *(Aquí se integra la **Figura 9.1: Marco de Gobernanza de la Implementación, Contratos y Conformidad (DIAG-10)**).*
- **8.4. Auditoría de Conformidad y Aceptación de Entregables:** Protocolos de certificación técnica para pase a producción.

### 2.9. Capítulo 10: Fase H — Gestión del Cambio de Arquitectura
- **9.1. Marco de Monitoreo Continuo y Sensores de Arquitectura:** Detección en vivo de desviaciones del SLA ($\le 30\text{ min}$) mediante la sonda `CMP-AUD-04` e inspección de cero escrituras en SAP R3.
- **9.2. Taxonomía de Solicitudes de Cambio (RFC) y Umbrales Operativos:** Clasificación de cambios en **Tipo 1 (Menor / ITIL)**, **Tipo 2 (Mayor / Fases E-F-G)** y **Tipo 3 (Estratégico / Reinicio ADM)**.
- **9.3. Ciclo de Vida del Cambio y Disparadores ADM:** Circuito de evaluación, aprobación del Architecture Board y activación de bucles del ciclo TOGAF ADM.  
  *(Aquí se integra la **Figura 10.1: Marco de Gestión Continua del Cambio y Ciclo Evolutivo ADM (DIAG-11)**).*
- **9.4. Mantenimiento del Repositorio de Arquitectura y Control de Riesgos:** Actualización continua bajo disciplina GitOps y mitigación de los riesgos `RSK-CHG-01` a `RSK-CHG-04`.

---

## 3. Modelos de Redacción Académica Depurada

### 3.1. Ejemplo de Redacción en Apartado Operativo (Fase B)
> **4.2. Proceso Operativo Actual (Línea Base AS-IS)**  
> En el Centro de Distribución de Lurín, la operación de despacho de pedidos para la venta directa B2B inicia formalmente cuando las órdenes facturadas descienden desde las plataformas comerciales hacia el sistema BWMS in-house `SPY (SIS-02)` (`Estado 1: Pedido facturado`).  
> 
> - **Consolidación y Armado de Cajas:** El personal operativo (`ACT-04`) realiza la apertura de las cajas master mono SKU selladas con Código UA provenientes de Almacén (`ACT-03`, `SIS-01 SAP R3`), recolecta unitariamente los artículos según la guía de picking de SPY y los empaca en uno de los 8 formatos de caja disponibles, asociando el Código UA al Número de Pedido (`Estado 2: Elaboración o en preparación`).  
> - **Zonificación y Despacho:** Los bultos cerrados se trasladan a la Zona de Despacho (`ACT-05`), donde se clasifican hacia los 24 departamentos del territorio nacional y se entregan a los transportistas externos (`ACT-07`), formalizando el `Estado 3: Pedido despachado`.  
> - **Transporte y Desfase Histórico:** Los socios logísticos ejecutan el transporte multimodal (terrestre, bimodal o aéreo). Debido a la dependencia de archivos planos y reportes por lotes (Batch ETL) consolidados al cierre de turno, se genera un desfase de seguimiento de aproximadamente dos horas ($\sim 120\text{ minutos}$) antes de que los eventos de ruta (`Estado 4: Pedido en ruta`) o entrega (`Estado 5: Pedido entregado`) se reflejen en los sistemas de consulta (`SIS-09 CELLFORCE`), provocando incertidumbre y reclamos recurrentes de las Consultoras (`[TO 20:18–21:35]`, `ASM-03`).

### 3.2. Ejemplo de Redacción en Apartado de Gobierno (Fase G)
> **8.3. Contratos de Arquitectura y Procedimiento de Excepciones**  
> Para asegurar que la construcción de la plataforma MTD respete los principios de diseño y los requerimientos aprobados, el Architecture Board emite cinco Contratos de Arquitectura oficiales (**`AC-PKG-01` a `AC-PKG-05`**), vinculando formalmente a los líderes de proyecto con las especificaciones de interfaz, seguridad y desempeño.  
> 
> - **Obligatoriedad Contractual:** Ningún paquete de trabajo podrá considerarse apto para producción sin haber demostrado conformidad estricta frente a los criterios del contrato correspondiente.  
> - **Gestión de Dispensas (Architecture Waivers):** Ante contingencias de desarrollo que impidan el cumplimiento temporal de una cláusula contractual, el equipo técnico deberá tramitar una solicitud de dispensa formal (`WVR-*`). El Architecture Board evaluará el impacto técnico y la deuda arquitectónica, autorizando dispensas temporales únicamente por plazos no mayores a 6 semanas. Bajo ninguna circunstancia se admitirán dispensas que vulneren el aislamiento de `SAP R3` (`REQ-RES-002`) o invadan la distribución minorista B2C (`REQ-RES-001`, `DEC-04`).
