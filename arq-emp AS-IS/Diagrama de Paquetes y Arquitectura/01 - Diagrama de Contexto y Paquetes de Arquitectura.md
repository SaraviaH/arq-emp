# Diagrama de Contexto y Paquetes de Arquitectura del Sistema Propuesto

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Marco Metodológico:** TOGAF / UML 2.5 — Arquitectura de Sistemas de Información / UTP APF1 (§ 3.3, ítem 9)  
> **Diagrama PlantUML:** [[Diagrama_Contexto_Paquetes_Sistema.puml]]  
> **Diagrama Previo (Negocio):** [[01 - Diagrama de Contexto del Negocio (AS-IS)]] | [[Contexto_Negocio_AS_IS.puml]]  
> **Enlaces a Requerimientos y CUS:**  
> [[01 - Matriz Consolidada de Requerimientos]] | [[01 - Diagrama General de Casos de Uso del Sistema (CUS)]] | [[01 - Diagrama de Clases de Diseño Lógico]]

---

## 1. Propósito y Delimitación de Fronteras

El **Diagrama de Contexto y Paquetes** formaliza la arquitectura técnica de alto nivel de la solución de software propuesta para Yanbal Perú. Su objetivo es delimitar de forma inequívoca:

1. **La Frontera del Sistema Propuesto:** Aquellos módulos, bases de datos y servicios que son diseñados, desarrollados y gobernados por la nueva solución de trazabilidad y distribución.
2. **Los Paquetes Lógicos Internos (Subsistemas):** Agrupaciones funcionales cohesivas de componentes de software estructuradas sin dependencias circulares.
3. **El Contexto de Interacción con Sistemas Externos:** Las interfaces de entrada y salida con los sistemas legados empresariales que operan en la corporación (SAP R3, SPY, Salesforce CRM, Driving/NSDG y el portal comercial Maya).

```mermaid
flowchart TB
    subgraph EXTERNOS ["Sistemas Legados Externos (Fuera de la Solución)"]
        SAP["SAP R3 / ERP<br/>(Órdenes y Stock)"]
        SPY["SPY / WMS<br/>(Líneas de Picking CD)"]
        CRM["Salesforce CRM<br/>(Servicio al Cliente)"]
        MAYA["Portal Maya<br/>(Canal Web Consultoras)"]
    end

    subgraph FRONTERA ["FRONTERA DEL SISTEMA PROPUESTO (ARQUITECTURA DE PAQUETES)"]
        P_DESP["<b>pkg_despacho</b><br/>• Clasificación Geográfica<br/>• Asignación Multimodal<br/>• Salida de CD y Custodia"]
        P_TRANS["<b>pkg_transporte</b><br/>• Inicio de Traslado<br/>• Telemetría GPS en Ruta"]
        P_ENT["<b>pkg_entrega</b><br/>• Confirmación Entrega<br/>• Identificación Receptor (PR-05)<br/>• Gestión de Retornos"]
        P_SYNC["<b>pkg_sincronizacion</b><br/>• Ingesta Lotes Offline<br/>• Broker de Integración<br/>• Propagador (≤ 30 min)"]
        P_CONS["<b>pkg_consulta</b><br/>• API REST Tracking<br/>• Búsqueda N° Pedido<br/>• Exposición Receptor Real"]
        P_SEG["<b>pkg_seguridad</b><br/>• Roles y Permisos (RBAC)<br/>• Auditoría Transaccional"]
    end

    SPY -->|Bultos consolidados rotulados| P_DESP
    SAP -->|Datos maestros de compra| P_DESP
    
    P_TRANS -->|<<use>>| P_DESP
    P_ENT -->|<<use>>| P_TRANS
    P_TRANS -->|<<use>>| P_SYNC
    P_ENT -->|<<use>>| P_SYNC
    P_SYNC -->|<<use>>| P_CONS
    
    P_SYNC -->|Notificación de entrega ≤ 30 min| CRM
    P_CONS <--|Consulta de estados| MAYA
    P_CONS <--|Consulta por ticket de reclamo| CRM

    style EXTERNOS fill:#ECEFF1,stroke:#546E7A,stroke-width:1px
    style FRONTERA fill:#E8EAF6,stroke:#283593,stroke-width:2px
    style P_DESP fill:#FFFFFF,stroke:#3F51B5
    style P_TRANS fill:#FFFFFF,stroke:#3F51B5
    style P_ENT fill:#FFFFFF,stroke:#3F51B5
    style P_SYNC fill:#FFF8E1,stroke:#FF8F00,stroke-width:2px
    style P_CONS fill:#FFFFFF,stroke:#3F51B5
    style P_SEG fill:#E0F2F1,stroke:#00796B
```

