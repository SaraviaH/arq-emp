# Plan de Mejora Arquitectónica para la Trazabilidad y Visibilidad de Estados en el Proceso de Despacho B2B de Yanbal Perú

**Organización Analizada:** Unique S.A. (Yanbal Perú)  
**Marco Metodológico:** TOGAF ADM e Ingeniería de Requerimientos ISO/IEC/IEEE 29148  
**Alcance del Proyecto:** Cadena logística de preparación, despacho y seguimiento de entregas B2B desde el Centro de Distribución de Lurín (Lima) hacia distribuidoras y sedes empresariales en los 24 departamentos del Perú.

---

## Contexto Organizacional y Marco de Arquitectura

### Descripción General y Problemática Operativa

Unique S.A. (Yanbal Perú) opera un modelo de venta directa multinivel respaldado por una cadena de suministro nacional que abastece a los **24 departamentos del Perú**, desde su complejo industrial y almacén de productos terminados en **Lurín (Lima)** —con 15,000 m² y capacidad estática para 2 millones de cajas master mono SKU— bajo promesas de entrega de **24 horas en Lima Metropolitana** y **hasta 7 días en provincias**.

Actualmente, el seguimiento de los pedidos una vez que salen hacia las distribuidoras y destinos empresariales B2B presenta un **desfase de actualización de estados de hasta 2 horas**. Este retardo origina un margen de desconocimiento operativo que impide al Área de Distribución (`ACT-06`) conocer la ubicación y etapa real de la carga, dificulta la atención oportuna de consultas y reclamos en Servicio al Cliente (`ACT-08`) a través de `CELLFORCE` (`SIS-09`), y retrasa la coordinación ante incidencias o retornos por logística inversa hacia el Almacén de Lurín (`ACT-03`).

### Principios de Arquitectura Empresarial

Los lineamientos que gobiernan el diseño de la solución se sintetizan en la siguiente tabla:

| Código | Principio de Arquitectura | Declaración Resumida | Implicación Principal en el Diseño |
| :--- | :--- | :--- | :--- |
| **`PAE-01`** | Interoperabilidad Desacoplada | Los sistemas deben comunicarse mediante eventos asíncronos sobre el Bus Corporativo (`SIS-05`). | Elimina integraciones punto a punto rígidas entre sistemas internos y de terceros. |
| **`PAE-02`** | Preservación de Sistemas Consolidados | Se reutilizan los sistemas actuales sin duplicar funciones transaccionales ni imponer reemplazos. | El mecanismo propuesto actúa como capa de integración mediante adaptadores (`ADP-XX`). |
| **`PAE-03`** | Estandarización Semántica Canónica | Todo evento de despacho debe homologarse a un catálogo único de 5 estados secuenciales. | Traduce los códigos heterogéneos de transportistas antes de su publicación. |
| **`PAE-04`** | Soberanía e Integridad del ERP | `SAP R3` (`SIS-01`) es operado exclusivamente por el Área de Almacén (`ACT-03`). | Se prohíbe cualquier interfaz de escritura directa desde sistemas externos hacia `SAP R3`. |
| **`PAE-05`** | Trazabilidad Temporal Auditable | Todo evento separa la marca de ocurrencia física (`T₀`) de la marca de ingesta central (`T₁`). | Permite medir objetivamente la latencia `ΔT = T₁ - T₀ ≤ 30 minutos`. |
| **`PAE-06`** | Seguridad desde el Diseño | Toda interfaz externa requiere cifrado robusto, autenticación mutua y control de acceso por roles. | Validación obligatoria por Seguridad Informática (`ACT-11`) y protección de datos personales. |
| **`PAE-07`** | Continuidad y Ergonomía Operativa | La solución no debe introducir doble digitación ni degradar el rendimiento del picking y despacho. | Captura transparente de eventos a partir de las transacciones operativas existentes. |

### Actores y Sistemas Involucrados

El ecosistema del proceso de distribución integra actores internos y externos junto con diez sistemas de información (tres sistemas internos adaptados, cuatro sistemas satélites de proveedores y herramientas operativas complementarias):

| Código | Actor / Sistema | Rol o Función Principal en el Proceso de Despacho |
| :--- | :--- | :--- |
| **`ACT-03`** | Área de Almacén PT (Lurín) | Custodia cajas master mono SKU (`Código UA`), opera en exclusividad `SAP R3` y evalúa retornos físicos. |
| **`ACT-04`** | Centro de Distribución (Picking/Packing) | Ejecuta el surtido unitario en `SPY`, asigna formato de caja (1 a 8) y prepara el pedido comercial. |
| **`ACT-05`** | Zona de Despacho | Consolida bultos preparados, zonifica envíos hacia los 24 departamentos y entrega carga al transportista. |
| **`ACT-06`** | Área de Distribución | Supervisa el seguimiento nacional de despachos, audita SLAs de transporte y coordina excepciones. |
| **`ACT-07`** | Socios Logísticos (Transportistas) | Ejecutan el transporte tercerizado (terrestre, bimodal o aéreo) y reportan eventos de ruta y entrega. |
| **`ACT-08`** | Área de Servicio al Cliente | Atiende consultas, reclamos y seguimiento de promesas de entrega mediante `CELLFORCE`. |
| **`ACT-09`** | Destinatarios Empresariales B2B | Distribuidoras y sedes empresariales en el Perú que reciben los pedidos despachados. |
| **`ACT-11` / `12`** | Seguridad Informática / Arquitectura TI | Gobiernan la seguridad perimetral, los contratos de integración y la arquitectura sobre el Bus Corporativo. |
| **`SIS-01`** | `SAP R3` (ERP Corporativo) | Gestión de inventarios de productos terminados (`libre disposición`, `calidad`, `bloqueado`) y facturación. |
| **`SIS-02`** | `SPY` (Sistema de Picking In-house) | Gestión de líneas de picking, volumetría teórica, pesaje y asignación de los 8 formatos de caja. |
| **`SIS-03` / `04`** | `Maya` y `ZapCommerce` | Plataformas comerciales de captura, validación y procesamiento transaccional de pedidos facturados. |
| **`SIS-05`** | `Bus de Integración Corporativo` | Middleware empresarial que canaliza el intercambio de órdenes y eventos entre los sistemas. |
| **`SIS-06` / `07`** | `NSDG` y `ENSDY` | Sistemas de gestión de salida de transporte (`NSDG`) y consulta de seguimiento de pedidos (`ENSDY`). |
| **`SIS-08`** | `DRIVING` | Plataforma externa empleada por proveedores logísticos para administración de rutas y entregas. |
| **`SIS-09` / `10`** | `CELLFORCE` y `Terminales RF` | Sistema CRM de atención de reclamos (`CELLFORCE`) y lectores de radiofrecuencia de almacén (`SIS-10`). |

### Requerimientos de Arquitectura (Agrupación Funcional y No Funcional)

Los 24 requerimientos del proyecto se estructuran en dos bloques principales (**Requerimientos Funcionales y de Negocio — RF** y **Requerimientos No Funcionales y Restricciones — RNF**):

| Bloque Macro | Categorías Incluidas | Códigos Canónicos | Cantidad |
| :--- | :--- | :--- | :---: |
| **Bloque I: Requerimientos Funcionales y de Negocio (`RF`)** | Funcionales (`REQ-FUN`), Integración (`REQ-INT`) y Negocio (`REQ-NEG`) | `REQ-FUN-001..005`, `REQ-INT-001..004`, `REQ-NEG-001..004` | **13** |
| **Bloque II: Requerimientos No Funcionales y Restricciones (`RNF`)** | Calidad (`REQ-RNF`), Arquitectónicos (`REQ-ARQ`) y Restricciones (`REQ-RES`) | `REQ-RNF-001..004`, `REQ-ARQ-001..003`, `REQ-RES-001..004` | **11** |

#### Síntesis de Requerimientos Funcionales, de Negocio y de Integración (`RF`)

