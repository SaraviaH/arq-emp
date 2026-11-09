# REGISTRO DE HALLAZGOS, AMBIGÜEDADES Y DECISIONES ARQUITECTÓNICAS DE MODELADO
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Separación Epistémica de Certeza, Homologación de Nomenclatura y Criterios de Fidelidad

---

## 1. Introducción y Marco Epistémico de Clasificación

El presente documento constituye el entregable **REV_08: Registro de Hallazgos, Ambigüedades y Decisiones Arquitectónicas de Modelado**, elaborado para establecer una distinción rigurosa entre hechos comprobados, supuestos de trabajo, inferencias técnicas y decisiones aprobadas, asegurando que ningún modelo visual ni apartado documental atribuya a Yanbal Perú compromisos tecnológicos no sustentados.

### 1.1. Principio Fundamental: Las Cuatro Categorías Epistémicas

Para garantizar el rigor académico y la solidez técnica del proyecto, se aplica estrictamente la separación en cuatro niveles de certeza documental:

1. **Hechos Comprobados (Evidencia Directa):** Información explícitamente documentada en la transcripción de la entrevista de operaciones (`[TO]`) o en las consignas académicas maestras (`[AC]`). No admiten alteración.
2. **Supuestos Maestros del Negocio (`ASM-01` a `ASM-16`):** Hipótesis analíticas formalmente adoptadas en el registro maestro `00_3` para posibilitar el diseño de la arquitectura ante vacíos o silencios informativos de la empresa.
3. **Inferencias de Ingeniería:** Deducciones técnicas especializadas fundamentadas en estándares internacionales (TOGAF ADM 10.ª Edición, ISO/IEC/IEEE 29148, OMG UML 2.5.1, ISO 31000) y buenas prácticas de ingeniería de software (patrón *Store-and-Forward*, DMZ, colas *Dead Letter*, cifrado mTLS).
4. **Decisiones de Arquitectura (`DEC-01` a `DEC-07`, `DEC-DOC-01`):** Resoluciones formales adoptadas por el equipo de arquitectura y sancionadas por el Architecture Board para delimitar la solución objetivo.

---

## 2. Tratamiento de Ambigüedades y Homologación de Nomenclatura

La auditoría técnica del repositorio resolvió de manera definitiva las ambigüedades de denominación detectadas en la documentación:

| Identificador / Término | Ubicación en Fuentes | Naturaleza de la Ambigüedad | Tratamiento Arquitectónico y Homologación Aprobada | Categoría Epistémica |
| :---: | :--- | :--- | :--- | :---: |
| **`SAP R3` (`SIS-01`)** | Transcripción `[TO 7:28..10:55]`, `00_2`, `DEC-01`, `REQ-RES-002`. | En borradores previos se introdujo erróneamente *SAP S/4HANA*. | **Corrección Innegociable:** El ERP central de Almacén Lurín es exclusivamente **`SAP R3 (SIS-01)`**. No existe aprobación corporativa ni justificación presupuestal para migración de versión en este proyecto. | **Hecho Comprobado Inmutable**. |
| **`CELLFORCE` (`SIS-09`)** | Transcripción `[TO 24:14]`, Catálogo `00_2`. | Aparente variación fonética o denominación corporativa interna de la plataforma CRM *Salesforce* utilizada por Customer Service (`ACT-08`). | Se modela bajo su identificador canónico **`SIS-09: CELLFORCE` (CRM / Atención a Consultoras)**. Se concibe como un sistema consumidor síncrono de APIs del MTD para consulta de estados en tiempo real. | **Hecho en fuente (`[TO]`)** con inferencia técnica de protocolo. |
| **`DRIVING` (`SIS-08`)** | Transcripción `[TO 1:24]`, Catálogo `00_2`. | Corresponde con alta probabilidad a la plataforma tecnológica SaaS de optimización de rutas y transporte *Drivin TMS*. | Se modela bajo su identificador canónico **`SIS-08: DRIVING` (Plataforma TMS / Seguimiento Transporte)**. Se integra al MTD mediante la pasarela de transportistas `ADP-LOG` (`PKG-04`). | **Hecho en fuente (`[TO]`)**; modelo SaaS es supuesto analítico (`ASM-07`). |
| **`NSDG` y `ENSDY` (`SIS-06`, `SIS-07`)** | Transcripción `[TO 1:24, 18:02]`, catálogo de sistemas. | Variaciones en la mención de operadores logísticos regionales o sistemas de transporte tercerizado. | Conforme al supuesto vinculante **`ASM-02`**, **se mantienen como dos entidades de software y actores logísticos formalmente independientes**, prohibiéndose su unificación arbitraria. | **Supuesto Maestro Vinculante (`ASM-02`)**. |
| **`ZapCommerce` (`SIS-04`)** | Transcripción `[TO 14:57]`, Fase A. | Término que en la transcripción operativa podría aludir a *SAP Commerce Cloud* (Hybris) o a una solución web satélite. | Conforme al supuesto **`ASM-08`**, se mantiene la denominación literal canónica **`ZapCommerce (SIS-04)`**, aclarando en nota técnica que su correspondencia con SAP Commerce queda sujeta a homologación de TI. | **Supuesto Maestro Vinculante (`ASM-08`)**. |

