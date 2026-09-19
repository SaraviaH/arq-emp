# Diagrama de Actividades AS-IS: Flujo General de los Procesos de Negocio Actuales de Distribución

> **Ubicación:** `diagram de actividades/02_DIAGRAMA_ACTIVIDADES_AS_IS_GENERAL.md`  
> **Curso:** Diseño e Implementación de Arquitectura Empresarial (UTP) — Entregable APF1 (Semana 6 - 7)  
> **Proyecto:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Fuente Operativa:** Entrevista oficial al **Ing. Joao Condorpusa Mendoza** (Encargado del Área de Distribución de Yanbal Perú), minutos 20:18 a 21:35 (Logística inversa, retornos y reposición de despachos).  
> **Estándar de Modelado:** UML 2.5 / Metodología RUP (Diagrama de Actividades General con Particiones / *Swimlanes*).

---

## 1. Alcance del Flujo General del Negocio Actual (AS-IS)

Mientras que el diagrama del problema crítico focaliza la latencia de actualización del tracking y su impacto en SAC, este **Diagrama de Actividades General** describe el proceso completo del negocio de distribución física de Yanbal de extremo a extremo, reflejando fielmente la operación actual descrita en la entrevista:

```mermaid
flowchart TD
    P1["1. Preparación y Despacho en Andén<br/>(CD Lurín / Supervisor de Distribución)"] --> P2["2. Traslado Troncal Nacional<br/>(Socio Logístico / Conductor)"]
    
    P2 --> DEC1{"¿Ocurre avería grave o<br/>siniestro en ruta?"}
    
    DEC1 -- "SÍ (Interrupción)" --> R1["Retorno a CD Lurín por Logística Inversa<br/>(Evaluación patrimonial y seguros)"]
    R1 --> R2["Cierre de despacho interrumpido y<br/>gestión de nuevo despacho independiente"]
    
    DEC1 -- "NO (Arribo)" --> P3["3. Arribo e Inspección Física<br/>(Punto de Destino / Agencia Receptora)"]
    
    P3 --> DEC2{"¿Recepción física<br/>conforme?"}
    
    DEC2 -- "SÍ" --> P4["4. Conformidad y Cierre Operativo<br/>(Liquidación y archivo documental)"]
    
    DEC2 -- "NO (Rechazo)" --> R3["Constancia de rechazo y retorno a CD Lurín<br/>(Logística inversa / Cierre de despacho fallido)"]
```

> [!NOTE]
> **Rigor Metodológico del Proceso General:**  
> Este diagrama representa **las actividades del negocio que ocurren actualmente en la realidad operativa**. Las actividades de preparación física (picking/consolidación) en CD Lurín, verificación física en andén, transporte troncal interprovincial, gestión de contingencias viales con retorno a almacén (logística inversa), inspección en destino y cierre administrativo forman parte del proceso real actual del negocio. No se introducen requerimientos futuros (RF001–RF028, RNF, Y-Trace ni Bus corporativo), los cuales corresponden estrictamente a la arquitectura **TO-BE**.

---

## 2. Particiones (*Swimlanes*) del Proceso General

Las responsabilidades operativas se estructuran en **5 Carriles (Particiones)** claramente delimitados:

1. **Centro de Distribución (Lurín):** Almacén central de Yanbal encargado de la recepción de órdenes comerciales, preparación física (*picking*), consolidación de pedidos por volumetría en 8 tipos de cajas, zonificación departamental y recepción/evaluación de retornos por logística inversa (peritaje de calidad, gestión de seguros y reingreso de mercadería).
2. **Supervisor de Distribución:** Responsable de verificar la carga física consolidada contra las guías de remisión y el manifiesto, emitir la documentación oficial de despacho y formalizar el traspaso de custodia al transportista.
3. **Socio Logístico / Conductor:** Transportista asociado a cargo del traslado físico troncal hacia los 24 departamentos (modalidades terrestre, bimodal o aérea), reporte y gestión de averías mecánicas o siniestros en carretera, entrega física en el punto de destino, y ejecución del retorno vehicular bajo logística inversa cuando se produzca una interrupción en ruta o un rechazo en destino.
4. **Punto de Destino / Agencia Receptora:** Agencia departamental o receptor comercial autorizado en destino. Recepciona el vehículo, descarga los paquetes, inspecciona la integridad física exterior de bultos y precintos contra las guías de remisión, y emite el dictamen de conformidad física o acta de rechazo (entrega no conforme).
5. **Administración y Cierre Operativo:** Área administrativa responsable del registro de confirmaciones en sistemas corporativos (SAP/ERP), control documental de guías firmadas y conciliación/liquidación del servicio de transporte.

---

## 3. Diagrama Visual Interactivo en Obsidian (Mermaid)