| ID | Categoría | Descripción Resumida del Requerimiento | Prioridad | Actor / Criterio Principal |
| :--- | :--- | :--- | :---: | :--- |
| **`REQ-NEG-001`** | Negocio | Seguimiento integral de entregas desde Lima hasta distribuidoras y sedes B2B en los 24 departamentos. | Alta | `ACT-06` / Visibilidad de los 5 estados del ciclo. |
| **`REQ-NEG-002`** | Negocio | Reducción del desfase de actualización de estados desde hasta 2 horas a un máximo de 30 minutos. | Alta | `ACT-06` / Latencia `ΔT = T₁ - T₀ ≤ 30 min`. |
| **`REQ-NEG-003`** | Negocio | Disponibilidad de estados actualizados en `CELLFORCE` para atención de consultas y reclamos. | Alta | `ACT-08` / Consulta actualizada en `≤ 30 min`. |
| **`REQ-NEG-004`** | Negocio | Trazabilidad de incidencias en ruta y del flujo de retorno por logística inversa hacia Lurín. | Media | `ACT-06`, `ACT-03` / Registro de causa y retorno. |
| **`REQ-FUN-001`** | Funcional | Registro y transición secuencial de los 5 estados: *Facturado*, *En preparación*, *Despachado*, *En ruta*, *Entregado*. | Alta | `ACT-06` / Secuencia lógica sin saltos de estado. |
| **`REQ-FUN-002`** | Funcional | Consulta de trazabilidad por `Número de Pedido` y cliente incluyendo fecha de promesa de entrega. | Alta | `ACT-08`, `ACT-09` / Respuesta `< 500 ms`. |
| **`REQ-FUN-003`** | Funcional | Registro condicional de identificación de `Persona Autorizada` (`ACT-10`) en la recepción B2B. | Media | `ACT-07` / Campo opcional y nullable en entrega. |
| **`REQ-FUN-004`** | Funcional | Asociación del pedido con su formato de caja (1 a 8), destino geográfico y socio logístico asignado. | Media | `ACT-04`, `ACT-05` / Vínculo bulto-pedido-ruta. |
| **`REQ-FUN-005`** | Funcional | Registro de incidencias de transporte y notificación asíncrona de retorno hacia Almacén (`ACT-03`). | Media | `ACT-03` / Aviso previo sin escritura en `SAP R3`. |
| **`REQ-INT-001`** | Integración | Sincronización de stock liberado `SAP R3`–`SPY` y canalización de órdenes hacia `NSDG` vía Bus. | Alta | `ACT-12` / Flujo desacoplado sobre `SIS-05`. |
| **`REQ-INT-002`** | Integración | Intercambio oportuno de eventos de despacho, ruta y entrega con sistemas de socios logísticos (`ACT-07`). | Alta | `ACT-07`, `ACT-12` / API/Webhook estándar. |
| **`REQ-INT-003`** | Integración | Propagación de estados consolidados vía `Bus de Integración` hacia `ENSDY`, `CELLFORCE` y `Maya`. | Alta | `ACT-12` / Suscripción asíncrona Pub/Sub. |
| **`REQ-INT-004`** | Integración | Coexistencia e integración con los 3 sistemas internos y 4 sistemas de proveedores mediante adaptadores. | Media | `ACT-12` / Capas Anti-Corrupción (`ADP-XX`). |

#### Síntesis de Requerimientos No Funcionales, Arquitectónicos y Restricciones (`RNF`)

| ID | Categoría | Descripción Resumida del Requerimiento | Prioridad | Actor / Criterio Principal |
| :--- | :--- | :--- | :---: | :--- |
| **`REQ-RNF-001`** | Calidad (SLA) | Latencia máxima permisible de 30 minutos en la actualización de seguimiento e ingesta offline en ruta. | Alta | `ACT-06` / `ΔT ≤ 30 min` y *Store-and-Forward*. |
| **`REQ-RNF-002`** | Calidad | Capacidad de procesamiento asíncrono para soportar volúmenes operativos y picos de campaña nacional. | Alta | `ACT-12` / Escalabilidad horizontal sin bloqueo. |
| **`REQ-RNF-003`** | Seguridad | Validación por Seguridad Informática (`ACT-11`), cifrado TLS 1.3/mTLS y control de acceso por roles (RBAC). | Alta | `ACT-11` / Cumplimiento de Ley N.° 29733. |
| **`REQ-RNF-004`** | Usabilidad | Preservación de la facilidad de uso en interfaces operativas sin exigir doble digitación manual. | Media | `ACT-04`, `ACT-05` / Cero pasos manuales extra. |
| **`REQ-ARQ-001`** | Arquitectura | Diseño del Mecanismo de Trazabilidad del Despacho (`MTD`) como capacidad integrada sin duplicar sistemas. | Alta | `ACT-12` / Servicios modulares sobre `SIS-05`. |
| **`REQ-ARQ-002`** | Arquitectura | Trazabilidad lógica multidimensional entre `Código UA` (almacén), picking (`SPY`) y `Número de Pedido`. | Alta | `ACT-12` / Correlación asociativa `M:N`. |
| **`REQ-ARQ-003`** | Arquitectura | Contrato canónico de eventos con marcas temporales duales (`T₀` ocurrencia y `T₁` ingesta) y hash SHA-256. | Alta | `ACT-12` / Inmutabilidad y deduplicación. |
| **`REQ-RES-001`** | Restricción | Alcance circunscrito exclusivamente al despacho B2B hacia distribuidoras y sedes en el Perú. | Alta | `ACT-06` / Excluye reparto secundario B2C. |
| **`REQ-RES-002`** | Restricción | Operación exclusiva de `SAP R3` por Almacén Lurín (`ACT-03`); prohibición de escritura directa externa. | Alta | `ACT-03` / Solo lectura y notificación asíncrona. |
| **`REQ-RES-003`** | Restricción | Tránsito primario desde Lurín hacia el CD únicamente en cajas master mono SKU selladas sin saldos. | Media | `ACT-03`, `ACT-04` / Apertura exclusiva en picking. |
| **`REQ-RES-004`** | Restricción | Transporte ejecutado por socios externos (`ACT-07`); exclusión de flota propia y de GPS propietario. | Alta | `ACT-06` / Integración mediante software/APIs. |

---

## Visión de la Arquitectura

### Declaración de la Visión de la Arquitectura y Objetivos Estratégicos

- **Declaración de la Visión de la Arquitectura (TO-BE):** Consolidar una capacidad arquitectónica distribuida, segura y orientada a eventos —el **Mecanismo de Trazabilidad del Despacho (`MTD`)**— articulada sobre el `Bus de Integración` corporativo (`SIS-05`), que capture, valide semánticamente, correlacione (desde el `Código UA` hasta el `Número de Pedido`) y propague los **5 estados canónicos del despacho B2B** con un desfase máximo de **30 minutos (`ΔT ≤ 30 min`)** hacia los 24 departamentos del Perú, coexistiendo con los sistemas actuales y notificando los retornos a Almacén Lurín (`ACT-03`) sin realizar escrituras directas en `SAP R3`.

| Código | Objetivo Estratégico SMART | Indicador Vinculado (KPI) | Meta Objetivo TO-BE |
| :--- | :--- | :--- | :--- |
| **`OBJ-01`** | Medir y auditar objetivamente el desfase entre la ocurrencia física (`T₀`) y el registro en sistema (`T₁`). | `KPI-01`: Latencia Media (`ΔT`) | `ΔT = T₁ - T₀ ≤ 30 minutos` |
| **`OBJ-02`** | Reducir la ventana de actualización de estados en ruta desde hasta 2 horas a máximo 30 minutos. | `KPI-02`: Cumplimiento SLA 30 min | `≥ 98%` de eventos en ventana (Piloto `≥ 95%`) |
| **`OBJ-03`** | Garantizar la secuencia lógica de los 5 estados del pedido y su correlación con cajas `UA` y picking. | `KPI-03`: Integridad Secuencial | `100%` de transiciones válidas sin saltos |
| **`OBJ-04`** | Integrar los 3 sistemas propios y 4 de proveedores a través del Bus Corporativo sin doble digitación. | `KPI-04`: Cobertura B2B Nacional | `100%` de despachos trazados en 24 departamentos |
| **`OBJ-05`** | Proveer visibilidad actualizada en `CELLFORCE` y portales B2B para reducir reclamos por incertidumbre. | `KPI-05`: Reducción de Reclamos | Reducción `≥ 40%` en llamadas de seguimiento |
| **`OBJ-06`** | Trazar incidencias de transporte y notificar oportunamente los retornos hacia Almacén Lurín (`ACT-03`). | `KPI-06`: Disponibilidad MTD | Disponibilidad `≥ 99.5%` y `100%` retornos avisados |
| **`OBJ-07`** | Cumplir los controles de Seguridad Informática (`ACT-11`) y segregación de accesos por nivel de rol. | `KPI-07`: Tasa de Duplicados/Error | `≤ 1%` de descartes por idempotencia |
| **`OBJ-08`** | Soportar los volúmenes y picos de campaña nacional manteniendo la resiliencia operativa. | `KPI-08`: Tiempo de Recuperación | `MTTR ≤ 2 horas` ante contingencias |

