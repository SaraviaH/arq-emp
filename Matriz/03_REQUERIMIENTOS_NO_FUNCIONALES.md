# Catálogo Oficial de Requerimientos No Funcionales: Y-Trace

> **Carpeta:** `detalles`  
> **Documento:** `03_REQUERIMIENTOS_NO_FUNCIONALES.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Propósito:** Especificación formal, exhaustiva y medible de todos los Requerimientos No Funcionales (RNF) del sistema, organizados bajo el estándar internacional de calidad ISO/IEC 25010 (SQuaRE).  
> **Estado:** [CONSOLIDADO OFICIAL — BASE PARA ARQUITECTURA DE SOFTWARE Y PRUEBAS]  
>  
> 🔗 **Documentos Vinculados:**  
> - Matriz Maestra Unificada: [[01_MATRIZ_DE_REQUERIMIENTOS]]  
> - Requerimientos Funcionales: [[02_REQUERIMIENTOS_FUNCIONALES]]  
> - Nodo Central del Proyecto: [[MAPA_MAESTRO_DEL_PROYECTO]]  
> - Restricciones Técnicas: [[Restricciones_Tecnicas]] | [[Definicion_Tecnologica]]  

---

## 1. Marco Metodológico y Definición del Requerimiento No Funcional

En la arquitectura empresarial de Y-Trace, un **Requerimiento No Funcional (RNF)** responde estrictamente a la pregunta:

> **"¿Cómo debe funcionar el sistema o qué atributo de calidad, restricción o condición técnica debe satisfacer Y-Trace?"**

No representa una acción o comportamiento que el usuario pueda ejecutar directamente (como presionar un botón o ver una pantalla), sino una **propiedad emergente del sistema**, un **límite de desempeño** o un **estándar de seguridad y resiliencia** que condiciona cómo se implementan los requerimientos funcionales.

### Principio Antirredundancia y Clasificación Crítica:
* **No clasificar como RNF una función solo por contener palabras como "automático", "seguro" o "rápido":** Por ejemplo, *"Publicar eventos al Bus"* es un Requerimiento Funcional ([[RF029]]); que la latencia de dicha publicación sea como máximo de *30 minutos* es el Requerimiento No Funcional asociado (`RNF007`).
* **Métricas Objetivas y Verificables:** Todo RNF debe contar con una unidad de medida, un umbral de aceptación y un método de prueba para evitar declaraciones ambiguas. Cuando un parámetro técnico dependa de validaciones pendientes con TI de Yanbal, se consigna expresamente como *"Pendiente de validación"*.
* **Delimitación de Plataforma Móvil (Exclusividad Android):** La aplicación móvil de Y-Trace está destinada **exclusivamente a dispositivos Android** (versión 8.0 Oreo o superior con Google Chrome v90+). La captura de ubicación GPS se realizará mediante los servicios de geolocalización disponibles en Android. **No se contempla compatibilidad con iOS en el alcance del proyecto.**

* **Definición del SLA central de integración (30 minutos):** El valor de **30 minutos es el límite máximo de publicación y aceptación de un evento en el Bus de Integración, contado desde que el backend de Y-Trace recibe el evento y lo incorpora a la cola de integración. Este valor no representa una frecuencia de captura GPS ni una frecuencia fija de envío. Durante una desconexión del dispositivo, los eventos se conservan localmente sin generar registros artificiales y, al recuperar conectividad, se transmiten al backend; desde esa recepción comienza el cómputo del SLA de 30 minutos.

---

## 2. Catálogo Detallado de Requerimientos No Funcionales (RNF001 – RNF023)

---

### Categoría 1: Seguridad, Privacidad y Gobernanza de Acceso

#### RNF001 — Seguridad en el Control de Acceso Dual (Web vs. Móvil)
* **ID:** `RNF001`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-01`
* **Nombre:** Seguridad en el Control de Acceso Dual (Web vs. Móvil).
* **Categoría:** Seguridad (Control de Acceso / Autenticación).
* **Descripción:** El sistema debe impedir taxativamente el acceso no autorizado a las consolas de la plataforma Web y bloquear cualquier intento de transmisión de telemetría GPS o registro de eventos desde dispositivos móviles que no cuenten con una sesión operativa vinculada a un despacho existente y habilitado para seguimiento.
* **Objetivo de Negocio:** Proteger la confidencialidad de la información corporativa y evitar el sabotaje, inyección de datos ficticios o fugas de información de las rutas de entrega.
* **Módulo o Componente Afectado:** API Gateway, Módulo de Seguridad Web RBAC, Módulo de Activación Móvil.
* **Condición o Métrica:** 
  - 100% de peticiones sin credenciales web válidas rechazadas con código HTTP 401 Unauthorized.
  - 100% de payloads móviles sin token de sesión operativa válido o con despacho no autorizado rechazados con código HTTP 403 Forbidden.
* **Criterio de Validación:** Pruebas de penetración de inyección de paquetes GPS sin código de activación válido; el sistema debe descartar el payload y registrar la IP emisora en la bitácora de seguridad.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 1, 8 y 10) | [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]] (RNF-SEG-01).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de seguridad porque impone una restricción inquebrantable sobre todos los canales de entrada del sistema. No es una función en sí misma, sino el estándar de control de acceso que gobierna a [[RF001]], [[RF010]] y [[RF014]].

---