---

## 3. Matriz de Auditoría de los Supuestos Maestros (`ASM-01` a `ASM-16`)

A continuación se detalla el estado, fundamentación e impacto de cada uno de los dieciséis supuestos maestros del proyecto (`00_3`):

| Código | Descripción y Alcance del Supuesto Maestro | Justificación Metodológica | Impacto en la Arquitectura y Modelado |
| :---: | :--- | :--- | :--- |
| **`ASM-ACAD-01`** | **Primacía metodológica de los requerimientos canónicos.** Los 24 requerimientos oficiales prevalecen sobre versiones preliminares. | Previene discrepancias de alcance con entregables intermedios. | La totalidad de matrices y diagramas se audita frente a la Matriz Maestra `01_5`. |
| **`ASM-01`** | **Alcance B2B nacional estricto.** El alcance se circunscribe a la distribución mayorista a Consultoras/Directoras (excluye distribución B2C domiciliaria a consumidores finales). | Respeta la frontera del problema operativo planteado en CD Lurín. | Delimita los casos de uso (`DIAG-02`), flujos de proceso (`DIAG-03`, `DIAG-04`) y entidades de datos. |
| **`ASM-02`** | **Independencia de operadores logísticos.** `NSDG` y `ENSDY` representan operadores o sistemas de transporte formalmente diferenciados. | Evita simplificaciones no respaldadas por la evidencia testimonial. | Se modelan como entidades y lifelines independientes en diagramas de contexto, componentes y despliegue. |
| **`ASM-03`** | **Latencia observada en línea base.** El retraso de $\sim 2\text{ horas}$ en reporte es una estimación empírica en entrevista por carencia de marcas $T_0/T_1$. | Honestidad científica; la empresa no posee telemetría exacta en la situación actual. | Se representa como brecha operativa en notas de proceso (`DIAG-03`) y secuencia (`DIAG-05`). |
| **`ASM-04`** | **Desfase contable de inventario por merma.** El desfase de hasta 6 horas en merma al cierre de jornada es una conciliación interna contable en Lurín. | Separa la gestión física del despacho respecto a la conciliación administrativa en SAP. | Se ubica en el carril interno de Almacén (`ACT-03`), fuera del camino crítico de despacho. |
| **`ASM-05`** | **Inexistencia de telemetría GPS continua.** Las flotas de transporte no disponen de rastreo satelital por segundo (`REQ-RES-004`). | Restricción presupuestal y de conectividad de los operadores en carreteras del interior. | El MTD se basa en eventos discretos por paradas/hitos, descartando arquitecturas de geofencing continuo. |
| **`ASM-06`** | **Topología híbrida y neutralidad tecnológica.** El bus de integración opera en entorno híbrido / nube corporativa. | Cumple el principio TOGAF de neutralidad de proveedores en etapas de arquitectura de diseño. | Se modela sobre el nodo conceptual Bus `SIS-05`, sin fijar servicios cloud cerrados (AWS/Azure) como obligatorios. |
| **`ASM-07`** | **Naturaleza externa de DRIVING.** Es una solución SaaS contratada de forma independiente por transportistas tercerizados. | Delimita las fronteras de integración y responsabilidades de soporte técnico. | Se ubica en el cuadrante de sistemas externos comunicándose vía HTTPS/TLS 1.3 con el API Gateway. |
| **`ASM-08`** | **Cautela en homologación de e-commerce.** *ZapCommerce* y *SAP Commerce Cloud* se mantienen como denominaciones no fusionadas sin confirmación de TI. | Protege la integridad de los activos de software ante ambigüedades léxicas. | Se conserva la etiqueta documental `ZapCommerce (SIS-04, ASM-08)`. |
| **`ASM-09`** | **Red de transporte multimodal nacional.** La distribución a provincias involucra tramos terrestres, fluviales (Iquitos) y aéreos. | Refleja la compleja geografía del territorio peruano hacia los 24 departamentos. | El caso de uso `CUN-03` y los contratos de transporte contemplan tramos de tránsito prolongado multimodal. |
| **`ASM-10`** | **Conectividad redundante en Lurín.** El Centro de Distribución Lurín cuenta con enlaces de telecomunicaciones redundantes hacia Lima. | Garantiza la viabilidad operativa de la ingesta de eventos hacia el MTD. | Se modela enlace principal Direct Connect respaldado por túnel VPN IPsec redundante (`DIAG-07`). |
| **`ASM-11`** | **Heterogeneidad de interfaces en transportistas.** Los transportistas carecen de APIs bidireccionales homogéneas preexistentes. | Justifica la necesidad de mecanismos de contingencia para operadores pequeños. | Se incluye el Portal Web de Contingencia B2B y adaptadores `ADP-LOG` en el paquete `PKG-04`. |
| **`ASM-12`** | **Cautela en volumetría de campañas punta.** No asumir picos de concurrencia comercial no medidos empíricamente. | Evita sobre-dimensionamiento de infraestructura y costos desproporcionados. | Se especifica escalamiento horizontal elástico del bus `SIS-05` y colas de desborde (*Dead Letter Queue*). |
| **`ASM-13`** | **Metas de referencia y percentiles.** Las metas de percentil ($P_{95}$) constituyen parámetros analíticos sujetos a confirmación con stakeholders. | Rigor analítico en la definición de niveles de servicio. | La meta oficial de diseño se establece como $\le 30\text{ minutos}$ (`DEC-03`, `DEC-05`), reportando $P_{95}$ a título referencial. |
| **`ASM-14`** | **Facultad vinculante del Architecture Board.** El comité de arquitectura tiene potestad resolutiva para vetar despliegues no conformes. | Garantiza la gobernanza efectiva de la implementación (Fase G). | Se modela como la autoridad máxima de aprobación en Quality Gates y en la concesión de dispensas (`DIAG-10`). |
| **`ASM-15`** | **Punto de emisión de la marca física $T_0$.** El instante inicial corresponde estrictamente al dispositivo o sistema que registra el evento en origen. | Permite una medición matemáticamente defendible de la latencia real ($\Delta T = T_1 - T_0$). | Se especifica como atributo obligatorio `timestamp_origen_t0` en el esquema canónico JSON. |
| **`ASM-16`** | **Tolerancia a zonas de sombra celular.** La geografía vial del Perú justifica un diseño desacoplado con buffer offline (*Store-and-Forward*). | Asegura la integridad del dato ante cortes prolongados de cobertura móvil en ruta. | Se especifica el almacenamiento local cifrado en la aplicación móvil de transporte (`RSK-06`). |