### Matriz de Interesados (Stakeholders)

| Actor | Rol en el Proyecto | Interés Principal | Influencia | Preocupación Clave |
| :--- | :--- | :--- | :---: | :--- |
| **`ACT-13` / `ACT-06`** | Dirección TI y Jefatura Distribución | Eliminar el margen de desconocimiento de 2 horas en ruta nacional. | Alta | Cumplimiento del SLA `≤ 30 min` y retorno de inversión. |
| **`ACT-03`** | Jefatura de Almacén PT (Lurín) | Mantener el control exclusivo de `SAP R3` y recibir alertas previas de devoluciones. | Alta | Que ningún sistema externo modifique inventarios en `SAP R3`. |
| **`ACT-04` / `ACT-05`** | Operadores de Picking y Despacho | Emitir eventos de preparación y salida sin fricción ni doble digitación. | Media | Velocidad operativa en línea de picking y empaque. |
| **`ACT-07`** | Socios Logísticos Externos | Reportar estados de viaje y entrega sin requerir hardware propietario costoso. | Media | Intermitencia de señal celular en rutas interprovinciales. |
| **`ACT-08` / `ACT-09`** | Servicio al Cliente y Destinatarias B2B | Consultar estados fidedignos y fechas de promesa de entrega actualizadas. | Media | Certeza en la atención de reclamos y recepción oportuna. |
| **`ACT-11` / `ACT-12`** | Seguridad Informática y Arquitectura TI | Garantizar ciberseguridad perimetral, desacoplamiento y gobernabilidad. | Alta | Exposición de APIs a terceros y protección de datos (Ley 29733). |

### Diagramas de la Visión de la Solución Propuesta (`DIAG-01` y Derivados)

El **Diagrama General `DIAG-01`** presenta la visión conceptual extremo a extremo de cómo el Mecanismo de Trazabilidad del Despacho (`MTD`) captura los eventos desde la preparación en Lurín y los socios logísticos, ejecuta sus seis responsabilidades canónicas (`MTD-R01` a `MTD-R06`) y propaga los estados a través del `Bus de Integración` (`SIS-05`) sin escribir en `SAP R3`.

![DIAG-01: Diagrama Conceptual de la Visión de la Solución Propuesta (MTD)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-01-concepto-solucion-to-be.png)

Para facilitar su lectura detallada, el diagrama general se descompone en dos vistas complementarias:
- **`DIAG-01.1` (Vista de Contexto e Interoperabilidad E2E):** Muestra las fronteras de generación en Lurín (`S1`), captura en el `MTD` (`S2`), publicación en el Bus (`S3`) y consumo por los actores (`S4`).
- **`DIAG-01.2` (Vista de Responsabilidades Internas `MTD-R01..06`):** Detalla el flujo interno de captura, homologación de los 5 estados, correlación `UA ↔ Pedido`, cálculo `ΔT ≤ 30 min`, publicación y notificación de retorno.

![DIAG-01.1: Visión de Contexto e Interoperabilidad E2E de la Capacidad Propuesta MTD](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-01.1-estructura-compuesta-contexto-e2e-mtd.png)

![DIAG-01.2: Descomposición Funcional Interna de Responsabilidades Canónicas del MTD](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-01.2-estructura-compuesta-responsabilidades-mtd-core.png)

---

## Arquitectura de Negocio

### Capacidades y Comparativa de Casos de Uso de Negocio (AS-IS vs. TO-BE)

La arquitectura de negocio articula seis casos de uso de negocio (`CUN-01` a `CUN-06`). La siguiente tabla contrasta explícitamente qué existía en la situación actual (**AS-IS**) y qué capacidades se mejoran o incorporan en el modelo objetivo (**TO-BE**):

| Código | Caso de Uso de Negocio | Situación Actual (AS-IS) | Mejora o Capacidad Incorporada en el TO-BE |
| :--- | :--- | :--- | :--- |
| **`CUN-01`** | Recepcionar PT y Controlar Stock en Almacén (`SAP R3` / `RF`) | **Existente:** Lectura RF de `Código UA` y control de stock en `SAP R3` por `ACT-03`. | **Conservado e Integrado:** Se mantiene intacto en `SAP R3` y provee metadatos de lectura para correlacionar lotes y cajas `UA`. |
| **`CUN-02`** | Preparar Pedido, Picking y Empaque (`SPY`) | **Parcial:** Asigna cajas (1 a 8) en `SPY`, pero sin emitir marca `T₀` estandarizada al exterior. | **Mejorado:** Emite el evento canónico `En preparación` con marca `T₀` y registra la correlación asociativa `Código UA ↔ Pedido`. |
| **`CUN-03`** | Zonificar Despacho y Asignar Transporte (`NSDG`) | **Con retraso:** Generación de hojas de ruta con sincronización diferida hacia seguimiento. | **Mejorado:** Publica inmediatamente el evento `Pedido despachado` (`T₀`) al asignar el manifiesto al socio logístico (`ACT-07`). |
| **`CUN-04`** | Ejecutar Transporte y Registrar Eventos en Ruta y Entrega | **Deficiente:** Reportes batch con desfase de hasta 2 horas y pérdida de datos sin señal celular. | **Rediseñado (Nuevo Flujo MTD):** Captura eventos `En ruta` y `Entregado` con `T₀`/`T₁`, buffer offline *Store-and-Forward* y `ΔT ≤ 30 min`. |
| **`CUN-05`** | Consultar Trazabilidad del Despacho y Atender Reclamos | **Reactivo:** Consultas en `CELLFORCE` y `ENSDY` con estados desactualizados hasta por 2 horas. | **Nuevo / Automatizado:** Propagación Pub/Sub en tiempo casi real hacia `CELLFORCE`, `ENSDY` y `Maya` con secuencia validada. |
| **`CUN-06`** | Gestionar Incidencias en Ruta y Logística Inversa | **Informal:** Devoluciones físicas a Lurín sin alerta estructurada previa en el sistema. | **Nuevo / Orquestado:** Registro de incidencia, apertura de retorno inverso y notificación asíncrona a `ACT-03` sin tocar `SAP R3`. |

### Diagramas de Casos de Uso de Negocio (`DIAG-02` y Derivados)

El **Diagrama General `DIAG-02`** representa los seis casos de uso de negocio (`CUN-01` a `CUN-06`), sus relaciones `<<include>>` y `<<extend>>`, y su interacción con los actores operativos y sistemas del ecosistema.

![DIAG-02: Diagrama General de Casos de Uso del Negocio (CUN-01 a CUN-06)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-02-casos-uso-negocio.png)

Para una visualización clara por frente funcional, se divide en dos diagramas derivados:
- **`DIAG-02.1` (Flujo Operativo Principal `CUN-01` a `CUN-04`):** Cubre desde la custodia de `Código UA` en almacén, preparación en `SPY` y despacho en `NSDG`, hasta el transporte y entrega B2B en los 24 departamentos.
- **`DIAG-02.2` (Consulta, Reclamos e Incidencias `CUN-05` y `CUN-06`):** Detalla la consulta de seguimiento para Servicio al Cliente (`ACT-08`) y destinatarias (`ACT-09`), así como la gestión de incidencias y retorno inverso hacia Lurín (`ACT-03`).

![DIAG-02.1: Casos de Uso del Flujo Operativo (CUN-01 a CUN-04)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-02.1-casos-uso-flujo-operativo-cun01-cun04.png)

![DIAG-02.2: Casos de Uso de Consulta, Reclamos e Incidencias (CUN-05 y CUN-06)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-02.2-casos-uso-consulta-incidencias-cun05-cun06.png)

---

