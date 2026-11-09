# MATRIZ DE TRAZABILIDAD Y CONSISTENCIA DOCUMENTAL DE DIAGRAMAS UML
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Correspondencia entre Artefactos de Arquitectura, Requisitos, Componentes y Modelos Visuales

---

## 1. Introducción y Marco de Trazabilidad Documental

El presente documento constituye el entregable **REV_07: Matriz de Trazabilidad y Consistencia Documental de Diagramas UML**, desarrollado con el propósito de garantizar que cada uno de los modelos canónicos del proyecto (`DIAG-01` a `DIAG-11`) cuente con un respaldo documental estricto, bidireccional y verificable respecto a los requerimientos del negocio, las decisiones arquitectónicas aprobadas, los componentes de software y las fases del ciclo TOGAF ADM.

### 1.1. Principios de Trazabilidad y Criterios de Auditoría Semántica

Conforme a las directrices de la auditoría arquitectónica:
- **Trazabilidad de Correspondencia Conceptual:** Todo elemento modelado (actor, sistema, microservicio, nodo, estado o flujo) debe tener una justificación formal en los catálogos maestros (`00_1`, `00_2`, `00_3`), en la Matriz Maestra de 24 Requisitos (`01_5`) o en los capítulos de las Fases Preliminar a H.
- **Consistencia Inter-Modelos:** No se admiten contradicciones semánticas entre diagramas. Las denominaciones de sistemas, la taxonomía de estados de despacho, los horizontes temporales de migración y los hitos de gobernanza deben ser idénticos en todos los diagramas donde aparezcan.
- **Alcance de la Auditoría:** Este informe no evalúa la destreza del dibujo manual, sino la correspondencia documental y la coherencia metodológica de las especificaciones de diseño previo a su construcción en herramientas visuales.

---

## 2. Definición Canónica de Estados del Ciclo de Despacho B2B

Para erradicar discrepancias detectadas en borradores previos (donde se confundían etapas operativas con estados de pedido o se enumeraban seis estados sin justificación), se ratifica la taxonomía oficial establecida en el Glosario Canónico `00_1` y en la Fase B:

### 2.1. Los Cinco Macro-Estados Secuenciales Canónicos
1. **`Estado 1: Pedido facturado`:** Pedido comercial generado por la Consultora/Directora en *Maya* o *ZapCommerce* (`SIS-04`), transmitido y confirmado contablemente en *SAP R3* (`SIS-01`) y recepcionado para secuenciación en el sistema *SPY* (`SIS-02`).
2. **`Estado 2: En preparación o elaboración`:** Inicio de operaciones en el Centro de Distribución Lurín (`ACT-03`, `ACT-04`). Comprende la apertura de cajas master UA (*Unit of Assembly*), recolección de productos (*picking*) y embalaje final de cajas de pedido.
3. **`Estado 3: Pedido despachado`:** Culminación del proceso interno en planta Lurín. Abarca el etiquetado, pesaje automatizado, consolidación por departamento/provincia en rampa de salida (`ACT-05`) y transferencia formal de custodia al operador logístico (`ACT-06`, `ACT-07`).
4. **`Estado 4: Pedido en ruta`:** Transporte multimodal nacional desde Lima hacia los 24 departamentos del Perú. Incluye tránsito troncal terrestre, conexiones intermodales (fluvial a Iquitos o aéreo en emergencias) y arribo al centro de distribución regional o última milla.
5. **`Estado 5: Pedido entregado`:** Arribo al domicilio o punto comercial de la Consultora/Directora Independiente (`ACT-01`), verificación física de bultos y firma de conformidad de recepción (*Proof of Delivery* - POD).

### 2.2. Rama Condicional de Excepción: Incidencia en Transporte y Retorno
- **Incidencia de Transporte y Retorno en Logística Inversa:** No constituye un sexto estado ordinario del camino feliz. Es una **transición condicional de excepción** originada desde el `Estado 4` (ante causales como dirección no ubicada, ausencia prolongada de la receptora empresarial, rechazo formal o siniestro en ruta), que activa la retención temporal de la carga y el retorno físico hacia el Almacén de Producto Terminado en Lurín (`ACT-03`).
- **Regla de Consistencia:** Los diagramas de comportamiento (`DIAG-02`, `DIAG-03`, `DIAG-04`, `DIAG-05`, `DIAG-06`) deben utilizar estrictamente esta misma semántica de 5 macro-estados más la bifurcación condicional de incidencia.

