# Especificación de Trabajadores y Entidades del Negocio

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Alcance del Proceso:** Despacho $\longrightarrow$ Transporte/Ruta $\longrightarrow$ Llegada $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta  
> **Marco Metodológico:** RUP (*Rational Unified Process*) — Disciplina de Modelado del Negocio / UML 2.5 / UTP APF1  
>
> **Navegación del Módulo MON:**  
> [[01 - Modelo de Objetos del Negocio (MON)]] | [[02 - Matriz de Realización de Casos de Uso del Negocio]] | [[03 - Especificación de Trabajadores y Entidades del Negocio]]  
> **Diagramas PlantUML:**  
> [[MON_Diagrama_General.puml]] | [[MON_Realizacion_CUN.puml]]  
> **Enlaces al Modelo de Dominio:**  
> [[01 - Diagrama de Clases de Dominio (Nivel Dominio)]] | [[02 - Diccionario de Datos del Modelo de Dominio]] | [[03 - Matriz del Modelo de Dominio]]

---

## 1. Fichas de Especificación de los Trabajadores del Negocio (`«business worker»`)

### Ficha W-01: Supervisor de CD
- **Nombre del Rol:** Supervisor de Centro de Distribución (Supervisor de Despacho).
- **Estereotipo RUP:** `«business worker»`.
- **Ubicación Operativa:** Muelle de Despacho, Centro de Distribución de Lurín, Yanbal Perú.
- **Misión del Rol:** Conducir las operaciones en muelle para asegurar que los pedidos consolidados de picking se clasifiquen correctamente según la geografía nacional, se asignen al socio logístico idóneo, se acompañen de la documentación legal reglamentaria y se transfieran formalmente a la custodia del transportista.
- **Casos de Uso del Negocio en que Participa:**
  - `CUN-01: Despachar Pedidos desde Centro de Distribución` (Líder operativo).
- **Herramientas y Sistemas AS-IS Utilizados:**
  - **Driving (TMS externo):** Asistencia en canalización de rutas, clasificación geográfica en 24 departamentos y asignación de transportista.
  - **NSDG:** Registro de salida de despacho y formalización de custodia.
- **Acciones Operativas Principales:**
  - `Validar / Verificar:` Valida la correspondencia del pedido empacado contra el reporte de picking (`Pedido`).
  - `Clasificar:` Clasifica las cajas por destino departamental, provincial y distrital (`Bulto`).
  - `Asignar:` Vincula socio logístico y modalidad de transporte (terrestre, bimodal, aérea) (`Pedido`).
  - `Emitir:` Genera las guías de remisión, manifiesto de carga y hojas de ruta (`Guía / Documentación de Despacho`).
  - `Registrar:` Formaliza el acta de egreso de carga y entrega de custodia en muelle (`Registro de Despacho`).
- **Entidades del Negocio Relacionadas:**
  - Entradas: `Pedido`, `Bulto`.
  - Salidas: `Registro de Despacho`, `Guía / Documentación de Despacho`.

---

### Ficha W-02: Conductor / Repartidor
- **Nombre del Rol:** Conductor / Repartidor de Carga (Operador de Transporte en Campo).
- **Estereotipo RUP:** `«business worker»`.
- **Ubicación Operativa:** Unidades de transporte multimodal y rutas de última milla a nivel nacional.
- **Misión del Rol:** Realizar la conducción y custodia material de los bultos a lo largo de las rutas departamentales y locales, entregar físicamente las cajas en el domicilio del cliente validando al receptor, o registrar oportunamente las contingencias e iniciar el retorno formal hacia el CD.
- **Casos de Uso del Negocio en que Participa:**
  - `CUN-02: Trasladar Pedidos hacia Destino Nacional` (Operador principal).
  - `CUN-03: Entregar Pedido en Domicilio` (Operador principal).
  - `CUN-04: Gestionar Entrega Fallida y Retorno por Logística Inversa` (Operador principal).
- **Herramientas y Sistemas AS-IS Utilizados:**
  - **NSDG Móvil / Herramientas de campo del transportista:** Registro de inicio de ruta, confirmación de entrega en domicilio, tipificación de fallas y registro de retorno.
  - **Unidad de transporte vehicular:** Camiones troncales, camionetas de reparto urbano, naves fluviales o vuelos comerciales.
