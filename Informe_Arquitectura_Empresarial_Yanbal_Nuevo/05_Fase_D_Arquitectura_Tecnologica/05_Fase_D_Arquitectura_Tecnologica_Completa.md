# 5. FASE D: ARQUITECTURA TECNOLÓGICA
## Infraestructura, Plataformas Híbridas, Comunicaciones y Seguridad para la Trazabilidad del Despacho B2B de Yanbal Perú (Unique S.A.)

> **Ubicación oficial del documento:** `05_Fase_D_Arquitectura_Tecnologica/05_Fase_D_Arquitectura_Tecnologica_Completa.md`  
> **Diagrama asociado:** `DIAG-07` (*Diagrama UML de Despliegue TO-BE*, disponible en código PlantUML editable en `05_Fase_D_Arquitectura_Tecnologica/diagramas/DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml`)  
> **Marco metodológico:** TOGAF Standard (The Open Group Architecture Framework) — Fase D: Arquitectura Tecnológica (*Technology Architecture*)  
> **Documentos de referencia y gobernanza:** Fase Preliminar, Fase A (Visión de Arquitectura), Fase B (Arquitectura de Negocio), Fase C (Arquitectura de Sistemas de Información), Matriz Maestra de Requerimientos, Catálogo Maestro de Actores y Sistemas, Glosario Canónico y Convenciones de Nomenclatura, y Registro Maestro de Supuestos, Incertidumbres y Decisiones de Arquitectura.  
> **Principio de gobernanza:** Todos los documentos precedentes del repositorio se preservan intactos como fuentes de verdad canónicas. La presente fase se desarrolla de manera autosuficiente, garantizando la trazabilidad hacia los 24 requerimientos oficiales y los principios de arquitectura empresarial.

---

## Introducción y Objetivos de la Arquitectura Tecnológica

### 1. Propósito y Alcance de la Fase D

La Fase D del ciclo TOGAF ADM (*Technology Architecture*) tiene como propósito fundamental estructurar la infraestructura tecnológica, los entornos de ejecución, las plataformas de cómputo, los sistemas operativos, las redes de comunicación y los estándares de seguridad necesarios para dar soporte operativo y físico a los componentes de datos y aplicaciones modelados en la Fase C, permitiendo materializar los procesos de negocio optimizados diseñados en la Fase B.

En el contexto del proyecto de transformación para Yanbal Perú (operando legalmente bajo la razón social Unique S.A.), la Arquitectura Tecnológica define los cimientos computacionales y de red requeridos para implementar el Mecanismo de Trazabilidad del Despacho (`MTD`), concebido como una suite de capacidades lógicas desacopladas (`CMP-ING-01` a `CMP-NOT-06`) y adaptadores especializados (`ADP-SAP`, `ADP-SPY`, `ADP-LOG`, `ADP-CRM`). 

El alcance tecnológico se circunscribe de forma estricta a la cadena logística de despacho B2B:
* **Frontera de Inclusión:** Abarca la infraestructura on-premise ubicada en las instalaciones industriales y de almacenamiento de Lurín (Lima), los entornos corporativos centrales de TI, la plataforma híbrida de integración y mensajería, los canales de intercambio seguro con los proveedores logísticos asociados (`ACT-07`), y los puntos de consulta de estados para el Área de Servicio al Cliente (`ACT-08`) y las distribuidoras o destinatarios empresariales (`ACT-09`) distribuidos en los 24 departamentos del territorio peruano.
* **Frontera de Exclusión:** Se excluye de manera categórica el diseño o provisión de infraestructura para el reparto capilar secundario hacia consumidoras finales (`REQ-RES-001`), la adquisición o monitoreo telemático continuo de una flota vehicular propietaria (`REQ-RES-004`), y la modificación o sustitución de los núcleos transaccionales consolidados de la compañía (`PAE-02`).

### 2. Objetivos de la Arquitectura Tecnológica

En concordancia con los lineamientos de TOGAF ADM y las metas fijadas en la Visión de Arquitectura, la Fase D persigue los siguientes objetivos técnicos:

1. **Establecer la línea base tecnológica (AS-IS):** Documentar la infraestructura física, los servidores, los sistemas operativos, el middleware y las redes de comunicación actualmente en producción, identificando las limitaciones de conectividad y silos de información que generan el desfase de seguimiento documentado en la operación.
2. **Definir la arquitectura tecnológica objetivo (TO-BE):** Diseñar una topología híbrida (on-premise y nube corporativa) articulada sobre un Bus de Integración (`SIS-05`) y una puerta de enlace segura (*API Gateway*), que garantice la interoperabilidad desacoplada entre sistemas internos y plataformas heterogéneas de transporte.
3. **Soportar el objetivo de reducción de latencia:** Habilitar un procesamiento asíncrono y orientado a eventos capaz de procesar y propagar las actualizaciones de estado en una ventana temporal de hasta 30 minutos ($\Delta T \le 30\text{ min}$), cumpliendo con el requerimiento de calidad `REQ-RNF-001`.
4. **Garantizar resiliencia y tolerancia a fallas en ruta:** Proveer mecanismos técnicos de persistencia local temporal (*Store-and-Forward*) en los aplicativos móviles de transporte, mitigando el riesgo de pérdida o intermitencia de conectividad celular en rutas nacionales remotas (`ASM-16`, `RSK-06`).
5. **Cumplir con los estándares de Seguridad Informática corporativa:** Implementar un perímetro de protección riguroso validado por el Área de Seguridad Informática (`ACT-11`), que incorpore cifrado robusto en tránsito (TLS 1.3), autenticación mutua (mTLS), autorización basada en roles (RBAC) y registros inmutables de auditoría temporal ($T_0, T_1$).
6. **Preservar el aislamiento operacional del ERP corporativo:** Garantizar a nivel de arquitectura física y de red que ningún componente externo ni servicio propuesto del `MTD` posea privilegios ni interfaces de escritura directa sobre la base de datos de `SAP R3` (`SIS-01`), respetando de forma vinculante la restricción `REQ-RES-002`.

---

## 5.1. Infraestructura tecnológica actual (Línea Base AS-IS)

La arquitectura tecnológica actual de Yanbal Perú refleja un crecimiento evolutivo caracterizado por la convivencia de sistemas heredados (*legacy*) alojados localmente con plataformas satélites de terceros y herramientas comerciales web. De acuerdo con el diagnóstico operativo del caso, este ecosistema opera bajo la premisa de "tres sistemas propios adaptados específicamente para la operación y cuatro sistemas de proveedores adaptados para integrarse a ellos". A continuación se describen los componentes tecnológicos de la línea base:

```
+----------------------------------------------------------------------------------------------------+
|                         LÍNEA BASE TECNOLÓGICA ACTUAL (AS-IS)                                      |
+----------------------------------------------------------------------------------------------------+
|  INSTALACIONES LURÍN (ON-PREMISE)          |  SEDE CENTRAL / CLOUD       |  RED EXTERNA DE TERCEROS|
|                                            |                             |                         |
|  [SAP R3 SIS-01]        [SPY SIS-02]       |  [Maya SIS-03]              |  [NSDG SIS-06]          |
|  Servidor On-Premise    Servidor On-Premise|  Portal Comercial           |  Servidor Contratista   |
|  BD Propietaria / RFC   Windows / Base SQL |                             |                         |
|         ^                      ^           |  [ZapCommerce SIS-04]       |  [DRIVING SIS-08]       |
|         |                      |           |  Motor Transaccional        |  Plataforma Externa     |
|  [Terminales RF SIS-10]        |           |                             |                         |
|  WiFi Industrial 802.11        |           |  [Bus Integración SIS-05]   |  [ENSDY SIS-07]         |
|  Emulación Terminal            |           |  Middleware Heterogéneo     |  Servidor de Trackeo    |
|                                |           |                             |                         |
|                                +-----------+-- Enlace WAN / VPN Punto ---+-- Conexiones Batch /    |
|                                            |   a Punto Intermitente      |   Sincronización Lenta  |
+----------------------------------------------------------------------------------------------------+
```

### 5.1.1. Servidores, almacenamiento y plataformas on-premise (Planta y Almacén Lurín)