---

## 4. Auditoría de Decisiones de Arquitectura Canónicas (`DEC-01` a `DEC-07`)

Las decisiones de arquitectura registradas en `00_3` constituyen mandatos vinculantes que rigen el diseño del sistema:

1. **`DEC-DOC-01` (Control de Integridad Documental):** Prohíbe la creación de fuentes documentales paralelas o contradictorias. La documentación maestra es la única fuente de verdad.
2. **`DEC-01` (Tratamiento de Requisitos bajo ISO/IEC/IEEE 29148):** Ningún componente, contrato ni modelo visual puede alterar la formulación de los 24 requerimientos canónicos.
3. **`DEC-02` (Capacidad MTD Orientada a Eventos):** El MTD se concibe como una capacidad arquitectónica desacoplada basada en eventos, nunca como un software monolítico ni como una modificación sobre el código de SAP R3.
4. **`DEC-03` (Meta de Latencia de hasta 30 Minutos):** Compromiso formal del proyecto para reducir el tiempo transcurrido entre el evento físico en ruta ($T_0$) y la disponibilidad de la notificación ($T_1$) a un máximo de 30 minutos.
5. **`DEC-04` (Frontera B2B Estricta):** Delimita el perímetro del sistema a la cadena de despacho mayorista de Yanbal Perú hacia sus Consultoras/Directoras en 24 departamentos.
6. **`DEC-05` (Medición Continua de Latencia $\Delta T$):** Establece la obligatoriedad de incorporar una sonda de auditoría (`CMP-AUD-04`) para computar $\Delta T = T_1 - T_0$ en cada evento procesado.
7. **`DEC-06` (Clasificación Aritmética de Requisitos):** Divide los 24 requerimientos en Clases A, B y C de acuerdo a su criticidad, manteniendo la suma exacta de $17 + 7 = 24$.
8. **`DEC-07` (Estándar de Contratos e Interfaces de Integración):** Adopta JSON Schema y especificaciones OpenAPI 3.0 para la definición formal de los ocho contratos de interfaz de datos (`INT-DAT-01..08` en `PKG-01`).

