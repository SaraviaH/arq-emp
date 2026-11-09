# AUDITORÍA PREVIA METODOLÓGICA Y TÉCNICA DE FASES ANTERIORES PARA LA FASE C
## Identificación de Discrepancias, Hipótesis Condicionantes y Brechas de Integración sin Modificación Retroactiva de Archivos Existentes

> **Documento:** Auditoría Previa para la Fase C: Arquitectura de Sistemas de Información  
> **Ubicación oficial:** [`04_Fase_C_Arquitectura_de_Sistemas_de_Informacion/AUDITORIA_PREVIA_SIN_MODIFICACIONES.md`](AUDITORIA_PREVIA_SIN_MODIFICACIONES.md)  
> **Estado de gobernanza:** Vigente / Aprobado como marco de referencia para la Fase C  
> **Regla de integridad documental:** Todos los documentos precedentes (`00_Referencias_Glosario_y_Decisiones/`, `01_Fase_Preliminar/`, `02_Fase_A_Vision_de_Arquitectura/`, `03_Fase_B_Arquitectura_de_Negocio/`, archivos `README.md`, `INDICE_GENERAL.md` e informes base) se mantienen estrictamente en modo **SOLO LECTURA**. Ningún archivo previo es editado, reescrito, renombrado o desplazado.

---

## 1. Propósito y Alcance de la Auditoría Previa

El desarrollo de la **Fase C: Arquitectura de Sistemas de Información** (conformada por la **Arquitectura de Datos** y la **Arquitectura de Aplicaciones**, según el marco TOGAF 9.2) exige un análisis exhaustivo y sistemático de las definiciones, modelos, supuestos y restricciones consolidados en las fases anteriores (Fase Preliminar, Fase A y Fase B), así como en los registros maestros (`00_1`, `00_2`, `00_3`) y en la `01_5_Matriz_Maestra_Requerimientos.md`.

El objetivo fundamental de este documento es:
1. **Identificar tensiones conceptuales y metodológicas** presentes en el material previo o heredadas de versiones tempranas (V1) que puedan inducir a errores en el modelado de datos o en la arquitectura de aplicaciones.
2. **Catalogar relaciones técnicas no confirmadas en la evidencia primaria** (entrevista `[TO]` y aclaración complementaria `[AC]`) para evitar que se asuman como verdades corporativas definitivas en los diagramas de datos y componentes.
3. **Señalar decisiones técnicas prematuras** que se hubieran formulado en versiones anteriores (como la adopción de motores de bases de datos específicos, tecnologías particulares de colas o protocolos propietarios de bus) para abstraerlas y tratarlas a nivel lógico-arquitectónico neutral.
4. **Delimitar el impacto de los requisitos y restricciones clave** (`REQ-RES-001` a `REQ-RES-004`, `REQ-ARQ-001` a `REQ-ARQ-003`, `REQ-RNF-001` a `REQ-RNF-004`, `REQ-INT-001` a `REQ-INT-004`, `REQ-FUN-001` a `REQ-FUN-005` y `REQ-NEG-001` a `REQ-NEG-004`) sobre las entidades de datos y los componentes de software propuestos.
5. **Establecer las directrices de mitigación y encapsulación** que gobernarán la redacción de la Fase C en el documento `04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md`, asegurando coherencia global sin vulnerar la regla de conservación inmutable de los archivos preexistentes.

---

## 2. Inventario de Tensiones Metodológicas y Hallazgos Críticos

### 2.1. Tensión Conceptual sobre la Naturaleza del MTD: Capacidad de Integración vs. Aplicativo Aislado
- **Situación identificada en antecedentes (V1):** La versión temprana V1 presentaba al Mecanismo de Trazabilidad del Despacho (MTD) como si fuera un aplicativo de software monolítico independiente, completo y autocontenido (con base de datos propia, interfaz de usuario para todos los actores y lógica de negocio duplicada).
- **Alineación consolidada en Fases Preliminar, A y B (`DEC-02`, `REQ-ARQ-001`):** El MTD no es un aplicativo independiente ni un desarrollo monolítico aislado. Es una **propuesta arquitectónica de integración y trazabilidad**, articulada como un conjunto desacoplado de componentes y servicios que operan sobre la infraestructura del `Bus de Integración` (`SIS-05`) y se comunican con los sistemas existentes (`SIS-01` a `SIS-10`).
- **Condicionamiento para la Fase C:**
  - *En Arquitectura de Datos (4.1):* No se modelará una base de datos monolítica de "un nuevo sistema Yanbal", sino el **modelo lógico de datos corporativo de eventos de distribución y trazabilidad**, definiendo entidades de dominio canónicas que interoperan a través del bus.
  - *En Arquitectura de Aplicaciones (4.2):* Los componentes del MTD se estructuran como servicios modulares y desacoplados (ingesta, correlación, validación semántica, auditoría temporal, publicación y notificación de excepciones), acompañados de adaptadores para los sistemas legados y de terceros.

