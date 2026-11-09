# 4. FASE C: ARQUITECTURA DE SISTEMAS DE INFORMACIÓN
## Consolidación de la Arquitectura de Datos y Arquitectura de Aplicaciones para la Trazabilidad del Despacho B2B de Yanbal Perú

> **Ubicación del documento:** [`04_Fase_C_Arquitectura_de_Sistemas_de_Informacion/04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md`](04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md)  
> **Documentos de referencia y auditoría:** [`AUDITORIA_PREVIA_SIN_MODIFICACIONES.md`](AUDITORIA_PREVIA_SIN_MODIFICACIONES.md) | [`04_0_Nomenclatura_Propuesta_Fase_C.md`](04_0_Nomenclatura_Propuesta_Fase_C.md) | [`01_5_Matriz_Maestra_Requerimientos.md`](../01_Fase_Preliminar/01_5_Matriz_Maestra_Requerimientos.md) | [`00_2_Catalogo_Maestro_Actores_y_Sistemas.md`](../00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md)  
> **Marco metodológico:** TOGAF 9.2 — Fase C: Arquitectura de Sistemas de Información (Data & Application Architecture)  
> **Principio de gobernanza:** Todos los documentos preexistentes del repositorio se preservan en modo **SOLO LECTURA**. Este documento integra de forma autónoma los modelos de datos y aplicaciones requeridos para resolver la problemática de distribución B2B.

---

## Introducción y Marco de Referencia Arquitectónico

La **Fase C de TOGAF** tiene como objetivo estructurar los activos de información de la organización para soportar los procesos de negocio optimizados diseñados en la Fase B. En el marco del proyecto de transformación de Yanbal Perú, la Fase C se divide en dos dominios complementarios y estrechamente articulados:

1. **Arquitectura de Datos (Apartado 4.1):** Define el modelo conceptual y lógico de las entidades de negocio, la estructura canónica de los eventos de trazabilidad, el diccionario de datos corporativo, la gestión de la calidad e integridad y las políticas de intercambio asíncrono sobre el middleware corporativo.
2. **Arquitectura de Aplicaciones (Apartado 4.2):** Establece el inventario de las aplicaciones existentes en Yanbal (`SIS-01` a `SIS-10`), descompone el Mecanismo de Trazabilidad del Despacho (MTD) como una suite modular de componentes y servicios desacoplados (`CMP-01` a `CMP-06`), especifica la topología de componentes e interfaces, diseña los patrones de integración y consolida la matriz de trazabilidad entre los 24 requerimientos oficiales y los componentes de software.

De acuerdo con las directrices de la auditoría previa ([`AUDITORIA_PREVIA_SIN_MODIFICACIONES.md`](AUDITORIA_PREVIA_SIN_MODIFICACIONES.md)), la Fase C respeta rigurosamente las siguientes fronteras:
- **Alcance B2B Estricto (`REQ-RES-001`, `DEC-04`):** El dominio de datos y aplicaciones se circunscribe a las operaciones hacia Consultoras y Directoras Independientes (`ACT-09`) en el territorio peruano.
- **Naturaleza del MTD (`DEC-02`, `REQ-ARQ-001`):** El MTD es concebido como una capacidad de integración distribuida y desacoplada, no como un sistema de información monolítico o redundante.
- **Operación Exclusiva de SAP R3 (`REQ-RES-002`):** El ERP es operado de forma exclusiva por Almacén Lurín (`ACT-03`). El MTD no efectúa escrituras directas sobre la base de datos de SAP R3.
- **Transporte Externo (`REQ-RES-004`, `ASM-05`):** Yanbal no cuenta con flota propia. La captura de eventos de transporte se realiza mediante interfaces de integración con socios logísticos (`ACT-07`).
- **Agnosticismo Físico:** Los modelos se expresan a nivel conceptual y lógico de dominio, sin asumir marcas propietarias de bases de datos relacionales ni tecnologías específicas de middleware no respaldadas (`ASM-08`).

---

## 4.1. Arquitectura de datos

La Arquitectura de Datos proporciona la estructura lógica de los datos que soportan los procesos de almacenamiento, preparación, zonificación, transporte, seguimiento y logística inversa de Yanbal Perú, resolviendo la discontinuidad informativa identificada en el diagnóstico del negocio.

### 4.1.1. Identificación de entidades y objetos de negocio de distribución

A partir de los procesos operativos modelados en la Fase B (`PROC-01` a `PROC-05`), el catálogo maestro y la evidencia de la entrevista (`[TO]`), se identifican diez entidades lógicas fundamentales que componen el dominio de datos del despacho:

#### 1. `ENT-01: Pedido_Comercial`
- **Definición de negocio:** Representa el encargo comercial consolidado emitido por una Consultora o Directora Independiente (`ACT-09`) a través de las plataformas comerciales de Yanbal.
- **Dominio:** Comercial / Facturación / Despacho.
- **Sistema de Registro (*System of Record* - SoR):** `Maya` (`SIS-03`) / `ZapCommerce` (`SIS-04`).
- **Ciclo de vida:** Creado $\rightarrow$ Facturado $\rightarrow$ En Preparación $\rightarrow$ Despachado $\rightarrow$ En Ruta $\rightarrow$ Entregado (o Retornado).
- **Atributos clave:** `Numero_Pedido` (código único de negocio), `Codigo_Cliente`, `Fecha_Emision`, `Tipo_Documento_Factura`, `Direccion_Destino_Normalizada`, `Ubigeo_Destino`, `Departamento_Destino`, `Provincia_Destino`, `Distrito_Destino`, `Tipo_Ciudad` (Capital/Provincia), `Modalidad_Transporte` (Terrestre, Bimodal, Aérea), `Fecha_Promesa_Entrega`, `Estado_Actual`.
- **Evidencia y requerimientos:** `[TO 14:43–15:06, 21:57–22:28]`, `REQ-NEG-001`, `REQ-FUN-001`, `REQ-FUN-002`, `REQ-RES-001`.

#### 2. `ENT-02: Linea_Detalle_Pedido`
- **Definición de negocio:** Ítems específicos de productos cosméticos o de cuidado personal solicitados dentro de un pedido comercial.
- **Dominio:** Comercial / Catálogo de Producto.
- **Sistema de Registro (SoR):** `ZapCommerce` (`SIS-04`).
- **Atributos clave:** `ID_Linea`, `Numero_Pedido`, `Codigo_SKU`, `Descripcion_Producto`, `Cantidad_Solicitada`, `Peso_Unitario_Teorico`, `Volumen_Unitario_Teorico`, `Importe_Linea`.
- **Evidencia y requerimientos:** `[TO 15:30–16:17]`, `REQ-FUN-004`, `REQ-RNF-002`.

#### 3. `ENT-03: Unidad_Almacenamiento_UA`
- **Definición de negocio:** Caja master mono SKU sellada procedente de la planta de Envasado (`ACT-02`), recepcionada y custodiada en el Almacén de Productos Terminados de Lurín (`ACT-03`).
- **Dominio:** Almacén e Inventario de Productos Terminados.
- **Sistema de Registro (SoR):** `SAP R3` (`SIS-01`) mediante `Terminales RF` (`SIS-10`).
- **Atributos clave:** `Codigo_UA` (código de barras unívoco), `Codigo_SKU`, `Numero_Lote`, `Fecha_Envasado`, `Fecha_Vencimiento`, `Cantidad_Unidades_Master`, `Estado_Stock` (*libre disposición*, *control de calidad*, *bloqueado*), `Ubicacion_Almacen`.
- **Regla operativa:** Tránsito primario sellado sin saldos hacia el Centro de Distribución (`REQ-RES-003`). No se abre en el almacén de Lurín; se apertura únicamente en la línea de picking del CD (`ACT-04`).
- **Evidencia y requerimientos:** `[TO 1:09, 3:10–3:20, 7:11–7:28, 8:12–8:55]`, `REQ-RES-002`, `REQ-RES-003`, `REQ-ARQ-002`.

#### 4. `ENT-04: Linea_Picking_Trazable` (Entidad Asociativa de Correlación)
- **Definición de negocio:** Registro asociativo que vincula las cajas master mono SKU aperturadas (`ENT-03`) con las líneas de surtido de los pedidos comerciales multi SKU (`ENT-01`). Resuelve la correlación lógica exigida por `REQ-ARQ-002` sin imponer una cardinalidad unívoca 1:1 falsa.
- **Dominio:** Preparación de Pedidos en Centro de Distribución.
- **Sistema de Registro (SoR):** `SPY` (`SIS-02`).
- **Atributos clave:** `ID_Tarea_Picking`, `Numero_Pedido`, `ID_Linea_Pedido`, `Codigo_SKU`, `Codigo_UA_Origen`, `Numero_Lote_Consumido`, `Cantidad_Surtida`, `Timestamp_Picking`, `ID_Estacion_Picking`.
- **Evidencia y requerimientos:** `[TO 4:01–4:19, 15:06–15:30, 26:04–26:09]`, `REQ-ARQ-002`, `REQ-INT-001`, `ASM-11`.

