# 7. FASE F: PLANIFICACIÓN DE LA MIGRACIÓN
## Plan de Implementación y Migración, Arquitecturas de Transición, Priorización Multicriterio, Gestión de Riesgos y Plan de Validación para Yanbal Perú (Unique S.A.)

> **Ubicación oficial del documento:** `07_Fase_F_Planificacion_de_Migracion/07_Fase_F_Planificacion_de_la_Migracion.md`  
> **Diagrama asociado:** `DIAG-09` (*Roadmap Estratégico de Implementación y Arquitecturas de Transición*, disponible en código PlantUML editable en `07_Fase_F_Planificacion_de_Migracion/diagramas/DIAG-09_Roadmap_Estrategico_Implementacion.puml`)  
> **Marco metodológico:** TOGAF Standard (The Open Group Architecture Framework) — Fase F: Planificación de la Migración (*Migration Planning*)  
> **Documentos de referencia y gobernanza:** Fase Preliminar, Fase A (Visión de Arquitectura), Fase B (Arquitectura de Negocio), Fase C (Arquitectura de Sistemas de Información), Fase D (Arquitectura Tecnológica), Fase E (Oportunidades y Soluciones), Matriz Maestra de Requerimientos, Catálogo Maestro de Actores y Sistemas, Glosario Canónico y Convenciones de Nomenclatura, y Registro Maestro de Supuestos, Incertidumbres y Decisiones de Arquitectura.  
> **Principio de gobernanza:** Todos los documentos precedentes del repositorio se preservan en modo de lectura como fuentes canónicas de verdad. La Fase F operativiza los cinco paquetes de trabajo canónicos (`PKG-01` a `PKG-05`) definidos en la Fase E, estableciendo las etapas evolutivas, la priorización multicriterio, la gestión de dependencias y el plan de validación integral bajo estricto rigor académico y neutralidad corporativa.

---

## Introducción y Objetivos de la Fase F

### 1. Propósito y Alcance del Plan de Migración

La **Fase F de TOGAF ADM (*Migration Planning*)** tiene como objetivo primordial formular un Plan de Implementación y Migración integral y viable que guíe la transición progresiva desde la línea base operativa y tecnológica actual (AS-IS) hacia la arquitectura objetivo (TO-BE) seleccionada en la Fase E (Arquitectura de Integración Orientada a Eventos y Servicios sobre el Bus Corporativo).

La planificación aborda cómo ordenar, priorizar, secuenciar, mitigar y validar los cinco Paquetes de Trabajo canónicos (`PKG-01` a `PKG-05`) sin interrumpir las operaciones críticas de producción, almacenamiento, picking, despacho y facturación de Yanbal Perú (Unique S.A.), garantizando el cumplimiento vinculante de los límites del proyecto:
1. **Alcance B2B Estricto (`REQ-RES-001`, `DEC-04`):** El plan programa exclusivamente la trazabilidad de los despachos desde las instalaciones de Lima hacia distribuidoras y destinos empresariales en los 24 departamentos del Perú, excluyendo la distribución secundaria hacia consumidoras finales.
2. **Aislamiento Total del ERP (`REQ-RES-002`):** Ninguna actividad de migración ni conector de software asumirá interfaces de escritura directa sobre la base de datos de `SAP R3` (`SIS-01`). La soberanía de registro sobre inventarios permanece reservada al Área de Almacén Lurín (`ACT-03`).
3. **Transporte Tercerizado sin Flota Propia (`REQ-RES-004`, `ASM-05`):** Las iniciativas de integración en ruta se coordinan con los socios logísticos asociados (`ACT-07`), descartando la adquisición o imposición de hardware telemático GPS propietario.
4. **Objetivo de Latencia de Actualización ($\le 30\text{ min}$):** La meta de reducir el desfase de seguimiento desde ~2 horas hasta un máximo de 30 minutos (`REQ-NEG-002`, `REQ-RNF-001`) se trata como un objetivo arquitectónico de diseño, distinguiendo la evidencia cualitativa de la entrevista de cualquier compromiso corporativo contractual (`DEC-05`, `ASM-13`).
5. **Naturaleza Referencial del Calendario (`ASM-14`):** Dado que la compañía no ha fijado un cronograma oficial de inversiones ni fechas calendario comprometidas, la planificación se expresa mediante **semanas relativas de proyecto (Semanas S1 a S24)** en un horizonte referencial de 6 meses, evitando presentar fechas gregorianas ficticias como compromisos aprobados.

---

## 7.1. Estrategia de transición y definición de Arquitecturas de Transición

### 7.1.1. Estrategia general y justificación de una migración gradual frente a un despliegue masivo (*Big Bang*)

La estrategia de migración se rige por un **enfoque incremental y modular por etapas evolutivas**, descartando de manera categórica una sustitución en bloque o despliegue masivo (*Big Bang*):
* **Justificación Operativa:** Las instalaciones industriales y el Centro de Distribución de Lurín procesan pedidos diariamente para abastecer a la red comercial nacional. Una migración masiva paralela expondría la operación al riesgo inaceptable de interrumpir el despacho, generar quiebres de inventario o provocar pérdidas de ventas (`PAE-07`).
* **Justificación de Dependencias:** No es factible integrar a los transportistas externos (`ACT-07`) ni exigirles el reporte de eventos en $\le 30\text{ min}$ si previamente Yanbal no ha desplegado el `API Gateway`, publicado los contratos canónicos JSON y estabilizado los microservicios del `MTD` en su propio entorno corporativo.
* **Justificación de Control de Riesgos:** Una transición por etapas permite validar el aislamiento de `SAP R3`, depurar los mecanismos de almacenamiento local (*Store-and-Forward*) en un grupo piloto controlado de rutas y certificar la seguridad perimetral antes de comprometer la operación en los 24 departamentos.

### 7.1.2. Definición formal de las Arquitecturas de Transición

A partir del análisis exhaustivo de dependencias técnicas y operativas entre los cinco paquetes de trabajo de la Fase E, se justifican **dos estados intermedios indispensables (Arquitecturas de Transición AT-1 y AT-2)** antes de alcanzar el estado objetivo definitivo (TO-BE):

```
+----------------------------------------------------------------------------------------------------+
|                         EVOLUCIÓN MEDIANTE ARQUITECTURAS DE TRANSICIÓN                             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [LÍNEA BASE AS-IS]                                                                                |
|  * Desfase de actualización de estados de hasta ~2 horas en seguimiento nacional.                  |
|  * Reportes batch heterogéneos y dispersos desde plataformas de transportistas.                   |
|  * Sin marcas temporales duales (T0/T1); consultas desfasadas en CELLFORCE (SIS-09).               |
|                                                                                                    |
|         |                                                                                          |
|         v Inicio de Migración: PKG-01, PKG-02 y PKG-03 (Fase A)                                    |
|                                                                                                    |
|  [ARQUITECTURA DE TRANSICIÓN 1 (AT-1): NÚCLEO MTD Y GOBERNANZA INTERNA]                            |
|  * Perímetro DMZ con API Gateway corporativo y contratos JSON INT-DAT-01..08 (PKG-01).             |
|  * Suite modular MTD desplegada: Ingesta, Validación, Correlación y Event Store (PKG-02).          |
|  * Adaptador ADP-SPY capturando Estado 2 con marca T0; conector solo lectura ADP-SAP (PKG-03).    |
|  * Aislamiento estricto de base de datos de SAP R3 certificado por Almacén Lurín.                 |
|                                                                                                    |
|         |                                                                                          |
|         v GATE 1 (Hito M1 - Cierre S12): Certificación Interna de Cero Escritura en SAP           |
|         v Ejecución PKG-04 y PKG-03 (Fase B)                                                       |
|                                                                                                    |
|  [ARQUITECTURA DE TRANSICIÓN 2 (AT-2): INTEGRACIÓN DE TRANSPORTE Y PILOTO]                         |
|  * Conexión segura con socios logísticos (ACT-07) mediante Adaptador ADP-LOG (PKG-04).             |
|  * App móvil con buffer Store-and-Forward offline ante pérdida de señal celular (ASM-16).          |
|  * Eventos en ruta y entrega ingresando al MTD con marca T0 en rutas piloto (Lima/Centro).        |
|  * Adaptador ADP-CRM sincronizando estados consolidados hacia CELLFORCE en <= 30 min.             |
|                                                                                                    |
|         |                                                                                          |
|         v GATE 2 (Hito M2 - Cierre S18): Validación de Resiliencia Offline y SLA Piloto           |
|         v Ejecución PKG-05                                                                         |
|                                                                                                    |
|  [ARQUITECTURA OBJETIVO FINAL (TO-BE): COBERTURA NACIONAL Y GOBIERNO TOTAL]                        |
|  * Cobertura plena en los 24 departamentos del territorio peruano (REQ-RES-001).                   |
|  * Cumplimiento auditado de ventana de actualización de estados de hasta 30 min (REQ-RNF-001).    |
|  * Canal formal de retornos por logística inversa (CMP-NOT-06) hacia Almacén Lurín (ACT-03).      |
|  * Telemetría en tiempo real (CMP-AUD-04) y certificación final de Seguridad Informática (ACT-11). |
+----------------------------------------------------------------------------------------------------+
```

