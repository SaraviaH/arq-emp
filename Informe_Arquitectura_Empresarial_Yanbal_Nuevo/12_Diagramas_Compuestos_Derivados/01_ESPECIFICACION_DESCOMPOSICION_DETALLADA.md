# ESPECIFICACIÓN TÉCNICA DETALLADA DE LOS DIAGRAMAS COMPUESTOS Y DERIVADOS (`DIAG-01` A `DIAG-11`)
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Documentación de Trazabilidad, Tipo de Diagrama UML 2.5.1, Elementos y Relaciones por Subdiagrama

> **Ubicación:** `12_Diagramas_Compuestos_Derivados/01_ESPECIFICACION_DESCOMPOSICION_DETALLADA.md`  
> **Catálogo Índice:** [`00_INDICE_Y_CATALOGO_DIAGRAMAS_DERIVADOS.md`](00_INDICE_Y_CATALOGO_DIAGRAMAS_DERIVADOS.md)  
> **Garantía de Inmutabilidad:** Todos los diagramas generales (`DIAG-01` a `DIAG-11`) y el texto de las Fases Preliminar a H se mantienen **sin ninguna modificación** en sus ubicaciones originales.

---

## 1. Descomposición de `DIAG-01` (Fase A — Visión de Arquitectura)

- **ID y Nombre del Diagrama Original:** `DIAG-01` — *Diagrama Conceptual de la Visión de la Solución Propuesta (Capacidad `MTD`)*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Estructura Compuesta / Diagrama de Paquetes y Flujo Conceptual** *(Categoría: Estructural)*
- **Ubicación Original:** [`02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md` (Apartado 2.7.5)](../02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md#275-diagrama-conceptual-de-la-visión-de-la-solución-to-be-diag-01)
- **Justificación de la División:** El diagrama original anida las 6 responsabilidades internas (`MTD-R01` a `MTD-R06`) dentro del bloque `S2`, junto con las fuentes de Lurín (`S1`), socios logísticos (`B2`), el bus (`S3`) y los cuatro actores consumidores (`S4`), sumando 17 nodos y 16 relaciones.
- **Cantidad de Diagramas Derivados:** **2 diagramas derivados** (`DIAG-01.1` y `DIAG-01.2`).

### 1.1. `DIAG-01.1` — *Visión de Contexto e Interoperabilidad E2E de la Capacidad Propuesta `MTD`*
- **Archivo:** [`DIAG-01_Derivados/DIAG-01.1_Estructura_Compuesta_Contexto_E2E_MTD.md`](DIAG-01_Derivados/DIAG-01.1_Estructura_Compuesta_Contexto_E2E_MTD.md)
- **Tipo de Diagrama:** Diagrama de Estructura Compuesta / Flujo Conceptual de Contexto (OMG UML 2.5.1 — Estructural)
- **Diagrama General de Procedencia:** `DIAG-01`
- **Elementos Originales:**
  - `S1["1. GENERACIÓN, ALMACÉN Y PREPARACIÓN (CD Lurín)"]`: `A1` (`Maya SIS-03 -> ZapCommerce SIS-04`), `A2` (`SPY SIS-02`), `A3` (`SAP R3 SIS-01 - Exclusivo ACT-03`), `A4` (`Zona de Despacho ACT-05`).
  - `S2["2. CAPACIDAD ARQUITECTÓNICA PROPUESTA (MTD - DEC-02)"]`: `B1` (`Mecanismo de Trazabilidad del Despacho (MTD)`), `B2` (`Socios Logísticos ACT-07 y Sistemas NSDG SIS-06 / ENSDY SIS-07 / DRIVING SIS-08`), `C5` (`MTD-R05`), `C6` (`MTD-R06`).
  - `S3["3. INTEROPERABILIDAD DESACOPLADA"]`: `D1` (`Bus de Integración SIS-05, ASM-08`).
  - `S4["4. CONSULTA Y NOTIFICACIÓN A ACTORES (SIN ESCRITURA DIRECTA EN SAP R3)"]`: `E1` (`ACT-08 / CELLFORCE SIS-09`), `E2` (`ACT-09`), `E3` (`ACT-06`), `E4` (`ACT-03`).
- **Relaciones Originales:** `A1 --> A2`, `A3 --> A2`, `A2 --> A4`, `A2 --> B1`, `A4 --> B1`, `B2 --> B1`, `B1 --- C5`, `B1 --- C6`, `C5 --> D1`, `C6 --> D1`, `D1 --> E1`, `D1 --> E2`, `D1 --> E3`, `D1 -.-> E4`.

### 1.2. `DIAG-01.2` — *Descomposición Funcional Interna de Responsabilidades Canónicas del `MTD` (`MTD-R01` a `MTD-R06`)*
- **Archivo:** [`DIAG-01_Derivados/DIAG-01.2_Estructura_Compuesta_Responsabilidades_MTD_CORE.md`](DIAG-01_Derivados/DIAG-01.2_Estructura_Compuesta_Responsabilidades_MTD_CORE.md)
- **Tipo de Diagrama:** Diagrama de Estructura Compuesta / Flujo Interno de Capacidad (OMG UML 2.5.1 — Estructural)
- **Diagrama General de Procedencia:** `DIAG-01`
- **Elementos Originales:**
  - `S2` y subgrafo `MTD_CORE["Responsabilidades Canónicas Propuestas (MTD-R01 a MTD-R06)"]`: `B1`, `C1` (`MTD-R01`), `C2` (`MTD-R02`), `C3` (`MTD-R03`), `C4` (`MTD-R04`), `C5` (`MTD-R05`), `C6` (`MTD-R06`).
  - `S3`: `D1` (`Bus de Integración SIS-05`).
- **Relaciones Originales:** `B1 --- MTD_CORE`, `C1 --> C2`, `C2 --> C3`, `C3 --> C4`, `C4 --> C5`, `C4 --> C6`, `C5 --> D1`, `C6 --> D1`.

---

## 2. Descomposición de `DIAG-02` (Fase B — Arquitectura de Negocio)

- **ID y Nombre del Diagrama Original:** `DIAG-02` — *Diagrama UML de Casos de Uso del Negocio: Preparación, Despacho y Trazabilidad B2B (`CUN-01` a `CUN-06`)*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Casos de Uso (*Use Case Diagram*)** *(Categoría: Comportamiento)*
- **Ubicación Original:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-02_Casos_Uso_Negocio.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-02_Casos_Uso_Negocio.puml)
- **Justificación de la División:** Separa las 18 asociaciones Actor–Caso de Uso y las 2 extensiones `<<extend>>` en dos vistas funcionales limpias sin cruces de líneas: el flujo operativo principal (`CUN-01` a `CUN-04`) y el flujo de consulta, reclamos y logística inversa (`CUN-05` y `CUN-06`).
- **Cantidad de Diagramas Derivados:** **2 diagramas derivados** (`DIAG-02.1` y `DIAG-02.2`).