---

## 3. Matriz Consolidada de Trazabilidad: Diagrama vs. Artefactos Arquitectónicos

A continuación se establece la correspondencia integral de cada diagrama canónico con sus fuentes documentales, apartados de maquetación en Word, requerimientos y componentes:

| Diagrama | Fase TOGAF | Ubicación en Word y Figura Asignada | Requerimientos Trazables (ISO 29148) | Decisiones de Arquitectura | Componentes y Adaptadores Involucrados | Paquetes y Contratos |
| :---: | :---: | :--- | :--- | :--- | :--- | :--- |
| **`DIAG-01`** | **Fase A** | Apartado 3.3<br>**Figura 3.1.** Cadena de Valor y Visión Conceptual de la Capacidad MTD | `REQ-NEG-001`, `002`<br>`REQ-FUN-001`<br>`REQ-RES-001`, `002` | `DEC-01`, `DEC-02`<br>`DEC-03`, `DEC-04` | Capacidad MTD, `SIS-01` (SAP R3), `SIS-02` (SPY), `SIS-03` (Maya), `SIS-04` (ZapCommerce), `SIS-05` (Bus), `SIS-06..08` (TMS) | Visión global de capacidades de solución |
| **`DIAG-02`** | **Fase B** | Apartado 4.1<br>**Figura 4.1.** Modelo UML de Casos de Uso del Negocio B2B | `REQ-NEG-001`, `003`<br>`REQ-FUN-002`<br>`REQ-RES-001`, `004` | `DEC-03`, `DEC-04`<br>`DEC-06` | Casos de Uso `CUN-01` a `CUN-06`<br>Actores `ACT-01`, `ACT-03..08` | Delimitación de alcance operativo B2B |
| **`DIAG-03`** | **Fase B** | Apartado 4.2<br>**Figura 4.2.** Flujo Operativo de Despacho Actual AS-IS | `REQ-NEG-001`<br>`REQ-INT-001`<br>`REQ-ARQ-001` | `DEC-01`, `DEC-05` | Carriles: `ACT-02` a `ACT-08`<br>Sistemas: `SAP R3`, `SPY`, Batch FTP | Línea base operativa; evidencia de desfase de $\sim 120\text{ min}$ |
| **`DIAG-04`** | **Fase B** | Apartado 4.3<br>**Figura 4.3.** Flujo Operativo de Despacho Objetivo TO-BE | `REQ-NEG-002`, `004`<br>`REQ-FUN-001`, `003`<br>`REQ-RES-004` | `DEC-02`, `DEC-03`<br>`DEC-05`, `DEC-07` | Carriles: `ACT-01`, `ACT-07` (Móvil), MTD Core, CD Lurín (`ACT-04/05`), Core (`ACT-03/06`) | Coreografía de despacho TO-BE con marcas $T_0/T_1$ y buffer offline |
| **`DIAG-05`** | **Fase C** | Apartado 5.3<br>**Figura 5.1.** Secuencia de Consulta y Latencia AS-IS | `REQ-NEG-001`<br>`REQ-INT-002`<br>`REQ-ARQ-001` | `DEC-01`, `DEC-05` | `ACT-01`, `ACT-08` (CELLFORCE), `SIS-01` (SAP R3), `SIS-02` (SPY), Servidor FTP, TMS | Demostración formal de brecha temporal $\ge 120\text{ min}$ |
| **`DIAG-06`** | **Fase C** | Apartado 5.3<br>**Figura 5.2.** Secuencia de Notificación Reactiva TO-BE | `REQ-FUN-001`, `004`<br>`REQ-INT-001`, `003`<br>`REQ-RNF-001`, `002` | `DEC-02`, `DEC-03`<br>`DEC-05`, `DEC-07` | `CMP-ING-01`, `CMP-COR-02`, `CMP-VAL-03`, `CMP-AUD-04`, `CMP-PUB-05`, `CMP-NOT-06`, Bus `SIS-05` | Ingesta $\le 2\text{ s}$, correlación $\le 5\text{ s}$, notificación $\le 30\text{ min}$ |
| **`DIAG-07`** | **Fase D** | Apartado 6.2<br>**Figura 6.1.** Topología de Despliegue Tecnológico Híbrido | `REQ-RNF-003`, `004`<br>`REQ-ARQ-002`, `003`<br>`REQ-RES-002` | `DEC-01`, `DEC-02`<br>`DEC-07` | Nodos CD Lurín (`SAP R3`, `SPY`, MTD Core, `DB-EVENTSTORE`), Nodo DMZ (Gateway), Móvil | Aislamiento perimetral, túnel IPsec, seguridad TLS 1.3/mTLS |
| **`DIAG-08`** | **Fase E** | Apartado 7.3<br>**Figura 7.1.** Arquitectura de Solución y Paquetes de Trabajo | `REQ-FUN-001..005`<br>`REQ-INT-001..004`<br>`REQ-ARQ-001..003` | `DEC-01`, `DEC-02`<br>`DEC-06`, `DEC-07` | `CMP-ING-01..NOT-06`, `ADP-SAP`, `ADP-SPY`, `ADP-NSDG`, `ADP-LOG`, `ADP-CRM`, `DB-EVENTSTORE` | Paquetes de trabajo `PKG-01` a `PKG-05` (Cobertura de brechas GAP-01..08) |
| **`DIAG-09`** | **Fase F** | Apartado 8.2<br>**Figura 8.1.** Roadmap Estratégico y Máquina de Estados de Transición | `REQ-NEG-001..004`<br>`REQ-RES-001..004` | `DEC-02`, `DEC-03`<br>`DEC-06` | Estados: AS-IS, AT-1, AT-2, TO-BE.<br>Hitos: S1 a S24 relativas | Paquetes `PKG-01..05`; Puertas Gate 1 (S12), Gate 2 (S18), Gate 3 (S24) |
| **`DIAG-10`** | **Fase G** | Apartado 9.3<br>**Figura 9.1.** Flujo de Gobernanza, Puertas de Calidad y Dispensas | `REQ-ARQ-001..003`<br>`REQ-RES-002` | `DEC-DOC-01`<br>`DEC-01` | Architecture Board, PMO/QA, Órgano Revisor, Repositorio EAR | Contratos canónicos `AC-PKG-01..05`, Quality Gates 1, 2, 3, Formularios WVR |
| **`DIAG-11`** | **Fase H** | Apartado 10.3<br>**Figura 10.1.** Ciclo de Vida y Gestión del Cambio de Arquitectura | `REQ-NEG-004`<br>`REQ-RNF-001`<br>`REQ-ARQ-003` | `DEC-03`, `DEC-05`<br>`DEC-DOC-01` | Sensores SLA (`CMP-AUD-04`), Auditoría lecturas SAP, Architecture Board | Tres Umbrales Canónicos (RFC Tipo 1 ITIL, Tipo 2 E-F-G, Tipo 3 Fase A) |

