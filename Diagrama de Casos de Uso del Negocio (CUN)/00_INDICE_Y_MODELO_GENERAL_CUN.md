# Modelo General de Casos de Uso del Negocio (CUN): Y-Trace

> **Carpeta:** `detalles/Diagrama de Casos de Uso del Negocio (CUN)`  
> **Documento:** `00_INDICE_Y_MODELO_GENERAL_CUN.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Marco Metodológico:** Rational Unified Process (RUP) — Modelado del Negocio (Sesión 4 UTP / Arquitectura Empresarial)  
> **Estado:** [CONSOLIDADO OFICIAL — ALCANCE B2B PUNTO A PUNTO]  
>  
> 🔗 **Documentos de Base del Proyecto:**  
> - [[01_MATRIZ_DE_REQUERIMIENTOS]] — Matriz maestra unificada (28 RF activos)  
> - [[02_REQUERIMIENTOS_FUNCIONALES]] — Especificación técnica de RFs (RF001 a RF028)  
> - [[03_REQUERIMIENTOS_NO_FUNCIONALES]] — Especificación técnica de RNFs (RNF001 a RNF022)  
> - [[04_RESUMEN_OPERATIVO_Y_TRAZABILIDAD_REQUERIMIENTOS]] — Resumen operativo extremo a extremo  

---

## 1. Introducción y Marco Teórico RUP

En la metodología **Rational Unified Process (RUP)** y bajo los principios de **Arquitectura Empresarial**, el **Modelado del Negocio** describe cómo una organización estructura sus procesos operacionales para brindar valor a sus clientes, socios y áreas usuarias.

Un **Caso de Uso del Negocio (CUN)** representa un proceso de negocio integral de principio a fin, ejecutado por personas, roles y recursos dentro de la empresa, que genera un resultado de valor observable para un actor del negocio.

### 1.1 Demarcación Rigurosa entre Niveles de Modelado

Es fundamental distinguir los tres niveles de abstracción del proyecto para evitar errores conceptuales habituales:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   NIVELES DE MODELADO DE SISTEMAS                               │
├──────────────────────────────┬──────────────────────────────────┬───────────────────────────────┤
│ Casos de Uso del Negocio     │ Casos de Uso del Sistema         │ Requerimientos Funcionales    │
│ (CUN - Macro-Procesos)       │ (CUS - Interacción con Software) │ (RF - Especificación Técnica) │
├──────────────────────────────┼──────────────────────────────────┼───────────────────────────────┤
│ • Nivel: Negocio / Proceso.  │ • Nivel: Frontera del Sistema.   │ • Nivel: Detalle de Software. │
│ • Foco: ¿Qué hace la empresa │ • Foco: ¿Cómo interactúa el      │ • Foco: ¿Qué capacidades de   │
│   para transportar la carga? │   usuario con las pantallas?     │   procesamiento tiene la app? │
│ • 5 CUN identificados.       │ • Se derivan de los CUN.         │ • 28 RF activos en Y-Trace.   │
│ • No depende de la UI.       │ • Pantallas, botones, inputs.    │ • Dan soporte a los CUS/CUN.  │
└──────────────────────────────┴──────────────────────────────────┴───────────────────────────────┘
```

> [!IMPORTANT]
> **Reglas Metodológicas Clave:**
> 1. **No convertir los 28 RFs en CUNs:** Hacerlo generaría "CUNs botón" o "CUNs pantalla" (ej. "CUN Autenticarse", "CUN Capturar GPS"). En el negocio no existen esos procesos; esos son servicios computacionales que dan soporte a los verdaderos procesos logísticos.
> 2. **Frontera B2B Punto a Punto:** El alcance modela estrictamente la distribución completa entre puntos de distribución (Origen CD Lurín $\rightarrow$ Traslado $\rightarrow$ Punto de Destino / Agencias departamentales). Quedan excluidos los procesos de manufactura, picking en SPY, ruteo maestro en Driving y reparto minorista domiciliario B2C a consultoras o familiares.

---

## 2. Mapa General de Casos de Uso del Negocio

El modelo de procesos de distribución B2B de Yanbal Perú se articula a través de **5 Casos de Uso del Negocio** interconectados con **5 Actores del Negocio** (2 externos y 3 internos):

