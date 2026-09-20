# Diagrama General de Casos de Uso del Negocio (CUN)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Alcance del Proceso:** Despacho $\longrightarrow$ Transporte/Ruta $\longrightarrow$ Llegada $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta  
> **Marco Metodológico:** RUP (*Rational Unified Process*) — Disciplina de Modelado del Negocio / UML 2.5 / UTP APF1  
>
> **Navegación del Módulo CUN:**  
> [[01 - Diagrama General de Casos de Uso del Negocio (CUN)]] | [[02 - Tabla de Roles y Actividades del Negocio]] | [[03 - Especificación de Casos de Uso del Negocio]] | [[04 - Trazabilidad de Casos de Uso del Negocio]]  
> **Enlaces a la Base AS-IS:**  
> [[01 - Proceso actual]] | [[02 - Actores relevantes]] | [[03 - Actividades y eventos]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]  
> **Enlaces a Requerimientos:**  
> [[01 - Matriz Consolidada de Requerimientos]] | [[02 - Especificación de Requerimientos Funcionales]] | [[03 - Especificación de Requerimientos No Funcionales]] | [[04 - Trazabilidad y Registro de Depuración]]

---

## 1. Marco Conceptual del Modelado del Negocio (RUP / UML)

En la disciplina de **Modelado del Negocio** (*Business Modeling*) del marco RUP (*Rational Unified Process*), un **Caso de Uso del Negocio (CUN)** (*Business Use Case*) representa un proceso central que ejecuta la organización para proporcionar un resultado de valor observable y directo a un actor que interactúa con ella.

A diferencia de los casos de uso de sistemas (CUS), los CUN modelan la **operación del negocio en su estado actual (AS-IS)**, delimitando las responsabilidades de los roles humanos y organizacionales sin incorporar como actores del negocio a componentes técnicos ni sistemas de software.

```mermaid
flowchart LR
    subgraph MODELO_NEGOCIO ["Simbología de Modelado del Negocio en RUP"]
        direction TB
        BA["«Business Actor»<br/>Actor del Negocio<br/>(Externo al proceso)"]
        BW["«Business Worker»<br/>Trabajador del Negocio<br/>(Colaborador interno)"]
        BUC["«Business Use Case»<br/>Caso de Uso del Negocio<br/>(Proceso generador de valor)"]
    end
```

### Clasificación de Actores del Negocio
1. **Actores del Negocio (*Business Actors*):** Entidades externas que interactúan con el negocio, demandando o ejecutando servicios:
   - **Consultora / Consultor de Yanbal:** Cliente primario que recibe el pedido en su domicilio y realiza el seguimiento de su entrega.
   - **Familiar Autorizado:** Receptor presencial en el domicilio cuando la consultora titular se encuentra ausente.
   - **Socio Logístico / Transportista:** Proveedor externo asociado (contratista) que asume la custodia física, traslada la mercancía, ejecuta la entrega en destino y gestiona las contingencias de retorno.
2. **Trabajadores del Negocio (*Business Workers*):** Roles internos de Yanbal que ejecutan tareas dentro del proceso:
   - **Supervisor de Despacho:** Operador interno responsable de clasificar los pedidos por destino geográfico, asociar transportista/modalidad y formalizar el egreso de carga en muelle.
   - **Agente de Servicio al Cliente:** Operador interno que consulta la situación del pedido en el CRM (Salesforce) para atender a las consultoras.
   - **Jefatura de Distribución:** Responsable de la supervisión de lead times, acuerdos de nivel de servicio y gobierno de la red logística.

> [!IMPORTANT]
> **Tratamiento Metodológico de la Sincronización:**  
> La sincronización de estados entre las herramientas de transporte y las plataformas corporativas (asociada a `ACT-12` y `RF-10`) **no constituye un Caso de Uso del Negocio independiente ni un actor del negocio**, sino una **actividad técnica transversal de actualización** del proceso AS-IS que opera de forma automatizada en segundo plano.

