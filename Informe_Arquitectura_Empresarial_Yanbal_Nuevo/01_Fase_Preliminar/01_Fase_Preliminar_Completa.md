# 1. FASE PRELIMINAR

> **Ubicación oficial:** [`01_Fase_Preliminar/01_Fase_Preliminar_Completa.md`](01_Fase_Preliminar_Completa.md)  
> **Índice general de referencia:** [`INDICE_GENERAL.md`](../INDICE_GENERAL.md)  
> **Registros maestros vinculados:** [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md) | [`00_2_Catalogo_Maestro_Actores_y_Sistemas.md`](../00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md) | [`00_3_Registro_Supuestos_Incertidumbres_y_Decisiones.md`](../00_Referencias_Glosario_y_Decisiones/00_3_Registro_Supuestos_Incertidumbres_y_Decisiones.md) | [`01_5_Matriz_Maestra_Requerimientos.md`](01_5_Matriz_Maestra_Requerimientos.md)  
> **Marco metodológico:** TOGAF Standard, 10th Edition / 9.2 — Fase Preliminar

---

## 1.1. Alcance y contexto de la arquitectura organizacional (Unique S.A. / Yanbal)

### Contexto organizacional y operativo
En el Perú, la corporación internacional de cosméticos y belleza Yanbal opera legalmente bajo la razón social **Unique S.A.** `[Fuente Pública]`. La cadena de valor de la compañía integra actividades de manufactura, almacenamiento, preparación de pedidos y distribución a escala nacional. Las operaciones logísticas e industriales centrales se concentran en Lima Metropolitana, articulando la Planta Industrial y el Almacén de Productos Terminados de Lurín con el Centro de Distribución (CD), desde donde se abastece a las distribuidoras y sedes comerciales en los 24 departamentos del territorio peruano.

El flujo físico de los despachos se estructura en etapas operativas secuenciales:
1. **Manufactura y Envasado (`ACT-01`, `ACT-02`):** Elaboración y envasado del producto terminado, agrupado en cajas master mono SKU selladas e identificadas unívocamente con el **Código UA** (`[TO 1:01–1:09, 7:11–7:26]`).
2. **Almacenamiento de Productos Terminados (`ACT-03`):** Recepción y custodia en el almacén de Lurín, captura de cajas con terminales de radiofrecuencia (`SIS-10`), administración de estados de stock (*libre disposición*, *control de calidad*, *bloqueado*) exclusivamente en el ERP **SAP R3** (`SIS-01`, `REQ-RES-002`) y traslado de cajas master selladas sin saldos hacia el Centro de Distribución (`REQ-RES-003`).
3. **Preparación de Pedidos (`ACT-04`):** Descenso de pedidos comerciales desde **Maya** (`SIS-03`) y **ZapCommerce** (`SIS-04`) a través del **Bus de Integración** (`SIS-05`) hacia el sistema in-house **SPY** (`SIS-02`). En el CD se aperturan las cajas master, se ejecuta el *picking* unitario guiado por tareas en `SPY` y se realiza el empaque (*packing*) en uno de los 8 formatos volumétricos de caja (`[TO 15:30–16:17]`), asociando los productos al **Número de Pedido**.
4. **Zonificación y Despacho (`ACT-05`):** Recepción de pedidos elaborados en la Zona de Despacho y canalización según destino geográfico para su entrega física a los transportistas (`[TO 17:26–18:26]`).
5. **Transporte y Entrega B2B (`ACT-06`, `ACT-07`, `ACT-09`):** Traslado de los despachos mediante socios logísticos asociados externos (`ACT-07`) bajo modalidades terrestre, bimodal y aérea hacia los 24 departamentos, supervisado por el Área de Distribución (`ACT-06`).