#### RNF002 — Autorización Estricta y Mínimo Privilegio (RBAC)
* **ID:** `RNF002`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-02`
* **Nombre:** Autorización Estricta y Mínimo Privilegio (RBAC).
* **Categoría:** Seguridad (Autorización / Segregación de Funciones).
* **Descripción:** El sistema debe garantizar que el 100% de las operaciones invocadas por usuarios web correspondan exclusivamente a los permisos definidos para su rol (`Administrador Principal`, `Supervisor de Distribución`, `Jefe de Distribución`, `Operador SAC`), y que la app móvil del conductor carezca de privilegios para alterar configuraciones, rutas o despachos ajenos.
* **Objetivo de Negocio:** Cumplir con los estándares de control interno de Yanbal, impidiendo que supervisores creen usuarios o que personal de atención al cliente modifique despachos.
* **Módulo o Componente Afectado:** Todos los controladores y servicios del Backend Core y Frontend Web.
* **Condición o Métrica:** Cero tolerancia a elevación horizontal o vertical de privilegios no autorizados (100% de efectividad en pruebas de matriz de roles).
* **Criterio de Validación:** Ejecución de suites de prueba de seguridad automatizadas donde un usuario autenticado con rol `Operador SAC` intenta invocar endpoints de creación o programación logística de despachos (debe retornar HTTP 403).
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 3 y 8) | [[Usuarios_Web]] (Sección 1).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de restricción de autorización. Condiciona la ejecución del requerimiento funcional [[RF004]], garantizando que el principio de mínimo privilegio sea inviolable a nivel de backend.

---

#### RNF003 — Protección y Gestión de Credenciales Web
* **ID:** `RNF003`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-03`
* **Nombre:** Protección y Gestión de Credenciales Web.
* **Categoría:** Seguridad (Gestión de Credenciales).
* **Descripción:** Las contraseñas de los usuarios de la plataforma Web deben almacenarse mediante BCrypt con factor de costo >= 12, utilizando el mecanismo de hash y salt correspondiente, y transmitirse exclusivamente a través de canales cifrados mediante TLS 1.3.
* **Objetivo de Negocio:** Proteger las credenciales de los usuarios frente a ataques de fuerza bruta, compromiso de la base de datos e interceptación durante la comunicación.
* **Módulo o Componente Afectado:** Base de Datos Principal (Tabla `USUARIOS_WEB`), Capa de Transporte HTTPS.
* **Condición o Métrica:** Todas las contraseñas almacenadas deben utilizar BCrypt con factor de costo >= 12. No se permite almacenar contraseñas en texto plano ni transmitirlas mediante canales no cifrados.
* **Criterio de Validación:** Verificación de los hashes almacenados en base de datos y comprobación de que las comunicaciones de autenticación utilizan TLS 1.3.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 4 y 10) | [[Requerimientos_No_Funcionales]] (RNF-SEG-04).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de seguridad criptográfica y gestión de credenciales. No describe qué hace el sistema, sino el mecanismo de derivación por hash y resguardo seguro de las contraseñas utilizadas en [[RF001]] y [[RF002]].

---

