# EVALUACIÓN DE CORRESPONDENCIA DOCUMENTAL Y SEMÁNTICA DE DIAGRAMAS UML
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Análisis de Representación, Alcance y Ubicación de los Modelos Arquitectónicos (DIAG-01 a DIAG-11)

---

## 1. Introducción y Enfoque Metodológico

El presente documento constituye la entrega actualizada de **REV_04: Evaluación de Correspondencia Documental y Semántica de Diagramas UML**, elaborada conforme a las directrices de normalización editorial y rigor metodológico.

### 1.1. Propósito de la Evaluación
Determinar para cada uno de los modelos visuales existentes en el repositorio:
1. Qué tipo de diagrama UML (estándar OMG UML 2.5.1) representa legítimamente.
2. Qué muestra conceptualmente y qué decisiones arquitectónicas comunica.
3. Cuál es su procedencia documental exacta (sección fuente, tablas y requerimientos).
4. En qué apartado específico del informe corresponde incorporarlo.
5. Cuáles son los límites estrictos de fidelidad que impiden inventar elementos o relaciones no respaldadas.

> **Regla Metodológica Central:** No se generan instrucciones para dibujar ni fichas de construcción manual. El propósito es validar la correspondencia documental estricta de cada modelo antes de su representación visual definitiva.

---

## 2. Evaluación Individual de Correspondencia Documental (DIAG-01 a DIAG-11)

---

### 2.1. DIAG-01: Visión Conceptual de la Capacidad MTD y Límites del Sistema
- **Identificador Canónico:** `DIAG-01`
- **Tipo y Categoría UML:** **Diagrama de Estructura Compuesta / Diagrama de Paquetes** (Categoría Estructural).  
  *Nota sobre límites de notación:* El modelo original es un diagrama de bloques/flujo conceptual (Mermaid). En UML 2.5.1 se encuadra como una **Estructura Compuesta** que modela los subsistemas de Almacén, el MTD y los Canales como partes conectadas por puertos de interfaz.
- **Qué Muestra el Modelo:**
  - Las fuentes de eventos transaccionales en el Centro de Distribución de Lurín (`SAP R3 SIS-01`, `SPY SIS-02`) y frentes comerciales (`Maya SIS-03`, `ZapCommerce SIS-04`).
  - La capacidad central del MTD (`DEC-02`) articulada en módulos de ingesta, repositorio de eventos, motor de correlación y APIs de consulta.
  - La ingesta asíncrona de eventos de transporte desde socios logísticos externos (`ACT-07` / `SIS-06`, `SIS-07`, `SIS-08`).
  - La exposición de estados hacia canales de consulta (`ACT-01`, `ACT-08 CELLFORCE SIS-09`).
- **Qué Representa Arquitectónicamente:** La declaración de visión To-Be (Fase A), demostrando cómo el MTD desacopla la captura y visibilidad de eventos sin invadir la base de datos de SAP R3.
- **Sección Documental Fuente:** Fase A (`02_Fase_A_Vision_Arquitectura_Completa.md`), Apartados 2.5 y 2.7.5.
- **Dónde Corresponde Incorporarlo:** **Capítulo 3 (Fase A: Visión de Arquitectura), Apartado 3.4 (Visión de la Capacidad MTD)**, como *Figura 3.1*.
- **Restricciones de Fidelidad:** Prohibido mostrar conexiones de escritura directa hacia `SAP R3` (`REQ-RES-002`). Prohibido fusionar `NSDG` y `ENSDY` (`ASM-02`).

---

### 2.2. DIAG-02: Casos de Uso del Negocio B2B para Despacho y Trazabilidad
- **Identificador Canónico:** `DIAG-02`
- **Tipo y Categoría UML:** **Diagrama de Casos de Uso** (Categoría Comportamiento).
- **Qué Muestra el Modelo:**
  - Los actores organizacionales internos de Lurín (`ACT-03` Almacén, `ACT-04` Picking/Packing, `ACT-05` Despacho, `ACT-06` Distribución, `ACT-08` Servicio al Cliente) y externos (`ACT-01` Consultora, `ACT-07` Transporte).
  - Los seis casos de uso del negocio: `CUN-01` (Preparar y empacar), `CUN-02` (Zonificar y despachar), `CUN-03` (Transporte multimodal), `CUN-04` (Entregar y confirmar), `CUN-05` (Consultar trazabilidad) y `CUN-06` (Registrar incidencia y retorno).
  - La relación de extensión formal `<<extend>>` mediante la cual `CUN-06` extiende a `CUN-03` y `CUN-04` ante contingencias en ruta.
