# Especificación de Requerimientos Funcionales (RF)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Alcance del Proceso:** Despacho $\longrightarrow$ Transporte/Ruta $\longrightarrow$ Llegada $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta  
>
> **Navegación:** [[01 - Matriz Consolidada de Requerimientos]] | [[03 - Especificación de Requerimientos No Funcionales]] | [[04 - Trazabilidad y Registro de Depuración]]  
> **Enlaces al AS-IS:** [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]

---

## 1. Convenciones y Clasificación del Respaldo Metodológico

Cada requerimiento funcional especifica el origen y nivel de certeza de sus elementos mediante las siguientes etiquetas:
- **`[Explícitamente sustentado]`**: Afirmaciones, estados o datos confirmados textualmente por la entrevista (`trascrito.text`).
- **`[Derivado de una necesidad expresada]`**: Capacidades de software o reglas de interfaz deducidas lógicamente para dar soporte a un dato o resolver un problema del negocio.
- **`[Pendiente de validar]`**: Mecanismos de automatización o parámetros sujetos a definición técnica posterior.

---

## 2. Catálogo Detallado de Requerimientos Funcionales

---

### RF-01: Clasificación de pedidos por destino geográfico para despacho (Zonificación)

| Atributo | Detalle |
|---|---|
| **ID** | **RF-01** |
| **Nombre** | Clasificación de pedidos por destino geográfico para despacho |
| **Actor Responsable** | Supervisor de Zona de Despacho |
| **Etapa del Proceso** | Despacho |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Derivado de una necesidad expresada]` |
| **Problema / Necesidad** | Soporte a la canalización y agrupación territorial de despachos |
| **Fuente Primaria** | `trascrito.text` (min 17:41–17:53, min 19:04–19:15). Documento [[01 - Proceso actual]] §Etapa 1. |

#### Descripción Funcional
El sistema debe proveer la capacidad de clasificar y canalizar los pedidos según su destino geográfico (departamento, provincia, distrito y tipificación de ciudad: principal o alejada), utilizando los datos de entrega asociados al número de pedido, como soporte sistémico a la actividad operativa de zonificación que realiza el personal de despacho.

> [!NOTE]
> **Aclaración metodológica:** La entrevista confirma que en despacho los pedidos son zonificados por regiones cubriendo 24 departamentos (`min 19:04–19:23`) y que la dirección ya viene amarrada al pedido (`min 17:41`). No se afirma que el sistema actual realice automáticamente toda la zonificación; este requerimiento se formula como una **capacidad sistémica propuesta/derivada** para asistir y registrar dicha clasificación.

#### Entradas
- Número de pedido.
- Datos de entrega asociados: departamento (1 a 24), provincia, distrito, dirección domiciliaria y tipificación de ciudad (principal / alejada).

#### Comportamiento del Sistema
1. El sistema recupera los datos de entrega asociados al pedido.
2. Identifica la zona geográfica correspondiente dentro de los 24 departamentos.
3. Permite al usuario visualizar y agrupar los pedidos clasificados por lote de destino para coordinar su despacho.

#### Salidas
- Pedidos clasificados y agrupados por zona geográfica de entrega.

#### Criterio de Validación
Verificar que el sistema permita visualizar y agrupar los pedidos según departamento, provincia y tipo de ciudad para organizar su canalización.

---

### RF-02: Gestión de datos de socio logístico y modalidad de transporte

| Atributo | Detalle |
|---|---|
| **ID** | **RF-02** |
| **Nombre** | Gestión de datos de socio logístico y modalidad de transporte |
| **Actor Responsable** | Supervisor de Zona de Despacho |
| **Etapa del Proceso** | Despacho |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Derivado de una necesidad expresada]` |
| **Problema / Necesidad** | Mantener asociados los datos del transportista y modo de envío para trazabilidad |
| **Fuente Primaria** | `trascrito.text` (min 1:24, min 18:02, min 19:15–19:30). Documento [[01 - Proceso actual]] §Etapa 1. |

#### Descripción Funcional
El sistema debe permitir registrar y mantener asociados al pedido los datos del proveedor logístico asignado y la modalidad de transporte seleccionada (terrestre, bimodal o aérea), garantizando que esta información quede vinculada al envío previo a su traslado.

