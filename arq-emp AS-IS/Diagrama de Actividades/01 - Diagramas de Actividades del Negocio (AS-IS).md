# Diagramas de Actividades del Negocio (AS-IS)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Alcance del Proceso:** Despacho $\longrightarrow$ Transporte/Ruta $\longrightarrow$ Llegada $\longrightarrow$ Entrega $\longrightarrow$ Logística Inversa $\longrightarrow$ Actualización $\longrightarrow$ Consulta  
> **Marco Metodológico:** RUP (*Rational Unified Process*) — Modelado de Comportamiento del Negocio / UML 2.5 / UTP APF1 (§ 3.3, ítem 3)  
>
> **Navegación del Módulo de Actividades:**  
> [[01 - Diagramas de Actividades del Negocio (AS-IS)]]  
> **Archivos PlantUML Fuentes:**  
> - [`01_Flujo_General_AS_IS_Integrado.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/01_Flujo_General_AS_IS_Integrado.puml)  
> - [`02_Flujo_AS_IS_Problema_Critico_Desfase_Receptor.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/02_Flujo_AS_IS_Problema_Critico_Desfase_Receptor.puml)  
> - [`CUN_01_Despachar.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_01_Despachar.puml) | [`CUN_02_Trasladar.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_02_Trasladar.puml) | [`CUN_03_Entregar.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_03_Entregar.puml) | [`CUN_04_Gestionar_Entrega_Fallida_Retorno.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_04_Gestionar_Entrega_Fallida_Retorno.puml) | [`CUN_05_Consultar_Trazabilidad.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_05_Consultar_Trazabilidad.puml)  
>
> **Enlaces a Módulos Relacionados:**  
> [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[05 - Problemas y desfases]]  
> [[01 - Matriz Consolidada de Requerimientos]] | [[01 - Diagrama General de Casos de Uso del Negocio (CUN)]] | [[02 - Tabla de Roles y Actividades del Negocio]] | [[01 - Diagrama de Clases de Dominio (Nivel Dominio)]]

---

## 1. Fundamentación y Propósito Metodológico

El presente documento formaliza los **Diagramas de Actividades del Negocio en su estado actual (AS-IS)**, dando cumplimiento riguroso y exhaustivo a las directrices estipuladas en la [Guía Oficial de Propuesta de Proyecto Final](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/trascirto%20pdf/02%20-%20Gu%C3%ADa%20Oficial%20de%20Propuesta%20de%20Proyecto%20Final.md) (APF1, § 3.3, numeral 3), la cual exige taxativamente:
1. **Flujo general de los procesos de negocio actuales:** Representación integrada de extremo a extremo que articula la totalidad de las actividades operativas de la cadena.
2. **Flujo de los procesos de negocio actuales enfocados en el problema crítico a resolver:** Representación focalizada que modela la causa raíz de las ineficiencias del negocio (el desfase de actualización de hasta 2 horas y la falta de visibilidad del receptor real).

### Principios de Modelado bajo RUP y UML 2.5:
- **Carriles de Responsabilidad (*Swimlanes*):** Las acciones están distribuidas en particiones organizacionales que delimitan claramente las obligaciones de cada rol humano:
  - `Supervisor de Zona de Despacho` («Business Worker»): Control interno de egreso en el CD de Yanbal.
  - `Socio Logístico / Transportista` («Business Actor»): Contratista tercero responsable de la custodia física, traslado multimodal y entrega en campo.
  - `Consultora / Consultor` («Business Actor»): Cliente primario y destinataria titular.
  - `Persona Autorizada` («Business Actor»): Receptor presencial en destino ante la ausencia de la titular.
  - `Agente de Servicio al Cliente` («Business Worker»): Operador interno de soporte en la plataforma CRM Salesforce.
- **Tratamiento Metodológico de los Sistemas:** Siguiendo el estándar RUP de modelado de negocio, las plataformas técnicas (Driving, NSDG, Salesforce, Portal Web) no se configuran como actores del negocio, sino como herramientas de soporte de las actividades humanas o como actividades transversales de sincronización técnica de datos.

---

## 2. Diagrama de Flujo General de Procesos AS-IS (End-to-End)

El flujo general modela el ciclo de vida operativo completo: desde que un pedido empacado arriba a la Zona de Despacho hasta su entrega física (o retorno por siniestro) y su posterior consulta informativa.

### 2.1. Representación Gráfica Integrada (Mermaid)

```mermaid
flowchart TD
    subgraph DESPACHO ["Zona de Despacho (Supervisor Yanbal)"]
        A1(["Inicio"]) --> A2["ACT-01: Recibir pedidos empacados de picking"]
        A2 --> A3["ACT-02: Clasificar por destino (24 departamentos)"]
        A3 --> A4["ACT-03: Asignar socio logístico y modalidad"]
        A4 --> A5["ACT-04: Asociar promesa estimada de entrega"]
        A5 --> A6["ACT-05: Registrar salida de despacho y entrega formal"]
    end

    subgraph TRANSPORTE ["Transporte y Ruta (Socio Logístico)"]
        A6 --> B1["ACT-05: Asumir custodia física en muelle"]
        B1 --> B2["ACT-06: Cargar e iniciar traslado ('En Ruta')"]
        B2 --> B3["ACT-07: Trasladar según modalidad (Terrestre/Bimodal/Aérea)"]
        B3 --> B4{"¿Entrega viable en destino?"}
    end

    subgraph ENTREGA ["Llegada y Destino (Campo)"]
        B4 -- Sí --> C1{"¿Quién recibe en domicilio?"}
        C1 -- Consultora Titular --> C2["ACT-09: Identificar y entregar a titular"]
        C1 -- Persona Autorizada --> C3["ACT-09: Identificar y entregar a persona autorizada"]
        C2 --> C4["ACT-08: Registrar entrega física ('Entregado')"]
        C3 --> C4
    end

    subgraph INCIDENCIAS ["Contingencias y Logística Inversa"]
        B4 -- No --> D1["ACT-10: Registrar entrega fallida e incidencia ('Entrega Fallida')"]
        D1 --> D2{"¿Bulto físico presente?"}
        D2 -- Sí (Retraso/Daño) --> D3["ACT-11: Registrar inicio de retorno y traslado al CD"]
        D2 -- No (Pérdida/Robo) --> D4["Cierre operativo de siniestro sin retorno físico"]
    end

    subgraph SINCRONIZACION ["Sincronización Transversal"]
        C4 --> E1["ACT-12: Sincronización batch hacia sistemas centrales\n(⚠️ Desfase de hasta 2 horas en AS-IS)"]
        D3 --> E1
        D4 --> E1
    end

    subgraph CONSULTA ["Actualización y Consulta"]
        E1 --> F1{"Canal de Consulta"}
        F1 -- Portal Web --> F2["ACT-13/ACT-14: Consultora consulta estado y receptor"]
        F1 -- Salesforce CRM --> F3["ACT-13/ACT-14: Agente consulta y atiende reclamo"]
        F2 --> F4(["Fin del Proceso"])
        F3 --> F4
    end

    style E1 fill:#FFEBEE,stroke:#C62828,stroke-width:2px;
    style C3 fill:#FFF3E0,stroke:#EF6C00,stroke-width:1px;
```

### 2.2. Código Fuente PlantUML Oficial

> **Archivo descargable:** [`01_Flujo_General_AS_IS_Integrado.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/01_Flujo_General_AS_IS_Integrado.puml)