- **Qué Representa Arquitectónicamente:** Los límites funcionales del proceso de negocio B2B (`DEC-04`, `REQ-RES-001`), separando con rigor las responsabilidades humanas y de sistemas en el despacho nacional.
- **Sección Documental Fuente:** Fase B (`03_Fase_B_Arquitectura_Negocio_Completa.md`), Apartado 3.3.4 y archivo `DIAG-02_Casos_Uso_Negocio.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 4 (Fase B: Arquitectura de Negocio), Apartado 4.1 (Modelo Funcional de Casos de Uso del Negocio B2B)**, como *Figura 4.1*.
- **Restricciones de Fidelidad:** Cero relaciones `<<include>>` o `<<extend>>` hacia actores. Prohibido incluir casos de uso de comercio electrónico minorista B2C.

---

### 2.3. DIAG-03: Proceso Operativo de Despacho Actual AS-IS
- **Identificador Canónico:** `DIAG-03`
- **Tipo y Categoría UML:** **Diagrama de Actividades con Particiones / Swimlanes** (Categoría Comportamiento).
- **Qué Muestra el Modelo:**
  - El flujo de tareas secuenciales a través de ocho carriles organizacionales (`ACT-02` Envasado, `ACT-03` Almacén PT, `ACT-04` Picking, `ACT-05` Despacho, `ACT-06` Distribución, `ACT-07` Transporte, `ACT-08` Call Center, `ACT-01` Consultora).
  - La bifurcación inicial entre envasado y recepción de órdenes; la captura de Código UA en terminales RF para SAP R3; el armado de cajas 1 a 8; la zonificación manual a 24 departamentos; y la entrega al transportista.
  - La nota explícita del punto de quiebre operacional: **retraso de hasta ~2 horas en la visibilidad del seguimiento** por la dependencia de archivos planos y procesos batch nocturnos (`[TO 20:18–21:35]`, `ASM-03`).
- **Qué Representa Arquitectónicamente:** La línea base operativa de la empresa, justificando objetivamente la necesidad del proyecto de mejora.
- **Sección Documental Fuente:** Fase B (`03_Fase_B_Arquitectura_Negocio_Completa.md`), Apartados 3.3 y 3.5.1 y archivo `DIAG-03_Actividades_AS_IS.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 4 (Fase B: Arquitectura de Negocio), Apartado 4.2 (Proceso Operativo Actual Línea Base AS-IS)**, como *Figura 4.2*.
- **Restricciones de Fidelidad:** No introducir artefactos del MTD ni marcas temporales duales (no existen en la línea base).

---

### 2.4. DIAG-04: Proceso Operativo de Despacho Propuesto TO-BE
- **Identificador Canónico:** `DIAG-04`
- **Tipo y Categoría UML:** **Diagrama de Actividades con Particiones / Swimlanes** (Categoría Comportamiento).
- **Qué Muestra el Modelo:**
  - El flujo de actividades rediseñado mediante cinco carriles: `ACT-01` (Consultora), `ACT-07` (Conductor/Móvil), `MTD Core` (Capacidad propuesta), `ACT-04/05` (CD Lurín) y `ACT-03/06` (Sistemas Core).
  - Los puntos de captura digital en origen físico ($T_0$) al terminar la preparación y al salir a despacho.
  - La correlación de estados y validación de reglas en el carril central del MTD.
  - El mecanismo de resiliencia offline *Store-and-Forward* en la app móvil del transportista ante sombras celulares (`ASM-16`, `RSK-06`).
  - La emisión oportuna de la notificación push ($T_1$) con latencia garantizada $\le 30\text{ minutos}$ (`DEC-03`, `DEC-05`).
