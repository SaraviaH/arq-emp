# Glosario de Nombres y Términos: Entrevista Yanbal

> **Navegación:** [[Índice de información]] | [[Transcripción original]] | [[Aclaración adicional del entrevistado]] | [[Inventario documental]] | [[Matriz de inconsistencias]] | [[Informe final de revisión]]

---

## 1. Convención de Estado Epistémico

- **✅ Confirmado:** Respaldado de manera inequívoca en [[Transcripción original]] (`[TO]`) o en la [[Aclaración adicional del entrevistado]] (`[AC]`).
- **🔷 Interpretación fundada:** Deducción razonable o término técnico estándar adoptado por el equipo a partir del contexto de la entrevista.
- **❓ Dudoso / No confirmado:** Denominación ambigua, discrepancia fonética o dato pendiente de validación formal con la jefatura de Yanbal.

---

## 2. Entidades Organizacionales, Personas y Cargos

| Término / Entidad | Estado | Definición y Alcance en el Proyecto | Evidencia en Fuente |
| :--- | :---: | :--- | :--- |
| **Yanbal** | ✅ | Corporación empresarial del sector cosmético. En la documentación se analiza específicamente la operación de la cadena de distribución en Perú. | [TO 0:21; 4:35; 9:37; 28:22] |
| **Ing. Joao Condorpusa Mendoza** | ✅ | Profesional entrevistado. Desempeña el rol de **Encargado del Área de Distribución de Yanbal (Perú)**. | [TO 0:19–0:21] |
| **Área de Distribución** | ✅ | Área funcional a cargo del entrevistado, responsable de la coordinación de entregas, seguimiento de transportistas y cumplimiento de promesas de entrega. | [TO 0:19–0:21] |
| **Área de Manufactura** | ✅ | Área a cargo de la parte productiva y la elaboración física de los productos cosméticos. | [TO 1:40] |
| **Área de Envasado** | ✅ | Etapa productiva donde se dosifican los productos elaborados y se empacan en cajas mono SKU con Código UA. | [TO 1:01; 1:48] |
| **Área de Almacén de Productos Terminados** | ✅ | Instalación de resguardo de stock en pallets. Es la única área que opera directamente el ERP SAP R3. | [TO 1:09; 1:55; 8:50] |
| **Centro de Distribución (CD)** | ✅ | Infraestructura dedicada a la desconsolidación de cajas master, picking unitario y consolidación de pedidos en 8 tipos de cajas. | [TO 1:12; 2:01; 4:01; 9:04] |
| **Zona de Despacho** | ✅ | Espacio operativo donde los pedidos consolidados son zonificados geográficamente para ser entregados a los transportistas. | [TO 17:26; 19:04] |
| **Proveedores Logísticos Asociados / Socios Logísticos** | ✅ | Empresas contratistas terceras responsables del transporte y traslado físico nacional en modalidades terrestre, bimodal y aérea. | [TO 1:24; 18:02; 19:26–19:30] |
| **Consultores / Consultoras** | ✅ | Clientes primarios de la cadena logística de Yanbal («para nosotros un consultor o una consultora es nuestro cliente»), quienes colocan órdenes en Maya. | [TO 21:35; 22:11] |
| **Cliente Final / Persona Autorizada** | ✅ | Destinatarios en el punto físico de entrega domiciliaria. La persona autorizada es quien puede recibir el paquete en ausencia del titular. | [TO 1:24; 24:40–24:45] |
| **Servicio al Cliente** | ✅ | Canal y función de atención a reclamos y consultas de pedidos que utiliza el sistema CELLFORCE. | [TO 24:36; 28:35–28:44] |
| **Distribuidoras** | ❓ | Entidades de destino mencionadas en la aclaración adicional `[AC]` («seguimiento de entregas desde la fábrica hacia sus distribuidoras y otras sedes»). Requiere aclarar si son sedes propias, concesionarios o intermediarios. | `[AC]` |
| *Áreas de Control de Calidad, Seguridad Patrimonial y Seguridad Informática* | 🔷 | No mencionadas como gerencias autónomas; en la entrevista corresponden a **criterios técnicos de evaluación** y validación en almacén y sistemas. | [TO 10:26; 21:04; s2 1:37] |

---

## 3. Catálogo de Sistemas y Software