```plantuml
@startuml 01_Flujo_General_AS_IS_Integrado
!theme plain
skinparam shadowing false
skinparam roundcorner 8
skinparam defaultFontName "Arial"
skinparam defaultFontSize 11
skinparam ArrowColor #37474F
skinparam ActivityBorderColor #455A64
skinparam ActivityBackgroundColor #ECEFF1

title Diagrama de Actividades AS-IS: Flujo General de Negocio Integrado (End-to-End)\nProceso: Despacho -> Transporte -> Entrega -> Logística Inversa -> Sincronización -> Consulta

|#EFEBE9|Supervisor de Zona de Despacho|
start
:Recibir pedidos consolidados y empacados\ndesde la línea de picking (ACT-01);
:Clasificar pedidos por destino geográfico\n(24 departamentos del Perú) (ACT-02);
:Asignar socio logístico y modalidad\nde transporte (Terrestre, Bimodal, Aérea) (ACT-03);
:Asociar promesa estimada de entrega según destino\n(24h Lima Metropolitana / hasta 7d Provincias) (ACT-04);
:Registrar salida de despacho y formalizar entrega de carga (ACT-05);
:Transferir formalmente la custodia física de la carga;

|#E8EAF6|Socio Logístico / Transportista|
:Asumir custodia física de la carga en muelle de CD (ACT-05);
:Cargar bultos en unidades según modalidad (ACT-06);
:Registrar inicio de traslado físico del pedido\n(Estado cambia a "En Ruta") (ACT-06);
:Trasladar pedidos por la ruta asignada según modalidad\n(Terrestre, Bimodal o Aérea) (ACT-07);
:Arribar a la localidad y presentarse en el domicilio de entrega;

if (¿Se puede efectuar la entrega física en destino?) then ([Entrega viable en domicilio])

  if (¿Quién recibe presencialmente en el domicilio?) then ([Consultora titular presente])
    |#E0F2F1|Consultora / Titular|
    :Identificarse como receptora titular;
    :Recibir físicamente los bultos del pedido;
    |#E8EAF6|Socio Logístico / Transportista|
    :Verificar identidad de la consultora titular;
    :Registrar datos del receptor titular (ACT-09);
  else ([Persona autorizada presente])
    |#FFF3E0|Persona Autorizada|
    :Identificarse como persona autorizada en domicilio;
    :Recibir físicamente los bultos del pedido;
    |#E8EAF6|Socio Logístico / Transportista|
    :Verificar identidad y registrar vínculo/parentesco;
    :Registrar datos del receptor (Personal Autorizado) (ACT-09);
  endif

  |#E8EAF6|Socio Logístico / Transportista|
  :Registrar confirmación de entrega física en destino\n(Estado cambia a "Entregado") (ACT-08);

else ([Incidencia impeditiva en ruta o domicilio])

  |#E8EAF6|Socio Logístico / Transportista|
  :Constatar incidencia tipificada\n(Retraso / Pérdida / Daño) (ACT-10);
  :Registrar entrega fallida e incidencia en aplicativo de campo\n(Estado cambia a "Entrega Fallida") (ACT-10);

  if (¿Existe bulto físico disponible para retornar?) then ([Retraso o Daño (bulto presente)])
    :Registrar inicio de retorno del pedido hacia el CD\n(Generar Orden de Retorno) (ACT-11);
    :Trasladar carga no entregada hacia el Centro de Distribución\n(Flujo de Logística Inversa) (ACT-11);
  else ([Pérdida o Siniestro (sin bulto físico)])
    :Cerrar gestión operativa en campo sin retorno de carga;
  endif

endif

|#ECEFF1|Proceso Técnico de Sincronización (Transversal)|
note right
  **Brecha Tecnológica AS-IS (ACT-12):**
  Los eventos registrados en campo (Driving / NSDG)
  no se sincronizan en tiempo real con las plataformas
  corporativas, sufriendo un desfase batch de ~2 horas.
end note
:Sincronizar lote de estados y confirmaciones hacia plataformas centrales\n(Desfase de hasta 2 horas en el AS-IS) (ACT-12);

if (¿Canal por el cual se consulta el pedido?) then ([Consultora vía Portal Web])
  |#E0F2F1|Consultora / Titular|
  :Ingresar al Portal Web de seguimiento con N° Pedido o Cód. Consultora;
  :Consultar situación actual y promesa estimada de entrega (ACT-13);
  :Visualizar estado disponible del pedido (ACT-13);
  :Visualizar datos disponibles sobre la entrega del pedido (ACT-14);
else ([Atención telefónica vía Servicio al Cliente])
  |#FCE4EC|Agente de Servicio al Cliente|
  :Recibir llamada o ticket de consulta de la consultora;
  :Ingresar N° Pedido o Cód. Consultora en Salesforce (CRM);
  :Consultar situación registrada y promesa de entrega (ACT-13);
  :Verificar datos disponibles sobre la entrega física (ACT-14);
  :Informar a la consultora el estado visualizado en sistema;
endif

stop
@enduml
```