```mermaid
flowchart TD
    %% Estilos de Nodos
    classDef inicioFin fill:#1E293B,stroke:#0F172A,stroke-width:2px,color:#fff;
    classDef normal fill:#F8FAFC,stroke:#334155,stroke-width:1.5px,color:#0F172A;
    classDef decision fill:#FEF3C7,stroke:#D97706,stroke-width:1.5px,color:#78350F;
    classDef contingencia fill:#FEE2E2,stroke:#DC2626,stroke-width:2px,color:#7F1D1D;
    classDef exito fill:#ECFDF5,stroke:#059669,stroke-width:1.5px,color:#065F46;

    %% Inicio
    INI(( )):::inicioFin --> A1["Generar pedido en plataforma comercial"]:::normal

    %% 1. Centro de Distribución (Lurín)
    subgraph CD_LURIN ["1. Centro de Distribución (Lurín)"]
        A1 --> A2["Elaborar picking y consolidar cajas por volumetría"]:::normal
        A2 --> A3["Zonificar despacho para los 24 departamentos"]:::normal
        A3 --> A4["Ubicar carga consolidada en andén de salida"]:::normal
        
        %% Gestión de Retornos por Incidencia en Ruta
        R_RUTA1["Almacén recepciona unidad y carga retornada"]:::contingencia
        R_RUTA2["Inspeccionar carga (evaluación de calidad y seguridad patrimonial)"]:::contingencia
        DEC_CAL{"¿Mercadería en buen estado<br/>apta para retorno?"}:::decision
        R_CAL_SI["Reingresar productos aptos a inventario disponible"]:::normal
        R_CAL_NO["Gestionar peritaje de siniestro o seguro patrimonial"]:::contingencia
        R_CIERRE_INC["Concluir y cerrar despacho original interrumpido"]:::contingencia
        R_NUEVO_PED["Programar orden de reposición como nuevo despacho independiente"]:::normal
        
        %% Gestión de Retornos por Rechazo en Destino
        R_DEST1["Almacén recepciona mercadería rechazada en destino"]:::contingencia
        R_DEST2["Inspeccionar bultos devueltos y registrar reingreso o baja"]:::contingencia
        R_CIERRE_REJ["Concluir y cerrar despacho original por entrega no conforme"]:::contingencia
    end

    %% 2. Supervisor de Distribución
    subgraph SUPERVISOR ["2. Supervisor de Distribución"]
        A4 --> B1["Emitir documentación de despacho (Guías y Manifiesto)"]:::normal
        B1 --> B2["Verificar carga física contra documentación"]:::normal
        B2 --> B3["Formalizar entrega de custodia al transportista"]:::normal
    end

    %% 3. Socio Logístico / Conductor
    subgraph CONDUCTOR ["3. Socio Logístico / Conductor"]
        B3 --> C1["Cargar unidad de transporte e iniciar ruta"]:::normal
        C1 --> C2["Traslado troncal hacia destino (24h a 7 días según región)"]:::normal
        C2 --> DEC_RUTA{"¿Ocurre avería mecánica grave<br/>o siniestro vial en ruta?"}:::decision
        
        %% Rama 1: Incidencia Grave en Ruta
        DEC_RUTA -- "SÍ" --> C_INC1["Reportar contingencia a la base del socio logístico"]:::contingencia
        C_INC1 --> C_INC2["Determinar imposibilidad operativa de continuar el viaje"]:::contingencia
        C_INC2 --> C_RET_RUTA["Iniciar retorno de la unidad a CD Lurín (Logística Inversa)"]:::contingencia
        
        %% Rama Normal: Arribo
        DEC_RUTA -- "NO" --> C3["Arribar al punto de destino regional o agencia"]:::normal
        C3 --> C4["Descargar bultos y presentar documentación de entrega"]:::normal
        
        %% Reacción al Rechazo en Destino
        C_REJ1["Retener bultos rechazados y firmar acta de no conformidad"]:::contingencia
        C_REJ1 --> C_RET_DEST["Iniciar retorno de la carga a CD Lurín (Logística Inversa)"]:::contingencia
        
        %% Continuación tras Entrega Conforme
        C5["Custodiar guía de remisión firmada y sellada"]:::normal
    end

    %% Enlace Retorno Ruta -> Almacén
    C_RET_RUTA --> R_RUTA1
    R_RUTA1 --> R_RUTA2
    R_RUTA2 --> DEC_CAL
    DEC_CAL -- "SÍ" --> R_CAL_SI
    DEC_CAL -- "NO" --> R_CAL_NO
    R_CAL_SI --> R_CIERRE_INC
    R_CAL_NO --> R_CIERRE_INC
    R_CIERRE_INC --> R_NUEVO_PED
    R_NUEVO_PED --> FIN_INC(( )):::inicioFin

    %% 4. Punto de Destino / Agencia Receptora
    subgraph DESTINO ["4. Punto de Destino / Agencia Receptora"]
        C4 --> D1["Inspeccionar integridad exterior de bultos y precintos"]:::normal
        D1 --> DEC_DEST{"¿Recepción física conforme<br/>(sin bultos dañados ni faltantes)?"}:::decision
        
        %% Rama 2: Rechazo en Destino
        DEC_DEST -- "NO" --> D_REJ["Dictaminar rechazo y asentar observaciones en guía/acta"]:::contingencia
        
        %% Rama 3: Recepción Conforme
        DEC_DEST -- "SÍ" --> D2["Firmar y sellar guía de remisión en señal de conformidad"]:::exito
        D2 --> D3["Ingresar bultos conformes al almacén de destino"]:::exito
    end

    %% Enlace Rechazo Destino -> Conductor y Retorno Almacén
    D_REJ --> C_REJ1
    C_RET_DEST --> R_DEST1
    R_DEST1 --> R_DEST2
    R_DEST2 --> R_CIERRE_REJ
    R_CIERRE_REJ --> FIN_REJ(( )):::inicioFin

    %% Enlace Conforme -> Conductor y Administración
    D3 --> C5

    %% 5. Administración y Cierre Operativo
    subgraph CIERRE ["5. Administración y Cierre Operativo"]
        C5 --> E1["Registrar confirmación de entrega en sistemas corporativos"]:::exito
        E1 --> E2["Realizar liquidación y conciliación administrativa del transporte"]:::exito
        E2 --> FIN_EXITO((( ))):::inicioFin
    end
```