La operación física nuclear de almacenamiento y producción se concentra en las instalaciones de Lurín:
* **Entorno ERP `SAP R3` (`SIS-01`):** Alojado en servidores dedicados on-premise dentro del centro de cómputo local de Lurín. Ejecuta la instancia productiva central del ERP, gestionando los módulos de inventario de productos terminados, control de tres estados de stock (*libre disposición*, *control de calidad*, *bloqueado*) y facturación. El acceso a este entorno está física y lógicamente restringido al personal del Área de Almacén (`ACT-03`). Las interfaces de comunicación hacia otros entornos se realizan mediante llamadas a funciones remotas propietarias (RFC) y transferencias batch periódicas.
* **Entorno BWMS `SPY` (`SIS-02`):** Sistema desarrollado *in-house* para la gestión del Centro de Distribución y las líneas de picking/packing. Se aloja en servidores de aplicación locales en Lurín sobre sistemas operativos tipo servidor y motores de bases de datos relacionales estándar. Gestiona los algoritmos de volumetría teórica, pesaje en básculas y asignación de los 8 tipos de cajas de despacho.
* **Almacenamiento Físico Local:** La infraestructura de almacenamiento de Lurín provee soporte a una instalación de 15,000 $\text{m}^2$ con capacidad estática para albergar hasta 2 millones de cajas mono SKU (`CAP-01`). No obstante, a nivel de cómputo, las bases de datos locales no están dimensionadas para atender consultas masivas concurrentes en tiempo real desde el exterior.

### 5.1.2. Plataformas comerciales y de atención corporativa

* **Plataforma Comercial `Maya` (`SIS-03`):** Aplicación corporativa web y móvil orientada a las consultoras y directoras de la fuerza de ventas para la captura e ingreso de órdenes de compra. Se aloja en un entorno corporativo centralizado, canalizando los pedidos hacia el motor transaccional.
* **Motor Comercial `ZapCommerce` (`SIS-04`):** Plataforma encargada de la liquidación comercial, validación de reglas de negocio y facturación de órdenes, conectada estrechamente con `Maya` y responsable de emitir los eventos que marcan el `Estado 1: Pedido facturado`.
* **Sistema de Gestión de Clientes `CELLFORCE` (`SIS-09`):** Plataforma utilizada por el Área de Servicio al Cliente (`ACT-08`) para el registro, consulta y resolución de reclamos e incidencias reportadas por los destinatarios empresariales. Consume datos de seguimiento para verificar el cumplimiento de las promesas de entrega.

### 5.1.3. Sistemas y plataformas satélites de transporte

* **Sistema de Transporte `NSDG` (`SIS-06`):** Aplicativo desarrollado por un contratista de software para la gestión del transporte de la operación en Perú. Administra la documentación de salida de despacho, guías de remisión y consolidación de hojas de ruta hacia los 24 departamentos.
* **Sistema de Trackeo `ENSDY` (`SIS-07`):** Herramienta que centraliza la visualización de estados del despacho para el seguimiento logístico interno. Presenta la brecha histórica de actualización con un desfase promedio y de hasta dos horas respecto al evento físico.
* **Plataforma Externa de Despacho `DRIVING` (`SIS-08`):** Plataforma externa (*Transport Management System* - TMS) empleada por diversas empresas socias de transporte para gestionar sus rutas y asignación de unidades vehiculares. Opera de manera satélite y autónoma a la infraestructura de Yanbal.

### 5.1.4. Middleware y Bus de Integración actual (`SIS-05`)

La compañía dispone de un `Bus de Integración` corporativo (`SIS-05`) que comunica los sistemas internos. Sin embargo, en la arquitectura AS-IS:
* Predominan integraciones punto a punto o adaptaciones propietarias entre sistemas específicos (p. ej., `SAP R3` con `SPY` y `SPY` con `NSDG`).
* No existen contratos canónicos universales para la transmisión de eventos de transporte en tiempo casi real.
* La ingesta de datos provenientes de transportistas externos opera mediante procesos por lotes (*batch*) o interfaces asíncronas no estandarizadas, lo que introduce colas de espera que originan el desfase documentado de hasta dos horas.

### 5.1.5. Dispositivos de campo, comunicaciones y conectividad en ruta

* **Terminales de Radiofrecuencia (`SIS-10`):** Dispositivos industriales móviles de mano desplegados en el almacén de Lurín, conectados a una red inalámbrica industrial cerrada (WiFi 802.11) que operan en emulación de terminal para la captura del `Código UA` en pallets y cajas master mono SKU.
* **Dispositivos Móviles de Transportistas:** Los conductores y tripulaciones de los socios logísticos (`ACT-07`) utilizan dispositivos móviles comerciales heterogéneos (teléfonos inteligentes y terminales portátiles) con conectividad celular provista por operadores de telecomunicaciones comerciales (redes 3G/4G/5G).
* **Conectividad Interprovincial:** En rutas troncales hacia localidades remotas de la sierra y selva, se experimentan zonas de sombra (*shadow areas*) sin cobertura celular, lo cual interrumpe la transmisión inmediata de reportes de entrega o incidencias.

### 5.1.6. Limitaciones tecnológicas identificadas en la línea base

| Dominio Tecnológico | Situación AS-IS | Limitación Técnica e Impacto Operativo | Requerimiento / Riesgo Afectado |
| :--- | :--- | :--- | :--- |
| **Integración con Terceros** | Conexiones ad hoc y volcados por lotes entre proveedores de transporte y sistemas centrales. | Ausencia de una puerta de enlace API unificada y segura; los eventos de despacho y entrega demoran hasta 2 horas en reflejarse. | `REQ-INT-002`, `REQ-RNF-001`, `RSK-02` |
| **Modelado de Eventos** | Mensajes heterogéneos sin marcas de tiempo universales ni diferenciadas. | Imposibilidad técnica de distinguir el momento de ocurrencia operativa ($T_0$) de la hora de llegada al sistema ($T_1$), impidiendo auditar SLAs. | `REQ-ARQ-003`, `OBJ-01`, `RSK-01` |
| **Resiliencia en Ruta** | Dependencia de conexión continua a internet en las aplicaciones de transportistas. | Pérdida de eventos o reintentos desordenados cuando el vehículo atraviesa tramos interprovinciales sin cobertura celular. | `ASM-16`, `RSK-06` |
| **Aislamiento del ERP** | Reglas de acceso a `SAP R3` no formalizadas en contratos de interfaz para logística inversa. | Riesgo de acoplamiento rígido o dependencia de procesos manuales para la notificación de retornos desde distribución hacia almacén. | `REQ-RES-002`, `REQ-FUN-005` |
| **Seguridad y Perímetro** | Interfaces expuestas de forma dispersa entre proveedores y sistemas propios. | Dificultad para aplicar políticas homogéneas de autenticación, autorización y prevención de vulnerabilidades requeridas por TI. | `REQ-RNF-003`, `OBJ-07`, `RSK-04` |

---

## 5.2. Arquitectura tecnológica propuesta (TO-BE)

La Arquitectura Tecnológica Objetivo propone una **Plataforma Híbrida de Integración** que combina la robustez y seguridad de los centros de cómputo on-premise de Yanbal (donde residen los sistemas transaccionales y de producción como `SAP R3` y `SPY`) con la escalabilidad y elasticidad de una plataforma de integración y mensajería alojada en la nube corporativa o en un entorno de infraestructura moderna de alta disponibilidad.

```
+----------------------------------------------------------------------------------------------------+
|                         ARQUITECTURA TECNOLÓGICA PROPUESTA (TO-BE)                                 |
+----------------------------------------------------------------------------------------------------+
|  NUBE CORPORATIVA YANBAL - PLATAFORMA DE INTEGRACIÓN Y MTD                                         |
|                                                                                                    |
|  [ API GATEWAY CORPORATIVO ] <--- TLS 1.3 / mTLS / OAuth2 <--- Socios Logísticos / Consultas Web   |
|         |                                                                                          |
|         v                                                                                          |
|  [ CLÚSTER DE SERVICIOS MTD ] ----------------------------+                                        |
|  * CMP-ING-01: Ingesta e Idempotencia (SHA-256)           |                                        |
|  * CMP-COR-02: Correlación Lógica (UA-Picking-Pedido)     | Pool JDBC / TLS                        |
|  * CMP-VAL-03: Motor Validación Secuencia 5 Estados       v                                        |
|  * CMP-AUD-04: Auditoría Temporal (T0, T1, Delta T)   [(DB) EVENT STORE CANÓNICO ENT-07]           |
|  * CMP-PUB-05: Despachador de Eventos Consolidados    [Hot Data <90d | Cold Archive 5 años]        |
|  * CMP-NOT-06: Notificación de Retornos a Almacén         |                                        |
|         |                                                 |                                        |
|         v AMQP / Pub-Sub (TLS 1.3)                        |                                        |
|  [ BUS DE INTEGRACIÓN CORPORATIVO (SIS-05) ] <------------+                                        |
|     Tópicos: despacho.eventos | despacho.alertas-sla | despacho.notificaciones-almacen             |
|         |                                       |                                    |             |
|         | Suscriptor CRM                        | Suscriptor Trackeo                 | VPN IPsec   |
|         v                                       v                                    v             |
|  [CELLFORCE SIS-09]                      [ENSDY SIS-07]                   [INSTALACIONES LURÍN]    |
|  CRM Servicio Cliente                    Portal Seguimiento               * SPY SIS-02 (ADP-SPY)   |
|  (Delta T <= 30 min)                     (Delta T <= 30 min)              * Notif. a ACT-03        |
|                                                                           * SAP R3 (Aislado)       |
+----------------------------------------------------------------------------------------------------+
```