- **Qué Representa Arquitectónicamente:** La arquitectura operacional futura optimizada, demostrando el cierre de las brechas de negocio `GAP-01` a `GAP-04`.
- **Sección Documental Fuente:** Fase B (`03_Fase_B_Arquitectura_Negocio_Completa.md`), Apartados 3.4 y 3.5.2 y archivo `DIAG-04_Actividades_TO_BE.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 4 (Fase B: Arquitectura de Negocio), Apartado 4.3 (Proceso Operativo Propuesto TO-BE)**, como *Figura 4.3*.
- **Restricciones de Fidelidad:** Prohibido modelar rastreo satelital GPS continuo por segundo (`REQ-RES-004`, `ASM-05`). Prohibido modelar escrituras directas sobre SAP R3 (`REQ-RES-002`).

---

### 2.5. DIAG-05: Interacción Temporal y Desfases en la Línea Base AS-IS
- **Identificador Canónico:** `DIAG-05`
- **Tipo y Categoría UML:** **Diagrama de Secuencia** (Categoría Interacción).
- **Qué Muestra el Modelo:**
  - El intercambio cronológico de mensajes entre lifelines: `ACT-01 Consultora`, `ACT-08 Call Center (CELLFORCE)`, `SIS-01 SAP R3`, `SIS-02 SPY`, Servidor Batch FTP y `SIS-06/07 Transportista`.
  - La secuencia temporal desfasada: transferencia de órdenes a SPY por la mañana; exportación batch diferida al cierre de turno (21:00 hrs); llamada telefónica de la Consultora por incertidumbre; y consulta desactualizada en terminales de atención por carencia de datos en vivo.
- **Qué Representa Arquitectónicamente:** La causa técnica de la insatisfacción y de las 8,500 llamadas mensuales al Call Center, producto de la arquitectura desacoplada por lotes de la línea base (`GAP-01`, `GAP-03`).
- **Sección Documental Fuente:** Fase B, Sección 3.5.1 / Fase C, Sección 4.3.1 y archivo `DIAG-05_Secuencia_AS_IS.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 5 (Fase C: Sistemas de Información), Apartado 5.3 (Interacción Dinámica y Cronológica AS-IS vs. TO-BE)**, como *Figura 5.1*.
- **Restricciones de Fidelidad:** Mantener los lifelines legados sin introducir componentes REST ni buses orientados a eventos.

---

### 2.6. DIAG-06: Coreografía Reactiva de Eventos y Notificación TO-BE
- **Identificador Canónico:** `DIAG-06`
- **Tipo y Categoría UML:** **Diagrama de Secuencia** (Categoría Interacción).
- **Qué Muestra el Modelo:**
  - La coreografía asíncrona entre lifelines: Móvil Transportista (`ACT-07`), Pasarela `CMP-PUB-05`, Bus de Eventos `SIS-05`, Motor `CMP-COR-02`, Sonda `CMP-AUD-04`, Servicio `CMP-NOT-06` y Smartphone Consultora (`ACT-01`).
  - La cronología precisa: publicación asíncrona $\le 2\text{ seg}$ ($T_0$); correlación de reglas $\le 5\text{ seg}$; cálculo de latencia $\Delta T = T_1 - T_0$; y notificación push temprana.
  - El fragmento combinado alternativo (`alt`):
    - Operando `[Entrega Exitosa]`: Notificación de pedido entregado en distribuidora.
    - Operando `[Incidencia en Ruta]`: Alerta inmediata de contingencia e inicio de logística inversa.
