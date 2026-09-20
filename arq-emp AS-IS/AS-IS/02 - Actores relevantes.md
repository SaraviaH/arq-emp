# Actores Relevantes del Proceso AS-IS

> **Criterio de selección:** Solo se incluyen actores que participan **directamente** en el proceso desde Despacho $\longrightarrow$ Transporte $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta/Trazabilidad. Los actores de producción, envasado, almacén, picking y otras áreas internas quedan documentados como contexto en la información general, pero **no forman parte del modelado AS-IS**.
>
> **Navegación:** [[01 - Proceso actual]] | [[03 - Actividades y eventos]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]  
> **Requerimientos asociados:** [[01 - Matriz Consolidada de Requerimientos]] | [[04 - Trazabilidad y Registro de Depuración]]

---

## Registro de Selección de Actores

### Actor 1: Supervisores de Zona de Despacho

| Campo | Detalle |
|---|---|
| **Elemento** | Supervisores de la Zona de Despacho |
| **Por qué pertenece al AS-IS** | Son quienes ejecutan la clasificación geográfica de los pedidos (canalización por departamento, provincia, distrito, tipo de ciudad) y realizan la entrega formal de la carga al socio logístico. Es el **punto de inicio** del alcance del modelado. |
| **Fuente / Evidencia** | Entrevista min 17:34–19:30: *"pasa a la zona de despacho [...] donde es zonificado [...] canalización a través de la dirección de entrega para ser distribuido por regiones"*. Documento [[Transporte y logística]] §2. |
| **Parte del proceso donde interviene** | **Despacho** — Clasifica pedidos por destino, registra socio logístico y modalidad de envío, entrega la carga. |
| **Correspondencia con Requerimientos** | [[02 - Especificación de Requerimientos Funcionales#RF-01\|RF-01]], [[02 - Especificación de Requerimientos Funcionales#RF-02\|RF-02]], [[02 - Especificación de Requerimientos Funcionales#RF-03\|RF-03]], [[02 - Especificación de Requerimientos Funcionales#RF-04\|RF-04]]. |

**Actividades actuales que realiza en el proceso:**
1. Recibe los pedidos empacados desde la línea de picking en zona de despacho.
2. Clasifica y agrupa los pedidos por destino geográfico (24 departamentos, provincias, distritos, tipo de ciudad: principal o alejada).
3. Vincula el socio logístico correspondiente a la ruta.
4. Registra la modalidad de envío aplicable (terrestre, bimodal, aérea).
5. Asocia la promesa estimada de entrega según la región (24h Lima / hasta 7d provincias).
6. Registra la salida formal de despacho y entrega la carga al transportista (estado *"Despachado"*).

**Sistemas que utiliza:** Driving (para canalización de rutas), NSDG (para registro de despacho).

---

### Actor 2: Socios Logísticos / Transportistas Asociados

| Campo | Detalle |
|---|---|
| **Elemento** | Proveedores logísticos asociados (socios de transporte) — empresas contratistas |
| **Por qué pertenece al AS-IS** | Son los actores que ejecutan el **núcleo** del transporte: asumen la carga en despacho, la trasladan a través de los 24 departamentos por modalidades terrestre, bimodal o aérea, entregan el paquete al receptor y reportan la entrega o incidencias. En caso de entrega fallida, inician el retorno por logística inversa. |
| **Fuente / Evidencia** | Entrevista min 19:15–20:55: *"seguido por un sistema de tracking [...] pedido en ruta [...] pedido entregado [...] si se presenta alguna incidencia [...] retraso, pérdida o daño [...] el mismo socio logístico hace el registro para hacer un proceso de logística inversa"*. |
| **Parte del proceso donde interviene** | **Transporte $\longrightarrow$ Entrega $\longrightarrow$ Retorno (Logística Inversa)**. |
| **Correspondencia con Requerimientos** | [[02 - Especificación de Requerimientos Funcionales#RF-05\|RF-05]], [[02 - Especificación de Requerimientos Funcionales#RF-06\|RF-06]], [[02 - Especificación de Requerimientos Funcionales#RF-07\|RF-07]], [[02 - Especificación de Requerimientos Funcionales#RF-08\|RF-08]], [[02 - Especificación de Requerimientos Funcionales#RF-09\|RF-09]]. |

**Actividades actuales que realiza en el proceso:**
1. Asume la carga en el Centro de Distribución y registra el inicio del traslado (estado *"En Ruta"*).
2. Realiza el traslado físico según la modalidad asignada (terrestre, bimodal o aérea).
3. Entrega el pedido al receptor físico en destino.
4. Registra la entrega completada (estado *"Entregado"*).
5. Captura quién recibió el paquete (si fue la consultora titular o una persona autorizada).
6. Registra la incidencia si la entrega no se concreta por retraso, pérdida o daño (estado *"Entrega Fallida"*).
7. Inicia el registro de retorno físico del pedido no entregado al almacén (logística inversa).

**Sistemas que utiliza:** NSDG / Driving.

---

### Actor 3: Consultora / Consultor de Yanbal (Cliente Primario)

| Campo | Detalle |
|---|---|
| **Elemento** | Consultoras y consultores independientes de Yanbal |
| **Por qué pertenece al AS-IS** | Son los **clientes primarios** de la red logística de Yanbal y los receptores directos del servicio. Consultan la trazabilidad de su pedido, reciben la entrega o reportan dudas ante el desfase de información. |
| **Fuente / Evidencia** | Entrevista min 22:03–22:28, min 24:45–25:01: *"la información necesaria que el usuario o el cliente debe conocer es el número del pedido [...] también puede hacer trazabilidad o seguimiento con el código de cliente o el código de consultor [...] para nosotros un consultor o una consultora es nuestro cliente"*. |
| **Parte del proceso donde interviene** | **Entrega $\longrightarrow$ Consulta/Trazabilidad**. |
| **Correspondencia con Requerimientos** | [[02 - Especificación de Requerimientos Funcionales#RF-11\|RF-11]], [[02 - Especificación de Requerimientos Funcionales#RF-12\|RF-12]]. |

**Actividades actuales que realiza en el proceso:**
1. Espera la entrega de su pedido en el domicilio registrado.
2. Recibe el paquete del transportista (cuando se encuentra presente).
3. Consulta la situación de su pedido utilizando su Número de Pedido o Código de Consultora.
4. Verifica la promesa de entrega (24h Lima / hasta 7d provincias).
5. Contacta a Servicio al Cliente si percibe demoras o desconoce quién recibió su pedido.

---

### Actor 4: Persona Autorizada / Receptor en Domicilio

| Campo | Detalle |
|---|---|
| **Elemento** | Persona autorizada presente en el domicilio de entrega |
| **Por qué pertenece al AS-IS** | Es quien **recibe físicamente el paquete** cuando la consultora titular no se encuentra presente en el domicilio. La falta de visibilidad sobre su identidad como receptor en los sistemas de consulta es la causa directa del problema **PR-05** (reclamos falsos por supuesta pérdida). |
| **Fuente / Evidencia** | Entrevista min 24:45–25:01: *"No necesariamente el cliente final es el que recibe la entrega del pedido, sino puede ser alguna persona autorizada y el cliente final, al hacer la trazabilidad a través de nuestro sistema, no necesariamente tiene esa información"*. |
| **Parte del proceso donde interviene** | **Entrega** — Recepción física en domicilio. |
| **Correspondencia con Requerimientos** | [[02 - Especificación de Requerimientos Funcionales#RF-07\|RF-07]], [[02 - Especificación de Requerimientos Funcionales#RF-12\|RF-12]]. |

**Actividades actuales que realiza en el proceso:**
1. Recibe físicamente el paquete en el domicilio de destino.
2. Se identifica ante el socio logístico como receptor autorizado.

---

### Actor 5: Agentes de Servicio al Cliente

| Campo | Detalle |
|---|---|
| **Elemento** | Operadores del centro de atención y servicio al cliente |
| **Por qué pertenece al AS-IS** | Son los operadores que **consultan la información de trazabilidad** en los sistemas corporativos para atender consultas de las consultoras. Padecen directamente el desfase de hasta 2 horas en los datos, lo que les impide dar respuestas confiables. |
| **Fuente / Evidencia** | Entrevista min 24:29–24:36, min 28:35–28:59: *"para poder responder ante algún reclamo o ante la consulta del mismo cliente final a través de nuestro servicio al cliente acerca del estatus de su pedido"*. |
| **Parte del proceso donde interviene** | **Consulta/Trazabilidad** — Consulta de estados y atención de solicitudes de información de distribución. |
| **Correspondencia con Requerimientos** | [[02 - Especificación de Requerimientos Funcionales#RF-11\|RF-11]], [[02 - Especificación de Requerimientos Funcionales#RF-12\|RF-12]]. |

**Actividades actuales que realiza en el proceso de trazabilidad:**
1. Recibe consultas de consultoras sobre la situación de sus despachos.
2. Consulta el estado del pedido y la promesa de entrega mediante Número de Pedido o Código de Consultora.
3. Verifica la información registrada sobre la persona receptora para aclarar consultas sobre entregas a personas autorizadas.

> [!NOTE]
> La gestión interna de tickets de quejas y reclamos comerciales pertenece al proceso de CRM/Mesa de Ayuda, externo al alcance de la trazabilidad logística de distribución.

---

### Actor 6: Jefatura de Distribución (Ing. Joao Condorpusa Mendoza)

| Campo | Detalle |
|---|---|
| **Elemento** | Encargado del Área de Distribución de Yanbal (Perú) |
| **Por qué pertenece al AS-IS** | Es el **responsable de gobierno** de todo el proceso desde el despacho hasta la entrega física. Supervisa los lead times, evalúa a los socios logísticos y es quien declara las necesidades de reducción del desfase temporal y cobertura multimodal. |
| **Fuente / Evidencia** | Entrevista completa (`trascrito.text`). |
| **Parte del proceso donde interviene** | **Gobierno transversal** del proceso logístico de distribución. |
| **Correspondencia con Requerimientos** | [[03 - Especificación de Requerimientos No Funcionales#RNF-01\|RNF-01]], [[03 - Especificación de Requerimientos No Funcionales#RNF-02\|RNF-02]], [[03 - Especificación de Requerimientos No Funcionales#RNF-05\|RNF-05]]. |

---

### Actor 7: Sistema de Sincronización e Integración (Actor de Sistema / Middleware)

| Campo | Detalle |
|---|---|
| **Elemento** | Componentes automatizados del ecosistema (NSDG / Bus de Integración Corporativo) |
| **Por qué pertenece al AS-IS** | En la arquitectura del proceso, la sincronización entre los sistemas donde el transportista registra eventos en campo y las plataformas corporativas donde consultan las consultoras y agentes de atención se ejecuta de manera automatizada. Es el componente responsable del procesamiento y disponibilidad de datos, y donde se origina la latencia actual de hasta 2 horas. |
| **Fuente / Evidencia** | Entrevista min 23:19–24:18, min 29:00–29:09: *"el bus de integración que amarra todos estos sistemas [...] el sistema de trackeo denominado NSDG [...] el sistema todavía se actualiza hasta en un lapso de dos horas"*. |
| **Parte del proceso donde interviene** | **Actualización / Sincronización transversal** de estados. |
| **Correspondencia con Requerimientos** | [[02 - Especificación de Requerimientos Funcionales#RF-10\|RF-10]] (Sincronización de estados), [[03 - Especificación de Requerimientos No Funcionales#RNF-01\|RNF-01]] (Latencia $\le 30$ min), [[03 - Especificación de Requerimientos No Funcionales#RNF-03\|RNF-03]] (Capacidad gran volumen), [[03 - Especificación de Requerimientos No Funcionales#RNF-04\|RNF-04]] (Bus corporativo), [[03 - Especificación de Requerimientos No Funcionales#RNF-06\|RNF-06]] (Seguridad informática). |

**Actividades que realiza en el proceso:**
1. Recibe los registros de despacho, tránsito, entrega física e incidencias reportados por los actores operativos.
2. Orquesta el paso de mensajes de estado a través del Bus corporativo hacia los repositorios de consulta.
3. Disponibiliza la información actualizada para los canales de consulta web y CRM (Salesforce).

---

## Actores Excluidos del Modelado Detallado

| Actor Excluido | Área de Origen | Razón Metodológica de Exclusión |
|---|---|---|
| Personal de Manufactura y Envasado | Producción | Proceso anterior al despacho de pedidos |
| Operarios de Almacén de PT | Almacenamiento | Gestión previa en pallets (6,800 posiciones) |
| Operarios de Picking del CD (SPY) | Centro de Distribución | Preparación unitaria que concluye antes de despacho |
| Inspectores de Control de Calidad | Calidad | Evaluación interna de peritaje en almacén (fuera del alcance) |
| Analistas de Seguridad Patrimonial | Prevención de Pérdidas | Evaluación de siniestros interna de almacén (fuera del alcance) |
| Operadores de Plataforma Comercial | Ventas (Maya / SAP Commerce) | Toma y facturación de pedidos (etapa previa comercial) |
