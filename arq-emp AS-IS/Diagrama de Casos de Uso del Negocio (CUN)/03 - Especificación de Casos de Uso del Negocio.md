# Especificación de Casos de Uso del Negocio (CUN)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Alcance del Proceso:** Despacho $\longrightarrow$ Transporte/Ruta $\longrightarrow$ Llegada $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta  
> **Estándar Académico:** Plantilla Oficial de Casos de Uso UTP (Sesión 8) adaptada a Casos de Uso del Negocio (RUP / UML)  
>
> **Navegación del Módulo CUN:**  
> [[01 - Diagrama General de Casos de Uso del Negocio (CUN)]] | [[02 - Tabla de Roles y Actividades del Negocio]] | [[03 - Especificación de Casos de Uso del Negocio]] | [[04 - Trazabilidad de Casos de Uso del Negocio]]  
> **Enlaces a la Base AS-IS:**  
> [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]  
> **Enlaces a Requerimientos:**  
> [[01 - Matriz Consolidada de Requerimientos]] | [[02 - Especificación de Requerimientos Funcionales]] | [[03 - Especificación de Requerimientos No Funcionales]] | [[04 - Trazabilidad y Registro de Depuración]]

---

## 1. Estructura Estándar de las Fichas de Especificación

Cada Caso de Uso del Negocio se documenta mediante la plantilla oficial establecida en la Guía de Casos de Uso (UTP), adaptada semánticamente a nivel del negocio para los **5 CUNs definitivos**:

```mermaid
flowchart LR
    CUN01["CUN-01<br/>Despachar Pedidos desde CD"] --> CUN02["CUN-02<br/>Trasladar Pedidos hacia Destino"]
    CUN02 -->|Camino Principal| CUN03["CUN-03<br/>Entregar Pedido en Domicilio"]
    CUN02 -->|Camino Alternativo| CUN04["CUN-04<br/>Gestionar Entrega Fallida y Retorno"]
    CUN03 & CUN04 -.->|Sincronización Transversal (ACT-12 / RF-10)| CUN05["CUN-05<br/>Consultar Trazabilidad y Situación"]
```

- **Código y Nombre:** Código identificador en infinitivo (`CUN-01` a `CUN-05`).
- **Actores Participantes:** Clasificación de Actores del Negocio («Business Actor») y Trabajadores del Negocio («Business Worker»).
- **Descripción:** Resumen del valor operativo entregado por el caso de uso.
- **Precondiciones:** Estado previo necesario para iniciar el flujo.
- **Flujo Normal (Básico):** Secuencia numerada de pasos operativos.
- **Flujos Alternativos:** Caminos de contingencia y excepciones operacionales.
- **Postcondiciones:** Estado final del pedido y de la información.
- **Reglas del Negocio Aplicables:** Políticas y restricciones operativas sustentadas en el AS-IS.
- **Trazabilidad:** Mapeo con requerimientos funcionales (`RF`), no funcionales (`RNF`) y actividades (`ACT`).

---

## 2. Fichas Técnicas de Especificación de los 5 CUN

---

### CUN-01: Despachar Pedidos desde Centro de Distribución