### Modelado del Proceso Actual AS-IS (`DIAG-03` y Derivados)

El **Diagrama General `DIAG-03`** modela el flujo de actividades actual (**AS-IS**), evidenciando el punto exacto donde se produce la ruptura de visibilidad y el desfase de hasta 2 horas durante el tránsito tercerizado, así como el impacto en las consultas de `CELLFORCE` y en los retornos no anunciados.

![DIAG-03: Diagrama General de Actividades del Proceso Actual (AS-IS)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-03-actividades-as-is.png)

A continuación se presentan sus tres vistas detalladas por etapa del proceso:
- **`DIAG-03.1` (AS-IS Parte 1 — Abastecimiento, Picking/Packing y Salida de Despacho):** Cubre los Estados 1 a 3 dentro de las instalaciones de Lurín (`SAP R3`, `SPY` y `NSDG`).
- **`DIAG-03.2` (AS-IS Parte 2 — Transporte, Entrega B2B y Desfase de hasta 2 horas):** Muestra los Estados 4 y 5 en ruta nacional y la desactualización en `ENSDY` y `CELLFORCE`.
- **`DIAG-03.3` (AS-IS Parte 3 — Incidencias en Ruta y Logística Inversa):** Representa el manejo reactivo de siniestros o entregas fallidas y su retorno físico a Lurín.

![DIAG-03.1: Actividades AS-IS (Parte 1) - Abastecimiento, Picking y Despacho](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-03.1-actividades-as-is-abastecimiento-picking-despacho.png)

![DIAG-03.2: Actividades AS-IS (Parte 2) - Transporte, Entrega y Desfase de 2 horas](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-03.2-actividades-as-is-transporte-entrega-desfase-2h.png)

![DIAG-03.3: Actividades AS-IS (Parte 3) - Incidencias y Logística Inversa](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-03.3-actividades-as-is-incidencias-logistica-inversa.png)

---

### Diseño del Proceso Optimizado TO-BE (`DIAG-04` y Derivados)

El **Diagrama General `DIAG-04`** modela el proceso de negocio optimizado (**TO-BE**), incorporando el carril del Mecanismo de Trazabilidad del Despacho (`MTD`) que captura marcas `T₀`, valida la secuencia de los 5 estados, mide `ΔT = T₁ - T₀ ≤ 30 min` y notifica de forma anticipada los retornos a Almacén Lurín sin intervenir `SAP R3`.

![DIAG-04: Diagrama General de Actividades del Proceso Optimizado (TO-BE)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-04-actividades-to-be.png)

Sus tres diagramas derivados permiten inspeccionar cada segmento del proceso TO-BE con total nitidez:
- **`DIAG-04.1` (TO-BE Parte 1 — Preparación Trazable y Despacho con Homologación MTD):** Captura estandarizada de los Estados 1, 2 y 3 y correlación `Código UA ↔ Pedido`.
- **`DIAG-04.2` (TO-BE Parte 2 — Transporte Multimodal, Entrega B2B y Propagación en `≤ 30 min`):** Captura de Estados 4 y 5 (incluyendo soporte offline *Store-and-Forward*) y actualización inmediata para `CELLFORCE`, `ENSDY` y `ACT-09`.
- **`DIAG-04.3` (TO-BE Parte 3 — Incidencias en Ruta y Notificación Previa de Retorno `MTD-R06`):** Flujo estructurado de excepción en ruta y aviso asíncrono hacia `ACT-03` para su recepción y ajuste soberano en `SAP R3`.

![DIAG-04.1: Actividades TO-BE (Parte 1) - Preparación y Despacho con MTD](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-04.1-actividades-to-be-preparacion-despacho-mtd.png)

![DIAG-04.2: Actividades TO-BE (Parte 2) - Transporte, Entrega y SLA <= 30 min](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-04.2-actividades-to-be-transporte-entrega-sla-30min.png)

![DIAG-04.3: Actividades TO-BE (Parte 3) - Incidencias y Notificación de Retorno](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-04.3-actividades-to-be-incidencias-retorno-mtd-r06.png)

---

### Diagramas de Secuencia del Proceso Central (`DIAG-05` AS-IS y `DIAG-06` TO-BE)

#### Secuencia del Proceso Actual AS-IS (`DIAG-05` y Derivados)

El **Diagrama General `DIAG-05`** muestra el intercambio cronológico de mensajes en la operación actual, destacando la latencia de transmisión entre `ACT-07` / `DRIVING` y los sistemas `ENSDY` y `CELLFORCE`.

![DIAG-05: Diagrama General de Secuencia del Proceso Actual (AS-IS)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-05-secuencia-as-is.png)

Sus tres vistas derivadas desglosan la interacción por bloque:

![DIAG-05.1: Secuencia AS-IS (Parte 1) - Abastecimiento, Picking y Salida](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-05.1-secuencia-as-is-abastecimiento-picking-despacho.png)

![DIAG-05.2: Secuencia AS-IS (Parte 2) - Transporte, Desfase de 2h y Consulta](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-05.2-secuencia-as-is-transporte-desfase-2h-consulta.png)

![DIAG-05.3: Secuencia AS-IS (Parte 3) - Incidencias y Logística Inversa](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-05.3-secuencia-as-is-incidencias-logistica-inversa.png)

#### Secuencia del Proceso Optimizado TO-BE (`DIAG-06` y Derivados)

El **Diagrama General `DIAG-06`** modela la secuencia de mensajes en la arquitectura objetivo (**TO-BE**), donde cada evento operativo viaja con su marca temporal `T₀` hacia el `MTD`, se sella con `T₁`, se valida y se distribuye asíncronamente por el `Bus de Integración` (`SIS-05`) dentro del umbral de 30 minutos.

![DIAG-06: Diagrama General de Secuencia del Proceso Optimizado (TO-BE)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-06-secuencia-to-be.png)

Sus tres vistas derivadas presentan el detalle de cada tramo de interacción:

![DIAG-06.1: Secuencia TO-BE (Parte 1) - Preparación y Despacho vía Bus y MTD](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-06.1-secuencia-to-be-preparacion-despacho-mtd.png)

![DIAG-06.2: Secuencia TO-BE (Parte 2) - Tránsito, Entrega B2B y Consulta en <= 30 min](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-06.2-secuencia-to-be-transito-entrega-sla-30min.png)

![DIAG-06.3: Secuencia TO-BE (Parte 3) - Incidencia en Ruta y Notificación de Retorno](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-06.3-secuencia-to-be-incidencias-retorno-mtd-r06.png)

### Servicios de Negocio y Brechas Principales (Fase B)

| Código | Servicio de Negocio / Brecha | Descripción Concisa | Impacto / Meta |
| :--- | :--- | :--- | :--- |
| **`SNB-01` / `02`** | Abastecimiento Trazable y Picking (`CAP-01`, `CAP-02`) | Vincula cajas `Código UA` con líneas de picking y emite el Estado 2 (`En preparación`) con `T₀`. | Cierra `GAP-04` (correlación `UA ↔ Pedido`). |
| **`SNB-03`** | Despacho Nacional Zonificado (`CAP-03`) | Registra asignación de bultos a manifiesto de carga y emite el Estado 3 (`Pedido despachado`). | Cierra `GAP-03` (secuencia homologada). |
| **`SNB-04`** | Seguimiento de Transporte B2B (`CAP-04`, `CAP-05`) | Captura eventos `En ruta` (Estado 4) y `Entregado` (Estado 5) con marcas `T₀`/`T₁` y soporte offline. | Cierra `GAP-01` y `GAP-02` (`ΔT ≤ 30 min`). |
| **`SNB-05`** | Consulta Unificada de Trazabilidad (`CAP-05`) | Expone estados validados y promesas de entrega a `CELLFORCE`, `ENSDY` y destinatarias (`ACT-09`). | Cierra `GAP-05` y `GAP-06` (visibilidad B2B). |
| **`SNB-06`** | Gestión de Excepciones y Retorno (`CAP-06`) | Registra incidencias en ruta y envía notificación previa de retorno hacia Almacén Lurín (`ACT-03`). | Cierra `GAP-07` (respetando `REQ-RES-002`). |

---

## Arquitectura de Sistemas de Información

### Arquitectura de Datos

#### Entidades Lógicas del Dominio de Distribución (`ENT-01` a `ENT-10`)