#### 5. `ENT-05: Bulto_Despacho_Caja`
- **Definición de negocio:** Unidad de empaque y embalaje final que contiene los productos preparados para un pedido comercial. Cada pedido puede dividirse en uno o más bultos según el consolidado de peso y volumetría teórica calculado en la estación de empaque.
- **Dominio:** Empaque y Despacho.
- **Sistema de Registro (SoR):** `SPY` (`SIS-02`) / Zona de Despacho (`ACT-05`).
- **Atributos clave:** `Codigo_Bulto_Tracking` (identificador único de despacho impreso en etiqueta), `Numero_Pedido`, `Secuencia_Bulto` (ej. 1 de 2), `Formato_Caja` (1 a 8), `Peso_Teorico_Kg`, `Peso_Bascula_Kg`, `Volumen_Cm3`, `Timestamp_Empaque`.
- **Evidencia y requerimientos:** `[TO 15:30–16:17, 17:03–17:26]`, `REQ-FUN-004`, `REQ-RES-001`.

#### 6. `ENT-06: Hoja_Ruta_Despacho` (Manifiesto de Carga)
- **Definición de negocio:** Documento y registro logístico que consolida los bultos despachados para ser transportados por un socio logístico asignado hacia una región o ruta específica.
- **Dominio:** Distribución y Transporte.
- **Sistema de Registro (SoR):** `NSDG` (`SIS-06`) / `DRIVING` (`SIS-08`).
- **Atributos clave:** `Numero_Manifiesto_Hoja_Ruta`, `Codigo_Socio_Logistico` (`ACT-07`), `Modalidad_Transporte` (Terrestre, Bimodal, Aérea), `Ruta_Troncal`, `Placa_Vehiculo_Tercero`, `Conductor_Identificacion`, `Fecha_Hora_Salida_CD`, `Total_Bultos`, `Total_Pedidos`, `Estado_Ruta`.
- **Evidencia y requerimientos:** `[TO 18:02–20:25, 28:00–28:30]`, `REQ-NEG-001`, `REQ-INT-001`, `REQ-RES-004`.

#### 7. `ENT-07: Evento_Trazabilidad_Canonica`
- **Definición de negocio:** Registro canónico e inmutable que captura cada cambio de estado significativo en el ciclo de vida del despacho, incorporando marcas de tiempo duales ($T_0$ y $T_1$) y firma criptográfica para auditoría y medición de latencia.
- **Dominio:** Trazabilidad e Integración MTD.
- **Sistema de Registro (SoR):** `MTD` / `Bus de Integración` (`SIS-05`).
- **Atributos clave:** `ID_Evento_UUID`, `Numero_Pedido`, `Codigo_Bulto_Tracking`, `Estado_Ciclo_Normalizado` (1 a 5), `Codigo_Emisor_Sistema`, `Identificador_Fuente_Externa`, `Timestamp_T0_Evento_Local`, `Timestamp_T1_Ingesta_MTD`, `Latencia_Calculada_DeltaT`, `Codigo_Socio_Logistico`, `Ubicacion_Geografica_Texto`, `Georreferencia_GPS_LatLong`, `Datos_Contexto_Payload`, `Firma_Idempotencia_Hash`.
- **Evidencia y requerimientos:** `[TO 21:57–22:45]`, `REQ-NEG-002`, `REQ-FUN-001`, `REQ-ARQ-003`, `REQ-RNF-001`.

#### 8. `ENT-08: Incidencia_Transporte`
- **Definición de negocio:** Registro de contingencias o eventos anómalos ocurridos durante el tránsito interprovincial o el proceso de entrega B2B (retraso por fuerza mayor, daño de empaque, siniestro/pérdida o imposibilidad de entrega en domicilio registrado).
- **Dominio:** Transporte / Trazabilidad de Excepciones.
- **Sistema de Registro (SoR):** `DRIVING` (`SIS-08`) / `NSDG` (`SIS-06`) / MTD.
- **Atributos clave:** `ID_Incidencia`, `Numero_Pedido`, `Codigo_Bulto_Tracking`, `Tipo_Incidencia` (Retraso, Daño, Siniestro, No_Recepcion), `Descripcion_Motivo`, `Timestamp_Incidencia_T0`, `Codigo_Socio_Logistico`, `Estado_Resolucion`, `Accion_Derivada` (Reintento, Retorno_Inverso).
- **Evidencia y requerimientos:** `[TO 20:55–21:25, 23:44–24:15]`, `REQ-NEG-004`, `REQ-FUN-005`, `MTD-R06`.

#### 9. `ENT-09: Registro_Retorno_Logistica_Inversa`
- **Definición de negocio:** Documento de control que formaliza el reingreso de un pedido no entregado o siniestrado hacia las instalaciones de Yanbal en Lurín, canalizando la notificación asíncrona hacia el Área de Almacén (`ACT-03`) sin escrituras directas en `SAP R3`.
- **Dominio:** Logística Inversa y Custodia Patrimonial.
- **Sistema de Registro (SoR):** MTD / Distribución (`ACT-06`) / Almacén (`ACT-03`).
- **Atributos clave:** `ID_Retorno_Inverso`, `Numero_Pedido`, `ID_Incidencia_Origen`, `Guia_Remision_Retorno`, `Codigo_Socio_Custodio`, `Fecha_Hora_Llegada_Lurin`, `Estado_Fisico_Cajas`, `Dictamen_Calidad_Almacen` (Reincorporacion_Stock, Bloqueo_Merma, Destruccion), `Numero_Documento_Ajuste_SAP`, `Usuario_Almacen_SAP`.
- **Evidencia y requerimientos:** `[TO 20:55–21:25, 27:06–27:43]`, `REQ-NEG-004`, `REQ-FUN-005`, `MTD-R06`, `REQ-RES-002`.

#### 10. `ENT-10: Receptor_Autorizado_Destino` (Entidad Condicional bajo `ASM-01`)
- **Definición de negocio:** Extensión de datos opcional y nullable que registra formalmente la identidad del tercero acreditado para recibir el pedido B2B en el domicilio comercial de la consultora o directora, cuando aplique la política corporativa.
- **Dominio:** Entrega y Recepción B2B.
- **Sistema de Registro (SoR):** MTD / Sistema Móvil de Transporte (`ACT-07`).
- **Atributos clave:** `ID_Receptor_Autorizado`, `Numero_Pedido`, `Tipo_Documento_Identidad` (DNI, Carné de Extranjería), `Numero_Documento_Identidad`, `Nombres_Apellidos`, `Parentesco_o_Relacion_Comercial`, `Firma_Digital_Blob`, `Timestamp_Recepcion`.
- **Evidencia y requerimientos:** `[AC líneas 45–58]`, `REQ-FUN-003`, `REQ-RNF-003`, `ASM-01`.

---

### 4.1.2. Modelo lógico de datos corporativo

El modelo lógico de datos formaliza las interrelaciones entre las entidades de negocio descritas. Su diseño resuelve de manera explícita dos desafíos arquitectónicos críticos:

1. **Resolución de la Cardinalidad `Código UA` $\leftrightarrow$ `Número de Pedido` (`REQ-ARQ-002`, `ASM-11`):**  
   En la planta de Envasado se producen cajas master mono SKU identificadas con `Código UA` (`ENT-03`). Cada caja contiene múltiples unidades de un único producto idéntico. En el Centro de Distribución (`ACT-04`), estas cajas se abren en la línea de picking para preparar pedidos comerciales multi SKU (`ENT-01`). Por lo tanto, un `Pedido_Comercial` está compuesto por unidades provenientes de múltiples UAs distintas, y una sola `Unidad_Almacenamiento_UA` puede abastecer a múltiples pedidos diferentes.  
   Esta relación **Muchos a Muchos ($M:N$)** se resuelve mediante la entidad asociativa **`Linea_Picking_Trazable` (`ENT-04`)**. Cada registro de `ENT-04` vincula exactamente la fracción de unidades tomada de una UA con la línea específica de un pedido, manteniendo la trazabilidad ascendente y descendente del lote productivo sin distorsionar la cardinalidad del modelo.

2. **Desacoplamiento del Evento de Trazabilidad (`REQ-ARQ-003`):**  
   Los eventos de trazabilidad (`ENT-07`) no sobreescriben el registro del pedido, sino que se almacenan como un flujo secuencial inmutable (*Append-Only Log*). Cada `Pedido_Comercial` genera a lo largo de su ciclo de vida entre 5 y $N$ eventos canónicos.

A continuación se presenta la matriz estructural de relaciones lógicas del modelo:

