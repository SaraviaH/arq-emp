# Sistemas e Información del Proceso AS-IS

> **Criterio de selección:** Solo se documentan en detalle los sistemas que participan **directamente** en el proceso desde Despacho $\longrightarrow$ Transporte $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta. Los sistemas de producción, almacén y comercio se mencionan como contexto de integración.
>
> **Navegación:** [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]  
> **Requerimientos asociados:** [[01 - Matriz Consolidada de Requerimientos]] | [[03 - Especificación de Requerimientos No Funcionales]] | [[04 - Trazabilidad y Registro de Depuración]]

---

## 1. Sistemas que Intervienen en el Proceso AS-IS

### Sistema 1: NSDG (TMS y Tracking de Pedidos)

| Campo | Detalle |
|---|---|
| **Elemento** | NSDG — Sistema de Transporte y Tracking |
| **Por qué pertenece al AS-IS** | Es el sistema principal de **trazabilidad de envíos** para la operación Yanbal Perú. Registra los cambios de estado (Despachado, En Ruta, Entregado, Entrega Fallida) y gestiona la confirmación de entregas o incidencias. Es la **fuente directa** del desfase de 2h: la latencia en la sincronización desde las herramientas de transporte hacia los sistemas centrales de consulta es la causa del problema central. |
| **Fuente / Evidencia** | Entrevista min 19:38: *"seguido por un sistema de tracking, donde ya interviene este sistema para poder darle seguimiento a los demás estados del pedido"*. Min 28:00–28:10: *"tracking de pedidos se maneja a través del sistema de trackeo denominado NSDG"*. |
| **Parte del proceso donde interviene** | **Despacho $\longrightarrow$ Transporte $\longrightarrow$ Entrega $\longrightarrow$ Actualización del estado**. |

**Funcionalidades relevantes para el AS-IS:**
- Registro de salida de despacho y vinculación con transportistas.
- Transición a estado "En Ruta".
- Confirmación de entrega física o registro de incidencias (retraso, pérdida, daño).
- Registro de retorno de carga por logística inversa.

**Tipo:** Sistema desarrollado a medida por un contratista para la operación de Yanbal Perú.

---

### Sistema 2: Driving (TMS Externo)

| Campo | Detalle |
|---|---|
| **Elemento** | Driving — Transportation Management System |
| **Por qué pertenece al AS-IS** | Asiste en la **canalización de rutas, zonificación geográfica y despacho** de los socios logísticos. Interviene desde el momento en que se clasifica el pedido por destino hasta el control del transporte. |
| **Fuente / Evidencia** | Entrevista min 28:22–28:35: *"DRIVING es un sistema externo bastante conocido, es utilizado en diversas cadenas logísticas de otros sectores"*. Min 31:00–31:16. |
| **Parte del proceso donde interviene** | **Despacho $\longrightarrow$ Transporte** (canalización de rutas y soporte a la zonificación). |

**Funcionalidades relevantes para el AS-IS:**
- Canalización de rutas de distribución.
- Soporte a la clasificación geográfica según los 24 departamentos.
- Gestión de transportistas asociados.

**Tipo:** TMS de proveedor externo especializado.

---

### Sistema 3: Salesforce / Cellforce (CRM de Servicio al Cliente)

| Campo | Detalle |
|---|---|
| **Elemento** | Salesforce (internamente denominado Cellforce) — CRM en la nube |
| **Por qué pertenece al AS-IS** | Es el sistema donde los **agentes de Servicio al Cliente consultan el estado de los pedidos** al atender consultas de consultoras. Recibe los datos con desfase, manifestando el problema central: el agente ve información desactualizada (pedido "En ruta" cuando ya fue entregado) y no puede dar respuestas certeras. |
| **Fuente / Evidencia** | Entrevista min 28:35–28:59: *"sistemas ya de gestión de servicio al cliente, específicamente denominado CELLFORCE para hacer este tipo de gestiones de consultas, reclamos y otro tipo de trámites con el cliente final"*. |
| **Parte del proceso donde interviene** | **Consulta / Trazabilidad** (visualización de estados y atención de consultas de consultoras). |

**Funcionalidades relevantes para el AS-IS:**
- Visualización de la situación del pedido y promesa de entrega mediante Número de Pedido o Código de Consultora.
- Visualización de datos del receptor cuando el pedido está entregado.

> [!NOTE]
> La gestión de tickets de reclamo postventa y solicitudes de reposición en Salesforce corresponde a procesos de soporte de CRM/Mesa de Ayuda, fuera del alcance delimitado de trazabilidad logística de distribución.

---

### Sistema 4: Bus de Integración (Middleware / ESB Corporativo)

| Campo | Detalle |
|---|---|
| **Elemento** | Bus de Integración Corporativo |
| **Por qué pertenece al AS-IS** | Es el **canal central** por donde se transmiten las actualizaciones de estado desde las herramientas de transporte hacia los sistemas centrales y de atención. Es el componente que interconecta los sistemas del ecosistema Yanbal. |
| **Fuente / Evidencia** | Entrevista min 29:00–29:09: *"el bus de integración que amarra todos estos sistemas y hace que converse en uno entre ellos"*. Min 2:12–2:20 p2: *"se han hecho las adaptaciones para que se integren de forma adecuada a nuestros tres sistemas internos"*. |
| **Parte del proceso donde interviene** | **Integración transversal** entre sistemas de transporte y consulta. |