---

## 4. Código PlantUML Oficial con Particiones (*Swimlanes*)

```plantuml
@startuml Diagrama_Actividades_AS_IS_Yanbal_General
title Proceso AS-IS General: Cadena de Distribución Física, Incidencias en Ruta y Rechazo en Destino
skinparam shadowing false
skinparam defaultFontName Arial
skinparam defaultFontSize 11

skinparam activity {
    BackgroundColor #F8FAFC
    BorderColor #1E293B
    BorderThickness 1.5
    FontSize 11
}

skinparam partition {
    BackgroundColor #FFFFFF
    BorderColor #475569
    FontColor #0F172A
    FontStyle bold
    FontSize 12
}

|#EFF6FF|Centro de Distribución (Lurín)|
start
:Generar pedido en la plataforma comercial;
:Elaborar picking y consolidar cajas en almacén;
note right: Asignación por volumetría en 8 tipos de cajas.
:Zonificar despacho para los 24 departamentos;
:Ubicar carga consolidada en andén de salida;

|#F1F5F9|Supervisor de Distribución|
:Emitir documentación de despacho (Guías y Manifiesto);
:Verificar carga física contra documentación;
:Formalizar entrega de custodia al transportista;

|#ECFDF5|Socio Logístico / Conductor|
:Cargar unidad de transporte e iniciar ruta;
:Traslado troncal hacia destino (modalidad terrestre, bimodal o aérea);
note right
  Lead times según destino:
  24h en Lima Metropolitana,
  hasta 7 días en provincias.
end note

if (¿Ocurre avería mecánica grave o siniestro en ruta?) then (sí)
  :Reportar contingencia a la base del socio logístico;
  :Determinar imposibilidad de continuar el viaje;
  :Iniciar retorno de la unidad a CD Lurín (Logística Inversa);

  |#EFF6FF|Centro de Distribución (Lurín)|
  :Almacén recepciona unidad y carga retornada;
  :Inspeccionar carga (evaluación de calidad y seguridad patrimonial);
  if (¿Mercadería en buen estado apta para retorno?) then (sí)
    :Reingresar productos aptos a inventario disponible;
  else (no / siniestro)
    :Gestionar peritaje de siniestro o seguro patrimonial;
  endif
  :Concluir y cerrar despacho original interrumpido;
  :Programar orden de reposición como nuevo despacho independiente;
  stop
else (no)
endif

|#ECFDF5|Socio Logístico / Conductor|
:Arribar al punto de destino regional o agencia;
:Descargar bultos y presentar documentación de entrega;

|#FEF3C7|Punto de Destino / Agencia Receptora|
:Inspeccionar integridad exterior de bultos y precintos;

if (¿Recepción física conforme?) then (sí)
  :Firmar y sellar guía de remisión en señal de conformidad;
  :Ingresar bultos conformes al almacén de destino;

  |#ECFDF5|Socio Logístico / Conductor|
  :Custodiar guía de remisión firmada y sellada;

  |#FFF1F2|Administración y Cierre Operativo|
  :Registrar confirmación de entrega en sistemas corporativos;
  :Realizar liquidación y conciliación administrativa del transporte;
  stop
else (no / rechazo)
  :Dictaminar rechazo y asentar observaciones en guía o acta;

  |#ECFDF5|Socio Logístico / Conductor|
  :Retener bultos rechazados y firmar constancia de no conformidad;
  :Iniciar retorno de la carga a CD Lurín (Logística Inversa);

  |#EFF6FF|Centro de Distribución (Lurín)|
  :Almacén recepciona mercadería rechazada en destino;
  :Inspeccionar bultos devueltos y registrar reingreso o baja;
  :Concluir y cerrar despacho original por entrega no conforme;
  stop
endif
@enduml
```