- **Acciones Operativas Principales:**
  - `Portar / Validar:` Porta y exhibe la documentación de transporte ante controles de ruta (`Guía / Documentación de Despacho`).
  - `Actualizar:` Registra el inicio de tránsito formal cambiando el estado a *"En Ruta"* (`Pedido`).
  - `Trasladar / Custodiar:` Conduce y resguarda los bultos por las vías correspondientes (`Bulto`).
  - `Entregar:` Realiza la entrega material del bulto al receptor titular o persona autorizada (`Bulto`).
  - `Registrar:` Genera el comprobante de entrega consignando receptor, DNI y firma (`Registro de Entrega`).
  - `Reportar / Registrar:` Registra la imposibilidad de entrega tipificando causal: retraso, pérdida o daño (`Incidencia de Entrega`).
  - `Gestionar / Iniciar:` Ampara la devolución física de carga en custodia hacia el CD (`Orden de Retorno`).
- **Entidades del Negocio Relacionadas:**
  - Entradas: `Bulto`, `Guía / Documentación de Despacho`, `Pedido`.
  - Salidas: `Registro de Entrega`, `Incidencia de Entrega`, `Orden de Retorno`, `Información de Entrega`.

---

### Ficha W-03: Personal SAC
- **Nombre del Rol:** Personal de Servicio de Atención al Cliente (Agente SAC / Mesa de Ayuda).
- **Estereotipo RUP:** `«business worker»`.
- **Ubicación Operativa:** Canales de atención remota (call center / oficinas de soporte), Yanbal Perú.
- **Misión del Rol:** Brindar atención y soporte informativo a las consultoras y consultores sobre la situación física de sus pedidos, plazos comprometidos de entrega y datos de recepción, intermediando además ante incidencias operativas reportadas en campo.
- **Casos de Uso del Negocio en que Participa:**
  - `CUN-05: Consultar Trazabilidad y Situación del Pedido` (Operador de consulta).
- **Herramientas y Sistemas AS-IS Utilizados:**
  - **Salesforce (Cellforce):** CRM corporativo de consulta de pedidos y gestión de casos.
- **Acciones Operativas Principales:**
  - `Consultar:` Accede a los datos de tracking, último estado sincronizado y promesa de entrega (`Información de Entrega`).
  - `Verificar:` Valida la existencia y correspondencia de la orden mediante N° Pedido o Código de Consultora (`Pedido`).
  - `Validar:` Inspecciona los datos del receptor real cuando la consultora manifiesta desconocer la entrega (`Información de Entrega`).
  - `Gestionar:` Revisa el motivo tipificado de entrega fallida para explicar retrasos o iniciar trámites comerciales (`Incidencia de Entrega`).
- **Impacto de Problemas AS-IS:** Padece la latencia de sincronización de hasta 2 horas (`PR-04`) y la falta de visualización oportuna de los datos del receptor real (`PR-05`).
- **Entidades del Negocio Relacionadas:**
  - Entradas: `Información de Entrega`, `Pedido`, `Incidencia de Entrega`.
  - Salidas: Respuestas de atención y registro de consultas en CRM.

---

## 2. Fichas de Especificación de las Entidades del Negocio (`«business entity»`)

### Ficha E-01: Pedido
- **Nombre de la Entidad:** `Pedido`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Entidad conceptual y transaccional central.
- **Definición:** Representa la orden de compra consolidada efectuada por una consultora de belleza independiente de Yanbal, la cual ampara el conjunto de productos a ser entregados en su domicilio.
- **Atributos Clave:**
  - `numeroPedido`: Identificador único alfanumérico de la orden.
  - `codigoConsultora`: Código único de la consultora compradora.
  - `departamentoDestino`: Departamento de entrega (1 de los 24 a nivel nacional).
  - `provinciaDestino`: Provincia del domicilio.
  - `distritoDestino`: Distrito de destino.
  - `estadoPedido`: Estado actual del pedido (*Despachado*, *En Ruta*, *Entregado*, *Entrega Fallida*).
  - `promesaEntrega`: Fecha y hora límite estimada de entrega (24h Lima / 7d provincias).
- **Ciclo de Vida y Estados en el AS-IS:**
  1. *Empacado (Picking completado)* $\longrightarrow$ 2. *Despachado (Muelle)* $\longrightarrow$ 3. *En Ruta (Tránsito)* $\longrightarrow$ 4a. *Entregado (Terminal exitoso)* ó 4b. *Entrega Fallida (Terminal contingencia)*.
- **Trabajadores que la manipulan:** Supervisor de CD (`valida`, `asigna`), Conductor / Repartidor (`actualiza`), Personal SAC (`verifica`).
- **CUNs Vinculados:** `CUN-01`, `CUN-02`, `CUN-03`, `CUN-04`, `CUN-05`.

---