| Entidad Origen | Cardinalidad | Entidad Destino | Clave Foránea de Enlace | Justificación de la Relación y Regla de Negocio |
| :--- | :---: | :--- | :--- | :--- |
| **`ENT-01: Pedido_Comercial`** | **$1 : N$** | `ENT-02: Linea_Detalle_Pedido` | `Numero_Pedido` | Un pedido comercial se compone de una o más líneas de productos solicitados. |
| **`ENT-01: Pedido_Comercial`** | **$1 : N$** | `ENT-04: Linea_Picking_Trazable` | `Numero_Pedido` | Cada pedido es preparado mediante tareas de recolección en las líneas de picking. |
| **`ENT-03: Unidad_Almacenamiento_UA`** | **$1 : N$** | `ENT-04: Linea_Picking_Trazable` | `Codigo_UA_Origen` | Una caja mono SKU puede ser consumida parcialmente en el surtido de múltiples pedidos. |
| **`ENT-01: Pedido_Comercial`** | **$1 : N$** | `ENT-05: Bulto_Despacho_Caja` | `Numero_Pedido` | Un pedido se empaca en uno o más bultos físicos (asignando formato de caja 1 a 8). |
| **`ENT-06: Hoja_Ruta_Despacho`** | **$1 : N$** | `ENT-05: Bulto_Despacho_Caja` | `Numero_Manifiesto_Hoja_Ruta` | Una hoja de ruta consolida múltiples bultos asignados a un socio logístico (`ACT-07`). |
| **`ENT-01: Pedido_Comercial`** | **$1 : N$** | `ENT-07: Evento_Trazabilidad_Canonica` | `Numero_Pedido` | Un pedido acumula una secuencia histórica de eventos canónicos inmutables. |
| **`ENT-05: Bulto_Despacho_Caja`** | **$1 : N$** | `ENT-07: Evento_Trazabilidad_Canonica` | `Codigo_Bulto_Tracking` | Los eventos de transporte y entrega se registran a nivel de bulto individual. |
| **`ENT-07: Evento_Trazabilidad_Canonica`** | **$0..1 : 1$** | `ENT-08: Incidencia_Transporte` | `ID_Evento_UUID` | Un evento de anomalía en ruta genera un registro detallado de incidencia. |
| **`ENT-08: Incidencia_Transporte`** | **$0..1 : 1$** | `ENT-09: Registro_Retorno_Logistica_Inversa` | `ID_Incidencia_Origen` | Una incidencia no resuelta en destino deriva en un expediente de retorno a Lurín. |
| **`ENT-07: Evento_Trazabilidad_Canonica`** | **$0..1 : 0..1$** | `ENT-10: Receptor_Autorizado_Destino` | `ID_Evento_UUID` | **Condicional (`ASM-01`):** Solo el evento de entrega puede enlazar a un receptor autorizado. |

---

### 4.1.3. Diccionario de datos y matriz de relaciones principales

A continuación se detalla la especificación lógica de los atributos de las entidades centrales del modelo de datos:

#### Diccionario Lógico de Datos de Entidades Principales

| Entidad | Atributo Lógico | Tipo de Dato | Longitud / Formato | Restricción / Clave | Nulo? | Descripción Funcional y Regla de Negocio |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| `ENT-01` | `Numero_Pedido` | String | Varchar(20) | **PK** | No | Código único de la orden comercial generado en `Maya`/`ZapCommerce`. |
| `ENT-01` | `Codigo_Cliente` | String | Varchar(15) | Index | No | Código de la Consultora o Directora Independiente (`ACT-09`). |
| `ENT-01` | `Fecha_Emision` | DateTime | ISO 8601 UTC | — | No | Fecha y hora de generación de la orden de compra. |
| `ENT-01` | `Direccion_Destino` | String | Varchar(250) | — | No | Dirección declarada de entrega en el territorio peruano. |
| `ENT-01` | `Departamento_Destino` | String | Varchar(50) | Index | No | Departamento geográfico de destino (1 de los 24 de Perú). |
| `ENT-01` | `Modalidad_Transporte` | Enum | Terrestre/Bimodal/Aérea | — | No | Modalidad logística asignada según lejanía y accesibilidad. |
| `ENT-01` | `Fecha_Promesa_Entrega`| DateTime | ISO 8601 UTC | — | No | Fecha límite calculada según lead time (24 h Lima / 7 d provincias). |
| `ENT-01` | `Estado_Actual` | Enum | 5 Estados Canónicos | Index | No | Estado actual consolidado del pedido comercial. |
| `ENT-03` | `Codigo_UA` | String | Varchar(30) | **PK** | No | Identificador unívoco de caja master mono SKU capturado con RF. |
| `ENT-03` | `Codigo_SKU` | String | Varchar(20) | Index | No | Código del producto cosmético terminado. |
| `ENT-03` | `Numero_Lote` | String | Varchar(20) | Index | No | Lote industrial asignado en Envasado (`ACT-02`). |
| `ENT-03` | `Estado_Stock` | Enum | Disposición/Calidad/Bloqueado | — | No | Estado maestro de inventario controlado en `SAP R3` (`ACT-03`). |
| `ENT-04` | `ID_Tarea_Picking` | UUID | UUID v4 | **PK** | No | Identificador único de la línea de recolección ejecutada en CD. |
| `ENT-04` | `Numero_Pedido` | String | Varchar(20) | **FK** | No | Enlace al pedido comercial beneficiario (`ENT-01`). |
| `ENT-04` | `Codigo_UA_Origen` | String | Varchar(30) | **FK** | No | Enlace a la caja master mono SKU consumida (`ENT-03`). |
| `ENT-04` | `Cantidad_Surtida` | Integer | Int $\ge 1$ | — | No | Cantidad de unidades físicas extraídas para el pedido. |
| `ENT-05` | `Codigo_Bulto_Tracking`| String | Varchar(35) | **PK** | No | Código de barras único adherido a la caja de despacho. |
| `ENT-05` | `Numero_Pedido` | String | Varchar(20) | **FK** | No | Enlace al pedido comercial correspondiente. |
| `ENT-05` | `Formato_Caja` | Integer | 1 a 8 | Check(1..8)| No | Formato volumétrico asignado según peso y cubitaje (`[TO 15:30]`).|
| `ENT-05` | `Peso_Bascula_Kg` | Decimal | Decimal(6,3) | $> 0.000$ | No | Peso físico verificado en báscula de packing. |
| `ENT-06` | `Numero_Manifiesto` | String | Varchar(25) | **PK** | No | Identificador formal del manifiesto de carga y hoja de ruta. |
| `ENT-06` | `Codigo_Socio_Logistico`| String | Varchar(20) | Index | No | Identificador del proveedor de transporte externo (`ACT-07`). |

---

#### Estructura Canónica Obligatoria del Evento de Trazabilidad (`REQ-ARQ-003`)

El evento canónico constituye el contrato de datos fundamental que viaja por el `Bus de Integración` (`SIS-05`). Cumple estrictamente los requerimientos `REQ-ARQ-003`, `REQ-RNF-001` y la decisión `DEC-05`:

```
ESTRUCTURA DEL REGISTRO CANÓNICO DE EVENTO DE TRAZABILIDAD (ENT-07)
├── ID_Evento_UUID                     : UUID v4 (Identificador unívoco universal)
├── Numero_Pedido                      : String (Identificador de correlación de negocio)
├── Codigo_Bulto_Tracking              : String (Identificador físico de despacho, opcional en facturación)
├── Estado_Ciclo_Normalizado           : Enum ('FACTURADO', 'PREPARADO', 'DESPACHADO', 'EN_RUTA', 'ENTREGADO', 'INCIDENCIA', 'RETORNO')
├── Codigo_Emisor_Sistema              : Enum ('SIS-01', 'SIS-02', 'SIS-04', 'SIS-06', 'SIS-08', 'ACT-07')
├── Identificador_Fuente_Externa       : String (ID de transacción o guía del sistema emisor)
├── Timestamp_T0_Evento_Local          : DateTime ISO 8601 UTC (Marca temporal inmutable de ocurrencia física)
├── Timestamp_T1_Ingesta_MTD           : DateTime ISO 8601 UTC (Marca temporal de recepción e ingesta central)
├── Latencia_Calculada_DeltaT          : Integer (Diferencial en segundos: DeltaT = T1 - T0)
├── Codigo_Socio_Logistico             : String (Código de transportista externo asignado)
├── Datos_Contexto_Payload             : JSON / Clave-Valor (Detalles específicos: coordenadas, receptor, incidencias)
└── Firma_Idempotencia_Hash            : SHA-256 (Hash criptográfico para deduplicación: SHA256(T0 + Pedido + Estado + Emisor))
```

A continuación se describe la función y restricciones de los 11 campos del esquema canónico:

| Campo del Evento Canónico | Tipo Lógico | Restricción / Regla de Validación | Propósito Arquitectónico y Gobernanza |
| :--- | :---: | :--- | :--- |
| **`ID_Evento_UUID`** | UUID v4 | Primaria, Generada en el punto de captura o adaptación. | Garantiza unicidad global del evento en sistemas distribuidos. |
| **`Numero_Pedido`** | Varchar(20) | Obligatorio. Debe existir en `ENT-01`. | Llave de negocio para correlación de estados y consultas. |
| **`Codigo_Bulto_Tracking`** | Varchar(35) | Opcional en `FACTURADO`; obligatorio desde `PREPARADO`. | Permite trazabilidad granular por bulto físico individual. |
| **`Estado_Ciclo_Normalizado`**| Enum | 1 de los 5 estados canónicos o subtipos de excepción. | Homologa los múltiples códigos heterogéneos de proveedores. |
| **`Codigo_Emisor_Sistema`** | Varchar(15) | Obligatorio. Catálogo oficial `SIS-01` a `SIS-10` o `ACT-07`. | Auditoría de procedencia del evento (*Provenance*). |
| **`Identificador_Fuente_Externa`**| Varchar(50)| Obligatorio. ID propio del sistema origen (remito, ticket). | Conciliación y cruce de datos con auditorías de proveedores. |
| **`Timestamp_T0_Evento_Local`** | DateTime UTC | Obligatorio. Formato ISO 8601 (`YYYY-MM-DDTHH:MM:SSZ`). | **Marca $T_0$:** Momento exacto de ocurrencia física in situ. |
| **`Timestamp_T1_Ingesta_MTD`** | DateTime UTC | Generado automáticamente por `CMP-AUD-04` al recibir. | **Marca $T_1$:** Momento de procesamiento central en el bus. |
| **`Latencia_Calculada_DeltaT`** | Integer | Calculado: $\Delta T = T_1 - T_0$ (en segundos). | **Métrica clave:** Monitorea el cumplimiento del SLA $\le 30\text{ min}$. |
| **`Codigo_Socio_Logistico`** | Varchar(20) | Obligatorio para eventos emitidos en ruta o entrega. | Permite evaluar el desempeño y SLA de cada transportista. |
| **`Firma_Idempotencia_Hash`** | Char(64) | SHA-256 sobre (`Numero_Pedido` + `Estado` + `T0` + `Emisor`). | **Deduplicación:** Evita el doble procesamiento de eventos repetidos. |

---

#### Matriz de Interacción Entidad - Proceso de Negocio (CRUD)

La siguiente matriz define qué procesos del negocio (`PROC-01` a `PROC-05`) crean (**C**), leen (**R**), actualizan (**U**) o archivan (**D**) las entidades de datos:

| Entidad Lógica | `PROC-01`: Ingreso PT y Tránsito Lurín | `PROC-02`: Preparación y Picking CD | `PROC-03`: Zonificación y Despacho | `PROC-04`: Transporte y Entrega B2B | `PROC-05`: Consultas y Logística Inversa |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`ENT-01: Pedido_Comercial`** | — | **R** | **R, U** | **R, U** | **R** |
| **`ENT-02: Linea_Detalle_Pedido`** | — | **R** | **R** | — | **R** |
| **`ENT-03: Unidad_Almacenamiento_UA`** | **C, R, U** | **R, U** (consumo) | — | — | **R** (auditoría lote) |
| **`ENT-04: Linea_Picking_Trazable`** | — | **C, R** | **R** | — | **R** |
| **`ENT-05: Bulto_Despacho_Caja`** | — | **C** | **R, U** | **R, U** | **R** |
| **`ENT-06: Hoja_Ruta_Despacho`** | — | — | **C, R** | **R, U** | **R** |
| **`ENT-07: Evento_Trazabilidad_Canonica`**| — | **C** (Prep.) | **C** (Desp.) | **C** (Ruta/Ent.) | **R** |
| **`ENT-08: Incidencia_Transporte`** | — | — | — | **C, R** | **R, U** |
| **`ENT-09: Registro_Retorno_Logistica_Inversa`**| **R, U** (recepción física) | — | — | **C** (inicia retorno) | **C, R, U** (dictamen) |
| **`ENT-10: Receptor_Autorizado_Destino`** | — | — | — | **C** (`ASM-01`) | **R** |

---

### 4.1.4. Gestión, calidad, integridad y seguridad de los datos

Para asegurar que los datos del despacho sean un activo confiable, oportuno y seguro para toda la corporación, se establecen las siguientes políticas de gobierno:

#### 1. Dimensiones de Calidad de Datos
- **Completitud:** Ningún evento canónico es admitido en el MTD si carece de `Numero_Pedido`, `Estado_Ciclo_Normalizado`, `Timestamp_T0_Evento_Local` o `Codigo_Emisor_Sistema`. Los eventos incompletos son desviados a una cola de reprocesamiento (*Dead Letter Queue* - DLQ) para subsanación técnica sin frenar el flujo operativo.
- **Consistencia y Homologación Semántica (`MTD-R02`):** Cada transportista externo (`ACT-07`) maneja códigos y terminologías dispares (ej. "En tránsito", "En reparto", "En viaje", "En camino"). El componente de validación semántica (`CMP-VAL-03`) homologa todas las variantes contra el catálogo canónico de 5 estados:
  1. `Pedido facturado`
  2. `Elaboración o en preparación`
  3. `Pedido despachado`
  4. `Pedido en ruta`
  5. `Pedido entregado`
- **Puntualidad (*Timeliness*) y Monitoreo de SLA:** Se mide de forma automatizada la latencia informativa $\Delta T = T_1 - T_0$. Si $\Delta T > 30\text{ min}$ (`REQ-RNF-001`, `DEC-03`, `DEC-05`), el evento se almacena con marca de degradación de SLA y se emite una alerta telemática al Área de Distribución (`ACT-06`) para auditar al socio logístico causante del retraso en la transmisión.
- **Unicidad e Idempotencia:** Dada la naturaleza intermitente de la conectividad en transporte interprovincial (`ASM-16`), los dispositivos de los socios logísticos pueden reintentar el envío del mismo evento múltiples veces. El MTD calcula la firma SHA-256 (`Firma_Idempotencia_Hash`); si la firma ya existe en la ventana operacional de 72 horas, el evento duplicado se confirma como recibido al emisor pero se descarta en el motor de persistencia, evitando transiciones espurias o duplicidad de estados.

#### 2. Seguridad, Privacidad y Cumplimiento Normativo (Ley 29733)
- **Protección de Datos Personales:** Los datos del destinatario comercial (`ACT-09`) y de la persona autorizada condicional (`ACT-10`, `ENT-10`), tales como nombres, números de DNI y direcciones domiciliares, están sujetos a la **Ley N° 29733 (Ley de Protección de Datos Personales del Perú)**.
- **Enmascaramiento y Cifrado:** En los logs de depuración del bus y en eventos de auditoría técnica, los datos identificables son anonimizados o enmascarados (ej. `DNI: *****123`). Todo dato sensible en tránsito se protege mediante protocolos seguros (cifrado TLS 1.3).
- **Control de Acceso Basado en Roles (RBAC - `REQ-RNF-003`):** El acceso a la consulta detallada de eventos está estrictamente delimitado por rol:
  - Operadores de Almacén (`ACT-03`): Acceso a datos de producto, UA, lote y notificación de retornos.
  - Operadores de Distribución (`ACT-06`): Acceso completo a hojas de ruta, telemetría y cumplimiento de transportistas.
  - Agentes de Servicio al Cliente (`ACT-08`): Acceso a la vista de estado consolidada, promesas de entrega e historial de incidencias en `CELLFORCE`.
  - Consultoras y Directoras (`ACT-09`): Acceso exclusivo de solo lectura a sus propios pedidos mediante `Numero_Pedido` y clave comercial en `Maya`.

#### 3. Política de Retención y Particionamiento
- **Partición Operativa Activa (Hot Data):** Los eventos de pedidos en curso y con antigüedad menor a 90 días residen en almacenamiento de alta disponibilidad y bajo tiempo de respuesta ($< 100\text{ ms}$).
- **Partición Histórica y Legal (Cold Data):** Transcurridos 90 días desde el estado `ENTREGADO` o `RETORNO`, los eventos se transfieren a repositorios de archivo comprimidos y particionados por año y mes, conservándose durante 5 años para efectos de auditoría contable, tributaria y contractual.

---

### 4.1.5. Estrategia de intercambio y sincronización de datos

La sincronización de datos entre los 10 sistemas del ecosistema no se realiza mediante acoplamientos punto a punto (*spaghetti architecture*), sino a través de una **Arquitectura Orientada a Eventos (EDA)** orquestada por el `Bus de Integración` (`SIS-05`):

```
                   ESTRATEGIA DE INTERCAMBIO Y SINCRONIZACIÓN
                   
  +------------------+         +------------------+         +------------------+
  |  ZapCommerce     |         |  SPY (Picking)   |         |  NSDG / DRIVING  |
  |  (Comercial)     |         |  (Almacén CD)    |         |  (Transportistas)|
  +--------+---------+         +--------+---------+         +--------+---------+
           | Evento Pedido              | Evento Preparado           | Evento Despacho/Ruta
           | Facturado                  | y Formato Caja             | y Entrega / Incidencia
           v                            v                            v
  +----------------------------------------------------------------------------+
  |               BUS DE INTEGRACIÓN CORPORATIVO (SIS-05)                      |
  |     Canales de Publicación / Suscripción (Tópicos de Eventos de Despacho)  |
  +-------------------------------------+--------------------------------------+
                                        |
                 +----------------------+----------------------+
                 |                                             |
                 v                                             v
  +-------------------------------+             +------------------------------+
  | MECANISMO DE TRAZABILIDAD MTD |             | ADAPTADOR DE NOTIFICACIÓN    |
  | Ingesta, Validación, Auditoría|             | A ALMACÉN LURÍN (ADP-SAP)    |
  | Latencia (T1-T0) y Correlación|             | Notificación asíncrona de    |
  +---------------+---------------+             | pedidos retornados sin       |
                  |                             | escritura directa (REQ-RES-02)
                  v                             +--------------+---------------+
  +-------------------------------+                            |
  |  TÓPICO DE ESTADOS CONSOLIDADOS                            v
  +---------------+---------------+             +------------------------------+
                  |                             | ACT-03: ALMACÉN LURÍN        |
        +---------+---------+                   | Evaluación física y registro |
        v                   v                   | soberano en SAP R3           |
  +-----------+       +-----------+             +------------------------------+
  |  ENSDY    |       | CELLFORCE |
  | (Trackeo) |       | (Reclamos)|
  +-----------+       +-----------+
```