> [!NOTE]
> **Aclaración metodológica:** La entrevista confirma explícitamente que el pedido viaja con la información del socio logístico amarrada (`min 18:02`) y que Yanbal opera bajo tres modalidades de transporte: terrestre, bimodal y aérea (`min 19:23–19:30`). La fuente no afirma textualmente que el software ejecute la "asignación autónoma"; se formula como una **capacidad propuesta** para registrar y vincular dichos datos al pedido.

#### Entradas
- Número de pedido.
- Identificador del socio logístico responsable.
- Modalidad de envío aplicable: Terrestre, Bimodal o Aérea.

#### Comportamiento del Sistema
1. El sistema permite registrar o verificar el socio logístico y la modalidad de transporte que corresponden al pedido.
2. Almacena la relación entre el número de pedido, el transportista y el modo de transporte.

#### Salidas
- Pedido con datos de socio logístico y modalidad de transporte vinculados.

#### Criterio de Validación
Comprobar que al consultar el detalle de un pedido en despacho se visualice qué socio logístico lo trasladará y bajo qué modalidad (terrestre, bimodal o aérea).

---

### RF-03: Registro de salida de despacho del Centro de Distribución

| Atributo | Detalle |
|---|---|
| **ID** | **RF-03** |
| **Nombre** | Registro de salida de despacho y cambio de estado a "Despachado" |
| **Actor Responsable** | Supervisor de Zona de Despacho |
| **Etapa del Proceso** | Despacho |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado (salida y estado)]` / `[Derivado (datos temporales)]` |
| **Problema / Necesidad** | Registro de la transferencia formal de la carga al transportista |
| **Fuente Primaria** | `trascrito.text` (min 19:04, min 19:44). Documento [[03 - Actividades y eventos]] ACT-06. |

#### Descripción Funcional
El sistema debe permitir registrar formalmente la salida del pedido desde la zona de despacho del Centro de Distribución hacia la custodia del socio logístico, actualizando el estado del pedido a *"Despachado"* (o *"Pedido despachado"*).

> [!IMPORTANT]
> **Aclaraciones de auditoría:**
> - Se **eliminó definitivamente cualquier referencia a "manifiesto de carga"** por carecer de sustento textual en la entrevista.
> - La denominación del estado se ajusta estrictamente a la terminología de la fuente: *"pedido despachado"* o *"despachado"* (se descartó el término compuesto no textual *"Zonificado / Despachado"*).

#### Entradas
- Número de pedido.
- Confirmación de salida de despacho.

#### Comportamiento del Sistema
1. El usuario registra la salida física del pedido de las instalaciones de despacho.
2. El sistema actualiza el estado del pedido a "Despachado".
3. Se registran los datos de auditoría de la transacción.

#### Salidas
- Pedido en estado "Despachado".

#### Criterio de Validación
Verificar que el estado del pedido cambie a "Despachado" en el sistema tras confirmarse la salida de la zona de despacho.

---

### RF-04: Gestión y visualización de la promesa de entrega (Lead Time)

| Atributo | Detalle |
|---|---|
| **ID** | **RF-04** |
| **Nombre** | Gestión y visualización de la promesa de entrega según destino |
| **Actor Responsable** | Sistema / Supervisor de Despacho |
| **Etapa del Proceso** | Despacho |
| **Prioridad** | **Media** |
| **Sustento Metodológico** | `[Explícitamente sustentado (datos de promesa: 24h / 7d)]`<br/>`[Derivado (asociación y visualización sistémica)]`<br/>`[Pendiente de validar (cálculo automático)]` |
| **Problema / Necesidad** | Visibilidad del compromiso de entrega para clientes y soporte |
| **Fuente Primaria** | `trascrito.text` (min 22:38–22:54). Documento [[01 - Proceso actual]] §Etapa 2. |

#### Descripción Funcional
El sistema debe permitir asociar y visualizar el plazo y promesa estimada de entrega del pedido en función de su región de destino, tomando como referencia los estándares operativos de Yanbal: promesa de 24 horas para Lima Metropolitana y rango de hasta 7 días para provincias.

> [!NOTE]
> **Aclaración metodológica:** La entrevista confirma expresamente los valores de la promesa (*"manejamos 24 horas para la zona de Lima... y a nivel de provincias podemos manejar hasta un rango de siete días"*, `min 22:47–22:54`). La fuente no afirma que el software realice un cálculo algorítmico complejo en tiempo real. Por tanto, el requerimiento cubre la **asociación y visualización** de la promesa; la implementación de un cálculo dinámico automatizado queda catalogada como un **aspecto pendiente de validar**.

#### Entradas
- Destino geográfico del pedido (Lima Metropolitana o Provincia).
- Fecha y hora de referencia de despacho.

#### Comportamiento del Sistema
1. El sistema asocia al pedido la promesa de entrega correspondiente según el destino:
   - Lima Metropolitana: promesa de 24 horas.
   - Provincias: plazo estimado dentro del rango de hasta 7 días.
2. Mantiene disponible este dato para consulta del cliente y de Servicio al Cliente.

#### Salidas
- Plazo y promesa de entrega asociados al pedido.

#### Criterio de Validación
Verificar que al consultar los datos del pedido despachado se visualice la promesa de entrega correspondiente a su zona de destino (24h para Lima / hasta 7 días para provincias).

---

### RF-05: Registro de inicio de traslado (Estado 'En Ruta')

| Atributo | Detalle |
|---|---|
| **ID** | **RF-05** |
| **Nombre** | Registro de inicio de traslado y cambio a estado "En Ruta" |
| **Actor Responsable** | Socio Logístico / Transportista |
| **Etapa del Proceso** | Transporte |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado]` |
| **Problema / Necesidad** | Reflejo del inicio efectivo del transporte físico hacia destino |
| **Fuente Primaria** | `trascrito.text` (min 19:44–19:53). Documento [[03 - Actividades y eventos]] ACT-07. |

