# 04.0. NOMENCLATURA PROPUESTA Y CONVENCIONES PARA LA FASE C
## Catálogo de Identificadores Candidatos para Arquitectura de Datos y Arquitectura de Aplicaciones

> **Ubicación:** [`04_Fase_C_Arquitectura_de_Sistemas_de_Informacion/04_0_Nomenclatura_Propuesta_Fase_C.md`](04_0_Nomenclatura_Propuesta_Fase_C.md)  
> **Estado de gobernanza:** Propuesta formal de Fase C (Documento complementario y autónomo)  
> **Principio de inmutabilidad:** Este documento define los identificadores y convenciones técnicas requeridos para el modelado de datos y aplicaciones de la Fase C sin alterar ni reescribir el glosario maestro general [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md), el cual permanece intacto en modo solo lectura.

---

## 1. Convenciones y Taxonomía de Prefijos para la Fase C

Para estructurar con rigor metodológico el modelado de la Arquitectura de Sistemas de Información, se establecen cuatro taxonomías de identificadores formales:

1. **`ENT-XX` (Entidades Lógicas de Negocio y Datos):** Identifica de manera unívoca cada objeto o entidad conceptual/lógica en el dominio de preparación, distribución, transporte, trazabilidad y logística inversa.
2. **`CMP-XX` (Componentes Lógicos de Software del MTD):** Identifica los módulos de software desacoplados que conforman el Mecanismo de Trazabilidad del Despacho como capacidad integrada sobre el `Bus de Integración` (`SIS-05`).
3. **`ADP-XX` (Adaptadores de Integración de Sistemas Legados y de Proveedores):** Identifica los componentes de software especializados en la capa de adaptación y mediación (*Anti-Corruption Layer*) que comunican el bus con los sistemas propios de Yanbal y los sistemas satélites de terceros.
4. **`INT-DAT-XX` (Interfaces y Contratos de Intercambio de Datos):** Identifica los contratos canónicos de mensaje y esquemas de carga útil (*payloads*) que viajan a través del middleware corporativo.

---

## 2. Catálogo Propuesto de Entidades Lógicas de Datos (`ENT-XX`)