#### RNF004 — Gobernanza y Caducidad de Sesiones Web y Móviles
* **ID:** `RNF004`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-04`
* **Nombre:** Gobernanza y Caducidad de Sesiones Web y Móviles.
* **Categoría:** Seguridad (Gobernanza de Sesiones).
* **Descripción:** El sistema debe gobernar la caducidad y vigencia de las sesiones: en la plataforma Web, la sesión debe expirar automáticamente tras 15 minutos de inactividad del usuario; en la aplicación móvil, la sesión operativa debe expirar y revocarse de forma inmediata en cuanto el despacho pase al estado `FINALIZADO`; si el despacho se cancela antes de la activación, el código asociado debe quedar invalidado aunque no exista sesión móvil.
* **Objetivo de Negocio:** Prevenir el secuestro de sesiones en terminales desatendidas de las oficinas de distribución o en smartphones de conductores tras culminar su turno.
* **Módulo o Componente Afectado:** Módulo de Seguridad Web, Backend Core API, Almacenamiento Local PWA.
* **Condición o Métrica:**
  - Sesión Web: Inactividad máxima de 15 minutos (+/- 10 segundos).
  - Sesión Móvil: Revocación en el servidor dentro de los 5 segundos posteriores a la finalización del seguimiento.
* **Criterio de Validación:** Comprobación de que una petición web enviada al minuto 16 sin actividad es rechazada y redirigida a login; comprobación de que la PWA no permite emitir nuevas coordenadas tras pulsar "Finalizar Despacho".
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 5 y 7) | [[Funciones_Movil]] (Sección 2.6).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de política de seguridad temporal. Condiciona el ciclo de vida de los estados autenticados provistos en [[RF005]] y [[RF011]].

---

#### RNF005 — Protección de Evidencias en Almacenamiento Externo (Cloud Storage)
* **ID:** `RNF005`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-05`
* **Nombre:** Protección de Evidencias en Almacenamiento Externo.
* **Categoría:** Seguridad y Cumplimiento Legal (Privacidad y Cifrado en Reposo).
* **Descripción:** Las fotografías opcionales de respaldo de entrega o contingencias y los archivos de evidencias deben almacenarse cifrados en reposo utilizando el estándar AES-256 en almacenamiento externo de objetos (Cloud Storage / S3), y su consulta desde la web debe gestionarse exclusivamente mediante URLs prefirmadas de acceso temporal con caducidad máxima de 15 minutos.
* **Objetivo de Negocio:** Salvaguardar la privacidad de las evidencias operativas de distribución de Yanbal y evitar fugas de información o exposición indebida de buckets.
* **Módulo o Componente Afectado:** Almacenamiento de Evidencias (Object Storage / Cloud Storage), Backend de Auditoría y Consulta Web.
* **Condición o Métrica:** 100% de archivos binarios cifrados en reposo; cero exposición de buckets públicos; URLs prefirmadas no válidas tras 15 minutos de generadas.
* **Criterio de Validación:** Verificación de intento de acceso a la URL prefirmada de una fotografía tras 16 minutos de emitida (debe responder HTTP 403 con enlace expirado).
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 9) | [[Restricciones_Tecnicas]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de seguridad en almacenamiento externo. Desacopla la persistencia de imágenes pesadas de la base de datos relacional y garantiza acceso restringido temporal para [[RF018]] y [[RF025]].

---

#### RNF006 — Inmutabilidad e Integridad de Eventos y Telemetría
* **ID:** `RNF006`
* **Caso de Prueba Asociado:** `CP-RNF-INT-01`
* **Nombre:** Inmutabilidad e Integridad de Eventos y Telemetría.
* **Categoría:** Integridad de Datos y Auditoría.
* **Descripción:** Todos los registros de coordenadas GPS, eventos de despacho (`DESPACHO_CANCELADO` cuando sea informado por un sistema externo, `EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`) e incidencias persistidos en el backend deben ser estrictamente inmutables (operaciones de solo inserción / *append-only*), prohibiendo modificaciones, sobreescrituras o eliminaciones en las tablas transaccionales.
* **Objetivo de Negocio:** Garantizar la fidelidad técnica y jurídica de la traza de auditoría ante controversias con empresas transportistas o discrepancias de recepción en puntos de destino.
* **Módulo o Componente Afectado:** Base de Datos Principal (Tablas `EVENTO_DE_ENTREGA`, `COORDENADAS_GPS`, `INCIDENCIA_RUTA`).
* **Condición o Métrica:** Cero operaciones de `UPDATE` o `DELETE` permitidas sobre tablas de eventos históricos; preservación inalterable de la estampa temporal original de captura en campo (`captured_at`).
* **Criterio de Validación:** Intentos de modificación de coordenadas o timestamps mediante la API deben ser rechazados con excepción de integridad a nivel de esquema de base de datos.
* **Fuente o Evidencia:** [[Reglas_De_Integridad]] | [[Entidades_Principales]] | [[Acceso_Y_Seguridad]] (Sección 10).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de integridad de información. Asegura que los servicios funcionales de captura ([[RF014]], [[RF015]], [[RF016]], [[RF018]]) no puedan corromper la veracidad de la historia operativa.

---

### Categoría 2: Rendimiento, Latencia y Eficiencia Operativa

#### RNF007 — Latencia Máxima de Sincronización hacia el Bus Corporativo (30 min máximo)
* **ID:** `RNF007`
* **Caso de Prueba Asociado:** `CP-RNF-PERF-01`
* **Nombre:** Latencia Máxima de Sincronización hacia el Bus Corporativo.
* **Categoría:** Rendimiento (Latencia de Integración).
* **Descripción:** El SLA único de integración de Y-Trace se define en **30 minutos como máximo** para que un evento elegible sea publicado y aceptado por el Bus, contado desde su recepción en el backend. Este límite no es un intervalo de captura GPS ni una frecuencia de envío. Cuando el dispositivo está sin conectividad, los eventos se conservan localmente sin generar registros repetitivos; el cómputo del SLA comienza cuando el backend recibe el evento y lo incorpora a la cola de integración.
* **Objetivo de Negocio:** Cumplir con la meta central cuantitativa del proyecto Y-Trace: **reducir el desfase crítico actual de 2 horas en al menos un 75%** para alimentar a Maya y Salesforce.
* **Módulo o Componente Afectado:** Motor de Sincronización Móvil (PWA), Pipeline de Ingesta Backend, Conector al Bus Corporativo.
* **Condición o Métrica:** 
  - Publicación y aceptación por el Bus <= 30 minutos desde la recepción en backend para el 100% de los eventos elegibles, sin definir un objetivo inferior adicional.
  - Una vez recibido el evento en el backend, la publicación y aceptación por el Bus debe completarse en <= 30 minutos, independientemente de que el evento haya sido capturado online u offline.
* **Criterio de Validación:** Medición del delta `T_bus_ack - backend_received_at` en el 100% de eventos auditados, verificando que ningún evento elegible exceda 30 minutos desde su recepción en backend.
* **Fuente o Evidencia:** [[Problematica_Validada]] | [[Oportunidad_de_Mejora]] | [[Evidencias_Problematica]] | [[Requerimientos_No_Funcionales]] (RNF-01).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el RNF rey del proyecto. No es una función (la función es [[RF029]]); es el umbral de desempeño estricto que valida si el sistema resolvió o no la problemática empresarial comprobada por el Ing. Joao Condorpusa.

---

#### RNF008 — Tiempo de Respuesta de la Interfaz Móvil PWA (< 500 ms)
* **ID:** `RNF008`
* **Caso de Prueba Asociado:** `CP-RNF-PERF-02`
* **Nombre:** Tiempo de Respuesta de la Interfaz Móvil PWA.
* **Categoría:** Rendimiento y Usabilidad (Tiempo de Respuesta Local).
* **Descripción:** Las operaciones táctiles locales ejecutadas por el conductor en la aplicación móvil (marcar llegada a destino, registrar observaciones, adjuntar foto opcional, confirmar recepción del despacho) deben procesarse y reflejar confirmación visual en pantalla en un tiempo menor a **500 milisegundos**.
* **Objetivo de Negocio:** Evitar demoras, fricciones o bloqueos en la interacción del transportista durante su permanencia en el punto de destino de distribución.
* **Módulo o Componente Afectado:** Frontend PWA (Capa UI y Controladores Locales).
* **Condición o Métrica:** Latencia de renderizado y confirmación táctil local <= 500 ms (percentil 95) en smartphones estándar de gama media/baja.
* **Criterio de Validación:** Pruebas de rendimiento en dispositivo real midiendo el tiempo transcurrido entre el evento `pointerdown` y la actualización del DOM sin spinners de espera de red.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]] (RNF-03) | [[Offline_First]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de experiencia de usuario y desempeño local. Hace posible la arquitectura offline-first desacoplando la UI del conductor de la latencia de red.

---

#### RNF009 — Consumo Eficiente de Batería del Dispositivo Móvil (< 15% por jornada)
* **ID:** `RNF009`
* **Caso de Prueba Asociado:** `CP-RNF-EFF-01`
* **Nombre:** Consumo Eficiente de Batería del Dispositivo Móvil.
* **Categoría:** Rendimiento y Eficiencia Energética.
* **Descripción:** El funcionamiento continuo de la PWA del conductor en segundo plano (incluyendo muestreo satelital GPS, persistencia local y sincronización de red) no debe consumir más del **15% de la capacidad de la batería** del smartphone durante una jornada típica de reparto de 8 horas.
* **Objetivo de Negocio:** Asegurar la viabilidad técnica y operativa de la solución, dado que los transportistas utilizan sus propios celulares y un drenaje excesivo de batería provocaría el rechazo de la herramienta.
* **Módulo o Componente Afectado:** Módulo de Telemetría GPS Móvil, Service Worker.
* **Condición o Métrica:** Consumo acumulado de energía <= 15% en batería estándar de 4000 mAh en 8 horas de operación activa en ruta.
* **Criterio de Validación:** Monitoreo del consumo energético mediante herramientas de perfilado de Android (Battery Historian) en recorridos reales de prueba.
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] (Sección 3) | [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]] (RNF-05).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de eficiencia de recursos. Justifica técnicamente por qué el muestreo GPS es periódico y optimizado, evitando una captura continua que aumentaría consumo de batería, tráfico y volumen de almacenamiento sin aportar valor proporcional.