El modelo de datos estructura diez entidades que resuelven dos retos arquitectónicos centrales:
1. **Correlación Muchos a Muchos (`M:N`) entre `Código UA` y `Número de Pedido` (`REQ-ARQ-002`):** Se resuelve mediante la entidad asociativa **`ENT-04: Linea_Picking_Trazable`**, que vincula cada caja master mono SKU (`ENT-03`) abierta en el CD con los pedidos comerciales multi SKU (`ENT-01`).
2. **Registro Inmutable de Eventos y Auditoría de SLA (`REQ-ARQ-003`):** La entidad **`ENT-07: Evento_Trazabilidad_Canonica`** opera como un log *Append-Only* que registra los 5 estados canónicos, las marcas `T₀` y `T₁`, la latencia calculada `ΔT = T₁ - T₀` y la firma hash `SHA-256` de idempotencia.

| Código | Entidad Lógica | Clave Primaria (`PK`) | Claves Foráneas (`FK`) / Restricciones Clave | Sistema de Registro (SoR) |
| :--- | :--- | :--- | :--- | :--- |
| **`ENT-01`** | `Pedido_Comercial` | `Numero_Pedido` (Varchar 20) | `Estado_Actual` (Enum 5 Estados, `NOT NULL`) | `Maya` / `ZapCommerce` |
| **`ENT-02`** | `Linea_Detalle_Pedido` | `ID_Linea` (Varchar 25) | `FK`: `Numero_Pedido` (`NOT NULL`), `Codigo_SKU` | `ZapCommerce` (`SIS-04`) |
| **`ENT-03`** | `Unidad_Almacenamiento_UA` | `Codigo_UA` (Varchar 30, `UNIQUE`) | `Estado_Stock` (*Disposición*, *Calidad*, *Bloqueado*) | `SAP R3` (`SIS-01`) |
| **`ENT-04`** | `Linea_Picking_Trazable` | `ID_Tarea_Picking` (UUID v4) | `FK`: `Numero_Pedido`, `FK`: `Codigo_UA_Origen` (`M:N`) | `SPY` (`SIS-02`) |
| **`ENT-05`** | `Bulto_Despacho_Caja` | `Codigo_Bulto_Tracking` (`UNIQUE`) | `FK`: `Numero_Pedido`, `FK`: `Numero_Manifiesto`, Caja `1..8` | `SPY` / Despacho |
| **`ENT-06`** | `Hoja_Ruta_Despacho` | `Numero_Manifiesto` (Varchar 25) | `Codigo_Socio_Logistico`, `Modalidad_Transporte` | `NSDG` / `DRIVING` |
| **`ENT-07`** | `Evento_Trazabilidad_Canonica`| `ID_Evento_UUID` (UUID v4) | `FK`: `Numero_Pedido`, `FK`: `Codigo_Bulto`, `Firma_SHA256` (`UNIQUE`) | `MTD` (`SIS-05`) |
| **`ENT-08`** | `Incidencia_Transporte` | `ID_Incidencia` (Varchar 25) | `FK`: `ID_Evento_UUID` (`UNIQUE`), `Tipo_Incidencia` | `DRIVING` / `MTD` |
| **`ENT-09`** | `Registro_Retorno_Inverso` | `ID_Retorno_Inverso` (Varchar 25) | `FK`: `ID_Incidencia_Origen` (`UNIQUE`), `Dictamen_Calidad` | `MTD` / Almacén `ACT-03` |
| **`ENT-10`** | `Receptor_Autorizado_Destino` | `ID_Receptor_Autorizado` | `FK`: `ID_Evento_UUID` (`NULL` condicional), `DNI` | `MTD` / App Transporte |

#### Diagramas del Modelo Lógico de Datos Corporativo (`DIAG-C1` y Derivados)

El **Diagrama General `DIAG-C1`** presenta el modelo entidad-relación completo (`ENT-01` a `ENT-10`) con sus tipos de datos, llaves primarias (`PK`), llaves foráneas (`FK`), restricciones de unicidad (`UNIQUE`) y nullabilidad (`NULL` / `NOT NULL`).

![DIAG-C1: Modelo Lógico de Datos Corporativo Completo (ENT-01 a ENT-10)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-c1-modelo-logico-datos-corporativo.png)

Para facilitar la lectura de cada atributo y cardinalidad, el modelo se divide en dos diagramas derivados:
- **`DIAG-C1.1` (Parte 1 — Dominio Comercial, Almacén PT, Picking Asociativo `M:N` y Empaque):** Detalla `ENT-01` a `ENT-06` y las relaciones `REL-01` a `REL-05`.
- **`DIAG-C1.2` (Parte 2 — Eventos Canónicos `T₀`/`T₁`, Auditoría SLA, Incidencias, Retorno y Receptor B2B):** Detalla `ENT-07` a `ENT-10` y las relaciones `REL-06` a `REL-10`.

![DIAG-C1.1: Modelo Lógico de Datos (Parte 1) - Comercial, UA, Picking M:N y Empaque](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-c1.1-modelo-datos-comercial-almacen-picking-empaque.png)

![DIAG-C1.2: Modelo Lógico de Datos (Parte 2) - Eventos Canónicos T0/T1, Incidencias y Retorno](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-c1.2-modelo-datos-eventos-incidencias-retorno-receptor.png)

---

### Arquitectura de Aplicaciones

#### Componentes Propuestos del Mecanismo de Trazabilidad del Despacho (`MTD`) y Adaptadores

Para materializar la solución sin duplicar los sistemas existentes (`REQ-ARQ-001`), el `MTD` se estructura en **6 componentes modulares** y **5 adaptadores Anti-Corruption Layer (`ADP-XX`)**:

| Código | Componente / Adaptador | Estereotipo | Responsabilidad Arquitectónica Principal | Interfaces Clave |
| :--- | :--- | :---: | :--- | :--- |
| **`CMP-ING-01`** | Ingesta y Normalización | `<<service>>` | Recibe eventos de sistemas internos y socios logísticos, valida sintaxis y descarta duplicados vía `SHA-256`. | `IEventIngestPort` |
| **`CMP-COR-02`** | Correlación Lógica | `<<mediator>>` | Resuelve la trazabilidad multidimensional entre `Código UA`, picking en `SPY` y `Número de Pedido`. | `ITraceabilityQueryPort` |
| **`CMP-VAL-03`** | Validación Semántica | `<<rule-engine>>`| Homologa los 5 estados canónicos, valida su secuencia lógica y reordena eventos por marca `T₀`. | `IValidateStateTransition` |
| **`CMP-AUD-04`** | Auditoría Temporal y SLA | `<<telemetry>>` | Estampa la marca de ingesta `T₁`, calcula `ΔT = T₁ - T₀` y emite alertas si `ΔT > 30 min`. | `IAuditStampPort` |
| **`CMP-PUB-05`** | Publicación Desacoplada | `<<dispatcher>>` | Publica estados validados en tópicos del `Bus de Integración` (`SIS-05`) hacia sistemas consumidores. | `IDispatchEventPort` |
| **`CMP-NOT-06`** | Gestión de Incidencias/Retorno| `<<event-handler>>`| Procesa excepciones de ruta y deposita notificaciones de retorno para Almacén Lurín (`ACT-03`). | `IRouteIncidentPort` |
| **`ADP-SAP`** | Adaptador `SAP R3` | `<<adapter>>` | Conector estrictamente de **solo lectura** para metadatos de stock y cajas `UA` (`REQ-RES-002`). | `ISAPRFCClientPort` |
| **`ADP-SPY` / `NSDG`**| Adaptadores Internos CD | `<<adapter>>` | Traducen eventos de preparación/cajas desde `SPY` (`SIS-02`) y salida de ruta desde `NSDG` (`SIS-06`). | `ISPYEventConsumerPort` |
| **`ADP-LOG`** | Adaptador Logística Externa | `<<gateway>>` | Recibe webhooks/colas desde `DRIVING` (`SIS-08`) y apps móviles con soporte *Store-and-Forward*. | `ICarrierWebhookPort` |
| **`ADP-CRM`** | Adaptador `CELLFORCE` | `<<adapter>>` | Suscribe eventos del Bus para actualizar en `≤ 30 min` la consola de Servicio al Cliente (`ACT-08`). | `ICRMNotificationPort` |