---

## 5. Criterios Arquitectónicos y Restricciones de Fidelidad

Para asegurar que los modelos visuales reflejen fielmente las decisiones del proyecto sin extralimitar su alcance, se establecen las siguientes restricciones de diseño:

### 5.1. Restricciones Vinculantes de Modelado
- **Cero Escritura sobre SAP R3 (`REQ-RES-002`, `DEC-01`):** Bajo ninguna circunstancia se debe representar una flecha o flujo de datos en el que el MTD o sus microservicios escriban datos directamente en las tablas transaccionales de SAP R3. La interacción se limita exclusivamente a lectura desacoplada mediante tablas de staging o vistas (`ZTRK_DESPACHO_STG`).
- **Respeto a la Identidad de Nomenclatura:** Se prohíbe el empleo de prefijos inventados en borradores previos (e.g., `CMP-ENG-*`, `CMP-CON-*`, `CTR-PKG-*`, `ADP-GEO-*`). Se debe utilizar estrictamente la taxonomía oficial de microservicios (`CMP-ING-01` a `NOT-06`), adaptadores (`ADP-SAP..CRM`), paquetes (`PKG-01..05`) y contratos (`AC-PKG-01..05`).
- **Taxonomía de Estados Cerrada:** Los modelos de comportamiento deben circunscribirse a los cinco macro-estados canónicos más la rama de incidencia. No se permite inventar estados intermedios no consensuados.
- **Horizonte Temporal Fijo:** El cronograma de transición arquitectónica comprende estrictamente 24 semanas relativas (S1 a S24) evaluadas en tres Quality Gates (Gate 1 en S12, Gate 2 en S18, Gate 3 en S24).

### 5.2. Validaciones Pendientes con Actores Clave de Yanbal Perú
Durante las etapas de implementación gobernada (Fase G), se deberán validar en campo los siguientes aspectos que se mantienen como supuestos de trabajo:
1. **Homologación Definitiva de CRM (`SIS-09`):** Ratificación con TI corporativa de la correspondencia exacta entre *CELLFORCE* y la instancia corporativa de Salesforce.
2. **Especificación Técnica de Drivin TMS (`SIS-08`):** Validación de los formatos y protocolos exactos expuestos por los transportistas tercerizados para su ingesta en `ADP-LOG`.
3. **Calibración Empírica del Umbral $P_{95}$:** Medición en la Arquitectura de Transición AT-1 del porcentaje real de eventos procesados en $\le 30\text{ minutos}$ en zonas de alta dispersión geográfica.