---

#### RNF010 — Consumo Optimizado de Datos Móviles Celulares (< 50 MB diarios)
* **ID:** `RNF010`
* **Caso de Prueba Asociado:** `CP-RNF-EFF-02`
* **Nombre:** Consumo Optimizado de Datos Móviles Celulares.
* **Categoría:** Rendimiento y Eficiencia de Red.
* **Descripción:** El tráfico total de datos móviles transmitido por la aplicación PWA (incluyendo ráfagas de telemetría GPS, payloads JSON y compresión de fotografías de evidencia) no debe superar los **50 Megabytes (MB) diarios** por unidad de transporte.
* **Objetivo de Negocio:** Evitar la saturación del plan de datos personal de los conductores y garantizar la transmisión ágil en redes celulares lentas (3G/H+) en provincias.
* **Módulo o Componente Afectado:** Pipeline de Compresión de Imágenes en PWA, Protocolo de Comunicación REST.
* **Condición o Métrica:** Volumen total de subida/bajada <= 50 MB/día para una operación diaria típica de despachos punto a punto, considerando los eventos operativos y las fotografías complementarias que efectivamente sean capturadas.
* **Criterio de Validación:** Compresión del lado del cliente de fotografías complementarias y envío de payloads JSON de telemetría optimizados; el tamaño final debe verificarse mediante prueba de carga con la operación B2B definida.
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] (Sección 3) | [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]] (RNF-05).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de consumo de ancho de banda. Condiciona el procesamiento de imágenes previo a la sincronización en [[RF018]] y [[RF022]].

---

### Categoría 3: Disponibilidad, Capacidad y Escalabilidad

#### RNF011 — Disponibilidad Operativa de la Plataforma Web y Backend (99.5%)
* **ID:** `RNF011`
* **Caso de Prueba Asociado:** `CP-RNF-REL-01`
* **Nombre:** Disponibilidad Operativa de la Plataforma Web y Backend.
* **Categoría:** Confiabilidad (Disponibilidad del Sistema).
* **Descripción:** Los servicios centrales de Y-Trace (Portal Web de supervisión y API REST de ingesta de eventos) deben mantener un índice de disponibilidad no menor al **99.5%** durante la ventana crítica de distribución y reparto nacional (lunes a sábado de 06:00 a 21:00 horas).
* **Objetivo de Negocio:** Evitar interrupciones que paralicen la Torre de Control o que impidan la recepción de telemetría de las flotas en los 24 departamentos de Perú.
* **Módulo o Componente Afectado:** Infraestructura de Servidores Backend, Balanceadores de Carga y Base de Datos Principal.
* **Condición o Métrica:** Uptime operativo >= 99.5% en horario de reparto (máximo tolerable de indisponibilidad no programada de 1.95 horas (aprox. 1 h 57 min) al mes sobre una base de 390 horas operativas).
* **Criterio de Validación:** Registros de sondas de disponibilidad externas (health checks) con muestreo cada 60 segundos hacia `/api/health`.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]] (RNF-02).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de confiabilidad y disponibilidad del servicio central.

---