### Problemática central de trazabilidad y ventana de latencia
El diagnóstico operativo evidencia una discontinuidad informativa crítica en la etapa de transporte externo:
- **Desfase actual de información:** Los eventos de cambio de estado generados durante el traslado y entrega por los socios logísticos (`ACT-07`) se reflejan en los sistemas internos con un retardo promedio de **hasta dos horas** (`[TO 23:41–24:09]`), alcanzando en contingencias lapsos superiores.
- **Impacto en la operación:** Esta latencia genera un margen de desconocimiento operativo (`[TO 24:09–24:18]`) que degrada la capacidad de respuesta del Área de Servicio al Cliente (`ACT-08`) frente a reclamos de consultoras (`[TO 24:18–24:36]`) y debilita el control de la entrega por parte del Área de Distribución (`ACT-06`).
- **Objetivo de latencia arquitectónica:** La arquitectura establece como meta operativa reducir el desfase de actualización de estados a un **máximo permisible de 30 minutos** ($\le 30\text{ min}$, `[TO 23:29; s2 0:57–1:07]`, `REQ-NEG-002`, `REQ-RNF-001`, `DEC-03`, `DEC-05`). La meta representa un objetivo de diseño arquitectónico y no una línea base histórica consolidada (`ASM-03`).

### Delimitación de situaciones operativas internas: merma en almacén
En la operación de almacén existe un desfase de hasta seis horas en la digitación de la **merma operativa** acumulada por manipuleo al final de la jornada (`[TO 12:38–14:14]`, `ASM-04`). Conforme a la aclaración complementaria del caso (`[AC]`), este desfase corresponde estrictamente a la conciliación interna del inventario físico frente al ERP en Lurín y **no forma parte del problema de trazabilidad del despacho hacia las distribuidoras**. Su tratamiento se mantiene fuera del alcance del mecanismo de trazabilidad para evitar desvíos funcionales.

### Delimitación del alcance operativo B2B y fronteras arquitectónicas
El proyecto se define como un plan de mejora arquitectónica articulado en torno a la capacidad del **Mecanismo de Trazabilidad del Despacho (`MTD`)** (`DEC-02`), sujeto a las siguientes fronteras formales:
- **Ámbito geográfico y organizacional:** Circunscrito a Unique S.A. (Yanbal Perú), abarcando los despachos desde Lima hacia las distribuidoras y destinos empresariales en los **24 departamentos del Perú** (`[AC]`, `[TO 19:15–19:23]`, `DEC-04`).
- **Frontera incluida (`In-Scope` — Alcance B2B):** Comprende la preparación del pedido en Lima (picking y packing en `SPY`), la zonificación en despacho, y la trazabilidad de los eventos de transporte ejecutados por terceros hasta la recepción en las distribuidoras y destinos empresariales (`[AC]`, `REQ-RES-001`, `DEC-04`), incluyendo el seguimiento de retornos por incidencias en ruta (`REQ-NEG-004`, `REQ-FUN-005`).
- **Frontera excluida (`Out-of-Scope`):** Queda excluida la distribución secundaria que realizan las distribuidoras hacia las consumidoras finales (alcance B2C), la gestión comercial interna de las sedes independientes y la administración general de inventario no ligada al despacho (`[AC]`).
- **Transporte externo sin flota vehicular propia:** Yanbal no opera una flota propia de camiones ni dispone de hardware GPS vehicular continuo sobre unidades de transporte (`REQ-RES-004`, `ASM-05`). El seguimiento se fundamenta en la ingesta e intercambio de eventos discretos de estado con los socios logísticos (`ACT-07`).
- **Tratamiento condicional de Persona Autorizada (`ASM-01`):** La entrega a un tercero facultado en destino (`ACT-10`, `REQ-FUN-003`) se mantiene registrada como condición sujeta a validación corporativa (`ASM-01`), evitando tratarla como un requisito obligatorio que bloquee el flujo B2B estándar.
- **Alcance académico (`ASM-ACAD-01`):** Se mantiene la estructura acumulativa de nueve apartados del informe, diferenciando los entregables de APF2 (Fase Preliminar, Fase A, Fase B y modelos de brechas/procesos) del desarrollo extendido de las Fases C a H (`ASM-ACAD-01`).

---

## 1.2. Principios de arquitectura empresarial

De conformidad con TOGAF ADM, se establecen ocho **Principios de Arquitectura Empresarial (`PAE-01` a `PAE-08`)** que rigen las decisiones de diseño, datos, aplicaciones y tecnología. Los principios aseguran neutralidad tecnológica y sustentan la integración sobre las capacidades corporativas instaladas.

