# Diagrama General de Casos de Uso del Negocio (CUN) — Y-Trace Yanbal

> [!WARNING]
> **Nota de Control de Versiones (H-14):** Este archivo `diagrama.md` actúa como una copia de referencia visual del modelo. Cualquier modificación en los requerimientos, actores o casos de uso debe realizarse prioritariamente en los documentos consolidados (`00_INDICE_Y_MODELO_GENERAL_CUN.md`, `01_ACTORES_DEL_NEGOCIO.md`, etc.) para evitar desincronizaciones.

> **Archivo:** `diagrama.md`  
> **Proyecto:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Metodología:** RUP (Sesión 4 UTP / Arquitectura Empresarial)  
> **Contenido:** Código PlantUML optimizado para Visual Paradigm + Diagrama interactivo nativo para Obsidian.

---

## 1. Visualización Nativa en Obsidian (Mermaid)

Este diagrama se renderiza de forma nativa e interactiva directamente en Obsidian sin necesidad de plugins externos:

```mermaid
flowchart LR
    %% Actores del Negocio
    subgraph ACTORES_IZQ ["Operación en CD y Ruta"]
        SUP["fa:fa-user-tie Supervisor de Distribución<br/>&lt;&lt;business actor&gt;&gt;"]
        SL["fa:fa-truck Socio Logístico / Conductor<br/>&lt;&lt;business actor&gt;&gt;"]
    end

    %% Límite del Negocio
    subgraph FRONTERA ["Negocio: Cadena de Distribución y Trazabilidad B2B (Yanbal Perú)"]
        CUN01(["<b>CUN-01</b><br/>Despacho y Salida de Carga<br/>en Centro de Distribución<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN02(["<b>CUN-02</b><br/>Traslado Interprovincial y<br/>Monitoreo de Carga en Tránsito<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN03(["<b>CUN-03</b><br/>Gestión de Incidencias y<br/>Contingencias Viales en Ruta<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN04(["<b>CUN-04</b><br/>Entrega y Recepción de Carga<br/>en Punto de Destino<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN05(["<b>CUN-05</b><br/>Auditoría de Trazabilidad y<br/>Rendimiento de Distribución<br/>&lt;&lt;business use case&gt;&gt;"])
    end

    %% Actores de Destino y Jefatura
    subgraph ACTORES_DER ["Destino, Control y Soporte"]
        PD["fa:fa-warehouse Punto de Destino / Agencia<br/>&lt;&lt;business actor&gt;&gt;"]
        JEF["fa:fa-chart-line Jefe de Distribución<br/>&lt;&lt;business actor&gt;&gt;"]
        SAC["fa:fa-headset Operador SAC / Soporte Logístico<br/>&lt;&lt;business actor&gt;&gt;"]
    end

    %% Conexiones
    SUP --- CUN01
    SUP --- CUN02
    SUP --- CUN03
    SUP --- CUN04
    SUP --- CUN05

    SL --- CUN01
    SL --- CUN02
    SL --- CUN03
    SL --- CUN04

    CUN04 --- PD

    CUN05 --- JEF
    CUN05 --- SAC
```

---

## 2. Código PlantUML Oficial (Para Importar o Dibujar en Visual Paradigm)

Copia el siguiente bloque completo para importarlo en **Visual Paradigm** o en cualquier visor de PlantUML:

```plantuml
@startuml CUN_Distribucion_B2B_Yanbal_YTrace
title Diagrama de Casos de Uso del Negocio (CUN) - Distribución B2B Yanbal Perú
left to right direction

' ==========================================
' CONFIGURACIÓN DE ESTILOS RUP
' ==========================================
skinparam shadowing false
skinparam packageStyle rectangle
skinparam defaultFontName Arial
skinparam defaultFontSize 12

skinparam rectangle {
    BackgroundColor #F8FAFC
    BorderColor #1E293B
    BorderThickness 2
    FontStyle bold
    FontSize 13
}

skinparam usecase {
    BackgroundColor #EFF6FF
    BorderColor #1D4ED8
    BorderThickness 1.5
    FontSize 12
}

skinparam actor {
    BackgroundColor #F1F5F9
    BorderColor #334155
    BorderThickness 1.5
}

' ==========================================
' ACTORES DEL NEGOCIO (<<business actor>>)
' ==========================================
actor "Supervisor de Distribución" as SUP <<business actor>>
actor "Socio Logístico / Conductor" as SL <<business actor>>
actor "Punto de Destino / Agencia Receptora" as PD <<business actor>>
actor "Jefe de Distribución" as JEF <<business actor>>
actor "Operador SAC / Soporte Logístico" as SAC <<business actor>>

' ==========================================
' FRONTERA DEL NEGOCIO (Business Boundary)
' ==========================================
rectangle "Negocio: Cadena de Distribución y Trazabilidad B2B (Yanbal Perú)" {

    usecase "<b>CUN-01</b>\nDespacho y Salida de Carga\nen Centro de Distribución" as CUN01 <<business use case>>

    usecase "<b>CUN-02</b>\nTraslado Interprovincial y\nMonitoreo de Carga en Tránsito" as CUN02 <<business use case>>

    usecase "<b>CUN-03</b>\nGestión de Incidencias y\nContingencias Viales en Ruta" as CUN03 <<business use case>>

    usecase "<b>CUN-04</b>\nEntrega y Recepción de Carga\nen Punto de Destino" as CUN04 <<business use case>>

    usecase "<b>CUN-05</b>\nAuditoría de Trazabilidad y\nRendimiento de Distribución" as CUN05 <<business use case>>
}

' ==========================================
' ASOCIACIONES DEL NEGOCIO (Líneas sólidas)
' ==========================================

' Conexiones del Supervisor de Distribución
SUP -- CUN01
SUP -- CUN02
SUP -- CUN03
SUP -- CUN04

' Conexiones del Conductor / Socio Logístico
SL -- CUN01
SL -- CUN02
SL -- CUN03
SL -- CUN04

' Conexión de Recepción en Destino
CUN04 -- PD

' Conexiones de Jefatura y Soporte
CUN05 -- JEF
CUN05 -- SAC

@enduml
```

---

## 3. Guía de Uso en Visual Paradigm

1. **En Visual Paradigm Community Edition:**
   - Ve a **Tools** $\rightarrow$ **PlantUML** (o abre un **Use Case Diagram** en blanco).
   - Si lo dibujas a mano:
     - Crea los **5 actores** y asígnales el estereotipo `business actor`.
     - Crea el recuadro **System Boundary** con el nombre: `Negocio: Cadena de Distribución y Trazabilidad B2B (Yanbal Perú)`.
     - Dibuja los **5 óvalos** dentro y asígnales el estereotipo `business use case` (VP dibujará automáticamente la línea diagonal interior).
     - Une con líneas de **Association** (sólidas, sin flechas).
2. **Exportar:**
   - **Project** $\rightarrow$ **Export** $\rightarrow$ **Active Diagram as Image (PNG/SVG)** a 300 DPI para tu entrega.