#### RNF012 — Capacidad y Escalabilidad Concurrente
* **ID:** `RNF012`
* **Caso de Prueba Asociado:** `CP-RNF-SCA-01`
* **Nombre:** Capacidad y Escalabilidad Concurrente.
* **Categoría:** Confiabilidad (Capacidad y Escalabilidad).
* **Descripción:** El sistema backend de Y-Trace debe tener la capacidad de procesar la transmisión concurrente de al menos **500 conductores en ruta simultáneamente** y digerir hasta **50,000 eventos de entrega y coordenadas GPS diarios**, sin que la latencia de respuesta de la API sufra degradaciones mayores al 10%.
* **Objetivo de Negocio:** Soportar la carga máxima de operación durante las semanas de cierre de campaña comercial de Yanbal Perú (picos de mayor volumen de pedidos del año).
* **Módulo o Componente Afectado:** API Gateway, Capa de Ingesta Asíncrona, Base de Datos Transaccional.
* **Condición o Métrica:** Procesamiento concurrente de al menos 500 conductores transmitiendo en paralelo y 50,000 eventos diarios, con degradación de latencia de la API < 10% bajo carga máxima.
* **Criterio de Validación:** Ejecución de pruebas de carga y estrés (JMeter / k6) simulando 500 agentes virtuales emitiendo telemetría y subiendo eventos simultáneamente sin exceder el 10% de degradación de latencia.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]] (RNF-07).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de escalabilidad y capacidad volumétrica alineado con los límites operativos del proyecto.

---

### Categoría 4: Resiliencia y Tolerancia a Fallos (Offline-First)

#### RNF013 — Capacidad de Persistencia Local Offline (>= 500 eventos)
* **ID:** `RNF013`
* **Caso de Prueba Asociado:** `CP-RNF-RES-01`
* **Nombre:** Capacidad de Persistencia Local Offline.
* **Categoría:** Resiliencia (Tolerancia a Desconexión de Red).
* **Descripción:** El motor de persistencia local de la aplicación móvil (PWA) debe ser capaz de almacenar de forma íntegra y segura al menos **500 eventos operativos** (por ejemplo estados, coordenadas GPS, incidencias y datos de entrega) durante una ausencia prolongada de red, sin pérdida ni corrupción de información. Las fotografías, cuando existan, se gestionan como adjuntos independientes y no deben invalidar la garantía mínima de 500 eventos.
* **Objetivo de Negocio:** Permitir que los despachos que atraviesan zonas sin cobertura completen su operación y sincronicen posteriormente la información acumulada.
* **Módulo o Componente Afectado:** Almacenamiento Local PWA (`IndexedDB`) y cola offline.
* **Condición o Métrica:** Retención íntegra de >= 500 eventos operativos pendientes de sincronización, manteniendo sus identificadores y `captured_at` originales. Los adjuntos fotográficos se gestionan de forma independiente y no se contabilizan como eventos adicionales para este umbral.
* **Criterio de Validación:** Simulación de 500 eventos en modo desconectado, reinicio del teléfono y validación de la integridad del 100% de los eventos retenidos y de los adjuntos que hayan sido efectivamente capturados.
* **Fuente o Evidencia:** [[Offline_First]] | [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]].
* **Estado:** Confirmado.
* **Análisis Crítico:** El umbral se define sobre eventos operativos y no sobre 500 fotografías, evitando una contradicción artificial entre la capacidad de almacenamiento y el tamaño variable de los archivos multimedia.

---