### `PAE-01`: Interoperabilidad desacoplada en el ecosistema de distribución
- **Dominio:** Aplicaciones e Integración.
- **Declaración:** Los sistemas internos y externos involucrados en la preparación, despacho, transporte y consulta de estados deben intercambiar información mediante contratos e interfaces estandarizadas, evitando acoplamientos rígidos punto a punto.
- **Justificación:** La coexistencia de plataformas comerciales (`Maya`, `ZapCommerce`), almacén (`SAP R3`, `SPY`), transporte (`NSDG`, `ENSDY`, `DRIVING`) y CRM (`CELLFORCE`) requiere canales de intercambio asíncronos para reducir la latencia informativa sin comprometer la autonomía operativa de cada plataforma (`[TO s2 0:29–1:07]`).
- **Implicaciones arquitectónicas:** La arquitectura define contratos canónicos de eventos y utiliza el **Bus de Integración** (`SIS-05`, `ASM-08`) como medio central de mediación, desacoplando emisores y consumidores.

### `PAE-02`: Reutilización y preservación de las capacidades tecnológicas existentes
- **Dominio:** Negocio y Aplicaciones.
- **Declaración:** La solución arquitectónica debe reutilizar y extender los sistemas en producción de Yanbal Perú, descartando la construcción de aplicativos redundantes que dupliquen funciones o almacenes de datos maestros.
- **Justificación:** La empresa cuenta con tres sistemas adaptados para Yanbal y cuatro sistemas de proveedores integrados (`[TO s2 1:57–2:20]`, `ASM-09`). Introducir un sistema aislado incrementaría costos y generaría silos de información.
- **Implicaciones arquitectónicas:** El `MTD` se concibe como una capacidad de integración y correlación distribuida sobre el middleware corporativo (`DEC-02`, `REQ-ARQ-001`), no como un nuevo software transaccional independiente.

### `PAE-03`: Consistencia semántica y correlación de identificadores del despacho
- **Dominio:** Datos.
- **Declaración:** La información de seguimiento debe mantener una semántica homogénea y preservar la correlación de trazabilidad entre el identificador de almacenamiento (`Código UA`), las tareas de picking en `SPY` y el identificador de negocio (`Número de Pedido`), sin forzar correspondencias unívocas no confirmadas.
- **Justificación:** Mientras el almacenamiento y traslado primario en Lurín opera a nivel de cajas master mono SKU (`Código UA`), la preparación, zonificación, transporte y consulta se gestionan por `Número de Pedido` multi SKU (`[TO 7:11–7:44, 16:30–18:26]`). La coherencia exige homologar la máquina de 5 estados y correlacionar ambos mundos (`REQ-FUN-001`, `REQ-ARQ-002`).
- **Implicaciones arquitectónicas:** El modelo lógico de datos formaliza una entidad asociativa de correlación (`ENT-04`) para vincular UAs consumidas con líneas de pedido, manteniendo la cardinalidad exacta abierta a validación técnica (`ASM-11`).

### `PAE-04`: Trazabilidad oportuna basada en eventos operativos verificables
- **Dominio:** Negocio y Datos.
- **Declaración:** El seguimiento de los despachos debe sustentarse en la captura, validación y propagación oportuna de eventos discretos con marcas temporales verificables, diseñados para cumplir la meta de $\le 30\text{ minutos}$ sin depender de telemetría GPS vehicular continua.
- **Justificación:** El transporte nacional se ejecuta íntegramente a través de terceros (`ACT-07`, `REQ-RES-004`). La reducción del desfase de ~2 horas exige fiscalizar el intercambio de eventos y medir con exactitud el retardo de transmisión.
- **Implicaciones arquitectónicas:** Cada evento de trazabilidad incorpora marcas temporales duales: ocurrencia local in situ ($T_0$) y disponibilidad central en el sistema ($T_1$), permitiendo auditar la latencia real $\Delta T = T_1 - T_0$ (`REQ-ARQ-003`, `DEC-05`) y soportar esquemas de almacenamiento y reenvío ante intermitencias de red en ruta (`ASM-16`).