#### 1. Arquitectura de Transición 1 (AT-1): Núcleo MTD, Gobernanza y Plataforma Interna
* **Horizonte Temporal Referencial:** Semanas Relativas S1 a S12.
* **Paquetes y Componentes Involucrados:**
  * `PKG-01` en su totalidad: Publicación de especificaciones de contratos canónicos (`INT-DAT-01` a `INT-DAT-08`) y despliegue del clúster de API Gateway en DMZ corporativa con soporte TLS 1.3 y mTLS.
  * `PKG-02` en su totalidad: Despliegue del clúster de microservicios del `MTD` (`CMP-ING-01`, `CMP-COR-02`, `CMP-VAL-03`, `CMP-PUB-05`) y aprovisionamiento del repositorio `DB-EVENTSTORE` (partición Hot Data y archivo histórico).
  * `PKG-03` (Fase A - Conectividad Interna Lurín): Implementación del agente emisor `ADP-SPY` en el CD Lurín para capturar el `Estado 2: Elaboración o en preparación` con marca in situ $T_0$, y despliegue del conector de solo lectura `ADP-SAP` para resolución de metadatos de cajas master mono SKU.
* **Capacidades Habilitadas en AT-1:**
  * Capacidad interna de ingesta, normalización, deduplicación mediante firma hash SHA-256 y almacenamiento inmutable de eventos.
  * Resolución lógica de la relación `Código UA` $\leftrightarrow$ líneas de picking $\leftrightarrow$ `Número de Pedido` (`REQ-ARQ-002`).
  * Emisión estandarizada de eventos desde las líneas de empaque de Lurín hacia el `Bus de Integración` (`SIS-05`).
* **Condiciones de Entrada:** Aprobación del Contrato de Arquitectura por el Architecture Board (`ACT-12`, `ACT-13`) y asignación de entornos de cómputo en la nube corporativa.
* **Condiciones de Salida (Gate 1 — Hito M1):**
  1. Certificación por Seguridad Informática (`ACT-11`) de las configuraciones perimetrales del API Gateway.
  2. Verificación y auditoría de Almacén Lurín (`ACT-03`) certificando que `ADP-SAP` ejecuta exclusivamente consultas RFC de lectura sin privilegios de escritura (`REQ-RES-002`).
  3. Pruebas de integración interna exitosas al 100% entre `SPY`, el MTD y el Bus de Integración.

#### 2. Arquitectura de Transición 2 (AT-2): Integración Externa de Transporte, Resiliencia y Piloto
* **Horizonte Temporal Referencial:** Semanas Relativas S11 a S18 (solapamiento planificado de 2 semanas con el cierre de AT-1).
* **Paquetes y Componentes Involucrados:**
  * `PKG-04` en su totalidad: Conector de ingesta `ADP-LOG`, librería cliente móvil con buffer local cifrado (*Store-and-Forward* sobre SQLite) desplegada en los dispositivos de los transportistas asociados (`ACT-07`), y captura de marcas $T_0$ en los eventos `Pedido en ruta` (Estado 4) y `Pedido entregado` (Estado 5).
  * `PKG-03` (Fase B - Canales de Consumo): Conector `ADP-CRM` para sincronizar eventos consolidados hacia `CELLFORCE` (`SIS-09`) y enlace de tópicos hacia `ENSDY` (`SIS-07`).
* **Capacidades Habilitadas en AT-2:**
  * Ingesta segura de eventos de ruta emitidos por plataformas externas (`DRIVING`, `NSDG`) y aplicativos móviles de terceros.
  * Resiliencia operativa ante zonas de sombra celular en carreteras nacionales mediante persistencia offline y transmisión en ráfaga (`ASM-16`, `RSK-06`).
  * Despliegue piloto controlado en Lima Metropolitana y rutas troncales principales, permitiendo a Servicio al Cliente (`ACT-08`) visualizar estatus actualizados con latencia significativamente reducida.
* **Condiciones de Entrada:** Superación satisfactoria de los criterios del Gate 1.
* **Condiciones de Salida (Gate 2 — Hito M2):**
  1. Verificación de cero pérdida de eventos en pruebas de desconexión y reconexión celular (*Store-and-Forward*).
  2. Cumplimiento de la ventana de actualización $\le 30\text{ min}$ en al menos el 95% de los eventos del grupo piloto (`OBJ-02`, `DEC-05`).
  3. Aprobación de usabilidad operativa por los conductores y supervisores de transporte (`ACT-06`, `ACT-07`).

#### 3. Arquitectura Objetivo Final (Target TO-BE): Despliegue Pleno Nacional y Cierre de Gobierno
* **Horizonte Temporal Referencial:** Semanas Relativas S17 a S24 (estabilización y pase a producción general).
* **Paquetes y Componentes Involucrados:**
  * `PKG-05` en su totalidad: Componente de Gestión de Incidencias (`CMP-NOT-06`), canal formal de notificación hacia la consola de Almacén Lurín para logística inversa (`REQ-FUN-005`), motor de telemetría de SLAs (`CMP-AUD-04`) calculando $\Delta T = T_1 - T_0$ en tiempo real, tableros de control operativo para Distribución (`ACT-06`) y certificación formal de Seguridad Informática (`ACT-11`).
* **Capacidades Consolidadas en TO-BE:**
  * Cobertura integral del despacho B2B hacia los 24 departamentos del territorio peruano (`REQ-RES-001`).
  * Monitoreo telemático continuo del cumplimiento de la meta de actualización de estados de hasta 30 minutos (`REQ-RNF-001`).
  * Circuito formal de retorno ante siniestros o entregas fallidas, alertando a Almacén Lurín para inspección física y registro manual en `SAP R3` sin intervención del MTD (`REQ-RES-002`).

### 7.1.3. Mecanismos de continuidad operacional y planes de reversión (*Rollback*)

Para garantizar que ningún incidente técnico paralice el despacho físico de cosméticos en Lurín, se establecen los siguientes mecanismos de contingencia:
1. **Desacoplamiento Operativo de las Líneas de Embalaje:** Si el agente `ADP-SPY` o el API Gateway experimentan una indisponibilidad, las estaciones de picking y pesaje en Lurín continúan operando localmente en modo autónomo. `SPY` almacena los eventos en su base de datos local y los sincroniza en diferido una vez reestablecido el canal, evitando la detención física de los operarios (`PAE-07`).
2. **Procedimiento de Reversión por Paquete (*Rollback*):**
   * *Reversión en AT-1:* En caso de inestabilidad crítica en los microservicios del MTD, se desconecta la suscripción del Bus `SIS-05`, retornando a las consultas tradicionales sin afectar la facturación en `ZapCommerce` ni el inventario de `SAP R3`.
   * *Reversión en AT-2:* Si los aplicativos móviles de un socio logístico fallan, se mantiene temporalmente la recepción de manifiestos por los canales existentes (`DRIVING`/`NSDG`), aislando al proveedor afectado sin degradar la ingesta de los demás transportistas.
