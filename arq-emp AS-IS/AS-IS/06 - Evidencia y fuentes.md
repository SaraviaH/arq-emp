# Evidencia y Fuentes del Proceso AS-IS

> **Propósito:** Este documento registra la **trazabilidad documental** de cada elemento seleccionado para el AS-IS. Permite verificar de dónde proviene cada dato, por qué fue incluido y en qué documento fuente se encuentra la evidencia.
>
> **Navegación:** [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]]  
> **Requerimientos asociados:** [[01 - Matriz Consolidada de Requerimientos]] | [[04 - Trazabilidad y Registro de Depuración]]

---

## 1. Fuentes Primarias Utilizadas

| ID Fuente | Documento | Ubicación | Tipo | Confiabilidad |
|---|---|---|---|---|
| **F-01** | Transcripción original de la entrevista | [[Transcripción original]] / `trascrito.text` | Transcripción textual de la entrevista grabada al Ing. Joao Condorpusa Mendoza | ✅ Alta — Declaraciones directas del responsable del área |
| **F-02** | Grabación audiovisual | `ENTREVISTA.mp4` (referenciada en el índice) | Video de la entrevista | ✅ Alta — Fuente primaria verificable |
| **F-03** | Transporte y logística | [[Transporte y logística]] (`02 - Transporte/`) | Documento derivado de la entrevista | ✅ Alta — Extraído y organizado desde F-01 |
| **F-04** | Trazabilidad | [[Trazabilidad]] (`05 - Trazabilidad/`) | Documento derivado de la entrevista | ✅ Alta — Extraído y organizado desde F-01 |
| **F-05** | Sistemas y tecnología | [[Sistemas y tecnología]] (`06 - Sistemas/`) | Documento derivado de la entrevista | ✅ Alta — Extraído y organizado desde F-01 |
| **F-06** | Problemas y necesidades | [[Problemas y necesidades]] (`07 - Problemas/`) | Documento derivado de la entrevista | ✅ Alta — Extraído y organizado desde F-01 |
| **F-07** | Actores y responsabilidades | [[Actores y responsabilidades]] (`08 - Actores/`) | Documento derivado de la entrevista | ✅ Alta — Extraído y organizado desde F-01 |

---

## 2. Trazabilidad de Actores

| Actor Seleccionado | Fuente Primaria (`trascrito.text`) | Cita Textual Específica | Verificado |
|---|---|---|:---:|
| Supervisores de Zona de Despacho | F-01 (min 17:34–19:04) | *"pasa a la zona de despacho [...] donde es zonificado"* | ✅ |
| Socios Logísticos / Transportistas | F-01 (min 1:24, 19:15–20:55) | *"proveedores logísticos asociados [...] el mismo socio logístico hace el registro"* | ✅ |
| Consultora / Consultor | F-01 (min 22:03–22:28) | *"la información necesaria que el usuario o el cliente debe conocer es el número del pedido [...] para nosotros un consultor o una consultora es nuestro cliente"* | ✅ |
| Persona Autorizada / Receptor en Domicilio | F-01 (min 24:45–25:01) | *"puede ser alguna persona autorizada y el cliente final, al hacer la trazabilidad [...] no necesariamente tiene esa información"* | ✅ |
| Agentes de Servicio al Cliente | F-01 (min 24:29–24:36) | *"para poder responder ante algún reclamo o ante la consulta del mismo cliente final a través de nuestro servicio al cliente acerca del estatus de su pedido"* | ✅ |
| Jefatura de Distribución (Ing. Joao) | F-01 (Entrevista completa) | Declaraciones directas del encargado de distribución a lo largo de toda la entrevista | ✅ |

---

## 3. Trazabilidad de Sistemas

| Sistema Seleccionado | Fuente Primaria (`trascrito.text`) | Cita Textual Específica | Verificado |
|---|---|---|:---:|
| NSDG (Tracking) | F-01 (min 28:00–28:10) | *"tracking de pedidos se maneja a través del sistema de trackeo denominado NSDG"* | ✅ |
| Driving (TMS) | F-01 (min 28:22–28:35) | *"DRIVING es un sistema externo bastante conocido, es utilizado en diversas cadenas logísticas"* | ✅ |
| Salesforce / Cellforce | F-01 (min 28:35–28:59) | *"CELLFORCE para hacer este tipo de gestiones de consultas, reclamos y otro tipo de trámites con el cliente final"* | ✅ |
| Bus de Integración | F-01 (min 29:00–29:09) | *"el bus de integración que amarra todos estos sistemas y hace que converse en uno entre ellos"* | ✅ |

---

## 4. Trazabilidad de Problemas

| Problema Seleccionado | Fuente Primaria (`trascrito.text`) | Cita Textual Específica | Verificado |
|---|---|---|:---:|
| PR-03: Desfase de hasta 2h en tracking | F-01 (min 23:19–24:18) | *"nuestro sistema de tracking nos arroja una trazabilidad [...] en promedio de dos horas [...] el sistema todavía se actualiza hasta en un lapso de dos horas"* | ✅ |
| PR-04: Desconocimiento de situación en atención | F-01 (min 24:29–24:36) | *"para poder responder ante algún reclamo o ante la consulta del mismo cliente final a través de nuestro servicio al cliente acerca del estatus de su pedido"* | ✅ |
| PR-05: Receptor real no comunicado (persona autorizada) | F-01 (min 24:45–25:01) | *"puede ser alguna persona autorizada y el cliente final, al hacer la trazabilidad [...] no necesariamente tiene esa información. Entonces es ahí donde está nuestro reto"* | ✅ |