### `PAE-05`: Seguridad informática y segregación de permisos por rol
- **Dominio:** Tecnología y Gobernanza.
- **Declaración:** Todos los flujos de integración, intercambio de datos con proveedores logísticos y accesos a consultas deben validar controles de ciberseguridad corporativos y aplicar el principio de mínimo privilegio según el rol.
- **Justificación:** La dirección técnica exige explícitamente someter toda mejora a validación de seguridad informática para prevenir vulnerabilidades y accesos no autorizados (`[TO s2 1:37–1:49]`, `REQ-RNF-003`), protegiendo además la regla de que `SAP R3` solo es operado por Almacén (`[TO 8:50–8:55]`).
- **Implicaciones arquitectónicas:** Los componentes de integración incorporan autenticación robusta, control de acceso basado en roles (RBAC), cifrado de datos en tránsito y prohibición absoluta de escrituras directas sobre `SAP R3` desde el exterior (`REQ-RES-002`).

### `PAE-06`: Calidad de datos y capacidad para soportar el volumen operativo nacional
- **Dominio:** Datos y Tecnología.
- **Declaración:** Los componentes de ingesta y procesamiento deben garantizar completitud, consistencia y deduplicación de eventos, soportando la carga transaccional del despacho nacional sin degradación del rendimiento.
- **Justificación:** La distribución cubre 24 departamentos y requiere alta confiabilidad en los datos (`[TO s2 1:19–1:24]`). La capacidad física del almacén (2 millones de cajas mono SKU, `[TO 6:17–6:32]`) representa almacenamiento estático y no tasa de eventos, por lo que el dimensionamiento debe basarse en el flujo dinámico de órdenes (`ASM-10`).
- **Implicaciones arquitectónicas:** La arquitectura implementa validación sintáctica de esquemas canónicos, control de idempotencia mediante firmas criptográficas SHA-256 y persistencia desacoplada para evitar bloqueos por picos de despacho (`REQ-RNF-002`, `REQ-ARQ-003`).

### `PAE-07`: Mínima alteración operativa y preservación de la facilidad de uso
- **Dominio:** Negocio.
- **Declaración:** La captura y propagación de eventos debe integrarse de forma transparente en las estaciones de trabajo de almacén, empaque y despacho, preservando la agilidad operativa y la usabilidad de las interfaces actuales.
- **Justificación:** El éxito del empaque rápido (1 a 8 formatos de caja) y la operación en terminales RF y SPY radican en su facilidad de uso (`[TO s2 1:53–2:20]`). La introducción de controles no debe generar pasos manuales redundantes para los operadores.
- **Implicaciones arquitectónicas:** Las adaptaciones se implementan a nivel de servicios de backend y adaptadores de integración (`REQ-RNF-004`), conservando las reglas de manipulación física de cajas master selladas sin saldos (`REQ-RES-003`).

### `PAE-08`: Gobernanza arquitectónica, trazabilidad y control formal de cambios
- **Dominio:** Transversal y Gobernanza.
- **Declaración:** Toda decisión de diseño, requerimiento, modelo y brecha técnica debe mantener trazabilidad bidireccional documentada, diferenciando hechos probados de supuestos abiertos y rigiéndose por procedimientos formales de gestión del cambio.
- **Justificación:** La existencia de incertidumbres sobre interfaces de transporte (`ASM-02`, `ASM-07`, `ASM-11`) exige un marco estricto que impida adoptar supuestos como hechos corporativos definitivos sin la debida validación.
- **Implicaciones arquitectónicas:** Ningún requerimiento se califica como formalmente aprobado por Yanbal sin validación corporativa (`DEC-01`). Las modificaciones a la línea base se gobiernan mediante el procedimiento de Solicitud de Cambio RFC (Fase H).

---

## 1.3. Roles y responsabilidades de los involucrados en la cadena logística

El ecosistema de la cadena logística y del plan de mejora arquitectónica involucra dieciséis roles y actores formalmente catalogados ([`00_2_Catalogo_Maestro_Actores_y_Sistemas.md`](../00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md)). Estos se estructuran en tres ámbitos funcionales: roles operativos internos, actores externos y de contraparte logística, y roles de gobierno corporativo, seguridad y evaluación.

Conforme a lo declarado en la entrevista (`[TO 25:34–26:43]`), cada rol opera bajo niveles de autorización y permisos segregados (`ASM-15`), asegurando que las funciones operativas de captura no interfieran con la gestión ni vulneren los sistemas de registro.