- **Qué Representa Arquitectónicamente:** La solución de integración dinámica reactiva que garantiza el cumplimiento de los requerimientos `REQ-NEG-002`, `REQ-FUN-001`, `REQ-INT-003` y el SLA global $\le 30\text{ minutos}$.
- **Sección Documental Fuente:** Fase B, Sección 3.5.2 / Fase C, Sección 4.3.2 y archivo `DIAG-06_Secuencia_TO_BE.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 5 (Fase C: Sistemas de Información), Apartado 5.3 (Interacción Dinámica y Cronológica AS-IS vs. TO-BE)**, como *Figura 5.2*.
- **Restricciones de Fidelidad:** Emplear exclusivamente los microservicios canónicos `CMP-*`.

---

### 2.7. DIAG-07: Despliegue de la Arquitectura Tecnológica TO-BE
- **Identificador Canónico:** `DIAG-07`
- **Tipo y Categoría UML:** **Diagrama de Despliegue** (Categoría Estructural).
- **Qué Muestra el Modelo:**
  - Nodos físicos y lógicos de ejecución:
    - *Data Center Corporativo (On-Premise Lurín):* Servidor `SAP R3 (SIS-01)` aislado, Servidor `SPY (SIS-02)`, Clúster del Bus Corporativo (`NODE-MSG-BUS SIS-05`), Servidor de Aplicaciones `NODE-APP-MTD` y Repositorio inmutable `NODE-DB-EVENTSTORE`.
    - *Zona Desmilitarizada (DMZ):* Servidor `NODE-GW` (API Gateway, WAF, TLS 1.3, mTLS) y Portal Web B2B.
    - *Entornos Externos de Socios:* Plataformas TMS (`DRIVING SIS-08`, `NSDG SIS-06`) y dispositivos móviles con buffer offline.
    - *Canales Clientes:* Smartphones Consultoras y estaciones Customer Service (`CELLFORCE SIS-09`).
  - Rutas de comunicación seguras con protocolos explícitos (`HTTPS/TLS 1.3`, `mTLS`, `AMQP/gRPC`, `Túnel IPsec Cifrado AES-256`).
- **Qué Representa Arquitectónicamente:** La topología de infraestructura híbrida (Fase D) que soporta la alta disponibilidad (99.5%), la resiliencia en red y la ciberseguridad corporativa (`REQ-RNF-001..004`).
- **Sección Documental Fuente:** Fase D (`05_Fase_D_Arquitectura_Tecnologica_Completa.md`), Sección 5.6 y archivo `DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 6 (Fase D: Arquitectura Tecnológica), Apartado 6.2 (Especificación de Despliegue Físico y Nodos de Ejecución)**, como *Figura 6.1*.
- **Restricciones de Fidelidad:** Prohibido conectar el MTD directamente a la base de datos de SAP R3. No imponer marcas comerciales de nube pública como hechos cerrados; modelar plataforma híbrida (`ASM-06`).

---

### 2.8. DIAG-08: Arquitectura de Solución y Mapa de Paquetes de Trabajo
- **Identificador Canónico:** `DIAG-08`
- **Tipo y Categoría UML:** **Diagrama de Componentes estructurado en Paquetes** (Categoría Estructural).
- **Qué Muestra el Modelo:**
  - Los cinco Paquetes de Trabajo canónicos (`PKG-01` a `PKG-05`):
    - `PKG-01:` Gobernanza de datos, contratos `INT-DAT-01..08` y esquemas JSON Schema.
    - `PKG-02:` Plataforma Core MTD (`CMP-ING-01..NOT-06`, `DB-EVENTSTORE`).
    - `PKG-03:` Adaptadores internos (`ADP-SAP`, `ADP-SPY`, `ADP-CRM`).
    - `PKG-04:` Pasarela de transporte (`ADP-LOG`, Portal Web B2B).
    - `PKG-05:` Monitoreo operacional y telemetría de latencia en vivo.
  - Las interfaces provistas (lollipop) y requeridas (socket) que conectan modularmente cada componente sobre el Bus `SIS-05`.