3. **Aislamiento Categórico de Datos de Inventario:** Dado que el MTD no tiene acceso de escritura a `SAP R3`, ante cualquier falla catastrófica de software, los saldos contables, estados de stock y movimientos de almacén permanecen 100% íntegros e inalterados.

---

## 7.2. Priorización multicriterio de proyectos (Costo-Beneficio-Riesgo)

La priorización de los cinco Paquetes de Trabajo (`PKG-01` a `PKG-05`) tiene como propósito determinar el orden óptimo de asignación de recursos, esfuerzo y secuencia técnica de ejecución. No debe confundirse con la evaluación de alternativas arquitectónicas de la Fase E (donde se compararon arquitecturas globales).

### 7.2.1. Criterios de evaluación y escala de ponderación

Se establecen ocho criterios de evaluación multicriterio cuyas ponderaciones porcentuales suman exactamente **100%**:

| Código Criterio | Criterio de Priorización Multicriterio | Peso (%) | Fundamento y Regla de Evaluación |
| :---: | :--- | :---: | :--- |
| **`C1`** | **Valor de Negocio y Alineación Estratégica** | **15%** | Grado de aporte directo a la visibilidad del despacho y certidumbre en la red B2B nacional. |
| **`C2`** | **Contribución a Objetivos SMART e Impacto en KPIs** | **15%** | Impacto directo en la reducción del desfase a $\le 30\text{ min}$ (`OBJ-02`, `KPI-01`, `KPI-02`). |
| **`C3`** | **Cierre de Brechas Arquitectónicas Críticas** | **10%** | Cantidad y severidad de brechas de la matriz consolidada de Fase E mitigadas por el paquete. |
| **`C4`** | **Costo y Esfuerzo Relativo de Implementación** | **10%** | Estimación cualitativa del consumo de recursos de ingeniería (Menor esfuerzo = Mayor puntaje). |
| **`C5`** | **Complejidad Técnica y Viabilidad de Ingeniería** | **10%** | Facilidad técnica de integración y madurez de patrones aplicados (Menor complejidad = Mayor puntaje). |
| **`C6`** | **Mitigación de Riesgo y Continuidad del Despacho** | **15%** | Capacidad del paquete para prevenir fallas operativas y garantizar el aislamiento de `SAP R3`. |
| **`C7`** | **Dependencias Previas y Habilitación de Otras Iniciativas** | **10%** | Rol como prerrequisito fundacional indispensable para habilitar los paquetes subsecuentes. |
| **`C8`** | **Urgencia y Criticidad Operativa** | **15%** | Necesidad inmediata de resolución frente a los dolores expresados por Distribución y Atención. |
| **TOTAL** | **Suma de Ponderaciones de Criterios** | **100%** | **Regla vinculante: Suma aritmética exacta al 100%.** |

**Escala de Calificación:**  
Cada criterio se evalúa de forma independiente en una escala ordinal del 1 al 5:
* **1 punto:** Muy bajo aporte / Muy alto costo o esfuerzo / Muy alta complejidad técnica o riesgo.
* **2 puntos:** Aporte bajo / Esfuerzo considerable / Complejidad media-alta.
* **3 puntos:** Aporte moderado / Esfuerzo y complejidad moderados.
* **4 puntos:** Alto aporte / Esfuerzo acotado / Alta viabilidad técnica y bajo riesgo.
* **5 puntos:** Excelente aporte estratégico / Bajo esfuerzo / Habilitador crítico indispensable.

**Fórmula de Puntuación Ponderada:**
$$\text{Puntuación Total}(PKG_i) = \sum_{j=1}^{8} \left( \text{Calificación}_{i,j} \times \text{Peso}_j \right)$$

### 7.2.2. Matriz formal de evaluación multicriterio individual y ponderada

A continuación se presenta la evaluación técnica rigurosa de cada paquete de trabajo:

| Paquete de Trabajo Canónico | C1 (15%) | C2 (15%) | C3 (10%) | C4 (10%) | C5 (10%) | C6 (15%) | C7 (10%) | C8 (15%) | Puntuación Ponderada Total | Orden de Prioridad |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`PKG-01: Gobernanza, Contratos Canónicos y API Gateway`** | 4.0 (0.60) | 4.0 (0.60) | 4.5 (0.45) | 4.0 (0.40) | 4.0 (0.40) | 5.0 (0.75) | 5.0 (0.50) | 4.5 (0.675) | **4.375 / 5.00** | **Prioridad 1 (Fundacional)** |
| **`PKG-02: Desarrollo y Despliegue de Microservicios MTD`** | 4.5 (0.675) | 4.5 (0.675) | 5.0 (0.50) | 3.5 (0.35) | 3.5 (0.35) | 4.5 (0.675) | 4.5 (0.45) | 4.5 (0.675) | **4.325 / 5.00** | **Prioridad 2 (Núcleo Lógico)** |
| **`PKG-03: Adaptadores y Conexión con Sistemas Internos`** | 4.5 (0.675) | 4.0 (0.60) | 4.0 (0.40) | 4.0 (0.40) | 4.0 (0.40) | 5.0 (0.75) | 3.5 (0.35) | 4.0 (0.60) | **4.175 / 5.00** | **Prioridad 3 (Integración Lurín/CRM)** |
| **`PKG-04: Socios Logísticos y Resiliencia en Ruta`** | 5.0 (0.75) | 5.0 (0.75) | 4.5 (0.45) | 2.5 (0.25) | 2.5 (0.25) | 3.5 (0.525) | 3.0 (0.30) | 5.0 (0.75) | **4.025 / 5.00** | **Prioridad 4 (Extensión Externa)** |
| **`PKG-05: Seguridad, Logística Inversa y SLAs`** | 4.0 (0.60) | 4.5 (0.675) | 4.0 (0.40) | 3.5 (0.35) | 4.0 (0.40) | 4.5 (0.675) | 2.0 (0.20) | 4.0 (0.60) | **3.900 / 5.00** | **Prioridad 5 (Cierre y Telemetría)** |

### 7.2.3. Justificación de la secuencia definitiva considerando dependencias obligatorias

El orden de prioridad cuantitativo resultante (`PKG-01` $\rightarrow$ `PKG-02` $\rightarrow$ `PKG-03` $\rightarrow$ `PKG-04` $\rightarrow$ `PKG-05`) coincide con la secuencia técnica natural de ingeniería de software e integración:

1. **`PKG-01` es el habilitador universal (Puntaje 4.375):**  
   Obtiene la máxima prioridad porque define las reglas del juego de todo el proyecto (contratos JSON, esquemas de seguridad TLS 1.3/mTLS y puerta de enlace API Gateway). Sin contratos canónicos normalizados, los microservicios de `PKG-02` y los adaptadores de `PKG-03` y `PKG-04` carecerían de especificación funcional.
2. **`PKG-02` construye el motor de procesamiento (Puntaje 4.325):**  
   Resuelve el procesamiento semántico de los 5 estados, la correlación de identificadores (`Código UA` $\leftrightarrow$ `Pedido`) y la persistencia de trazas inmutables. Debe existir antes de conectar fuentes externas.
3. **`PKG-03` estabiliza el entorno interno (Puntaje 4.175):**  
   Garantiza que la emisión de eventos desde el CD Lurín (`SPY`) y la sincronización con `CELLFORCE` operen sobre el Bus sin tocar `SAP R3`. Su ejecución temprana en Lurín reduce la incertidumbre antes de abrir interfaces a transportistas.
4. **`PKG-04` atiende el síntoma principal de transporte pero exige bases sólidas (Puntaje 4.025):**  
   Aunque aporta el mayor valor de cara a la reducción del desfase de 2 horas en ruta (máxima calificación en C1, C2 y C8), presenta mayor complejidad de coordinación y esfuerzo debido a la dispersión de transportistas externos (`ACT-07`). No puede ejecutarse antes de que el API Gateway (`PKG-01`) y el motor MTD (`PKG-02`) estén operativos.