**Funcionalidades relevantes para el AS-IS:**
- Orquestación y paso de mensajes de estado entre sistemas de transporte y CRM.
- Mecanismo corporativo de integración para evitar acoplamientos desordenados.

---

## 2. Sistemas Previos al Alcance (Solo Contexto)

| Sistema | Función en Yanbal | Razón de Exclusión del Alcance Detallado |
|---|---|---|
| **SAP R3** (ERP) | Control de stock, inventario y estados contables | Opera en manufactura y almacén general. Concluye antes del despacho de pedidos. |
| **SPY** (WMS in-house) | Gestión de tareas de recolección en picking unitario | Opera dentro de la línea de preparación del Centro de Distribución. |
| **Maya** (Plataforma comercial) | Captura de pedidos de consultoras | Etapa comercial previa a la preparación física. |
| **SAP Commerce** | Procesamiento transaccional de ventas | Etapa comercial previa a la preparación física. |

---

## 3. Flujo de Información en el Proceso AS-IS

```mermaid
flowchart LR
    subgraph CONTEXTO_PREVIO ["Contexto Previo"]
        MAYA["Maya / SAP Commerce<br/>(Orden comercial)"] --> SPY_S["SPY<br/>(Picking unitario)"]
    end

    subgraph SISTEMAS_AS_IS ["Sistemas del Proceso AS-IS"]
        DRIVING["Driving<br/>(Canalización de rutas)"]
        NSDG["NSDG<br/>(TMS y Tracking de transporte)"]
        BUS["Bus de Integración<br/>(Middleware corporativo)"]
        PORTAL["Portal Web / Canal de Tracking<br/>(Consulta para consultoras)"]
        SF["Salesforce (Cellforce)<br/>(Consulta en Servicio al Cliente)"]
    end

    subgraph ACTORES ["Actores"]
        SUP["Supervisor Despacho"]
        SOCIO["Socio Logístico"]
        CONS["Consultora"]
        AG["Agente Atención"]
    end

    SPY_S -->|Caja empacada con N° pedido| SUP
    SUP -->|Clasifica destino y asigna| DRIVING
    SUP -->|Registra salida 'Despachado'| NSDG
    NSDG -->|Asume carga e inicia 'En Ruta'| SOCIO
    SOCIO -->|Reporta entrega o incidencia| NSDG
    NSDG -.->|Sincronización (Desfase ~2h)| BUS
    BUS -->|Estados y datos de entrega| PORTAL
    BUS -->|Actualización de estado y receptor| SF
    CONS -->|Consulta por N° o Código| PORTAL
    AG -->|Consulta por N° o Código| SF
```

---

## 4. Información que Viaja y se Genera en el Proceso

### Datos del Pedido (Presentes desde Despacho)

| Dato | Origen del Dato | Quién lo Utiliza en el AS-IS | Para qué se Utiliza |
|---|---|---|---|
| **Número de pedido** | Orden comercial | Todos los actores | Identificador maestro que amarra toda la trazabilidad |
| **Nombre del destinatario** | Datos comerciales | Socio logístico / Agente | Identificar a la consultora titular |
| **Código de consultora/cliente** | Plataforma comercial | Consultora / Agente | Criterio de búsqueda para consulta de trazabilidad |
| **Dirección de entrega** | Datos comerciales | Supervisor / Transportista | Clasificación geográfica por departamento, provincia, distrito y ciudad (principal/alejada) |
| **Socio logístico asignado** | Asignación en despacho | Transportista / Sistemas | Identificar a la empresa responsable del traslado |
| **Modalidad de envío** | Asignación en despacho | Transportista | Determinar modo de transporte: terrestre, bimodal o aérea |
| **Promesa estimada de entrega** | Regla de lead time | Consultora / Agente | Promesa de servicio: 24h Lima Metropolitana / hasta 7 días provincias |
| **Estado del pedido** | Transiciones en proceso | Todos los actores y sistemas | Trazabilidad (*Despachado, En Ruta, Entregado, Entrega Fallida*) |

### Datos que se Generan Durante el Proceso

| Dato Generado | En qué Etapa se Genera | Quién lo Genera | Dónde se Registra |
|---|---|---|---|
| **Registro de salida de despacho** | Despacho | Supervisor de Despacho | Driving / NSDG (Estado *"Despachado"*) |
| **Registro de inicio de traslado** | Transporte | Socio Logístico | NSDG / Driving (Estado *"En Ruta"*) |
| **Confirmación de entrega física** | Entrega | Socio Logístico | NSDG (Estado *"Entregado"*) |
| **Identidad del receptor real** | Entrega | Socio Logístico | NSDG (nombre y condición: titular o persona autorizada) |
| **Registro de incidencia** | Entrega fallida | Socio Logístico | NSDG / Driving (causales: retraso, pérdida o daño) |
| **Registro de retorno** | Logística inversa | Socio Logístico | NSDG (retorno físico hacia el Centro de Distribución) |

> [!NOTE]
> **Aclaración de depuración:** Se eliminó *"manifiesto de carga"* (no existe en las fuentes) y se descartaron los *"tickets de reclamo"* y *"solicitudes de reposición"* por corresponder a procesos externos de CRM y almacén.