| Campo | Especificación del Estándar UTP / RUP |
|:---|:---|
| **Código y Nombre:** | **CUN-01 — Despachar Pedidos desde Centro de Distribución** |
| **Actores Participantes:** | - **Supervisor de Zona de Despacho** *(«Business Worker» — Responsable interno en CD)*<br/>- **Socio Logístico / Transportista** *(«Business Actor» — Receptor de carga)* |
| **Descripción:** | Proceso operativo mediante el cual los pedidos empacados provenientes de la línea de picking se reciben en la Zona de Despacho, se clasifican geográficamente según su destino en los 24 departamentos, se vinculan al transportista asociado y modalidad de transporte, se asocia su promesa de entrega y se transfieren formalmente en custodia física al transportista, actualizando el estado a *"Despachado"*. |
| **Precondiciones:** | 1. El pedido físico ha concluido su empaque en el CD.<br/>2. El pedido cuenta con su Número de Pedido y datos de entrega consolidados desde la plataforma comercial.<br/>3. El pedido se encuentra en estado inicial en los sistemas del CD. |
| **Flujo Normal (Básico):** | 1. El Supervisor de Despacho recibe físicamente las cajas embaladas en la Zona de Despacho (`ACT-01`).<br/>2. El Supervisor consulta en el sistema Driving la dirección de destino del pedido y clasifica el pedido según su canalización geográfica en los 24 departamentos (`ACT-02`).<br/>3. El Supervisor asigna al pedido el socio logístico contratado para esa ruta y registra la modalidad de envío aplicable: terrestre, bimodal o aérea (`ACT-03`).<br/>4. El sistema vincula la promesa estimada de entrega según la región geográfica: 24 horas para Lima Metropolitana y hasta 7 días para provincias (`ACT-04`).<br/>5. El Supervisor entrega formalmente las cajas al transportista asignado y registra el egreso en los sistemas Driving/NSDG, cambiando el estado a *"Despachado"* (`ACT-05`).<br/>6. El egreso formal queda registrado y disponible para la actividad técnica transversal de actualización (`ACT-12` / `RF-10`). |
| **Flujos Alternativos:** | No se presentan flujos alternativos en el alcance operativo sustentado del AS-IS. |
| **Postcondiciones:** | - La custodia física del paquete queda formalmente bajo responsabilidad del transportista.<br/>- El estado del pedido queda actualizado a **"Despachado"** en NSDG/Driving.<br/>- Quedan registrados: socio logístico, modalidad de envío, estampa de tiempo y promesa de entrega. |
| **Reglas del Negocio:** | - **RN-01 (Cobertura Territorial):** La clasificación de pedidos debe cubrir los 24 departamentos del Perú.<br/>- **RN-02 (Modalidades de Despacho):** Se asigna el envío según tres modalidades sustentadas: terrestre, bimodal o aérea.<br/>- **RN-03 (Asociación de Promesa de Entrega):** Se vincula el tiempo estimado estándar de entrega: 24 horas para Lima Metropolitana y hasta 7 días para provincias. |
| **Trazabilidad:** | - **Requerimientos Funcionales:** [[02 - Especificación de Requerimientos Funcionales#RF-01\|RF-01]], [[02 - Especificación de Requerimientos Funcionales#RF-02\|RF-02]], [[02 - Especificación de Requerimientos Funcionales#RF-03\|RF-03]], [[02 - Especificación de Requerimientos Funcionales#RF-04\|RF-04]].<br/>- **Actividades AS-IS:** [[03 - Actividades y eventos#ACT-01\|ACT-01]], [[03 - Actividades y eventos#ACT-02\|ACT-02]], [[03 - Actividades y eventos#ACT-03\|ACT-03]], [[03 - Actividades y eventos#ACT-04\|ACT-04]], [[03 - Actividades y eventos#ACT-05\|ACT-05]].<br/>- **Requerimientos No Funcionales:** [[03 - Especificación de Requerimientos No Funcionales#RNF-02\|RNF-02]], [[03 - Especificación de Requerimientos No Funcionales#RNF-05\|RNF-05]], [[03 - Especificación de Requerimientos No Funcionales#RNF-07\|RNF-07]]. |

---

### CUN-02: Trasladar Pedidos hacia Destino Nacional

| Campo | Especificación del Estándar UTP / RUP |
|:---|:---|
| **Código y Nombre:** | **CUN-02 — Trasladar Pedidos hacia Destino Nacional** |
| **Actores Participantes:** | - **Socio Logístico / Transportista** *(«Business Actor» — Ejecutor del transporte)* |
| **Descripción:** | Proceso mediante el cual el transportista carga los pedidos despachados en sus unidades de transporte, registra el inicio del recorrido cambiando el estado a *"En Ruta"* y ejecuta el traslado físico a través de la red vial nacional, vías fluviales o rutas aéreas hasta la localidad o domicilio de destino. |
| **Precondiciones:** | 1. El pedido se encuentra en estado *"Despachado"* en el Centro de Distribución.<br/>2. La carga ha sido transferida al vehículo del socio logístico asignado a la ruta. |
| **Flujo Normal (Básico):** | 1. El Socio Logístico confirma la recepción de la carga y registra en el sistema de transporte (NSDG/Driving) el inicio de la ruta (`ACT-06`).<br/>2. El estado del pedido se actualiza formalmente a *"En Ruta"*.<br/>3. El evento queda registrado y disponible para la actividad transversal de actualización (`ACT-12` / `RF-10`).<br/>4. El transportista traslada físicamente la carga por el trayecto asignado según su modalidad (`ACT-07`): terrestre, bimodal o aérea.<br/>5. La unidad arriba a la localidad de destino dentro del plazo estimado (24h Lima / 7d provincias). |
| **Flujos Alternativos:** | No se registran flujos alternativos durante el tránsito. Si durante el traslado se suscita una contingencia tipificada (retraso, pérdida o daño) que impida concretar la entrega, el proceso se bifurca hacia el camino alternativo CUN-04. |
| **Postcondiciones:** | - El pedido se mantiene en tránsito con estado formal **"En Ruta"**.<br/>- La carga arriba físicamente a la localidad de entrega para dar inicio a la última milla (`CUN-03` o `CUN-04`). |
| **Reglas del Negocio:** | - **RN-04 (Permanencia en Estado 'En Ruta'):** El pedido permanece en estado macro "En Ruta" durante todo el recorrido hacia el destino nacional, sin garitas ni checkpoints intermedios sustentados en el AS-IS.<br/>- **RN-05 (Modalidades de Traslado):** El traslado físico se efectúa por las redes terrestres, fluviales/bimodales o aéreas según la cobertura territorial. |
| **Trazabilidad:** | - **Requerimientos Funcionales:** [[02 - Especificación de Requerimientos Funcionales#RF-05\|RF-05]].<br/>- **Actividades AS-IS:** [[03 - Actividades y eventos#ACT-06\|ACT-06]], [[03 - Actividades y eventos#ACT-07\|ACT-07]].<br/>- **Requerimientos No Funcionales:** [[03 - Especificación de Requerimientos No Funcionales#RNF-02\|RNF-02]], [[03 - Especificación de Requerimientos No Funcionales#RNF-03\|RNF-03]], [[03 - Especificación de Requerimientos No Funcionales#RNF-07\|RNF-07]]. |

---

### CUN-03: Entregar Pedido en Domicilio

| Campo | Especificación del Estándar UTP / RUP |
|:---|:---|
| **Código y Nombre:** | **CUN-03 — Entregar Pedido en Domicilio** |
| **Actores Participantes:** | - **Socio Logístico / Transportista** *(«Business Actor» — Ejecutor de la entrega en campo)*<br/>- **Consultora / Consultor de Yanbal** *(«Business Actor» — Receptor titular)*<br/>- **Familiar Autorizado** *(«Business Actor» — Receptor alterno en domicilio)* |
| **Descripción:** | Camino operativo principal en destino mediante el cual el transportista se presenta en el domicilio, efectúa la entrega física del paquete a la consultora titular o a un familiar autorizado presente, captura los datos del receptor real y registra la confirmación de la entrega, actualizando el estado del pedido a *"Entregado"*. |
| **Precondiciones:** | 1. El pedido se encuentra en estado *"En Ruta"* (`CUN-02`) y la unidad de transporte ha llegado a la dirección registrada.<br/>2. Existe una persona receptora calificada presente en el inmueble (consultora titular o familiar autorizado). |
| **Flujo Normal (Básico):** | 1. El transportista arriba a la dirección de entrega e interactúa con la persona presente en el domicilio.<br/>2. La **consultora titular** se identifica como receptora del pedido.<br/>3. El transportista entrega físicamente la caja embalada en manos de la consultora.<br/>4. El transportista registra en la herramienta de campo de NSDG la confirmación de la entrega (`ACT-08`), capturando que la receptora fue la titular (`ACT-09`).<br/>5. El estado del pedido se actualiza a **"Entregado"**.<br/>6. El evento de entrega queda registrado en campo y disponible para la actividad transversal de actualización (`ACT-12` / `RF-10`), quedando sujeto al desfase de sincronización de hasta 2 horas. |
| **Flujos Alternativos:** | **2.a. Recepción por Familiar Autorizado (Escenario Crítico PR-05):**<br/>- 2.a.1. La consultora titular no se encuentra en el domicilio.<br/>- 2.a.2. Un familiar presente se identifica ante el transportista como persona autorizada para recibir.<br/>- 2.a.3. El transportista entrega el paquete al familiar y registra la entrega completada en NSDG (`ACT-08`), consignando los datos del familiar como receptor real (`ACT-09`).<br/>- 2.a.4. El flujo continúa en el paso 5 con estado *"Entregado"*.<br/><br/>**2.b. Imposibilidad de entrega por contingencia (Bifurcación hacia CUN-04):**<br/>- 2.b.1. Si en el intento de entrega se suscita una contingencia tipificada exclusivamente por la matriz (**retraso, pérdida o daño**), el transportista no concluye la entrega y el proceso continúa por el camino alternativo `CUN-04`. |
| **Postcondiciones:** | - El pedido físico queda en posesión del receptor en el domicilio.<br/>- El estado del pedido cambia formalmente a **"Entregado"**.<br/>- Queda registrada la condición de quien recibió físicamente (titular o familiar autorizado). |
| **Reglas del Negocio:** | - **RN-06 (Recepción Válida en Domicilio):** Se autoriza la entrega física a la consultora titular o a un familiar autorizado presente en el domicilio.<br/>- **RN-07 (Captura de Condición de Receptor):** El transportista debe registrar si quien recibió el paquete fue la titular o un familiar autorizado. |
| **Trazabilidad:** | - **Requerimientos Funcionales:** [[02 - Especificación de Requerimientos Funcionales#RF-06\|RF-06]], [[02 - Especificación de Requerimientos Funcionales#RF-07\|RF-07]].<br/>- **Actividades AS-IS:** [[03 - Actividades y eventos#ACT-08\|ACT-08]], [[03 - Actividades y eventos#ACT-09\|ACT-09]].<br/>- **Requerimientos No Funcionales:** [[03 - Especificación de Requerimientos No Funcionales#RNF-06\|RNF-06]], [[03 - Especificación de Requerimientos No Funcionales#RNF-07\|RNF-07]].<br/>- **Problemas AS-IS:** [[05 - Problemas y desfases#PR-05\|PR-05]] (Receptor real no comunicado). |

---

### CUN-04: Gestionar Entrega Fallida y Retorno por Logística Inversa

| Campo | Especificación del Estándar UTP / RUP |
|:---|:---|
| **Código y Nombre:** | **CUN-04 — Gestionar Entrega Fallida y Retorno por Logística Inversa** |
| **Actores Participantes:** | - **Socio Logístico / Transportista** *(«Business Actor» — Responsable operativo en campo y retorno)* |
| **Descripción:** | Camino operativo alternativo mediante el cual, ante una incidencia confirmada durante el transporte o en destino (**retraso, pérdida o daño**), el transportista registra la entrega fallida, actualiza el estado a *"Entrega Fallida"* y registra el inicio del retorno de la carga no entregada hacia el Centro de Distribución a través del flujo de logística inversa. |
| **Precondiciones:** | 1. El pedido se encuentra en estado *"En Ruta"* (`CUN-02`).<br/>2. Se confirma formalmente una causal de contingencia sustentada en las fuentes: **retraso, pérdida o daño**. |
| **Flujo Normal (Básico):** | 1. El Socio Logístico constata la contingencia tipificada en ruta o en destino.<br/>2. El transportista registra la entrega fallida en el sistema de transporte (NSDG/Driving), tipificando la causa exclusivamente como **retraso, pérdida o daño** (`ACT-10`).<br/>3. El estado del pedido se actualiza formalmente a **"Entrega Fallida"**.<br/>4. El evento queda disponible para la actividad técnica transversal de actualización (`ACT-12` / `RF-10`).<br/>5. El transportista registra el inicio de retorno del pedido hacia el Centro de Distribución (`ACT-11`).<br/>6. Concluye el caso de uso del negocio al quedar formalizado el inicio del retorno de la carga no entregada. |
| **Flujos Alternativos:** | **2.a. Pérdida de mercancía en ruta:**<br/>- 2.a.1. En caso de pérdida de la carga durante el transporte, el transportista registra la entrega fallida con causal *pérdida*.<br/>- 2.a.2. Al no existir bulto físico que retornar, no se realiza el registro de inicio de retorno y concluye la gestión en campo. |
| **Postcondiciones:** | - El estado del pedido queda registrado formalmente como **"Entrega Fallida"**.<br/>- Queda formalmente registrado el inicio del retorno del pedido hacia el Centro de Distribución (`ACT-11` / `RF-09`). |
| **Reglas del Negocio:** | - **RN-08 (Causales Exclusivas Sustentadas):** Las únicas causales válidas para registrar entrega fallida son exclusivamente **retraso, pérdida o daño** (según entrevista min 20:03–20:15). Se descartan causales no sustentadas.<br/>- **RN-09 (Frontera del Registro de Retorno):** El alcance del caso de uso concluye con el registro del inicio del retorno de la carga (`ACT-11` / `RF-09`). La recepción física en almacén, inventario, peritajes técnicos, seguros o reposición comercial quedan fuera del alcance del proceso de distribución. |
| **Trazabilidad:** | - **Requerimientos Funcionales:** [[02 - Especificación de Requerimientos Funcionales#RF-08\|RF-08]], [[02 - Especificación de Requerimientos Funcionales#RF-09\|RF-09]].<br/>- **Actividades AS-IS:** [[03 - Actividades y eventos#ACT-10\|ACT-10]], [[03 - Actividades y eventos#ACT-11\|ACT-11]].<br/>- **Requerimientos No Funcionales:** [[03 - Especificación de Requerimientos No Funcionales#RNF-05\|RNF-05]], [[03 - Especificación de Requerimientos No Funcionales#RNF-07\|RNF-07]]. |

---

### CUN-05: Consultar Trazabilidad y Situación del Pedido

| Campo | Especificación del Estándar UTP / RUP |
|:---|:---|
| **Código y Nombre:** | **CUN-05 — Consultar Trazabilidad y Situación del Pedido** |
| **Actores Participantes:** | - **Consultora / Consultor de Yanbal** *(«Business Actor» — Consultante en canal de atención)*<br/>- **Agente de Servicio al Cliente** *(«Business Worker» — Consultante de atención)* |
| **Descripción:** | Proceso de negocio mediante el cual la consultora o el agente de servicio al cliente consultan la situación del pedido y la promesa de entrega mediante el Número de Pedido o el Código de Consultora. En el proceso actual (AS-IS), la consulta refleja la información disponible según las actualizaciones recibidas, evidenciando como problemática operativa el desfase de actualización y la falta de visibilidad oportuna del receptor cuando el pedido es recibido por un familiar autorizado. |
| **Precondiciones:** | 1. El pedido ha sido despachado del Centro de Distribución (`CUN-01`).<br/>2. El consultante dispone del identificador de búsqueda: Número de Pedido o Código de Consultora. |
| **Flujo Normal (Básico):** | 1. El actor (Consultora o Agente de Servicio al Cliente) realiza la consulta de la situación del pedido indicando el identificador de búsqueda: **Número de Pedido** o **Código de Consultora/Cliente** (`ACT-13`).<br/>2. El consultante obtiene la situación registrada del pedido: su estado actual de distribución (*Despachado, En Ruta, Entregado, Entrega Fallida*) y la promesa estimada de entrega (`ACT-13`).<br/>3. **Condición actual del AS-IS (Problemas PR-03 y PR-04):** La información obtenida refleja la última actualización transmitida desde campo, la cual presenta un desfase de hasta 2 horas respecto a los eventos reales debido a la latencia en la propagación de estados (`ACT-12` / `RF-10`).<br/>4. **Problemática de visibilidad del receptor real (Problema PR-05 y Necesidad RF-12):** Cuando el pedido fue entregado en domicilio a un familiar autorizado, los datos de quién recibió no se encuentran necesariamente visibles ni disponibles de forma oportuna para la consultora ni para el agente (`ACT-14`), lo que genera incertidumbre en la consultora y consultas o reclamos prematuros al área de atención, fundamentando la necesidad operativa formulada en `RF-12`. |
| **Flujos Alternativos:** | **3.a. Consulta durante la ventana de desfase de actualización (~2 horas) (Manifestación de PR-03 y PR-04):**<br/>- 3.a.1. El pedido fue físicamente entregado en campo, pero al consultarse figura aún como "En Ruta" debido al tiempo de propagación de hasta 2 horas.<br/>- 3.a.2. La consultora contacta a Servicio al Cliente, donde el agente visualiza la misma información desactualizada ("En Ruta"), viéndose imposibilitado de confirmar la entrega en vivo.<br/><br/>**4.a. Falta de visibilidad de entrega a familiar autorizado (Manifestación de PR-05):**<br/>- 4.a.1. El pedido fue recibido por un familiar en domicilio, pero dicha condición de recepción no se visualiza oportunamente en la consulta.<br/>- 4.a.2. La consultora presume que su mercancía no ha llegado e inicia un reclamo ante el agente de atención al cliente. |
| **Postcondiciones:** | - El consultante obtiene la información disponible sobre el estado actual registrado y la promesa estimada de entrega del pedido según la última actualización recibida. |
| **Reglas del Negocio:** | - **RN-10 (Criterios de Búsqueda Homologados):** La consulta admite indistintamente la búsqueda por Número de Pedido o Código de Consultora/Cliente (`RF-11`).<br/>- **RN-11 (Necesidad Operativa de Identificación del Receptor - RF-12):** En el AS-IS se constata la necesidad operativa de identificar y mostrar la información del receptor real (familiar autorizado) en la consulta para prevenir reclamos infundados de no-entrega (**PR-05**). |
| **Trazabilidad:** | - **Requerimientos Funcionales:** [[02 - Especificación de Requerimientos Funcionales#RF-11\|RF-11]], [[02 - Especificación de Requerimientos Funcionales#RF-12\|RF-12]].<br/>- **Actividades AS-IS:** [[03 - Actividades y eventos#ACT-13\|ACT-13]], [[03 - Actividades y eventos#ACT-14\|ACT-14]].<br/>- **Requerimientos No Funcionales:** [[03 - Especificación de Requerimientos No Funcionales#RNF-03\|RNF-03]], [[03 - Especificación de Requerimientos No Funcionales#RNF-05\|RNF-05]].<br/>- **Problemas AS-IS:** [[05 - Problemas y desfases#PR-03\|PR-03]], [[05 - Problemas y desfases#PR-04\|PR-04]], [[05 - Problemas y desfases#PR-05\|PR-05]]. |