5. **`PKG-05` formaliza el cierre de gobierno y control de SLAs (Puntaje 3.900):**  
   Constituye la fase de instrumentación fina, medición automatizada de latencia ($\Delta T$) y formalización de avisos de logística inversa hacia Almacén Lurín, culminando con la certificación final de Seguridad Informática.

---

## 7.3. Roadmap estratégico de implementación temporalizado

### 7.3.1. Ficha técnica del modelo de roadmap (`DIAG-09`)

* **Identificador Canónico:** `DIAG-09`
* **Denominación Oficial:** Diagrama del Roadmap Estratégico de Implementación y Arquitecturas de Transición
* **Estándar de Modelado:** OMG Unified Modeling Language (OMG UML) versión 2.5.1 / TOGAF Standard Phase F
* **Propósito Metodológico:** Representar visualmente la progresión temporal y lógica del plan de migración, vinculando las fases de transición (AT-1 y AT-2), las compuertas de decisión (*Gating*), los paquetes de trabajo involucrados y los hitos de entrega corporativos.
* **Archivo Fuente Editable:** `07_Fase_F_Planificacion_de_Migracion/diagramas/DIAG-09_Roadmap_Estrategico_Implementacion.puml`

### 7.3.2. Representación visual integrada del roadmap estratégico

```mermaid
gantt
    title DIAG-09 — Roadmap Estratégico de Implementación (Horizonte Referencial Semanas S1 a S24)
    dateFormat  X
    axisFormat  Sem %s

    section FASE DE GOBERNANZA Y BASE
    Formalización de Contratos y API Gateway (PKG-01) :active, p1, 1, 6
    Validación de Aislamiento SAP R3 con Almacén Lurín :crit, p1_sub, 1, 4

    section TRANSICIÓN 1 (AT-1)
    Desarrollo Suite Microservicios MTD y EventStore (PKG-02) :p2, 4, 12
    Adaptador Emisor SPY CD Lurín y Conector ADP-SAP (PKG-03A) :p3a, 6, 12
    HITO M1: Certificación Núcleo Interno Lurín / MTD (Gate 1) :milestone, m1, 12, 12

    section TRANSICIÓN 2 (AT-2)
    Integración TMS Socios y Store-and-Forward Móvil (PKG-04) :p4, 11, 18
    Adaptador CRM CELLFORCE y Tópicos Bus SIS-05 (PKG-03B) :p3b, 12, 16
    Despliegue Piloto Rutas Lima / Troncales Principales :p4_piloto, 15, 18
    HITO M2: Validación Resiliencia Offline y SLA Piloto (Gate 2) :milestone, m2, 18, 18

    section ARQUITECTURA OBJETIVO (TO-BE)
    Canal Logística Inversa hacia Almacén y Telemetría SLA (PKG-05) :p5, 17, 22
    Auditoría y Certificación Seguridad Informática (ACT-11) :crit, p5_sec, 20, 23
    Despliegue y Cobertura Nacional en los 24 Departamentos :p5_rollout, 21, 24
    HITO M3: Pase a Producción Plena y Cierre de Migración :milestone, m3, 24, 24
```

### 7.3.3. Descripción de etapas, hitos principales y compuertas de decisión (*Gates*)

El roadmap se articula en cuatro periodos estratégicos distribuidos a lo largo de 24 semanas relativas de trabajo:

| Etapa / Periodo | Ventana Temporal | Paquetes Activos | Hitos Principales y Entregables Clave | Compuerta de Decisión (*Gate Criteria*) |
| :--- | :---: | :---: | :--- | :--- |
| **Etapa 1: Cimientos y Gobernanza** | Semanas S1 a S6 | `PKG-01` | * Especificación aprobada de 8 contratos JSON.<br>* Despliegue de API Gateway en DMZ.<br>* Configuración de certificados mTLS y tokens. | **Aprobación de Contratos:** Visto bueno conjunto de TI (`ACT-12`) y Seguridad (`ACT-11`) para habilitar el desarrollo de microservicios. |
| **Etapa 2: Construcción de AT-1** | Semanas S4 a S12 | `PKG-02`, `PKG-03` *(Lurín)* | * Microservicios MTD en contenedores OCI.<br>* Repositorio `DB-EVENTSTORE` activo.<br>* Agente `ADP-SPY` capturando Estado 2 con $T_0$.<br>* Conector RFC de solo lectura `ADP-SAP`. | **GATE 1 (Hito M1 - Semana 12):**<br>Auditoría física de Almacén Lurín (`ACT-03`) ratificando cero escrituras sobre `SAP R3` (`REQ-RES-002`) y pruebas unitarias/integradas al 100%. |
| **Etapa 3: Integración Externa (AT-2)** | Semanas S11 a S18 | `PKG-04`, `PKG-03` *(CRM)* | * Adaptador `ADP-LOG` recibiendo webhooks.<br>* App móvil transportistas con SQLite offline.<br>* Adaptador `ADP-CRM` sincronizando `CELLFORCE`.<br>* Piloto operativo en Lima y rutas troncales. | **GATE 2 (Hito M2 - Semana 18):**<br>Comprobación empírica en piloto de latencia $\Delta T \le 30\text{ min}$ y verificación de cero pérdida de datos ante desconexión celular en ruta. |
| **Etapa 4: Cierre, Seguridad y TO-BE** | Semanas S17 a S24 | `PKG-05` | * Componente `CMP-NOT-06` alertando a Almacén.<br>* Motor `CMP-AUD-04` monitoreando SLA en vivo.<br>* Cobertura en los 24 departamentos.<br>* Auditoría formal de Seguridad Informática. | **HITO M3 (Semana 24 — Pase a Producción):**<br>Dictamen formal de Seguridad Informática (`ACT-11`) certificando cero vulnerabilidades y recepción formal de Distribución (`ACT-06`). |

---

## 7.4. Cronograma estimado de actividades por fases

En concordancia con el principio `PAE-08` y las cautelas de `ASM-14`, el siguiente cronograma de desglose de trabajo (*Work Breakdown Schedule*) expresa las duraciones y dependencias en **semanas relativas de proyecto**, estableciendo la ruta crítica y las actividades que se ejecutan en paralelo:

| Paquete | Código Actividad | Descripción Detallada de la Actividad de Implementación | Duración Estimada | Dependencias Obligatorias | Rol / Equipo Participante (`ACT-XX`) | Hito / Entregable Asociado |
| :---: | :---: | :--- | :---: | :---: | :--- | :--- |
| **`PKG-01`** | **ACT-P1-01** | Definición y formalización de esquemas JSON canónicos (`INT-DAT-01` a `08`). | 3 sem (S1–S3) | Inicio de Proyecto | `ACT-12` (TI), `ACT-14` (Arquitectura) | Catálogo de Contratos Aprobado |
| **`PKG-01`** | **ACT-P1-02** | Aprovisionamiento del clúster de API Gateway en zona DMZ con soporte TLS 1.3. | 3 sem (S2–S4) | ACT-P1-01 (parcial) | `ACT-12` (TI), `ACT-11` (Seguridad) | API Gateway Operativo en DMZ |
| **`PKG-01`** | **ACT-P1-03** | Configuración de políticas de seguridad, mTLS, WAF y emisión de credenciales. | 2 sem (S4–S5) | ACT-P1-02 | `ACT-11` (Seguridad Informática) | Perímetro Seguro Certificado |
| **`PKG-02`** | **ACT-P2-01** | Construcción de microservicios de Ingesta (`CMP-ING-01`) e Idempotencia SHA-256. | 4 sem (S4–S7) | ACT-P1-01 | `ACT-12` (TI / Fábrica de Software) | Servicio de Ingesta Desplegado |
| **`PKG-02`** | **ACT-P2-02** | Desarrollo del Motor de Validación Semántica (`CMP-VAL-03`) y 5 estados canónicos. | 4 sem (S6–S9) | ACT-P2-01 | `ACT-12` (TI), `ACT-06` (Distribución) | Validador de Reglas de Negocio |
| **`PKG-02`** | **ACT-P2-03** | Implementación del Módulo de Correlación Lógica (`CMP-COR-02`) y caché distribuida. | 4 sem (S7–S10)| ACT-P2-02 | `ACT-12` (TI) | Componente de Correlación Activo |
| **`PKG-02`** | **ACT-P2-04** | Aprovisionamiento de base de datos `DB-EVENTSTORE` (partición Hot Data y archivo). | 3 sem (S5–S7) | ACT-P1-01 | `ACT-12` (Administrador de BD) | Repositorio Canónico Disponible |
| **`PKG-03`** | **ACT-P3-01** | Desarrollo del conector de solo lectura `ADP-SAP` (RFC) y auditoría de permisos. | 3 sem (S5–S7) | ACT-P1-01 | `ACT-03` (Almacén), `ACT-12` (SAP Basis)| Acta de Cero Escritura en SAP |
| **`PKG-03`** | **ACT-P3-02** | Implementación del agente emisor `ADP-SPY` en estaciones de picking del CD Lurín. | 5 sem (S7–S11)| ACT-P1-01, ACT-P3-01 | `ACT-04` (Picking CD), `ACT-16` (SPY) | Emisión Estado 2 con marca $T_0$ |
| **`PKG-03`** | **ACT-P3-03** | Configuración de tópicos Pub/Sub en el Bus (`SIS-05`) y conector `ADP-CRM`. | 4 sem (S12–S15)| ACT-P2-02 | `ACT-12` (Middleware), `ACT-08` (CRM) | Tópicos Despacho Activos en Bus |
| — | **HITO-M1** | **Evaluación de Compuerta GATE 1: Certificación de Núcleo Interno AT-1.** | 1 sem (S12) | ACT-P2-03, ACT-P3-02 | `ACT-13` (Directivos), `ACT-11`, `ACT-03`| **Hito M1 Superado (Pase a AT-2)** |
| **`PKG-04`** | **ACT-P4-01** | Desarrollo del adaptador webhook `ADP-LOG` para plataformas `DRIVING` y `NSDG`. | 4 sem (S11–S14)| ACT-P1-03, ACT-P2-01 | `ACT-12` (TI), `ACT-16` (TMS Proveedor)| Conector Webhook Publicado |
| **`PKG-04`** | **ACT-P4-02** | Construcción e integración del módulo móvil *Store-and-Forward* con buffer offline. | 5 sem (S12–S16)| ACT-P4-01 | `ACT-16` (Fábrica Móvil), `ACT-07` | App Móvil con Persistencia Local |
| **`PKG-04`** | **ACT-P4-03** | Despliegue piloto en transportistas de Lima Metropolitana y rutas troncales. | 3 sem (S15–S17)| ACT-P4-02, ACT-P3-03 | `ACT-06` (Distribución), `ACT-07` | Piloto en Producción Controlada |
| — | **HITO-M2** | **Evaluación de Compuerta GATE 2: Certificación de Piloto y Resiliencia AT-2.** | 1 sem (S18) | ACT-P4-03 | `ACT-06`, `ACT-08`, `ACT-11`, `ACT-12` | **Hito M2 Superado (Pase a TO-BE)**|
| **`PKG-05`** | **ACT-P5-01** | Desarrollo del canal de notificación de retorno hacia Almacén Lurín (`CMP-NOT-06`). | 3 sem (S17–S19)| ACT-P3-01, ACT-P2-02 | `ACT-03` (Almacén), `ACT-06` (Distrib.) | Módulo de Logística Inversa Activo |
| **`PKG-05`** | **ACT-P5-02** | Implementación del motor de telemetría de SLAs (`CMP-AUD-04`) y alertas de desvío. | 4 sem (S18–S21)| ACT-P4-03, ACT-P2-01 | `ACT-12` (TI), `ACT-06` (Distribución) | Tableros de Control de Latencia |
| **`PKG-05`** | **ACT-P5-03** | Auditoría integral de vulnerabilidades, pruebas éticas y validación de Seguridad. | 3 sem (S20–S22)| ACT-P5-01, ACT-P5-02 | `ACT-11` (Seguridad Informática) | Informe de Cero Vulnerabilidades |
| **`PKG-05`** | **ACT-P5-04** | Despliegue masivo y enrolamiento de transportistas hacia los 24 departamentos. | 4 sem (S21–S24)| ACT-P5-03, HITO-M2 | `ACT-06` (Distribución), `ACT-07` | Cobertura Nacional B2B Activa |
| — | **HITO-M3** | **Pase a Producción General y Cierre de Migración de Arquitectura.** | 1 sem (S24) | ACT-P5-04 | `ACT-13` (Patrocinador), Todos Actores | **Hito M3: Arquitectura TO-BE** |

* **Ruta Crítica del Proyecto:**  
  $$\text{ACT-P1-01} \rightarrow \text{ACT-P1-02} \rightarrow \text{ACT-P1-03} \rightarrow \text{ACT-P4-01} \rightarrow \text{ACT-P4-02} \rightarrow \text{ACT-P4-03} \rightarrow \text{HITO-M2} \rightarrow \text{ACT-P5-04} \rightarrow \text{HITO-M3}$$  
  Duración mínima requerida: **24 semanas relativas**.
* **Actividades Ejecutables en Paralelo:**  
  * El desarrollo de la suite MTD (`ACT-P2-01` a `ACT-P2-04`) y los adaptadores de Lurín (`ACT-P3-01`, `ACT-P3-02`) se ejecutan en paralelo a partir de la semana S4 una vez formalizados los contratos en `ACT-P1-01`.
  * La configuración del canal de logística inversa (`ACT-P5-01`) se desarrolla en paralelo con las pruebas del piloto de transporte (`ACT-P4-03`).

---

## 7.5. Asignación de recursos, responsabilidades y gestión de dependencias

### 7.5.1. Matriz de Responsabilidades RACI

En estricta conformidad con el Catálogo Maestro de Actores (`00_2`), la siguiente matriz asigna los roles canónicos para cada paquete de trabajo y actividad crítica, distinguiendo: **R** (*Responsible* / Responsable de ejecutar), **A** (*Accountable* / Responsable final y aprobador), **C** (*Consulted* / Consultado) e **I** (*Informed* / Informado):

| Código y Paquete de Trabajo / Hito | `ACT-03` Almacén | `ACT-04` Picking | `ACT-05` Despacho | `ACT-06` Distrib. | `ACT-07` Socios | `ACT-08` CRM | `ACT-11` Seguridad | `ACT-12` TI / Arq. | `ACT-13` Dirección | `ACT-14` Consultor | `ACT-16` Proveed. |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`PKG-01: Gobernanza y API Gateway`** | I | I | I | C | C | I | **A** | **R** | I | C | C |
| - *Aprobación de Contratos JSON Canónicos* | I | I | I | C | C | I | C | **A / R** | I | R | C |
| - *Despliegue y Pruebas Perimetrales DMZ* | I | I | I | I | I | I | **A** | **R** | I | C | I |
| **`PKG-02: Microservicios MTD y EventStore`** | I | I | I | C | I | I | C | **A / R** | I | C | C |
| - *Desarrollo CMP-ING, COR, VAL, PUB* | I | I | I | C | I | I | I | **R** | I | C | I |
| - *Aprovisionamiento DB-EVENTSTORE* | I | I | I | I | I | I | C | **A / R** | I | I | I |
| **`PKG-03: Adaptadores y Conexión Interna`** | **A** | R | R | C | I | C | C | **R** | I | C | R |
| - *Auditoría de Cero Escritura en SAP R3* | **A / R** | I | I | C | I | I | C | **R** | I | C | I |
| - *Despliegue ADP-SPY en CD Lurín* | C | **R** | C | C | I | I | I | **A** | I | C | R |
| - *Conexión Bus SIS-05 hacia CELLFORCE* | I | I | I | I | I | **A / R** | I | **R** | I | C | C |
| **HITO-M1: Certificación AT-1 (Gate 1)** | **A** | C | C | C | I | I | **A** | **A** | **A** | C | I |
| **`PKG-04: Socios Logísticos y Resiliencia`** | I | I | C | **A** | **R** | I | C | **R** | I | C | **R** |
| - *Despliegue Webhook ADP-LOG para TMS* | I | I | I | C | C | I | C | **A / R** | I | C | R |
| - *Librería Store-and-Forward en App Móvil*| I | I | I | C | **R** | I | C | C | I | C | **R** |
| - *Ejecución de Piloto en Lima y Rutas* | I | I | C | **A / R** | **R** | C | I | C | I | C | C |
| **HITO-M2: Certificación AT-2 (Gate 2)** | I | I | C | **A** | C | **A** | **A** | **A** | **A** | C | I |
| **`PKG-05: Seguridad, Retornos y SLAs`** | **A** | I | I | **A** | C | C | **A** | **R** | I | C | C |
| - *Canal Notificación Retorno a Lurín* | **A / R** | I | I | **R** | C | C | I | C | I | C | I |
| - *Telemetría SLA Delta T <= 30 min* | I | I | I | **A** | C | C | I | **R** | I | C | I |
| - *Auditoría de Seguridad Informática* | I | I | I | I | I | I | **A / R** | C | I | C | I |
| - *Despliegue Nacional en 24 Departamentos*| I | I | C | **A / R** | **R** | C | I | C | **A** | C | C |
| **HITO-M3: Cierre de Migración (TO-BE)** | **A** | **A** | **A** | **A** | C | **A** | **A** | **A** | **A** | C | C |

