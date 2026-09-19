# Diagrama General de Casos de Uso del Negocio (CUN) — Y-Trace Yanbal

> **Archivo:** `diagrama.md`  
> **Carpeta:** `detalles/Diagrama de Casos de Uso del Negocio (CUN)`  
> **Proyecto:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Metodología:** Rational Unified Process (RUP) — Modelado del Negocio (Sesión 4 UTP / Arquitectura Empresarial)  
> **Estado:** [CONSOLIDADO OFICIAL — ALCANCE B2B PUNTO A PUNTO]  
>  
> 🔗 **Documentos Vinculados:**  
> - [[00_INDICE_Y_MODELO_GENERAL_CUN]] — Marco conceptual y matriz de participación  
> - [[01_ACTORES_DEL_NEGOCIO]] — Caracterización detallada de actores  
> - [[07_MATRIZ_TRAZABILIDAD_CUN_VS_RF]] — Matriz CUN vs 28 Requerimientos Funcionales  

---

## 1. Diagrama de Casos de Uso del Negocio en Mermaid (Nativo para Obsidian)

Este diagrama representa la relación formal entre los **5 Actores del Negocio** y los **5 Casos de Uso del Negocio (CUN)** dentro de la frontera del negocio de distribución B2B de Yanbal:

```mermaid
flowchart LR
    %% Actores Operativos en CD y Ruta (Izquierda)
    subgraph ACTORES_IZQ ["Operación en CD y Carretera"]
        SUP["fa:fa-user-tie Supervisor de Distribución<br/>&lt;&lt;business actor&gt;&gt;"]
        SL["fa:fa-truck Socio Logístico / Conductor<br/>&lt;&lt;business actor&gt;&gt;"]
    end

    %% Frontera del Negocio
    subgraph FRONTERA ["Negocio: Cadena de Distribución y Trazabilidad B2B (Yanbal Perú)"]
        CUN01(["<b>CUN-01</b><br/>Despacho y Salida de Carga<br/>en Centro de Distribución<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN02(["<b>CUN-02</b><br/>Traslado y Monitoreo<br/>de Carga en Tránsito<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN03(["<b>CUN-03</b><br/>Cancelación Forzada<br/>del Seguimiento<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN04(["<b>CUN-04</b><br/>Entrega y Recepción de Carga<br/>en Punto de Destino<br/>&lt;&lt;business use case&gt;&gt;"])
        CUN05(["<b>CUN-05</b><br/>Consulta de Trazabilidad y<br/>Análisis de Rendimiento<br/>&lt;&lt;business use case&gt;&gt;"])
    end

    %% Actores de Destino, Jefatura y Soporte (Derecha)
    subgraph ACTORES_DER ["Destino, Control y Soporte"]
        PD["fa:fa-warehouse Punto de Destino / Agencia<br/>&lt;&lt;business actor&gt;&gt;"]
        JEF["fa:fa-chart-line Jefe de Distribución<br/>&lt;&lt;business actor&gt;&gt;"]
        SAC["fa:fa-headset Operador SAC / Soporte Logístico<br/>&lt;&lt;business actor&gt;&gt;"]
    end

    %% Asociaciones del Supervisor de Distribución
    SUP --- CUN01
    SUP --- CUN02
    SUP --- CUN03

    %% Asociaciones del Socio Logístico / Conductor
    SL --- CUN01
    SL --- CUN02
    SL --- CUN03
    SL --- CUN04

    %% Asociación del Punto de Destino
    CUN04 --- PD

    %% Asociaciones de Jefatura y Soporte
    CUN05 --- JEF
    CUN05 --- SAC
```

---

## 2. Mapa Conceptual de Interacción entre Procesos de Negocio

El siguiente esquema refleja la articulación secuencial y de control de los macro-procesos operacionales de la distribución B2B:

```
                    CADENA LOGÍSTICA Y-TRACE
                               │
       ┌───────────────────────┼───────────────────────┐
       │                       │                       │
       ▼                       ▼                       ▼
    CUN-01                  CUN-02                  CUN-04
Despacho y Salida     Traslado y Monitoreo     Entrega y Recepción
       │                       │                       │
       │                       │                       ▼
       │                       │                    CUN-05
       │                       │                 Trazabilidad
       │                       │                 y Rendimiento
       │                       │
       │                       └───────────┐
       │                                   ▼
       │                                CUN-03
       │                             Cancelación
       │                               Forzada
       │
       └───────────────────────────────────►
```

> [!NOTE]
> **Precisiones de Frontera y Flujo:**
> - El flujo ordinario (*happy path*) avanza linealmente de **CUN-01** (Salida) $\rightarrow$ **CUN-02** (Traslado) $\rightarrow$ **CUN-04** (Entrega).
> - Al culminar las entregas, la información histórica alimenta **CUN-05** para consulta analítica y evaluación de rendimiento.
> - Si se suscita una contingencia externa o siniestro insalvable durante el traslado (o antes de la salida física), el proceso se desvía formalmente hacia **CUN-03 (Cancelación Forzada del Seguimiento)** para el cierre formal, administrativo y técnico.

---

## 3. Código PlantUML Oficial (Para Importar o Dibujar en Visual Paradigm)

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

    usecase "<b>CUN-02</b>\nTraslado y Monitoreo\nde Carga en Tránsito" as CUN02 <<business use case>>

    usecase "<b>CUN-03</b>\nCancelación Forzada\ndel Seguimiento" as CUN03 <<business use case>>

    usecase "<b>CUN-04</b>\nEntrega y Recepción de Carga\nen Punto de Destino" as CUN04 <<business use case>>

    usecase "<b>CUN-05</b>\nConsulta de Trazabilidad y\nAnálisis de Rendimiento" as CUN05 <<business use case>>
}

' ==========================================
' ASOCIACIONES DEL NEGOCIO (Líneas sólidas)
' ==========================================

' Conexiones del Supervisor de Distribución
SUP -- CUN01
SUP -- CUN02
SUP -- CUN03

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

## 4. Guía de Uso en Visual Paradigm

1. **En Visual Paradigm Community Edition:**
   - Abre o crea un diagrama de tipo **Use Case Diagram**.
   - Si se dibuja a mano:
     - Crea los **5 actores del negocio** y asígnales el estereotipo `business actor`.
     - Crea el contenedor **System Boundary** con el nombre: `Negocio: Cadena de Distribución y Trazabilidad B2B (Yanbal Perú)`.
     - Dibuja los **5 óvalos** dentro del contenedor y asígnales el estereotipo `business use case`.
     - Traza líneas de **Association** (sólidas, sin flechas direccionales ni includes/extends) conectando a cada actor con sus respectivos CUNs según la matriz oficial.
2. **Exportar:**
   - **Project** $\rightarrow$ **Export** $\rightarrow$ **Active Diagram as Image (PNG/SVG)** a 300 DPI.