| Nombre del Sistema | Denominación Textual en Entrevista | Estado | Función Verificada |
| :--- | :--- | :---: | :--- |
| **SAP R3** | «sistema SAP, es una ERP» [TO 2:25], «SAP R3» [TO 8:20; 27:17] | ✅ | ERP core manejado únicamente por el área de almacén para controlar inventario en vivo, tres estados de stock y reportes de ubicación. |
| **SPY** | «SPY por las siglas de sistema de picking de Yanbal» [TO 9:12] | ✅ | BWMS desarrollado *in-house* para el Centro de Distribución. Calcula volumetrías para asignar 1 de 8 cajas y guía las tareas de picking. Transcrito también como *Spy* [TO 15:16] y *Spike* [TO 17:12]. |
| **Maya** | «plataforma comercial denominada Maya» [TO 14:49] | ✅ | Aplicación web/móvil utilizada por consultoras para registrar pedidos. Se conecta con SAP Commerce. |
| **SAP Commerce** | «sistema comercial de Zap mismo, ZapCommerce» [TO 14:57] | 🔷 | Motor de procesamiento transaccional de ventas que recibe pedidos de Maya y los deriva al Bus de integración. |
| **Bus de Integración** | «bus de integración» [TO 15:06; 28:59] | ✅ | Middleware corporativo que interconecta y hace conversar bidireccionalmente a los 7 sistemas de la cadena. |
| **NSDG** | «sistema de transporte… denominado NSDG» [TO 9:21–9:37] | ✅ | Software de transporte desarrollado por un contratista especializado para la operación de Yanbal en Perú. |
| **ENSDY** | «sistema de trackeo denominado ENSDY» [TO 28:10] | ❓ | Término fonético utilizado por el entrevistado al referirse al tracking. Se registra la hipótesis de tratarse de la misma herramienta NSDG o de un submódulo de tracking del mismo contratista. |
| **DRIVING** | «de transporte y despacho se maneja DRIVING» [TO 28:10] | ✅ | TMS externo de mercado empleado en minería, alimentos y cosméticos para ruteo, despacho y flota de socios logísticos. |
| **CELLFORCE** | «específicamente denominado CELLFORCE» [TO 28:44] | ✅ (❓) | Software utilizado por Servicio al Cliente para atender reclamos y consultas. Corresponde con alta probabilidad (🔷) al CRM comercial *Salesforce*. |
| **Terminales RF** | «terminales de radiofrecuencia» [TO 7:28] | ✅ | Hardware móvil utilizado para leer códigos UA en muelle de recepción y racks. |

---

## 4. Conceptos Técnicos y Procesos Logísticos

| Concepto / Término | Estado | Explicación Técnica y Evidencia |
| :--- | :---: | :--- |
| **Código UA (*Unidad de Almacenamiento*)** | ✅ | Código identificador adherido a cada caja master mono SKU desde manufactura para su captura por radiofrecuencia y control en SAP R3 [TO 7:11–7:26]. |
| **Cajas Mono SKU** | ✅ | Cajas de embalaje que contienen única y exclusivamente unidades de un mismo código de producto sin mezclar referencias a la salida de producción [TO 3:20]. |
| **Cajas Master Selladas** | ✅ | Cajas completas y precintadas que viajan desde el almacén hacia el Centro de Distribución sin apertura previa ni retiros parciales de saldos [TO 3:40–3:52]. |
| **Formatos de Caja de Pedido (8 tipos)** | ✅ | Ocho tamaños estandarizados de cajas de despacho asignados automáticamente según el cálculo y consolidación teórica de peso y volumetría de la orden [TO 15:59–16:17]. |
| **Zonificación** | ✅ | Clasificación física y lógica de los pedidos embalados según su destino geográfico para su asignación a rutas en los 24 departamentos [TO 19:04–19:23]. |
| **Envío Bimodal** | ✅ | Modalidad de transporte combinada (terrestre + fluvial o lacustre) utilizada para atender localidades con accesos fluviales en selva y sierra (transcrito «biomodal») [TO 19:26]. |
| **Lead Times de Entrega** | ✅ | Compromisos de entrega de Yanbal: **24 horas** para Lima Metropolitana y **hasta 7 días** para ciudades y localidades de provincias [TO 22:38–22:54]. |
| **Merma Operativa** | ✅ | Productos dañados o deteriorados por manipulación física durante las labores de picking y preparación, acumulados durante el turno laboral [TO 12:43–12:59]. |
| **Desfase de Merma (hasta 6 horas)** | ✅ | Brecha temporal entre el momento en que se rompe físicamente un producto y su digitación contable en SAP R3 al cierre de la jornada de trabajo [TO 13:08–13:18]. |
| **Desfase de Tracking (hasta 2 horas)** | ✅ | Retardo promedio de actualización de los sistemas de transporte que provoca una ventana de desconocimiento sobre la entrega efectiva [TO 23:41–24:09; s2 0:49]. |
| **Quiebre de Stock (*Stockout*)** | ✅ | Situación en la que el sistema comercial ofrece unidades comprometidas que ya no existen por falta de sinceramiento de merma, derivando en venta perdida [TO 13:42–14:14]. |
| **Persona Autorizada** | ✅ | Tercero que recibe formalmente el pedido en el domicilio de entrega en ausencia de la consultora titular [TO 24:40–24:45]. |
| **Logística Inversa** | ✅ | Flujo de retorno del pedido devuelto hacia el almacén gestionado por el socio logístico ante retraso, daño o pérdida, con posterior peritaje técnico de calidad y seguridad [TO 20:25–21:35]. |
| *Y-Trace / Cláusula H-16* | 🔷❓ | Términos internos del proyecto académico para nombrar el prototipo y delimitar el alcance a distribución troncal. **No forman parte de la entrevista** y están pendientes de validación externa. |