#### Principios Clave de Intercambio de Datos:
1. **Publicación y Suscripción Asíncrona (Pub/Sub):** Los emisores (`ZapCommerce`, `SPY`, `NSDG`, `DRIVING`) emiten sus eventos de forma no bloqueante hacia canales temáticos específicos del bus. Ningún sistema emisor se detiene a esperar la respuesta síncrona del MTD ni de los sistemas consumidores.
2. **Tolerancia a Conectividad Intermitente (*Store-and-Forward* - `ASM-16`):** Para los transportistas que recorren tramos sin cobertura celular, las aplicaciones móviles o sistemas de los proveedores (`ACT-07`) almacenan localmente los eventos con su marca inmutable $T_0$. Al recuperar la conectividad, se transmite el lote de eventos al bus; el MTD registra la marca de llegada central $T_1$, calcula $\Delta T$ e identifica que el desfase respondió a la contingencia geográfica sin descartar el registro.
3. **Desacoplamiento Estricto con `SAP R3` (`REQ-RES-002`):** El MTD no interactúa directamente con la base de datos de `SAP R3`. En los eventos de logística inversa (`REQ-FUN-005`, `MTD-R06`), el MTD deposita un mensaje de notificación de retorno en un canal exclusivo para el Área de Almacén Lurín (`ACT-03`). Es el personal autorizado de Lurín quien inspecciona físicamente los bultos devueltos e ingresa la transacción de regularización de inventario en `SAP R3` según sus protocolos vigentes.

---

## 4.2. Arquitectura de aplicaciones

La Arquitectura de Aplicaciones define el mapa de sistemas y componentes de software que dan soporte a las capacidades del negocio, describe los servicios propuestos del Mecanismo de Trazabilidad del Despacho (MTD), detalla la estructura e interfaces de los componentes de software y demuestra la cobertura total de los requerimientos.

### 4.2.1. Inventario de aplicaciones actuales de Yanbal

En concordancia con el catálogo maestro (`00_2`) y la regla de coexistencia declarada en la entrevista de "3 sistemas propios adaptados y 4 sistemas de proveedores" (`[TO s2 1:57–2:20]`, `ASM-09`), se formaliza a continuación el inventario de las aplicaciones existentes en el ecosistema Yanbal involucradas en el flujo de distribución:

| Código | Denominación Canónica | Tipo y Plataforma | Rol / Área Usuaria Principal | Funcionalidad y Alcance en el Flujo de Despacho | Modo de Integración Actual / Limitaciones Identificadas | Incertidumbres Asociadas |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`SIS-01`** | **SAP R3** | ERP Corporativo Productivo | Exclusivo de `ACT-03` (Almacén Lurín) | Control de ingresos de cajas mono SKU desde Envasado, custodia de inventarios, gestión de los 3 estados de stock (*libre disposición*, *calidad*, *bloqueado*) y facturación. | Operado exclusivamente por Almacén (`REQ-RES-002`). Desfase operativo de hasta 6 h en registro de merma interna (`ASM-04`). No expone datos de ruta en tiempo real. | `ASM-08`, `ASM-11` |
| **`SIS-02`** | **SPY** | BWMS (*Yanbal Picking System*) In-house | `ACT-03` (Almacén) y `ACT-04` (CD Picking) | Recibe órdenes comerciales, genera tareas de surtido en líneas de picking, controla pesos/volumetrías teóricas y asigna los 8 formatos de caja. | Conectado al bus corporativo; sincroniza inventario con `SAP R3`. No transmite marcas $T_0$ de fin de preparación a los canales comerciales de forma homogénea. | `ASM-08`, `ASM-11` |
| **`SIS-03`** | **Maya** | Plataforma Comercial Web/Móvil | Área Comercial / Consultoras (`ACT-09`) | Captura pedidos de las consultoras independientes, valida límites de crédito y canaliza órdenes hacia el backend comercial. | Integrado estrechamente con `ZapCommerce`. Carece de visibilidad detallada de estados en ruta durante el transporte interprovincial. | `ASM-02`, `ASM-08` |
| **`SIS-04`** | **ZapCommerce** | Plataforma Transaccional Comercial | Canal Comercial Corporativo | Procesa pedidos comerciales, liquida promociones, consolida líneas de detalle y transfiere las órdenes de despacho hacia el CD vía el bus. | Conectado al bus. Nombre canónico provisional (`ASM-08`); no se asume formalmente idéntico al producto de mercado SAP Commerce. | `ASM-08` |
| **`SIS-05`** | **Bus de Integración** | Middleware de Integración Corporativo | TI y Arquitectura (`ACT-12`) | Plataforma de mensajería e integración que comunica los sistemas internos de Yanbal (`[TO 14:57, 28:59]`). | Conecta sistemas internos de forma heterogénea; requiere normalización de eventos y estandarización de contratos de datos canónicos. | `ASM-08` |
| **`SIS-06`** | **NSDG** | Sistema de Transporte (Desarrollo por Contrata) | `ACT-05` (Despacho) y `ACT-06` (Distribución) | Administra la asignación de pedidos elaborados a transportistas externos y genera la documentación de salida de despacho. | Desarrollado por un contratista de software (`[TO 9:33]`). Interfaz con `SPY` / `SAP R3` con sujeto gramatical ambiguo (`ASM-11`). | `ASM-02`, `ASM-11` |
| **`SIS-07`** | **ENSDY** | Sistema de Trackeo de Pedidos | `ACT-06` (Distribución) y Atención | Proporciona consultas de seguimiento de órdenes a nivel corporativo (`[TO 28:00]`). | Información desfasada por demoras en los reportes de transportistas externos. No se asume idéntico a `NSDG` (`ASM-02`). | `ASM-02` |
| **`SIS-08`** | **DRIVING** | Sistema Externo de Transporte y Despacho | Proveedores de Transporte (`ACT-07`) | Gestiona rutas de entrega, asignación de unidades vehiculares y captura de estados de viaje en ciertas empresas de transporte. | Sistema satélite de terceros (`[TO 28:10]`). Requiere adaptador para transformar eventos propietarios al esquema canónico del MTD. | `ASM-07` |
| **`SIS-09`** | **CELLFORCE** | Sistema de Gestión de Servicio al Cliente (CRM) | `ACT-08` (Servicio al Cliente) | Gestión de tickets, consultas y reclamos de consultoras independientes por demoras o extravíos en despachos. | Consulta de estados con desfase significativo. Denominación provisional fonética (`ASM-06`); no se asume idéntico a Salesforce. | `ASM-06` |
| **`SIS-10`** | **Terminales RF** | Dispositivos Móviles de Radiofrecuencia | `ACT-03` (Almacén Lurín) | Lectura de códigos de barras de cajas master mono SKU (`Código UA`) para registro en vivo en `SAP R3` (`[TO 7:28]`). | Dispositivos dedicados exclusivamente al almacén y líneas primarias. No operan en ruta externa. | — |

---

### 4.2.2. Componentes y servicios de aplicación propuestos

Para materializar las responsabilidades del Mecanismo de Trazabilidad del Despacho (`MTD-R01` a `MTD-R06`) sin duplicar los sistemas existentes (`REQ-ARQ-001`), se diseñan **seis componentes modulares de software** que residen como servicios desacoplados en el ecosistema, complementados por adaptadores (*Anti-Corruption Layers*):

```
+----------------------------------------------------------------------------------------------------+
|                         MECANISMO DE TRAZABILIDAD DEL DESPACHO (MTD)                               |
|                                                                                                    |
|  +--------------------------+  +--------------------------+  +----------------------------------+  |
|  |       CMP-ING-01         |  |       CMP-COR-02         |  |           CMP-VAL-03             |  |
|  | Ingesta y Normalización  |  | Correlación y            |  | Validación Semántica y Secuencia |  |
|  | de Eventos Heterogéneos  |  | Trazabilidad Lógica      |  | de Estados Canónicos             |  |
|  | (MTD-R01 / REQ-INT-002)  |  | (MTD-R03 / REQ-ARQ-002)  |  | (MTD-R02 / REQ-FUN-001)          |  |
|  +------------+-------------+  +------------+-------------+  +----------------+-----------------+  |
|               |                             |                                 |                    |
|               +-----------------------------+---------------------------------+                    |
|                                             |                                                      |
|  +--------------------------+  +------------v-------------+  +----------------------------------+  |
|  |       CMP-AUD-04         |  |       CMP-PUB-05         |  |           CMP-NOT-06             |  |
|  | Auditoría Temporal,      |  | Publicación y            |  | Gestión de Incidencias en Ruta   |  |
|  | Medición SLA Delta T     |  | Sincronización Desacoplada| | y Notificación Retorno Almacén   |  |
|  | (MTD-R04 / REQ-NEG-002)  |  | (MTD-R05 / REQ-NEG-003)  |  | (MTD-R06 / REQ-FUN-005 / REQ-RES-02|
|  +--------------------------+  +--------------------------+  +----------------------------------+  |
+----------------------------------------------------------------------------------------------------+
```