#### Diagramas de Componentes UML e Interfaces (`DIAG-C2` y Derivados)

El **Diagrama General `DIAG-C2`** modela la arquitectura de componentes UML completa, mostrando el cableado de puertos provistos y requeridos entre los sistemas emisores, la capa de adaptadores ACL, el núcleo del `MTD`, el `Bus de Integración` (`SIS-05`) y los sistemas consumidores.

![DIAG-C2: Diagrama General de Componentes UML e Interfaces del MTD](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-c2-componentes-uml-e-interfaces.png)

Sus dos diagramas derivados presentan el zoom arquitectónico por subsistema:
- **`DIAG-C2.1` (Parte 1 — Emisores, Adaptadores ACL, Ingesta, Auditoría SLA y Correlación `UA ↔ Pedido`):** Enfatiza la entrada de datos y las reglas de negocio internas.
- **`DIAG-C2.2` (Parte 2 — Publicación Pub/Sub, Gestión de Retornos, Bus Corporativo y Consumidores):** Enfatiza la distribución asíncrona hacia `Maya`, `ENSDY`, `CELLFORCE` y la bandeja de notificación de Almacén Lurín (`ACT-03`).

![DIAG-C2.1: Componentes UML (Parte 1) - Ingesta, Auditoría SLA y Correlación](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-c2.1-componentes-ingesta-validacion-correlacion-auditoria.png)

![DIAG-C2.2: Componentes UML (Parte 2) - Publicación, Excepciones y Consumidores](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-c2.2-componentes-publicacion-excepciones-bus-consumidores.png)

---

## Arquitectura Tecnológica

### Infraestructura Híbrida, Seguridad y Resiliencia en Ruta

La Arquitectura Tecnológica reemplaza los intercambios batch dispersos por una **Topología Híbrida de Alta Disponibilidad** dividida en tres entornos interconectados con seguridad perimetral validada por `ACT-11`:

| Zona / Nodo Tecnológico | Componentes Desplegados | Especificación Técnica y de Seguridad |
| :--- | :--- | :--- |
| **1. Centro de Cómputo On-Premise Lurín (`Node_Lurin`)** | `SAP R3` (`SIS-01`), `SPY` (`SIS-02`), `Terminales RF` (`SIS-10`) y adaptadores locales (`ADP-SAP`, `ADP-SPY`). | Red industrial segmentada; firewall interno que bloquea toda escritura externa hacia `SAP R3` (`REQ-RES-002`); enlace privado VPN IPsec / MPLS redundante hacia la nube corporativa. |
| **2. Nube Corporativa Yanbal (`Node_Cloud`)** | **DMZ:** `API Gateway` + WAF.<br>**Clúster K8s:** Microservicios `MTD` (`CMP-01..06`), `Bus SIS-05` (Kafka/RabbitMQ), `DB-EVENTSTORE` (PostgreSQL/TimescaleDB + Redis) y apps (`Maya`, `ZapCommerce`, `ENSDY`, `CELLFORCE`). | Cifrado en tránsito **TLS 1.3** y autenticación mutua **mTLS** / OAuth 2.0; clúster autoescalable (disponibilidad `≥ 99.5%`); partición *Hot Data* (`< 90 días`, respuesta `< 100 ms`) y *Cold Archive* (`5 años`). |
| **3. Entorno de Socios Logísticos y Destinos (`Node_Logistics`)** | Sistemas `NSDG` (`SIS-06`), `DRIVING` (`SIS-08`), App Móvil de Transporte (`ACT-07`) y portales de consulta B2B (`ACT-09`). | Mecanismo **Store-and-Forward** con base local cifrada (`SQLite AES-256`) que guarda eventos con su marca `T₀` en tramos sin cobertura celular y los retransmite en ráfaga al reconectar. |

### Diagramas de Despliegue UML (`DIAG-07` y Derivados)

El **Diagrama General `DIAG-07`** ilustra la topología física y lógica de despliegue TO-BE a través de los tres entornos (On-Premise Lurín, Nube Corporativa Yanbal y Red Externa de Socios Logísticos/Destinos B2B), especificando protocolos, puertos y controles de seguridad.

![DIAG-07: Diagrama General de Despliegue de la Arquitectura Tecnológica TO-BE](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-07-despliegue-arquitectura-tecnologica-to-be.png)

Sus tres diagramas derivados permiten examinar cada entorno de infraestructura por separado:
- **`DIAG-07.1` (Parte 1 — Nodo On-Premise Lurín e Interconexión Segura):** Detalla el aislamiento de `SAP R3`, el servidor `SPY`, la red RF industrial y el túnel VPN IPsec/MPLS.
- **`DIAG-07.2` (Parte 2 — Nube Corporativa Yanbal, DMZ, Clúster MTD, Bus y Persistencia):** Detalla el `API Gateway`, el clúster Kubernetes del `MTD`, el `Bus SIS-05`, el Event Store y las aplicaciones corporativas.
- **`DIAG-07.3` (Parte 3 — Infraestructura de Socios Logísticos, Móviles Offline y Destinos B2B):** Detalla `DRIVING`, `NSDG`, el buffer móvil *Store-and-Forward* y los canales de acceso B2B.

![DIAG-07.1: Despliegue TO-BE (Parte 1) - Nodo On-Premise Lurín e Interconexión](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-07.1-despliegue-onpremise-lurin-e-interconexion.png)

![DIAG-07.2: Despliegue TO-BE (Parte 2) - Nube Corporativa, Clúster MTD y Bus](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-07.2-despliegue-nube-corporativa-cluster-mtd-y-apps.png)

![DIAG-07.3: Despliegue TO-BE (Parte 3) - Socios Logísticos y Destinos B2B](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-07.3-despliegue-socios-logisticos-movil-y-destinos-b2b.png)

---

## Oportunidades y Soluciones

### Consolidación de Paquetes de Trabajo y Selección de la Solución

Para cerrar las brechas de negocio, datos, aplicaciones y tecnología sin interrumpir la operación diaria de Lurín, las intervenciones se estructuran en **cinco Paquetes de Trabajo (`PKG-01` a `PKG-05`)** tras evaluar tres alternativas arquitectónicas (seleccionándose la **Alternativa 2: Plataforma de Integración Orientada a Eventos y Servicios sobre el Bus Corporativo**, por preservar los sistemas actuales, evitar bloqueos de proveedor y cumplir el aislamiento de `SAP R3`):

| Paquete | Nombre del Paquete de Trabajo | Componentes y Entregables Clave | Brechas que Cierra | Dependencias |
| :--- | :--- | :--- | :--- | :--- |
| **`PKG-01`** | Gobernanza de Datos, Contratos Canónicos y API Gateway | Esquemas JSON (`INT-DAT-01..08`), catálogo de 5 estados y despliegue de `API Gateway` + WAF en DMZ. | `GAP-02`, `GAP-03`, `GAP-08`, `GAP-TEC-01` | Ninguna (Base fundacional). |
| **`PKG-02`** | Plataforma Core MTD y Repositorio de Eventos | Microservicios `CMP-ING-01`, `CMP-COR-02`, `CMP-VAL-03`, `CMP-PUB-05` y base `DB-EVENTSTORE` (`ENT-04`, `ENT-07`). | `GAP-04`, `GAP-TEC-02`, `GAP-TEC-03` | Depende de `PKG-01`. |
| **`PKG-03`** | Adaptadores de Integración con Sistemas Internos | Conectores `ADP-SAP` (solo lectura), `ADP-SPY`, `ADP-NSDG`, `ADP-CRM` y tópicos en `Bus SIS-05`. | `GAP-05`, `GAP-SYS-01` | Depende de `PKG-01` y `PKG-02`. |
| **`PKG-04`** | Pasarela de Socios Logísticos y Resiliencia en Ruta | Gateway `ADP-LOG`, integración con `DRIVING` y módulo móvil *Store-and-Forward* (`SQLite` offline) + `ENT-10`. | `GAP-01`, `GAP-06`, `GAP-TEC-04` | Depende de `PKG-01` y `PKG-02`. |
| **`PKG-05`** | Logística Inversa, Telemetría de SLAs y Seguridad | Componentes `CMP-NOT-06` (retorno a `ACT-03` sin tocar `SAP`), `CMP-AUD-04` (`ΔT ≤ 30 min`) y auditoría `ACT-11`. | `GAP-07`, `GAP-TEC-05`, `GAP-TEC-06` | Depende de `PKG-03` y `PKG-04`. |