#### RNF014 — Idempotencia y Garantía de Entrega en Sincronización
* **ID:** `RNF014`
* **Caso de Prueba Asociado:** `CP-RNF-INT-02`
* **Nombre:** Idempotencia y Garantía de Entrega en Sincronización.
* **Categoría:** Integridad y Confiabilidad de Integración.
* **Descripción:** Los servicios backend de ingesta de eventos de Y-Trace deben ser estrictamente **idempotentes**, garantizando que el reenvío múltiple de una misma transacción (ocasionado por cortes de red o timeouts durante el viaje) no duplique registros de paradas, eventos o coordenadas en la base de datos.
* **Objetivo de Negocio:** Evitar inconsistencias de datos, duplicación de estados o alteraciones en las métricas de cumplimiento de entrega.
* **Módulo o Componente Afectado:** API de Sincronización Backend, Capa de Servicios de Eventos.
* **Condición o Métrica:** Procesamiento de identificadores universales (UUIDv4) únicos generados en el dispositivo móvil; si un UUID ya fue procesado, el servidor debe responder con acuse de éxito (HTTP 200) sin duplicar el registro en la base de datos.
* **Criterio de Validación:** Envío reiterado de un mismo payload 10 veces consecutivas; el conteo final de registros en BD debe ser exactamente 1.
* **Fuente o Evidencia:** [[Offline_First]] (Sección 4) | [[Componentes_Aplicacion]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de integridad transaccional. Condiciona la arquitectura del backend para procesar con seguridad la cola de eventos emitida por [[RF022]].

---

### Categoría 5: Usabilidad, Accesibilidad y Rendimiento Humano

#### RNF015 — Usabilidad y Ergonomía Operativa para Campo (máximo 3 toques)
* **ID:** `RNF015`
* **Caso de Prueba Asociado:** `CP-RNF-USA-01`
* **Nombre:** Usabilidad y Ergonomía Operativa para Campo.
* **Categoría:** Usabilidad y Ergonomía de Software.
* **Descripción:** La interfaz de la aplicación móvil del conductor debe estar diseñada ergonómicamente para su utilización en campo, permitiendo completar cualquier flujo operativo crítico (marcar llegada en puerta, confirmar entrega o reportar incidencia) en un **máximo de 3 toques de pantalla**, con botones de alto contraste y tamaño mínimo accesible.
* **Objetivo de Negocio:** Minimizar las distracciones del conductor en cabina, acelerar la atención física en puerta y asegurar la rápida adopción por transportistas de cualquier nivel de alfabetización digital.
* **Módulo o Componente Afectado:** Frontend PWA (Capa UI y Diseño de Interacción).
* **Condición o Métrica:**
  - Máximo 3 interacciones táctiles requeridas desde la hoja de ruta para completar un registro.
  - Dimensiones mínimas de botones interactivos de 48x48 dp (según estándares Material Design / WCAG).
  - Paleta de colores con contraste mínimo de 4.5:1 legible bajo luz solar intensa en calle.
* **Criterio de Validación:** Evaluación de usabilidad con conductores midiendo tiempos de tarea en calle bajo simulación operativa real.
* **Fuente o Evidencia:** [[Funciones_Movil]] (Sección 2) | [[Flujo_Conductor]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de usabilidad y diseño centrado en el usuario. Condiciona la ergonomía visual de los requerimientos [[RF012]], [[RF015]], [[RF016]] y [[RF017]].

---

#### RNF016 — Tiempo de Respuesta en Consulta de Trazabilidad (< 2 segundos)
* **ID:** `RNF016`
* **Caso de Prueba Asociado:** `CP-RNF-PERF-03`
* **Nombre:** Tiempo de Respuesta en Consulta de Trazabilidad.
* **Categoría:** Rendimiento (Tiempo de Consulta Web).
* **Descripción:** El módulo de consulta rápida de trazabilidad de despachos en la plataforma Web debe devolver la cronología completa del traslado, mapa de recorrido y evidencias asociadas en un tiempo no mayor a **2.0 segundos** tras introducir el código de despacho o placa de la unidad.
* **Objetivo de Negocio:** Permitir a los supervisores y operadores responder de forma inmediata sobre el estado y ubicación de cualquier carga en ruta.
* **Módulo o Componente Afectado:** Base de Datos (Índices B-Tree en `codigo_despacho` y `placa_vehiculo`), Endpoint de Consulta de Trazabilidad.
* **Condición o Métrica:** Tiempo total de respuesta de la API y renderizado final en pantalla <= 2.0 segundos sobre un histórico de al menos 1,000,000 de registros.
* **Criterio de Validación:** Pruebas de carga sobre base de datos poblada con 1 millón de despachos históricos; el tiempo de respuesta debe mantenerse inferior a 2 s.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Flujo_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de rendimiento de consulta de datos. Condiciona directamente la efectividad del requerimiento funcional [[RF027]].

---

### Categoría 6: Compatibilidad, Portabilidad e Interoperabilidad

#### RNF017 — Compatibilidad Exclusiva con Plataforma Android (Chrome / Servicios de Android)
* **ID:** `RNF017`
* **Caso de Prueba Asociado:** `CP-RNF-COMP-01`
* **Nombre:** Compatibilidad Exclusiva con Plataforma Android.
* **Categoría:** Compatibilidad y Plataforma Móvil.
* **Descripción:** La aplicación móvil del conductor debe funcionar de forma homogénea exclusivamente como Progressive Web App (PWA) sobre el navegador Google Chrome para Android (versión 8.0 Oreo o superior con Google Chrome v90+), utilizando los servicios nativos de geolocalización disponibles en el sistema operativo Android. No se contempla soporte para iOS ni navegadores del ecosistema Apple en el alcance del proyecto.
* **Objetivo de Negocio:** Concentrar los recursos de desarrollo, soporte y homologación en la flota homogénea de dispositivos Android de los transportistas de Yanbal, evitando costes y complejidades de desarrollo o compatibilidad con Safari/iOS.
* **Módulo o Componente Afectado:** Core PWA Móvil, Service Worker, Web APIs nativas de Android (Geolocation API, IndexedDB, MediaDevices / Camera API).
* **Condición o Métrica:** 100% de funcionalidades operativas (GPS, captura fotográfica, persistencia offline) en dispositivos con Android 8.0+ y Google Chrome v90+. Plataforma iOS formalmente excluida del alcance.
* **Criterio de Validación:** Pruebas de compatibilidad sobre terminales físicos Android representativos del mercado logístico local (Samsung, Xiaomi, Motorola).
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Funciones_Movil]] | Decisión de Arquitectura: Exclusividad Android.
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de compatibilidad y portabilidad que fija una restricción arquitectónica determinante: delimita el alcance a Android y descarga al equipo del mantenimiento multiplataforma para terminales Apple inexistentes en la flota de reparto.

---