---

## 2. Catálogo Detallado de Paquetes Lógicos del Sistema

### 2.1. Paquete: `pkg_despacho` (Subsistema de Despacho y Zonificación en CD)
- **Responsabilidad:** Gestionar las operaciones de muelle en el Centro de Distribución de Lurín. Recibe la notificación de bultos terminados de picking, agrupa los pedidos por destino geográfico (24 departamentos y tipo de localidad) y formaliza la transferencia de custodia física al socio logístico.
- **Módulos Internos:**
  - `Módulo de Clasificación Geográfica`: Algoritmo de filtrado por ubigeo nacional (`CUS-01`, `RF-01`).
  - `Módulo de Asignación Multimodal`: Vinculación de transportista y modo terrestre/bimodal/aéreo (`CUS-02`, `RF-02`).
  - `Módulo de Salida de CD y Custodia`: Registro del egreso y asociación del lead time comprometido (`CUS-03`, `CUS-04`, `RF-03`, `RF-04`).
- **Dependencias:** Consume servicios de `pkg_seguridad` para validar credenciales del supervisor.

---

### 2.2. Paquete: `pkg_transporte` (Subsistema de Transporte y Control de Ruta)
- **Responsabilidad:** Dar soporte a la unidad de transporte durante el trayecto físico nacional. Registra el hito formal de partida desde el muelle y mantiene la línea de tiempo del envío activa.
- **Módulos Internos:**
  - `Módulo de Inicio de Viaje`: Transiciona el estado del pedido a *"En Ruta"* al momento en que el transportista inicia el traslado (`CUS-05`, `RF-05`).
  - `Módulo de Telemetría GPS en Tránsito`: Captura marcas de tiempo y posición en puntos de control o geocercas intermedias.
- **Dependencias:** Utiliza los datos del pedido provistos por `pkg_despacho` y encola los eventos en `pkg_sincronizacion`.

---

### 2.3. Paquete: `pkg_entrega` (Subsistema de Entrega en Sede y Contingencias)
- **Responsabilidad:** Gestionar la culminación de la entrega física en la Sede o Agencia de Distribución de destino y atender las incidencias que impidan el cierre regular.
- **Módulos Internos:**
  - `Módulo de Confirmación de Entrega`: Captura la estampa temporal de cierre y la firma electrónica (`CUS-06`, `RF-06`).
  - `Módulo de Identificación de Receptor en Sede`: Captura obligatoriamente los datos de acreditación del personal autorizado en la sede de destino (nombres, DNI y cargo) que consigna y firma la recepción de la carga mono SKU (`CUS-07`, `RF-07`).
  - `Módulo de Incidencias y Retorno`: Tipifica contingencias en ruta o arribo (retraso, pérdida o daño) y genera la orden de logística inversa hacia el CD de Lurín cuando existe carga física rechazada o siniestrada (`CUS-08`, `CUS-09`, `RF-08`, `RF-09`).
- **Dependencias:** Utiliza la información de ruta de `pkg_transporte` y despacha el acta de entrega a `pkg_sincronizacion`.

---