### 7.5.2. Matriz de dependencias y precondiciones cruzadas

| Paquete / Iniciativa | Precondiciones Técnicas Obligatorias | Precondiciones Organizacionales y de Gobierno | Entregables Habilitantes Requeridos |
| :--- | :--- | :--- | :--- |
| **`PKG-01`** | Aislamiento de red para DMZ corporativa; certificados TLS corporativos vigentes. | Aprobación de la política de interoperabilidad por TI (`ACT-12`) y Seguridad (`ACT-11`). | Declaración de Trabajo y Ficha de Contrato de Arquitectura. |
| **`PKG-02`** | Clúster de contenedores OCI provisionado; almacenamiento SSD para EventStore. | Aprobación del esquema de datos canónico (`ENT-07`) por el equipo de datos. | Esquemas JSON de `PKG-01` validados. |
| **`PKG-03`** | Acceso a ambiente de pruebas de `SPY` y `SAP R3` (RFC de lectura); colas en Bus `SIS-05`. | Autorización formal expresa del Área de Almacén Lurín (`ACT-03`) para pruebas de lectura RFC. | Contratos `INT-DAT-01` a `04` aprobados. |
| **`PKG-04`** | Endpoints de API Gateway accesibles desde internet; API REST/Webhook en TMS de socios. | Convenios de colaboración técnica suscritos con las empresas transportistas (`ACT-07`). | Endpoint seguro de `PKG-01` y motor MTD de `PKG-02`. |
| **`PKG-05`** | Piloto en ruta operando con flujo continuo de eventos; enlaces VPN activos con Lurín. | Protocolo de recepción física y regularización de retornos coordinado entre Distribución y Almacén. | Flujo de eventos de `PKG-04` y compuerta Gate 2 aprobada. |

---

## 7.6. Matriz formal de riesgos de transformación empresarial y planes de mitigación

Se integran los riesgos técnicos identificados en las Fases A y D (`RSK-01` a `RSK-06`) y se formulan cuatro riesgos específicos de transformación y migración (`RSK-07` a `RSK-10`), evaluados bajo escalas de probabilidad e impacto (Baja, Media, Alta):

| Código | Riesgo de Transformación | Causa Raíz Identificada | Paquete Afectado | Prob. | Imp. | Nivel de Riesgo | Plan de Mitigación Preventiva | Plan de Contingencia Operativa | Responsable (`ACT-XX`) | Indicador de Monitoreo |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :--- | :--- | :---: | :--- |
| **`RSK-01`** | Imposibilidad de medir con precisión la reducción del desfase temporal. | Carencia de marcas $T_0/T_1$ en los sistemas legados actuales (`ASM-03`). | `PKG-01`, `PKG-02` | Media | Alta | **Alto** | Instrumentar el sellado $T_0$ en emisores y $T_1$ automático en `CMP-AUD-04`. | Muestreo manual controlado en piloto para calibrar la línea base empírica (`OBJ-01`). | `ACT-12` (TI), `ACT-06` (Distribución) | % de eventos con marcas $T_0$ y $T_1$ válidas. |
| **`RSK-02`** | El desfase persiste por demoras exclusivas del transportista externo. | Procesos internos lentos o batch de los socios logísticos (`ASM-07`). | `PKG-04`, `PKG-05` | Alta | Alta | **Crítico** | Endpoint API REST de baja latencia y monitoreo de SLA individual por transportista. | Aplicación de penalizaciones o cláusulas de nivel de servicio en contratos de transporte. | `ACT-06` (Distribución), `ACT-07` (Socios) | $\Delta T$ promedio desglosado por transportista. |
| **`RSK-03`** | Desalineación de interfaces por confusión entre `NSDG`, `ENSDY` y `DRIVING`. | Ambigüedad en la identidad de sistemas satélites (`ASM-02`, `ASM-07`). | `PKG-03`, `PKG-04` | Media | Media | **Medio** | Validación nominal formal en etapa S1 con TI y contratistas antes de codificar. | Uso de adaptadores desacoplados (`ADP-LOG`) que aíslan el dialecto del sistema subyacente. | `ACT-12` (TI), `ACT-14` (Arquitectura) | Actas de conformidad técnica de interfaces. |
| **`RSK-04`** | Observaciones o rechazo de interfaces por Seguridad Informática. | Incumplimiento de políticas de cifrado, roles o exposición perimetral (`REQ-RNF-003`). | `PKG-01`, `PKG-05` | Media | Alta | **Alto** | Involucrar a Seguridad (`ACT-11`) desde el diseño de `PKG-01` bajo estándar TLS 1.3/mTLS. | Subsanación inmediata en ventana de mitigación previo al Gate 1. | `ACT-11` (Seguridad Informática) | N.° de vulnerabilidades altas/críticas (Meta: 0). |
| **`RSK-05`** | Intrusión accidental de flujos de reparto B2C hacia consumidoras finales. | Confusión sobre el rol de consultoras o directoras en entregas (`ASM-01`). | `PKG-01`, `PKG-04` | Baja | Media | **Bajo** | Regla de filtrado estricto en API Gateway: solo números de pedido y guías B2B (`REQ-RES-001`). | Descarte automático en `CMP-VAL-03` de códigos de entrega capilar no empresarial. | `ACT-06` (Distribución), `ACT-14` | Auditoría de números de pedido procesados. |
| **`RSK-06`** | Pérdida o acumulación desordenada de eventos en rutas sin red celular. | Geografía nacional accidentada en tramos de sierra y selva (`ASM-16`). | `PKG-04` | Alta | Alta | **Crítico** | Módulo móvil con buffer offline (*Store-and-Forward*) y base de datos local SQLite. | Reordenamiento automático en `CMP-VAL-03` según marca inmutable $T_0$ tras sincronización. | `ACT-07` (Socios Logísticos), `ACT-12` | Tasa de eventos sincronizados en ráfaga sin error. |
| **`RSK-07`** | Resistencia operativa de tripulaciones de transporte a la nueva app. | Sobrecarga de digitación o interfaces móviles complejas (`REQ-RNF-004`). | `PKG-04` | Media | Alta | **Alto** | Diseño ergonómico de 2 toques: escaneo de código de bulto y selección rápida de estado. | Capacitación obligatoria in situ y soporte telefónico de Distribución durante piloto. | `ACT-06` (Distribución), `ACT-07` (Socios) | Tasa de adopción de la app móvil por transportista. |
| **`RSK-08`** | Saturación del Bus corporativo durante picos de cierre de campaña. | Crecimiento exponencial de órdenes en pocos días comerciales (`REQ-RNF-002`). | `PKG-02`, `PKG-03` | Media | Alta | **Alto** | Clúster de contenedores autoescalable y dimensionamiento elástico con TI (`ASM-10`). | Priorización de colas de despacho sobre eventos comerciales de menor criticidad. | `ACT-12` (TI / Middleware) | Tiempo de respuesta y uso de memoria en el Bus. |
| **`RSK-09`** | Desincronización de datos maestros (`Código UA`, SKU, destinos). | Cambios de catálogo en `SAP R3` no replicados oportunamente en el MTD. | `PKG-02`, `PKG-03` | Baja | Media | **Medio** | Sincronización diaria automatizada de tablas maestras y validación referencial estricta. | Enrutamiento a cola DLQ de eventos con códigos huérfanos para reprocesamiento. | `ACT-12` (TI), `ACT-03` (Almacén) | Cantidad de eventos derivados a cola de error (DLQ). |
| **`RSK-10`** | Retraso o interrupción en líneas de empaque al desplegar `ADP-SPY`. | Fallas de software durante la puesta en producción del agente en CD Lurín. | `PKG-03` | Baja | Alta | **Alto** | Despliegue en ventana nocturna de fin de semana; pruebas previas en ambiente réplica. | Rollback inmediato al ejecutable anterior de `SPY` en menos de 15 minutos sin impacto. | `ACT-04` (Picking CD), `ACT-16` (SPY) | Disponibilidad operativa del sistema `SPY` en CD. |