### 2.1. `DIAG-02.1` — *Casos de Uso del Negocio B2B (Parte 1): Preparación, Despacho, Transporte y Entrega (`CUN-01` a `CUN-04`)*
- **Archivo:** [`DIAG-02_Derivados/DIAG-02.1_Casos_Uso_Flujo_Operativo_CUN01_CUN04.puml`](DIAG-02_Derivados/DIAG-02.1_Casos_Uso_Flujo_Operativo_CUN01_CUN04.puml)
- **Tipo de Diagrama:** Diagrama de Casos de Uso (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-02`
- **Elementos Originales:** Actores `ACT03`, `ACT04`, `ACT05`, `ACT06`, `ACT07`, `ACT09`, `ACT10` (con `note bottom of ACT10`); Casos de uso `CUN01`, `CUN02`, `CUN03`, `CUN04`; `legend bottom`.
- **Relaciones Originales (11 asociaciones):** `ACT03 -- CUN01`, `ACT04 -- CUN01`, `ACT05 -- CUN02`, `ACT06 -- CUN02`, `ACT06 -- CUN03`, `ACT06 -- CUN04`, `CUN02 -- ACT07`, `CUN03 -- ACT07`, `CUN04 -- ACT07`, `CUN04 -- ACT09`, `CUN04 -- ACT10`.

### 2.2. `DIAG-02.2` — *Casos de Uso del Negocio B2B (Parte 2): Consulta, Reclamos e Incidencias / Logística Inversa (`CUN-05` y `CUN-06`)*
- **Archivo:** [`DIAG-02_Derivados/DIAG-02.2_Casos_Uso_Consulta_Incidencias_CUN05_CUN06.puml`](DIAG-02_Derivados/DIAG-02.2_Casos_Uso_Consulta_Incidencias_CUN05_CUN06.puml)
- **Tipo de Diagrama:** Diagrama de Casos de Uso (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-02`
- **Elementos Originales:** Actores `ACT03`, `ACT06`, `ACT08`, `ACT07`, `ACT09`; Casos de uso `CUN05`, `CUN06` y casos base extendidos `CUN03`, `CUN04`; `legend bottom`.
- **Relaciones Originales (7 asociaciones + 2 `<<extend>>`):** `ACT06 -- CUN05`, `ACT06 -- CUN06`, `ACT08 -- CUN05`, `ACT08 -- CUN06`, `CUN05 -- ACT09`, `CUN06 -- ACT07`, `CUN06 -- ACT09`, `CUN06 -- ACT03`, `CUN03 <.. CUN06 : <<extend>>`, `CUN04 <.. CUN06 : <<extend>>`.

---

## 3. Descomposición de `DIAG-03` (Fase B — Actividades AS-IS)

- **ID y Nombre del Diagrama Original:** `DIAG-03` — *Diagrama UML de Actividades del Proceso de Negocio Actual (AS-IS): Preparación, Despacho, Transporte B2B y Logística Inversa*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Actividades con Calles de Responsabilidad / Particiones (*Activity Diagram with Swimlanes*)** *(Categoría: Comportamiento)*
- **Ubicación Original:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-03_Actividades_AS_IS.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-03_Actividades_AS_IS.puml)
- **Justificación de la División:** Contiene 7 carriles organizacionales, 18 actividades con descripciones extensas, 3 bloques `fork` y 3 condicionales `if/else`.
- **Cantidad de Diagramas Derivados:** **3 diagramas derivados** (`DIAG-03.1`, `DIAG-03.2` y `DIAG-03.3`).