| Código | Actor / Rol Canónico | Ámbito y Nivel | Responsabilidad Principal en la Cadena | Relación con Datos y Sistemas de Trazabilidad | Estado de Validación |
| :--- | :--- | :---: | :--- | :--- | :---: |
| **`ACT-01`** | **Área de Manufactura** | Interno (Operativo) | Elaboración industrial de productos terminados en Planta Lurín. | Entrega producto a Envasado; no genera ni consume eventos de despacho. | Confirmado (`[TO 0:52–1:01]`) |
| **`ACT-02`** | **Área de Envasado** | Interno (Operativo) | Envasado y acondicionamiento en cajas master mono SKU selladas. | Genera e imprime el `Código UA` en cada caja master sellada. | Confirmado (`[TO 1:01–1:09]`) |
| **`ACT-03`** | **Área de Almacén de PT** | Interno (Operativo/Gestión) | Custodia de stock en Lurín, traslado primario al CD y dictamen de retornos. | **Operador exclusivo de SAP R3** (`SIS-01`); lee UAs con RF (`SIS-10`); recibe avisos de retorno sin escritura externa (`REQ-RES-002`). | Confirmado (`[TO 8:12–8:55]`) |
| **`ACT-04`** | **CD — Picking y Packing** | Interno (Operativo) | Apertura de UAs, surtido unitario y empaque en 1 de 8 formatos de caja. | Opera `SPY` (`SIS-02`); genera evento de fin de preparación por `Número de Pedido`. | Confirmado (`[TO 15:06–17:26]`) |
| **`ACT-05`** | **Zona de Despacho** | Interno (Operativo) | Recepción de bultos preparados, zonificación y entrega al transportista. | Registra salida física y genera evento de despacho a través de `NSDG` (`SIS-06`). | Confirmado (`[TO 17:26–19:23]`) |
| **`ACT-06`** | **Área de Distribución** | Interno (Coordinación) | Supervisión del transporte nacional B2B, cumplimiento de SLAs y logística inversa. | Principal interesado en el seguimiento; monitorea latencia $\le 30\text{ min}$ e incidencias en ruta. | Confirmado (`[TO 0:19–0:21]`) |
| **`ACT-07`** | **Socio Logístico (Transporte)**| Externo (Operativo) | Traslado físico multimodal (terrestre, bimodal, aérea) y entrega en destino. | Captura marcas $T_0$ de tránsito y entrega; transmite eventos vía interfaces de transporte (`SIS-08`). | Confirmado (`[TO 1:24, 18:02]`) |
| **`ACT-08`** | **Área de Servicio al Cliente** | Interno (Atención) | Gestión de reclamos y consultas de consultoras por pedidos no recibidos o demorados. | Consulta estados de seguimiento e histórico de despacho a través de `CELLFORCE` (`SIS-09`). | Confirmado (`[TO 24:18–24:36]`) |
| **`ACT-09`** | **Destinatario Empresarial B2B** | Externo (Cliente B2B) | Recepción de pedidos comerciales en distribuidoras y sedes en los 24 departamentos. | Consulta estado de entrega por `Número de Pedido` en portal comercial `Maya` (`SIS-03`). | Confirmado (`[AC]`, `DEC-04`) |
| **`ACT-10`** | **Persona Autorizada en Destino**| Externo (Receptor delegado)| Recepción formal del pedido en sede B2B cuando no se encuentra el titular. | Acredita identidad y firma en recepción; atributo condicional y nullable (`REQ-FUN-003`). | **Pendiente validación (`ASM-01`)** |
| **`ACT-11`** | **Área de Seguridad Informática**| Interno (Gobierno/Seguridad)| Establecimiento de políticas de ciberseguridad, validación de riesgos y cifrado. | Audita y valida contratos de interfaces y no vulnerabilidad del middleware (`REQ-RNF-003`). | Confirmado (`[TO s2 1:37]`, `DEC-07`)|
| **`ACT-12`** | **Área de TI y Arquitectura** | Interno (Gobierno Técnico) | Administración del `Bus de Integración` (`SIS-05`) y del ecosistema de sistemas. | Define y opera especificaciones de interfaces, colas de mensajería y adaptadores. | Confirmado (`[TO 28:59]`, `DEC-07`) |
| **`ACT-13`** | **Roles Directivos / Sponsor** | Interno (Dirección Estratégica)| Aprobación de presupuestos, alcance y cambios sustanciales del proyecto. | Consume métricas ejecutivas consolidadas y aprueba formalmente requerimientos (`DEC-01`). | Confirmado (`[TO 25:34]`, `DEC-07`) |
| **`ACT-14`** | **Equipo Consultor Arquitectura**| Técnico / Consultoría | Modelado arquitectónico TOGAF ADM, matrices y especificaciones To-Be. | Genera y actualiza los artefactos del repositorio de arquitectura empresarial. | Formalizado (`DEC-07`) |
| **`ACT-15`** | **Comité Académico / Docente** | Externo (Evaluación) | Evaluación metodológica y académica del cumplimiento curricular y rúbricas. | Revisa y retroalimenta los informes de arquitectura empresarial (`ASM-ACAD-01`). | Formalizado (`DEC-07`) |
| **`ACT-16`** | **Proveedores de Software** | Externo (Soporte Técnico) | Desarrollo y mantenimiento evolutivo de sistemas de transporte (`NSDG`, `DRIVING`). | Ajusta adaptadores y contratos de integración según lineamientos de TI (`ACT-12`). | Confirmado (`[TO 9:21]`, `DEC-07`) |