#### Especificación Detallada de los Componentes Propuestos:

1. **`CMP-ING-01: Componente de Ingesta y Normalización de Eventos`**
   - **Estereotipo UML:** `<<service>>`
   - **Responsabilidad:** Constituye la puerta de entrada para todos los eventos del ciclo de despacho generados por los sistemas internos (`SPY`, `NSDG`) y los sistemas satélites de transportistas (`DRIVING`, aplicaciones móviles de terceros). Valida la sintaxis del mensaje, extrae la marca temporal in situ $T_0$, genera el hash SHA-256 de idempotencia y descarta duplicados antes de la propagación interna.
   - **Interfaces Provistas:** `IEventIngestionPort` (recepción de eventos HTTP/REST, colas de mensajería o webhook).
   - **Interfaces Requeridas:** `IMessageQueuePort` (envío hacia el bus de integración).

2. **`CMP-COR-02: Componente de Correlación y Trazabilidad Lógica`**
   - **Estereotipo UML:** `<<mediator>>`
   - **Responsabilidad:** Mantiene y resuelve la relación lógica multidimensional entre el `Código UA` (caja master mono SKU de almacén), las líneas de preparación ejecutadas en `SPY` y el `Número de Pedido` comercial (`REQ-ARQ-002`, `ASM-11`). Permite que ante una consulta o reclamo por un pedido, el sistema determine de qué UAs y lotes productivos específicos provino cada artículo cosmético, y viceversa.
   - **Interfaces Provistas:** `ICorrelationService` (consulta de trazabilidad ascendente y descendente).
   - **Interfaces Requeridas:** `ISPYAdapterPort` (lectura de líneas de picking en SPY).

3. **`CMP-VAL-03: Componente de Validación Semántica y Secuencia de Estados`**
   - **Estereotipo UML:** `<<rule-engine>>`
   - **Responsabilidad:** Aplica las reglas del negocio sobre la transición de estados (`REQ-FUN-001`, `MTD-R02`). Verifica que ningún pedido pase a `ENTREGADO` sin haber registrado previamente `DESPACHADO` y `EN_RUTA`. En caso de recibir eventos fuera de orden temporal (por latencia en redes móviles), reordena el historial basándose en la marca inmutable $T_0$.
   - **Interfaces Provistas:** `IStateValidationEngine`.
   - **Interfaces Requeridas:** `IEventStoragePort` (persistencia del historial de eventos).

4. **`CMP-AUD-04: Componente de Auditoría Temporal y Monitoreo de SLA`**
   - **Estereotipo UML:** `<<telemetry>>`
   - **Responsabilidad:** Registra automáticamente la marca de tiempo de ingesta central $T_1$ en el momento exacto en que el evento entra al MTD (`REQ-ARQ-003`). Calcula la latencia de información $\Delta T = T_1 - T_0$. Si $\Delta T > 30\text{ min}$, emite una señal telemática de incumplimiento de SLA para la gestión del Área de Distribución (`ACT-06`), generando métricas para evaluar a los socios logísticos (`DEC-05`).
   - **Interfaces Provistas:** `IAuditTelemetryService`.
   - **Interfaces Requeridas:** `IMonitoringAlertPort`.

5. **`CMP-PUB-05: Componente de Publicación y Sincronización Desacoplada`**
   - **Estereotipo UML:** `<<dispatcher>>`
   - **Responsabilidad:** Publica los eventos validados y normalizados en los canales temáticos del `Bus de Integración` (`SIS-05`), permitiendo que sistemas consumidores como `ENSDY` (`SIS-07`), `CELLFORCE` (`SIS-09`) y portales comerciales actualicen sus vistas de datos de forma desacoplada y en tiempo casi real (`REQ-NEG-003`, `REQ-FUN-002`).
   - **Interfaces Provistas:** `IEventPublisherService`.
   - **Interfaces Requeridas:** `IBusTopicPort` (publicación en tópicos del bus).

6. **`CMP-NOT-06: Componente de Gestión de Incidencias y Notificación de Retorno`**
   - **Estereotipo UML:** `<<event-handler>>`
   - **Responsabilidad:** Captura y procesa las excepciones operativas en ruta (retrasos justificados, daños o entregas fallidas). Cuando un despacho no puede entregarse y se declara en retorno inverso (`MTD-R06`, `REQ-FUN-005`), formatea y emite una notificación estructurada hacia el Área de Almacén Lurín (`ACT-03`) a través del bus, asegurando que el personal de Lurín prepare la recepción física del bulto y realice soberanamente el ajuste de stock en `SAP R3` sin intervención directa del MTD (`REQ-RES-002`).
   - **Interfaces Provistas:** `IIncidentHandlerPort`.
   - **Interfaces Requeridas:** `IWarehouseNotificationPort` (notificación asíncrona a `ACT-03`).

---

### 4.2.3. Diagrama de componentes UML (Especificación Textual Profunda y Tabular)

En estricto cumplimiento de las directrices metodológicas (sin generación de imágenes ni edición de archivos `.puml` en este turno), a continuación se proporciona la especificación textual exhaustiva y tabular de la arquitectura de componentes de software:

#### Catálogo Arquitectónico de Componentes y Puertos

| Subsistema / Capa | Componente | Estereotipo | Puertos Provistos (*Provided Interfaces*) | Puertos Requeridos (*Required Interfaces*) | Colaboradores Directos |
| :--- | :--- | :---: | :--- | :--- | :--- |
| **Capa de Ingesta** | `CMP-ING-01: Ingesta y Normalización` | `<<service>>` | `IEventIngestPort` (HTTP/Webhook/Queue) | `IValidationQueuePort`, `ITelemetryPort` | `ADP-LOG`, `ADP-SPY`, `ADP-NSDG`, `CMP-AUD-04` |
| **Capa de Negocio** | `CMP-VAL-03: Validación Semántica` | `<<rule-engine>>`| `IValidateStateTransition` | `ICorrelationLookupPort`, `IPersistencePort` | `CMP-ING-01`, `CMP-COR-02`, Repositorio Eventos |
| **Capa de Negocio** | `CMP-COR-02: Correlación Lógica` | `<<mediator>>` | `ITraceabilityQueryPort` | `ISPYReadPort`, `ISAPMetadataPort` | `CMP-VAL-03`, `ADP-SPY`, `ADP-SAP` |
| **Capa de Telemetría**| `CMP-AUD-04: Auditoría y SLA` | `<<telemetry>>` | `IAuditStampPort` | `ISLAAlertPort`, `IMetricsStorePort` | `CMP-ING-01`, Monitor TI (`ACT-12`) |
| **Capa de Salida** | `CMP-PUB-05: Publicación Desacoplada`| `<<dispatcher>>` | `IDispatchEventPort` | `IBusPublishPort` (Tópicos Pub/Sub) | `Bus de Integración` (`SIS-05`), `ADP-CRM` |
| **Capa de Excepciones**| `CMP-NOT-06: Gestión de Retorno` | `<<event-handler>>`| `IRouteIncidentPort` | `IWarehouseNoticePort` (Canal Almacén) | `ADP-LOG`, `ACT-03` (Almacén Lurín) |
| **Capa Adaptadores** | `ADP-SAP: Adaptador SAP R3` | `<<adapter>>` | `ISAPQueryService` | `ISAPRFCClientPort` (Solo Lectura) | `SIS-01: SAP R3` (`ACT-03`) |
| **Capa Adaptadores** | `ADP-SPY: Adaptador SPY` | `<<adapter>>` | `ISPYOrderDescentPort` | `ISPYEventConsumerPort` | `SIS-02: SPY` (`ACT-04`) |
| **Capa Adaptadores** | `ADP-NSDG: Adaptador NSDG` | `<<adapter>>` | `ITransportDispatchPort`| `INSDGMessageTranslatorPort` | `SIS-06: NSDG` (`ACT-05`) |
| **Capa Adaptadores** | `ADP-LOG: Adaptador Logística Externa`| `<<gateway>>` | `ICarrierWebhookPort` | `IStoreAndForwardQueuePort` | `SIS-08: DRIVING`, `ACT-07` |
| **Capa Adaptadores** | `ADP-CRM: Adaptador CRM CELLFORCE` | `<<adapter>>` | `ICRMNotificationPort` | `ICellforceEventClientPort` | `SIS-09: CELLFORCE` (`ACT-08`) |

---

#### Matriz de Interconexión y Cableado de Interfaces

La siguiente tabla especifica la interacción física e interfaces entre los componentes emisores y receptores:

| Componente Origen | Interfaz Provista/Requerida | Componente Destino | Modo de Interacción | Mecanismo / Patrón | Carga Útil (*Payload*) / DTO |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `ADP-LOG` (Transportistas)| `IEventIngestPort` | `CMP-ING-01` | Asíncrono / Push | Webhook / Queue Listener | DTO Evento Crudo de Transporte con $T_0$ y GPS |
| `CMP-ING-01` | `IAuditStampPort` | `CMP-AUD-04` | Síncrono Interno | Direct In-Process / Call | Objeto Evento para estampar $T_1$ y calcular $\Delta T$ |
| `CMP-ING-01` | `IValidateStateTransition` | `CMP-VAL-03` | Asíncrono | Mensaje en Cola Interna | DTO Evento Normalizado con marcas temporales |
| `CMP-VAL-03` | `ITraceabilityQueryPort` | `CMP-COR-02` | Síncrono / Caché | Remote Procedure / Shared Memory | Consulta de correspondencia `UA` $\leftrightarrow$ `Pedido` |
| `CMP-VAL-03` | `IDispatchEventPort` | `CMP-PUB-05` | Asíncrono | Event Dispatcher | DTO Evento Validado Canónico (`ENT-07`) |
| `CMP-PUB-05` | `IBusPublishPort` | `Bus de Integración` (`SIS-05`)| Asíncrono | Publish / Subscribe Topic | Mensaje Canónico de Cambio de Estado |
| `Bus de Integración` (`SIS-05`)| `ICRMNotificationPort` | `ADP-CRM` $\rightarrow$ `CELLFORCE` | Asíncrono | Topic Subscriber | Evento Canónico para actualización de CRM |
| `CMP-VAL-03` (Incidencia)| `IRouteIncidentPort` | `CMP-NOT-06` | Asíncrono | Priority Event Channel | DTO Incidencia de Ruta (`ENT-08`) |
| `CMP-NOT-06` | `IWarehouseNoticePort` | `Bus de Integración` $\rightarrow$ `ACT-03`| Asíncrono | Message Queue dedicada | DTO Notificación de Retorno en Logística Inversa |

---

### 4.2.4. Integración e interoperabilidad entre aplicaciones

La integración entre aplicaciones responde al diseño de una **Plataforma de Integración Híbrida** mediada por el `Bus de Integración` (`SIS-05`), implementando los siguientes patrones canónicos:

1. **Patrón *Anti-Corruption Layer* (ACL) mediante Adaptadores (`ADP-XX`):**  
   Ningún componente interno del MTD conoce la estructura interna de las bases de datos de `SPY`, `SAP R3` o los sistemas satélites de los transportistas. Cada adaptador especializado traduce las llamadas y dialectos propietarios al Modelo Canónico de Datos (`ENT-07`), protegiendo la integridad del dominio.

2. **Patrón *Idempotent Receiver*:**  
   Dado que los eventos provenientes de transportistas interprovinciales pueden duplicarse debido a reintentos automáticos en zonas con baja conectividad celular (`ASM-16`), el componente `CMP-ING-01` evalúa la firma hash SHA-256 de cada evento antes de ingresarlo a la cola de procesamiento. Si el evento ya fue procesado, se descarta silenciosamente enviando un acuse de recibo conforme (ACK) al remitente para evitar congestión de red.

3. **Patrón *Store-and-Forward* para Rutas Remotas (`ASM-16`, `RSK-06`):**  
   Los socios logísticos que realizan rutas troncales hacia la sierra y selva (rutas de hasta 7 días de lead time) operan bajo el patrón de almacenamiento y reenvío. El dispositivo móvil captura la marca $T_0$ de forma local y firma criptográficamente el registro. Cuando el vehículo ingresa a una zona con cobertura celular o llega al centro de transferencia provincial, los eventos acumulados se descargan en ráfaga hacia el adaptador `ADP-LOG`.

4. **Especificación de Contratos de Intercambio de Datos (`INT-DAT-01` a `INT-DAT-08`):**

| Código | Denominación del Contrato | Emisor | Receptor | Protocolo / Transporte | Carga Útil y Reglas de Negocio | SLA / Frecuencia |
| :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| **`INT-DAT-01`** | **Pedido Facturado** | `ZapCommerce` (`SIS-04`)| MTD / `SPY` | Asíncrono / Bus | Cabecera de pedido comercial, cliente B2B, líneas y dirección normalizada. | Inmediato ($< 1\text{ s}$) |
| **`INT-DAT-02`** | **Preparación y Empaque** | `SPY` (`SIS-02`) | MTD | Asíncrono / Bus | Fin de picking, formato de caja (1 a 8), peso báscula y bultos generados. | Inmediato ($< 5\text{ s}$) |
| **`INT-DAT-03`** | **Salida de Despacho CD** | `NSDG` (`SIS-06`) | MTD | Asíncrono / Bus | Entrega a transportista, hoja de ruta asignada y código de socio logístico. | Batch / Inmediato ($< 5\text{ min}$) |
| **`INT-DAT-04`** | **Despacho en Ruta** | `DRIVING` / `ACT-07` | MTD | Asíncrono / Webhook | Arribo a puntos de control en ruta troncal o inicio de reparto en destino. | $\Delta T \le 30\text{ min}$ |
| **`INT-DAT-05`** | **Pedido Entregado B2B** | `Socio Logístico` (`ACT-07`)| MTD | Asíncrono / Webhook | Conformidad de entrega en destino comercial con firma y receptor (`ASM-01`). | $\Delta T \le 30\text{ min}$ |
| **`INT-DAT-06`** | **Incidencia de Transporte**| `DRIVING` / `ACT-07` | MTD | Asíncrono Prioritario | Reporte de daño, retraso justificado o no recepción en destino. | Prioritario ($\le 15\text{ min}$) |
| **`INT-DAT-07`** | **Notificación de Retorno** | MTD (`CMP-NOT-06`) | `ACT-03` (Almacén) | Asíncrono / Cola Bus | Alerta de retorno por logística inversa hacia Lurín (sin escritura en SAP). | Inmediato ($< 5\text{ min}$) |
| **`INT-DAT-08`** | **Consulta de Trazabilidad**| `Maya` / `CELLFORCE` | MTD (`CMP-PUB-05`) | Síncrono / Caché NRT | Consulta de historial de eventos y estado actual por `Numero_Pedido`. | Síncrono ($< 500\text{ ms}$) |

---

### 4.2.5. Matriz de trazabilidad entre requerimientos y componentes de software

La siguiente matriz demuestra la cobertura exhaustiva y la trazabilidad bidireccional entre los **24 requerimientos oficiales** de la [`01_5_Matriz_Maestra_Requerimientos.md`](../01_Fase_Preliminar/01_5_Matriz_Maestra_Requerimientos.md) y los componentes de la Arquitectura de Sistemas de Información (datos, aplicaciones y adaptadores):