```mermaid
flowchart LR
    %% Subgrafo de Actores Externos
    subgraph ACTORES_EXTERNOS ["fa:fa-globe Actores Externos del Negocio"]
        SL["fa:fa-truck Socio Logístico / Conductor<br/>&lt;&lt;business actor&gt;&gt;"]
        PD["fa:fa-warehouse Punto de Destino / Agencia Receptora<br/>&lt;&lt;business actor&gt;&gt;"]
    end

    %% Subgrafo de Actores Internos
    subgraph ACTORES_INTERNOS ["fa:fa-building Actores Internos / Trabajadores del Negocio"]
        SUP["fa:fa-user-tie Supervisor de Distribución<br/>&lt;&lt;business actor&gt;&gt;"]
        JEF["fa:fa-chart-line Jefe de Distribución<br/>&lt;&lt;business actor&gt;&gt;"]
        SAC["fa:fa-headset Operador SAC / Soporte Logístico<br/>&lt;&lt;business actor&gt;&gt;"]
    end

    %% Subgrafo del Límite del Negocio
    subgraph FRONTERA_NEGOCIO ["Procesos de Negocio: Distribución y Trazabilidad B2B Yanbal Perú"]
        CUN01(["((CUN-01))<br/><b>Despacho y Salida de Carga<br/>en Centro de Distribución</b><br/>&lt;&lt;business use case&gt;&gt;"])
        CUN02(["((CUN-02))<br/><b>Traslado y Monitoreo<br/>de Carga en Tránsito</b><br/>&lt;&lt;business use case&gt;&gt;"])
        CUN03(["((CUN-03))<br/><b>Cancelación Forzada<br/>del Seguimiento</b><br/>&lt;&lt;business use case&gt;&gt;"])
        CUN04(["((CUN-04))<br/><b>Entrega y Recepción de Carga<br/>en Punto de Destino</b><br/>&lt;&lt;business use case&gt;&gt;"])
        CUN05(["((CUN-05))<br/><b>Consulta de Trazabilidad y<br/>Análisis de Rendimiento</b><br/>&lt;&lt;business use case&gt;&gt;"])
    end

    %% Asociaciones de Negocio CUN-01
    SUP --- CUN01
    SL --- CUN01

    %% Asociaciones de Negocio CUN-02
    SL --- CUN02
    SUP --- CUN02

    %% Asociaciones de Negocio CUN-03
    SL --- CUN03
    SUP --- CUN03

    %% Asociaciones de Negocio CUN-04
    SL --- CUN04
    PD --- CUN04

    %% Asociaciones de Negocio CUN-05
    JEF --- CUN05
    SAC --- CUN05
```

---

## 3. Catálogo Sintético de Actores y Casos de Uso

### 3.1 Actores del Negocio
| Identificador | Nombre del Actor | Clasificación | Rol Principal en la Cadena B2B |
| :--- | :--- | :---: | :--- |
| **ACT-NEG-01** | **Socio Logístico / Conductor** | Externo | Transportista tercero encargado de la recepción física en andén, custodia de la carga en ruta, comunicación externa de contingencias, entrega física y registro de cierre en la app móvil. |
| **ACT-NEG-02** | **Punto de Destino / Agencia Receptora** | Externo | Agencia comercial o almacén departamental que recepciona físicamente los bultos/pallets, inspecciona precintos y otorga la conformidad física en documento de remisión. |
| **ACT-NEG-03** | **Supervisor de Distribución** | Interno | Colaborador en CD Lurín que valida andenes, genera habilitaciones de viaje mediante códigos efímeros, monitorea la grilla operativa y ejecuta la cancelación forzada del seguimiento cuando una contingencia externa lo exige. |
| **ACT-NEG-04** | **Jefe de Distribución** | Interno | Responsable táctico/estratégico que evalúa el cumplimiento de SLAs (*Lead Time*, puntualidad), analiza comparativamente a los transportistas y genera reportes consolidados en Excel. |
| **ACT-NEG-05** | **Operador SAC / Soporte Logístico** | Interno | Personal de servicio al cliente que atiende consultas operacionales de áreas comerciales consultando la cronología histórica y coordenadas GPS de los despachos. |