Para la descripción detallada de atributos, entradas, salidas y fuentes de cada actor, véase el [`Catálogo Maestro de Actores y Sistemas (00_2)`](../00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md).

---

## 1.4. Capacidades y recursos para el desarrollo de la arquitectura

### Capacidades organizacionales del alcance
El desarrollo de la arquitectura empresarial se articula sobre las seis capacidades del negocio (`CAP-01` a `CAP-06`) identificadas en la cadena de distribución:
- **Capacidades operativas de línea base:** `CAP-01: Gestión de Almacenamiento e Inventario de PT` (Lurín, `PROC-01`), `CAP-02: Preparación y Empaque de Pedidos` (picking y packing en CD, `PROC-02`), `CAP-03: Zonificación y Despacho en Lima` (`PROC-03`) y `CAP-04: Gestión del Transporte Multimodal B2B` (itinerarios interprovinciales, `PROC-04`). Estas capacidades se mantienen en operación sin alteraciones disruptivas en sus flujos físicos (`PAE-07`).
- **Capacidades objeto de mejora arquitectónica:** `CAP-05: Trazabilidad y Sincronización de Estados de Entrega` (`PROC-04`, `PROC-05`) y `CAP-06: Atención de Consultas, Reclamos y Logística Inversa` (`PROC-05`). En ellas radica la intervención para superar la latencia de ~2 horas mediante la captura estandarizada y propagación oportuna de eventos.

### Recursos tecnológicos y ecosistema de aplicaciones de línea base
La arquitectura reutiliza el ecosistema de diez sistemas y dispositivos catalogados ([`00_2`](../00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md)), respetando la regla de convivencia de "3 sistemas propios adaptados y 4 de proveedores externos" (`[TO s2 1:57–2:20]`, `ASM-09`):
1. **`SIS-01: SAP R3`:** ERP corporativo operado exclusivamente por Almacén (`ACT-03`) para stock en vivo y estados de producto (`REQ-RES-002`).
2. **`SIS-02: SPY`:** BWMS in-house para tareas de picking, pesos teóricos y asignación de cajas 1 a 8 (`ACT-04`).
3. **`SIS-03: Maya`:** Plataforma comercial web/móvil para ingreso de pedidos y consultas por consultoras (`ACT-09`).
4. **`SIS-04: ZapCommerce`:** Plataforma transaccional comercial conectada al bus (`ASM-08`).
5. **`SIS-05: Bus de Integración`:** Middleware corporativo que comunica las plataformas de la empresa (`[TO 28:59]`, `ASM-08`).
6. **`SIS-06: NSDG`:** Sistema de transporte desarrollado por contrata para despacho nacional (`[TO 9:33]`, `ASM-02`, `ASM-11`).
7. **`SIS-07: ENSDY`:** Sistema para seguimiento y trackeo de órdenes (`[TO 28:00]`, `ASM-02`).
8. **`SIS-08: DRIVING`:** Plataforma externa de transporte para gestión de despachos (`[TO 28:10]`, `ASM-07`).
9. **`SIS-09: CELLFORCE`:** Sistema de CRM para atención de reclamos y consultas en Servicio al Cliente (`ACT-08`, `ASM-06`).
10. **`SIS-10: Terminales RF`:** Dispositivos móviles de radiofrecuencia en almacén para lectura de `Código UA` (`[TO 7:28]`).