#### Descripción Funcional
El sistema debe permitir registrar la transición del pedido al estado *"En Ruta"* (o *"Pedido en ruta"*), indicando que el socio logístico ha iniciado el traslado físico hacia la localidad de destino.

> [!IMPORTANT]
> **Aclaración de auditoría:** No se introducen funciones de GPS satelital, monitoreo continuo de coordenadas geográficas ni registro de hitos/peajes intermedios por no formar parte del testimonio del entrevistado.

#### Entradas
- Número de pedido.
- Confirmación de inicio de traslado por el transportista.

#### Comportamiento del Sistema
1. El transportista confirma la recepción de la carga e inicio de trayecto.
2. El sistema actualiza el estado del pedido a "En Ruta".

#### Salidas
- Estado del pedido actualizado a "En Ruta".

#### Criterio de Validación
Comprobar que el sistema permita registrar el paso del pedido a "En Ruta" y que este cambio quede registrado en el ciclo de vida del envío.

---

### RF-06: Registro de entrega exitosa (Estado 'Entregado')

| Atributo | Detalle |
|---|---|
| **ID** | **RF-06** |
| **Nombre** | Registro de entrega física exitosa y cambio a estado "Entregado" |
| **Actor Responsable** | Socio Logístico / Transportista |
| **Etapa del Proceso** | Entrega |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado (estado)]` / `[Derivado (datos temporales del registro)]` |
| **Problema / Necesidad** | Cierre formal del proceso principal de distribución |
| **Fuente Primaria** | `trascrito.text` (min 19:53). Documento [[03 - Actividades y eventos]] ACT-11. |

#### Descripción Funcional
El sistema debe permitir registrar que el pedido ha sido completado y entregado físicamente en el domicilio de destino, actualizando su estado a *"Entregado"* (o *"Pedido entregado"*), asociando los datos temporales del momento del registro.

> [!NOTE]
> **Aclaración metodológica:** La existencia del estado *"pedido entregado cuando ya es finalizado y entregado hacia el cliente final"* está explícitamente confirmada en la fuente (`min 19:53`). La captura de fecha y hora exactas se reconoce formalmente como un **requerimiento derivado** de buenas prácticas de registro en software, no como frase literal del entrevistado.

#### Entradas
- Número de pedido.
- Confirmación de entrega física efectuada.

#### Comportamiento del Sistema
1. El transportista reporta la finalización de la entrega en destino.
2. El sistema actualiza el estado del pedido a "Entregado".
3. Se registran los datos temporales de la transacción de entrega.

#### Salidas
- Pedido en estado "Entregado".

#### Criterio de Validación
Verificar que al confirmarse la entrega, el pedido cambie a estado "Entregado" en el registro del sistema.

---

### RF-07: Registro de la identidad del receptor real

| Atributo | Detalle |
|---|---|
| **ID** | **RF-07** |
| **Nombre** | Registro de la identidad del receptor real (titular o persona autorizada) |
| **Actor Responsable** | Socio Logístico / Transportista |
| **Etapa del Proceso** | Entrega |
| **Prioridad** | **Alta (Crítico)** |
| **Sustento Metodológico** | `[Derivado de una necesidad expresada]` |
| **Problema / Necesidad** | **PR-05:** Desconocimiento de recepción cuando entrega una persona autorizada y reclamos prematuros |
| **Fuente Primaria** | `trascrito.text` (min 24:45–25:01). Documento [[05 - Problemas y desfases]] §PR-05. |

#### Descripción Funcional
El sistema debe permitir capturar y registrar la identidad de la persona que recibe físicamente el paquete en el domicilio de destino, permitiendo consignar si la entrega fue recibida directamente por la consultora titular o por una persona autorizada.

> [!IMPORTANT]
> **Aclaraciones de auditoría:**
> - La fuente **confirma expresamente la necesidad de negocio**: *"No necesariamente el cliente final es el que recibe la entrega del pedido, sino puede ser alguna persona autorizada y el cliente final, al hacer la trazabilidad... no necesariamente tiene esa información. Entonces es ahí donde está nuestro reto"* (`min 24:45–25:01`).
> - Se **retiran o clasifican como derivados** los detalles técnicos no sustentados textualmente:
>   - Exigencia obligatoria de DNI/CE (*derivado*).
>   - Catálogo cerrado de parentescos (*derivado*).
>   - Bloqueo duro del software que impida cerrar la entrega sin dichos datos (*regla de diseño propuesta, no hecho confirmado*).

#### Entradas
- Condición de recepción: `Consultora Titular` o `Persona Autorizada / Tercero`.
- Nombre de la persona que recibe el paquete.
- Datos complementarios de filiación o vínculo (*campos propuestos derivados*).

#### Comportamiento del Sistema
1. Al momento de reportar la entrega, el sistema habilita campos para ingresar quién recibe el paquete.
2. Si la entrega se efectúa a una persona autorizada, el sistema registra el nombre y condición declarada.
3. Asocia estos datos al registro de la entrega del pedido.

#### Salidas
- Datos del receptor real registrados y vinculados al pedido entregado.

#### Criterio de Validación
Comprobar que el sistema permita registrar el nombre de una persona autorizada como receptor del pedido en lugar de la consultora titular.

---

### RF-08: Registro de entrega fallida e incidencias en ruta o destino

| Atributo | Detalle |
|---|---|
| **ID** | **RF-08** |
| **Nombre** | Registro de entrega fallida e incidencias tipificadas |
| **Actor Responsable** | Socio Logístico / Transportista |
| **Etapa del Proceso** | Entrega (Flujo Alternativo) |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado]` |
| **Problema / Necesidad** | Registro de contingencias que impiden la entrega |
| **Fuente Primaria** | `trascrito.text` (min 20:03–20:30). Documento [[03 - Actividades y eventos]] ACT-13. |