---

## 3. Diagrama Enfocado en el Problema Crítico (Desfase de 2h y Receptor)

Este diagrama modela de forma minuciosa la patología operacional que experimenta Yanbal en su operación actual: la interacción perversa entre la **latencia de sincronización batch de 2 horas (`PR-03` / `PR-04`)** y la **recepción por Persona Autorizada sin visibilidad en línea (`PR-05`)**.

### 3.1. Representación del Problema Crítico (Mermaid)

```mermaid
sequenceDiagram
    autonumber
    actor T as Transportista en Campo
    actor PA as Persona Autorizada (Domicilio)
    participant APP as Driving / NSDG (Móvil)
    participant BATCH as Cola Sincronización Batch
    actor C as Consultora Titular
    participant WEB as Portal Web / Tracking
    actor A as Agente Servicio al Cliente
    participant SF as Salesforce CRM

    T->>PA: Llega al domicilio y entrega paquete
    PA-->>T: Se identifica (DNI y parentesco: Hija)
    T->>APP: ACT-08 / ACT-09: Registra entrega física y datos de receptor
    Note over APP,BATCH: Momento T0: Entrega completada físicamente
    APP->>BATCH: Encola lote para transmisión
    
    rect rgb(255, 235, 238)
        Note over BATCH,WEB: ⚠️ DESFASE CRÍTICO DE 2 HORAS (PR-03 / PR-04)<br/>Los sistemas centrales no reciben la actualización
        C->>WEB: ACT-13: Consulta tracking desde su trabajo
        WEB-->>C: Muestra estado obsoleto: "En Ruta" (o "Entregado" sin datos de quién recibió)
        Note over C: PR-05: Titular no ve su paquete en casa ni sabe quién lo recibió.<br/>Presume pérdida o robo.
        C->>A: Llama angustiada al Call Center reclamando su pedido
        A->>SF: ACT-13: Consulta N° Pedido en CRM
        SF-->>A: Muestra datos desfasados: "En Ruta" (o sin datos de receptor)
        A-->>C: Confirma que no figura entregado o no tiene datos de quién firmó
        A->>SF: Registra ticket de reclamo formal por siniestro/pérdida
    end

    Note over BATCH,SF: 2 horas después de T0: Ejecuta sincronización batch
    BATCH->>WEB: Actualiza estado a "Entregado"
    BATCH->>SF: Actualiza datos de entrega y receptor
    A->>SF: Detecta tardíamente que el paquete fue recibido por la hija
    Note over A,C: Se anula el reclamo, pero ya se incurrió en costos operativos,<br/>desgaste de Call Center y pérdida de confianza.
```