### 2.2. Delimitación Estricta del Alcance B2B y Tratamiento de Destinatarios (`REQ-RES-001`, `DEC-04`, `ASM-01`)
- **Situación identificada en antecedentes:** Ciertas menciones en el informe V1 y transcripciones informales hacían alusión a entregas a "clientes finales", "hogares" o "personas naturales no vinculadas", mezclando conceptos B2C y B2B.
- **Alineación previa inmutable:** El alcance está formalmente restringido por `REQ-RES-001` y ratificado por la decisión `DEC-04` a un modelo **estrictamente Business-to-Business (B2B)**. Los destinatarios legítimos son Directores/as Independientes y Consultoras Independientes de Yanbal (`ACT-09`), en sus domicilios comerciales o puntos autorizados en los 24 departamentos del Perú.
- **Condicionamiento para la Fase C:**
  - *En Datos:* Las entidades de negocio deben reflejar destinatarios empresariales (`Destinatario_Empresarial_B2B`, código de cliente comercial). La figura de `Persona Autorizada en Destino` (`ACT-10`, `REQ-FUN-003`) se mantiene clasificada como **condicional bajo `ASM-01`**, modelándose como una entidad opcional/extensible y nunca como un campo obligatorio que bloquee el flujo B2B estándar.
  - *En Aplicaciones:* Los servicios de consulta y trazabilidad (`REQ-FUN-002`, `REQ-NEG-003`) exponen interfaces orientadas a los canales comerciales y de atención corporativa (`Maya`, `ENSDY`, `CELLFORCE`), sin asumir un portal de comercio electrónico masivo B2C abierto a terceros consumidores no registrados.

### 2.3. Operación Exclusiva de SAP R3 por Almacén Lurín y Prohibición de Escrituras Directas (`REQ-RES-002`)
- **Situación identificada en antecedentes:** Existían ambigüedades sobre si el MTD u otros sistemas podían modificar tablas en `SAP R3` o si el MTD debía actuar como el nuevo "gestor de inventario".
- **Alineación previa inmutable:** `ACT-03` (Área de Almacén de Productos Terminados en Lurín) es el único rol autorizado y facultado para operar `SAP R3` (`[TO 8:50–8:55]`). El requerimiento `REQ-RES-002` prohíbe taxativamente cualquier escritura directa por parte del MTD en `SAP R3`.
- **Condicionamiento para la Fase C:**
  - *En Datos:* Ninguna entidad del MTD asume propiedad (*System of Record*) sobre el inventario de almacén ni sobre los estados maestros de producto (*libre disposición*, *control de calidad*, *bloqueado*). La SoR de inventario maestro reside exclusivamente en `SAP R3`.
  - *En Aplicaciones:* La comunicación hacia o desde `SAP R3` se concibe mediante mecanismos asíncronos y desacoplados mediados por el `Bus de Integración` (`SIS-05`) y adaptadores especializados. En el caso de logística inversa (`MTD-R06`, `REQ-FUN-005`), el MTD notifica al área de almacén (`ACT-03`), quien evalúa físicamente el pedido y registra, de forma soberana en `SAP R3`, la reincorporación o merma del stock.