#### Descripción Funcional
El sistema debe permitir registrar cuando una entrega no puede completarse con éxito, actualizando el estado del pedido a *"Entrega Fallida"* y tipificando la incidencia ocurrida de acuerdo con las causales expresamente indicadas en la fuente: **retraso, pérdida o daño**.

> [!IMPORTANT]
> **Aclaración de auditoría:** Se eliminaron las causales no mencionadas por el entrevistado (*"ausencia de receptor"* y *"dirección errónea o inaccesible"*). El catálogo de incidencias del requerimiento se restringe estrictamente a los motivos declarados por Joao Condorpusa: **retraso, pérdida y daño** (`min 20:03–20:15`).

#### Entradas
- Número de pedido.
- Causal de la incidencia: `Retraso`, `Pérdida` o `Daño`.
- Observaciones del transportista sobre la contingencia.

#### Comportamiento del Sistema
1. El transportista reporta la incidencia que impide la entrega.
2. Selecciona la causal entre las opciones sustentadas (retraso, pérdida o daño).
3. El sistema actualiza el estado del pedido a "Entrega Fallida".

#### Salidas
- Pedido en estado "Entrega Fallida" con causal documentada.

#### Criterio de Validación
Verificar que el sistema permita registrar una entrega fallida seleccionando causales de retraso, pérdida o daño.