---

## 2. Delimitación de Frontera del Negocio (Alcance AS-IS)

```mermaid
flowchart TD
    subgraph FUERA_ALCANCE_PREVIO ["Contexto Previo (Fuera de la Frontera CUN)"]
        CP1["Manufactura y Envasado Industrial"]
        CP2["Almacenamiento PT (SAP R3)"]
        CP3["Picking Unitario en CD (SPY)"]
        CP4["Captura Comercial (Maya / SAP Commerce)"]
    end

    subgraph FRONTERA_CUN ["Frontera del Sistema de Negocio: Cadena de Distribución (Yanbal Perú)"]
        direction TB
        C1["CUN-01: Despachar Pedidos desde Centro de Distribución"]
        C2["CUN-02: Trasladar Pedidos hacia Destino Nacional"]
        C3{"Destino: ¿Entrega Exitosa o Fallida?"}
        C4["CUN-03: Entregar Pedido en Domicilio"]
        C5["CUN-04: Gestionar Entrega Fallida y Retorno por Logística Inversa"]
        C6["CUN-05: Consultar Trazabilidad y Situación del Pedido"]
        
        ACT_TRANS["[Actividad Transversal de Actualización]<br/>ACT-12 / RF-10: Sincronización de estados (Desfase ~2h)"]
    end

    subgraph FUERA_ALCANCE_POSTERIOR ["Procesos Posteriores (Fuera de la Frontera CUN)"]
        POST1["Recepción y peritaje de calidad/seguridad en almacén"]
        POST2["Activación de pólizas de seguro y siniestros"]
        POST3["Generación de pedidos de reposición comercial"]
        POST4["Gestión de tickets de reclamo en CRM (Salesforce)"]
    end

    FUERA_ALCANCE_PREVIO -.->|Caja consolidada con N° Pedido| C1
    C1 --> C2
    C2 --> C3
    C3 -->|Camino Principal| C4
    C3 -->|Camino Alternativo| C5
    C4 -.-> ACT_TRANS
    C5 -.-> ACT_TRANS
    ACT_TRANS -.-> C6
    C5 -.->|Carga en retorno / Procesos posteriores fuera de frontera| FUERA_ALCANCE_POSTERIOR
```

- **Punto de Inicio:** Recepción física de la caja empacada con su Número de Pedido en la Zona de Despacho del Centro de Distribución.
- **Bifurcación Operativa en Destino:** Desde el traslado en ruta (`CUN-02`), el flujo contempla dos caminos alternativos directos:
  1. *Camino Principal:* Entrega exitosa al receptor en domicilio (`CUN-03`).
  2. *Camino Alternativo:* Incidencia confirmada (retraso, pérdida o daño) y registro del inicio de retorno por logística inversa (`CUN-04`).
- **Punto de Cierre:** Entrega efectiva en manos del receptor (`CUN-03`) o registro del inicio de retorno de la carga no entregada (`CUN-04`).

---

## 3. Catálogo de los 5 Casos de Uso del Negocio (CUN)

El módulo se estructura en **exactamente 5 Casos de Uso del Negocio**, correspondientes con los Requerimientos Funcionales de la [[01 - Matriz Consolidada de Requerimientos]]:

| Código | Nombre del Caso de Uso del Negocio | Actor(es) Principal(es) | Requerimientos Trazados | Actividades AS-IS Cubiertas |
|:---:|:---|:---|:---:|:---:|
| **CUN-01** | **Despachar Pedidos desde Centro de Distribución** | Supervisor de Zona de Despacho *(Worker)*<br/>Socio Logístico *(Actor)* | **RF-01, RF-02, RF-03, RF-04** | `ACT-01`, `ACT-02`, `ACT-03`, `ACT-04`, `ACT-05` |
| **CUN-02** | **Trasladar Pedidos hacia Destino Nacional** | Socio Logístico *(Actor)* | **RF-05** | `ACT-06`, `ACT-07` |
| **CUN-03** | **Entregar Pedido en Domicilio** | Socio Logístico *(Actor)*<br/>Consultora *(Actor)*<br/>Familiar Autorizado *(Actor)* | **RF-06, RF-07** | `ACT-08`, `ACT-09` |
| **CUN-04** | **Gestionar Entrega Fallida y Retorno por Logística Inversa** | Socio Logístico *(Actor)* | **RF-08, RF-09** | `ACT-10`, `ACT-11` |
| **CUN-05** | **Consultar Trazabilidad y Situación del Pedido** | Consultora *(Actor)*<br/>Agente de Servicio al Cliente *(Worker)* | **RF-11, RF-12** | `ACT-13`, `ACT-14` |

*(Nota de trazabilidad: `RF-10` corresponde a la actividad transversal de actualización `ACT-12`, que alimenta los datos consultados en `CUN-05`).*

---

## 4. Diagramas de Casos de Uso del Negocio (CUN)

### 4.1. Diagrama General en Mermaid

```mermaid
flowchart TD
    %% Actores del Negocio (Externos)
    subgraph ACTORES_EXTERNOS ["«Business Actors» (Actores del Negocio)"]
        direction TB
        CONS["👤 Consultora / Consultor<br/>«Business Actor»"]
        FAM["👤 Familiar Autorizado<br/>«Business Actor»"]
        SOCIO["🚚 Socio Logístico / Transportista<br/>«Business Actor»"]
    end

    %% Trabajadores del Negocio (Internos)
    subgraph TRABAJADORES_INTERNOS ["«Business Workers» (Trabajadores del Negocio)"]
        direction TB
        SUP["👔 Supervisor de Zona de Despacho<br/>«Business Worker»"]
        AG["🎧 Agente de Servicio al Cliente<br/>«Business Worker»"]
        JEF["📋 Jefatura de Distribución<br/>«Business Worker»"]
    end

    %% Frontera de Casos de Uso del Negocio
    subgraph FRONTERA_NEGOCIO ["Sistema de Negocio: Cadena de Distribución y Trazabilidad (Yanbal Perú)"]
        direction TB

        CUN01(["«Business Use Case»<br/><b>CUN-01</b><br/>Despachar Pedidos desde<br/>Centro de Distribución"])
        CUN02(["«Business Use Case»<br/><b>CUN-02</b><br/>Trasladar Pedidos hacia<br/>Destino Nacional"])
        CUN03(["«Business Use Case»<br/><b>CUN-03</b><br/>Entregar Pedido<br/>en Domicilio"])
        CUN04(["«Business Use Case»<br/><b>CUN-04</b><br/>Gestionar Entrega Fallida y<br/>Retorno por Logística Inversa"])
        CUN05(["«Business Use Case»<br/><b>CUN-05</b><br/>Consultar Trazabilidad y<br/>Situación del Pedido"])
    end

    %% Asociaciones CUN-01
    SUP --- CUN01
    SOCIO --- CUN01

    %% Asociaciones CUN-02
    SOCIO --- CUN02

    %% Asociaciones CUN-03 (Camino Principal de Entrega)
    SOCIO --- CUN03
    CONS --- CUN03
    FAM --- CUN03

    %% Asociaciones CUN-04 (Camino Alternativo de Contingencia)
    SOCIO --- CUN04

    %% Asociaciones CUN-05 (Consulta de Seguimiento)
    CONS --- CUN05
    AG --- CUN05
```

---

### 4.2. Diagrama General en PlantUML (Estándar Formal UML / RUP)