---

## 7.7. Plan de validación, pruebas de carga y sincronización de datos maestros

### 7.7.1. Marco General del Plan de Pruebas

El plan de validación establece la metodología empírica para comprobar que cada arquitectura de transición y la arquitectura final cumplen rigurosamente con los requerimientos funcionales, atributos de calidad y restricciones operativas antes de autorizar su pase a producción:

```
+----------------------------------------------------------------------------------------------------+
|                         MARCO INTEGRAL DEL PLAN DE PRUEBAS DE ARQUITECTURA                         |
+----------------------------------------------------------------------------------------------------+
|  A. PRUEBAS FUNCIONALES Y DE INTEGRACIÓN    |  B. PRUEBAS DE CARGA Y RENDIMIENTO                   |
|  * Secuencia canónica de 5 estados          |  * Simulación de carga normal y picos de campaña     |
|  * Correlación UA - Picking - Pedido        |  * Medición empírica de latencia Delta T = T1 - T0   |
|  * Interoperabilidad Pub/Sub sobre Bus      |  * Pruebas de estrés y autoescalado de contenedores  |
|                                             |                                                      |
|---------------------------------------------+------------------------------------------------------|
|  C. SINCRONIZACIÓN Y CALIDAD DE DATOS       |  D. RESILIENCIA Y SEGURIDAD INFORMÁTICA              |
|  * Integridad referencial y SoR respetado   |  * Pruebas de corte y reconexión (Store-and-Forward) |
|  * Filtro de duplicados (Idempotencia)      |  * Verificación de CERO escrituras sobre SAP R3      |
|  * Cola de reprocesamiento (DLQ)            |  * Análisis de vulnerabilidades SAST/DAST y mTLS     |
+----------------------------------------------------------------------------------------------------+
```

#### A. Validación Funcional y de Integración
* **Prueba de Secuencia de Estados (`REQ-FUN-001`, `CMP-VAL-03`):** Se inyectan eventos simulando transiciones válidas e inválidas (p. ej., intentar pasar a `Pedido entregado` sin registrar previamente `Pedido despachado`). Se verifica que el motor bloquee el salto fuera de orden y alerte la anomalía.
* **Prueba de Correlación Multidimensional (`REQ-ARQ-002`, `CMP-COR-02`):** Se verifica que, dado un `Número de Pedido`, el MTD retorne con precisión los códigos UA de procedencia y lotes de envasado registrados en `SPY`.
* **Prueba de Aislamiento de `SAP R3` (`REQ-RES-002`, `ADP-SAP`):** Se auditan los logs de base de datos de SAP para corroborar que el conector `ADP-SAP` únicamente ejecute sentencias `SELECT` de lectura mediante RFC autorizadas, verificando que no exista ninguna transacción `INSERT`, `UPDATE` o `DELETE` originada por el MTD.

#### B. Validación de Rendimiento, Carga y Latencia
* **Medición Automatizada de Latencia ($\Delta T \le 30\text{ min}$ — `REQ-RNF-001`):**  
  Se compara el sello temporal de emisión in situ ($T_0$) contra el sello de persistencia central ($T_1$) en `CMP-AUD-04`. El criterio de aceptación exige que en condiciones normales de red el 100% de los eventos procesados presenten un $\Delta T \le 30\text{ minutos}$, y que en pruebas piloto con enlaces móviles se alcance al menos el **98% de cumplimiento** (`DEC-05`, `KPI-02`), registrando alertas justificadas para el margen restante.
* **Pruebas de Carga bajo Picos de Campaña (`REQ-RNF-002`, `ASM-10`):**  
  Respetando `ASM-10` (no dimensionar el software sobre los 2 millones de cajas de almacenamiento físico de Lurín), se coordinará con TI (`ACT-12`) el levantamiento de la tasa histórica real de transacciones por segundo (TPS) en días de cierre comercial. Se someterá el clúster a una carga sintética del **200% del pico histórico** durante 4 horas continuas, verificando que la latencia interna no se degrade y que los contenedores escalen automáticamente.

#### C. Validación de Sincronización y Calidad de Datos
* **Verificación de Sistemas de Registro (*System of Record* - SoR):** Se comprueba que cada entidad respete su autoridad soberana: `SAP R3` para stock de productos terminados, `SPY` para pesos y tareas de picking, `Maya`/`ZapCommerce` para órdenes comerciales y el MTD para trazas de eventos (`ENT-07`).
* **Prueba de Idempotencia y Deduplicación:** Se transmiten eventos duplicados con la misma firma hash SHA-256. Se verifica que el API Gateway retorne acuse conforme `202 Accepted` al emisor pero que el motor `CMP-ING-01` descarte la persistencia repetida, evitando dobles transiciones de estado.

#### D. Validación de Resiliencia y Seguridad
* **Simulación de Pérdida de Cobertura (*Store-and-Forward* — `ASM-16`, `RSK-06`):**  
  En un trayecto de prueba entre Lima y una provincia de la sierra central, se desconecta la interfaz de red del dispositivo móvil durante 60 minutos mientras el operador registra dos cambios de estado. Al reconectar la red celular, se verifica que la app transmita la ráfaga acumulada sin corrupción, que `CMP-AUD-04` registre la hora de llegada $T_1$ identificando la causa geográfica y que el historial conserve la hora original $T_0$.
* **Auditoría de Vulnerabilidades y Cifrado (`REQ-RNF-003`, `OBJ-07`):**  
  El Área de Seguridad Informática (`ACT-11`) ejecuta escaneos SAST/DAST sobre los microservicios y pruebas de penetración (*pentesting*) sobre el API Gateway, exigiendo como condición de pase **0 vulnerabilidades críticas o altas no mitigadas**. Se valida que toda sesión externa rechace conexiones TLS inferiores a la versión 1.3.

### 7.7.2. Matriz de Pruebas y Criterios de Aceptación Formal