### Ficha E-02: Bulto
- **Nombre de la Entidad:** `Bulto`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Entidad física y material.
- **Definición:** Caja o paquete físico consolidado que contiene los productos de un pedido. Cada pedido puede dividirse en 1 o varios bultos según el peso y volumetría de los artículos solicitados.
- **Atributos Clave:**
  - `codigoBulto`: Código de barra identificador de la caja individual.
  - `formatoCaja`: Tipo de caja asignada en picking (Formatos 1 a 8).
  - `pesoKg`: Peso medido en báscula de picking/despacho.
  - `secuenciaBulto`: Índice del bulto respecto al total (ejemplo: Bulto 1 de 3).
  - `estadoFisico`: Condición de integridad física (*Íntegro*, *Observado*, *Dañado*).
- **Ciclo de Vida:**
  1. *Recepcionado en muelle* $\longrightarrow$ 2. *Clasificado por región* $\longrightarrow$ 3. *Estibado y en traslado* $\longrightarrow$ 4a. *Entregado a cliente* ó 4b. *En custodia/retorno al CD*.
- **Trabajadores que la manipulan:** Supervisor de CD (`inspecciona`, `clasifica`), Conductor / Repartidor (`traslada`, `custodia`, `entrega`).
- **CUNs Vinculados:** `CUN-01`, `CUN-02`, `CUN-03`, `CUN-04`.

---

### Ficha E-03: Registro de Despacho
- **Nombre de la Entidad:** `Registro de Despacho`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Entidad transaccional y probatoria interna.
- **Definición:** Constancia formal que acredita el egreso de la carga del CD de Lurín y formaliza el traspaso de la custodia y responsabilidad física desde Yanbal hacia el transportista contratado.
- **Atributos Clave:**
  - `idDespacho`: Código identificador de la transacción de despacho.
  - `fechaHoraDespacho`: Timestamp exacto de salida de muelle.
  - `supervisorResponsable`: Nombre del Supervisor de CD que autoriza el egreso.
  - `socioLogisticoAsignado`: Razón social de la empresa de transporte receptora.
  - `modalidadTransporte`: Modo operativo acordado (*Terrestre Troncal*, *Bimodal*, *Aéreo*).
  - `cantidadBultosDespachados`: Total de cajas entregadas al transportista.
- **Ciclo de Vida:** Creado y cerrado en muelle de despacho al formalizar la salida.
- **Trabajadores que la manipulan:** Supervisor de CD (`registra`).
- **CUNs Vinculados:** `CUN-01`.

---

### Ficha E-04: Guía / Documentación de Despacho
- **Nombre de la Entidad:** `Guía / Documentación de Despacho`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Entidad documental legal y fiscal.
- **Definición:** Conjunto de documentos físicos y electrónicos (Guía de Remisión Remitente, Guía de Remisión Transportista, Manifiesto de Carga y Hoja de Ruta) obligatorios para amparar el traslado de mercancías según la normativa fiscal y de transporte en el Perú.
- **Atributos Clave:**
  - `numeroGuiaRemision`: Numeración fiscal de la guía de remisión.
  - `numeroManifiesto`: Identificador del manifiesto consolidado de carga del vehículo.
  - `placaVehiculo`: Matrícula de la unidad de transporte autorizada.
  - `datosConductor`: Nombre y licencia de conducir del transportista.
  - `rutaAsignada`: Itinerario geográfico de viaje autorizado.
  - `fechaEmision`: Fecha y hora de expedición documental.
- **Ciclo de Vida:**
  1. *Emitida en CD* $\longrightarrow$ 2. *Portada y fiscalizada en ruta* $\longrightarrow$ 3. *Firmada y archivada como descargo*.
- **Trabajadores que la manipulan:** Supervisor de CD (`emite`), Conductor / Repartidor (`porta`, `valida`).
- **CUNs Vinculados:** `CUN-01`, `CUN-02`.

---

### Ficha E-05: Registro de Entrega
- **Nombre de la Entidad:** `Registro de Entrega`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Comprobante transaccional probatorio en destino.
- **Definición:** Documento electrónico o físico capturado en el punto de entrega que certifica la recepción material y conforme de los bultos en la dirección consignada.
- **Atributos Clave:**
  - `idRegistroEntrega`: Código de confirmación generado por el dispositivo de campo.
  - `fechaHoraEntrega`: Timestamp exacto de recepción en domicilio.
  - `tipoReceptor`: Condición del receptor (*Consultora Titular* o *Persona Autorizada*).
  - `nombreReceptor`: Nombres y apellidos completos de quien recibe.
  - `dniReceptor`: Documento Nacional de Identidad del receptor.
  - `parentescoRelacion`: Vínculo con la consultora (en caso de persona autorizada: familiar, vecino, encargado).
  - `firmaDigitalConformidad`: Registro de firma o evidencia de recepción en app móvil.
