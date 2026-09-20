# Diccionario de Datos del Modelo de Dominio (AS-IS)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Marco Metodológico:** RUP (*Rational Unified Process*) — Catálogo Estático de Entidades del Negocio  
>
> **Navegación del Módulo de Dominio:**  
> [[01 - Diagrama de Clases de Dominio (Nivel Dominio)]] | [[02 - Diccionario de Datos del Modelo de Dominio]] | [[03 - Matriz del Modelo de Dominio]]  
> **Enlaces a la Base AS-IS:**  
> [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]

---

## 1. Presentación y Estructura del Diccionario

El **Diccionario de Datos del Modelo de Dominio** documenta exhaustivamente la totalidad de las entidades conceptuales y sus atributos identificados en el [[01 - Diagrama de Clases de Dominio (Nivel Dominio)]].

Cada atributo se describe desde una perspectiva **estrictamente de negocio**, detallando qué representa dentro de la operación actual de Yanbal Perú, su tipo conceptual (sin sintaxis de SQL o lenguaje de programación), su regla operativa y la fuente de evidencia que lo sustenta.

---

## 2. Catálogo Detallado por Entidad

### 2.1. Entidad: `Pedido`
* **Definición en el Negocio:** Unidad comercial y logística central que consolida los productos solicitados por una consultora. Es el objeto principal sobre el cual se realiza el seguimiento y control a lo largo de toda la cadena de distribución.
* **Paquete:** Núcleo de Pedido y Despacho.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `numeroPedido` | `String` | Identificador único y maestro del pedido asignado desde la captura comercial. Amarra toda la trazabilidad física y documental. | Es obligatorio, inmutable y sirve como criterio principal de búsqueda de tracking. | Entrevista min 14:43–17:41, min 22:03–22:28 |
| `estadoPedido` | `String` | Situación actual del ciclo de vida del pedido en la red de distribución. | Toma exclusivamente uno de los 4 estados AS-IS: *"Despachado"*, *"En Ruta"*, *"Entregado"* o *"Entrega Fallida"*. | Entrevista min 19:15–20:55, [[01 - Proceso actual]] §4 |
| `promesaEntrega` | `String` | Compromiso de plazo máximo de entrega contraído con la consultora según la zonificación de destino. | 24 horas para Lima Metropolitana; hasta 7 días calendario para departamentos de provincias. | Entrevista min 22:38–22:54, [[02 - Actores relevantes]] §Actor 1 |
| `fechaDespacho` | `DateTime` | Fecha y hora en que el pedido es formalmente despachado y transferido al transportista en el muelle de carga del CD. | Se registra al egresar de la Zona de Despacho (disparador del estado *"Despachado"*). | Entrevista min 17:34–19:30, `ACT-05` |
| `fechaEntregaEstimada` | `Date` | Fecha máxima calendario calculada sumando el plazo de la promesa de entrega a la fecha de despacho. | Sirve como fecha de control para la evaluación de lead times y monitoreo de demoras. | Entrevista min 22:38–22:54, `ACT-04` |

---

### 2.2. Entidad: `Bulto`
* **Definición en el Negocio:** Unidad física de carga embalada (paquete individual) que contiene los productos consolidados del pedido. Un pedido puede consolidarse en uno o más bultos según el volumen total.
* **Paquete:** Núcleo de Pedido y Despacho.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `formatoCaja` | `String` | Tipo o tamaño estandarizado de empaque asignado al bulto en el Centro de Distribución. | Corresponde a uno de los 8 formatos estandarizados de caja (Formato 1 a Formato 8). | Entrevista min 14:43–17:41, [[01 - Proceso actual]] §1 |
| `estadoFisico` | `String` | Condición material externa del embalaje durante su manipulación, traslado y recepción. | Valores de negocio: *"Íntegro"* o *"Dañado"*. Si presenta deterioro exterior, condiciona la entrega o el retorno. | Entrevista min 20:03–20:55, `ACT-10` |

---