| ID Requisito | Denominación Oficial del Requisito | Tipo | Componentes Software Asignados | Sistemas Involucrados | Mecanismo Arquitectónico de Cumplimiento | Método de Verificación / Criterio de Aceptación |
| :--- | :--- | :---: | :--- | :--- | :--- | :--- |
| **`REQ-NEG-001`** | Seguimiento de entregas hacia distribuidoras y destinos B2B | Negocio | `CMP-ING-01`, `CMP-VAL-03`, `CMP-PUB-05` | `SIS-03`, `SIS-05`, `SIS-07`, `SIS-08` | Captura secuencial de eventos y publicación en tópicos accesibles por canales de distribución. | Prueba de consulta de estado de pedidos en ruta con confirmación de entrega en destino. |
| **`REQ-NEG-002`** | Reducción del desfase de actualización a máximo 30 min | Negocio | `CMP-AUD-04`, `CMP-ING-01` | `SIS-05`, `ACT-07` | Registro de marcas duales ($T_0, T_1$), cálculo de $\Delta T = T_1 - T_0$ y alerta telemática ante $\Delta T > 30\text{ min}$. | Muestreo de eventos de transporte verificando que $\Delta T \le 30\text{ min}$ en $\ge 95\%$ de casos. |
| **`REQ-NEG-003`** | Disponibilidad de estados actualizados para consultas y reclamos | Negocio | `CMP-PUB-05`, `ADP-CRM` | `SIS-05`, `SIS-09: CELLFORCE` | Propagación asíncrona de eventos hacia la vista 360 de `CELLFORCE` para los operadores `ACT-08`. | Verificación de visualización del estado del pedido en pantalla de atención de reclamos. |
| **`REQ-NEG-004`** | Trazabilidad del proceso de logística inversa ante incidencias | Negocio | `CMP-NOT-06`, `ADP-LOG` | `SIS-05`, `SIS-08`, `ACT-03`, `ACT-06` | Registro de incidentes en ruta y generación de expediente de retorno inverso hacia Almacén Lurín. | Inspección de registro de retorno en cola de notificación de almacén ante entrega fallida. |
| **`REQ-FUN-001`** | Registro y transición secuencial de los cinco estados del pedido | Funcional | `CMP-VAL-03`, `ENT-07` | `SIS-02`, `SIS-04`, `SIS-06`, `SIS-08` | Motor de reglas que enforcea la máquina de estados finita (1 a 5) y detecta omisiones de secuencia. | Caso de prueba con eventos fuera de orden validando reordenamiento por marca $T_0$. |
| **`REQ-FUN-002`** | Consulta de trazabilidad por Número de Pedido y cliente con promesa | Funcional | `CMP-PUB-05`, `ENT-01` | `SIS-03: Maya`, `SIS-07: ENSDY` | Exposición de interfaz de consulta desacoplada con cálculo de promesa de entrega (24 h / 7 d). | Ejecución de consulta por `Numero_Pedido` retornando estado, histórico y fecha estimada. |
| **`REQ-FUN-003`** | Registro de identificación de Persona Autorizada (`ASM-01`) | Funcional | `CMP-ING-01`, `ENT-10` | `ACT-07` (Móvil), `SIS-05` | Entidad condicional y nullable (`ENT-10`) que almacena DNI y firma digital del receptor delegado. | Prueba de captura de recepción con DNI de tercero autorizado y validación de nullabilidad. |
| **`REQ-FUN-004`** | Asociación de Pedido con formato de caja, geografía y socio | Funcional | `CMP-COR-02`, `ENT-05` | `SIS-02: SPY`, `ACT-05`, `ACT-07` | Registro de formato de caja (1 a 8), pesaje en báscula, ubigeo de destino y transportista asignado. | Verificación de bultos empaquetados asociados unívocamente al pedido con formato 1 a 8. |
| **`REQ-FUN-005`** | Registro de incidencias, retorno y notificación a almacén | Funcional | `CMP-NOT-06`, `ENT-08`, `ENT-09` | `SIS-05`, `ACT-03`, `ACT-07` | Orquestación del flujo de excepción y despacho de mensaje de aviso a Almacén Lurín sin tocar SAP R3. | Notificación de retorno emitida en bus y verificada por personal de almacén en su panel. |
| **`REQ-INT-001`** | Sincronización de stock SAP R3 - SPY y canalización a NSDG | Integración | `ADP-SAP`, `ADP-SPY`, `ADP-NSDG` | `SIS-01`, `SIS-02`, `SIS-05`, `SIS-06` | Sincronización desacoplada de stock entre ERP y BWMS, canalizando órdenes de transporte vía bus. | Auditoría de mensajes entre SPY y NSDG a través del bus corporativo sin colisiones. |
| **`REQ-INT-002`** | Intercambio oportuno de eventos de despacho y entrega con socios | Integración | `ADP-LOG`, `CMP-ING-01` | `SIS-05`, `SIS-08`, `ACT-07` | Gateways heterogéneos con ingesta por webhook, API y cola batch tolerante a fallas. | Pruebas de integración con simulador de transportista enviando eventos de ruta y entrega. |
| **`REQ-INT-003`** | Integración de estados vía Bus entre sistemas comerciales | Integración | `CMP-PUB-05`, `SIS-05` | `SIS-03`, `SIS-04`, `SIS-07`, `SIS-09` | Arquitectura Pub/Sub sobre el bus corporativo distribuyendo cambios de estado en tiempo real. | Verificación de recepción simultánea de eventos en `ENSDY`, `CELLFORCE` y `Maya`. |
| **`REQ-INT-004`** | Coexistencia e integración con 3 sistemas internos y 4 de proveedores | Integración | `ADP-SAP`, `ADP-SPY`, `ADP-LOG`, `ADP-NSDG` | `SIS-01` a `SIS-10`, `ACT-07` | Implementación de capas de adaptación (ACL) para desacoplar los 7 sistemas según `ASM-09`. | Validación de interfaces activas sin modificar el código fuente de los sistemas legados. |
| **`REQ-RNF-001`** | Latencia máxima permisible de 30 minutos en seguimiento | Calidad | `CMP-AUD-04` | `SIS-05`, `ACT-07` | Monitoreo telemático activo de latencia $\Delta T \le 30\text{ min}$ con generación de alertas automáticas. | Reporte de telemetría confirmando procesamiento de eventos con $\Delta T$ dentro de umbral. |
| **`REQ-RNF-002`** | Capacidad de manejo de grandes volúmenes de datos operativos | Calidad | `CMP-ING-01`, `ENT-07` | `SIS-05` | Procesamiento asíncrono no bloqueante, deduplicación hash y particionamiento de datos. | Pruebas de estrés y rendimiento simulando picos de facturación y despacho de campaña. |
| **`REQ-RNF-003`** | Validación por Seguridad Informática y control por roles (RBAC) | Calidad | Todos los componentes | `ACT-11` (Seguridad), MTD | Cifrado TLS 1.3, hashing SHA-256 de eventos, RBAC estricto y cumplimiento de Ley 29733. | Informe de auditoría de seguridad informática corporativa sin hallazgos de vulnerabilidad. |
| **`REQ-RNF-004`** | Preservación de facilidad de uso en interfaces operativas | Calidad | `ADP-SPY`, `ADP-SAP`, `ADP-NSDG` | `SIS-01`, `SIS-02`, `SIS-06` | Adaptaciones transparentes a nivel de backend sin alterar flujos de pantalla de operadores. | Encuesta de usabilidad operativa validando que no hubo cambios disruptivos en pantallas. |
| **`REQ-ARQ-001`** | Diseño del MTD como capacidad integrada sin duplicación | Arquitectura | Ecosistema MTD (`CMP-01` a `06`)| `SIS-05` (Bus Corporativo) | Suite de componentes modulares sobre el middleware corporativo sin construir silos aislados. | Revisión de arquitectura confirmando reutilización del bus y bases de datos existentes. |
| **`REQ-ARQ-002`** | Trazabilidad lógica entre Código UA, picking y Pedido | Arquitectura | `CMP-COR-02`, `ENT-04` | `SIS-01`, `SIS-02` | Modelo asociativo $M:N$ (`ENT-04`) que enlaza cajas mono SKU selladas con pedidos multi SKU. | Consulta de trazabilidad inversa rastreando una UA específica hasta sus pedidos destino. |
| **`REQ-ARQ-003`** | Estructura canónica de eventos con marcas temporales $T_0$ y $T_1$ | Arquitectura | `ENT-07`, `CMP-AUD-04` | Todos los sistemas | Esquema canónico estándar de 11 campos con marcas temporales inmutables y firma hash. | Inspección de esquema de eventos en base de datos validando existencia de $T_0$ y $T_1$. |
| **`REQ-RES-001`** | Alcance exclusivo de despacho B2B en el Perú | Restricción | Dominio de Datos MTD | Toda la solución | Delimitación estricta a consultoras/directoras (`ACT-09`) y 24 departamentos de Perú (`DEC-04`). | Validación de filtros geográficos y de tipo de cliente en el motor de ingesta comercial. |
| **`REQ-RES-002`** | Operación exclusiva de SAP R3 por Almacén Lurín | Restricción | `ADP-SAP`, `CMP-NOT-06` | `SIS-01: SAP R3`, `ACT-03` | Prohibición absoluta de escrituras directas en SAP R3; notificación asíncrona a `ACT-03`. | Auditoría de base de datos de SAP R3 demostrando cero conexiones de escritura del MTD. |
| **`REQ-RES-003`** | Tránsito primario en cajas mono SKU selladas sin saldos | Restricción | `ENT-03`, `PROC-01` | `SIS-01`, `SIS-10`, `ACT-02`, `ACT-03`| Regla de negocio que prohíbe apertura de cajas master en Lurín antes del ingreso a CD. | Inspección física y de registros de almacén certificando integridad de precintos de UA. |
| **`REQ-RES-004`** | Transporte mediante socios externos y exclusión de flota propia | Restricción | `ADP-LOG`, `ENT-06` | `SIS-08`, `ACT-07` | Exclusión de software de gestión de flota propia; modelado de interfaces con terceros (`ASM-05`). | Verificación de contratos de datos orientados a hojas de ruta de socios logísticos externos. |

---

## Conclusiones de la Fase C

1. **Robustez del Modelo de Datos:** Se ha definido un modelo lógico de dominio normalizado y robusto que resuelve de forma definitiva la correlación lógica entre el `Código UA` mono SKU y los pedidos multi SKU (`REQ-ARQ-002`) mediante la entidad asociativa `ENT-04`, formalizando además el contrato canónico de eventos (`ENT-07`) con marcas temporales duales ($T_0, T_1$) conforme a `REQ-ARQ-003`.
2. **Modularidad y Desacoplamiento de Aplicaciones:** El Mecanismo de Trazabilidad del Despacho (MTD) queda plenamente especificado no como una aplicación aislada, sino como una suite modular de 6 componentes de software (`CMP-01` a `CMP-06`) articulados sobre el `Bus de Integración` (`SIS-05`) y aislados de los sistemas legados mediante adaptadores específicos (`ADP-XX`).
3. **Cumplimiento Estricto de Restricciones:** Se garantiza el respeto absoluto a la exclusividad de operación de `SAP R3` por Almacén Lurín (`REQ-RES-002`), la delimitación al alcance B2B (`REQ-RES-001`, `DEC-04`), el transporte ejecutado por terceros (`REQ-RES-004`, `ASM-05`) y el tratamiento condicional de la persona autorizada bajo `ASM-01`.
4. **Trazabilidad Total:** Los 24 requerimientos de la matriz maestra oficial cuentan con asignación de componentes de software, sistemas responsables y criterios de verificación, asegurando una cobertura del 100% que sienta las bases para el modelado de la infraestructura en la Fase D.