### 5.2.1. Visión y diseño de la Plataforma Híbrida de Integración

La solución se estructura sobre un modelo de capas que garantiza el desacoplamiento operativo y técnico:
1. **Perímetro Seguro de Ingesta (API Management):** Puerta de entrada única para el tráfico proveniente de redes públicas y plataformas externas de transportistas (`ACT-07`), con funciones de inspección de seguridad, limitación de tasa (*rate limiting*) y terminación criptográfica segura.
2. **Capa de Procesamiento Asíncrono del `MTD`:** Conjunto de microservicios o componentes sin estado (*stateless*) alojados en contenedores ligeros que ejecutan las seis responsabilidades canónicas (`MTD-R01` a `MTD-R06`), diseñados para escalar horizontalmente ante picos estacionales de campaña.
3. **Columna Vertebral de Mensajería (Event Broker):** Instancia reforzada del `Bus de Integración` (`SIS-05`) que provee un patrón de publicación/suscripción (*Pub/Sub*) de alto rendimiento, asegurando la distribución desacoplada de eventos hacia los sistemas consumidores internos (`ENSDY`, `CELLFORCE`, portales comerciales).
4. **Almacén Canónico de Eventos (*Event Store*):** Repositorio optimizado para escrituras masivas que almacena la serie inmutable de eventos de trazabilidad (`ENT-07`) con marcas temporales duales y hashes criptográficos de idempotencia.
5. **Capa Adaptadora de Aislamiento (*Anti-Corruption Layer*):** Módulos adaptadores (`ADP-XX`) que traducen las especificaciones propietarias de los sistemas legados y garantizan que `SAP R3` permanezca enteramente protegido frente a escrituras externas.

### 5.2.2. Entorno de ejecución y hosting para los componentes del MTD

Para garantizar la neutralidad tecnológica (`DEC-02`), la arquitectura TO-BE no prescribe marcas propietarias de proveedores cloud, sino que exige el cumplimiento de un entorno de ejecución estandarizado:
* **Entorno de Contenedores:** Los componentes modulares `CMP-ING-01` a `CMP-NOT-06` se empaquetan en imágenes de contenedores compatibles con el estándar OCI (*Open Container Initiative*), permitiendo su despliegue agnóstico en clústeres gestionados (p. ej., Kubernetes) tanto en nube pública corporativa como en infraestructura privada on-premise.
* **Políticas de Autoescalado:** El clúster de cómputo del `MTD` implementa escalado horizontal automático (*Horizontal Pod Autoscaler*) basado en la longitud de las colas de eventos y en la utilización de CPU/memoria, asegurando el procesamiento fluido de la carga nacional sin retrasar la propagación de eventos más allá del umbral de 30 minutos.
* **Separación de Responsabilidades de Memoria:** El componente `CMP-COR-02` utiliza una memoria caché distribuida en memoria (*in-memory distributed cache*) de solo lectura para acceder rápidamente a la correspondencia entre identificadores de pedido y empaques sin saturar las bases de datos transaccionales de almacén.

---

## 5.3. Componentes de hardware, software de base, red y comunicaciones

A continuación se detallan las especificaciones técnicas de los componentes físicos, lógicos y de red que conforman la arquitectura tecnológica objetivo, diferenciando los recursos existentes a preservar de los propuestos:

### 5.3.1. Nodos de cómputo y entornos de ejecución

| Nodo de Cómputo | Ubicación / Entorno | Tipo de Recurso | Condición | Software de Base y Plataforma | Rol Arquitectónico en la Solución |
| :--- | :--- | :--- | :---: | :--- | :--- |
| **`NODE-SRV-SAP`** | Data Center Lurín (On-Premise) | Servidor Empresarial Dedicado | Existente | Sistema operativo empresarial Linux/Unix de 64 bits, motor SAP NetWeaver y BD relacional certificada. | Hospeda la instancia productiva de `SAP R3` (`SIS-01`). Operado exclusivamente por Almacén (`ACT-03`, `REQ-RES-002`). |
| **`NODE-SRV-SPY`** | Data Center Lurín (On-Premise) | Servidores Locales de Aplicaciones | Existente | Sistema operativo para servidores x86-64, motor relacional transaccional y entorno de ejecución .NET/Java. | Hospeda el BWMS `SPY` (`SIS-02`) y el agente emisor `ADP-SPY`. Gestiona picking, empaque y emite evento con $T_0$. |
| **`NODE-CLUSTER-MTD`** | Nube Corporativa / Data Center Yanbal | Clúster de Cómputo Elástico | **Propuesto** | Clúster de orquestación de contenedores (estándar OCI/Kubernetes), nodos con escalado dinámico. | Ejecuta los componentes `CMP-ING-01` a `CMP-NOT-06` con tolerancia a fallas y alta concurrencia. |
| **`NODE-MSG-BUS`** | Nube Corporativa / Data Center Yanbal | Clúster de Mensajería Distribuida | Existente / Adaptado | Plataforma de mensajería orientada a eventos (`SIS-05`), replicación geográfica y persistencia en disco de colas. | Provee los canales de tópicos Pub/Sub y colas de mensajes del Bus de Integración corporativo. |
| **`NODE-APIGW`** | Zona Desmilitarizada (DMZ) Cloud | Clúster de Gateways de Red | **Propuesto** | Plataforma de API Management de alto rendimiento, soporte nativo TLS 1.3, módulos de inspección WAF. | Punto de contacto único para recepción de webhooks y solicitudes REST de transportistas (`ACT-07`). |
| **`NODE-DB-EVENTSTORE`**| Nube Corporativa / Data Center Yanbal | Servidor de Bases de Datos Gestionado | **Propuesto** | Motor de base de datos relacional/orientada a series o documentos con alta velocidad de ingestión y réplicas. | Persistencia del registro canónico inmutable de eventos (`ENT-07`) y logs de auditoría temporal. |
| **`NODE-MOB-CARRIER`** | Red Nacional Externa | Dispositivos Móviles Portátiles | Existente (Terceros) | Android / iOS comercial de socios logísticos, equipados con almacenamiento SQLite cifrado local. | Captura de marcas $T_0$ de eventos de transporte y entrega en ruta por conductores externos. |
| **`NODE-DEV-RF`** | Planta y Almacén Lurín | Terminales Industriales Rugged | Existente | Sistema operativo embebido industrial, lectores láser de código de barras 1D/2D de largo alcance. | Captura in situ del `Código UA` en pallets y cajas master mono SKU (`SIS-10`). |

### 5.3.2. Software de base y motores de persistencia

1. **Motores de Persistencia y Políticas de Almacenamiento:**
   * **Capa Activa (*Hot Storage*):** Diseñada para almacenar los eventos de despachos generados en los últimos 90 días. Se configura con almacenamiento de estado sólido (SSD NVMe) y réplicas activas de lectura para proveer tiempos de respuesta inferiores a 100 milisegundos en consultas operativas.
   * **Capa Histórica (*Cold Storage*):** Transcurridos 90 días desde el cierre del pedido (estado `ENTREGADO` o `RETORNO`), los registros se trasladan mediante tareas automatizadas de particionamiento hacia depósitos de objetos de bajo costo comprimidos y firmados, conservándolos durante 5 años para efectos de auditoría legal y fiscal.
2. **Mecanismos de Serialización y Protocolos de Comunicación:**
   * Para la comunicación externa con socios logísticos: Intercambio de cargas útiles en formato JSON estándar mediante conexiones HTTPS (REST).
   * Para la comunicación interna de alto rendimiento entre componentes del MTD y el Bus: Protocolos binarios de baja sobrecarga (p. ej., AMQP, gRPC sobre HTTP/2), reduciendo la latencia de procesamiento interno a fracciones de segundo.

### 5.3.3. Topología de red, enlaces WAN y conectividad industrial

```
+----------------------------------------------------------------------------------------------------+
|                         TOPOLOGÍA DE RED Y ENLACES DE COMUNICACIONES                               |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  INSTALACIONES LURÍN                                NUBE CORPORATIVA / TI CENTRAL                  |
|  +--------------------------------+                 +--------------------------------+             |
|  | VLAN 10: Terminales RF 802.11  |                 | DMZ: API Gateway               |             |
|  | VLAN 20: Servidores SAP y SPY  | <== VPN IPsec ==| VLAN 100: Clúster MTD          |             |
|  | VLAN 30: Estaciones Operativas |    Túnel Cifrado| VLAN 200: Bus Integración      |             |
|  +--------------------------------+    (100 Mbps)   | VLAN 300: Event Store / Apps   |             |
|                                                     +--------------------------------+             |
|                                                                     ^                              |
|                                                                     | Internet Seguro              |
|                                                                     | HTTPS (TLS 1.3)              |
|                                                                     v                              |
|                                                     +--------------------------------+             |
|                                                     | REDES DE SOCIOS LOGÍSTICOS     |             |
|                                                     | * Red Celular 4G/5G en ruta    |             |
|                                                     | * Enlace HTTPS Transportistas  |             |
|                                                     +--------------------------------+             |
+----------------------------------------------------------------------------------------------------+
```