| Código | Nombre Canónico de la Entidad | Dominio Funcional | Sistema de Registro (*System of Record*) | Descripción y Propósito en la Arquitectura de Datos | Requerimientos Asociados |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`ENT-01`** | **`Pedido_Comercial`** | Comercial / Despacho | `Maya` (`SIS-03`) / `ZapCommerce` (`SIS-04`) | Representa la orden de compra consolidada emitida por una consultora o directora independiente (`ACT-09`). Contiene identificador único (`Numero_Pedido`), cliente, dirección de destino normalizada, canal y promesa de entrega. | `REQ-NEG-001`, `REQ-FUN-001`, `REQ-FUN-002`, `REQ-RES-001` |
| **`ENT-02`** | **`Linea_Detalle_Pedido`** | Comercial / Catálogo | `ZapCommerce` (`SIS-04`) | Ítems individuales solicitados dentro de un `Pedido_Comercial`. Especifica el código de producto SKU, descripción, cantidad solicitada, peso unitario teórico y volumen cúbico. | `REQ-FUN-004`, `REQ-RNF-002` |
| **`ENT-03`** | **`Unidad_Almacenamiento_UA`** | Almacén Lurín | `SAP R3` (`SIS-01`) / `Terminales RF` (`SIS-10`) | Caja master mono SKU sellada procedente de Envasado (`ACT-02`), identificada con código de barras unívoco (`Codigo_UA`). Contiene lote, fecha de vencimiento, estado de stock (*libre disposición*, *control de calidad*, *bloqueado*). | `REQ-RES-002`, `REQ-RES-003`, `REQ-ARQ-002` |
| **`ENT-04`** | **`Linea_Picking_Trazable`** | Picking y Packing CD | `SPY` (`SIS-02`) | **Entidad asociativa fundamental (`REQ-ARQ-002`).** Resuelve la correlación M:N entre las cajas master mono SKU (`ENT-03`) abiertas en el CD y las líneas de preparación de pedidos multi SKU (`ENT-01`). Registra qué unidades físicas de qué UA surtieron cada pedido. | `REQ-ARQ-002`, `REQ-INT-001`, `REQ-FUN-004` |
| **`ENT-05`** | **`Bulto_Despacho_Caja`** | Packing y Despacho | `SPY` (`SIS-02`) / Zona Despacho (`ACT-05`) | Bulto físico final embalado para entrega al transporte. Posee un identificador único de bulto, código de formato de caja asignado (1 a 8), peso consolidado real, volumetría y etiqueta con código de barras de despacho. | `REQ-FUN-004`, `REQ-INT-002`, `REQ-RES-001` |
| **`ENT-06`** | **`Hoja_Ruta_Despacho`** | Distribución / Transporte | `NSDG` (`SIS-06`) / `DRIVING` (`SIS-08`) | Agrupación logística de bultos asignados a un socio logístico (`ACT-07`) para un itinerario y modalidad de transporte específica (terrestre, bimodal, aérea) hacia un departamento o ruta troncal. | `REQ-NEG-001`, `REQ-INT-001`, `REQ-RES-004` |
| **`ENT-07`** | **`Evento_Trazabilidad_Canonica`** | Trazabilidad MTD | `MTD` / `Bus de Integración` (`SIS-05`) | Registro canónico inmutable de cambio de estado en el ciclo del despacho (`REQ-ARQ-003`). Almacena ID único, `Numero_Pedido`, estado normalizado (1 de los 5), emisor, marcas duales ($T_0, T_1$), $\Delta T$ y firma hash. | `REQ-NEG-002`, `REQ-FUN-001`, `REQ-ARQ-003`, `REQ-RNF-001` |
| **`ENT-08`** | **`Incidencia_Transporte`** | Transporte y Rutas | `DRIVING` (`SIS-08`) / `NSDG` (`SIS-06`) / MTD | Registro de excepción operativa ocurrida en ruta (retraso justificado por clima/vía, daño de embalaje, siniestro/pérdida o intento fallido de entrega). Vinculado al `Bulto_Despacho_Caja` o `Pedido_Comercial`. | `REQ-NEG-004`, `REQ-FUN-005`, `MTD-R06` |
| **`ENT-09`** | **`Registro_Retorno_Logistica_Inversa`** | Logística Inversa | MTD / Distribución (`ACT-06`) | Registro formal de retorno de un despacho no entregado o dañado hacia el almacén de Lurín. Documenta causa de devolución, socio custodio, guía de retorno y notificación asíncrona a `ACT-03`. | `REQ-NEG-004`, `REQ-FUN-005`, `MTD-R06`, `REQ-RES-002` |
| **`ENT-10`** | **`Receptor_Autorizado_Destino`** | Entrega y Recepción B2B | MTD / Operador Móvil Socio Logístico | **Entidad Condicional bajo `ASM-01` (`REQ-FUN-003`).** Extensión opcional nullable del evento de entrega para capturar tipo/número de documento (DNI/CE) y nombre del tercero formalmente autorizado en la sede o domicilio B2B. | `REQ-FUN-003`, `REQ-RNF-003`, `ASM-01` |

---

## 3. Catálogo Propuesto de Componentes Lógicos del MTD (`CMP-XX`)