- **Qué Representa Arquitectónicamente:** La descomposición modular de la solución seleccionada (Fase E: Alternativa 1 EDA/SOA Híbrida) y la cobertura formal de las ocho brechas `GAP-01` a `GAP-08`.
- **Sección Documental Fuente:** Fase E (`06_Fase_E_Oportunidades_y_Soluciones_Completa.md`), Secciones 6.3 y 6.5 y archivo `DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 7 (Fase E: Oportunidades y Soluciones), Apartado 7.3 (Arquitectura de Solución Integrada y Mapeo Modular)**, como *Figura 7.1*.
- **Restricciones de Fidelidad:** Utilizar exclusivamente los componentes y adaptadores canónicos del repositorio.

---

### 2.9. DIAG-09: Roadmap Estratégico de Transición (Horizonte S1 a S24)
- **Identificador Canónico:** `DIAG-09`
- **Tipo y Categoría UML:** **Diagrama de Máquina de Estados** (Categoría Comportamiento).
- **Qué Muestra el Modelo:**
  - La trayectoria temporal de evolución arquitectónica a través de cuatro estados discretos:
    - `Estado 0: Línea Base Actual (AS-IS)`.
    - `Arquitectura de Transición 1 (AT-1): Núcleo MTD y Gobernanza Interna [Semanas Relativas S1 a S12]`.
    - `Arquitectura de Transición 2 (AT-2): Integración de Transporte y Piloto B2B [Semanas Relativas S11 a S18]`.
    - `Arquitectura Objetivo (TO-BE): Rollout Nacional y Handover Operacional [Semanas Relativas S19 a S24]`.
  - Las condiciones de transición estrictas gobernadas por tres puntos de control vinculantes: **Gate 1 (Hito M1 en S12)**, **Gate 2 (Hito M2 en S18)** y **Gate 3 (Hito M3 en S24)**.
  - Los paquetes de trabajo asignados a cada fase de transición.
- **Qué Representa Arquitectónicamente:** La hoja de ruta estratégica de migración de la Fase F, demostrando que la empresa no acomete un cambio disruptivo ("Big Bang"), sino una transición controlada en un horizonte referencial de **24 semanas relativas (S1 a S24)**.
- **Sección Documental Fuente:** Fase F (`07_Fase_F_Planificacion_de_la_Migracion.md`), Secciones 7.1 y 7.3 y archivo `DIAG-09_Roadmap_Estrategico_Implementacion.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 8 (Fase F: Planificación de la Migración), Apartado 7.2 (Definición de Arquitecturas de Transición e Hitos)**, como *Figura 8.1*.
- **Restricciones de Fidelidad:** Mantener rigurosamente el horizonte de 24 semanas (S1-S24). Prohibido introducir Gate 4 o Gate 5.

---

### 2.10. DIAG-10: Marco de Gobernanza de la Implementación y Puertas de Calidad
- **Identificador Canónico:** `DIAG-10`
- **Tipo y Categoría UML:** **Diagrama de Actividades / Diagrama de Paquetes** (Categoría Comportamiento / Estructural).
- **Qué Muestra el Modelo:**
  - Los órganos de control: Architecture Board (`ACT-11..13`) y PMO/QA (`ACT-14..15`).
  - Los cinco Contratos de Arquitectura oficiales: **`AC-PKG-01` a `AC-PKG-05`**.
  - El procedimiento formal de auditoría y certificación a través de las tres Puertas de Calidad Canónicas: **Gate 1 (S12)**, **Gate 2 (S18)** y **Gate 3 (S24)**.
  - El circuito de gestión de excepciones y solicitudes de dispensa arquitectónica (`WVR-*`) con plazo máximo de 6 semanas y veto absoluto a excepciones sobre SAP R3 o B2B.