### 2.3. Entidad: `RegistroDespacho`
* **Definición en el Negocio:** Documento o registro formal que certifica la clasificación por destino, la asignación de transporte y la salida material de la carga desde el Centro de Distribución hacia la red nacional.
* **Paquete:** Núcleo de Pedido y Despacho.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `codigoDespacho` | `String` | Identificador único del egreso formal de carga generado en la zona de despacho. | Asocia los pedidos consolidados al manifiesto de salida física del muelle. | Entrevista min 17:34–19:30, `ACT-05` |
| `fechaHoraSalida` | `DateTime` | Estampa de tiempo exacta en que la carga abandona físicamente el muelle del Centro de Distribución. | Marca el inicio formal de la custodia externa del transportista. | Entrevista min 19:15–19:53, `ACT-05` |
| `supervisorDespacho` | `String` | Identificación del Supervisor de Zona de Despacho que autoriza la salida de los pedidos. | Rol interno (*Business Worker*) responsable del control de egreso. | Entrevista min 17:34–19:30, `ACT-05` |
| `conformidadCarga` | `Boolean` | Indicador de aceptación y conformidad de entrega formal de la carga entre Yanbal y el socio logístico. | Valor verdadero (`true`) formaliza la transferencia de custodia física al transportista. | Entrevista min 17:34–19:30, [[02 - Actores relevantes]] §Actor 1 |

---

### 2.4. Entidad: `Consultora`
* **Definición en el Negocio:** Cliente primario independiente de Yanbal que realiza la orden comercial y es la titular y destinataria formal del pedido.
* **Paquete:** Comercial y Destino Geográfico.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `codigoConsultora` | `String` | Código comercial único asignado a la consultora independiente en las plataformas de Yanbal. | Es un criterio fundamental de búsqueda en los canales de consulta y CRM (Salesforce). | Entrevista min 22:03–22:28, `ACT-13` |
| `nombreTitular` | `String` | Nombres y apellidos completos de la consultora registrada como propietaria de la cuenta. | Se utiliza para validar la titularidad presencial al momento de la entrega en domicilio. | Entrevista min 24:45–25:01, `ACT-09` |
| `telefonoContacto` | `String` | Número de teléfono o celular registrado de la consultora para contacto en ruta. | Permite al socio logístico comunicarse en caso de no ubicar el domicilio o requerir confirmación. | Entrevista min 24:45–25:01, [[04 - Sistemas e información]] §4 |

---

### 2.5. Entidad: `DireccionEntrega`
* **Definición en el Negocio:** Ubicación geográfica de destino domiciliario registrada en la orden para la entrega del pedido a nivel nacional, cubriendo los 24 departamentos del Perú.
* **Paquete:** Comercial y Destino Geográfico.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `departamento` | `String` | Uno de los 24 departamentos políticos del Perú donde se ubica el punto de entrega. | Criterio primario para la zonificación, asignación de socio y modalidad de transporte. | Entrevista min 17:34–19:30, `ACT-02` |
| `provincia` | `String` | Subdivisión provincial dentro del departamento correspondiente al domicilio de entrega. | Determina el corredor logístico y la unidad de reparto secundaria. | Entrevista min 17:34–19:30, `ACT-02` |
| `distrito` | `String` | Distrito político específico de residencia de la consultora. | Delimita la última milla del transportista asignado a la zona. | Entrevista min 17:34–19:30, `ACT-02` |
| `direccionDetallada` | `String` | Vía, avenida, jirón, número exterior/interior, urbanización o referencias del domicilio. | Información física provista por la consultora para la localización exacta del inmueble. | Entrevista min 24:45–25:01, [[04 - Sistemas e información]] §4 |
| `tipoCiudad` | `String` | Clasificación logística operativa de la localidad de destino según su accesibilidad. | Valores de negocio: *"Ciudad Principal"* o *"Ciudad Alejada"*. Determina el lead time comprometido. | Entrevista min 17:34–19:30, [[02 - Actores relevantes]] §Actor 1 |

---

