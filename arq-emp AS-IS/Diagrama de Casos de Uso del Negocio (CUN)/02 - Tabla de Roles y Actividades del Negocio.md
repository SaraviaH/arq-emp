# Tabla de Roles y Actividades del Negocio

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Alcance del Proceso:** Despacho $\longrightarrow$ Transporte/Ruta $\longrightarrow$ Llegada $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta  
> **Marco Metodológico:** RUP (*Rational Unified Process*) / UML 2.5 / UTP APF1  
>
> **Navegación del Módulo CUN:**  
> [[01 - Diagrama General de Casos de Uso del Negocio (CUN)]] | [[02 - Tabla de Roles y Actividades del Negocio]] | [[03 - Especificación de Casos de Uso del Negocio]] | [[04 - Trazabilidad de Casos de Uso del Negocio]]  
> **Enlaces a la Base AS-IS:**  
> [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]  
> **Enlaces a Requerimientos:**  
> [[01 - Matriz Consolidada de Requerimientos]] | [[02 - Especificación de Requerimientos Funcionales]] | [[04 - Trazabilidad y Registro de Depuración]]

---

## 1. Propósito y Estándar Académico

El presente documento constituye el **entregable obligatorio de la Guía Oficial de Propuesta de Proyecto Final (APF1 - UTP)** correspondiente a la sección *«Diagrama de Casos de Uso del Negocio (CUN)»*, la cual establece expresamente:  
> *«Identificación de actores del negocio y funcionalidades principales. **Obligatorio: Elaborar la tabla con sus roles y actividades del negocio**»*.

Su objetivo es caracterizar formalmente a cada actor del negocio y trabajador del negocio, delimitando sus actividades operativas actuales (`ACT-01` a `ACT-14`) y su participación exacta en los 5 Casos de Uso del Negocio (`CUN-01` a `CUN-05`).

> [!NOTE]
> **Delimitación de Actores del Negocio:**  
> En estricto apego al estándar RUP, los componentes tecnológicos de software o infraestructura (tales como el Bus de Integración o NSDG) no se consideran actores del negocio. La sincronización de datos (`ACT-12` / `RF-10`) se modela como una actividad técnica transversal de actualización que opera de forma automatizada en el proceso.

---

## 2. Tabla Consolidada de Roles y Actividades del Negocio