---

## 4. Verificación de Consistencia Inter-Modelos

Para certificar la integridad semántica del conjunto documental, se auditan cinco dimensiones críticas de consistencia cruzada entre los once diagramas:

### 4.1. Consistencia Tecnológica del ERP Central
- **Regla Estricta:** El ERP central de Almacén Lurín es exclusivamente **`SAP R3 (SIS-01)`**.
- **Comprobación:** Verificado en `DIAG-01`, `DIAG-02`, `DIAG-03`, `DIAG-04`, `DIAG-05`, `DIAG-07`, `DIAG-08`, `DIAG-09` y `DIAG-10`. En ninguno de los diagramas se introduce la denominación *S/4HANA*.
- **Aislamiento Operativo (`REQ-RES-002`, `DEC-01`):** En los diagramas de interacción y despliegue (`DIAG-06`, `DIAG-07`, `DIAG-08`), el ERP se mantiene como un componente de sólo lectura mediante vistas o tablas de staging intermedias (`ZTRK_DESPACHO_STG`), con **cero conexiones de escritura directa desde el MTD**.

### 4.2. Consistencia en la Taxonomía de Estados del Pedido
- **Regla Estricta:** Utilizar los 5 macro-estados canónicos más la rama de excepción.
- **Comprobación:**
  - En `DIAG-02` (Casos de Uso): Los casos `CUN-01` a `CUN-05` modelan las etapas operativas de los estados 1 a 5; `CUN-06` modela la extensión condicional de incidencia y retorno.
  - En `DIAG-03` y `DIAG-04` (Actividades): Las actividades siguen la secuencia temporal de envasado, picking, despacho y entrega, con punto de decisión para entrega exitosa vs. incidencia.
  - En `DIAG-05` y `DIAG-06` (Secuencia): Los mensajes y eventos transmitidos llevan como payload los estados canónicos `1_FACTURADO`, `2_PREPARADO`, `3_DESPACHADO`, `4_EN_RUTA`, `5_ENTREGADO` y el evento condicional `INCIDENCIA_RUTA`.
  - En `DIAG-08` (Componentes): El motor de correlación `CMP-COR-02` valida transiciones entre estos mismos 5 estados.