### 2.6. Entidad: `SocioLogistico`
* **Definición en el Negocio:** Empresa proveedora externa contratada (tercero logístico) responsable de la custodia material, el traslado físico por la red nacional y la entrega en destino.
* **Paquete:** Transporte y Red Logística.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `razonSocial` | `String` | Denominación legal o comercial de la empresa de transporte tercerizada contratada por Yanbal. | Identifica formalmente al proveedor responsable del servicio y de las contingencias. | Entrevista min 19:15–20:55, `ACT-03` |
| `ruc` | `String` | Registro Único de Contribuyentes que formaliza la personería fiscal del contratista de transporte. | Valida el marco contractual y de liquidación de fletes y seguros. | Entrevista min 19:15–20:55, [[04 - Sistemas e información]] §1 |
| `zonaAsignada` | `String` | Cobertura o región logística contractual asignada al socio logístico según la matriz de transporte. | Restringe qué departamentos o corredores viales atiende cada proveedor. | Entrevista min 17:34–19:30, `ACT-03` |

---

### 2.7. Entidad: `ModalidadTransporte` (Clase Abstracta)
* **Definición en el Negocio:** Canal o medio físico de transporte asignado al pedido para garantizar su traslado según la configuración geográfica del destino.
* **Paquete:** Transporte y Red Logística.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `tipoModalidad` | `String` | Tipificación macro del medio de transporte utilizado para el traslado. | Valores de negocio: *"Terrestre"*, *"Bimodal"* o *"Aérea"*. | Entrevista min 17:34–19:30, `ACT-03` |
| `descripcionRuta` | `String` | Descripción textual del corredor o ruta física troncal definida para el trayecto. | Detalla las vías nacionales, puertos o aeropuertos involucrados en el tramo. | Entrevista min 22:38–22:54, `ACT-07` |

#### 2.7.1. Subclase: `TransporteTerrestre`
* **Definición:** Especialización de transporte que emplea vehículos de carga terrestre sobre la red vial nacional.
* **Atributo adicional:**
  * `tipoRedVial` (`String`): Categoría de la vía transitada (*Red Troncal Costa / Vía Sierra*).

#### 2.7.2. Subclase: `TransporteBimodal`
* **Definición:** Especialización de transporte que articula un tramo terrestre con un tramo fluvial o lacustre (selva y zonas ribereñas).
* **Atributos adicionales:**
  * `tramoTerrestre` (`String`): Trayecto carretero previo al embarque fluvial.
  * `tramoFluvial` (`String`): Cuenca, río o tramo lacustre navegado en barcaza o lancha.

#### 2.7.3. Subclase: `TransporteAereo`
* **Definición:** Especialización de transporte mediante aerolínea de carga hacia destinos inaccesibles por carretera (ej. Iquitos).
* **Atributo adicional:**
  * `rutaAerea` (`String`): Tramo aéreo de conexión entre aeropuertos comerciales.

---

### 2.8. Entidad: `RegistroEntrega`
* **Definición en el Negocio:** Constancia del acto de entrega física de los bultos en el domicilio de destino, formalizada por el transportista al concretar la entrega exitosa.
* **Paquete:** Entrega y Recepción en Destino.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `fechaHoraEntrega` | `DateTime` | Estampa de tiempo exacta en que se hace la entrega material del paquete en el domicilio. | Disparador del cambio de estado a *"Entregado"* en el sistema de tracking en campo. | Entrevista min 19:15–20:55, `ACT-08` |
| `estadoConfirmacion` | `String` | Constancia formal del cierre exitoso de la distribución física. | Registra invariablemente el valor *"Entregado"*. | Entrevista min 19:15–20:55, `ACT-08` |
| `observacionesEntrega` | `String` | Anotaciones o referencias registradas por el repartidor al momento de efectuar la descarga. | Documenta si hubo alguna particularidad en la recepción en el domicilio. | Entrevista min 24:45–25:01, `ACT-08` |

---

