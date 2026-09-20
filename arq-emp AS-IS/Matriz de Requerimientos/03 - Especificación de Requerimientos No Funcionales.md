# Especificación de Requerimientos No Funcionales (RNF)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Alcance del Proceso:** Despacho $\longrightarrow$ Transporte/Ruta $\longrightarrow$ Llegada $\longrightarrow$ Entrega $\longrightarrow$ Actualización $\longrightarrow$ Consulta  
>
> **Navegación:** [[01 - Matriz Consolidada de Requerimientos]] | [[02 - Especificación de Requerimientos Funcionales]] | [[04 - Trazabilidad y Registro de Depuración]]  
> **Enlaces al AS-IS:** [[01 - Proceso actual]] | [[04 - Sistemas e información]] | [[05 - Problemas y desfases]] | [[06 - Evidencia y fuentes]]

---

## 1. Criterios de Rigor Metodológico para RNF

Para no distorsionar el análisis técnico ni imponer suposiciones no respaldadas, los Requerimientos No Funcionales se rigen bajo los siguientes principios:

1. **Distinción entre Línea Base y Meta:** Se explicita el estado operativo actual documentado en el AS-IS frente a las expectativas u objetivos declarados por el negocio.
2. **Cero Invención de Métricas o Escenarios:** No se introducen términos no declarados en las fuentes (como *"campañas comerciales"*, porcentajes de SLA arbitrarios, número de clics o tiempos en segundos).
3. **Etiquetado Explícito de Certeza:**
   - **`[Explícitamente sustentado]`**: Afirmaciones directas del entrevistado sobre la realidad de Yanbal.
   - **`[Pendiente de validar]`**: Metas aspiracionales ($\le 30$ min) o requerimientos cualitativos que requieran una métrica numérica formal acordada con TI corporativa.

---

## 2. Catálogo Detallado de Requerimientos No Funcionales

---

### RNF-01: Tiempo de propagación de estados de entrega (Latencia de Tracking)

| Atributo | Detalle |
|---|---|
| **ID** | **RNF-01** |
| **Nombre** | Tiempo de propagación en la actualización de estados de entrega |
| **Categoría** | Rendimiento / Tiempo de Respuesta |
| **Prioridad** | **Alta (Principal reto operativo)** |
| **Sustento Metodológico** | `[Explícitamente sustentado (línea base y meta)]`<br/>`[Pendiente de validar (meta de $\le 30$ min)]` |
| **Problema que atiende** | **PR-03:** Desfase de hasta 2 horas en la actualización digital de seguimiento |
| **Fuente Primaria** | `trascrito.text` (min 23:29–24:18, min 0:49–1:16 parte 2). Documento [[05 - Problemas y desfases]] §PR-03. |

#### Enunciado Formal
El sistema debe propagar las actualizaciones de estado de entrega registradas en campo hacia las plataformas corporativas de consulta reduciendo la latencia de información actual para acortar la ventana de desconocimiento del estado del pedido.

#### Métrica y Criterio de Medición
- **Línea Base Actual (AS-IS confirmado):** El sistema actual presenta un desfase promedio de actualización de **hasta 2 horas (aproximadamente 120 minutos)** entre la entrega física y su visibilidad en el sistema central (`min 23:41–24:09`).
- **Meta del Negocio Declarada:** La necesidad y reto declarado por el negocio es reducir dicho desfase a un máximo permisible de **30 minutos ($\le 30$ min)** o lograr una comunicación en simultáneo (`min 23:29, min 1:07–1:16 parte 2`).

> [!WARNING]
> **Condición de Validación:** La cifra de $\le 30$ minutos representa una meta aspiracional y un reto expresado por la Jefatura de Distribución. Queda formalmente catalogada como **`[Pendiente de validar]`** sujeta a la evaluación de factibilidad técnica y conectividad en las diferentes zonas de distribución.

#### Sustento Textual
> *"Nuestro mayor reto ahora es hacer las actualizaciones de los estados de forma en vivo, o sea, en línea, con un rango común de fase menor a media hora, podríamos mencionarlo. Actualmente nuestro sistema de tracking nos arroja una trazabilidad o un estatus de seguimiento en promedio de dos horas [...] nuestro reto ahora es conseguir un sistema integrado que nos dé una comunicación en simultáneo o tal vez con un desfase máximo permisible de media hora, de 30 minutos."*  
> — Ing. Joao Condorpusa (`trascrito.text`, min 23:19–23:50 y min 0:57–1:16 parte 2)

---

### RNF-02: Soporte territorial nacional y operación multimodal