### Diagramas de Arquitectura de Solución y Paquetes de Trabajo (`DIAG-08` y Derivados)

El **Diagrama General `DIAG-08`** integra la vista de la arquitectura de solución seleccionada junto con el mapa de los cinco paquetes de trabajo (`PKG-01` a `PKG-05`) y sus relaciones de dependencia.

![DIAG-08: Diagrama General de Arquitectura de Solución y Paquetes de Trabajo](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-08-arquitectura-solucion-y-paquetes-trabajo.png)

Sus dos diagramas derivados separan limpiamente la arquitectura funcional de la estructura programática de paquetes:
- **`DIAG-08.1` (Arquitectura de Solución Seleccionada — EDA/SOA Híbrida):** Muestra los componentes de la solución operando sobre el Bus Corporativo.
- **`DIAG-08.2` (Mapa y Dependencias de Paquetes de Trabajo `PKG-01` a `PKG-05`):** Muestra la secuencia técnica de construcción y el alcance de cada paquete.

![DIAG-08.1: Componentes de la Arquitectura de Solución Seleccionada](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-08.1-componentes-arquitectura-solucion-seleccionada.png)

![DIAG-08.2: Alineación y Dependencias de Paquetes de Trabajo (PKG-01 a PKG-05)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-08.2-paquetes-mapa-trabajo-pkg01-pkg05.png)

---

## Planificación de la Migración

### Arquitecturas de Transición, Roadmap y Gestión de Riesgos

Se descarta una migración masiva (*Big Bang*) para proteger la continuidad del Centro de Distribución de Lurín. El despliegue se planifica en un horizonte referencial de **24 semanas (S1 a S24)** estructurado en **dos Arquitecturas de Transición (`AT-1` y `AT-2`)** previas al estado objetivo (**`TO-BE`**):

| Etapa Evolutiva | Horizonte | Paquetes Ejecutados | Capacidades Habilitadas e Hito de Control (*Quality Gate*) |
| :--- | :---: | :--- | :--- |
| **Línea Base (`AS-IS`)** | S0 | — | Seguimiento con desfase de hasta 2 horas, reportes batch heterogéneos y sin marcas `T₀`/`T₁`. |
| **Transición 1 (`AT-1`): Núcleo MTD e Integración Interna** | Semanas S1 – S12 | `PKG-01`, `PKG-02`, `PKG-03` (Fase A: Lurín) | `API Gateway` activo, microservicios Core `MTD`, correlación `UA ↔ Pedido` desde `SPY` y lectura aislada `ADP-SAP`.<br>**Gate 1 (S12):** Certificación de cero escrituras en `SAP R3` por `ACT-03` y seguridad DMZ por `ACT-11`. |
| **Transición 2 (`AT-2`): Transporte Externo y Piloto B2B** | Semanas S11 – S18 | `PKG-04`, `PKG-03` (Fase B: Consumo CRM) | Onboarding de socios logísticos (`ACT-07`), app móvil con *Store-and-Forward* offline y sincronización a `CELLFORCE`.<br>**Gate 2 (S18):** Cero pérdida de eventos offline y cumplimiento de `ΔT ≤ 30 min` en `≥ 95%` de rutas piloto. |
| **Arquitectura Objetivo (`TO-BE`): Despliegue Nacional** | Semanas S17 – S24 | `PKG-05` | Cobertura en los 24 departamentos, canal formal de logística inversa (`CMP-NOT-06`) hacia Lurín y telemetría SLA activa.<br>**Gate 3 (S24):** Cumplimiento nacional `≥ 98%` en ventana de 30 min y pase a operación continua. |

#### Principales Riesgos de Transformación y Mitigación

| Código | Riesgo Identificado | Nivel | Medida de Mitigación Arquitectónica |
| :--- | :--- | :---: | :--- |
| **`RSK-01` / `02`**| Retraso en adopción de contratos API por transportistas externos (`ACT-07`). | Alto | Provisión del adaptador `ADP-LOG`, entorno Sandbox y librería móvil lista para usar sin exigir GPS propietario. |
| **`RSK-03`** | Intento de acoplamiento o escritura accidental sobre la base de datos de `SAP R3`. | Crítico | Bloqueo por diseño mediante cuenta RFC de solo lectura en `ADP-SAP` y veto vinculante de `ACT-03` en el Gate 1. |
| **`RSK-04`** | Vulnerabilidades en endpoints expuestos hacia internet desde rutas nacionales. | Alto | Perímetro DMZ con WAF, cifrado TLS 1.3, mTLS y auditoría obligatoria SAST/DAST por Seguridad Informática (`ACT-11`). |
| **`RSK-06`** | Pérdida de cobertura celular en tramos de sierra y selva durante el tránsito. | Alto | Almacenamiento local cifrado (*Store-and-Forward*) con sello `T₀` in situ y deduplicación `SHA-256` al reconectar. |

### Diagramas del Roadmap Estratégico de Implementación (`DIAG-09` y Derivados)

El **Diagrama General `DIAG-09`** modela la línea de tiempo y la máquina de estados evolutiva desde `AS-IS` pasando por `AT-1` (`Gate 1`), `AT-2` (`Gate 2`) hasta alcanzar la arquitectura `TO-BE` (`Gate 3`).

![DIAG-09: Roadmap Estratégico de Implementación y Arquitecturas de Transición](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-09-roadmap-estrategico-implementacion.png)

Sus dos diagramas derivados presentan el detalle de cada mitad del horizonte de migración:
- **`DIAG-09.1` (Parte 1 — De Línea Base AS-IS a `AT-1` y `Gate 1`, Semanas S1 a S12):** Construcción del cimiento híbrido e integración interna en Lurín.
- **`DIAG-09.2` (Parte 2 — De `Gate 1` a `AT-2`, `Gate 2` y `TO-BE`, Semanas S11 a S24):** Integración externa de transporte, piloto B2B y despliegue nacional en los 24 departamentos.

![DIAG-09.1: Roadmap (Parte 1) - De AS-IS a AT-1 y Gate 1 (Semanas S1 a S12)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-09.1-estados-roadmap-as-is-a-at1-gate1.png)

![DIAG-09.2: Roadmap (Parte 2) - De AT-2 y Gate 2 a TO-BE (Semanas S11 a S24)](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-09.2-estados-roadmap-at2-gate2-a-to-be.png)

---

## Gobierno de la Implementación

### Órganos de Gobierno, Rol del Arquitecto Empresarial y Contratos de Arquitectura

El aseguramiento de la conformidad durante el desarrollo y despliegue está dirigido por el **Consejo de Arquitectura (*Architecture Board*)** e instrumentado mediante cinco **Contratos de Arquitectura (`AC-PKG-01` a `AC-PKG-05`)** y tres compuertas de calidad (**`GATE 1`**, **`GATE 2`** y **`GATE 3`**).

| Rol / Instancia de Gobernanza | Actor Canónico | Responsabilidad Específica Durante la Implementación (Fase G) |
| :--- | :--- | :--- |
| **Sponsor Ejecutivo (Presidente)** | Gerencia de TI (`ACT-13`) | Aprueba contratos de arquitectura, presupuesto de transición y autoriza el pase final a producción. |
| **Arquitecto Empresarial Líder** | Arquitectura TI (`ACT-12`) | **Rol central en Fase G:** Audita el cumplimiento de esquemas OpenAPI/JSON y modelos UML, lidera las revisiones de conformidad en los *Quality Gates*, evalúa solicitudes de dispensa (*Waivers*) y previene desviaciones de diseño. |
| **Custodio Soberano del ERP** | Jefatura Almacén (`ACT-03`) | Ejerce **derecho de veto vinculante** para certificar que ningún componente tenga permisos de escritura en `SAP R3`. |
| **Autoridad de Ciberseguridad** | Oficial CISO (`ACT-11`) | Ejerce **derecho de veto vinculante** sobre la exposición de APIs, certificados mTLS/TLS 1.3 y pruebas de penetración. |
| **Propietario Funcional del Proceso**| Jefatura Distribución (`ACT-06`) | Valida el cumplimiento funcional de los 5 estados, la latencia `ΔT ≤ 30 min` y la homologación de socios logísticos. |

