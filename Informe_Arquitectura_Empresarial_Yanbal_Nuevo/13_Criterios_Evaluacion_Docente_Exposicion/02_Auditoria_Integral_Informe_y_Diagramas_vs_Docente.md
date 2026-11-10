# Auditoría Integral del Informe y Diagramas frente a los Criterios de Evaluación del Docente

> **Propósito del documento:** Presentar el diagnóstico completo del repositorio `Informe_Arquitectura_Empresarial_Yanbal_Nuevo` contrastado contra las observaciones, preguntas y exigencias reales del docente durante las exposiciones de clase (`01_Sintesis_Observaciones_y_Preguntas_Docente_Expo.md`).  
> **Estado de archivos originales:** **Intactos (0 modificaciones realizadas).**

---

## Índice de la Auditoría

1. [Auditoría de Requerimientos: ¿6 Categorías o solo RF y RNF?](#1-auditoría-de-requerimientos-6-categorías-o-solo-rf-y-rnf)
2. [Auditoría de Diagramas: ¿Por qué faltan diagramas visuales en ciertas fases?](#2-auditoría-de-diagramas-por-qué-faltan-diagramas-visuales-en-ciertas-fases)
3. [Estrategia Dual de Diagramación: Cómo ordenar Diagramas Generales + Diagramas Separados (Derivados)](#3-estrategia-dual-de-diagramación-cómo-ordenar-diagramas-generales--diagramas-separados-derivados)
4. [Auditoría del Orden General del Informe (Fase por Fase frente al Docente)](#4-auditoría-del-orden-general-del-informe-fase-por-fase-frente-al-docente)
5. [Auditoría de Carpetas del Repositorio: Qué entra al Informe Entregable y qué es Control Interno](#5-auditoría-de-carpetas-del-repositorio-qué-entra-al-informe-entregable-y-qué-es-control-interno)
6. [Plan de Acción Recomendado (Pendiente de Aprobación)](#6-plan-de-acción-recomendado-pendiente-de-aprobación)

---

## 1. Auditoría de Requerimientos: ¿6 Categorías o solo `RF` y `RNF`?

### 1.1. Comparativa: Lo que hicieron otros grupos vs. Nuestro Informe

- **En las exposiciones (Grupo 2, min `16:10`–`16:28`):** Los grupos evaluados presentaron únicamente **2 listas básicas**:
  1. **RF (Requerimientos Funcionales)**
  2. **RNF (Requerimientos No Funcionales)**
- **En nuestro informe (`01_Fase_Preliminar/01_5_Matriz_Maestra_Requerimientos.md`):** Tenemos **24 requerimientos canónicos** estructurados en **6 categorías formales** alineadas con **TOGAF ADM** e **ISO/IEC/IEEE 29148**:
  1. `REQ-NEG-001` al `004`: Requerimientos de Negocio (4)
  2. `REQ-FUN-001` al `005`: Requerimientos Funcionales (5)
  3. `REQ-INT-001` al `004`: Requerimientos de Integración e Interoperabilidad (4)
  4. `REQ-RNF-001` al `004`: Requerimientos No Funcionales / Atributos de Calidad (4)
  5. `REQ-ARQ-001` al `003`: Requerimientos Arquitectónicos (3)
  6. `REQ-RES-001` al `004`: Restricciones de Diseño y Mandatos Corporativos (4)

### 1.2. Veredicto de Auditoría

- **¿Está bien trabajar con las 6 categorías?** **SÍ, está muy bien y es metodológicamente superior.** Toda la trazabilidad transversal del informe (Fases A, B, C, D, E, F, G, H y `ANX-02`) utiliza los códigos `REQ-NEG-*`, `REQ-FUN-*`, `REQ-INT-*`, `REQ-RNF-*`, `REQ-ARQ-*` y `REQ-RES-*`. Cambiar o borrar esos códigos rompería la trazabilidad de todo el informe.
- **¿Cuál es el riesgo con el profesor?** Que el profesor revise rápido buscando solo las siglas **"RF"** y **"RNF"** y pregunte por qué hay 6 tipos en vez de 2.
- **Solución sin alterar ningún ID existente:** Mantener las 6 categorías intactas, pero encabezar la sección `1.5` con una **Tabla Puente de Agrupación Macro (`RF` vs. `RNF`)**, demostrando que las 6 categorías se agrupan exactamente en los 2 bloques clásicos que pide el docente:

| Macro-Bloque Clásico (Docente) | Categorías Específicas de Nuestro Informe (TOGAF / ISO 29148) | Códigos Canónicos Incluidos | Cantidad |
| :--- | :--- | :--- | :---: |
| **BLOQUE I: Requerimientos Funcionales y de Negocio (`RF` en sentido amplio)** | • **Requerimientos Funcionales de Aplicación (`REQ-FUN`)**<br>• **Requerimientos Funcionales de Integración (`REQ-INT`)**<br>• **Requerimientos / Reglas de Negocio (`REQ-NEG`)** | • `REQ-FUN-001` al `REQ-FUN-005`<br>• `REQ-INT-001` al `REQ-INT-004`<br>• `REQ-NEG-001` al `REQ-NEG-004` | **13 requerimientos** |
| **BLOQUE II: Requerimientos No Funcionales y Restricciones (`RNF` en sentido amplio)** | • **Atributos de Calidad / No Funcionales (`REQ-RNF`)**<br>• **Requerimientos No Funcionales Arquitectónicos (`REQ-ARQ`)**<br>• **Restricciones Tecnológicas y Regulatorias (`REQ-RES`)** | • `REQ-RNF-001` al `REQ-RNF-004`<br>• `REQ-ARQ-001` al `REQ-ARQ-003`<br>• `REQ-RES-001` al `REQ-RES-004` | **11 requerimientos** |
| **TOTAL** | **6 Categorías Canónicas Trazables** | **`REQ-001` a `REQ-024`** | **24 requerimientos** |

---

## 2. Auditoría de Diagramas: ¿Por qué faltan diagramas visuales en ciertas fases?

Al auditar físicamente todas las carpetas del proyecto frente al `INDICE_GENERAL.md` y frente a lo que el docente exigió ver en pantalla, se identificaron **4 hallazgos exactos** que explican por qué sientes que no están todos los diagramas:

### Hallazgo 1 (CRÍTICO): La Fase C (`04_Fase_C`) NO TIENE carpeta `diagramas/` ni archivos `.puml`

- En `INDICE_GENERAL.md` están declarados los apartados:
  - `4.1.2. Modelo lógico de datos corporativo (entidades maestras y transaccionales)`
  - `4.2.3. Diagrama de componentes UML e interfaces de servicio expuestas y consumidas`
- Sin embargo, dentro de `04_Fase_C_Arquitectura_de_Sistemas_de_Informacion/04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md`:
  - **En `4.1.2` (líneas 56–234):** Solo existen **tablas markdown** describiendo las 10 entidades (`ENT-01` a `ENT-10`) y las 10 relaciones (`REL-01` a `REL-10`), pero **NO existe el diagrama visual Entidad-Relación (`.puml`)**.
  - **En `4.2.3` (líneas 319–351):** Solo existe un **esquema dibujado con caracteres ASCII** para representar los componentes `CMP-01` al `CMP-06` y los adaptadores `ADP-*`, pero **NO existe el archivo `.puml` del Diagrama de Componentes UML**.
- **¿Por qué ocurrió esto?** En las revisiones previas (`REV_04` y `REV_10`), se bloqueó la generación de `DIAG-C1` (Modelo Lógico de Datos) y `DIAG-C2` (Diagrama de Componentes) para no mover la numeración `DIAG-01` a `DIAG-11`.
- **Impacto frente al docente (ALTO RIESGO):** En la exposición (min `19:50`–`20:40`, `21:50` y `24:29`–`24:55`), el docente **exigió ver el Diagrama del Modelo Lógico de Datos (con `PK`, `FK`, `UNIQUE`, `NULL`, estados nuevos y tabla de auditoría) y el Diagrama de Componentes en la Fase C**. Todos esos datos ya están escritos en las tablas de nuestro Capítulo 4, por lo que **debemos generar sus diagramas `.puml` (general y derivados) sin inventar ningún dato nuevo**.

### Hallazgo 2: La Fase A (`02_Fase_A`) no tiene carpeta `diagramas/` con el `.puml` del diagrama general `DIAG-01`

- En `02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md` (líneas 583–632), el diagrama `DIAG-01` (Concepto de Solución TO-BE) está incrustado únicamente como bloque `mermaid`.
- A diferencia de las Fases B, D, E, F, G y H, la carpeta `02_Fase_A_Vision_de_Arquitectura/` **no tiene una subcarpeta `diagramas/` con el archivo `DIAG-01_Concepto_Solucion_TO_BE.puml`** (aunque sus 3 diagramas derivados `DIAG-01.1`, `01.2` y `01.3` ya fueron creados en `.puml` dentro de `12_Diagramas_Compuestos_Derivados/DIAG-01_Derivados/`).

### Hallazgo 3: Desfase de ubicación de `DIAG-05` y `DIAG-06` (Diagramas de Secuencia AS-IS y TO-BE)

- Físicamente, `DIAG-05` (Secuencia AS-IS) y `DIAG-06` (Secuencia TO-BE) están dentro de `03_Fase_B_Arquitectura_de_Negocio/diagramas/` y listados en la sección `3.6` del `INDICE_GENERAL.md`.
- Sin embargo, en `REV_03` y `REV_04` se indicó que pertenecen a la **Fase C (`4.2.3`)** porque modelan interacciones entre componentes de software (`Order Service`, `Inventory Service`, `RabbitMQ`, `DB2`, `Redis`).
- **Cómo resolverlo sin perder nada:** Pueden mantenerse referenciados como puente entre el proceso de negocio (Fase B) y la interacción de aplicaciones (Fase C), o ubicarse formalmente en Fase C dejando en Fase B los Casos de Uso (`DIAG-02`) y los Diagramas de Actividades AS-IS y TO-BE (`DIAG-03` y `DIAG-04`).

### Hallazgo 4: Diferenciación visual AS-IS vs. TO-BE en Casos de Uso (`DIAG-02`)

- En la exposición (min `9:46`–`12:30` y `16:40`–`17:20`), el docente preguntó reiteradamente:
  - *"¿Cuál es el AS-IS y cuál es el TO-BE?"*
  - *"¿Qué cajitas han agregado en este diagrama? ¿Qué casos de uso no estaban en el AS-IS y cuáles han mejorado?"*
- En Actividades tenemos `DIAG-03` (AS-IS) y `DIAG-04` (TO-BE) separados (excelente).
- En Casos de Uso tenemos `DIAG-02` (`UC-01` al `UC-10`), donde todos los casos de uso tienen el mismo color. Para responder de inmediato a la pregunta del profesor, conviene tener identificado visualmente (o mediante nota/tabla adjunta) cuáles `UC` eran manuales/parciales en el AS-IS y cuáles son **100% nuevos o automatizados en el TO-BE** (por ejemplo, `UC-02` Reserva Temporal Redis, `UC-05` Despacho Dinámico Multi-Tienda, `UC-08` Tracking en Tiempo Real, `UC-09` Captura POD Digital).

---

## 3. Estrategia Dual de Diagramación: Cómo ordenar Diagramas Generales + Diagramas Separados (Derivados)

Conservar **ambos tipos de diagramas** (**1. Diagrama General Canónico** + **2. Diagramas Separados/Derivados**) resuelve simultáneamente las dos exigencias del profesor en clase:
1. Cuando pide ver **todo el panorama completo en un solo gráfico** (min `10:51`: *"Tienen que poner todo el diagrama"*), está el **Diagrama General (`DIAG-XX`)**.
2. Cuando pide **leer el detalle de cada caja sin que la letra sea microscópica** (min `16:44`: *"Vamos a hacer un zoom de detalle"*), están los **Diagramas Derivados (`DIAG-XX.1`, `DIAG-XX.2`, `DIAG-XX.3`)**.

### Orden Estándar de Presentación dentro de cada Capítulo del Informe

Dentro de cada fase del informe, la subsección de diagramas debe seguir siempre esta secuencia:
1. **Diagrama General (`DIAG-XX`):** Vista macro extremo a extremo de la fase.
2. **Descomposición en Vistas Detalladas (`DIAG-XX.1` a `DIAG-XX.N`):** Subdiagramas derivados con trazabilidad `100%` idéntica de IDs, actores, componentes y relaciones.

### Catálogo Completo de Diagramas (Generales + Derivados + Faltantes en Fase C)

| Fase TOGAF | Sección del Informe | 1. Diagrama General (Vista Macro) | 2. Diagramas Separados / Derivados (Vistas de Detalle / Zoom) | Estado Actual en el Repositorio |
| :--- | :--- | :--- | :--- | :--- |
| **Fase A: Visión** | `2.7.5` | **`DIAG-01`**: Concepto de Solución TO-BE (6 capas arquitectónicas) | • **`DIAG-01.1`**: Canales, Experiencia y Desacoplamiento<br>• **`DIAG-01.2`**: Core Transaccional, Integración Híbrida y Sistemas<br>• **`DIAG-01.3`**: Datos, Analítica y Gobierno Transversal | • General en Mermaid dentro de `02_Fase_A` (falta crear su `.puml` en `02_Fase_A/diagramas/`).<br>• 3 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-01_Derivados/`. |
| **Fase B: Negocio** | `3.6.1` | **`DIAG-02`**: Casos de Uso de Negocio y Sistema (`UC-01` al `UC-10`) | • **`DIAG-02.1`**: Dominio Comercial, Captura y Reservas (`UC-01`..`04`)<br>• **`DIAG-02.2`**: Dominio Logístico, Última Milla y Conciliación (`UC-05`..`10`) | • General listo en `03_Fase_B/diagramas/`.<br>• 2 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-02_Derivados/`. |
| **Fase B: Negocio** | `3.2` / `3.6.2` | **`DIAG-03`**: Actividades del Proceso Order-to-Delivery AS-IS (`PAS-01`..`04`) | • **`DIAG-03.1`**: Captura, Validación Manual y Batch ERP (`PAS-01`, `PAS-02`)<br>• **`DIAG-03.2`**: Almacén Central, Despacho Ciego y Cierre Tardío (`PAS-03`, `PAS-04`) | • General listo en `03_Fase_B/diagramas/`.<br>• 2 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-03_Derivados/`. |
| **Fase B: Negocio** | `3.3` / `3.6.3` | **`DIAG-04`**: Actividades del Proceso Order-to-Delivery TO-BE (`PTO-01`..`06`) | • **`DIAG-04.1`**: Reserva Atómica, Crédito Asíncrono y Eventos (`PTO-01`, `PTO-02`)<br>• **`DIAG-04.2`**: Despacho Multi-Tienda, Ruteo y Entrega OTIF (`PTO-03`, `PTO-04`, `PTO-06`)<br>• **`DIAG-04.3`**: Gestión de Excepciones, Reintentos y Logística Inversa (`PTO-05`) | • General listo en `03_Fase_B/diagramas/`.<br>• 3 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-04_Derivados/`. |
| **Fase C: Datos** | `4.1.2` | 🚨 **FALTA GRAFICAR EN `.puml` (`DIAG-C1`)**: Modelo Lógico de Datos Corporativo (`ENT-01`..`10` y `REL-01`..`10`) | 🚨 **FALTA GRAFICAR DERIVADOS**:<br>• **Vista Datos 1**: Dominio Maestro, Pedidos, Pagos y Reservas (`ENT-01`..`05`, `ENT-09`)<br>• **Vista Datos 2**: Dominio Logístico, Despachos, Tracking, POD y Eventos (`ENT-06`..`08`, `ENT-10`) | ❌ **Hoy solo está en tablas markdown** dentro de `04_Fase_C`. El profesor exige ver el diagrama Entidad-Relación con `PK`, `FK`, `UNIQUE`, `NULL` y estados. |
| **Fase C: Aplicaciones** | `4.2.3` | 🚨 **FALTA GRAFICAR EN `.puml` (`DIAG-C2`)**: Diagrama de Componentes UML e Interfaces (`CMP-01`..`06` y `ADP-*`) | 🚨 **FALTA GRAFICAR DERIVADOS**:<br>• **Vista Componentes 1**: Canales, API Gateway y Microservicios Core (`CMP-01`..`06`)<br>• **Vista Componentes 2**: Bus de Eventos, Adaptadores ACL (`ADP-*`) y Sistemas Legacy/Externos | ❌ **Hoy solo está en texto ASCII** dentro de `04_Fase_C`. El profesor exige ver el Diagrama de Componentes en Fase C. |
| **Fase B / Fase C** | `3.6.4` / `4.2.3` | **`DIAG-05`**: Secuencia de Sistemas AS-IS (`S1-01`..`17`)<br>**`DIAG-06`**: Secuencia de Sistemas TO-BE (`S2-01`..`25`) | • **`DIAG-05.1`** y **`05.2`**: Secuencia AS-IS (Captura/Crédito y Batch/Ruteo)<br>• **`DIAG-06.1`**, **`06.2`**, **`06.3`**: Secuencia TO-BE (Reserva Redis, Saga Asíncrona, Tracking/POD) | • Generales listos en `03_Fase_B/diagramas/`.<br>• 5 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-05_Derivados/` y `DIAG-06_Derivados/`. |
| **Fase D: Tecnología** | `5.2.5` | **`DIAG-07`**: Despliegue e Infraestructura TO-BE (6 zonas) | • **`DIAG-07.1`**: Zona Edge, API Gateway y Cómputo EKS (`CMP-01`..`06`)<br>• **`DIAG-07.2`**: Persistencia Gestionada, Eventos y Conectividad Híbrida<br>• **`DIAG-07.3`**: Red Privada On-Premise (`APP-05`, `APP-08`) y Seguridad | • General listo en `05_Fase_D/diagramas/`.<br>• 3 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-07_Derivados/`. |
| **Fase E: Soluciones** | `6.5` | **`DIAG-08`**: Arquitectura de Solución y Paquetes de Trabajo (`WP-01`..`05`) | • **`DIAG-08.1`**: Cimientos Híbridos y Core de Inventario/Pedidos (`WP-01`, `WP-02`)<br>• **`DIAG-08.2`**: Logística Omnicanal, Última Milla y Analítica (`WP-03`..`05`) | • General listo en `06_Fase_E/diagramas/`.<br>• 2 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-08_Derivados/`. |
| **Fase F: Migración** | `7.2.4` | **`DIAG-09`**: Roadmap Estratégico de Implementación (Olas 1, 2 y 3) | • **`DIAG-09.1`**: Línea Base AS-IS y Ola 1 (Fundación Híbrida — Meses 1-6)<br>• **`DIAG-09.2`**: Ola 2 (Core — Meses 7-12), Ola 3 (Omnicanal — Meses 13-18) y Estado Objetivo | • General listo en `07_Fase_F/diagramas/`.<br>• 2 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-09_Derivados/`. |
| **Fase G: Gobierno** | `8.5` | **`DIAG-10`**: Gobernanza de Implementación y Conformidad (`GATE-01`..`04`) | • **`DIAG-10.1`**: Órganos de Gobierno, Roles y Flujo de Gates (`GATE-01`..`04`)<br>• **`DIAG-10.2`**: Auditoría Continua, Estados de Conformidad y Dispensa | • General listo en `08_Fase_G/diagramas/`.<br>• 2 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-10_Derivados/`. |
| **Fase H: Cambio** | `9.5` | **`DIAG-11`**: Gestión del Cambio y Ciclo Evolutivo ADM | • **`DIAG-11.1`**: Monitoreo Continuo y Clasificación de Cambios (`RFC` Tipo I, II y III)<br>• **`DIAG-11.2`**: Tablero de Valor, KPIs y Ciclo Evolutivo TOGAF ADM | • General listo en `09_Fase_H/diagramas/`.<br>• 2 Derivados listos en `12_Diagramas_Compuestos_Derivados/DIAG-11_Derivados/`. |

---

## 4. Auditoría del Orden General del Informe (Fase por Fase frente al Docente)

El orden macro de los capítulos `01` al `09` en `INDICE_GENERAL.md` sigue rigurosamente el ciclo **TOGAF ADM**. A continuación se audita cada capítulo frente a las preguntas reales del docente:

| Capítulo del Informe | Lo que el Docente Exigió en la Exposición | Estado en Nuestro Informe | Observación / Ajuste Recomendado |
| :--- | :--- | :---: | :--- |
| **1. Fase Preliminar**<br>(`01_Fase_Preliminar`) | • Principios de arquitectura completos (Nombre, Declaración, Justificación, Implicaciones).<br>• Requerimientos claros (`RF` y `RNF`) y restricciones. | **98% Listo** | • Los 10 principios (`PRIN-01`..`10`) tienen las 4 partes completas.<br>• Solo falta añadir al inicio de `1.5` el cuadro resumen que agrupa las 6 categorías en **Bloque I (`RF`)** y **Bloque II (`RNF`)**. |
| **2. Fase A: Visión**<br>(`02_Fase_A`) | • **¡Lo primero que pregunta!** *"¿Dónde está la Visión de la Arquitectura (no la visión de la empresa)? ¿Cuáles son los objetivos estratégicos?"* (`5:20`–`6:20`).<br>• Matriz de Stakeholders (Rol, Interés, Influencia, Preocupación).<br>• Alcance (Qué incluye y qué excluye).<br>• Diagrama conceptual TO-BE. | **90% Listo** | • **Detalle de orden:** En `02_Fase_A`, la declaración explícita de la **"Visión de la Arquitectura"** aparece recién en la sección `2.7` (al final del capítulo), mientras que `2.1` abre con la problemática. Conviene poner un recuadro o subapartado con la **Declaración Ejecutiva de la Visión de la Arquitectura TO-BE** al inicio del capítulo (entre `2.1` y `2.2`) para que el docente la vea de inmediato.<br>• Añadir el `.puml` de `DIAG-01` + sus 3 derivados `DIAG-01.1..3`. |
| **3. Fase B: Negocio**<br>(`03_Fase_B`) | • Mapa de Capacidades.<br>• **Comparación explícita AS-IS vs TO-BE** en Casos de Uso y Diagramas de Actividades: *"¿Qué cajitas han agregado? ¿Cuál es la diferencia con el AS-IS?"* (`9:46`–`12:30`).<br>• Matriz de Brechas de Negocio. | **95% Listo** | • Tenemos Capacidades (`3.1`), Actividades AS-IS (`3.2` + `DIAG-03`), Actividades TO-BE (`3.3` + `DIAG-04`) y Brechas (`3.5`).<br>• Conviene añadir en `3.6.1` una tabla comparativa breve junto a `DIAG-02` que especifique qué Casos de Uso (`UC-01`..`10`) son **Nuevos en el TO-BE** vs **Mejorados respecto al AS-IS**, e integrar los diagramas derivados `DIAG-02.1..2`, `DIAG-03.1..2` y `DIAG-04.1..3`. |
| **4. Fase C: Sistemas de Información**<br>(`04_Fase_C`) | • Dividir claramente en **Datos (`4.1`)** y **Aplicaciones (`4.2`)** (`18:55`).<br>• En **Datos**: Tablas nuevas, estados nuevos, `PK`, `FK`, `UNIQUE`, `NULL`, auditoría y **Diagrama del Modelo Lógico de Datos** (`19:50`–`24:55`).<br>• En **Aplicaciones**: Catálogo de módulos nuevos vs legacy y **Diagrama de Componentes** (`21:50`). | **75% Listo (Faltan los 2 diagramas `.puml`)** | • El texto y las tablas de `4.1` (`ENT-01..10`) y `4.2` (`CMP-01..06`, `ADP-*`) son excelentes y responden todo lo que pide el profesor.<br>• **Falta urgente:** Crear la carpeta `04_Fase_C/diagramas/` con el `.puml` del **Modelo Lógico de Datos (`ENT-01..10`)** y del **Diagrama de Componentes (`CMP-01..06`)**, más sus vistas derivadas. |
| **5. Fase D: Tecnología**<br>(`05_Fase_D`) | • Limitaciones físicas/red del AS-IS (`5.1`).<br>• Arquitectura de despliegue TO-BE en nube/híbrida (`5.2` + `DIAG-07`).<br>• Seguridad, red y brechas tecnológicas (`5.3`, `5.4`). | **100% Listo** | • Integrar los derivados `DIAG-07.1`, `07.2` y `07.3` debajo del general `DIAG-07`. |
| **6. Fase E: Oportunidades y Soluciones**<br>(`06_Fase_E`) | • Consolidación de brechas de Negocio, Datos, Aplicaciones y Tecnología.<br>• Agrupación en **Paquetes de Trabajo (`WP-01`..`05`)** y **dependencias entre paquetes** (`26:35`–`32:10`). | **100% Listo** | • Integrar los derivados `DIAG-08.1` y `08.2` debajo del general `DIAG-08`. |
| **7. Fase F: Planificación de Migración**<br>(`07_Fase_F`) | • Arquitecturas de Transición (Olas 1, 2 y 3).<br>• Matriz de priorización (Valor vs Riesgo), estimación financiera y **Roadmap cronológico** (`29:00`–`34:30`). | **100% Listo** | • Integrar los derivados `DIAG-09.1` y `09.2` debajo del general `DIAG-09`. |
| **8. Fase G: Gobierno de Implementación**<br>(`08_Fase_G`) | • Mecanismos de conformidad durante el desarrollo (`GATE-01`..`04`).<br>• **Rol específico del Arquitecto Empresarial durante la implementación** (`35:58`). | **100% Listo** | • Nuestro capítulo `8.1` y `8.2` ya define los roles y los Gates. Integrar los derivados `DIAG-10.1` y `10.2` debajo del general `DIAG-10`. |
| **9. Fase H: Gestión del Cambio**<br>(`09_Fase_H`) | • Monitoreo de KPIs post-implementación.<br>• **Los 3 tipos de cambio (`RFC`) y cuáles reinician el ciclo ADM** (`36:35`–`36:56`). | **100% Listo** | • Nuestro capítulo `9.2` tiene exactamente los 3 niveles (Tipo I Menor, Tipo II Moderado, Tipo III Mayor que reinicia ADM). Integrar `DIAG-11.1` y `11.2` debajo de `DIAG-11`. |

---

## 5. Auditoría de Carpetas del Repositorio: Qué entra al Informe Entregable y qué es Control Interno

Actualmente el proyecto tiene **13 bloques/carpetas**. Es fundamental distinguir cuáles forman parte del **Informe Académico Entregable al Docente** y cuáles son **archivos internos de auditoría y preparación**:

| Bloque / Carpeta | ¿Incluir en el Informe Final para el Docente? | Explicación |
| :--- | :---: | :--- |
| **`00` Portada e Índice + Capítulos `01` al `09`** | **SÍ (Cuerpo Principal)** | Son las 9 fases obligatorias de TOGAF ADM. |
| **`10_Anexos_Tecnicos` (`ANX-01` al `ANX-07`)** | **SÍ (Anexos Oficiales)** | Glosario (`ANX-01`), Matriz de Trazabilidad E2E (`ANX-02`), Diccionario de Datos (`ANX-03`), Inventario de Diagramas (`ANX-04`), Catálogo de Interfaces (`ANX-05`), SLAs/KPIs (`ANX-06`) y Supuestos (`ANX-07`). |
| **`12_Diagramas_Compuestos_Derivados` (`DIAG-01.1` al `DIAG-11.2`)** | **SÍ (Junto a cada Diagrama General o en Anexo Visual)** | Son los subdiagramas descompuestos para garantizar legibilidad visual total sin perder el diagrama general. |
| **Subapartado `10.8` (`AUD_10`..`12`) y Carpeta `11_Revision_Editorial_y_Modelado` (`REV_01`..`10`)** | **NO (Respaldo Interno del Equipo)** | Son reportes de control de calidad editorial y prompts de Visual Paradigm. **No deben pegarse en el Word final del profesor** para no ensuciar la lectura académica. |
| **`13_Criterios_Evaluacion_Docente_Exposicion`** | **NO (Guía Interna de Defensa y Auditoría)** | Material de estudio y verificación interna del equipo frente a las preguntas del jurado/docente. |

---

## 6. Plan de Acción Recomendado (Pendiente de tu Aprobación)

Si estás de acuerdo con esta auditoría, los pasos concretos que podemos ejecutar (cuando tú lo autorices) son:

1. **Completar los Diagramas Faltantes en Fase C y Fase A (sin inventar datos, usando 100% las tablas existentes):**
   - Crear `02_Fase_A_Vision_de_Arquitectura/diagramas/DIAG-01_Concepto_Solucion_TO_BE.puml`.
   - Crear `04_Fase_C_Arquitectura_de_Sistemas_de_Informacion/diagramas/` con:
     - Diagrama General y Derivados del **Modelo Lógico de Datos Corporativo (`ENT-01` a `ENT-10`)** mostrando `PK`, `FK`, tipos de datos y estados.
     - Diagrama General y Derivados del **Diagrama de Componentes UML (`CMP-01` a `CMP-06` y `ADP-*`)** mostrando interfaces expuestas y consumidas.
2. **Ordenar el Esquema Dual de Diagramas (General + Derivados):**
   - Dejar en cada fase primero el **Diagrama General (`DIAG-XX`)** e inmediatamente después sus **Diagramas Derivados (`DIAG-XX.1..N`)**.
3. **Ajustes Puntuales Blindados para el Profesor:**
   - Añadir la tabla puente **Macro-Bloque RF vs. Macro-Bloque RNF** al inicio de `01_5_Matriz_Maestra_Requerimientos.md` (conservando las 6 categorías intactas).
   - Destacar al inicio de la **Fase A (`02_Fase_A`)** el recuadro de la **Declaración de la Visión de la Arquitectura TO-BE** junto con los objetivos estratégicos.
   - Añadir junto a `DIAG-02` (Fase B) la tabla rápida que identifica qué Casos de Uso son **Nuevos** vs **Mejorados** respecto al AS-IS.