* **Segmentación de Red en Instalaciones de Lurín:** Se implementa una segregación por redes virtuales de área local (VLAN):
  * *VLAN Industrial (Terminales RF):* Red inalámbrica protegida con protocolo WPA3-Enterprise, aislada del tráfico administrativo y configurada exclusivamente para la comunicación entre los terminales `SIS-10` y los servicios de almacén.
  * *VLAN de Servidores de Producción:* Segmento de red cerrado donde residen los servidores de `SAP R3` y `SPY`, accesible únicamente desde terminales autorizados mediante listas de control de acceso (ACL).
* **Interconexión Lurín - Nube Corporativa:** Se establece a través de un túnel VPN redundante bajo protocolo IPsec con cifrado AES-256 sobre enlaces dedicados de fibra óptica, garantizando un ancho de banda simétrico no menor a 100 Mbps con contingencia automática a través de un enlace secundario.

---

## 5.4. Integración, middleware y mecanismos de comunicación

### 5.4.1. Servicios de mensajería orientada a eventos sobre el Bus Corporativo (`SIS-05`)

El `Bus de Integración` actúa como el árbitro desacoplado de las comunicaciones. Para asegurar el cumplimiento de `PAE-01` y `REQ-INT-003`, el intercambio de datos se organiza en una taxonomía de tópicos temáticos normalizados:

| Tópico Canónico | Tipo de Intercambio | Productores (*Publishers*) | Consumidores (*Subscribers*) | Propósito y Contenido |
| :--- | :---: | :--- | :--- | :--- |
| `yanbal.despacho.eventos.crudos` | Cola Asíncrona | `ADP-LOG`, `ADP-SPY`, `ADP-NSDG` | `CMP-ING-01` (Ingesta MTD) | Recepción de eventos crudos con marca $T_0$, metadatos y payload original de transportistas. |
| `yanbal.despacho.eventos.validados`| Tópico Pub/Sub | `CMP-PUB-05` (MTD) | `SIS-07` (ENSDY), `ADP-CRM`, Auditoría | Emisión de eventos normalizados en los 5 estados canónicos con sello $T_1$ y latencia calculada. |
| `yanbal.despacho.alertas.sla` | Tópico Pub/Sub | `CMP-AUD-04` (MTD) | Monitor TI (`ACT-12`), Distribución (`ACT-06`) | Difusión inmediata de eventos que exceden la ventana límite ($\Delta T > 30\text{ min}$) para auditoría de transportistas. |
| `yanbal.despacho.retornos.almacen`| Cola Dedicada | `CMP-NOT-06` (MTD) | Almacén Lurín (`ACT-03`) vía `ADP-SAP` | Mensaje estructurado de aviso de reingreso por logística inversa (sin tocar la base de datos de SAP). |
| `yanbal.despacho.errores.dlq` | *Dead Letter Queue*| Todos los componentes | Equipo de Operaciones TI (`ACT-12`) | Aislamiento de mensajes malformados o no reconocidos para análisis técnico sin bloquear la operación. |

### 5.4.2. Puerta de enlace corporativa (*API Gateway*) y gestión de interfaces de socios

Para integrar a los socios logísticos asociados (`ACT-07`), la arquitectura despliega un API Gateway en la zona desmilitarizada corporativa:
* **Punto de Ingesta Unificado (*Ingress Webhook*):** Expone un punto de enlace HTTP seguro (`POST /api/v1/logistics/events/dispatch`) donde los servidores satélites de los transportistas (como `DRIVING` `SIS-08` o plataformas propietarias de los socios) publican los eventos de ruta y entrega.
* **Control de Tráfico y Protección perimetral:** Aplica políticas de limitación de tasa (*rate limiting*) por transportista para prevenir ataques de denegación de servicio (DoS) y cuenta con un cortafuegos de aplicaciones web (*WAF*) configurado para bloquear inyecciones maliciosas.
* **Respuesta Asíncrona Inmediata:** Al recibir una carga útil válida, el API Gateway deposita el mensaje en el Bus y responde al emisor con un código `202 Accepted` acompañado del identificador unívoco de transacción (`Event_UUID`), completando la recepción en menos de 200 ms y liberando la conexión de red del transportista.

### 5.4.3. Tolerancia a intermitencias en ruta (*Store-and-Forward* — `ASM-16`, `RSK-06`)

Para cumplir con la promesa de servicio en los 24 departamentos bajo condiciones geográficas complejas (sierra y selva), la solución incorpora el patrón *Store-and-Forward*:

```
+----------------------------------------------------------------------------------------------------+
|                         PATRÓN STORE-AND-FORWARD ANTE INTERMITENCIA                                |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [DISPOSITIVO MÓVIL EN RUTA]                                [API GATEWAY / MTD CENTRAL]            |
|                                                                                                    |
|  1. Evento Operativo Ocurre In Situ                                                                |
|     (Entrega o Incidencia en Ruta)                                                                 |
|     Marca local T0 = 10:15:00 UTC                                                                  |
|                                                                                                    |
|  2. ¿Existe Cobertura Celular 4G/5G?                                                               |
|     * SI: Transmisión inmediata HTTPS ---------------------> Ingesta Inmediata Central             |
|     * NO: [Almacenamiento Local Cifrado SQLite]              Marca T1 = 10:18:00 UTC               |
|           (Persistencia con T0 y Firma Hash)                 Delta T = 3 min (<= 30 min) [OK]      |
|                                                                                                    |
|  3. Recuperación de Señal en Centro Urbano                                                         |
|     (p. ej. 45 minutos después)                                                                    |
|     Transmisión en Ráfaga de Eventos Pendientes -----------> Ingesta en Diferido                   |
|                                                              Marca T1 = 11:00:00 UTC               |
|                                                              Delta T = 45 min                      |
|                                                              * Alerta SLA registrada con justif.   |
|                                                              * Historial reordenado según T0       |
+----------------------------------------------------------------------------------------------------+
```

1. **Captura Local Offline:** El aplicativo móvil del transportista captura el evento (p. ej., `Pedido en ruta`, `Pedido entregado` o registro de incidencia), estampa de forma autónoma la marca inmutable de tiempo local ($T_0$) utilizando el reloj de hardware sincronizado por red (NTP) y calcula el hash de integridad.
2. **Buffer Local Cifrado:** Si el dispositivo carece de señal celular, el evento se encripta y se guarda en una base de datos embebida local protegida en el terminal móvil.
3. **Descarga en Ráfaga (*Burst Synchronization*):** En el instante en que el dispositivo recupera conectividad de datos (al aproximarse a un centro poblado o repetidora), el agente en segundo plano detecta la red e inicia una transmisión en ráfaga de los eventos almacenados.
4. **Tratamiento en el MTD:** El componente `CMP-ING-01` recibe el evento, `CMP-AUD-04` registra la hora de llegada $T_1$ y calcula $\Delta T$. Aunque $\Delta T$ sea superior a 30 minutos debido a la contingencia geográfica, el motor `CMP-VAL-03` reordena cronológicamente el estado del pedido basándose estrictamente en la marca original $T_0$, evitando anomalías de secuencia y registrando la justificación de conectividad para no penalizar indebidamente al socio logístico.

---

## 5.5. Seguridad técnica, cifrado, disponibilidad y rendimiento

### 5.5.1. Controles exigidos por Seguridad Informática (`ACT-11`, `REQ-RNF-003`, `OBJ-07`)

En estricto cumplimiento del principio `PAE-05` y los requisitos de calidad institucionales, la arquitectura tecnológica integra controles de seguridad en cada una de sus capas:
* **Evaluación de Vulnerabilidades:** Todos los componentes de software desarrollados y las imágenes de contenedores deben someterse a análisis estático de código (SAST) y análisis dinámico (DAST), estableciendo como criterio de pase la existencia de cero vulnerabilidades críticas o altas no mitigadas.
* **Superficie de Ataque Reducida:** Ningún servidor interno de base de datos ni los nodos de procesamiento de Lurín exponen puertos directamente hacia internet; toda comunicación pasa obligatoriamente por la zona DMZ.

### 5.5.2. Cifrado en tránsito y en reposo