La capacidad propuesta del **Mecanismo de Trazabilidad del Despacho (`MTD`)** no se concibe como un nuevo aplicativo monolítico, sino como una suite modular de servicios de integración montada sobre el `Bus de Integración` (`SIS-05`), estructurada en seis responsabilidades canónicas (`MTD-R01` a `MTD-R06`: ingesta, validación, correlación, auditoría temporal dual, publicación y notificación de retornos sin escritura en SAP R3, `DEC-02`).

### Recursos humanos, metodológicos y de gobierno
- **Equipo y gobernanza:** El trabajo arquitectónico lo ejecuta el Equipo de Arquitectura Empresarial (`ACT-14`) en coordinación técnica con Distribución (`ACT-06`), Almacén (`ACT-03`), Seguridad Informática (`ACT-11`) y TI (`ACT-12`), con revisión del Comité Académico (`ACT-15`) y patrocinio de la Dirección (`ACT-13`).
- **Marcos de trabajo:** Metodología TOGAF Standard (ADM Fases Preliminar a H), estándar ISO/IEC/IEEE 29148:2018 para ingeniería de requerimientos y OMG UML 2.5.1 para especificación visual y modelos textuales editables.
- **Repositorio de activos:** Repositorio canónico unificado con nomenclatura normalizada (`00_1`), control de decisiones formales (`00_3`), catálogo de actores/sistemas (`00_2`) y fuente única de requerimientos (`01_5`).

---

## 1.5. Gestión de requerimientos de arquitectura

### Proceso de ingeniería de requerimientos y criterios de gobernanza
La gestión de requerimientos de arquitectura se rige por las directrices de la norma **ISO/IEC/IEEE 29148:2018** y los lineamientos de TOGAF ADM, aplicando un ciclo riguroso de captura, especificación, validación y control de cambios:

1. **Clasificación en tres clases de fuentes (`DEC-06`):**
   - **Clase A (Evidencia directa):** Requisitos respaldados directamente en testimonios de la entrevista (`[TO]`), aclaración oficial (`[AC]`) o documentación corporativa (17 requisitos). Se registran con estado inicial `Pendiente de validación`.
   - **Clase B (Evidencia con derivación analítica):** Requisitos cuya necesidad general consta en la fuente, pero cuya formulación técnica o canalización contiene deducciones analíticas sujetas a confirmación (4 requisitos: `REQ-FUN-003` por `ASM-01`, `REQ-FUN-005` por `ASM-02`/`07`, `REQ-INT-001` por `ASM-11`, `REQ-INT-002` por `ASM-02`/`07`).
   - **Clase C (Propuesta arquitectónica):** Requisitos estructurales formulados por el equipo de arquitectura para habilitar el MTD bajo TOGAF (3 requisitos: `REQ-ARQ-001` a `REQ-ARQ-003`). Los requisitos de Clase B y C (7 en total) se registran como `Propuesto (Pendiente de validación)`.
2. **Escala de priorización uniforme (`DEC-01`):** Se aplica una única escala ordinal de tres niveles: **`Alta`** (imprescindibles para resolver la trazabilidad y respetar restricciones no negociables), **`Media`** (capacidades complementarias y optimizaciones) y **`Baja`** (mejoras optativas de fases posteriores).
3. **Control formal de estados:** Ningún requerimiento se califica como `Aprobado` en tanto no cuente con validación corporativa expresa (`DEC-01`).
4. **Trazabilidad bidireccional:** Cada requisito vincula sus necesidades de negocio con componentes, casos de uso, principios y métodos de verificación, separando la trazabilidad ya establecida de la que se completará progresivamente en las Fases C a H.

### Relación entre necesidades del negocio y arquitectura
Las necesidades estratégicas del negocio (`REQ-NEG-001` a `REQ-NEG-004`) no se traducen en interfaces aisladas, sino que se articulan orgánicamente con requisitos funcionales de captura y secuencia (`REQ-FUN-001` a `REQ-FUN-005`), requerimientos de integración desacoplada entre sistemas (`REQ-INT-001` a `REQ-INT-004`), requerimientos de calidad y rendimiento (`REQ-RNF-001` a `REQ-RNF-004`), condiciones estructurales del MTD (`REQ-ARQ-001` a `REQ-ARQ-003`) y restricciones operativas no negociables (`REQ-RES-001` a `REQ-RES-004`).