| Código           | Nombre Canónico del Componente                                     |   Estereotipo UML   | Responsabilidad Arquitectónica Principal                                                                                                                           | Responsabilidad MTD / Requisito                        |
| :--------------- | :----------------------------------------------------------------- | :-----------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------- |
| **`CMP-ING-01`** | **Componente de Ingesta y Normalización de Eventos**               |    `<<service>>`    | Recibe eventos heterogéneos desde sistemas emisores (`SPY`, `NSDG`, `DRIVING`), valida estructura preliminar, extrae marcas $T_0$ y calcula firma de idempotencia. | `MTD-R01`, `REQ-INT-002`, `REQ-RNF-002`                |
| **`CMP-COR-02`** | **Componente de Correlación y Trazabilidad Lógica**                |   `<<mediator>>`    | Resuelve y mantiene la trazabilidad lógica multidimensional entre `Código UA`, órdenes de preparación en `SPY` y `Número de Pedido` comercial.                     | `MTD-R03`, `REQ-ARQ-002`, `REQ-FUN-004`                |
| **`CMP-VAL-03`** | **Componente de Validación Semántica y Secuencia de Estados**      |  `<<rule-engine>>`  | Valida que los eventos cumplan la secuencia cronológica estricta de los 5 estados, detecta saltos o duplicados y homologa códigos de estado de terceros.           | `MTD-R02`, `REQ-FUN-001`, `REQ-RNF-003`                |
| **`CMP-AUD-04`** | **Componente de Auditoría Temporal y Monitoreo de SLA**            |   `<<telemetry>>`   | Estampa marca temporal de ingesta $T_1$, calcula el desfase $\Delta T = T_1 - T_0$, evalúa cumplimiento de meta $\le 30\text{ min}$ y genera alertas de latencia.  | `MTD-R04`, `REQ-NEG-002`, `REQ-RNF-001`, `REQ-ARQ-003` |
| **`CMP-PUB-05`** | **Componente de Publicación y Sincronización Desacoplada**         |  `<<dispatcher>>`   | Publica eventos consolidados en tópicos/colas del `Bus de Integración` (`SIS-05`) para actualizar `ENSDY`, `CELLFORCE` y portales de consulta de pedidos.          | `MTD-R05`, `REQ-NEG-003`, `REQ-FUN-002`, `REQ-INT-003` |
| **`CMP-NOT-06`** | **Componente de Gestión de Incidencias y Notificación de Retorno** | `<<event-handler>>` | Procesa eventos de siniestro, daño o entrega fallida, orquesta la notificación de retorno a `ACT-03` para logística inversa sin escrituras directas en `SAP R3`.   | `MTD-R06`, `REQ-NEG-004`, `REQ-FUN-005`, `REQ-RES-002` |

---

## 4. Catálogo de Adaptadores de Sistemas Legados y Externos (`ADP-XX`)

| Código | Nombre del Adaptador | Sistema Conectado | Modo de Interacción | Patrón Arquitectónico Implementado |
| :--- | :--- | :--- | :---: | :--- |
| **`ADP-SAP`** | **Adaptador de Consulta e Integración SAP R3** | `SIS-01: SAP R3` | Asíncrono / Solo Lectura | *Anti-Corruption Layer (ACL)*. Consulta de metadatos de producto, estado de UA y recepción de eventos de facturación. No efectúa escrituras directas (`REQ-RES-002`). |
| **`ADP-SPY`** | **Adaptador de Preparación y Picking SPY** | `SIS-02: SPY` | Bidireccional Asíncrono | *Adapter / Event Producer*. Transmite descenso de pedidos desde el bus hacia SPY y captura eventos de fin de preparación, empaque y formatos de caja. |
| **`ADP-NSDG`** | **Adaptador de Asignación y Salida NSDG** | `SIS-06: NSDG` | Asíncrono | *Message Translator*. Recibe órdenes de transporte canalizadas y remite eventos de despacho físico desde la zona de despacho. |
| **`ADP-LOG`** | **Adaptador de Integración Logística Externa (DRIVING / Socios)** | `SIS-08: DRIVING` / `ACT-07` | Híbrido (Webhooks / Push / Polling) | *Store-and-Forward Gateway*. Ingesta eventos de ruta y entrega de socios logísticos, tolerante a desconexión y latencia de red (`ASM-16`). |
| **`ADP-CRM`** | **Adaptador de Atención y Reclamos CELLFORCE** | `SIS-09: CELLFORCE` | Asíncrono / Event-Driven | *Event Consumer*. Consume eventos normalizados de trazabilidad desde el bus corporativo para alimentar la vista 360 del cliente en atención al cliente. |