* **Cifrado de Datos en Tránsito:** Todas las conexiones externas e inter-servicios se ejecutan mediante el protocolo **Transport Layer Security versión 1.3 (TLS 1.3)**, deshabilitando versiones obsoletas o suites criptográficas débiles. Se exige el uso de cifrado perfecto hacia adelante (*Forward Secrecy*).
* **Cifrado de Datos en Reposo:** Los volúmenes de almacenamiento del repositorio de trazabilidad (`DB-EVENTSTORE`), las colas persistentes del Bus de Integración y los backups históricos aplican cifrado simétrico mediante el estándar **AES-256** gestionado mediante claves criptográficas corporativas rotadas periódicamente.

### 5.5.3. Esquema de autenticación, autorización y segregación de roles (RBAC)

Para interactuar con la plataforma de trazabilidad, se establecen esquemas diferenciados según la naturaleza del actor:
1. **Autenticación entre Sistemas y Socios Logísticos (Machine-to-Machine):**
   * *Transportistas Externos (`ACT-07`):* Autenticación mediante certificados digitales mutuos (**mTLS**) combinados con tokens de corta duración bajo estándar **OAuth 2.0 / JSON Web Tokens (JWT)** firmados con algoritmo asimétrico (RS256).
   * *Sistemas Satélites Internos:* Credenciales de servicio rotativas con acceso limitado a tópicos específicos del Bus.
2. **Matriz de Control de Acceso Basado en Roles (RBAC):**

| Nivel de Rol | Actores Comprendidos | Alcance de Permisos Tecnológicos en el Ecosistema | Interfaces y Sistemas Habilitados |
| :--- | :--- | :--- | :--- |
| **Nivel Operativo** | `ACT-03`, `ACT-04`, `ACT-05`, `ACT-07`, `ACT-10` | Ejecución de transacciones asignadas; captura de eventos in situ; lectura restringida a su tramo operativo. | `SIS-10` (RF Almacén), `SIS-02` (SPY), App Móvil de Transporte, Terminal Despacho. |
| **Nivel Coordinación**| Supervisores de Almacén, Distribución y Atención | Consulta operativa de estados consolidados; recepción de alertas tempranas de desviación temporal. | Consola de Monitoreo de Despacho, Interfaces de `ENSDY` (`SIS-07`). |
| **Nivel Gestión** | `ACT-06` (Distribución), `ACT-08` (Servicio Cliente), `ACT-12` (TI) | Visualización integral de métricas de desempeño; auditoría de SLAs por transportista; parametrización de reglas de negocio. | `CELLFORCE` (`SIS-09`), Paneles de Control de Trazabilidad, Consola de API Gateway. |
| **Nivel Directivo** | `ACT-13` (Roles Directivos) | Acceso a reportes ejecutivos consolidados, indicadores estratégicos de entrega y costos operativos. | Módulos de Inteligencia de Negocio y Reportes Ejecutivos. |
| **Nivel Seguridad / Gobierno** | `ACT-11` (Seguridad Informática), `ACT-14`, `ACT-15` | Auditoría de trazas criptográficas; inspección de vulnerabilidades; control de cumplimiento normativo y legal. | Repositorio de Logs Inmutables de Seguridad, Plataformas SIEM corporativas. |

### 5.5.4. Auditoría inmutable, sellado temporal dual y prevención de duplicados

Para respaldar el requerimiento `REQ-ARQ-003` y los principios de gobernanza:
* **Sellado Temporal Dual Obligatorio:** Todo registro persistido almacena de forma inmutable la marca $T_0$ (instante de ocurrencia física) y la marca $T_1$ (instante de ingesta en el MTD), ambas en formato UTC estándar ISO 8601.
* **Firma Criptográfica de Idempotencia:** El componente `CMP-ING-01` genera para cada mensaje entrante una firma hash SHA-256 compuesta por:
  $$\text{Hash} = \text{SHA-256}(\text{Numero\_Pedido} \parallel \text{Estado\_Ciclo} \parallel T_0 \parallel \text{Codigo\_Emisor})$$
  Si ingresa un mensaje con un hash ya registrado en la ventana móvil de 72 horas, el motor confirma la recepción pero descarta el procesamiento repetido, protegiendo las bases de datos contra duplicaciones generadas por reintentos de red.

### 5.5.5. Disponibilidad, resiliencia y dimensionamiento ante picos de campaña

* **Acuerdo de Nivel de Servicio (SLA de Infraestructura):** La infraestructura de cómputo y middleware del `MTD` se dimensiona para proveer una disponibilidad operativa no menor al **99.9%** durante los días laborales regulares y del **99.99%** durante los cierres de campaña comercial de Yanbal.
* **Resiliencia ante Caídas del Middleware:** En caso de indisponibilidad temporal del Bus de Integración, los componentes de ingesta almacenan los mensajes entrantes en discos locales persistentes (*local spooling*), asegurando cero pérdida de datos.
* **Dimensionamiento Técnico:** Respetando la decisión `ASM-10`, la capacidad de procesamiento no se deriva del volumen estático de 2 millones de cajas de almacenamiento físico de Lurín. La arquitectura prevé un clúster de cómputo elástico capaz de escalar dinámicamente desde un estado base de baja carga transaccional hasta picos estimados de alta concurrencia en días pico de facturación nacional.

---

## 5.6. Diagrama de despliegue UML (`DIAG-07`)

### 5.6.1. Ficha técnica y propósito del modelo

* **Identificador Canónico:** `DIAG-07`
* **Denominación Oficial:** Diagrama UML de Despliegue de la Arquitectura Tecnológica Objetivo (TO-BE)
* **Estándar de Modelado:** OMG Unified Modeling Language (OMG UML) versión 2.5.1
* **Propósito Metodológico:** Representar formalmente la topología física y lógica de los nodos de cómputo, dispositivos, entornos de red y middleware que sustentan la solución TO-BE, evidenciando los límites de responsabilidad organizacional, los canales de integración y el aislamiento estricto de los sistemas críticos de la compañía.
* **Archivo Fuente Editable:** `05_Fase_D_Arquitectura_Tecnologica/diagramas/DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml`

### 5.6.2. Especificación tabular de nodos, artefactos, protocolos y responsabilidades

| Nodo de Despliegue | Artefactos / Componentes Hospedados | Interfaz / Protocolo de Conexión | Nivel de Responsabilidad y Actor |
| :--- | :--- | :--- | :--- |
| **Instalaciones Industriales y Almacén Lurín** | `SIS-01: SAP R3`<br>`ADP-SAP` (Conector RFC)<br>`SIS-02: SPY`<br>`ADP-SPY`<br>`SIS-10: Terminales RF` | * Interna: WiFi WPA3 / Telnet<br>* Inter-servidor: LAN 1 Gbps<br>* Salida: VPN IPsec (TLS 1.3) | **Responsabilidad Operativa y Física:**<br>Exclusivo de Almacén Lurín (`ACT-03`) y Picking (`ACT-04`). Restricción `REQ-RES-002` aplicada: sin escrituras externas en SAP. |
| **Nube Corporativa / Clúster MTD** | `API Gateway Corporativo`<br>`CMP-ING-01` a `CMP-NOT-06`<br>`DB-EVENTSTORE` (Canónico)<br>`SIS-05: Bus de Integración` | * Ingesta: HTTPS / TLS 1.3<br>* Bus: AMQP / Pub-Sub cifrado<br>* DB: JDBC sobre TLS 1.3 | **Responsabilidad de Integración y TI:**<br>Área de TI y Arquitectura (`ACT-12`) con auditoría de Seguridad Informática (`ACT-11`). Capacidad propuesta (`DEC-02`). |
| **Servidores de Aplicaciones Corporativas** | `SIS-03: Maya`<br>`SIS-04: ZapCommerce`<br>`SIS-07: ENSDY`<br>`SIS-09: CELLFORCE`<br>`ADP-CRM` | * Interna: Pub/Sub vía Bus `SIS-05`<br>* Salida: HTTPS hacia clientes web | **Responsabilidad Comercial y de Servicio:**<br>Área Comercial y Área de Servicio al Cliente (`ACT-08`). Acceso a estados actualizados en $\le 30\text{ min}$. |
| **Infraestructura de Socios Logísticos** | `SIS-08: DRIVING`<br>`SIS-06: NSDG`<br>`ADP-LOG`<br>App Móvil con Buffer *Store-and-Forward* | * Red Móvil: 4G/5G HTTPS<br>* Gateway: mTLS / OAuth 2.0<br>* Local: SQLite cifrado offline | **Responsabilidad Externa Tercerizada:**<br>Socios Logísticos Asociados (`ACT-07`). No existe flota vehicular propia de Yanbal (`REQ-RES-004`). |
| **Destinatarios Empresariales B2B** | Navegador Web / Terminal de Consulta de Pedidos | * Conexión: HTTPS (TLS 1.3)<br>* Proxy inverso vía API Gateway | **Destinatarios del Despacho:**<br>Distribuidoras y Sedes Empresariales (`ACT-09`). Consulta de estados y promesas de entrega B2B (`REQ-FUN-002`). |