#### RNF018 — Estandarización de Formatos de Intercambio JSON Canónico
* **ID:** `RNF018`
* **Caso de Prueba Asociado:** `CP-RNF-INT-03`
* **Nombre:** Estandarización de Formatos de Intercambio JSON Canónico.
* **Categoría:** Interoperabilidad de Sistemas.
* **Descripción:** Los eventos de integración se estructuran mediante payloads JSON conforme al esquema canónico definido para la interoperabilidad con el Bus corporativo de Yanbal.
* **Objetivo de Negocio:** Garantizar la interoperabilidad fluida y desacoplada con el middleware corporativo de Yanbal ([[Bus_Integracion]]).
* **Módulo o Componente Afectado:** Conector al Bus de Integración, Serializadores JSON de Backend.
* **Condición o Métrica:** Mensajes de integración formateados en JSON conforme al esquema canónico establecido para los eventos del despacho.
* **Criterio de Validación:** Validación de estructura JSON en los eventos publicados hacia el Bus de Integración.
* **Fuente o Evidencia:** [[Bus_Integracion]] | [[Integracion_Bus_Eventos]] | [[Integraciones_Aplicacion]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Precisión técnica de formato que gobierna la serialización de datos para la interoperabilidad con el Bus en [[RF029]], [[RF030]] y [[RF031]].

---

### Categoría 7: Calidad del Dato, Continuidad, Privacidad y Accesibilidad Web

#### RNF019 — Retención y Eliminación de Evidencias y Registros Operativos
* **ID:** `RNF019`
* **Caso de Prueba Asociado:** `CP-RNF-LEG-01`
* **Nombre:** Retención y Eliminación de Evidencias y Registros Operativos.
* **Categoría:** Seguridad y Gobernanza de Datos (Cumplimiento de Retención).
* **Descripción:** Las evidencias operativas y, cuando existan, las fotografías de respaldo, registros de coordenadas satelitales y trazas de despachos deben conservarse durante el plazo definido por la política corporativa de retención de Yanbal (establecido preliminarmente en 24 meses) y eliminarse o anonimizarse irreversiblemente de forma automática una vez vencido dicho plazo.
* **Objetivo de Negocio:** Cumplir con las directrices de gobernanza de la información y optimización de costos de almacenamiento cloud en Yanbal.
* **Módulo o Componente Afectado:** Servicio de Almacenamiento de Evidencias (Object Storage / Cloud Storage), Base de Datos Relacional, Job de Purga y Archivado.
* **Condición o Métrica:** 100% de los registros y archivos binarios cuya antigüedad supere los 24 meses deben ser anonimizados o eliminados físicamente en el siguiente ciclo semanal de purga programada.
* **Criterio de Validación:** Ejecución auditable del job de purga automática en ambiente de pruebas sobre registros simulados con antigüedad > 24 meses; verificación de eliminación de objetos en storage y purga en base de datos.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 9) | [[RNF005]].
* **Estado:** Pendiente de validación.
* **Nota de Regla Pendiente de Validación (RN-PV-04):** El mecanismo de purga/anonimización automática y su arquitectura (job semanal, append-only, storage externo) quedan confirmados como necesidad funcional. Lo que permanece pendiente es el **valor exacto del plazo** (24 meses es una cifra preliminar de referencia, no un número validado por Legal Yanbal); debe ratificarse formalmente antes de configurarlo como parámetro productivo.
* **Análisis Crítico:** Es un RNF de ciclo de vida de datos que gobierna la expiración y purga programada de las evidencias capturadas en [[RF018]].

---

#### RNF020 — Recuperación ante Desastres (RPO y RTO)
* **ID:** `RNF020`
* **Caso de Prueba Asociado:** `CP-RNF-DIS-01`
* **Nombre:** Recuperación ante Desastres (RPO y RTO).
* **Categoría:** Confiabilidad (Disponibilidad y Recuperación ante Desastres).
* **Descripción:** El sistema debe definir y garantizar métricas estrictas de Objetivo de Punto de Recuperación (RPO) y Objetivo de Tiempo de Recuperación (RTO) ante un escenario de caída catastrófica o pérdida total del centro de datos o infraestructura principal en la nube.
* **Objetivo de Negocio:** Garantizar la continuidad de las operaciones de distribución de Yanbal a nivel nacional, minimizando el impacto comercial, el retraso en procesos posteriores de los sistemas externos y la pérdida de comprobantes de entrega ante desastres.
* **Módulo o Componente Afectado:** Infraestructura Cloud, Base de Datos (replicación continua y snapshots), API de Ingesta, Storage de Evidencias.
* **Condición o Métrica:**
  - **RPO (Recovery Point Objective):** <= 15 minutos (pérdida máxima tolerable de transacciones o telemetría).
  - **RTO (Recovery Time Objective):** <= 4 horas (tiempo máximo tolerable para restaurar la operatividad completa del sistema en infraestructura secundaria).
* **Criterio de Validación:** Ejecución semestral de simulacro de recuperación ante desastres (*DR drill*) con conmutación por error hacia zona de contingencia, cronometrando la restauración del servicio y la consistencia transaccional.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | Políticas corporativas de continuidad de negocio de Yanbal | [[RNF011]].
* **Estado:** Pendiente de validación.
* **Nota de Regla Pendiente de Validación (RN-PV-02):** Los valores de RPO <= 15 min y RTO <= 4 h son una propuesta de arquitectura basada en buenas prácticas del sector, no un SLA acordado. Deben confirmarse formalmente con el área de Infraestructura/TI de Yanbal, ya que condicionan decisiones costosas de topología (réplicas multizona, frecuencia de snapshots).
* **Análisis Crítico:** Es un RNF de resiliencia y continuidad de infraestructura. Establece los límites de tolerancia operacional que condicionan la arquitectura física, la topología de base de datos multizona y la política de copias de seguridad de Y-Trace.

---

#### RNF021 — Precisión Mínima de Geolocalización en Dispositivos Android
* **ID:** `RNF021`
* **Caso de Prueba Asociado:** `CP-RNF-GEO-01`
* **Nombre:** Precisión Mínima de Geolocalización en Dispositivos Android.
* **Categoría:** Calidad del Dato y Rendimiento (Precisión Geoespacial).
* **Descripción:** Las coordenadas GPS capturadas mediante los servicios de geolocalización de Android deben presentar un radio de precisión (*accuracy*) reportado por el hardware del dispositivo no mayor a **50 metros** en condiciones de cielo abierto. Aquellas lecturas satelitales que presenten una imprecisión mayor a 50 metros no deben ser descartadas, sino catalogadas y persistidas con una bandera de `"baja confiabilidad"` para conocimiento de la Torre de Control y algoritmos de geocerca.
* **Objetivo de Negocio:** Asegurar la calidad y confiabilidad del dato satelital que actúa como evidencia geoespacial de arribo ([[RF015]]) y auditoría de presencia del transportista.
* **Módulo o Componente Afectado:** Módulo Móvil de Telemetría GPS (PWA Android), Backend de Ingesta GPS.
* **Condición o Métrica:**
  - Al menos el 90% de las lecturas capturadas en campo bajo cielo abierto deben presentar `accuracy <= 50 m`.
  - 100% de las lecturas con `accuracy > 50 m` etiquetadas con atributo `baja_confiabilidad = true` tanto en el payload como en la base de datos.