### 3.2 Casos de Uso del Negocio (CUN)
| Código | Caso de Uso del Negocio | Objetivo de Valor del Negocio | Actores Asociados | Documento Detallado |
| :---: | :--- | :--- | :--- | :---: |
| **CUN-01** | **Despacho y Salida de Carga en Centro de Distribución** | Formalizar la verificación física, traspaso de custodia y habilitación de salida de la carga preparada en CD Lurín hacia el transportista. | Supervisor de Distribución, Socio Logístico / Conductor | [[02_CUN_01_DESPACHO_Y_SALIDA_CD]] |
| **CUN-02** | **Traslado y Monitoreo de Carga en Tránsito** | Garantizar el transporte físico seguro a nivel nacional con visibilidad continua del avance de las unidades de transporte en la consola interna de supervisión. | Socio Logístico / Conductor, Supervisor de Distribución | [[03_CUN_02_TRASLADO_Y_MONITOREO]] |
| **CUN-03** | **Cancelación Forzada del Seguimiento** | Concluir formal y técnicamente el seguimiento de un despacho en Y-Trace cuando una contingencia externa insalvable imposibilita culminar el traslado. | Socio Logístico / Conductor, Supervisor de Distribución | [[04_CUN_03_CANCELACION_FORZADA_SEGUIMIENTO]] |
| **CUN-04** | **Entrega y Recepción de Carga en Punto de Destino** | Certificar el arribo por geocerca, la recepción e inspección física, y formalizar el cierre formal del seguimiento con datos operativos estructurados. | Socio Logístico / Conductor, Punto de Destino | [[05_CUN_04_ENTREGA_Y_RECEPCION_DESTINO]] |
| **CUN-05** | **Consulta de Trazabilidad y Análisis de Rendimiento** | Consultar la cronología histórica de los despachos y proveer análisis de indicadores de desempeño y comparativas por transportista con exportación a Excel. | Operador SAC, Jefe de Distribución | [[06_CUN_05_CONSULTA_TRAZABILIDAD_Y_RENDIMIENTO]] |

---

## 4. Matriz de Relacionamiento y Participación (Actores vs. CUN)

La siguiente matriz define el grado de interacción de cada actor con los procesos de negocio:
- **I (Iniciador):** Detona o ejecuta proactivamente el flujo del proceso.
- **P (Participante):** Interviene activamente en tareas intermedias o colaborativas.
- **R (Receptor):** Recibe el valor o resultado final del proceso.

| Actor del Negocio | CUN-01: Despacho y Salida CD | CUN-02: Traslado y Monitoreo | CUN-03: Cancelación Forzada | CUN-04: Entrega y Recepción | CUN-05: Trazabilidad y Rendimiento |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Socio Logístico / Conductor** | **P** (Recibe carga y código) | **I** (Conduce y emite datos) | **I** (Comunica contingencia externa) | **I** (Entrega bultos y registra en app) | — |
| **Punto de Destino / Agencia** | — | — | — | **R** (Inspecciona y recibe físicamente) | — |
| **Supervisor de Distribución** | **I** (Controla andén y habilita) | **P** (Torre de Control) | **P** (Evalúa y cancela en Web) | — | — |
| **Jefe de Distribución** | — | — | — | — | **I** (Evalúa KPIs y exporta Excel) |
| **Operador SAC / Soporte** | — | — | — | — | **P** (Consulta historial de viaje) |

---

## 5. Estructura Documental de la Carpeta

Para una exploración detallada, los módulos documentales se organizan de la siguiente manera:

1. **[[00_INDICE_Y_MODELO_GENERAL_CUN]]** (Este documento): Marco conceptual, diagrama macro en Mermaid y visión consolidada.
2. **[[01_ACTORES_DEL_NEGOCIO]]**: Fichas exhaustivas de caracterización de actores, atribuciones, gobernanza e información intercambiada.
3. **[[02_CUN_01_DESPACHO_Y_SALIDA_CD]]**: Especificación formal de CUN-01 (Despacho y Salida de Carga en Centro de Distribución).
4. **[[03_CUN_02_TRASLADO_Y_MONITOREO]]**: Especificación formal de CUN-02 (Traslado y Monitoreo de Carga en Tránsito).
5. **[[04_CUN_03_CANCELACION_FORZADA_SEGUIMIENTO]]**: Especificación formal de CUN-03 (Cancelación Forzada del Seguimiento).
6. **[[05_CUN_04_ENTREGA_Y_RECEPCION_DESTINO]]**: Especificación formal de CUN-04 (Entrega y Recepción de Carga en Punto de Destino).
7. **[[06_CUN_05_CONSULTA_TRAZABILIDAD_Y_RENDIMIENTO]]**: Especificación formal de CUN-05 (Consulta de Trazabilidad y Análisis de Rendimiento).
8. **[[07_MATRIZ_TRAZABILIDAD_CUN_VS_RF]]**: Mapeo bidireccional entre los 5 CUN y los 28 Requerimientos Funcionales oficiales de software.
9. **[[08_GUIA_MODELADO_VISUAL_PARADIGM]]**: Manual paso a paso para diagramar el CUN en Visual Paradigm Community Edition conforme a la Sesión 4 de la UTP.