### 5.6.3. Representación visual integrada del modelo de despliegue (`DIAG-07`)

A continuación se presenta el modelo conceptual de despliegue mediante diagramación Mermaid para su visualización inmediata en entornos de documentación:

```mermaid
flowchart TD
    %% ESTILOS VISUALES
    classDef lurinNode fill:#EBF3FB,stroke:#1B365D,stroke-width:2px;
    classDef cloudNode fill:#E8F5E9,stroke:#2E7D32,stroke-width:2px;
    classDef partnerNode fill:#FFF3E0,stroke:#E65100,stroke-width:2px;
    classDef clientNode fill:#F3E5F5,stroke:#4A148C,stroke-width:2px;
    classDef boundary fill:#FFFFFF,stroke:#666666,stroke-dasharray: 5 5;

    %% NODO ON-PREMISE LURÍN
    subgraph NODE_LURIN ["Instalaciones Industriales y Almacén Lurín (On-Premise)"]
        direction TB
        DEV_RF["Terminales RF (SIS-10)\n[Lectura Código UA]"]:::lurinNode
        SRV_SAP["Servidor SAP R3 (SIS-01)\n[Operación Exclusiva ACT-03 (REQ-RES-002)]\n+ Adaptador RFC Solo Lectura (ADP-SAP)"]:::lurinNode
        SRV_SPY["Servidor BWMS SPY (SIS-02)\n[Picking, Balanzas y Cajas 1-8]\n+ Agente Emisor Estado 2 (ADP-SPY)"]:::lurinNode
        
        DEV_RF -->|WiFi WPA3| SRV_SAP
        SRV_SAP -.->|Sincronización Stock PT| SRV_SPY
    end

    %% NODO NUBE CORPORATIVA / MTD
    subgraph NODE_CLOUD ["Nube Corporativa Yanbal - Plataforma de Integración y MTD"]
        direction TB
        APIGW["API Gateway Corporativo\n[TLS 1.3 | mTLS | OAuth 2.0 | WAF]"]:::cloudNode
        
        subgraph CLUSTER_MTD ["Clúster de Microservicios MTD (DEC-02)"]
            CMP_ING["CMP-ING-01: Ingesta e Idempotencia"]:::cloudNode
            CMP_COR["CMP-COR-02: Correlación Lógica (UA-Picking-Pedido)"]:::cloudNode
            CMP_VAL["CMP-VAL-03: Validación 5 Estados Canónicos"]:::cloudNode
            CMP_AUD["CMP-AUD-04: Auditoría Temporal (T0, T1, Delta T)"]:::cloudNode
            CMP_PUB["CMP-PUB-05: Despachador de Publicación"]:::cloudNode
            CMP_NOT["CMP-NOT-06: Gestión Retornos a Almacén"]:::cloudNode
        end
        
        BUS_INT["Bus de Integración Corporativo (SIS-05)\n[Tópicos Pub/Sub de Despacho B2B]"]:::cloudNode
        DB_STORE[("Event Store Canónico (ENT-07)\n[Hot Data <90d | Cold Archive]")]:::cloudNode
        
        subgraph APPS_CORP ["Aplicaciones Corporativas Yanbal"]
            APP_MAYA["Maya (SIS-03) / ZapCommerce (SIS-04)"]:::cloudNode
            APP_ENSDY["ENSDY (SIS-07) [Trackeo de Órdenes]"]:::cloudNode
            APP_CRM["CELLFORCE (SIS-09) [CRM Atención ACT-08]\n+ Adaptador ADP-CRM"]:::cloudNode
        end
        
        APIGW -->|Enrutamiento Interno| CMP_ING
        CMP_ING --> CMP_AUD
        CMP_ING --> CMP_VAL
        CMP_VAL <--> CMP_COR
        CMP_VAL --> DB_STORE
        CMP_VAL --> CMP_PUB
        CMP_PUB -->|Publicación Evento Validado| BUS_INT
        CMP_VAL -.->|Alerta Incidencia| CMP_NOT
        
        BUS_INT --> APP_ENSDY
        BUS_INT --> APP_CRM
        APP_MAYA -->|Orden Aprobada| BUS_INT
    end

    %% NODO EXTERNO SOCIOS LOGÍSTICOS
    subgraph NODE_LOGISTICS ["Infraestructura Externa - Socios Logísticos (ACT-07)"]
        direction TB
        SRV_TMS["Plataformas TMS Socios (DRIVING SIS-08 / NSDG SIS-06)\n+ Adaptador Webhook Transportista (ADP-LOG)"]:::partnerNode
        DEV_MOBILE["Dispositivos Móviles en Ruta\n[App con Buffer Store-and-Forward Offline (ASM-16)]"]:::partnerNode
        
        DEV_MOBILE -->|Red Celular 4G/5G| SRV_TMS
    end

    %% NODO DESTINATARIOS EMPRESARIALES
    subgraph NODE_DEST ["Destinatarios Empresariales B2B (ACT-09)"]
        BROWSER["Portal Web de Consulta B2B\n[Estatus Actualizado en <= 30 min]"]:::clientNode
    end

    %% CONEXIONES INTER-NODOS (TOPOLOGÍA DE RED)
    SRV_SPY ==>|Túnel VPN IPsec Cifrado| BUS_INT
    CMP_NOT -.->|Notificación Retorno (Sin tocar SAP)| SRV_SAP
    
    SRV_TMS ==>|Internet Seguro HTTPS / TLS 1.3 (mTLS)| APIGW
    BROWSER ==>|HTTPS TLS 1.3| APIGW
    APIGW -.->|Proxy Consulta| APP_ENSDY
```

### 5.6.4. Análisis técnico del diagrama de despliegue

1. **Aislamiento Físico y Lógico de `SAP R3` (`REQ-RES-002`):**  
   El diagrama evidencia que no existe ningún enlace de red ni canal de comunicación de escritura que penetre desde el exterior o desde los componentes del `MTD` hacia la base de datos de `SAP R3`. En los eventos de logística inversa, el componente `CMP-NOT-06` deposita una notificación de aviso en el Bus; el adaptador `ADP-SAP` presenta la alerta en la consola de Almacén Lurín para que sea el operador humano autorizado (`ACT-03`) quien ejecute manualmente la regularización en el ERP.
2. **Desacoplamiento mediante el Bus Corporativo (`SIS-05`):**  
   Los sistemas de seguimiento (`ENSDY`) y de servicio al cliente (`CELLFORCE`) no consultan directamente las bases de datos de empaque ni los servidores de los transportistas. Ambos se comportan como suscriptores pasivos del tópico canónico de despacho, garantizando que cuando un evento es procesado por el MTD, la actualización se propague en simultáneo hacia todos los canales de consulta.
3. **Frontera de Tercerización de Transporte (`REQ-RES-004`):**  
   El nodo de transportistas se modela estrictamente como infraestructura externa de terceros. La comunicación entre los camiones en ruta y Yanbal se efectúa a través de los servidores de los socios logísticos (`ACT-07`), eliminando cualquier presunción de que Yanbal administre hardware de posicionamiento global propio.

---

## 5.7. Estándares tecnológicos, análisis de brechas y componentes candidatos

### 5.7.1. Estándares tecnológicos: Confirmados vs. Propuestos

En concordancia con el principio `PAE-08`, la siguiente tabla clasifica los estándares y tecnologías aplicables a la arquitectura tecnológica, distinguiendo aquello que se encuentra confirmado en la operación frente a las propuestas que requieren validación técnica con TI (`ACT-12`) y Seguridad Informática (`ACT-11`):

| Dominio Tecnológico | Estándar o Tecnología | Categoría / Estado | Fundamento y Regla de Aplicación |
| :--- | :--- | :---: | :--- |
| **Comunicaciones en Red** | Protocolo TLS versión 1.3 | **Propuesto (Pendiente `ACT-11`)** | Exigencia para todo canal de transporte de datos público o inter-nube (`REQ-RNF-003`). Sustituye versiones obsoletas. |
| **Seguridad de Interfaces**| Autenticación mutua mTLS + JWT | **Propuesto (Pendiente `ACT-11`)** | Estándar recomendado para la autenticación entre plataformas de transportistas y el API Gateway (`OBJ-07`). |
| **Formato de Carga Útil** | Estructura JSON Canónica (ISO 8601) | **Propuesto (Pendiente `ACT-12`)** | Formato universal para la representación de eventos de trazabilidad (`ENT-07`) con marcas $T_0$ y $T_1$ (`REQ-ARQ-003`). |
| **Intercambio Middleware** | Modelo Publish/Subscribe sobre Bus | **Confirmado en Operación** | El uso del Bus de Integración (`SIS-05`) está confirmado en la empresa; se formaliza su uso en tópicos desacoplados (`PAE-01`). |
| **Conectividad Industrial** | WiFi 802.11 Enterprise (WPA3) | **Propuesto (Pendiente `ACT-12`)** | Protocolo de conectividad inalámbrica para terminales de radiofrecuencia en planta (`SIS-10`), aislado por VLAN. |
| **Persistencia Offline** | Almacenamiento local cifrado SQLite | **Propuesto (Pendiente `ACT-07`)** | Estándar técnico propuesto para habilitar el patrón *Store-and-Forward* en dispositivos móviles de transportistas (`ASM-16`). |
| **ERP y Datos Maestros** | Protocolos propietarios RFC / BAPI | **Confirmado en Operación** | Protocolo existente de comunicación hacia `SAP R3` (`SIS-01`), limitado a consultas de solo lectura (`REQ-RES-002`). |