### 3.1. `DIAG-03.1` — *Actividades AS-IS (Parte 1): Abastecimiento, Facturación B2B, Picking/Packing y Despacho (Estados 1 a 3)*
- **Archivo:** [`DIAG-03_Derivados/DIAG-03.1_Actividades_AS_IS_Abastecimiento_Picking_Despacho.puml`](DIAG-03_Derivados/DIAG-03.1_Actividades_AS_IS_Abastecimiento_Picking_Despacho.puml)
- **Tipo de Diagrama:** Diagrama de Actividades con Particiones / Swimlanes (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-03` (Líneas 16 a 40)
- **Elementos y Relaciones Originales:** Carriles `ACT-02`, `ACT-03`, `ACT-09`, `ACT-04`, `ACT-05`; `fork` inicial entre envasado/ingreso UA en `SAP R3` (`SIS-01`) con nota sobre merma de 6 h (`ASM-04`) y facturación en `Maya`/`ZapCommerce` (`Estado 1`); apertura de cajas master y picking en `SPY` (`Estado 2`); zonificación a 24 departamentos y entrega a socio logístico (`Estado 3`).

### 3.2. `DIAG-03.2` — *Actividades AS-IS (Parte 2): Transporte Ordinario, Entrega B2B y Pérdida de Visibilidad (~2 h) (Estados 4 y 5)*
- **Archivo:** [`DIAG-03_Derivados/DIAG-03.2_Actividades_AS_IS_Transporte_Entrega_Desfase_2h.puml`](DIAG-03_Derivados/DIAG-03.2_Actividades_AS_IS_Transporte_Entrega_Desfase_2h.puml)
- **Tipo de Diagrama:** Diagrama de Actividades con Particiones / Swimlanes (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-03` (Líneas 42 a 60)
- **Elementos y Relaciones Originales:** Carriles `ACT-07`, `ACT-09`, `ACT-06`, `ACT-08`; traslado multimodal (`Estado 4`); decisión sin incidencia; entrega y recepción en destino B2B; `fork` paralelo que evidencia la actualización diferida con pérdida de visibilidad (~2 h) en `ACT-06` (`Estado 5`) y la atención afectada por margen de desconocimiento (~2 h) en `CELLFORCE` (`SIS-09`, `ACT-08`).

### 3.3. `DIAG-03.3` — *Actividades AS-IS (Parte 3): Incidencias en Transporte, Reposición y Logística Inversa hacia Almacén Lurín*
- **Archivo:** [`DIAG-03_Derivados/DIAG-03.3_Actividades_AS_IS_Incidencias_Logistica_Inversa.puml`](DIAG-03_Derivados/DIAG-03.3_Actividades_AS_IS_Incidencias_Logistica_Inversa.puml)
- **Tipo de Diagrama:** Diagrama de Actividades con Particiones / Swimlanes (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-03` (Líneas 42–45 y 61 a 87)
- **Elementos y Relaciones Originales:** Carriles `ACT-07`, `ACT-06`, `ACT-03`; decisión de incidencia en transporte; validación de si llegó a destino (`ACT-06`) o ocurrió en tránsito (`ACT-07`); inicio de logística inversa; `fork` paralelo entre activación de reposición (`ACT-06`) y retorno físico al Almacén de Lurín (`ACT-07` $\rightarrow$ `ACT-03`), con evaluación en `SAP R3` (`SIS-01`) para reincorporar stock o activar seguro/garantía por siniestro.

---

## 4. Descomposición de `DIAG-04` (Fase B — Actividades TO-BE)

- **ID y Nombre del Diagrama Original:** `DIAG-04` — *Diagrama UML de Actividades del Proceso de Negocio Objetivo Propuesto (TO-BE): Despacho B2B con Capacidad Propuesta MTD ($\Delta T \le 30\text{ min}$)*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Actividades con Calles de Responsabilidad / Particiones (*Activity Diagram with Swimlanes*)** *(Categoría: Comportamiento)*
- **Ubicación Original:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-04_Actividades_TO_BE.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-04_Actividades_TO_BE.puml)
- **Justificación de la División:** Contiene 6 carriles, 21 actividades detalladas con clasificación epistémica, 3 decisiones condicionales y un `fork` triple.
- **Cantidad de Diagramas Derivados:** **3 diagramas derivados** (`DIAG-04.1`, `DIAG-04.2` y `DIAG-04.3`).

### 4.1. `DIAG-04.1` — *Actividades TO-BE (Parte 1): Preparación Trazable en CD y Salida de Despacho con Homologación MTD (Estados 1 a 3)*
- **Archivo:** [`DIAG-04_Derivados/DIAG-04.1_Actividades_TO_BE_Preparacion_Despacho_MTD.puml`](DIAG-04_Derivados/DIAG-04.1_Actividades_TO_BE_Preparacion_Despacho_MTD.puml)
- **Tipo de Diagrama:** Diagrama de Actividades con Particiones / Swimlanes (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-04` (Líneas 17 a 39 y leyenda)
- **Elementos y Relaciones Originales:** Carriles `ACT-03`, `ACT-09`, `ACT-04`, `Capacidad Propuesta MTD`, `ACT-05`; `fork` de precondición `PROC-01` y generación de orden `Estado 1`; picking en `SPY` y emisión propuesta de `Estado 2` con $T_0$; correlación `Código UA`–picking–`Número de Pedido` y sello $T_1$ en `MTD`; zonificación en `ACT-05`, emisión de `Estado 3` con $T_0$ y validación/propagación en `MTD` ($\Delta T \le 30\text{ min}$).

### 4.2. `DIAG-04.2` — *Actividades TO-BE (Parte 2): Transporte Multimodal, Entrega B2B y Propagación en $\Delta T \le 30\text{ min}$ (Estados 4 y 5)*
- **Archivo:** [`DIAG-04_Derivados/DIAG-04.2_Actividades_TO_BE_Transporte_Entrega_SLA_30min.puml`](DIAG-04_Derivados/DIAG-04.2_Actividades_TO_BE_Transporte_Entrega_SLA_30min.puml)
- **Tipo de Diagrama:** Diagrama de Actividades con Particiones / Swimlanes (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-04` (Líneas 40 a 58 y leyenda)
- **Elementos y Relaciones Originales:** Carriles `ACT-07`, `Capacidad Propuesta MTD`, `ACT-06 / ACT-08 / ACT-09`; reporte de `Estado 4: Pedido en ruta` con $T_0$ y *store-and-forward* (`ASM-16`); homologación y propagación de `Estado 4` en `MTD`; decisión de entrega exitosa y subdecisión condicionada de `Persona Autorizada ACT-10` (`ASM-01`); homologación de `Estado 5` ($\Delta T \le 30\text{ min}$) y consulta actualizada en `CELLFORCE` (`SIS-09`).

### 4.3. `DIAG-04.3` — *Actividades TO-BE (Parte 3): Incidencias en Ruta y Notificación Operativa Previa de Retorno (`MTD-R06`)*
- **Archivo:** [`DIAG-04_Derivados/DIAG-04.3_Actividades_TO_BE_Incidencias_Retorno_MTD_R06.puml`](DIAG-04_Derivados/DIAG-04.3_Actividades_TO_BE_Incidencias_Retorno_MTD_R06.puml)
- **Tipo de Diagrama:** Diagrama de Actividades con Particiones / Swimlanes (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-04` (Líneas 47–48 y 59 a 89)
- **Elementos y Relaciones Originales:** Carriles `ACT-07`, `ACT-06 / ACT-08 / ACT-09`, `Capacidad Propuesta MTD`, `ACT-03`; corroboración o identificación de incidencia; registro de causal e inicio de logística inversa con $T_0$; `fork` triple entre correlación `MTD-R06` (con notificación operativa previa a `ACT-03` sin escritura en `SAP R3`), reposición comercial paralela y retorno físico evaluado por `ACT-03` en `SAP R3`.

---

## 5. Descomposición de `DIAG-05` (Fase B / C — Secuencia AS-IS)

- **ID y Nombre del Diagrama Original:** `DIAG-05` — *Diagrama UML de Secuencia del Flujo Actual (AS-IS): Despacho, Actualización Diferida de Estados (~2 h) y Consulta*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Secuencia (*Sequence Diagram*)** *(Categoría: Interacción)*
- **Ubicación Original:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-05_Secuencia_AS_IS.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-05_Secuencia_AS_IS.puml)
- **Justificación de la División:** Presenta 16 líneas de vida horizontales y 25 mensajes numerados. Su división por los bloques `==` y las ramas del `alt` reduce las columnas activas por diagrama de 16 a entre 6 y 10.
- **Cantidad de Diagramas Derivados:** **3 diagramas derivados** (`DIAG-05.1`, `DIAG-05.2` y `DIAG-05.3`).

### 5.1. `DIAG-05.1` — *Secuencia AS-IS (Parte 1): Abastecimiento Primario (`PROC-01`), Picking/Packing (`PROC-02`) y Salida (`PROC-03`)*
- **Archivo:** [`DIAG-05_Derivados/DIAG-05.1_Secuencia_AS_IS_Abastecimiento_Picking_Despacho.puml`](DIAG-05_Derivados/DIAG-05.1_Secuencia_AS_IS_Abastecimiento_Picking_Despacho.puml)
- **Tipo de Diagrama:** Diagrama de Secuencia (OMG UML 2.5.1 — Interacción)
- **Diagrama General de Procedencia:** `DIAG-05` (Mensajes 1 al 11, líneas 19 a 62)
- **Elementos y Relaciones Originales:** Lifelines `ACT03`, `SIS01`, `SIS03`, `SIS04`, `SIS05`, `SIS02`, `ACT04`, `ACT05`, `ACT07`, `SIS06`, `ACT09`; divisores `PROC-01`, `PROC-02`, `PROC-03`; nota `ASM-11` sobre `SIS-06: NSDG`; mensajes 1 a 11.

### 5.2. `DIAG-05.2` — *Secuencia AS-IS (Parte 2): Transporte Multimodal, Pérdida de Visibilidad (~2 h) y Consulta en `CELLFORCE` (`PROC-04/05`)*
- **Archivo:** [`DIAG-05_Derivados/DIAG-05.2_Secuencia_AS_IS_Transporte_Desfase_2h_Consulta.puml`](DIAG-05_Derivados/DIAG-05.2_Secuencia_AS_IS_Transporte_Desfase_2h_Consulta.puml)
- **Tipo de Diagrama:** Diagrama de Secuencia (OMG UML 2.5.1 — Interacción)
- **Diagrama General de Procedencia:** `DIAG-05` (Mensajes 12 al 21, líneas 63 a 79)
- **Elementos y Relaciones Originales:** Lifelines `SIS05`, `ACT06`, `ACT07`, `SIS06`, `SIS08`, `SIS07`, `SIS09`, `ACT08`, `ACT09`; rama ordinaria del bloque `alt`; nota roja `#FFEBEE` de ventana de pérdida de visibilidad (~2 h); mensajes 12 a 21.

### 5.3. `DIAG-05.3` — *Secuencia AS-IS (Parte 3): Excepción Documentada de Incidencia en Transporte y Logística Inversa (`PROC-05`)*
- **Archivo:** [`DIAG-05_Derivados/DIAG-05.3_Secuencia_AS_IS_Incidencias_Logistica_Inversa.puml`](DIAG-05_Derivados/DIAG-05.3_Secuencia_AS_IS_Incidencias_Logistica_Inversa.puml)
- **Tipo de Diagrama:** Diagrama de Secuencia (OMG UML 2.5.1 — Interacción)
- **Diagrama General de Procedencia:** `DIAG-05` (Mensajes 22 al 25, líneas 80 a 91)
- **Elementos y Relaciones Originales:** Lifelines `ACT03`, `SIS01`, `ACT06`, `ACT07`, `SIS08`, `ACT09`; rama `else` de excepción documentada con bloque `opt` y nota sobre reposición en `ACT06`; mensajes 22 a 25.

---

## 6. Descomposición de `DIAG-06` (Fase B / C — Secuencia TO-BE)

- **ID y Nombre del Diagrama Original:** `DIAG-06` — *Diagrama UML de Secuencia del Flujo Objetivo Propuesto (TO-BE): Captura, Correlación, Propagación ($\Delta T \le 30\text{ min}$) y Notificación de Retorno mediante Capacidad Propuesta MTD*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Secuencia (*Sequence Diagram*)** *(Categoría: Interacción)*
- **Ubicación Original:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-06_Secuencia_TO_BE.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-06_Secuencia_TO_BE.puml)
- **Justificación de la División:** Es el diagrama de mayor tamaño del repositorio (18 lifelines horizontales y 49 mensajes numerados). Se divide en 3 partes coherentes con sus separadores `==` y su estructura `alt/else`.
- **Cantidad de Diagramas Derivados:** **3 diagramas derivados** (`DIAG-06.1`, `DIAG-06.2` y `DIAG-06.3`).

### 6.1. `DIAG-06.1` — *Secuencia TO-BE (Parte 1): Preparación en CD y Salida de Despacho vía `SIS-05` y `MTD` (`PROC-01..03` — Estados 1 a 3)*
- **Archivo:** [`DIAG-06_Derivados/DIAG-06.1_Secuencia_TO_BE_Preparacion_Despacho_MTD.puml`](DIAG-06_Derivados/DIAG-06.1_Secuencia_TO_BE_Preparacion_Despacho_MTD.puml)
- **Tipo de Diagrama:** Diagrama de Secuencia (OMG UML 2.5.1 — Interacción)
- **Diagrama General de Procedencia:** `DIAG-06` (Mensajes 1 al 19, líneas 19 a 70)
- **Elementos y Relaciones Originales:** Lifelines `ACT03`, `SIS01`, `SIS03`, `SIS04`, `SIS05`, `MTD`, `SIS02`, `ACT04`, `ACT05`, `ACT07`, `SIS07`, `SIS09`, `ACT09`; nota de neutralidad tecnológica sobre `MTD`; divisores `PROC-01/02` y `PROC-03`; mensajes 1 a 19.

### 6.2. `DIAG-06.2` — *Secuencia TO-BE (Parte 2): Tránsito Multimodal, Entrega B2B y Consulta en $\Delta T \le 30\text{ min}$ (`PROC-04/05` — Estados 4 y 5)*
- **Archivo:** [`DIAG-06_Derivados/DIAG-06.2_Secuencia_TO_BE_Transito_Entrega_SLA_30min.puml`](DIAG-06_Derivados/DIAG-06.2_Secuencia_TO_BE_Transito_Entrega_SLA_30min.puml)
- **Tipo de Diagrama:** Diagrama de Secuencia (OMG UML 2.5.1 — Interacción)
- **Diagrama General de Procedencia:** `DIAG-06` (Mensajes 20 al 37, líneas 71 a 92)
- **Elementos y Relaciones Originales:** Lifelines `SIS05`, `MTD`, `ACT07`, `SIS08`, `SIS07`, `SIS09`, `ACT08`, `ACT09`, `ACT10`; rama ordinaria del bloque `alt` con fragmento `opt` condicionado a `ASM-01`; mensajes 20 a 37.

### 6.3. `DIAG-06.3` — *Secuencia TO-BE (Parte 3): Incidencia en Ruta y Notificación Operativa Previa de Retorno (`MTD-R06`)*
- **Archivo:** [`DIAG-06_Derivados/DIAG-06.3_Secuencia_TO_BE_Incidencias_Retorno_MTD_R06.puml`](DIAG-06_Derivados/DIAG-06.3_Secuencia_TO_BE_Incidencias_Retorno_MTD_R06.puml)
- **Tipo de Diagrama:** Diagrama de Secuencia (OMG UML 2.5.1 — Interacción)
- **Diagrama General de Procedencia:** `DIAG-06` (Mensajes 38 al 47, líneas 93 a 110)
- **Elementos y Relaciones Originales:** Lifelines `ACT03`, `SIS01`, `SIS05`, `MTD`, `ACT06`, `ACT07`, `SIS08`, `SIS07`, `SIS09`, `ACT09`; rama `else` de excepción propuesta con fragmento `opt`, notificación `MTD-R06` sin escritura en `SAP R3` y nota sobre `ACT06`; mensajes 38 a 47.

---

## 7. Descomposición de `DIAG-07` (Fase D — Arquitectura Tecnológica)

- **ID y Nombre del Diagrama Original:** `DIAG-07` — *Diagrama UML de Despliegue de la Arquitectura Tecnológica Objetivo (TO-BE)*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Despliegue (*Deployment Diagram*)** *(Categoría: Estructural)*
- **Ubicación Original:** [`05_Fase_D_Arquitectura_Tecnologica/diagramas/DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml`](../05_Fase_D_Arquitectura_Tecnologica/diagramas/DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml)
- **Justificación de la División:** Contiene 4 macro-nodos de infraestructura, 12 subnodos internos, 22 artefactos y 25 enlaces con protocolos detallados. Se divide por zona de infraestructura física/lógica.
- **Cantidad de Diagramas Derivados:** **3 diagramas derivados** (`DIAG-07.1`, `DIAG-07.2` y `DIAG-07.3`).

### 7.1. `DIAG-07.1` — *Despliegue TO-BE (Parte 1): Nodo On-Premise Lurín (`Node_Lurin`) e Interconexión Segura con Nube Corporativa*
- **Archivo:** [`DIAG-07_Derivados/DIAG-07.1_Despliegue_OnPremise_Lurin_e_Interconexion.puml`](DIAG-07_Derivados/DIAG-07.1_Despliegue_OnPremise_Lurin_e_Interconexion.puml)
- **Tipo de Diagrama:** Diagrama de Despliegue (OMG UML 2.5.1 — Estructural)
- **Diagrama General de Procedencia:** `DIAG-07` (Nodo 1 y enlaces Lurín $\leftrightarrow$ Nube)
- **Elementos y Relaciones Originales:** `Node_Lurin` (`Srv_SAP` con `Art_SAP` y `Art_ADPSAP`, `Srv_SPY` con `Art_SPY` y `Art_ADPSPY`, `Dev_RF` con `Art_RF`, `WS_Despacho` con `Art_DespachoWS`), nodos puente `Node_Middleware` (`Art_Bus`) y `Node_MTD` (`Art_COR02`), y sus 6 relaciones de comunicación LAN/WiFi/VPN IPsec.

### 7.2. `DIAG-07.2` — *Despliegue TO-BE (Parte 2): Nube Corporativa Yanbal (`Node_Cloud`), DMZ, Clúster MTD, Bus y Apps Corporativas*
- **Archivo:** [`DIAG-07_Derivados/DIAG-07.2_Despliegue_Nube_Corporativa_Cluster_MTD_y_Apps.puml`](DIAG-07_Derivados/DIAG-07.2_Despliegue_Nube_Corporativa_Cluster_MTD_y_Apps.puml)
- **Tipo de Diagrama:** Diagrama de Despliegue (OMG UML 2.5.1 — Estructural)
- **Diagrama General de Procedencia:** `DIAG-07` (Nodo 2 completo y enlaces internos)
- **Elementos y Relaciones Originales:** `Node_Cloud` (`Node_DMZ` con `Art_APIGW`, `Node_Middleware` con `Art_Bus`, `Node_MTD` con `Art_ING01..Art_NOT06`, `DB_MTD` con `Art_EventStore`, `Node_CorpApps` con `Art_Maya`, `Art_Zap`, `Art_ENSDY`, `Art_CRM`, `Art_ADPCRM`) y sus 14 enlaces internos.

### 7.3. `DIAG-07.3` — *Despliegue TO-BE (Parte 3): Infraestructura de Socios Logísticos (`Node_Logistics`), Móviles Offline y Destinos B2B*
- **Archivo:** [`DIAG-07_Derivados/DIAG-07.3_Despliegue_Socios_Logisticos_Movil_y_Destinos_B2B.puml`](DIAG-07_Derivados/DIAG-07.3_Despliegue_Socios_Logisticos_Movil_y_Destinos_B2B.puml)
- **Tipo de Diagrama:** Diagrama de Despliegue (OMG UML 2.5.1 — Estructural)
- **Diagrama General de Procedencia:** `DIAG-07` (Nodos 3 y 4 y enlaces hacia DMZ)
- **Elementos y Relaciones Originales:** `Node_Logistics` (`Srv_Partner` con `Art_DRIVING`, `Art_NSDG`, `Art_ADPLOG`; `Dev_CarrierMobile` con `Art_MobileApp` y `DB_LocalBuffer`), `Node_Destinos` (`Art_ClientPortal`), nodos puente `Node_DMZ` (`Art_APIGW`) y `Art_ENSDY`, y sus 5 enlaces externos 4G/5G/HTTPS TLS 1.3/mTLS.

---

## 8. Descomposición de `DIAG-08` (Fase E — Oportunidades y Soluciones)

- **ID y Nombre del Diagrama Original:** `DIAG-08` — *Diagrama de Arquitectura de Solución Seleccionada y Mapa de Paquetes de Trabajo (Fase E)*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Componentes y Diagrama de Paquetes** *(Categoría: Estructural)*
- **Ubicación Original:** [`06_Fase_E_Oportunidades_y_Soluciones/diagramas/DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml`](../06_Fase_E_Oportunidades_y_Soluciones/diagramas/DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml)
- **Justificación de la División:** Separa los dos bloques independientes que coexisten en el mismo archivo: la Arquitectura de Componentes de la Solución (Capas 1 a 4) y el Mapa de Dependencias de Paquetes de Trabajo (`PKG-01` a `PKG-05`).
- **Cantidad de Diagramas Derivados:** **2 diagramas derivados** (`DIAG-08.1` y `DIAG-08.2`).

### 8.1. `DIAG-08.1` — *Diagrama de Componentes: Arquitectura de Solución Seleccionada (EDA/SOA Híbrida sobre Bus `SIS-05`)*
- **Archivo:** [`DIAG-08_Derivados/DIAG-08.1_Componentes_Arquitectura_Solucion_Seleccionada.puml`](DIAG-08_Derivados/DIAG-08.1_Componentes_Arquitectura_Solucion_Seleccionada.puml)
- **Tipo de Diagrama:** Diagrama de Componentes estructurado en Paquetes (OMG UML 2.5.1 — Estructural)
- **Diagrama General de Procedencia:** `DIAG-08` (Capas 1 a 4, líneas 42 a 118)
- **Elementos y Relaciones Originales:** Paquetes `Pkg_Fuentes`, `Pkg_Transporte`, `Pkg_MTD` (con `Suite de Microservicios MTD (PKG-02)`) y `Pkg_Consumidores`, sus 18 componentes/BD/cola, nota `REQ-RES-002` y sus 20 relaciones de integración.

### 8.2. `DIAG-08.2` — *Diagrama de Paquetes: Alineación y Dependencias de Paquetes de Trabajo (`PKG-01` a `PKG-05`)*
- **Archivo:** [`DIAG-08_Derivados/DIAG-08.2_Paquetes_Mapa_Trabajo_PKG01_PKG05.puml`](DIAG-08_Derivados/DIAG-08.2_Paquetes_Mapa_Trabajo_PKG01_PKG05.puml)
- **Tipo de Diagrama:** Diagrama de Paquetes (OMG UML 2.5.1 — Estructural)
- **Diagrama General de Procedencia:** `DIAG-08` (Capa de Paquetes de Trabajo, líneas 122 a 134)
- **Elementos y Relaciones Originales:** Paquete `Pkg_WP` con los 5 rectángulos `WP1` (`PKG-01`) a `WP5` (`PKG-05`) y sus 5 relaciones de dependencia (`WP1 -> WP2`, `WP1 -> WP3`, `WP2 -> WP4`, `WP3 -> WP5`, `WP4 -> WP5`).

---

## 9. Descomposición de `DIAG-09` (Fase F — Planificación de la Migración)

- **ID y Nombre del Diagrama Original:** `DIAG-09` — *Diagrama del Roadmap Estratégico de Implementación y Arquitecturas de Transición*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Máquina de Estados / Roadmap Temporalizado** *(Categoría: Comportamiento)*
- **Ubicación Original:** [`07_Fase_F_Planificacion_de_Migracion/diagramas/DIAG-09_Roadmap_Estrategico_Implementacion.puml`](../07_Fase_F_Planificacion_de_Migracion/diagramas/DIAG-09_Roadmap_Estrategico_Implementacion.puml)
- **Justificación de la División:** Divide la cadena vertical de 6 bloques extensos en dos tramos de transición articulados por `Gate_1` (cierre de S12).
- **Cantidad de Diagramas Derivados:** **2 diagramas derivados** (`DIAG-09.1` y `DIAG-09.2`).

### 9.1. `DIAG-09.1` — *Máquina de Estados / Roadmap (Parte 1): De Línea Base (`AS-IS`) a `AT-1` y `Gate 1` (Semanas S1 a S12)*
- **Archivo:** [`DIAG-09_Derivados/DIAG-09.1_Estados_Roadmap_AS_IS_a_AT1_Gate1.puml`](DIAG-09_Derivados/DIAG-09.1_Estados_Roadmap_AS_IS_a_AT1_Gate1.puml)
- **Tipo de Diagrama:** Diagrama de Máquina de Estados (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-09` (Líneas 36 a 46 y 66–67)
- **Elementos y Relaciones Originales:** Bloques `State_ASIS`, `State_AT1`, `Gate_1` y transiciones `State_ASIS -down-> State_AT1` y `State_AT1 -down-> Gate_1`.

### 9.2. `DIAG-09.2` — *Máquina de Estados / Roadmap (Parte 2): De `Gate 1` a `AT-2`, `Gate 2` y `TO-BE` (Semanas S11 a S24)*
- **Archivo:** [`DIAG-09_Derivados/DIAG-09.2_Estados_Roadmap_AT2_Gate2_a_TO_BE.puml`](DIAG-09_Derivados/DIAG-09.2_Estados_Roadmap_AT2_Gate2_a_TO_BE.puml)
- **Tipo de Diagrama:** Diagrama de Máquina de Estados (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-09` (Líneas 46 a 61 y 68–70)
- **Elementos y Relaciones Originales:** Bloques `Gate_1`, `State_AT2`, `Gate_2`, `State_TOBE` y transiciones `Gate_1 -down-> State_AT2`, `State_AT2 -down-> Gate_2` y `Gate_2 -down-> State_TOBE`.

---

## 10. Descomposición de `DIAG-10` (Fase G — Gobernanza de la Implementación)

- **ID y Nombre del Diagrama Original:** `DIAG-10` — *Marco de Gobernanza de la Implementación, Contratos y Control de Conformidad*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Paquetes y Actividades de Gobernanza** *(Categoría: Estructural / Comportamiento)*
- **Ubicación Original:** [`08_Fase_G_Gobierno_de_Implementacion/diagramas/DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml`](../08_Fase_G_Gobierno_de_Implementacion/diagramas/DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml)
- **Justificación de la División:** Separa los 15 bloques de texto distribuidos en 5 paquetes en dos diagramas enfocados: el flujo principal de Contratos y *Quality Gates*, y el circuito de gestión de Excepciones/Dispensas (*Architecture Waivers*).
- **Cantidad de Diagramas Derivados:** **2 diagramas derivados** (`DIAG-10.1` y `DIAG-10.2`).

### 10.1. `DIAG-10.1` — *Gobernanza (Parte 1): Órganos (`SEC_GOB`), Contratos (`AC-PKG-01..05`), Quality Gates (`GATE 1..3`) y Post-Implementación*
- **Archivo:** [`DIAG-10_Derivados/DIAG-10.1_Paquetes_Gobernanza_Contratos_y_Quality_Gates.puml`](DIAG-10_Derivados/DIAG-10.1_Paquetes_Gobernanza_Contratos_y_Quality_Gates.puml)
- **Tipo de Diagrama:** Diagrama de Paquetes y Flujo de Gobernanza (OMG UML 2.5.1 — Estructural / Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-10` (Paquetes 1, 2, 3 y 5)
- **Elementos y Relaciones Originales:** Paquetes `SEC_GOB` (`BOARD`, `PMO_QA`), `SEC_CONTRATOS` (`AC_P1..AC_P5`), `SEC_GATES` (`GATE1..GATE3`), `SEC_POST` (`PIR`, `FASE_H`) y sus 11 relaciones de emisión y certificación.

### 10.2. `DIAG-10.2` — *Gobernanza (Parte 2): Circuito de Gestión de Excepciones y Solicitudes de Dispensa (`Architecture Waivers`)*
- **Archivo:** [`DIAG-10_Derivados/DIAG-10.2_Actividades_Circuito_Excepciones_y_Waivers.puml`](DIAG-10_Derivados/DIAG-10.2_Actividades_Circuito_Excepciones_y_Waivers.puml)
- **Tipo de Diagrama:** Diagrama de Actividades / Flujo de Dispensas (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-10` (Paquete 4 `SEC_WAIVERS` e interfaces con Paquetes 1, 2 y 3)
- **Elementos y Relaciones Originales:** Paquete `SEC_WAIVERS` (`WAIVER_REQ`, `WAIVER_EVAL`, `WAIVER_CLOSE`), nodos de interfaz `BOARD`, `SEC_CONTRATOS`, `SEC_GATES`, y sus 5 relaciones de control de dispensas.

---

## 11. Descomposición de `DIAG-11` (Fase H — Gestión del Cambio de Arquitectura)

- **ID y Nombre del Diagrama Original:** `DIAG-11` — *Marco de Gestión Continua del Cambio de Arquitectura y Ciclo Evolutivo ADM*
- **Tipo de Diagrama UML (OMG 2.5.1):** **Diagrama de Actividades y Flujo de Proceso** *(Categoría: Comportamiento)*
- **Ubicación Original:** [`09_Fase_H_Gestion_del_Cambio/diagramas/DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml`](../09_Fase_H_Gestion_del_Cambio/diagramas/DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml)
- **Justificación de la División:** Divide los 5 paquetes apilados verticalmente (11 rectángulos extensos y 14 relaciones) en dos etapas secuenciales: detección/evaluación de impacto y decisión/actualización del EAR/reinicio ADM.
- **Cantidad de Diagramas Derivados:** **2 diagramas derivados** (`DIAG-11.1` y `DIAG-11.2`).

### 11.1. `DIAG-11.1` — *Gestión del Cambio (Parte 1): Sensores de Monitoreo Continuo (`SEC_MONITOREO`) y Evaluación de RFC (`SEC_RFC`)*
- **Archivo:** [`DIAG-11_Derivados/DIAG-11.1_Actividades_Monitoreo_Continuo_y_Evaluacion_RFC.puml`](DIAG-11_Derivados/DIAG-11.1_Actividades_Monitoreo_Continuo_y_Evaluacion_RFC.puml)
- **Tipo de Diagrama:** Diagrama de Actividades / Flujo de Proceso (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-11` (Paquetes 1 y 2)
- **Elementos y Relaciones Originales:** Paquetes `SEC_MONITOREO` (`MON_SLA`, `MON_SEC`, `MON_BIZ`, `MON_EXT`) y `SEC_RFC` (`RFC_REG`, `RFC_EVAL`), y sus 5 relaciones de activación y evaluación.

### 11.2. `DIAG-11.2` — *Gestión del Cambio (Parte 2): Clasificación Tripartita (`RFC Tipo 1, 2 y 3`), Repositorio EAR y Reinicio Ciclo ADM*
- **Archivo:** [`DIAG-11_Derivados/DIAG-11.2_Actividades_Clasificacion_RFC_EAR_y_Reinicio_ADM.puml`](DIAG-11_Derivados/DIAG-11.2_Actividades_Clasificacion_RFC_EAR_y_Reinicio_ADM.puml)
- **Tipo de Diagrama:** Diagrama de Actividades y Decisión Arquitectónica (OMG UML 2.5.1 — Comportamiento)
- **Diagrama General de Procedencia:** `DIAG-11` (Conector `RFC_EVAL` y Paquetes 3, 4 y 5)
- **Elementos y Relaciones Originales:** Conector `RFC_EVAL`, paquetes `SEC_DECISION` (`TIPO_1`, `TIPO_2`, `TIPO_3`), `SEC_REPO` (`REPO_EA`), `SEC_NUEVO_CICLO` (`ADM_RESTART`), retorno a `SEC_MONITOREO`, y sus 7 relaciones de derivación y reinicio de ciclo.