### 2.9. Entidad: `Receptor` (Clase Abstracta)
* **Definición en el Negocio:** Persona física real que se encuentra presente en el domicilio al momento de la llegada del transportista, se identifica y asume la recepción material de los bultos.
* **Paquete:** Entrega y Recepción en Destino.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `nombreReceptor` | `String` | Nombres y apellidos de la persona que físicamente abre la puerta y recibe los paquetes. | Capturado por el socio logístico en campo al momento de la entrega. | Entrevista min 24:45–25:01, `ACT-09` |
| `documentoIdentidad` | `String` | Número de documento oficial de identidad (DNI, carné de extranjería) verificado por el transportista. | Constata formalmente la identificación de quien recibe la carga. | Entrevista min 24:45–25:01, `ACT-09` |
| `condicionReceptor` | `String` | Clasificación del vínculo legal o autorización de la persona respecto a la consultora titular. | Valores de negocio: *"Titular"* o *"Personal Autorizado"*. | Entrevista min 24:45–25:01, [[05 - Problemas y desfases]] §PR-05 |

#### 2.9.1. Subclase: `ConsultoraTitular`
* **Definición:** Especialización donde quien recibe es personalmente la consultora titular del pedido. No añade atributos adicionales al heredar la totalidad de los datos de `Receptor`.

#### 2.9.2. Subclase: `PersonalAutorizado`
* **Definición:** Especialización donde quien recibe es una persona o personal autorizado presente en el domicilio ante la ausencia de la titular.
* **Atributo adicional:**
  * `parentescoRelacion` (`String`): Vínculo, relación de parentesco o autorización declarada con la consultora titular.

---

### 2.10. Entidad: `IncidenciaEntrega`
* **Definición en el Negocio:** Registro formal de contingencia tipificada en campo por el transportista cuando el pedido no puede ser entregado con éxito a la consultora.
* **Paquete:** Contingencias y Logística Inversa.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `tipoIncidencia` | `String` | Causal estandarizada que motivó la imposibilidad de completar la entrega física. | Tipificación estricta de negocio: *"Retraso"*, *"Pérdida"* o *"Daño"*. | Entrevista min 20:03–20:55, `ACT-10` |
| `fechaHoraIncidencia` | `DateTime` | Momento exacto en que el transportista constata la contingencia y la reporta en el sistema. | Dispara la transición del pedido al estado *"Entrega Fallida"*. | Entrevista min 20:03–20:55, `ACT-10` |
| `motivoDetallado` | `String` | Descripción operativa detallada de las circunstancias del hecho en ruta o domicilio. | Registra si el domicilio estuvo cerrado, si hubo siniestro vial o avería material del paquete. | Entrevista min 20:03–20:55, `ACT-10` |
| `disponibilidadBulto` | `Boolean` | Indicador de disponibilidad física de los bultos para ser retornados hacia el almacén. | `true` en caso de Retraso o Daño (bulto físico presente); `false` en caso de Pérdida o robo. | Entrevista min 20:03–20:55, `CUN-04` |

---

### 2.11. Entidad: `OrdenRetorno`
* **Definición en el Negocio:** Documento y registro formal que ampara el inicio del traslado material de la carga no entregada hacia el Centro de Distribución mediante el flujo de logística inversa.
* **Paquete:** Contingencias y Logística Inversa.

| Atributo | Tipo Conceptual | Significado y Rol en el Negocio | Regla de Negocio / Restricción AS-IS | Fuente / Evidencia |
|---|:---:|---|---|---|
| `codigoRetorno` | `String` | Identificador único del expediente de retorno por logística inversa. | Asocia la carga física siniestrada o demorada con la orden original para su posterior peritaje. | Entrevista min 20:03–20:55, `ACT-11` |
| `fechaHoraInicioRetorno` | `DateTime` | Estampa de tiempo en que el transportista formaliza el inicio del flete de retorno al CD. | Marca el cierre formal de la frontera de distribución en el proceso AS-IS. | Entrevista min 20:03–20:55, `ACT-11` |
| `motivoRetorno` | `String` | Justificación técnica del retorno de la mercancía. | Se deriva directamente de la causal tipificada de la incidencia (Retraso o Daño). | Entrevista min 20:03–20:55, `ACT-11` |
| `estadoRetorno` | `String` | Situación operativa del traslado de logística inversa. | Registra el valor *"En Retorno"* hacia las instalaciones centrales de Yanbal. | Entrevista min 20:03–20:55, `ACT-11` |