> **Archivos fuente `.puml` disponibles en la carpeta:**  
> - 📄 Diagrama Maestro: [`CUN_Diagrama_General.puml`](file:///c:/Users/joses/Obsidian/arq-emp%20AS-IS/Diagrama%20de%20Casos%20de%20Uso%20del%20Negocio%20(CUN)/CUN_Diagrama_General.puml)  
> - 📄 Vista Despacho y Transporte: [`CUN_Paquete_Despacho_Transporte.puml`](file:///c:/Users/joses/Obsidian/arq-emp%20AS-IS/Diagrama%20de%20Casos%20de%20Uso%20del%20Negocio%20(CUN)/CUN_Paquete_Despacho_Transporte.puml)  
> - 📄 Vista Entrega y Logística Inversa: [`CUN_Paquete_Entrega_LogisticaInversa.puml`](file:///c:/Users/joses/Obsidian/arq-emp%20AS-IS/Diagrama%20de%20Casos%20de%20Uso%20del%20Negocio%20(CUN)/CUN_Paquete_Entrega_LogisticaInversa.puml)  
> - 📄 Vista Consulta de Trazabilidad: [`CUN_Paquete_Consulta.puml`](file:///c:/Users/joses/Obsidian/arq-emp%20AS-IS/Diagrama%20de%20Casos%20de%20Uso%20del%20Negocio%20(CUN)/CUN_Paquete_Consulta.puml)

```plantuml
@startuml CUN_Diagrama_General_Yanbal
!theme plain
skinparam shadowing false
skinparam roundcorner 10
skinparam defaultFontName "Arial"
skinparam defaultFontSize 12

' Estilos y Paleta de Colores
skinparam package {
    BackgroundColor #FAFAFA
    BorderColor #9E9E9E
    FontColor #212121
    FontStyle bold
}

skinparam actor {
    BackgroundColor<<Business Actor>> #E3F2FD
    BorderColor<<Business Actor>> #1976D2
    FontColor<<Business Actor>> #0D47A1
    
    BackgroundColor<<Business Worker>> #FFF3E0
    BorderColor<<Business Worker>> #F57C00
    FontColor<<Business Worker>> #E65100
}

skinparam usecase {
    BackgroundColor #FFFFFF
    BorderColor #424242
    ArrowColor #37474F
    FontColor #212121
}

skinparam note {
    BackgroundColor #FFFDE7
    BorderColor #FBC02D
    FontColor #212121
}

left to right direction

header Universidad Tecnológica del Perú (UTP) - Diseño e Implementación de Arquitectura Empresarial
title Diagrama de Casos de Uso del Negocio (CUN) - Estado Actual (AS-IS)\nProceso: Despacho -> Transporte -> Entrega / Retorno -> Consulta (Yanbal Perú)

' =======================================================
' ACTORES DEL NEGOCIO (EXTERNOS)
' =======================================================
package "Actores del Negocio («Business Actors»)" as PkgActores {
    actor "Consultora / Consultor\n(Cliente Primario)" as ActorConsultora <<Business Actor>>
    actor "Familiar Autorizado\n(Receptor en Domicilio)" as ActorFamiliar <<Business Actor>>
    actor "Socio Logístico / Transportista\n(Proveedor Contratado)" as ActorSocio <<Business Actor>>
}

' =======================================================
' TRABAJADORES DEL NEGOCIO (INTERNOS)
' =======================================================
package "Trabajadores del Negocio («Business Workers»)" as PkgTrabajadores {
    actor "Supervisor de Zona de Despacho\n(Operador en CD)" as WorkerSupervisor <<Business Worker>>
    actor "Agente de Servicio al Cliente\n(Atención en Salesforce)" as WorkerAgente <<Business Worker>>
    actor "Jefatura de Distribución\n(Gobierno y Lead Times)" as WorkerJefatura <<Business Worker>>
}

' =======================================================
' FRONTERA DEL SISTEMA DEL NEGOCIO (5 CUN)
' =======================================================
rectangle "Frontera del Negocio: Cadena de Distribución y Trazabilidad (Yanbal Perú)" as SistemaNegocio {
    
    usecase "<b>CUN-01</b>\nDespachar Pedidos desde\nCentro de Distribución\n---\n..Trazabilidad..\nRF-01, RF-02, RF-03, RF-04\nACT-01 a ACT-05\n..Estado..\nDespachado" as CUN01
    
    usecase "<b>CUN-02</b>\nTrasladar Pedidos hacia\nDestino Nacional\n---\n..Trazabilidad..\nRF-05\nACT-06, ACT-07\n..Estado..\nEn Ruta (24 Deptos)" as CUN02
    
    usecase "<b>CUN-03</b>\nEntregar Pedido\nen Domicilio\n---\n..Trazabilidad..\nRF-06, RF-07\nACT-08, ACT-09\n..Estado..\nEntregado" as CUN03
    
    usecase "<b>CUN-04</b>\nGestionar Entrega Fallida\ny Retorno por Logística Inversa\n---\n..Trazabilidad..\nRF-08, RF-09\nACT-10, ACT-11\n..Estado..\nEntrega Fallida" as CUN04
    
    usecase "<b>CUN-05</b>\nConsultar Trazabilidad\ny Situación del Pedido\n---\n..Trazabilidad..\nRF-11, RF-12\nACT-13, ACT-14\n..Búsqueda..\nN° Pedido / Cód. Consultora" as CUN05
}

' =======================================================
' RELACIONES DE ASOCIACIÓN (ACTORES <-> CUN)
' Asociaciones estándar UML (sin direccionalidad de flujo)
' =======================================================
WorkerSupervisor -- CUN01
ActorSocio -- CUN01

ActorSocio -- CUN02

ActorSocio -- CUN03
ActorConsultora -- CUN03
ActorFamiliar -- CUN03

ActorSocio -- CUN04

ActorConsultora -- CUN05
WorkerAgente -- CUN05

' Notas de Reglas y Delimitación
note bottom of CUN04
  <b>Responsable Único en Campo:</b>
  El Socio Logístico registra la incidencia
  (retraso, pérdida o daño) y formaliza
  el inicio del retorno (RF-09 / ACT-11).
  Concluye sin incluir recepción física en CD.
end note

note right of CUN03
  <b>Caminos Alternativos desde 'En Ruta':</b>
  - CUN-03: Entrega exitosa en destino.
  - CUN-04: Entrega fallida e inicio de retorno.
  (Modelados sin relación <<extend>>).
end note

note bottom of CUN05
  <b>Actividad Transversal (ACT-12 / RF-10):</b>
  La actualización de estados opera en segundo plano
  con desfase AS-IS de hasta 2h. Se constata la
  necesidad derivada de visibilizar al receptor (RF-12).
end note

footer Caso de Estudio: Yanbal Perú | Modelo de Casos de Uso del Negocio (APF1)

@enduml
```

---

## 5. Justificación Metodológica de las Correcciones

1. **Eliminación de la sincronización como CUN:**  
   La sincronización de bases de datos (`ACT-12` / `RF-10`) es una tarea técnica automatizada de integración, no un proceso donde un actor humano del negocio reciba un servicio directo. Se mantiene en la matriz como requerimiento funcional del sistema, pero se excluye de la capa de casos de uso del negocio.
2. **Eliminación de la relación `<<extend>>` entre CUN-03 y CUN-04:**  
   La entrega física en domicilio (`CUN-03`) y la contingencia por logística inversa (`CUN-04`) representan **dos caminos alternativos directos** del ciclo de transporte desde el estado *"En Ruta"*. Si la entrega se concreta, se ejecuta `CUN-03`; si ocurre una incidencia tipificada (retraso, pérdida o daño), se ejecuta directamente `CUN-04`.
3. **Actor único en CUN-04:**  
   En estricta consonancia con `ACT-10`, `ACT-11`, `RF-08` y `RF-09`, el responsable de tipificar la entrega fallida y ejecutar el retorno material hacia el Centro de Distribución es exclusivamente el **Socio Logístico / Transportista**.