---

### RF-09: Registro de retorno de carga por logística inversa

| Atributo | Detalle |
|---|---|
| **ID** | **RF-09** |
| **Nombre** | Registro de retorno de paquete hacia almacén por logística inversa |
| **Actor Responsable** | Socio Logístico / Transportista |
| **Etapa del Proceso** | Entrega (Flujo Alternativo) |
| **Prioridad** | **Media** |
| **Sustento Metodológico** | `[Explícitamente sustentado (retorno del transportista)]` |
| **Problema / Necesidad** | Registro del flujo físico de regreso de paquetes no entregados |
| **Fuente Primaria** | `trascrito.text` (min 20:25–20:55). Documento [[01 - Proceso actual]] §Camino Alternativo. |

#### Descripción Funcional
El sistema debe permitir registrar que el socio logístico inicia el proceso de retorno físico del pedido no entregado hacia el centro de distribución como parte de la logística inversa.

> [!IMPORTANT]
> **Aclaración de auditoría de alcance:**
> Se **eliminó cualquier referencia** a:
> - Activación de reposición comercial de productos.
> - Evaluación interna en almacén.
> - Peritaje de Control de Calidad y Seguridad Patrimonial.
> - Activación de seguros o garantías.
> 
> Esos procesos ocurren al interior del almacén central y están **fuera del alcance delimitado**. El requerimiento en este sistema termina estrictamente en el **registro del retorno de la carga por parte del transportista**.

#### Entradas
- Número de pedido en estado "Entrega Fallida".
- Registro de inicio de retorno por el socio logístico.

#### Comportamiento del Sistema
1. El socio logístico registra que el pedido fallido inicia su retorno físico hacia el Centro de Distribución.
2. El sistema deja constancia de que la carga se encuentra en trayecto de retorno (logística inversa).

#### Salidas
- Registro del pedido en condición de retorno logístico.

#### Criterio de Validación
Comprobar que un pedido con entrega fallida pueda registrarse en retorno hacia el centro de distribución por parte del transportista.

---

### RF-10: Sincronización y disponibilidad de estados de distribución

| Atributo | Detalle |
|---|---|
| **ID** | **RF-10** |
| **Nombre** | Sincronización y disponibilidad de estados de distribución para consulta |
| **Actor Responsable** | Sistema (Sincronización) |
| **Etapa del Proceso** | Actualización de Información |
| **Prioridad** | **Alta (Crítico)** |
| **Sustento Metodológico** | `[Derivado de una necesidad expresada]` |
| **Problema / Necesidad** | **PR-03:** Desfase de hasta 2 horas en la visibilidad digital de los estados |
| **Fuente Primaria** | `trascrito.text` (min 23:19–24:18, min 0:49–1:16 parte 2). Documento [[05 - Problemas y desfases]] §PR-03. |

#### Descripción Funcional
El sistema debe sincronizar y hacer disponibles las actualizaciones de estado (Despachado, En Ruta, Entregado, Entrega Fallida) registradas en campo, de modo que la información quede reflejada en las plataformas corporativas utilizadas para la consulta de clientes y atención de Servicio al Cliente.