---

## 5. Catálogo Propuesto de Interfaces de Datos (`INT-DAT-XX`)

| Código | Denominación del Contrato de Datos | Emisor Lógico | Receptor Lógico | Protocolo Lógico / Mecanismo | Frecuencia y SLA Esperado |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`INT-DAT-01`** | **Evento de Pedido Facturado** | `ZapCommerce` (`SIS-04`) | Bus (`SIS-05`) / MTD | Publicación asíncrona de evento comercial con cabecera y líneas. | Por pedido individual ($< 1\text{ s}$). |
| **`INT-DAT-02`** | **Evento de Preparación y Empaque Completado** | `SPY` (`SIS-02`) | Bus (`SIS-05`) / MTD | Notificación de fin de picking, bultos embalados y formato de caja (1 a 8). | Inmediato al sellado de bultos ($< 5\text{ s}$). |
| **`INT-DAT-03`** | **Evento de Salida de Despacho CD** | `Zona Despacho` / `NSDG` | Bus (`SIS-05`) / MTD | Registro de entrega física a socio logístico y manifiesto de hoja de ruta. | Inmediato a la salida del CD ($< 5\text{ min}$). |
| **`INT-DAT-04`** | **Evento de Despacho en Ruta / Tránsito** | `Socio Logístico` / `DRIVING` | Bus (`SIS-05`) / MTD | Notificación de arribo a centros de transferencia, ruta troncal o reparto local. | Asíncrono Near Real-Time ($\Delta T \le 30\text{ min}$). |
| **`INT-DAT-05`** | **Evento de Pedido Entregado B2B** | `Socio Logístico` (`ACT-07`) | Bus (`SIS-05`) / MTD | Conformidad de entrega en destino comercial con receptor y georreferencia. | Inmediato / Lote diferido ($\Delta T \le 30\text{ min}$). |
| **`INT-DAT-06`** | **Evento de Incidencia de Transporte** | `Socio Logístico` / `DRIVING` | Bus (`SIS-05`) / MTD | Notificación de excepción (demora justificada, daño, siniestro, no recepción). | Prioridad Alta ($\le 15\text{ min}$). |
| **`INT-DAT-07`** | **Notificación de Retorno en Logística Inversa** | MTD (`CMP-NOT-06`) | Bus (`SIS-05`) / `ACT-03` | Notificación asíncrona a almacén de Lurín para inspección física y registro en `SAP R3`. | Asíncrono ($< 5\text{ min}$). |
| **`INT-DAT-08`** | **Consulta de Estado y Trazabilidad** | `Maya` / `ENSDY` / `CELLFORCE` | MTD (`CMP-PUB-05`) | Consulta de solo lectura por `Numero_Pedido` y código de cliente comercial. | Síncrono / Caché desacoplada ($< 500\text{ ms}$). |

---

## 6. Mapeo Cruzado con Identificadores Preexistentes

La nomenclatura propuesta se integra sinérgicamente con el ecosistema canónico sin duplicaciones:
- Las entidades `ENT-01` a `ENT-10` dan soporte a los procesos de negocio `PROC-01` a `PROC-05` y a las capacidades `CAP-01`, `CAP-02`, `CAP-04` y `CAP-06`.
- Los componentes `CMP-01` a `CMP-06` implementan formalmente las responsabilidades arquitectónicas `MTD-R01` a `MTD-R06`.
- Los adaptadores `ADP-XX` interconectan los sistemas actuales `SIS-01` a `SIS-10`, respetando la operación por los actores `ACT-01` a `ACT-16`.
- Las interfaces `INT-DAT-01` a `INT-DAT-08` garantizan el cumplimiento de los 24 requisitos de la matriz maestra (`REQ-NEG-001` a `REQ-RES-004`).