### 5.7.2. Matriz formal de análisis de brechas tecnológicas (Gap Analysis AS-IS vs. TO-BE)

La comparación formal entre la línea base tecnológica y la arquitectura propuesta arroja las siguientes brechas de infraestructura y plataformas:

| Código Brecha | Capacidad Tecnológica Analizada | Situación Actual (AS-IS) | Arquitectura Objetivo (TO-BE) | Naturaleza de la Brecha | Acción de Evolución Requerida |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **`GAP-TEC-01`** | **Perímetro de Ingesta Segura para Socios Logísticos** | No existe puerta de enlace centralizada; integraciones dispersas y no normalizadas con transportistas. | API Gateway corporativo en DMZ con soporte TLS 1.3, mTLS y validación de tokens de seguridad. | **Faltante (Nuevo)** | Adquirir o configurar un componente de API Gateway gestionado dentro de la infraestructura corporativa de Yanbal. |
| **`GAP-TEC-02`** | **Estandarización y Normalización de Eventos** | Cargas de datos heterogéneas sin marcas de tiempo $T_0/T_1$ ni control criptográfico de duplicados. | Servicio de normalización e idempotencia (`CMP-ING-01`) que estampa $T_1$, valida SHA-256 y descarta duplicados. | **Faltante (Nuevo)** | Desarrollar y desplegar el microservicio de ingesta e idempotencia sobre la plataforma de contenedores. |
| **`GAP-TEC-03`** | **Persistencia Canónica de Trazabilidad** | Datos de despacho fragmentados entre `SPY`, `NSDG`, `DRIVING` y logs de seguimiento desactualizados. | Repositorio centralizado de eventos (`DB-EVENTSTORE`) con partición operativa caliente (<90d) y archivo frío. | **Faltante (Nuevo)** | Provisionar una base de datos de alta velocidad de escritura optimizada para series de eventos inmutables. |
| **`GAP-TEC-04`** | **Tolerancia a Conectividad en Rutas Nacionales** | Aplicaciones móviles de transporte dependientes de cobertura celular continua; pérdida o retraso de eventos. | Protocolo *Store-and-Forward* en apps móviles con buffer cifrado local y reordenamiento automático por $T_0$. | **Limitación (Modificar)** | Coordinar con los socios logísticos (`ACT-07`) la incorporación de la lógica de almacenamiento offline en sus apps. |
| **`GAP-TEC-05`** | **Canal Asíncrono de Logística Inversa hacia Almacén** | El reporte de entregas fallidas se maneja mediante coordinaciones telefónicas o manuales entre áreas. | Canal de eventos de retorno en el Bus (`CMP-NOT-06`) con aviso estructurado a `ACT-03` sin alterar `SAP R3`. | **Faltante (Nuevo)** | Configurar el tópico de notificación y la interfaz en consola para Almacén Lurín sin escritura en el ERP. |
| **`GAP-TEC-06`** | **Monitoreo Automatizado de Latencia de SLA** | Medición manual y reactiva basada en quejas de clientes; desconocimiento del retraso real del transportista. | Componente de telemetría (`CMP-AUD-04`) que calcula $\Delta T = T_1 - T_0$ en tiempo real y emite alertas telemáticas. | **Faltante (Nuevo)** | Instrumentar el motor de cálculo de latencia e integrar tableros de control para el Área de Distribución (`ACT-06`). |

### 5.7.3. Componentes tecnológicos candidatos y matriz de dependencias

Para materializar la arquitectura TO-BE, se identifican los siguientes bloques constructivos tecnológicos candidatos (*Technology Building Blocks* - TBB):

```
+----------------------------------------------------------------------------------------------------+
|                         MAPA DE DEPENDENCIAS DE COMPONENTES CANDIDATOS                             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [TBB-01: API Gateway DMZ] <----------- Requiere validación de políticas de seguridad (ACT-11)    |
|         |                                                                                          |
|         v Ingesta de tráfico seguro                                                                |
|  [TBB-02: Clúster Contenedores MTD] <--- Despliega microservicios CMP-ING-01 a CMP-NOT-06          |
|         |                     |                                                                    |
|         | Publica eventos     | Persiste histórico                                                 |
|         v                     v                                                                    |
|  [TBB-03: Event Broker SIS-05]    [TBB-04: Repositorio EventStore]                                 |
|         |                                                                                          |
|         +---> Sincroniza hacia: [TBB-05: Adaptador CRM CELLFORCE]                                   |
|         +---> Notifica hacia:   [TBB-06: Conector Seguro Almacén Lurín ACT-03]                     |
+----------------------------------------------------------------------------------------------------+
```

| Código TBB | Componente Candidato | Tipo de Elemento | Dependencia Técnica Principal | Dependencia Organizacional |
| :---: | :--- | :--- | :--- | :--- |
| **`TBB-01`** | **API Gateway Corporativo** | Infraestructura / DMZ | Conectividad a internet y certificados TLS corporativos. | Aprobación de configuración por Seguridad Informática (`ACT-11`). |
| **`TBB-02`** | **Clúster de Contenedores MTD** | Plataforma de Cómputo | Repositorio corporativo de imágenes y motor de orquestación. | Aprovisionamiento de infraestructura por el Área de TI (`ACT-12`). |
| **`TBB-03`** | **Event Broker del Bus (`SIS-05`)** | Middleware de Integración | Configuración de tópicos y particiones con soporte de persistencia. | Coordinación técnica de interfaces con administradores del Bus. |
| **`TBB-04`** | **Repositorio Canónico de Eventos**| Base de Datos de Eventos | Almacenamiento rápido en disco y políticas de backup continuo. | Definición de políticas de retención con el Área Legal y TI. |
| **`TBB-05`** | **Adaptador CRM `CELLFORCE`** | Conector de Aplicación | Disponibilidad de API REST o webhook en `CELLFORCE` (`SIS-09`). | Coordinación con el equipo de soporte de Servicio al Cliente (`ACT-08`). |
| **`TBB-06`** | **Conector Seguro Almacén Lurín** | Interfaz Operativa | Enlace VPN IPsec activo entre la nube corporativa y Lurín. | Validación de procedimientos de recepción con Almacén (`ACT-03`). |

---

## 5.8. Justificación técnica y económica, impacto y transición

### 5.8.1. Justificación técnica de las soluciones propuestas

La arquitectura tecnológica seleccionada se fundamenta en los siguientes criterios de ingeniería:
1. **Preservación del Núcleo Transaccional frente a Reingeniería Total:**  
   Frente a la alternativa de reemplazar el ERP `SAP R3` o construir un nuevo sistema monolítico de distribución —lo cual demandaría inversiones multimillonarias y riesgos operativos severos para el negocio—, la solución de una plataforma híbrida de integración reutiliza los activos existentes (`PAE-02`, `REQ-ARQ-001`) y resuelve el problema en la capa de integración.
2. **Procesamiento Asíncrono Orientado a Eventos frente a Sondeo Periódico (*Polling*):**  
   El modelo tradicional de consultar periódicamente las bases de datos de los transportistas mediante procesos batch satura los enlaces WAN y garantiza desfases informativos. El modelo Pub/Sub propuesto reacciona instantáneamente ante la llegada del evento, reduciendo el consumo de ancho de banda y haciendo viable la meta de $\Delta T \le 30\text{ minutos}$.
3. **Desacoplamiento Estricto frente a Integraciones Punto a Punto:**  
   La incorporación del API Gateway y el Bus previene la creación de una "arquitectura espagueti", permitiendo añadir, cambiar o sustituir proveedores de transporte (`ACT-07`) en el futuro sin modificar los sistemas centrales de Yanbal.

### 5.8.2. Criterios para la evaluación económica preliminar