* **Criterio de Validación:** Pruebas de campo con emulación de señales satelitales degradadas (zonas bajo techo o túneles); comprobación de que el dato se persiste y el backend asigna el indicador de confiabilidad correspondiente.
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] | [[RF014]] | [[RF015]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de calidad del dato. Define el umbral métrico de confiabilidad de la lectura GPS sin descartar paquetes ni interrumpir la operación del conductor.

---

#### RNF022 — Accesibilidad Web para Consola Administrativa (WCAG 2.1 Nivel AA)
* **ID:** `RNF022`
* **Caso de Prueba Asociado:** `CP-RNF-ACC-01`
* **Nombre:** Accesibilidad Web para Consola Administrativa.
* **Categoría:** Usabilidad y Accesibilidad Web.
* **Descripción:** La plataforma Web de Y-Trace (utilizada por Supervisores de Distribución, Jefes de Distribución y Agentes SAC) debe construirse en estricto cumplimiento del estándar internacional de accesibilidad **WCAG 2.1 Nivel AA**, asegurando ratios de contraste adecuados, compatibilidad con lectores de pantalla, navegación completa por teclado y semántica HTML5 accesible.
* **Objetivo de Negocio:** Cierra la asimetría de diseño del proyecto: mientras RNF015 protege la ergonomía móvil del conductor, la plataforma web corporativa debe garantizar condiciones óptimas para jornadas continuas de supervisión de 8 horas y accesibilidad para colaboradores con discapacidades visuales o motrices.
* **Módulo o Componente Afectado:** Toda la interfaz de usuario web (Módulo de Despachos, Monitoreo y Grilla Operativa de Despachos, Gestión de Incidencias, Consulta SAC y Dashboard).
* **Condición o Métrica:**
  - Relación de contraste de texto normal >= 4.5:1 y texto grande / componentes gráficos >= 3:1.
  - Navegación completa por teclado accesible con foco visible en el 100% de botones, formularios y tablas.
  - Puntaje de accesibilidad >= 90 / 100 en auditorías automatizadas (Google Lighthouse / axe-core).
* **Criterio de Validación:** Evaluación mediante herramientas automatizadas (Lighthouse, axe DevTools) y pruebas manuales con teclado y lectores de pantalla (NVDA / VoiceOver).
* **Fuente o Evidencia:** WCAG 2.1 Nivel AA (W3C) | [[Funciones_Web]] | Complemento de [[RNF015]].
* **Estado:** Pendiente de validación.
* **Nota de Regla Pendiente de Validación (RN-PV-03):** El estándar técnico (WCAG 2.1 AA) y su forma de medición quedan definidos. Lo pendiente es la aprobación de negocio del presupuesto y cronograma adicional que implica construir la consola web bajo este estándar desde el diseño, ya que no fue un requisito solicitado originalmente por el área usuaria.
* **Análisis Crítico:** Es un RNF de accesibilidad y ergonomía de interfaz web. Asegura que el software corporativo cumpla con los estándares internacionales de inclusión digital y reduzca la fatiga visual de los operadores de la Torre de Control de Yanbal.

---

#### RNF023 — Inmutabilidad de la Bitácora de Auditoría Administrativa
* **ID:** `RNF023`
* **Caso de Prueba Asociado:** `CP-RNF-INT-04`
* **Nombre:** Inmutabilidad de la Bitácora de Auditoría Administrativa.
* **Categoría:** Seguridad e Integridad de Datos.
* **Descripción:** Los registros de la bitácora de auditoría administrativa y de seguridad generados por [[RF006]] deben almacenarse bajo una arquitectura estricta de **solo anexión (append-only)** a nivel de base de datos relacional, prohibiendo de forma irrevocable operaciones de actualización (`UPDATE`) o eliminación (`DELETE`) mediante restricciones de esquema, triggers y roles de base de datos sin permisos destructivos.
* **Objetivo de Negocio:** Garantizar la validez legal, técnica y forense de las evidencias de auditoría interna de Yanbal, previniendo que administradores o actores maliciosos puedan encubrir acciones fraudulentas borrando rastros.
* **Módulo o Componente Afectado:** Base de Datos Relacional (esquema `audit_logs`), Capa de Persistencia Backend.
* **Condición o Métrica:** Cero tolerancia a modificaciones o eliminaciones en tablas de auditoría (100% de rechazo inmediato por el motor de base de datos ante sentencias `UPDATE` o `DELETE`).
* **Criterio de Validación:** Ejecución deliberada de comandos SQL `UPDATE` y `DELETE` sobre la tabla de auditoría con la cuenta de servicio de la aplicación; el gestor relacional debe abortar la transacción con error de violación de permisos o excepción de trigger.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 4) | [[RF006]] | Simetría con la inmutabilidad de eventos de entrega de [[RNF006]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RNF de integridad y no repudiación. Asegura que la bitácora de auditoría web ([[RF006]]) permanezca inalterable ante cualquier intento de manipulación en base de datos.