| Atributo | Detalle |
|---|---|
| **ID** | **RNF-02** |
| **Nombre** | Soporte territorial en los 24 departamentos y modalidades de transporte |
| **Categoría** | Capacidad / Alcance Operativo |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado]` |
| **Problema que atiende** | Alcance de la distribución a nivel nacional |
| **Fuente Primaria** | `trascrito.text` (min 19:23–19:30). Documento [[01 - Proceso actual]] §Etapa 2. |

#### Enunciado Formal
El sistema de distribución y seguimiento debe operar y dar cobertura a los envíos en los **24 departamentos** del Perú, contemplando las tres modalidades de transporte utilizadas por Yanbal: **terrestre, bimodal y aérea**.

#### Métrica y Criterio de Medición
- **Cobertura Territorial:** 24 departamentos del país.
- **Modalidades de Envío Reconocidas:** Terrestre, Bimodal y Aérea.
- **Criterio:** Capacidad de registrar y consultar estados de envíos en cualquiera de los 24 departamentos bajo las tres modalidades operativas.

#### Sustento Textual
> *"Nosotros manejamos los 24 departamentos del país. Hacemos la distribución en los 24 departamentos del país por diferentes modalidades de envío. Tenemos envío terrestre, envío biomodal, envío aéreo también."*  
> — Ing. Joao Condorpusa (`trascrito.text`, min 19:23–19:30)

---

### RNF-03: Capacidad para grandes volúmenes de datos y disponibilidad del servicio

| Atributo | Detalle |
|---|---|
| **ID** | **RNF-03** |
| **Nombre** | Capacidad para grandes volúmenes de datos y disponibilidad operativa |
| **Categoría** | Disponibilidad / Capacidad |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado (cualitativo)]`<br/>`[Pendiente de validar (SLA numérico y concurrencia)]` |
| **Problema que atiende** | Soporte al volumen operativo de datos de la empresa |
| **Fuente Primaria** | `trascrito.text` (min 1:24 parte 2). |

#### Enunciado Formal
El sistema debe contar con la capacidad necesaria para manejar grandes volúmenes de datos y mantener la disponibilidad requerida para el registro de estados y la atención de consultas del proceso de distribución.

> [!NOTE]
> **Aclaración de auditoría:**
> - Se **eliminaron las referencias no sustentadas a "campañas" o "cierres de catálogo"**, ya que la fuente no vinculó este requisito a escenarios comerciales específicos.
> - La fuente declara de forma cualitativa la necesidad de *"gran capacidad para poder manejar volúmenes de data de mayor capacidad"* y *"disponibilidad, sí"* (`min 1:24 parte 2`).
> - Al no existir en la entrevista una cifra porcentual de SLA (ej. 99.5%) ni una métrica de peticiones por segundo, cualquier parámetro numérico formal queda catalogado como **`[Pendiente de validar]`**.

#### Sustento Textual
> *"Disponibilidad, sí, también debe contar con gran capacidad para poder manejar volúmenes de data de mayor capacidad."*  
> — Ing. Joao Condorpusa (`trascrito.text`, min 1:24 parte 2)

---

### RNF-04: Restricción de interoperabilidad mediante Bus de Integración corporativo

| Atributo | Detalle |
|---|---|
| **ID** | **RNF-04** |
| **Nombre** | Interoperabilidad mediante el Bus de Integración corporativo |
| **Categoría** | Arquitectura / Restricción de Integración |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado (restricción corporativa)]` |
| **Problema que atiende** | Coexistencia e integración con el ecosistema de sistemas de Yanbal |
| **Fuente Primaria** | `trascrito.text` (min 29:00–29:09, min 2:12–2:20 parte 2). Documento [[04 - Sistemas e información]] §1. |

#### Enunciado Formal
Los sistemas que participan en el proceso de despacho, transporte y consulta deben interoperar a través del **Bus de Integración corporativo** que interconecta las aplicaciones de Yanbal, adaptándose a sus mecanismos de intercambio de información.

> [!IMPORTANT]
> **Aclaración de auditoría:**
> - La entrevista confirma que Yanbal cuenta con un Bus de Integración que *"amarra todos estos sistemas y hace que converse en uno entre ellos"* (`min 29:00–29:09`) y que los sistemas externos se integran mediante adaptaciones (`min 2:12–2:20 p2`).
> - Se **eliminaron afirmaciones no sustentadas**, tales como *"prohibiendo conexiones punto a punto propietarias y no estandarizadas"*, evitando presentar decisiones de diseño de software como mandatos explícitos del negocio.

#### Sustento Textual
> *"Y luego manejamos también el bus de integración que amarra todos estos sistemas y hace que converse en uno entre ellos, cada uno entre ellos [...] y los otros cuatro sistemas que manejamos son de proveedores que se han hecho las adaptaciones para que se integren de forma adecuada a nuestros tres sistemas internos."*  
> — Ing. Joao Condorpusa (`trascrito.text`, min 29:00–29:09 y min 2:12–2:20 parte 2)

---

### RNF-05: Restricción de acceso según nivel de rol

| Atributo | Detalle |
|---|---|
| **ID** | **RNF-05** |
| **Nombre** | Restricción de acceso según el nivel de responsabilidad del rol |
| **Categoría** | Seguridad / Control de Acceso |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado (principio de roles)]`<br/>`[Derivado (matriz de permisos específica de la solución)]` |
| **Problema que atiende** | Preservación de la integridad y control en la cadena logística |
| **Fuente Primaria** | `trascrito.text` (min 25:38–26:43). Documento [[02 - Actores relevantes]]. |