### Ubicación oficial única y resumen de control de la Matriz Maestra
Para evitar versiones duplicadas o divergentes en el repositorio, los **24 requerimientos oficiales de la línea base con sus 15 campos normalizados** residen de manera exclusiva en:

> **[Consultar Fuente Única Oficial: Matriz Maestra de Requerimientos (24 Registros, 15 Campos) — `01_5_Matriz_Maestra_Requerimientos.md`](01_5_Matriz_Maestra_Requerimientos.md)**

A continuación se presenta el cuadro de control y distribución cuantitativa de la línea base de requerimientos:

| Categoría Taxonómica                |   Rango de Identificadores    | Total  | Prioridad Alta | Prioridad Media | Prioridad Baja | Clase A (`Pendiente de validación`) | Clase B + C (`Propuesto - Pendiente`) | Estado `Aprobado` |
| :---------------------------------- | :---------------------------: | :----: | :------------: | :-------------: | :------------: | :---------------------------------: | :-----------------------------------: | :---------------: |
| **Requerimientos de Negocio**       | `REQ-NEG-001` a `REQ-NEG-004` |   4    |       3        |        1        |       0        |                  4                  |                   0                   |       **0**       |
| **Requerimientos Funcionales**      | `REQ-FUN-001` a `REQ-FUN-005` |   5    |       2        |        3        |       0        |                  3                  |       2 (`REQ-FUN-003`, `005`)        |       **0**       |
| **Requerimientos de Integración**   | `REQ-INT-001` a `REQ-INT-004` |   4    |       4        |        0        |       0        |                  2                  |       2 (`REQ-INT-001`, `002`)        |       **0**       |
| **Requerimientos de Calidad (RNF)** | `REQ-RNF-001` a `REQ-RNF-004` |   4    |       3        |        1        |       0        |                  4                  |                   0                   |       **0**       |
| **Requerimientos Arquitectónicos**  | `REQ-ARQ-001` a `REQ-ARQ-003` |   3    |       2        |        1        |       0        |                  0                  |       3 (`REQ-ARQ-001` a `003`)       |       **0**       |
| **Restricciones Obligatorias**      | `REQ-RES-001` a `REQ-RES-004` |   4    |       3        |        1        |       0        |                  4                  |                   0                   |       **0**       |
| **TOTAL LÍNEA BASE**                |     **24 Requerimientos**     | **24** |     **17**     |      **7**      |     **0**      |               **17**                |    **7** *(4 Clase B + 3 Clase C)*    |       **0**       |

---

## Referencias Bibliográficas y Documentales de la Fase Preliminar

- **ISO/IEC/IEEE.** (2018). *ISO/IEC/IEEE 29148:2018 — Systems and software engineering — Life cycle processes — Requirements engineering*. International Organization for Standardization. https://www.iso.org/standard/72089.html
- **Object Management Group (OMG).** (2017). *OMG Unified Modeling Language (OMG UML), Version 2.5.1*. https://www.omg.org/spec/UML/
- **The Open Group.** (2022). *The TOGAF Standard, 10th Edition / 9.2 — Architecture Development Method (ADM)*. The Open Group. https://www.opengroup.org/togaf
- **Unique S.A. (Yanbal Perú).** (2026). *Información Corporativa Oficial — Yanbal Perú*. https://www.yanbal.com/pe/corporate
- **Evidencia Interna del Caso (`[TO]`):** Entrevista académica al Ing. Joao Condorpusa Mendoza, Encargado del Área de Distribución de Yanbal Perú ([`Transcripción original.md`](../../Entrevista/00%20-%20Fuente/Transcripci%C3%B3n%20original.md)).
- **Evidencia Complementaria del Caso (`[AC]`):** Delimitación del problema central de trazabilidad ([`Aclaración adicional del entrevistado.md`](../../Entrevista/00%20-%20Fuente/Aclaraci%C3%B3n%20adicional%20del%20entrevistado.md)).