### 3.2. Código Fuente PlantUML del Problema Crítico

> **Archivo descargable:** [`02_Flujo_AS_IS_Problema_Critico_Desfase_Receptor.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/02_Flujo_AS_IS_Problema_Critico_Desfase_Receptor.puml)

```plantuml
@startuml 02_Flujo_AS_IS_Problema_Critico_Desfase_Receptor
!theme plain
skinparam shadowing false
skinparam roundcorner 8
skinparam defaultFontName "Arial"
skinparam defaultFontSize 11
skinparam ArrowColor #B71C1C
skinparam ActivityBorderColor #C62828
skinparam ActivityBackgroundColor #FFEBEE
skinparam NoteBorderColor #E53935
skinparam NoteBackgroundColor #FFEBEE

title Diagrama de Actividades AS-IS: Flujo Enfocado en el Problema Crítico\n(Desfase de 2 Horas en Sincronización y Brecha de Visibilidad del Receptor Real)

|#E8EAF6|Socio Logístico / Transportista|
start
:Arribar al domicilio de la consultora;
note right
  **Condición de campo común:**
  La consultora titular se encuentra fuera del domicilio
  por motivos laborales o personales.
end note

|#FFF3E0|Persona Autorizada (en domicilio)|
:Atender en la puerta del domicilio;
:Identificarse como persona autorizada (familiar/encargado);
:Recibir físicamente los bultos del pedido;

|#E8EAF6|Socio Logístico / Transportista|
:Capturar nombres, DNI y parentesco del receptor en aplicativo móvil (ACT-09);
:Registrar confirmación de entrega física en su terminal (ACT-08);
note right
  **Momento T0:**
  La entrega física se consumó válidamente en destino.
  El registro queda almacenado localmente en Driving/NSDG.
end note

|#ECEFF1|Proceso de Sincronización Batch (Desfase AS-IS)|
:Encolar registro de entrega para sincronización por lotes;
note right #FFCDD2
  **CUELLO DE BOTELLA CRÍTICO (PR-03 / PR-04):**
  Desfase de hasta 2 horas en la transmisión de datos
  hacia el Bus de Integración, Salesforce y el Portal Web.
  Durante esta ventana de tiempo, los sistemas centrales
  mantienen información obsoleta ("En Ruta").
end note

|#E0F2F1|Consultora / Titular|
:Retornar a su domicilio o revisar aplicativo web de Yanbal;
:Ingresar N° Pedido para verificar estado del paquete (ACT-13);

if (¿Qué visualiza la consultora en el portal web?) then ([Portal desfasado: Estado 'En Ruta'])
  :Constatar que el sistema indica 'En Ruta' o 'Despachado';
  :Observar que el paquete ya no está en la puerta o desconocer su paradero;
else ([Portal actualizado parcialmente: Estado 'Entregado'])
  :Visualizar estado 'Entregado' sin detalle del receptor;
  note right #FFCDD2
    **BRECHA DE INFORMACIÓN (PR-05):**
    El portal no expone el nombre ni DNI de la persona
    autorizada que recibió físicamente el paquete.
    La titular no sabe quién lo tiene en su casa.
  end note
endif

:Presumir no-entrega, pérdida o sustracción de su mercadería;
:Llamar de inmediato a la central telefónica de Yanbal;

|#FCE4EC|Agente de Servicio al Cliente|
:Atender la llamada de reclamo de la consultora;
:Consultar el N° de Pedido en la plataforma Salesforce CRM (ACT-13);

if (¿Qué información visualiza el Agente en Salesforce?) then ([Desfase activo: Estado 'En Ruta'])
  :Confirmar a la consultora que el pedido sigue 'En Ruta';
  :Ingresar ticket de reclamo formal por 'Demora o no entrega';
else ([Sincronizado sin datos de receptor (PR-05)])
  :Indicar que figura 'Entregado' pero sin datos de quién firmó;
  :Registrar ticket de reclamo por 'Paquete no recibido por titular / Pérdida';
endif

note right #FFCDD2
  **IMPACTO OPERATIVO Y FINANCIERO:**
  - Sobrecarga innecesaria del Call Center (Salesforce).
  - Activación improcedente de reclamos por siniestro.
  - Fricción y desconfianza de la consultora con Yanbal.
end note

|#ECEFF1|Proceso de Sincronización Batch (Desfase AS-IS)|
:Culminar la ventana de desfase (~2 horas después de T0);
:Impactar finalmente la confirmación y datos del receptor en la base central (ACT-12);

|#FCE4EC|Agente de Servicio al Cliente|
:Verificar tardíamente que el paquete fue recibido por la persona autorizada;
:Proceder al cierre o anulación manual del ticket de reclamo infundado;

stop
@enduml
```

---

## 4. Catálogo de Diagramas de Actividades por Caso de Uso del Negocio (CUN)

Complementando los diagramas integrados, el proyecto dispone de la descomposición modular por cada uno de los 5 Casos de Uso del Negocio definidos en el módulo [[01 - Diagrama General de Casos de Uso del Negocio (CUN)]]:

| Caso de Uso del Negocio | Título Operativo del CUN | Archivo Fuente `.puml` | Actividades AS-IS Comprendidas | Actores / Swimlanes Intervinientes |
| :--- | :--- | :--- | :---: | :--- |
| **`CUN-01`** | **Despachar Pedidos desde Centro de Distribución** | [`CUN_01_Despachar.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_01_Despachar.puml) | `ACT-01` a `ACT-05` | Supervisor de Zona de Despacho, Socio Logístico |
| **`CUN-02`** | **Trasladar Pedidos hacia Destino Nacional** | [`CUN_02_Trasladar.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_02_Trasladar.puml) | `ACT-06`, `ACT-07` | Socio Logístico / Transportista |
| **`CUN-03`** | **Entregar Pedido en Domicilio** | [`CUN_03_Entregar.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_03_Entregar.puml) | `ACT-08`, `ACT-09` | Socio Logístico, Consultora Titular, Persona Autorizada |
| **`CUN-04`** | **Gestionar Entrega Fallida y Retorno por Logística Inversa** | [`CUN_04_Gestionar_Entrega_Fallida_Retorno.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_04_Gestionar_Entrega_Fallida_Retorno.puml) | `ACT-10`, `ACT-11` | Socio Logístico / Transportista |
| **`CUN-05`** | **Consultar Trazabilidad y Situación del Pedido** | [`CUN_05_Consultar_Trazabilidad.puml`](file:///c:/Users/joses/Obsidian/arq-emp/arq-emp%20AS-IS/Diagrama%20de%20Actividades/CUN_05_Consultar_Trazabilidad.puml) | `ACT-13`, `ACT-14` | Consultora / Consultor, Agente de Servicio al Cliente |

---

## 5. Matriz de Trazabilidad Integral de Actividades

La siguiente matriz certifica la coherencia y consistencia bidireccional entre las actividades operativas (`ACT-01` a `ACT-14`), los roles ejecutores, los diagramas de comportamiento (CUN / Actividades), los requerimientos de solución (`RF` / `RNF`) y los problemas reales constatados en la entrevista (`PR-01` a `PR-05`):

| Código Actividad | Nombre Operativo de la Actividad | Rol Responsable (*Swimlane*) | CUN Vinculado | Requerimiento de Solución | Problema AS-IS Asociado | Evidencia en Entrevista (`trascrito.text`) |
| :---: | :--- | :--- | :---: | :---: | :---: | :--- |
| **`ACT-01`** | Recepción de pedidos empacados de picking | Supervisor de Zona de Despacho | `CUN-01` | — | — | min 14:43–17:41 |
| **`ACT-02`** | Clasificación geográfica por destino (24 deptos) | Supervisor de Zona de Despacho | `CUN-01` | `RF-01` | — | min 17:34–19:30 |
| **`ACT-03`** | Asignación de transportista y modalidad | Supervisor de Zona de Despacho | `CUN-01` | `RF-02` | — | min 17:34–19:30 |
| **`ACT-04`** | Asociación de promesa estimada de entrega | Supervisor de Zona de Despacho | `CUN-01` | `RF-04` | — | min 22:38–22:54 |
| **`ACT-05`** | Registro de salida de despacho y entrega física | Supervisor Despacho / Transportista | `CUN-01` | `RF-03` | — | min 19:15–19:30 |
| **`ACT-06`** | Carga e inicio de traslado ("En Ruta") | Socio Logístico / Transportista | `CUN-02` | `RF-05` | — | min 19:15–19:53 |
| **`ACT-07`** | Traslado físico por ruta nacional según modalidad | Socio Logístico / Transportista | `CUN-02` | — | — | min 22:38–22:54 |
| **`ACT-08`** | Registro de confirmación de entrega en destino | Socio Logístico / Transportista | `CUN-03` | `RF-06` | `PR-01`, `PR-03` | min 19:15–20:55 |
| **`ACT-09`** | Registro de identidad del receptor presencial | Socio Logístico / Transportista | `CUN-03` | `RF-07` | `PR-05` | min 24:45–25:01 |
| **`ACT-10`** | Registro de entrega fallida por incidencia tipificada | Socio Logístico / Transportista | `CUN-04` | `RF-08` | `PR-02` | min 20:03–20:55 |
| **`ACT-11`** | Registro de inicio de retorno (Logística Inversa) | Socio Logístico / Transportista | `CUN-04` | `RF-09` | `PR-02` | min 20:03–20:55 |
| **`ACT-12`** | Sincronización diferida de estados (Transversal) | Proceso Batch Transversal | — | `RF-10`, `RNF-01` | `PR-03`, `PR-04` | min 28:35–28:50 |
| **`ACT-13`** | Consulta de situación y promesa de entrega | Consultora / Agente Servicio Cliente | `CUN-05` | `RF-11` | `PR-03`, `PR-04` | min 22:03–22:28 |
| **`ACT-14`** | Visualización de datos de entrega y receptor | Consultora / Agente Servicio Cliente | `CUN-05` | `RF-12` | `PR-05` | min 24:45–25:01 |