#### Enunciado Formal
El sistema debe restringir las acciones y permisos de los usuarios según su nivel de responsabilidad en la organización: los roles de nivel operativo deben estar limitados estrictamente a la ejecución de sus tareas operativas asignadas, mientras que los roles de nivel de gestión cuentan con atribuciones para realizar modificaciones, configuraciones o ajustes de parámetros.

> [!NOTE]
> **Aclaración de auditoría:**
> - El principio rector está **explícitamente sustentado**: los permisos dependen de la responsabilidad; los operativos solo ejecutan tareas y los de gestión configuran parámetros (`min 25:38–26:43`).
> - Se **retiraron las reglas sobre-especificadas** que definían de antemano el comportamiento exacto de pantallas para transportistas, consultoras o jefaturas, reconociendo que la matriz fina de privilegios corresponde al diseño derivado de la solución y no a una declaración textual de la fuente.

#### Sustento Textual
> *"Dentro de toda la cadena logística manejamos una cantidad grande de roles desde roles operativos, roles de coordinación, roles de gestión y roles directivos. Cada uno de estos roles tiene ciertos atributos [...] los roles operativos tienen la limitación de solamente ejecutar una tarea operativa, como se puede entender. Los de gestión sí se pueden hacer modificaciones, tal vez configuraciones o parámetros que nosotros atribuimos a este tipo de rol para hacer más permisible o menos permisible la vulneración de la cadena logística..."*  
> — Ing. Joao Condorpusa (`trascrito.text`, min 25:38–26:35)

---

### RNF-06: Validación formal de seguridad informática

| Atributo | Detalle |
|---|---|
| **ID** | **RNF-06** |
| **Nombre** | Validación y cumplimiento de seguridad informática corporativa |
| **Categoría** | Seguridad de la Información / Restricción de Cumplimiento |
| **Prioridad** | **Alta** |
| **Sustento Metodológico** | `[Explícitamente sustentado (restricción)]`<br/>`[Derivado (mecanismos técnicos de cifrado o auditoría)]` |
| **Problema que atiende** | Mitigación de vulnerabilidades y riesgos en la cadena de distribución |
| **Fuente Primaria** | `trascrito.text` (min 1:37–1:49 parte 2). |

#### Enunciado Formal
La solución técnica y sus componentes de software deben superar la validación formal exigida por el área de Seguridad Informática corporativa de Yanbal, con el fin de mitigar vulnerabilidades y prevenir contingencias en los sistemas.

> [!NOTE]
> **Aclaración de auditoría:**
> - El mandato de pasar validación por seguridad informática está **explícitamente confirmado** en la entrevista (`min 1:37–1:49 p2`).
> - Se **eliminaron o clasifican como derivados** los detalles técnicos no sustentados en la fuente, tales como la imposición de algoritmos de cifrado específicos o esquemas obligatorios de auditoría inmutable.

#### Sustento Textual
> *"Respecto a la seguridad, también debe pasar una validación por seguridad informática para evitar tipos de vulnerabilidad o alguna contingencia que requiera de este aspecto."*  
> — Ing. Joao Condorpusa (`trascrito.text`, min 1:37–1:49 parte 2)

---

### RNF-07: Facilidad de uso y adaptación a la operación de Yanbal

| Atributo | Detalle |
|---|---|
| **ID** | **RNF-07** |
| **Nombre** | Facilidad de uso y adaptación ergonómica a la operación |
| **Categoría** | Usabilidad / Operatividad |
| **Prioridad** | **Media** |
| **Sustento Metodológico** | `[Explícitamente sustentado (directriz cualitativa)]`<br/>`[Pendiente de validar (métricas cuantitativas)]` |
| **Problema que atiende** | Adopción efectiva de los sistemas en la operación diaria |
| **Fuente Primaria** | `trascrito.text` (min 1:57–2:20 parte 2). |

#### Enunciado Formal
Las interfaces y mecanismos de interacción del sistema deben estar adaptados específicamente a la dinámica de trabajo de la operación de Yanbal para asegurar su facilidad de uso y evitar entorpecer las actividades diarias.

> [!IMPORTANT]
> **Aclaración de auditoría:**
> - La fuente sustenta que los sistemas de Yanbal cumplen con la facilidad de uso porque *"han sido adaptados específicamente para la operación de Yanbal"* (`min 1:57–2:20 p2`).
> - Se **eliminaron las referencias no sustentadas a "movilidad", "vía pública", "número de clics" o límites de tiempo en segundos**, evitando presuponer tecnologías móviles particulares o dispositivos específicos no declarados por el entrevistado.
> - La fijación de métricas ergonómicas cuantitativas queda catalogada como **`[Pendiente de validar]`**.

#### Sustento Textual
> *"Y facilidad de uso, bueno, todos tres de los sistemas han sido adaptados específicamente para la operación de IAMBAL, así que esas tres se cumplen con la facilidad de uso y los otros cuatro sistemas que manejamos son de proveedores que se han hecho las adaptaciones para que se integren de forma adecuada a nuestros tres sistemas internos."*  
> — Ing. Joao Condorpusa (`trascrito.text`, min 1:57–2:20 parte 2)