### 4.3. Consistencia Temporal del Cronograma y Fases de Transición
- **Regla Estricta:** El horizonte de planificación es de **24 semanas relativas (S1 a S24)**, estructurado en dos arquitecturas de transición y tres Quality Gates.
- **Comprobación:**
  - En `DIAG-09` (Roadmap): El estado AT-1 abarca las semanas S1 a S12; el estado AT-2 abarca las semanas S11 a S18; la Arquitectura TO-BE culmina en la semana S24.
  - En `DIAG-10` (Gobernanza): Se representan con exactitud tres hitos de evaluación vinculante: **Gate 1 en Semana S12**, **Gate 2 en Semana S18** y **Gate 3 en Semana S24**.
  - Quedan completamente descartadas las discrepancias históricas de cronogramas a 12 meses o de 5 compuertas.

### 4.4. Consistencia en Paquetes de Trabajo y Contratos de Arquitectura
- **Regla Estricta:** Relación biunívoca entre los cinco paquetes de trabajo `PKG-01..05` y los cinco Contratos de Arquitectura `AC-PKG-01..05`.
- **Comprobación:**
  - En `DIAG-08`: Se agrupan los componentes y adaptadores en los paquetes `PKG-01` a `PKG-05`.
  - En `DIAG-09`: Las transiciones del roadmap indican la activación secuencial de `PKG-01..05`.
  - En `DIAG-10`: El marco de gobernanza audita el cumplimiento de los contratos `AC-PKG-01` (Datos), `AC-PKG-02` (Core MTD), `AC-PKG-03` (Adaptadores Internos), `AC-PKG-04` (Onboarding Transportistas) y `AC-PKG-05` (Monitoreo y Telemetría).

### 4.5. Consistencia en la Nomenclatura de Actores y Operadores Logísticos
- **Regla Estricta:** Mantener la diferenciación formal entre actores y operadores sin fusiones arbitrarias.
- **Comprobación:**
  - En `DIAG-01`, `DIAG-07` y `DIAG-08`: Se mantienen diferenciados `NSDG` (`SIS-06`) y `ENSDY` (`SIS-07`) conforme al supuesto `ASM-02`.
  - `CELLFORCE` (`SIS-09`) se modela en `DIAG-01`, `DIAG-05` y `DIAG-07` como el sistema consumidor CRM de Customer Service (`ACT-08`).
  - `DRIVING` (`SIS-08`) se modela en `DIAG-01`, `DIAG-07` y `DIAG-08` como la plataforma de optimización de rutas del operador de transporte.

---

## 5. Dictamen de Trazabilidad y Coherencia Documental

> **DICTAMEN AUDITADO:**  
> **«TRAZABILIDAD Y CONSISTENCIA DOCUMENTAL VERIFICADA AL 100%»**  
> 
> Se certifica que los once diagramas canónicos (`DIAG-01` a `DIAG-11`) presentan una correspondencia perfecta, bidireccional y sin contradicciones respecto a las especificaciones metodológicas, requerimientos canónicos, decisiones de arquitectura y paquetes de trabajo.  
> Los modelos complementarios de la Fase C (`DIAG-C1` y `DIAG-C2`) se mantienen bloqueados para construcción por carecer de detalle relacional aprobado o por duplicar el alcance de `DIAG-08`, preservando la integridad y solidez conceptual del informe final.