- **Qué Representa Arquitectónicamente:** El modelo de gobierno de la implementación de la Fase G (TOGAF ADM), garantizando que ningún desarrollo pase a producción sin verificación de conformidad.
- **Sección Documental Fuente:** Fase G (`08_Fase_G_Gobernanza_de_la_Implementacion.md`), Secciones 8.2 y 8.3 y archivo `DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 9 (Fase G: Gobernanza de la Implementación), Apartado 8.3 (Contratos de Arquitectura y Procedimiento de Excepciones)**, como *Figura 9.1*.
- **Restricciones de Fidelidad:** Nomenclatura oficial obligatoria `AC-PKG-*`. Tres puertas de calidad canónicas estrictas.

---

### 2.11. DIAG-11: Gestión Continua del Cambio y Ciclo Evolutivo ADM
- **Identificador Canónico:** `DIAG-11`
- **Tipo y Categoría UML:** **Diagrama de Actividades** (Categoría Comportamiento).
- **Qué Muestra el Modelo:**
  - Los sensores continuos de arquitectura: telemetría en vivo de SLAs ($\Delta T = T_1 - T_0$, `CMP-AUD-04`), auditoría de cero escrituras en SAP R3 y monitoreo de seguridad perimetral.
  - La recepción y evaluación de Solicitudes de Cambio (RFC).
  - El nodo de decisión de umbral de impacto con tres rutas de derivación canónicas:
    - *RFC Tipo 1 (Cambio Menor):* Despliegue operativo ITIL estándar.
    - *RFC Tipo 2 (Cambio Mayor):* Ciclo parcial TOGAF ADM (Fases E-F-G y actualización de contratos).
    - *RFC Tipo 3 (Cambio Estratégico):* Reinicio de ciclo completo TOGAF ADM (Fase Preliminar y Fase A).
  - La aprobación vinculante del Architecture Board y la sincronización del Repositorio de Arquitectura (EAR).
- **Qué Representa Arquitectónicamente:** El marco de gestión continua del cambio de la Fase H, asegurando que la arquitectura empresarial evolucione de forma controlada y no sufra obsolescencia ni arquitecturas en la sombra (*Shadow Architecture*).
- **Sección Documental Fuente:** Fase H (`09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md`), Secciones 9.2 y 9.3 y archivo `DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml`.
- **Dónde Corresponde Incorporarlo:** **Capítulo 10 (Fase H: Gestión del Cambio de Arquitectura), Apartado 9.3 (Ciclo de Vida del Cambio y Disparadores ADM)**, como *Figura 10.1*.
- **Restricciones de Fidelidad:** Prohibido degradar cambios de interfaz o de core a cambios menores sin autorización del Architecture Board.

---

## 3. Evaluación Factual de Modelos Complementarios de Fase C

Conforme a la regla innegociable de no inventar relaciones ni atributos para completar modelos:

### 3.1. Evaluación de DIAG-C1 (Modelo Lógico de Datos de Despacho)
- **Estado de la Fuente:** En la Fase C (Apartado 4.1.2) se documentan 10 entidades lógicas canónicas (`ENT-01 Pedido_Despacho` a `ENT-10 Notificacion_Consultora`).
- **Análisis de Suficiencia UML:** No existe en el repositorio un diccionario de datos relacional formal que especifique tipos de datos SQL aprobados, claves primarias compuestas, claves foráneas indexadas ni cardinalidades verificadas en el ERP.
- **Dictamen:** **NO PROCEDE COMO DIAGRAMA UML**. Intentar construir un Diagrama de Clases o un ERD forzaría al equipo a inventar campos y multiplicidades no respaldadas en la documentación. Las entidades deben permanecer documentadas en **formato tabular analítico** dentro del cuerpo del Capítulo 5.

### 3.2. Evaluación de DIAG-C2 (Arquitectura de Componentes MTD)
- **Estado de la Fuente:** Los microservicios `CMP-ING-01..NOT-06` y los adaptadores `ADP-*` ya se encuentran rigurosamente especificados y contextualizados dentro de los paquetes de trabajo en **`DIAG-08`**.
- **Dictamen:** **NO PROCEDE COMO DIAGRAMA INDEPENDIENTE**. Constituiría una duplicación innecesaria de `DIAG-08` que sobrecargaría el informe sin aportar nuevas decisiones arquitectónicas.

---

## 4. Conclusión y Resumen de Correspondencia

Los once diagramas canónicos oficiales (`DIAG-01` a `DIAG-11`) cuentan con pleno respaldo en las fuentes maestras y en los archivos `.puml` del repositorio. Quedan perfectamente identificados por su tipo y categoría OMG UML 2.5.1, su justificación arquitectónica y su ubicación precisa en los capítulos del informe principal.