Aunque el dimensionamiento financiero detallado corresponde a las Fases E y F de TOGAF ADM, la arquitectura tecnológica incorpora criterios de optimización de costo total de propiedad (*Total Cost of Ownership* - TCO):
* **Modelo Basado en Consumo:** El clúster de cómputo en contenedores permite escalar a cero o a un nivel mínimo de recursos durante los horarios nocturnos de baja actividad física de transporte, incrementando nodos únicamente durante las ventanas diurnas y picos de campaña.
* **Aprovechamiento de Licencias y Middleware Existente:** La solución se ancla sobre el `Bus de Integración` (`SIS-05`) ya licenciado y operativo en la empresa, evitando la duplicidad de costos de plataforma.
* **Reducción de Sobrecarga Operativa:** Al eliminar la doble digitación manual en la Zona de Despacho (`REQ-RNF-004`) y dotar a Servicio al Cliente de información inmediata, se reduce drásticamente el tiempo promedio de atención de llamadas en `CELLFORCE`.

### 5.8.3. Dependencias con las fases subsecuentes de TOGAF ADM

* **Fase E (Oportunidades y Soluciones):** La definición de los componentes tecnológicos candidatos (`TBB-01` a `TBB-06`) y la matriz de brechas (`GAP-TEC-01` a `GAP-TEC-06`) constituyen el insumo indispensable para estructurar los Paquetes de Trabajo (`PKG-XX`) y seleccionar las alternativas definitivas de implementación.
* **Fase F (Planificación de la Migración):** El despliegue de la infraestructura se programará de manera escalonada a través de Arquitecturas de Transición, priorizando primero la interconexión de Lurín con el API Gateway antes de la integración con los socios logísticos externos.
* **Fase G (Gobierno de la Implementación):** Los estándares de cifrado (TLS 1.3), segregación por roles (RBAC) y aislamiento de `SAP R3` formarán parte vinculante del Contrato de Arquitectura que supervisará el Architecture Board.
* **Fase H (Gestión del Cambio de Arquitectura):** Los indicadores de latencia generados por `CMP-AUD-04` alimentarán los tableros de monitoreo continuo de SLA en producción.

### 5.8.4. Matriz de gestión temprana de riesgos tecnológicos (`RSK-01` a `RSK-06`)

La siguiente matriz documenta cómo la Arquitectura Tecnológica mitiga tempranamente los riesgos formalizados en el registro maestro:

| Código Riesgo | Descripción del Riesgo Tecnológico | Efecto Potencial | Mitigación Arquitectónica Implementada en Fase D | Estado |
| :---: | :--- | :--- | :--- | :---: |
| **`RSK-01`** | Imposibilidad de medir la línea base real del desfase por falta de marcas temporales. | Metas de latencia no auditables o mal dimensionadas. | Se instrumenta la captura obligatoria de $T_0$ en emisores y el sellado automático $T_1$ en `CMP-AUD-04` (`REQ-ARQ-003`). | Mitigado en Diseño |
| **`RSK-02`** | El desfase se origina en la lentitud de reporte de socios logísticos externos. | Fracaso en alcanzar la meta de 30 minutos a pesar de mejoras internas. | API Gateway público con endpoints REST de baja latencia y monitoreo telemático de SLA por transportista. | Mitigado en Diseño |
| **`RSK-03`** | Integraciones erróneas por confusión entre `NSDG`, `ENSDY` y `DRIVING`. | Duplicación de interfaces o componentes innecesarios. | Se aíslan los sistemas mediante adaptadores ACL específicos (`ADP-LOG`, `ADP-NSDG`), pendientes de ratificar en Fase E. | Mitigado en Diseño |
| **`RSK-04`** | Rechazo o demoras por parte de Seguridad Informática corporativa. | Retrasos críticos en la autorización de despliegue. | Inclusión de controles de seguridad desde el diseño base (TLS 1.3, mTLS, RBAC, WAF, segregación VLAN, `OBJ-07`). | Mitigado en Diseño |
| **`RSK-05`** | Invasión del alcance fuera de la frontera B2B hacia consumidoras finales. | Sobredimensionamiento y complejidad de interfaces. | Delimitación estricta de la red B2B; las interfaces de consulta se limitan a códigos de clientes y pedidos mayoristas. | Mitigado en Diseño |
| **`RSK-06`** | Pérdida de eventos por intermitencia de red celular en rutas nacionales remotas. | Desactualización de seguimiento en trayectos interprovinciales. | Incorporación del patrón *Store-and-Forward* con buffer local cifrado y reordenamiento cronológico por $T_0$. | Mitigado en Diseño |

### 5.8.5. Trazabilidad hacia la Matriz Maestra de Requerimientos

La Arquitectura Tecnológica desarrollada en la presente fase da cumplimiento directo a los siguientes requisitos de la Matriz Maestra:

| Requisito Oficial | Enunciado Sintético | Mecanismo de Cobertura Tecnológica en la Fase D |
| :--- | :--- | :--- |
| **`REQ-INT-001`** | Sincronización de stock y despacho entre SAP, SPY y transporte. | Adaptadores `ADP-SAP` y `ADP-SPY` conectados mediante LAN local y enlaces seguros hacia el Bus. |
| **`REQ-INT-002`** | Integración de eventos de socios logísticos en $\le 30\text{ min}$. | API Gateway corporativo con ingesta REST asíncrona y tolerancia a intermitencia *Store-and-Forward*. |
| **`REQ-INT-003`** | Conexión desacoplada mediante Bus de Integración corporativo. | Publicación y suscripción sobre tópicos temáticos en `SIS-05`, eliminando conexiones punto a punto. |
| **`REQ-INT-004`** | Compatibilidad con 3 sistemas adaptados y 4 de proveedores. | Conservación de todos los sistemas de la línea base mediante una capa adaptadora (*Anti-Corruption Layer*). |
| **`REQ-RNF-001`** | Latencia de actualización máxima permisible de 30 minutos. | Procesamiento en memoria de eventos, enrutamiento asíncrono y componentes sin estado elásticos. |
| **`REQ-RNF-002`** | Capacidad para grandes volúmenes de datos y picos de campaña. | Clúster de contenedores autoescalable horizontalmente y repositorio de eventos de alta velocidad. |
| **`REQ-RNF-003`** | Validación por Seguridad Informática y control por roles. | Cifrado TLS 1.3, autenticación mTLS/JWT, segregación de VLANs y matriz RBAC por niveles de permiso. |
| **`REQ-RNF-004`** | Preservación de la facilidad de uso sin doble digitación manual. | Agentes automáticos en terminales de despacho y estaciones de báscula que capturan eventos de fondo. |
| **`REQ-ARQ-001`** | Diseño del MTD como capacidad integrada sin duplicar sistemas. | Despliegue del MTD como microservicios de soporte y no como una base de datos o ERP redundante. |
| **`REQ-ARQ-003`** | Estructura canónica de eventos con marcas temporales $T_0$ y $T_1$. | Especificación del esquema de datos canónico y componente de sellado temporal `CMP-AUD-04`. |
| **`REQ-RES-001`** | Frontera de alcance delimitada estrictamente al despacho B2B. | Configuración de endpoints y reglas de negocio acotadas a pedidos hacia distribuidoras y sedes. |
| **`REQ-RES-002`** | Operación exclusiva de `SAP R3` por el Área de Almacén. | Aislamiento físico y lógico de red; ninguna interfaz del MTD escribe en la base de datos del ERP. |
| **`REQ-RES-004`** | Transporte ejecutado por terceros sin flota propia con GPS. | Nodo de socios logísticos modelado como infraestructura externa; seguimiento basado en eventos operativos. |

---

## Referencias Bibliográficas (Norma APA 7.ª Edición)

* Drivin. (s. f.). *Yanbal Perú: 100% de visibilidad para su logística*. Recuperado de https://driv.in/casos-de-exito/yanbal-peru
* Institute of Electrical and Electronics Engineers. (2018). *ISO/IEC/IEEE 29148:2018 Systems and software engineering — Life cycle processes — Requirements engineering*. IEEE Standards Association. https://standards.ieee.org/ieee/29148/7212/
* Object Management Group. (2017). *OMG Unified Modeling Language (OMG UML) Specification, Version 2.5.1*. Object Management Group. https://www.omg.org/spec/UML/2.5.1/
* Rescorla, E. (2018). *The Transport Layer Security (TLS) Protocol Version 1.3* (RFC 8446). Internet Engineering Task Force (IETF). https://doi.org/10.17487/RFC8446
* The Open Group. (2018). *The TOGAF® Standard, Version 9.2*. Van Haren Publishing. https://www.opengroup.org/togaf
* The Open Group. (2022). *The TOGAF® Standard, 10th Edition — Architecture Development Method (ADM)*. Van Haren Publishing. https://www.opengroup.org/togaf-standard-10th-edition
* Unique S.A. / Yanbal Perú. (s. f.-a). *Nuestra Historia corporativa*. Portal oficial de Yanbal Perú. Recuperado de https://www.yanbal.com/pe/corporate/nuestra-historia
* Unique S.A. / Yanbal Perú. (s. f.-b). *Instalaciones y Canales de Contacto Oficiales*. Portal corporativo de Yanbal Perú. Recuperado de https://www.yanbal.com/pe/corporate/contactanos