### 2.4. Ejecución del Transporte Mediante Socios Logísticos Externos (`REQ-RES-004`, `ASM-05`, `RSK-02`)
- **Situación identificada en antecedentes:** Hipótesis erróneas sobre la existencia de una flota vehicular propia de camiones monitoreada por Yanbal mediante telemetría GPS directa.
- **Alineación previa inmutable:** Yanbal no posee flota propia de transporte de larga distancia ni de última milla (`ASM-05`). El transporte es ejecutado por terceros contratados (`ACT-07`), con modalidades terrestre, bimodal (terrestre + fluvial) y aérea, bajo la supervisión del Área de Distribución (`ACT-06`).
- **Condicionamiento para la Fase C:**
  - *En Datos:* Los datos de seguimiento en ruta no provienen de dispositivos GPS instalados en vehículos de Yanbal, sino de eventos reportados por los socios logísticos (`ACT-07`) o capturados en sistemas como `DRIVING` (`SIS-08`) y `NSDG` (`SIS-06`).
  - *En Aplicaciones:* Los componentes de software del MTD deben ofrecer puntos de ingesta (*gateways* o adaptadores) tolerantes a la heterogeneidad de los proveedores externos, garantizando el registro de marcas temporales duales ($T_0$ y $T_1$, `REQ-ARQ-003`).

---

## 3. Catálogo de Relaciones No Confirmadas, Decisiones Prematuras e Incertidumbres

En la siguiente tabla se consolidan los puntos críticos detectados en la auditoría cruzada que deben ser tratados con máxima rigurosidad en la Fase C, detallando el riesgo arquitectónico y la estrategia de encapsulación sin editar los archivos previos:

| ID Incertidumbre | Elemento Afectado | Situación en Evidencia Primaria / Antecedentes | Riesgo para la Fase C si se Asume Erróneamente | Estrategia de Tratamiento en Fase C (Datos y Aplicaciones) |
| :--- | :--- | :--- | :--- | :--- |
| **`ASM-02`** | `NSDG` (`SIS-06`) vs. `ENSDY` (`SIS-07`) | Mencionados en momentos distintos de la entrevista (`[TO 9:33]` y `[TO 28:00]`). `NSDG` es calificado como desarrollo por contrata para transporte; `ENSDY` como sistema de trackeo de pedidos. No hay evidencia de que sean el mismo sistema ni de que sean completamente independientes. | Fusionar ambos sistemas prematuramente o inventar interfaces no sustentadas entre ellos. | Mantener `NSDG` y `ENSDY` como componentes lógicos diferenciados en el catálogo de aplicaciones actuales, interactuando con el MTD a través de adaptadores conceptuales desacoplados en el Bus. |
| **`ASM-06`** | `CELLFORCE` (`SIS-09`) | Mencionado en `[TO 28:44]` como herramienta de servicio al cliente (`ACT-08`). Fonéticamente similar a Salesforce, pero no confirmado oficialmente. | Asumir que Yanbal cuenta con una implementación estándar de Salesforce Cloud con APIs REST comerciales específicas. | Modelar `SIS-09` con su denominación canónica provisional `CELLFORCE` a nivel lógico, definiendo una interfaz de sincronización de eventos de consulta (`INT-DAT-08`) abstracta y neutral. |
| **`ASM-07`** | `DRIVING` (`SIS-08`) | Mencionado en `[TO 28:10, 28:16]` como sistema externo de transporte. No se detalla si pertenece a un proveedor logístico particular o si es operado por Yanbal. | Prescribir protocolos propietarios o endpoints fijos sin validación de su interfaz técnica. | Tratar `DRIVING` como un sistema satélite de transporte que alimenta el MTD mediante eventos de despacho/ruta vía el adaptador logístico (`ADP-LOG`). |
| **`ASM-08`** | `Bus de Integración` (`SIS-05`) y `ZapCommerce` (`SIS-04`) | El testimonio confirma la existencia de un bus corporativo (`[TO 14:57, 16:58, 28:59]`), pero no especifica la tecnología de middleware subyacente. `ZapCommerce` es transcripción fonética no homologada formalmente a *SAP Commerce*. | Imponer prematuramente tecnologías físicas específicas (ej. Apache Kafka, RabbitMQ, MuleSoft, Oracle OSB) o renombrar el sistema comercial sin validación. | Modelar el bus como un patrón de integración corporativa (Publish-Subscribe, Message Broker, Channel) agnóstico a la tecnología física. Mantener `ZapCommerce` como denominación canónica provisional. |
| **`ASM-09`** | Coexistencia de los 3 sistemas propios y 4 de proveedores | En `[TO s2 1:57–2:20]` se afirma la existencia de 3 sistemas propios adaptados y 4 sistemas de proveedores en el ecosistema. La correspondencia nominal exacta no está cerrada en las fuentes. | Forzar una lista cerrada arbitraria de aplicaciones y violar el requerimiento `REQ-INT-004`. | En la Arquitectura de Aplicaciones (4.2.1), documentar la taxonomía respetando la regla `3+4`, categorizando las aplicaciones confirmadas y dejando la correspondencia abierta bajo `ASM-09`. |
| **`ASM-10`** | Capacidad física vs. Rendimiento transaccional | La entrevista menciona una capacidad de almacenamiento de ~2,000,000 de cajas en el almacén de Lurín (`[TO 1:19]`). Ciertos borradores tempranos derivaron de allí tasas irreales de miles de transacciones por segundo. | Dimensionar los componentes de software y colas de mensajes basándose en almacenamiento cúbico estático en lugar de volumen transaccional dinámico de despachos. | Desacoplar explícitamente el almacenamiento estático de Lurín del flujo de eventos de despacho. Modelar las tasas de mensajes de acuerdo con la escala operativa real de distribución B2B. |
| **`ASM-11`** | Cardinalidad `Código UA` $\leftrightarrow$ `Número de Pedido` y Sujeto de Integración `NSDG` | En Lurín ingresan cajas master mono SKU con `Código UA`. En el CD (`ACT-04`) se abren cajas para picking unitario de pedidos multi SKU. La correspondencia unívoca 1:1 no existe. Además, el sujeto de integración en `[TO 8:50–9:37]` entre SAP R3, SPY y NSDG es gramaticalmente ambiguo. | Modelar una relación 1:1 rígida entre UA y Pedido, o asumir conexión directa entre SAP R3 y NSDG violando `REQ-RES-002`. | En el Modelo Lógico de Datos (4.1.2), introducir la entidad asociativa `Línea_Picking_Trazable` (`ENT-04`), que vincula las UAs consumidas con las líneas de cada `Número de Pedido`. Tratar la integración de `NSDG` a través del Bus y no de forma directa desde `SAP R3`. |
| **`ASM-03` / `DEC-05`** | Desfase actual (2 h a días) y Meta de 30 min | El desfase actual de información es una problemática operativa declarada cualitativamente. La meta de $\le 30\text{ min}$ (`REQ-RNF-001`, `DEC-03`, `DEC-05`) es un objetivo de diseño, no una medición histórica disponible. | Tratar los 30 minutos como una métrica existente o confundir la latencia de procesamiento del software con el tiempo físico del camión en carretera. | Modelar la estructura de auditoría temporal con marcas duales ($T_0$ de ocurrencia y $T_1$ de ingesta), definiendo el KPI de latencia de información $\Delta T = T_1 - T_0 \le 30\text{ min}$ como regla de control y monitoreo en el componente `CMP-AUD-04`. |
| **`ASM-04`** | Merma operativa en almacén (desfase 6 h) | En `[TO 12:38–14:14]` se describe que la merma interna en el almacén de Lurín se digita con hasta 6 horas de desfase al final del turno. | Introducir el proceso de ajuste contable de merma de manufactura/almacén dentro del alcance del MTD. | Excluir tajantemente la merma interna de almacén del alcance del MTD, limitando la gestión de eventos de merma a las mermas o pérdidas ocurridas durante el transporte externo (`MTD-R06`). |
| **`ASM-16` / `RSK-06`** | Conectividad intermitente en transporte interprovincial | En rutas terrestres remotas (ej. sierra y selva) o transporte bimodal, la cobertura celular es deficiente o nula durante tramos prolongados. | Asumir conectividad en línea continua (100% online) en tiempo real para todos los camiones de los socios logísticos. | Incorporar en la estrategia de sincronización (4.1.5) y en los adaptadores logísticos (4.2.4) patrones de almacenamiento y reenvío (*Store-and-Forward*), marcas de tiempo locales inmutables ($T_0$) e idempotencia en la ingesta por lotes. |

---

## 4. Auditoría de Requisitos Aplicable a Datos y Aplicaciones (Los 24 Requisitos Maestros)

Para garantizar que ningún requisito quede huérfano ni sea reinterpretado indebidamente en la Fase C, se audita a continuación cómo cada uno de los 24 requerimientos oficiales de la `01_5_Matriz_Maestra_Requerimientos.md` condiciona las entidades de datos y los componentes de software:

### 4.1. Requerimientos de Negocio (`REQ-NEG-001` a `REQ-NEG-004`)
1. **`REQ-NEG-001` (Seguimiento de entregas B2B):** Exige entidades para pedidos, bultos y estados normalizados, y componentes de consulta para clientes y distribución.
2. **`REQ-NEG-002` (Reducción de latencia a $\le 30$ min):** Exige marcas de tiempo duales ($T_0, T_1$) en el esquema de eventos y un componente de auditoría de latencia (`CMP-AUD-04`).
3. **`REQ-NEG-003` (Disponibilidad de estados para reclamos):** Exige que el modelo de datos provea persistencia desacoplada de estados y que la arquitectura de aplicaciones publique actualizaciones hacia `CELLFORCE` (`SIS-09`) y canales de atención.
4. **`REQ-NEG-004` (Trazabilidad de logística inversa):** Exige una entidad específica para incidencias de retorno (`Registro_Retorno_Logistica_Inversa`), gestionada por el componente `CMP-NOT-06`.

### 4.2. Requerimientos Funcionales (`REQ-FUN-001` a `REQ-FUN-005`)
5. **`REQ-FUN-001` (Transición secuencial de 5 estados):** Define el ciclo de vida de la entidad `Pedido_Comercial` y exige un motor de validación semántica de estados (`CMP-VAL-03`).
6. **`REQ-FUN-002` (Consulta por Pedido y Cliente con promesa):** Exige atributos de promesa de entrega (lead times: 24 h Lima / 7 d provincias) y componentes de consulta desacoplada (`CMP-PUB-05`).
7. **`REQ-FUN-003` (Identificación de Persona Autorizada - Condicional `ASM-01`):** Se modela como entidad de datos complementaria nullable (`Receptor_Autorizado_Destino`), activa solo cuando aplique la política corporativa.
8. **`REQ-FUN-004` (Asociación Pedido, Formato de Caja 1 a 8, Geografía y Socio):** Exige que la entidad `Bulto_Despacho_Caja` contenga el formato asignado (1 a 8), la volumetría teórica y el socio logístico (`ACT-07`), gestionado entre `SPY`, el CD y el MTD.
9. **`REQ-FUN-005` (Registro de incidencias y notificación a almacén):** Exige entidades de incidencias de ruta y notificación asíncrona hacia `ACT-03` a través del Bus, sin escrituras directas en `SAP R3`.

### 4.3. Requerimientos de Integración (`REQ-INT-001` a `REQ-INT-004`)
10. **`REQ-INT-001` (Sincronización SAP R3 - SPY y canalización a NSDG):** Delimita los flujos de integración: sincronización interna de stock entre `SAP R3` y `SPY`, y canalización de órdenes de transporte hacia `NSDG` a través del Bus Corporativo.
11. **`REQ-INT-002` (Intercambio oportuno de eventos con socios logísticos):** Exige adaptadores de ingesta heterogéneos (`ADP-LOG`) que capturen eventos de transporte en tiempo oportuno.
12. **`REQ-INT-003` (Integración de estados vía Bus entre sistemas comerciales):** Establece a `SIS-05` como el canal central de publicación/suscripción entre `Maya`, `ZapCommerce`, `SPY`, `ENSDY` y `CELLFORCE`.
13. **`REQ-INT-004` (Coexistencia con 3 sistemas propios y 4 de proveedores):** Requiere un patrón de integración desacoplado (Anti-Corruption Layer / Adaptadores) que aísle la lógica del MTD de los contratos propietarios de los 7 sistemas.

### 4.4. Requerimientos de Calidad / No Funcionales (`REQ-RNF-001` a `REQ-RNF-004`)
14. **`REQ-RNF-001` (Latencia máxima de 30 min):** Rige el diseño de los canales de ingesta y publicación, exigiendo procesamiento en tiempo cercano al real (Near Real-Time / NRT).
15. **`REQ-RNF-002` (Manejo de grandes volúmenes operativos):** Exige diseño de esquemas de datos normalizados con particionamiento temporal y procesamiento asíncrono no bloqueante.
16. **`REQ-RNF-003` (Seguridad informática corporativa y control por rol):** Impone principios de control de acceso basado en roles (RBAC), integridad criptográfica (hash de eventos) y cumplimiento de la Ley 29733 (protección de datos personales).
17. **`REQ-RNF-004` (Preservación de facilidad de uso en interfaces operativas):** Dictamina que los componentes del MTD no deben sobrecargar a los operadores con nuevas interfaces complejas; las adaptaciones deben ser no invasivas en los sistemas existentes.