---

## 5. Trazabilidad de Estados del Pedido

| Estado | Fuente Primaria (`trascrito.text`) | Pasaje Específico | Verificado |
|---|---|---|:---:|
| **Despachado** (o Pedido despachado) | F-01 (min 19:44) | *"pasa hacia la zona de despacho o pedido despachado"* | ✅ |
| **En Ruta** (o Pedido en ruta) | F-01 (min 19:44–19:53) | *"pedido en ruta, que es el siguiente estado"* | ✅ |
| **Entregado** (o Pedido entregado) | F-01 (min 19:53) | *"pedido entregado cuando ya es finalizado y entregado hacia el cliente final"* | ✅ |
| **Entrega Fallida** | F-01 (min 20:03–20:15) | *"si durante ese transporte se presenta alguna incidencia [...] entrega fallida"* (causales: retraso, pérdida, daño) | ✅ |

---

## 6. Trazabilidad de Cifras y Métricas

| Métrica / Parámetro | Valor Declarado | Fuente Primaria (`trascrito.text`) | Cita Textual Específica | Estado de Certeza |
|---|---|---|---|:---:|
| Desfase en tracking | Hasta 2 horas (~120 min) | F-01 (min 23:41–24:09) | *"en promedio de dos horas [...] el sistema todavía se actualiza hasta en un lapso de dos horas"* | `[Explícito AS-IS]` |
| Lead time Lima | 24 horas | F-01 (min 22:47) | *"manejamos 24 horas para la zona de Lima, todo Lima metropolitana"* | `[Explícito AS-IS]` |
| Lead time provincias | Hasta 7 días | F-01 (min 22:54) | *"a nivel de provincias podemos manejar hasta un rango de siete días"* | `[Explícito AS-IS]` |
| Cobertura territorial | 24 departamentos | F-01 (min 19:23) | *"Nosotros manejamos los 24 departamentos del país"* | `[Explícito AS-IS]` |
| Modalidades de transporte | 3 modalidades | F-01 (min 19:23–19:30) | *"Tenemos envío terrestre, envío biomodal, envío aéreo también"* | `[Explícito AS-IS]` |
| Meta de reducción de desfase | Máximo 30 minutos ($\le 30$ min) | F-01 (min 23:29–23:41, min 1:07–1:16 p2) | *"con un rango común de fase menor a media hora [...] o tal vez con un desfase máximo permisible de media hora, de 30 minutos"* | `[Meta TO-BE - Pendiente de validar]` |
| Capacidad transaccional | Gran volumen de datos | F-01 (min 1:24 parte 2) | *"gran capacidad para poder manejar volúmenes de data de mayor capacidad"* | `[Cualitativo - SLA por validar]` |

---

## 7. Elementos Deliberadamente Excluidos tras la Auditoría

Para asegurar que los modelos AS-IS y los requerimientos no incorporen datos no sustentados, se deja constancia expresa de las exclusiones fundamentadas:

### A. Elementos No Sustentados en la Fuente Textual
- **"Manifiesto de carga":** Cero apariciones en `trascrito.text`. Se eliminó de todos los documentos AS-IS y requerimientos.
- **"Pesaje de comprobación en despacho":** La fuente no menciona pesaje físico en despacho; el peso es un cálculo volumétrico previo de picking. Se eliminó de las actividades de despacho.
- **"Hitos o checkpoints intermedios en ruta":** No descritos en la entrevista; el pedido permanece en estado macro *"En Ruta"*.
- **"Ausencia de receptor" y "Dirección errónea" como causales de entrega fallida:** La fuente declara textualmente únicamente: **retraso, pérdida y daño** (`min 20:03–20:15`).
- **"Prohibición de conexiones punto a punto":** No declarada por el entrevistado; el Bus corporativo se mantiene como restricción existente sin adjetivos arquitectónicos no comprobados.

### B. Elementos del TO-BE o Propuestas de Solución (No AS-IS)
- Dispositivos GPS satelitales propuestos.
- Aplicación móvil Y-Trace.
- Código de despacho de Y-Trace.
- Captura de evidencia fotográfica obligatoria.
- Algoritmos de cálculo automático de promesas en tiempo real (clasificados como por validar).

### C. Procesos y Áreas Ajenas al Alcance Delimitado
- Manufactura y envasado industrial (cajas mono SKU / Código UA).
- Almacenamiento masivo de productos terminados en pallets (SAP R3).
- Preparación y recolección unitaria en líneas de picking del CD (SPY).
- Facturación comercial y plataforma de ventas (Maya / SAP Commerce).
- Peritaje técnico de siniestros al interior del almacén (Calidad + Seguridad Patrimonial).
- Activación de reposición comercial y gestión de pólizas de seguros.
- Creación y gestión de tickets de reclamos comerciales en CRM (CELLFORCE).