### 2.4. Paquete: `pkg_sincronizacion` (Subsistema de Sincronización e Integración)
- **Responsabilidad:** Núcleo de comunicación asíncrona que resuelve el cuello de botella histórico del proceso actual (**PR-03 y PR-04: desfase de 2 horas**).
- **Módulos Internos:**
  - `Servicio de Ingesta de Lotes Offline`: Recibe los paquetes de transacciones almacenados localmente en los terminales de los transportistas durante tramos sin cobertura celular (`RNF-03`).
  - `Cola de Mensajería y Broker ESB`: Desacopla la ingesta de alto volumen del procesamiento transaccional en la base de datos central.
  - `Propagador de Estados en Tiempo Cuasi-Real`: Publica los eventos hacia los sistemas consumidores asegurando una latencia menor a 30 minutos (**RNF-01**).
- **Dependencias:** Impacta directamente en `pkg_consulta` y expone eventos hacia el CRM corporativo.

---

### 2.5. Paquete: `pkg_consulta` (Subsistema de Consulta y Seguimiento)
- **Responsabilidad:** Exponer la información consolidada del ciclo de vida del pedido a las usuarias y al personal de soporte al cliente.
- **Módulos Internos:**
  - `API REST de Trazabilidad`: Endpoints JSON protegidos para consultas de tracking.
  - `Módulo de Consulta por N° Pedido / Cód. Consultora`: Motor de búsqueda indexado de alta velocidad (`CUS-11`, `RF-11`).
  - `Módulo de Exposición del Receptor Real`: Despliega de forma transparente los datos del receptor (nombre, parentesco y fecha/hora) para evitar reclamos infundados por presunta pérdida (`CUS-12`, `RF-12`).

---

### 2.6. Paquete: `pkg_seguridad_transversal` (Seguridad, Roles y Auditoría)
- **Responsabilidad:** Proporcionar gobernanza transversal a todos los subsistemas.
- **Módulos Internos:**
  - `Control de Accesos Basado en Roles (RBAC)`: Delimita permisos específicos para Supervisor de Despacho, Transportista, Agente SAC y Consultora (`RNF-05`).
  - `Log Transaccional de Auditoría`: Registra pistas de auditoría inmutables de cada cambio de estado, hora de salida y entrega (`RNF-06`).

---

## 3. Matriz de Dependencias e Intercambios del Sistema

| Paquete Origen | Paquete / Sistema Destino | Tipo de Dependencia | Objeto / Dato Transferido | Protocolo / Mecanismo |
|:---|:---|:---:|:---|:---|
| `SPY (WMS)` | `pkg_despacho` | Interfaz Externa | Pedidos consolidados en cajas formatos 1-8 | Mensajería / Webhook WMS |
| `SAP R3` | `pkg_despacho` | Interfaz Externa | Órdenes comerciales y direcciones de entrega | Interfaz RFC / API REST |
| `pkg_transporte` | `pkg_despacho` | `<<use>>` | Validación de pedido asignado a ruta | Llamada a servicio interno |
| `pkg_entrega` | `pkg_transporte` | `<<use>>` | Confirmación de arribo a destino | Llamada a servicio interno |
| `pkg_transporte` | `pkg_sincronizacion` | `<<use>>` | Hitos de ruta y telemetría encolada | HTTP POST / JSON batch |
| `pkg_entrega` | `pkg_sincronizacion` | `<<use>>` | Acta de entrega, firma y datos del receptor | HTTP POST / Multipart JSON |
| `pkg_sincronizacion` | `pkg_consulta` | `<<use>>` | Actualización de tabla maestra de tracking | Event-Driven / Pub-Sub |
| `pkg_sincronizacion` | `Salesforce CRM` | Interfaz Externa | Actualización de estado y receptor ($\le 30$ min) | API REST Salesforce |
| `Portal Maya` | `pkg_consulta` | Interfaz Externa | Petición de trazabilidad por consultora | API REST JSON (TLS 1.3) |
| `Salesforce CRM` | `pkg_consulta` | Interfaz Externa | Consulta de estatus por agente SAC | Canvas App / REST API |

---