- **Ciclo de Vida:** Generado en campo en el instante de entrega; insumo base para la sincronización central.
- **Trabajadores que la manipulan:** Conductor / Repartidor (`registra`, `captura`).
- **CUNs Vinculados:** `CUN-03`.

---

### Ficha E-06: Incidencia de Entrega
- **Nombre de la Entidad:** `Incidencia de Entrega`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Registro de excepción y control de no conformidades.
- **Definición:** Registro operativo que documenta formalmente la imposibilidad de concretar la entrega de un pedido, sustentado en una de las 3 causales tipificadas por el negocio: retraso severo, pérdida o daño material.
- **Atributos Clave:**
  - `idIncidencia`: Identificador único del reporte de contingencia.
  - `fechaHoraReporte`: Timestamp de captura del incidente en campo.
  - `tipoCausal`: Causal tipificada exclusiva (*Retraso por corte de vía/clima*, *Pérdida/Siniestro*, *Daño en bulto/derrame*).
  - `descripcionHecho`: Detalle circunstancial del evento.
  - `ubicacionIncidente`: Coordenadas o localidad geográfica del suceso.
  - `requiereLogisticaInversa`: Booleano (*true* si hay bulto físico presente para devolución; *false* en pérdida total).
- **Ciclo de Vida:** Creado en campo al frustrarse la entrega; consultado en SAC para atención de reclamos.
- **Trabajadores que la manipulan:** Conductor / Repartidor (`reporta`, `registra`), Personal SAC (`gestiona`, `consulta`).
- **CUNs Vinculados:** `CUN-04`, `CUN-05`.

---

### Ficha E-07: Orden de Retorno
- **Nombre de la Entidad:** `Orden de Retorno`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Instrumento logístico de reversa y custodia.
- **Definición:** Documento que autoriza y ampara el retorno material de bultos no entregados desde la ruta nacional o destino final hacia las instalaciones del Centro de Distribución de Lurín.
- **Atributos Clave:**
  - `idOrdenRetorno`: Código de tracking de logística inversa.
  - `idIncidenciaOrigen`: Vínculo con la incidencia que motivó la devolución.
  - `motivoDevolucion`: Razón operativa (*Dirección inaccesible*, *Rechazo por daño físico*, *Ausencia reiterada*).
  - `fechaGeneracion`: Timestamp de inicio de retorno.
  - `unidadTransporteRetorno`: Placa del vehículo que transporta la carga de regreso.
  - `estadoRetorno`: Estado del flujo (*En Retorno*, *Ingresado a CD*, *En Inspección de Calidad*).
- **Ciclo de Vida:**
  1. *Generada en campo* $\longrightarrow$ 2. *En tránsito inverso* $\longrightarrow$ 3. *Cerrada al ingresar a almacén CD*.
- **Trabajadores que la manipulan:** Conductor / Repartidor (`gestiona`, `inicia`).
- **CUNs Vinculados:** `CUN-04`.

---

### Ficha E-08: Información de Entrega
- **Nombre de la Entidad:** `Información de Entrega`.
- **Estereotipo RUP:** `«business entity»`.
- **Naturaleza:** Entidad informativa consolidada y sincronizada.
- **Definición:** Vista unificada de los datos operativos de entrega (estado de tracking, fecha y hora de entrega efectiva, y nombre/documento del receptor real) que se disponibiliza para los agentes de atención en Salesforce y para las consultoras en el portal web.
- **Atributos Clave:**
  - `estadoConsolidado`: Situación del pedido visible en canales de atención (*Despachado*, *En Ruta*, *Entregado*, *Entrega Fallida*).
  - `fechaHoraUltimoEstado`: Timestamp de la última novedad operativa procesada.
  - `nombreReceptorReal`: Identidad de quien recibió en domicilio.
  - `dniReceptorReal`: Documento de identidad visualizado.
  - `condicionReceptor`: Titular o Persona Autorizada.
  - `latenciaSincronizacion`: Desfase temporal respecto al evento en campo (hasta ~2 horas en el AS-IS).
- **Ciclo de Vida:** Actualizada periódicamente por los procesos de sincronización desde NSDG hacia Salesforce y portales de seguimiento.
- **Trabajadores que la manipulan:** Personal SAC (`consulta`, `valida`).
- **CUNs Vinculados:** `CUN-05`.