| Código Requisito | Objetivo SMART / KPI | Paquete / Componente | Escenario de Prueba Diseñado | Resultado Esperado | Evidencia Requerida | Responsable de Aprobación | Criterio de Aceptación Formal |
| :---: | :---: | :---: | :--- | :--- | :--- | :---: | :--- |
| **`REQ-NEG-001`** | `OBJ-03`<br>`KPI-03` | `PKG-02`<br>`CMP-VAL-03` | Transición ordenada de pedido desde facturación hasta entrega en destino B2B. | Secuencia estricta por los 5 estados sin saltos ni inconsistencias. | Log de traza de auditoría del pedido. | `ACT-06` (Distribución) | 100% de pedidos auditados cumplen la secuencia canónica. |
| **`REQ-NEG-002`** | `OBJ-02`<br>`KPI-01`, `02` | `PKG-04`, `PKG-05`<br>`CMP-AUD-04` | Evento capturado en ruta y propagado al sistema corporativo. | Intervalo $\Delta T = T_1 - T_0$ menor o igual a 30 minutos. | Reporte telemático de marcas temporales. | `ACT-06` (Distribución) | $\Delta T \le 30\text{ min}$ en $\ge 98\%$ de eventos piloto (`DEC-05`). |
| **`REQ-NEG-003`** | `OBJ-05`<br>`KPI-05` | `PKG-03`<br>`ADP-CRM` | Consulta de estatus de despacho por un agente en `CELLFORCE` (`SIS-09`). | Visualización de estado en ruta actualizado en $\le 30\text{ min}$ tras el evento. | Captura de pantalla y traza de API en CRM. | `ACT-08` (Servicio Cliente) | Disponibilidad oportuna en $\ge 98\%$ de consultas auditadas. |
| **`REQ-NEG-004`** | `OBJ-06`<br>`KPI-06` | `PKG-05`<br>`CMP-NOT-06` | Registro de entrega fallida por transportista e inicio de logística inversa. | Notificación estructurada emitida hacia Almacén Lurín (`ACT-03`). | Mensaje recibido en cola de Almacén Lurín. | `ACT-03` (Almacén Lurín) | 100% de incidencias de retorno formalizadas en sistema. |
| **`REQ-FUN-001`** | `OBJ-03`<br>`KPI-03` | `PKG-02`<br>`CMP-VAL-03` | Intento de inyectar evento `ENTREGADO` sobre un pedido en estado `PREPARADO`. | Rechazo del evento con código de error de secuencia inválida. | Log de error y desvío a cola de anomalías. | `ACT-12` (TI / Arquitectura) | 0 transiciones fuera de orden admitidas en producción. |
| **`REQ-FUN-005`** | `OBJ-06`<br>`KPI-06` | `PKG-05`<br>`CMP-NOT-06` | Llegada física de pedido devuelto a la garita de Almacén Lurín. | El operador inspecciona físicamente y registra el ajuste en `SAP R3`. | Documento de material generado en SAP R3. | `ACT-03` (Almacén Lurín) | Regularización soberana en SAP sin escrituras externas. |
| **`REQ-INT-001`** | `OBJ-04`<br>`KPI-04` | `PKG-03`<br>`ADP-SAP`, `SPY` | Descenso de orden aprobada y correlación con `Código UA` en picking. | `SPY` vincula UA de origen con las líneas de pedido y emite aviso. | Tabla asociativa en `SPY` y evento emitido. | `ACT-04` (Picking CD) | Trazabilidad lógica completa entre UA y Pedido. |
| **`REQ-INT-002`** | `OBJ-02`<br>`KPI-01` | `PKG-04`<br>`ADP-LOG` | Publicación de webhook desde plataforma TMS de transportista (`ACT-07`). | Recepción en API Gateway, respuesta `202 Accepted` en $< 200\text{ ms}$. | Log de acceso HTTP del API Gateway. | `ACT-12` (TI / Middleware) | Tiempo de respuesta del Gateway $\le 200\text{ ms}$. |
| **`REQ-RNF-001`** | `OBJ-02`<br>`KPI-01` | `PKG-02`<br>`CMP-AUD-04` | Carga de 10,000 eventos concurrentes en prueba de rendimiento. | Procesamiento en memoria sin colas acumuladas; $\Delta T$ preservado. | Métricas de rendimiento de microservicios. | `ACT-12` (TI) | Latencia de procesamiento interno $< 1\text{ segundo}$. |
| **`REQ-RNF-002`** | `OBJ-08`<br>`KPI-08` | `PKG-02`<br>`Clúster MTD` | Simulación del 200% del pico de campaña comercial de Yanbal. | Clúster escala pods automáticamente; cero pérdida de eventos. | Gráfica de autoescalado y cola del Bus. | `ACT-12` (Infraestructura) | 0 mensajes perdidos; tasa de éxito $\ge 99.9\%$. |
| **`REQ-RNF-003`** | `OBJ-07`<br>`KPI-07` | `PKG-01`, `PKG-05`<br>`API Gateway` | Escaneo SAST/DAST y pruebas de penetración sobre la plataforma. | Cero vulnerabilidades críticas o altas; cifrado TLS 1.3 forzado. | Dictamen formal de Seguridad Informática. | `ACT-11` (Seguridad Informática) | Conformidad técnica al 100% de controles de seguridad. |
| **`REQ-RNF-004`** | `OBJ-04`<br>`FCE-05` | `PKG-03`<br>`Estación Lurín` | Operario en CD embala pedido y pesa caja en báscula industrial. | Captura automática de peso y bulto sin exigir doble digitación. | Inspección de puesto de trabajo en planta. | `ACT-04` (Picking CD) | 0 campos duplicados de digitación manual en empaque. |
| **`REQ-RES-001`** | `OBJ-04`<br>`DEC-04` | `PKG-01`<br>`API Gateway` | Intento de enviar evento con código de destino de consumidora final. | Filtrado perimetral por regla B2B; mensaje bloqueado. | Log de filtrado de reglas en API Gateway. | `ACT-06` (Distribución) | 100% de operaciones circunscritas a frontera B2B. |
| **`REQ-RES-002`** | `OBJ-07`<br>`PAE-05` | `PKG-03`<br>`ADP-SAP` | Inspección de permisos de usuario de servicio de base de datos SAP. | Usuario configurado exclusivamente con rol de consulta RFC (`S_RFC`). | Reporte de auditoría de autorizaciones SAP. | `ACT-03` (Almacén Lurín) | Cero permisos de modificación/escritura en `SAP R3`. |
| **`REQ-RES-004`** | `OBJ-02`<br>`ASM-05` | `PKG-04`<br>`App Móvil` | Verificación de arquitectura de comunicación con transportistas. | Seguimiento basado en eventos de estado; sin hardware GPS continuo. | Especificación técnica de la solución. | `ACT-14` (Arquitectura) | Cumplimiento del modelo de transporte tercerizado. |

---

## Referencias Bibliográficas (Norma APA 7.ª Edición)

* Drivin. (s. f.). *Yanbal Perú: 100% de visibilidad para su logística*. Recuperado de https://driv.in/casos-de-exito/yanbal-peru
* Institute of Electrical and Electronics Engineers. (2018). *ISO/IEC/IEEE 29148:2018 Systems and software engineering — Life cycle processes — Requirements engineering*. IEEE Standards Association. https://standards.ieee.org/ieee/29148/7212/
* Object Management Group. (2017). *OMG Unified Modeling Language (OMG UML) Specification, Version 2.5.1*. Object Management Group. https://www.omg.org/spec/UML/2.5.1/
* Project Management Institute. (2021). *A guide to the project management body of knowledge (PMBOK guide)* (7.ª ed.). Project Management Institute.
* Rescorla, E. (2018). *The Transport Layer Security (TLS) Protocol Version 1.3* (RFC 8446). Internet Engineering Task Force (IETF). https://doi.org/10.17487/RFC8446
* The Open Group. (2018). *The TOGAF® Standard, Version 9.2*. Van Haren Publishing. https://www.opengroup.org/togaf
* The Open Group. (2022). *The TOGAF® Standard, 10th Edition — Architecture Development Method (ADM)*. Van Haren Publishing. https://www.opengroup.org/togaf-standard-10th-edition
* Unique S.A. / Yanbal Perú. (s. f.-a). *Nuestra Historia corporativa*. Portal oficial de Yanbal Perú. Recuperado de https://www.yanbal.com/pe/corporate/nuestra-historia
* Unique S.A. / Yanbal Perú. (s. f.-b). *Instalaciones y Canales de Contacto Oficiales*. Portal corporativo de Yanbal Perú. Recuperado de https://www.yanbal.com/pe/corporate/contactanos