#### Gestión de Excepciones y Solicitudes de Dispensa (*Architecture Waivers*)

Ningún equipo de desarrollo puede introducir atajos técnicos ocultos. Si surge un impedimento técnico temporal, debe tramitarse un **Formulario de Dispensa (`WVR`)**:
1. **Regla de Veto Innegociable:** Ningún *Waiver* puede autorizar escrituras en `SAP R3` (`REQ-RES-002`) ni omitir el cifrado perimetral (`REQ-RNF-003`).
2. **Vigencia Temporal y Extinción:** Toda dispensa aprobada registra su deuda técnica y tiene una fecha máxima de caducidad que debe subsanarse obligatoriamente antes del **`GATE 3` (Semana S24)**.

### Diagramas de Gobernanza de la Implementación (`DIAG-10` y Derivados)

El **Diagrama General `DIAG-10`** presenta la estructura del *Architecture Board*, los cinco contratos `AC-PKG-01..05`, las compuertas `GATE 1..3` y el flujo de control de conformidad y dispensas.

![DIAG-10: Diagrama General de Gobernanza de la Implementación y Conformidad](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-10-gobernanza-implementacion-y-conformidad.png)

Sus dos diagramas derivados desglosan la estructura institucional y el circuito de excepciones:
- **`DIAG-10.1` (Parte 1 — Órganos de Gobierno, Contratos `AC-PKG-01..05` y Quality Gates `GATE 1..3`):** Muestra quién gobierna, qué contratos rigen cada paquete y qué se audita en cada compuerta.
- **`DIAG-10.2` (Parte 2 — Circuito de Gestión de Excepciones y Solicitudes de Dispensa `Waivers`):** Muestra el flujo de decisión ante no conformidades, el bloqueo de violaciones críticas y el control de vencimiento de deuda técnica.

![DIAG-10.1: Gobernanza (Parte 1) - Órganos, Contratos y Quality Gates](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-10.1-paquetes-gobernanza-contratos-y-quality-gates.png)

![DIAG-10.2: Gobernanza (Parte 2) - Circuito de Excepciones y Waivers](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-10.2-actividades-circuito-excepciones-y-waivers.png)

---

## Gestión del Cambio de Arquitectura

### Monitoreo Continuo y Clasificación de Solicitudes de Cambio (`RFC` Tipos 1, 2 y 3)

Una vez en producción nacional, la Fase H asegura que la arquitectura evolucione sin degradarse mediante sensores de telemetría continua (`CMP-AUD-04` midiendo `ΔT = T₁ - T₀ ≤ 30 min`, disponibilidad `≥ 99.5%` y volumen de reclamos en `CELLFORCE`) y un modelo de clasificación de **Solicitudes de Cambio de Arquitectura (`RFC`) en tres niveles**:

| Tipo de Cambio (`RFC`) | Alcance e Impacto Arquitectónico | Ejemplo en Yanbal Perú | Instancia Aprobadora y Acción en el Ciclo TOGAF ADM |
| :--- | :--- | :--- | :--- |
| **Tipo 1: Cambio Simplificado (Menor)** | Paramétrico o de configuración; no altera esquemas de datos, interfaces ni componentes. | Incorporar un nuevo socio logístico (`ACT-07`) que ya cumple el contrato estándar JSON en `ADP-LOG`. | Aprobación ágil por el **Arquitecto Líder (`ACT-12`)**.<br>**No reinicia el ciclo ADM** (actualización menor del repositorio EAR). |
| **Tipo 2: Cambio Incremental (Moderado)** | Extiende atributos opcionales o adapta un conector sin romper retrocompatibilidad ni principios. | Agregar un campo informativo opcional en el payload de `DRIVING` o ajustar umbrales de alerta preventiva. | Aprobación por el **Comité Técnico del Architecture Board**.<br>**Iteración parcial en Fases C/D → G** sin reabrir la Visión. |
| **Tipo 3: Cambio de Re-arquitectura (Mayor)** | Altera objetivos estratégicos, fronteras de alcance, principios rectores o núcleos transaccionales. | Migración futura de `SAP R3` a `SAP S/4HANA` o solicitud para ampliar la trazabilidad al reparto secundario B2C. | Aprobación ejecutiva por el **Architecture Board en pleno (`ACT-13`, `ACT-03`, `ACT-11`)**.<br>**REINICIA UN NUEVO CICLO COMPLETO DE TOGAF ADM (vuelta a Fase A).** |

### Diagramas de Gestión del Cambio y Ciclo Evolutivo ADM (`DIAG-11` y Derivados)

El **Diagrama General `DIAG-11`** modela el ciclo cerrado de monitoreo continuo de SLAs, detección de disparadores de cambio, evaluación de impacto y bifurcación según los tres tipos de `RFC`.

![DIAG-11: Diagrama General de Gestión del Cambio y Ciclo Evolutivo ADM](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-11-gestion-del-cambio-y-ciclo-evolutivo-adm.png)

Sus dos diagramas derivados permiten visualizar por separado el monitoreo y la toma de decisiones:
- **`DIAG-11.1` (Parte 1 — Sensores de Monitoreo Continuo y Evaluación de Impacto de RFC):** Muestra la captura de métricas operativas y técnicas y el filtro inicial de restricciones fundacionales.
- **`DIAG-11.2` (Parte 2 — Clasificación Tripartita `RFC Tipo 1, 2 y 3`, Repositorio EAR y Reinicio del Ciclo ADM):** Muestra las tres rutas de aprobación, la actualización del repositorio de arquitectura y el retorno a la Fase A cuando ocurre un cambio mayor (Tipo 3).

![DIAG-11.1: Gestión del Cambio (Parte 1) - Monitoreo Continuo y Evaluación de RFC](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-11.1-actividades-monitoreo-continuo-y-evaluacion-rfc.png)

![DIAG-11.2: Gestión del Cambio (Parte 2) - Clasificación RFC (Tipos 1, 2 y 3) y Reinicio ADM](C:/Users/joses/.gemini/antigravity/brain/9e66d8c5-f676-4305-b146-fe944d7256e4/scratch/img/diag-11.2-actividades-clasificacion-rfc-ear-y-reinicio-adm.png)

---

## Conclusiones Generales

1. **Resolución de la Problemática Central de Negocio:** La arquitectura diseñada reduce el desfase de actualización de estados del despacho B2B desde hasta 2 horas a un máximo auditable de **30 minutos (`ΔT = T₁ - T₀ ≤ 30 min`)** en los 24 departamentos del Perú, dotando de visibilidad oportuna a Distribución (`ACT-06`), Servicio al Cliente (`ACT-08` en `CELLFORCE`) y destinatarias empresariales (`ACT-09`).
2. **Coexistencia Armónica y Respeto de Restricciones:** El Mecanismo de Trazabilidad del Despacho (`MTD`) se integra sobre el `Bus de Integración` corporativo (`SIS-05`) mediante seis componentes modulares (`CMP-01` a `CMP-06`) y cinco adaptadores (`ADP-XX`), preservando los sistemas actuales y garantizando **cero escrituras directas sobre `SAP R3` (`REQ-RES-002`)**, cuya operación permanece exclusiva para el Área de Almacén Lurín (`ACT-03`).
3. **Trazabilidad Estructural Completa:** El modelo lógico de datos (`ENT-01` a `ENT-10`) resuelve la correlación muchos a muchos (`M:N`) entre las cajas master mono SKU (`Código UA`) y los pedidos comerciales multi SKU mediante la entidad asociativa `ENT-04`, mientras que el registro inmutable `ENT-07` y el mecanismo *Store-and-Forward* aseguran la integridad de los eventos incluso en rutas interprovinciales sin cobertura celular.
4. **Transición Segura y Gobernada:** El plan de implementación gradual en tres etapas (`AT-1`, `AT-2` y `TO-BE`) controlado por tres *Quality Gates* (`GATE 1` a `GATE 3`), contratos de arquitectura vinculantes y el modelo de gestión de cambios (`RFC` Tipos 1, 2 y 3) asegura una adopción controlada, medible y sostenible en el tiempo.