> [!NOTE]
> **Aclaración metodológica:** Se **retiró la mención al Bus de Integración de la redacción de este requerimiento funcional**. La función del software consiste en sincronizar y poner a disposición los datos de estado; el mecanismo técnico de comunicación mediante el Bus corporativo se especifica como restricción de interoperabilidad en [[03 - Especificación de Requerimientos No Funcionales#RNF-04|RNF-04]].

#### Entradas
- Actualización de estado registrada por el transportista o supervisor.

#### Comportamiento del Sistema
1. El sistema procesa la actualización de estado capturada en la operación.
2. Actualiza la información en los repositorios accesibles para consulta.
3. Deja los datos actualizados a disposición de los canales de seguimiento.

#### Salidas
- Estados de pedidos disponibles para consulta corporativa.

#### Criterio de Validación
Comprobar que un cambio de estado registrado en campo se refleje de manera efectiva en las herramientas de consulta del pedido.

---

### RF-11: Consulta de trazabilidad con soporte multicriterio

| Atributo | Detalle |
|---|---|
| **ID** | **RF-11** |
| **Nombre** | Consulta de trazabilidad por Número de Pedido o Código de Consultora |
| **Actor Responsable** | Consultora / Consultor / Agente de Servicio al Cliente |
| **Etapa del Proceso** | Consulta y Seguimiento |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado]` |
| **Problema / Necesidad** | Seguimiento y visibilidad de pedidos para clientes y agentes |
| **Fuente Primaria** | `trascrito.text` (min 22:03–22:28). Documento [[01 - Proceso actual]] §Etapa 5. |

#### Descripción Funcional
El sistema debe permitir a las consultoras y a los agentes de Servicio al Cliente consultar el estado vigente de un pedido y su promesa estimada de entrega, admitiendo la búsqueda a través de dos identificadores expresamente sustentados: el *Número de Pedido* o el *Código de Consultora/Cliente*.

> [!NOTE]
> **Aclaración metodológica:** La fuente confirma literalmente que con ambas informaciones se puede hacer seguimiento: *"la información necesaria que el usuario o el cliente debe conocer es el número del pedido. También puede hacer trazabilidad o seguimiento con el código de cliente o el código de consultor... Con esas dos informaciones puede hacer el seguimiento de cada estatus del pedido"* (`min 22:03–22:28`). Se consolidó en un único RF para evitar duplicidad funcional.

#### Entradas
- Criterio de búsqueda ingresado: *Número de Pedido* o *Código de Consultora/Cliente*.

#### Comportamiento del Sistema
1. El usuario ingresa el parámetro de búsqueda elegido.
2. Si busca por Número de Pedido: el sistema despliega el estado actual del pedido y su plazo/promesa estimada de entrega.
3. Si busca por Código de Consultora: el sistema retorna la relación de pedidos asociados a esa consultora con su correspondiente situación.

#### Salidas
- Información de estado y seguimiento del pedido consultado.

#### Criterio de Validación
Realizar consultas de prueba ingresando un número de pedido y un código de consultora; verificar que en ambos casos el sistema devuelva el estado de distribución correspondiente.

---

### RF-12: Visualización de datos del receptor en la consulta de trazabilidad

| Atributo | Detalle |
|---|---|
| **ID** | **RF-12** |
| **Nombre** | Visualización de datos del receptor en los canales de consulta |
| **Actor Responsable** | Consultora / Consultor / Agente de Servicio al Cliente |
| **Etapa del Proceso** | Consulta y Seguimiento |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Derivado de una necesidad expresada]` |
| **Problema / Necesidad** | **PR-04 / PR-05:** Incertidumbre de recepción y reclamos innecesarios en Servicio al Cliente |
| **Fuente Primaria** | `trascrito.text` (min 24:29–25:01). Documento [[05 - Problemas y desfases]] §PR-04, §PR-05. |

#### Descripción Funcional
El sistema debe exhibir en los canales de consulta de pedidos entregados la información del receptor real capturada en campo (distinguiendo si fue recibido por la consultora titular o por una persona autorizada), permitiendo a la consultora y al agente de atención verificar quién recibió el paquete para mitigar reclamos prematuros por supuesta no-entrega.

> [!NOTE]
> **Aclaración metodológica:** Responde directamente a la brecha documentada en la entrevista: cuando una persona autorizada recibe el paquete, la consultora no dispone de esa información al consultar el sistema y reporta que no recibió su pedido (`min 24:45–25:01`). No se añaden campos obligatorios no sustentados (como "hora exacta" o documentos específicos) salvo que se consideren propuestas derivadas de diseño.

#### Entradas
- Consulta de un pedido que se encuentra en estado "Entregado".

#### Comportamiento del Sistema
1. Al desplegar los datos de un pedido entregado, el sistema muestra la información registrada sobre la persona que recibió físicamente el paquete.
2. Identifica si la recepción fue titular o por persona autorizada.

#### Salidas
- Detalle de la persona receptora visible en la interfaz de consulta.

#### Criterio de Validación
Consultar un pedido entregado a una persona autorizada; comprobar que la pantalla de consulta muestre quién recibió el paquete, permitiendo confirmar la entrega.