---

## 5. Secuencia y Relación de Sistemas Corporativos en el Proceso Actual

Para mantener la fidelidad a la operación actual descrita en la entrevista, los sistemas corporativos vigentes intervienen ordenadamente en sus respectivas fases:

| Etapa Operativa | Sistema / Componente de Soporte Actual | Función en el Negocio Real Vigente |
| :--- | :--- | :--- |
| **1. Generación Comercial** | Maya / SAP Commerce | Ingreso comercial de pedidos y transmisión de órdenes consolidadas a almacén. |
| **2. Preparación y Picking** | SPY (Gestor de Picking in-house) | Optimización del recorrido de preparación y cubicaje de pedidos en 8 tipos de cajas. |
| **3. Despacho y Custodia** | Centro de Distribución Lurín / Guías Físicas | Zonificación departamental, emisión de guías de remisión impresas y entrega de custodia. |
| **4. Transporte y Seguimiento** | Socio Logístico / Plataformas de Transporte (Drivin/ENSDY) | Desplazamiento interprovincial y registro de eventos de transporte con latencia de sincronización. |
| **5. Retornos y Calidad** | Almacén CD Lurín / Área de Calidad y Seguridad | Peritaje de mercadería retornada en logística inversa, control de mermas y activación de seguros. |
| **6. Cierre Operativo** | Administración / SAP R/3 | Conciliación de guías físicas suscritas y liquidación de servicios de transporte. |

---

## 6. Comparativa Estructural: Diagrama General vs. Diagrama del Problema Crítico

| Criterio | Diagrama de Actividades AS-IS General | Diagrama de Actividades AS-IS (Problema Crítico) |
| :--- | :--- | :--- |
| **Enfoque** | Proceso completo actual de distribución física, incluyendo la bifurcación de incidencias en ruta y rechazo en destino. | Zoom del proceso actual enfocado en el desfase de trazabilidad (~2 horas) y su impacto en SAC. |
| **Alcance** | Desde la preparación comercial en almacén hasta el cierre administrativo o la terminación por retorno. | Desde el despacho en andén, traslado y entrega física, hasta la consulta en SAC y actualización tardía. |
| **Duración del Ciclo** | Ciclo integral de distribución (días de tránsito interprovincial, logística inversa y liquidación). | Horas críticas del viaje y momento de entrega (ventana de 120 minutos de desactualización). |
| **Actores Clave** | Centro de Distribución (Lurín), Supervisor de Distribución, Socio Logístico / Conductor, Punto de Destino / Agencia Receptora, Administración y Cierre Operativo. | Supervisor de Distribución, Socio Logístico / Conductor, Punto de Destino / Agencia Receptora, Sistema de Tracking y Consulta, Solicitante / Destinatario, Operador SAC / Soporte Logístico. |
| **Rol en el APF1** | Cumple con el requisito de *«Flujo general de los procesos de negocio actuales»*. | Cumple con el requisito de *«Flujo enfocado en el problema crítico a resolver»*. |

---

## 7. Principios de Modelado y Rigor Metodológico del AS-IS

1. **Separación de Incidencia en Ruta vs. Rechazo en Destino:** Se delimita de forma independiente la contingencia en carretera (que interrumpe el tránsito y devuelve la unidad completa) de la no conformidad en destino (que ocurre tras el arribo y la inspección física de bultos).
2. **Independencia de Despachos y Terminación Operativa:** Un despacho interrumpido o rechazado concluye de forma definitiva en el almacén central (`stop`); cualquier reposición posterior constituye un pedido y despacho nuevo e independiente para salvaguardar el servicio sin encadenar viajes en el mismo flujo.
3. **Logística Inversa como Proceso Operativo Real:** Conforme a lo declarado por el Ing. Joao Condorpusa (minutos 20:18 a 21:35), la logística inversa representa el procedimiento operativo real vigente para retornar mercadería a CD Lurín ante averías, siniestros o rechazos.
4. **Ausencia Total de Elementos TO-BE:** El diagrama no incluye RF001–RF028, RNF, Y-Trace, Bus corporativo ni sincronizaciones automáticas. Refleja exclusivamente el proceso fáctico actual.
5. **Homogeneización de Carriles (*Swimlanes*):** Los nombres de los carriles están alineados con los roles del modelo oficial empresarial, garantizando coherencia formal a lo largo de toda la documentación del proyecto.