### 4.5. Requerimientos Arquitectónicos (`REQ-ARQ-001` a `REQ-ARQ-003`)
18. **`REQ-ARQ-001` (MTD como capacidad integrada sin duplicación):** Rige la arquitectura de aplicaciones: los componentes de trazabilidad residen como extensiones de integración en el ecosistema, no como un sistema paralelo redundante.
19. **`REQ-ARQ-002` (Trazabilidad lógica UA $\leftrightarrow$ Picking $\leftrightarrow$ Pedido):** Rige la arquitectura de datos: define la entidad asociativa que resuelve la correlación entre unidades de almacenamiento mono SKU y pedidos multi SKU.
20. **`REQ-ARQ-003` (Estructura canónica de eventos con marcas $T_0$ y $T_1$):** Rige el diccionario y contratos de datos: especifica el esquema canónico obligatorio de 11 atributos clave para todo evento de trazabilidad.

### 4.6. Requerimientos de Restricción (`REQ-RES-001` a `REQ-RES-004`)
21. **`REQ-RES-001` (Alcance exclusivo de despacho B2B en el Perú):** Delimita el dominio de datos y el universo de aplicaciones a los 24 departamentos del Perú y clientes empresariales.
22. **`REQ-RES-002` (Operación exclusiva de SAP R3 por Almacén Lurín):** Restringe las interfaces de aplicación: prohíbe escrituras directas y delega en `ACT-03` la actualización de inventarios.
23. **`REQ-RES-003` (Tránsito primario en cajas mono SKU selladas sin saldos):** Condiciona el modelo de datos logístico en la etapa entre Envasado, Almacén de PT y CD.
24. **`REQ-RES-004` (Transporte mediante socios externos y exclusión de flota propia):** Prohíbe modelar subsistemas de gestión de flotas o geolocalización vehicular directa en Yanbal.

---

## 5. Directrices y Reglas de Decisión para la Redacción de la Fase C

Con base en la presente auditoría, la redacción de la Fase C en `04_Fase_C_Arquitectura_Sistemas_Informacion_Completa.md` adoptará de forma mandataria las siguientes directrices:

1. **Agnosticismo Físico en Datos:** El modelo de datos se presentará a nivel lógico-conceptual, utilizando tipos de datos estándar de modelado de dominio (String, DateTime, Decimal, UUID, Boolean) sin atarse a marcas comerciales de bases de datos relacionales ni esquemas NoSQL particulares.
2. **Resolución Explícita de Correlación UA - Pedido:** Se incluirá un modelo relacional y tabular explicativo de la entidad `Línea_Picking_Trazable` (`ENT-04`), demostrando cómo se mantiene la trazabilidad desde el ingreso de la caja mono SKU en Lurín hasta el empaque multi SKU en el CD, sin forzar una cardinalidad unívoca 1:1.
3. **Esquema Canónico de Eventos de Trazabilidad (`REQ-ARQ-003`):** Se especificará en detalle la estructura canónica del evento, definiendo cada uno de sus campos, incluyendo obligatoriamente el identificador de evento, identificador de negocio, estado normalizado, emisor, marca de ocurrencia local ($T_0$), marca de ingesta central ($T_1$), latencia calculada ($\Delta T$), datos de contexto y firma de idempotencia.
4. **Arquitectura de Componentes Modular y Desacoplada:** El MTD se representará como una suite de 6 componentes de integración (`CMP-01` a `CMP-06`) orquestados por el `Bus de Integración` (`SIS-05`), apoyados por adaptadores específicos (`ADP-XX`) para aislar los sistemas legados y de proveedores (`3+4`).
5. **Especificación Textual Profunda de Componentes:** Dado que en este turno rige la prohibición de crear o editar diagramas gráficos o archivos `.puml`, el apartado 4.2.3 se desarrollará mediante una especificación textual y tabular exhaustiva de componentes, puertos, interfaces provistas (`<<interface>>`) e interfaces requeridas (`<<use>>`).
6. **Matriz de Trazabilidad Requerimientos-Componentes (4.2.5):** Se mapearán los 24 requerimientos oficiales contra los componentes propuestos y los sistemas existentes, demostrando cobertura del 100% sin duplicar innecesariamente la matriz de 15 campos de la Fase Preliminar.