| Rol / Actor del Negocio | Tipo en Metodología RUP | Responsabilidad Operativa Principal en el AS-IS | Actividades AS-IS Vinculadas | CUNs en que Participa | Sistemas Utilizados | Evidencia Primaria (`trascrito.text`) |
|:---|:---:|:---|:---:|:---:|:---|:---|
| **Consultora / Consultor de Yanbal** | «Business Actor»<br/>*(Actor del Negocio)* | Cliente primario de la distribución. Recibe físicamente el pedido en su domicilio y consulta la situación de entrega en el portal web. | [[03 - Actividades y eventos#ACT-13\|ACT-13]]<br/>*(Receptora presencial en CUN-03; el registro operativo corresponde al transportista)* | [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-03\|CUN-03]], [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-05\|CUN-05]] | Portal Web / Canal de Tracking | Líneas 21-22, 24-25<br/>(min 22:03–22:28, 24:45) |
| **Familiar Autorizado** | «Business Actor»<br/>*(Actor del Negocio)* | Receptor alterno presente en el domicilio cuando la consultora titular está ausente. Recibe físicamente el paquete e interactúa con el transportista. | *(Receptor presencial en CUN-03; el registro operativo corresponde al transportista)* | [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-03\|CUN-03]] | Interacción física en campo | Línea 25<br/>(min 24:45–25:01) |
| **Socio Logístico / Transportista** | «Business Actor»<br/>*(Actor del Negocio)* | Proveedor externo contratado. Asume la custodia en despacho, realiza el transporte multimodal nacional, ejecuta la entrega física y realiza los registros operativos en campo (entrega, receptor, falla e inicio de retorno). | [[03 - Actividades y eventos#ACT-05\|ACT-05]], [[03 - Actividades y eventos#ACT-06\|ACT-06]], [[03 - Actividades y eventos#ACT-07\|ACT-07]], [[03 - Actividades y eventos#ACT-08\|ACT-08]], [[03 - Actividades y eventos#ACT-09\|ACT-09]], [[03 - Actividades y eventos#ACT-10\|ACT-10]], [[03 - Actividades y eventos#ACT-11\|ACT-11]] | [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-01\|CUN-01]], [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-02\|CUN-02]], [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-03\|CUN-03]], [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-04\|CUN-04]] | NSDG / Driving | Líneas 1:24, 18, 19, 20<br/>(min 19:15–20:55) |
| **Supervisor de Zona de Despacho** | «Business Worker»<br/>*(Trabajador del Negocio)* | Operador interno de Yanbal en el CD. Clasifica pedidos por destino geográfico, asocia transportista, modalidad y promesa de entrega, y registra el egreso formal (Estado *"Despachado"*). | [[03 - Actividades y eventos#ACT-01\|ACT-01]], [[03 - Actividades y eventos#ACT-02\|ACT-02]], [[03 - Actividades y eventos#ACT-03\|ACT-03]], [[03 - Actividades y eventos#ACT-04\|ACT-04]], [[03 - Actividades y eventos#ACT-05\|ACT-05]] | [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-01\|CUN-01]] | Driving / NSDG | Líneas 14, 16-18<br/>(min 17:34–19:44) |
| **Agente de Servicio al Cliente** | «Business Worker»<br/>*(Trabajador del Negocio)* | Operador interno de atención. Consulta la situación y promesa del pedido en Salesforce para responder a las consultas de las consultoras. | [[03 - Actividades y eventos#ACT-13\|ACT-13]] | [[01 - Diagrama General de Casos de Uso del Negocio (CUN)#CUN-05\|CUN-05]] | Salesforce (Cellforce) | Líneas 24-25, 32<br/>(min 24:29–24:36, 28:35) |
| **Jefatura de Distribución** *(Ing. Joao Condorpusa)* | «Business Worker»<br/>*(Trabajador del Negocio)* | Responsable de gobierno logístico. Supervisa el cumplimiento de los tiempos de entrega (24h Lima / 7d provincias) y evalúa el desempeño de los transportistas asociados. | Ninguna actividad operativa directa en los CUN definidos | Ninguno *(Rol de gobierno/supervisión general, sin CUN asociado)* | Plataformas de gestión | Entrevista completa (`trascrito.text`) |

---

## 3. Fichas Descriptivas Detalladas por Rol

### Ficha 1: Consultora / Consultor de Yanbal
- **Clasificación RUP:** «Business Actor» (Actor del Negocio externo).
- **Misión Operativa:** Cliente final del proceso de distribución. Recibe físicamente los pedidos de su campaña y consulta el estado de seguimiento.
- **Participación en los CUN:**
  - `CUN-03 (Entregar Pedido en Domicilio):` Receptora titular del pedido en destino (participación física presencial; el registro operativo en sistema es realizado por el transportista).
  - `CUN-05 (Consultar Trazabilidad y Situación del Pedido):` Consultante de la situación de su pedido mediante Número de Pedido o Código de Consultora (`ACT-13`).
- **Impacto de Problemas AS-IS:** Padece **PR-03** (información desfasada por hasta 2 horas en el portal) y **PR-05** (al no ser notificada oportunamente si recibió un familiar, presume pérdida de su paquete).

### Ficha 2: Familiar Autorizado
- **Clasificación RUP:** «Business Actor» (Actor del Negocio externo).
- **Misión Operativa:** Persona presente en el domicilio que actúa como receptor alterno cuando la consultora titular no se encuentra disponible.
- **Participación en los CUN:**
  - `CUN-03 (Entregar Pedido en Domicilio):` Recibe físicamente el paquete e interactúa con el transportista (participación física presencial; el registro operativo es ejecutado por el transportista).
- **Impacto de Problemas AS-IS:** Centro del problema **PR-05**: su recepción física es válida, pero la falta de visibilidad oportuna de sus datos en la consulta genera falsos reclamos por pérdida.

### Ficha 3: Socio Logístico / Transportista Asociado
- **Clasificación RUP:** «Business Actor» (Actor del Negocio externo / Contratista de transporte).
- **Misión Operativa:** Empresa proveedora responsable de la conducción material de la distribución desde el CD hasta el punto de entrega o retorno, ejecutando los registros operativos correspondientes.
- **Participación en los CUN:**
  - `CUN-01 (Despachar Pedidos desde CD):` Asume la custodia formal de la carga consolidada (`ACT-05`).
  - `CUN-02 (Trasladar Pedidos hacia Destino Nacional):` Registra el inicio de ruta (Estado *"En Ruta"*, `ACT-06`) y realiza el traslado multimodal cubriendo los 24 departamentos (`ACT-07`).
  - `CUN-03 (Entregar Pedido en Domicilio):` Ejecuta la entrega física, registra la confirmación (Estado *"Entregado"*, `ACT-08`) y la condición de quien recibió (`ACT-09`).
  - `CUN-04 (Gestionar Entrega Fallida y Retorno por Logística Inversa):` Responsable de tipificar la contingencia (retraso, pérdida o daño, pasando a Estado *"Entrega Fallida"*, `ACT-10`) y registrar el inicio del retorno de la carga no entregada (`ACT-11`).
- **Sistemas Operativos:** Herramientas de campo vinculadas a **NSDG** y **Driving**.

### Ficha 4: Supervisor de Zona de Despacho
- **Clasificación RUP:** «Business Worker» (Trabajador del Negocio interno).
- **Misión Operativa:** Operador del Centro de Distribución responsable de clasificar los pedidos y formalizar la salida de muelle.
- **Participación en los CUN:**
  - `CUN-01 (Despachar Pedidos desde CD):` Recibe pedidos de picking (`ACT-01`), los clasifica geográficamente mediante Driving (`ACT-02`), asocia socio logístico y modalidad (`ACT-03`), vincula lead time (`ACT-04`) y formaliza la salida en muelle (`ACT-05`, Estado *"Despachado"*).
- **Aclaración de Delimitación:** Participa exclusivamente en `CUN-01`. No interviene en `CUN-04` (la gestión de contingencias y registro de inicio de retorno en campo compete al transportista).

### Ficha 5: Agente de Servicio al Cliente
- **Clasificación RUP:** «Business Worker» (Trabajador del Negocio interno).
- **Misión Operativa:** Operador de atención que responde a las consultas de las consultoras.
- **Participación en los CUN:**
  - `CUN-05 (Consultar Trazabilidad y Situación del Pedido):` Consulta en Salesforce el estado de entrega y la promesa del pedido (`ACT-13`) ante solicitudes de seguimiento.
- **Impacto de Problemas AS-IS:** Padece directamente **PR-04** (margen de desconocimiento de hasta 2 horas provocado por la latencia en la actualización) y la falta de visibilidad del receptor real (PR-05).

### Ficha 6: Jefatura de Distribución (Ing. Joao Condorpusa)
- **Clasificación RUP:** «Business Worker» (Trabajador del Negocio interno / Gobierno logístico).
- **Misión Operativa:** Autoridad responsable del gobierno logístico y supervisión general de lead times.
- **Participación en los CUN:** Rol de gobierno y supervisión general de lead times. No se asocia a ningún CUN por no existir una actividad operativa concreta que ejecute dentro de los CUN definidos.

---

## 4. Matriz de Roles Excluidos del Modelado del Negocio

| Rol Excluido | Área Funcional | Justificación Técnica de Exclusión |
|:---|:---|:---|
| **Operarios de Manufactura y Envasado** | Producción | Elaboración de producto y rotulado UA en cajas mono SKU. Etapa previa al despacho. |
| **Operarios de Almacén de PT** | Almacenamiento General | Manejo de pallets en estanterías (SAP R3). Etapa previa a la consolidación unitaria. |
| **Operarios de Picking Unitario** | Centro de Distribución (SPY) | Armado y empaque de pedidos en cajas 1 a 8. Concluye al derivar el bulto a despacho. |
| **Inspectores de Control de Calidad** | Calidad Técnica | Evaluación pericial interna en almacén sobre devoluciones. Fuera del transporte. |
| **Analistas de Seguridad Patrimonial** | Prevención de Pérdidas | Liquidación de siniestros y pólizas de seguro dentro de la empresa. Fuera de transporte. |
| **Operadores de Plataforma Comercial** | Ventas (Maya / SAP Commerce) | Toma y facturación de órdenes comerciales. Etapa comercial previa. |
| **Analistas de CRM / Mesa de Ayuda** | Servicio Postventa | Gestión de tickets de reclamo comercial en Salesforce. Excluidos de trazabilidad logística. |
