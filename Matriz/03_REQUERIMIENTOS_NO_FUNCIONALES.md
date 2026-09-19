# Catálogo Oficial de Requerimientos No Funcionales: Y-Trace (RNF001 – RNF022)

> **Carpeta:** `detalles`  
> **Documento:** `03_REQUERIMIENTOS_NO_FUNCIONALES.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Propósito:** Especificación formal, exhaustiva y medible de todos los Requerimientos No Funcionales (RNF) del sistema, organizados bajo el estándar internacional de calidad ISO/IEC 25010 (SQuaRE).  
> **Estado:** [CONSOLIDADO OFICIAL — ALCANCE B2B PUNTO A PUNTO CONGELADO (22 RNF)]  
>  
> 🔗 **Documentos Vinculados:**  
> - Matriz Maestra Unificada: [[01_MATRIZ_DE_REQUERIMIENTOS]]  
> - Requerimientos Funcionales: [[02_REQUERIMIENTOS_FUNCIONALES]]  
> - Resumen Operativo y Trazabilidad: [[04_RESUMEN_OPERATIVO_Y_TRAZABILIDAD_REQUERIMIENTOS]]  
> - Nodo Central del Proyecto: [[MAPA_MAESTRO_DEL_PROYECTO]]  
> - Restricciones Técnicas: [[Restricciones_Tecnicas]] | [[Definicion_Tecnologica]]  

---

## 1. Marco Metodológico y Definición del Requerimiento No Funcional

En la arquitectura empresarial de Y-Trace, un **Requerimiento No Funcional (RNF)** responde estrictamente a la pregunta:

> **"¿Cómo debe funcionar el sistema o qué atributo de calidad, restricción o condición técnica debe satisfacer Y-Trace?"**

No representa una acción o comportamiento que el usuario pueda ejecutar directamente (como presionar un botón o ver una pantalla), sino una **propiedad emergente del sistema**, un **límite de desempeño** o un **estándar de seguridad y resiliencia** que condiciona cómo se implementan los requerimientos funcionales.

### Principio Antirredundancia y Clasificación Crítica:
* **No clasificar como RNF una función solo por contener palabras como "automático", "seguro" o "rápido":** Por ejemplo, *"Publicar eventos al Bus"* es un Requerimiento Funcional ([[RF025]]); que la latencia de dicha publicación sea como máximo de *30 minutos* es el Requerimiento No Funcional asociado (`RNF006`).
* **Métricas Objetivas y Verificables:** Todo RNF cuenta con una unidad de medida, un umbral de aceptación y un método de prueba para evitar declaraciones ambiguas. Cuando un parámetro técnico dependa de validaciones pendientes con TI de Yanbal, se consigna expresamente como *"Pendiente de validación"*.
* **Delimitación de Plataforma Móvil (Exclusividad Android):** La aplicación móvil de Y-Trace está destinada **exclusivamente a dispositivos Android** (versión 8.0 Oreo o superior) como App Nativa. La captura de ubicación GPS se realizará mediante los servicios de geolocalización disponibles en Android. **No se contempla compatibilidad con iOS ni formato PWA en el alcance del proyecto.**
* **Definición del SLA central de integración (30 minutos):** El valor de **30 minutos es el límite máximo de publicación y aceptación de un evento en el Bus de Integración**, contado desde que el backend de Y-Trace recibe el evento y lo incorpora a la cola de integración. Este valor no representa una frecuencia de captura GPS ni una frecuencia fija de envío. Durante una desconexión del dispositivo, los eventos se conservan localmente sin generar registros artificiales y, al recuperar conectividad, se transmiten al backend; desde esa recepción comienza el cómputo del SLA de 30 minutos.

### 1.1 Taxonomía de Estados de Certeza en Requerimientos No Funcionales
* **Confirmado:** Atributo de calidad o restricción respaldada explícitamente por entrevistas oficiales (Ing. Joao Condorpusa), reglas de negocio validadas o definiciones corporativas de Yanbal (ej. SLA de 30 min al Bus, muestreo GPS cada 10 min en ruta, compatibilidad nativa Android 8.0+, navegadores modernos, BCrypt >= 12, TLS 1.3, autenticación local sin Azure AD, inmutabilidad transaccional de eventos y bitácora, UTF-8 canónico, idempotencia UUIDv4).
* **Propuesta Técnica:** Parámetro cuantitativo o umbral de ingeniería propuesto por la arquitectura de software para garantizar rendimiento, resiliencia y ergonomía (ej. inactividad web de 15 min, tiempo de respuesta móvil < 500 ms, consumo de batería < 15% en 8 h, consumo de datos móviles < 50 MB diarios, escalabilidad de 500 conductores concurrentes / 50,000 eventos diarios, capacidad offline de 500 eventos en SQLite, regla ergonómica de 3 toques, renderizado de consulta web < 2.0 s, precisión GPS <= 50 m en Android).
* **Pendiente de validación:** Parámetro técnico, legal o presupuestal que requiere ratificación formal de SLA o aprobación contractual con TI, Legal u Operaciones de Yanbal (ej. disponibilidad del 99.5%, plazo legal de retención de 24 meses, RPO <= 15 min / RTO <= 4 h ante desastres, estándar de accesibilidad WCAG 2.1 AA).

---

## 2. Catálogo Detallado de Requerimientos No Funcionales (RNF001 – RNF022)

---

### Categoría 1: Seguridad, Privacidad y Gobernanza de Acceso

#### RNF001 — Seguridad en el Control de Acceso Dual (Web vs. Móvil)
* **ID:** `RNF001`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-01`
* **Nombre:** Seguridad en el Control de Acceso Dual (Web vs. Móvil).
* **Categoría:** Seguridad (Control de Acceso / Autenticación).
* **Descripción:** El sistema debe impedir taxativamente el acceso no autorizado a las consolas de la plataforma Web y bloquear cualquier intento de transmisión de telemetría GPS o registro de eventos desde dispositivos móviles que no cuenten con una sesión operativa válida vinculada a un despacho existente y habilitado para seguimiento.
* **Objetivo de Negocio:** Proteger la confidencialidad de la información corporativa y evitar el sabotaje, inyección de datos ficticios o fugas de información de las rutas de entrega.
* **Módulo o Componente Afectado:** API Gateway, Módulo de Seguridad Web RBAC, Módulo de Activación Móvil.
* **Condición o Métrica:** 
  - 100% de peticiones sin credenciales web válidas rechazadas con código HTTP 401 Unauthorized.
  - 100% de payloads móviles sin token de sesión operativa válido o con despacho no autorizado rechazados con código HTTP 403 Forbidden.
* **Criterio de Validación:** Pruebas de penetración de inyección de paquetes GPS sin token de activación válido; el sistema debe descartar el payload y registrar la IP emisora en la bitácora de seguridad.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]].
* **Estado:** Confirmado.
* **Análisis Crítico:** RNF de seguridad perimetral que gobierna a [[RF001]], [[RF010]] y [[RF014]].

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
* **Criterio de Validación:** Suites automatizadas donde un usuario con rol `Operador SAC` intenta invocar endpoints de cancelación forzada o administración de cuentas (debe retornar HTTP 403).
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Usuarios_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Condiciona la ejecución del requerimiento funcional [[RF004]], garantizando que el principio de mínimo privilegio sea inviolable a nivel de backend.

---

#### RNF003 — Protección y Gestión de Credenciales Web
* **ID:** `RNF003`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-03`
* **Nombre:** Protección y Gestión de Credenciales Web.
* **Categoría:** Seguridad (Gestión de Credenciales).
* **Descripción:** Las contraseñas de los usuarios de la plataforma Web deben almacenarse mediante BCrypt con factor de costo >= 12, utilizando el mecanismo de hash y salt correspondiente, y transmitirse exclusivamente a través de canales cifrados mediante TLS 1.3.
* **Objetivo de Negocio:** Proteger las credenciales de los usuarios frente a ataques de fuerza bruta, compromiso de la base de datos e interceptación durante la comunicación.
* **Módulo o Componente Afectado:** Base de Datos Principal (Tabla `USUARIOS_WEB`), Capa de Transporte HTTPS.
* **Condición o Métrica:** Todas las contraseñas almacenadas deben utilizar BCrypt con factor de costo >= 12. Prohibido el almacenamiento en texto plano o transmisión sin TLS 1.3.
* **Restricción y Decisión Arquitectónica:** Autenticación local delegada al Backend Core mediante Spring Security y base de datos relacional interna, sin integración con Azure AD / Microsoft Entra ID.
* **Criterio de Validación:** Verificación de los hashes almacenados en base de datos y comprobación de que las comunicaciones de autenticación utilizan TLS 1.3.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Requerimientos_No_Funcionales]].
* **Estado:** Confirmado.
* **Análisis Crítico:** RNF de seguridad criptográfica que resguarda las contraseñas utilizadas en [[RF001]] y [[RF002]].

---

#### RNF004 — Gobernanza y Caducidad de Sesiones Web y Móviles
* **ID:** `RNF004`
* **Caso de Prueba Asociado:** `CP-RNF-SEC-04`
* **Nombre:** Gobernanza y Caducidad de Sesiones Web y Móviles.
* **Categoría:** Seguridad (Gobernanza de Sesiones).
* **Descripción:** El sistema debe gobernar la caducidad y vigencia de las sesiones: en la plataforma Web, la sesión debe expirar automáticamente tras 15 minutos de inactividad del usuario (parámetro propuesto por ingeniería de software); en la aplicación móvil, la sesión operativa debe expirar y revocarse de forma inmediata en cuanto el despacho pase al estado `FINALIZADO` o `DESPACHO_CANCELADO`; si el despacho se cancela antes de la activación, el código asociado debe quedar invalidado de inmediato.
* **Objetivo de Negocio:** Prevenir el secuestro de sesiones en terminales desatendidas de las oficinas de distribución o en smartphones de conductores tras culminar su turno.
* **Módulo o Componente Afectado:** Módulo de Seguridad Web, Backend Core API, Almacenamiento Seguro App Nativa.
* **Condición o Métrica:**
  - Sesión Web: Inactividad máxima de 15 minutos (+/- 10 segundos). Propuesta técnica de diseño seguro.
  - Sesión Móvil: Revocación en el servidor dentro de los 5 segundos posteriores a la finalización o cancelación del seguimiento.
* **Criterio de Validación:** Comprobación de que una petición web enviada al minuto 16 sin actividad es rechazada y redirigida a login; comprobación de que la App Nativa no permite emitir nuevas coordenadas tras finalizar o cancelar el despacho.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Funciones_Movil]].
* **Estado:** Propuesta Técnica.
* **Nota de Estado:** El umbral de inactividad web de 15 min es una propuesta técnica de seguridad de la información; la revocación inmediata de la sesión móvil al finalizar/cancelar el viaje es regla de negocio confirmada.
* **Análisis Crítico:** Condiciona el ciclo de vida de los estados autenticados provistos en [[RF005]], [[RF011]], [[RF018]] y [[RF009]].

---

#### RNF005 — Inmutabilidad e Integridad de Eventos y Telemetría Operativa
* **ID:** `RNF005`
* **Caso de Prueba Asociado:** `CP-RNF-INT-01`
* **Nombre:** Inmutabilidad e Integridad de Eventos y Telemetría Operativa.
* **Categoría:** Integridad de Datos y Auditoría Operativa.
* **Descripción:** Todos los registros de coordenadas GPS y eventos operativos de despacho (`EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`, `DESPACHO_CANCELADO`) persistidos en el backend deben ser estrictamente inmutables (operaciones de solo inserción / *append-only*), prohibiendo modificaciones o eliminaciones durante su ciclo de vida activo; la única eliminación física permitida corresponde a la purga administrativa automatizada por vencimiento de la política corporativa de retención ([[RNF018]]).
* **Objetivo de Negocio:** Garantizar la fidelidad técnica y jurídica de la traza de auditoría ante controversias con empresas transportistas o discrepancias de recepción en puntos de destino.
* **Módulo o Componente Afectado:** Base de Datos Principal (Tablas `EVENTO_DE_ENTREGA`, `COORDENADAS_GPS`).
* **Condición o Métrica:** Cero operaciones de `UPDATE` o `DELETE` ejecutadas por usuarios o aplicaciones sobre tablas de eventos históricos y coordenadas; preservación inalterable de la estampa temporal original de captura en campo (`captured_at`). Única excepción técnica programada: ejecución del job de depuración por retención corporativa ([[RNF018]]).
* **Criterio de Validación:** Intentos de modificación de coordenadas o timestamps mediante la API deben ser rechazados con excepción de integridad a nivel de base de datos.
* **Fuente o Evidencia:** [[Reglas_De_Integridad]] | [[Acceso_Y_Seguridad]].
* **Estado:** Confirmado.
* **Nota de Armonización:** Armonizado con RNF018: la inmutabilidad transaccional rige de forma absoluta durante la vida útil del dato; al vencer el plazo legal de retención, opera la purga automatizada programada sin intervención manual.
* **Análisis Crítico:** Asegura que los servicios funcionales de captura ([[RF013]], [[RF014]], [[RF015]], [[RF016]], [[RF017]], [[RF009]]) no puedan corromper la veracidad de la historia operativa, manteniéndose separado de la bitácora administrativa ([[RNF022]]).

---

### Categoría 2: Rendimiento, Latencia e Integración

#### RNF006 — Latencia Máxima de Sincronización hacia el Bus Corporativo (30 min máximo)
* **ID:** `RNF006`
* **Caso de Prueba Asociado:** `CP-RNF-PERF-01`
* **Nombre:** Latencia Máxima de Sincronización hacia el Bus Corporativo.
* **Categoría:** Rendimiento (Latencia de Integración).
* **Descripción:** El SLA único de integración de Y-Trace se define en **30 minutos como máximo** para que un evento elegible sea publicado y aceptado por el Bus de Integración, contado desde su recepción en el backend. Este límite no es un intervalo de captura GPS ni una frecuencia de envío. Cuando el dispositivo está sin conectividad, los eventos se conservan localmente sin generar registros repetitivos; el cómputo del SLA comienza cuando el backend recibe el evento y lo incorpora a la cola de integración.
* **Objetivo de Negocio:** Cumplir con la meta central cuantitativa del proyecto Y-Trace: **reducir el desfase crítico actual de 2 horas en al menos un 75%** para alimentar a Maya, SAP y Salesforce.
* **Módulo o Componente Afectado:** Pipeline de Ingesta Backend, Cola de Mensajería, Conector al Bus Corporativo.
* **Condición o Métrica:** Publicación y aceptación por el Bus <= 30 minutos desde la recepción en backend para el 100% de los eventos elegibles (`EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`, `DESPACHO_CANCELADO`).
* **Criterio de Validación:** Medición del delta `T_bus_ack - backend_received_at` en el 100% de eventos auditados, verificando que ningún evento elegible exceda 30 minutos.
* **Fuente o Evidencia:** [[Problematica_Validada]] | [[Oportunidad_de_Mejora]] | [[Evidencias_Problematica]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Umbral de desempeño estricto que valida si el sistema resolvió o no la problemática empresarial central; gobierna a [[RF025]].

---

#### RNF007 — Tiempo de Respuesta de la Interfaz Móvil App Nativa (< 500 ms)
* **ID:** `RNF007`
* **Caso de Prueba Asociado:** `CP-RNF-PERF-02`
* **Nombre:** Tiempo de Respuesta de la Interfaz Móvil App Nativa.
* **Categoría:** Rendimiento y Usabilidad (Tiempo de Respuesta Local).
* **Descripción:** Las operaciones táctiles locales ejecutadas por el conductor en la aplicación móvil (marcar inicio de ruta, marcar llegada a destino, registrar causal, confirmar recepción del despacho) deben procesarse y reflejar confirmación visual en pantalla en un tiempo menor a **500 milisegundos**.
* **Objetivo de Negocio:** Evitar demoras, fricciones o bloqueos en la interacción del transportista durante su permanencia en andén o en el punto de destino.
* **Módulo o Componente Afectado:** Frontend App Nativa (Capa UI y Controladores Locales Android).
* **Condición o Métrica:** Latencia de renderizado y confirmación táctil local <= 500 ms (percentil 95) en smartphones estándar de gama media/baja. Sin carga de fotos.
* **Criterio de Validación:** Pruebas de rendimiento en dispositivo físico midiendo el tiempo transcurrido entre el evento táctil y la actualización de la pantalla sin spinners bloqueantes de red.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Offline_First]].
* **Estado:** Propuesta Técnica.
* **Análisis Crítico:** Desacopla la experiencia de usuario de la latencia de red, garantizando agilidad inmediata para el transportista.

---

#### RNF008 — Consumo Eficiente de Batería del Dispositivo Móvil (< 15% por jornada)
* **ID:** `RNF008`
* **Caso de Prueba Asociado:** `CP-RNF-EFF-01`
* **Nombre:** Consumo Eficiente de Batería del Dispositivo Móvil.
* **Categoría:** Rendimiento y Eficiencia Energética.
* **Descripción:** El funcionamiento continuo de la App Nativa del conductor en segundo plano (incluyendo muestreo satelital GPS cada 10 minutos, persistencia local y sincronización de red) no debe consumir más del **15% de la capacidad de la batería** del smartphone durante una jornada típica de reparto de 8 horas.
* **Objetivo de Negocio:** Asegurar la viabilidad técnica y operativa de la solución, dado que los transportistas utilizan sus propios celulares y un drenaje excesivo de batería provocaría el rechazo de la herramienta.
* **Módulo o Componente Afectado:** Módulo de Telemetría GPS Móvil, Foreground Service de Android.
* **Condición o Métrica:** Consumo acumulado de energía <= 15% en batería estándar de 4000 mAh en 8 horas de operación activa en ruta.
* **Criterio de Validación:** Monitoreo del consumo energético mediante herramientas de perfilado de Android (Battery Historian) en recorridos reales de prueba.
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] | [[Restricciones_Tecnicas]].
* **Estado:** Propuesta Técnica.
* **Análisis Crítico:** Justifica técnicamente el intervalo de muestreo GPS de 10 minutos en [[RF014]], evitando el rastreo continuo innecesario.

---

#### RNF009 — Consumo Optimizado de Datos Móviles Celulares (< 50 MB diarios)
* **ID:** `RNF009`
* **Caso de Prueba Asociado:** `CP-RNF-EFF-02`
* **Nombre:** Consumo Optimizado de Datos Móviles Celulares.
* **Categoría:** Rendimiento y Eficiencia de Red.
* **Descripción:** El tráfico total de datos móviles transmitido por la aplicación App Nativa (incluyendo ráfagas de telemetría GPS y payloads JSON minificados) no debe superar los **50 Megabytes (MB) diarios** por unidad de transporte.
* **Objetivo de Negocio:** Evitar la saturación del plan de datos personal de los conductores y garantizar la transmisión ágil en redes celulares lentas (3G/H+) en carreteras interprovinciales.
* **Módulo o Componente Afectado:** Serializadores JSON Móviles, Protocolo de Comunicación REST.
* **Condición o Métrica:** Volumen total de datos de red <= 50 MB/día por dispositivo móvil para una operación diaria típica punto a punto. Se elimina por completo el tráfico de imágenes.
* **Criterio de Validación:** Medición del consumo de datos de red mediante Network Profiler en Android durante simulaciones de jornada completa.
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] | [[Restricciones_Tecnicas]].
* **Estado:** Propuesta Técnica.
* **Análisis Crítico:** Al no transmitir fotografías ni comprobantes multimedia, el consumo real se reduce drásticamente a pocos megabytes diarios de telemetría pura.

---

### Categoría 3: Disponibilidad, Capacidad y Escalabilidad

#### RNF010 — Disponibilidad Operativa de la Plataforma Web y Backend (99.5%)
* **ID:** `RNF010`
* **Caso de Prueba Asociado:** `CP-RNF-REL-01`
* **Nombre:** Disponibilidad Operativa de la Plataforma Web y Backend.
* **Categoría:** Confiabilidad (Disponibilidad del Sistema).
* **Descripción:** Los servicios centrales de Y-Trace (Portal Web de supervisión y API REST de ingesta de eventos) deben mantener un índice de disponibilidad no menor al **99.5%** durante la ventana crítica de distribución y despacho nacional (lunes a sábado de 06:00 a 21:00 horas).
* **Objetivo de Negocio:** Evitar interrupciones que paralicen la Torre de Control o que impidan la recepción de telemetría de las flotas en los 24 departamentos de Perú.
* **Módulo o Componente Afectado:** Infraestructura de Servidores Backend, Balanceadores de Carga y Base de Datos Principal.
* **Condición o Métrica:** Uptime operativo >= 99.5% en horario de reparto (máximo tolerable de indisponibilidad no programada de 1.95 horas al mes sobre una base de 390 horas operativas).
* **Criterio de Validación:** Registros de sondas de disponibilidad externas (health checks) con muestreo cada 60 segundos hacia `/api/health`.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]].
* **Estado:** Pendiente de validación.
* **Nota de Regla Pendiente de Validación (RN-PV-01):** El índice de disponibilidad del 99.5% es un objetivo de nivel de servicio propuesto por la arquitectura, pendiente de validación y formalización en el SLA de infraestructura cloud y soporte con TI Yanbal.
* **Análisis Crítico:** Garantiza la continuidad operativa de la supervisión logística en los horarios de mayor tráfico de carga.

---

#### RNF011 — Capacidad y Escalabilidad Concurrente
* **ID:** `RNF011`
* **Caso de Prueba Asociado:** `CP-RNF-SCA-01`
* **Nombre:** Capacidad y Escalabilidad Concurrente.
* **Categoría:** Confiabilidad (Capacidad y Escalabilidad).
* **Descripción:** El sistema backend de Y-Trace debe tener la capacidad de procesar la transmisión concurrente de al menos **500 conductores en ruta simultáneamente** y digerir hasta **50,000 eventos de entrega y coordenadas GPS diarios**, sin que la latencia de respuesta de la API sufra degradaciones mayores al 10%.
* **Objetivo de Negocio:** Soportar la carga máxima de operación durante las semanas de cierre de campaña comercial de Yanbal Perú (picos de mayor volumen de carga del año).
* **Módulo o Componente Afectado:** API Gateway, Capa de Ingesta Asíncrona, Base de Datos Transaccional.
* **Condición o Métrica:** Procesamiento concurrente de al menos 500 conductores transmitiendo en paralelo y 50,000 eventos diarios, con degradación de latencia de la API < 10% bajo carga máxima.
* **Criterio de Validación:** Ejecución de pruebas de carga y estrés (JMeter / k6) simulando 500 agentes virtuales emitiendo telemetría y subiendo eventos simultáneamente sin exceder el 10% de degradación de latencia.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Requerimientos_No_Funcionales]].
* **Estado:** Propuesta Técnica.
* **Análisis Crítico:** Fija la escala volumétrica del sistema de acuerdo con la flota nacional de transporte B2B de Yanbal.

---

### Categoría 4: Resiliencia y Tolerancia a Fallos (Offline-First)

#### RNF012 — Capacidad de Persistencia Local Offline en Dispositivo (>= 500 eventos)
* **ID:** `RNF012`
* **Caso de Prueba Asociado:** `CP-RNF-RES-01`
* **Nombre:** Capacidad de Persistencia Local Offline en Dispositivo.
* **Categoría:** Resiliencia (Tolerancia a Desconexión de Red).
* **Descripción:** El motor de persistencia local de la aplicación móvil (App Nativa Android) debe ser capaz de almacenar de forma íntegra y segura al menos **500 eventos operativos y coordenadas GPS** durante una ausencia prolongada de red celular, sin pérdida ni corrupción de información. No almacena fotos ni archivos multimedia.
* **Objetivo de Negocio:** Permitir que los despachos que atraviesan zonas interprovinciales sin cobertura completen su viaje y sincronicen posteriormente toda la información acumulada.
* **Módulo o Componente Afectado:** Almacenamiento Local App Nativa (`SQLite (Room)`) y cola offline.
* **Condición o Métrica:** Retención íntegra de >= 500 eventos operativos pendientes de sincronización, manteniendo sus identificadores UUIDv4 y `captured_at` originales.
* **Criterio de Validación:** Simulación de 500 eventos en modo desconectado, reinicio del teléfono y validación de la integridad del 100% de los eventos retenidos en SQLite (Room).
* **Fuente o Evidencia:** [[Offline_First]] | [[Restricciones_Tecnicas]].
* **Estado:** Propuesta Técnica.
* **Análisis Crítico:** Garantiza que los viajes interprovinciales extensos (ej. Lima - Cusco, 24 horas) retengan toda su telemetría sin riesgo de desborde de memoria.

---

#### RNF013 — Idempotencia y Garantía de Entrega en Sincronización
* **ID:** `RNF013`
* **Caso de Prueba Asociado:** `CP-RNF-INT-02`
* **Nombre:** Idempotencia y Garantía de Entrega en Sincronización.
* **Categoría:** Integridad y Confiabilidad de Integración.
* **Descripción:** Los servicios backend de ingesta de eventos de Y-Trace deben ser estrictamente **idempotentes**, garantizando que el reenvío múltiple de una misma transacción (ocasionado por cortes de red o reintentos automáticos en carretera) no duplique registros de eventos o coordenadas en la base de datos.
* **Objetivo de Negocio:** Evitar inconsistencias de datos, duplicación de estados o alteraciones en las métricas de cumplimiento de entrega.
* **Módulo o Componente Afectado:** API de Sincronización Backend, Capa de Servicios de Eventos.
* **Condición o Métrica:** Procesamiento de identificadores universales (UUIDv4) únicos generados en el dispositivo móvil; si un UUID ya fue procesado, el servidor debe responder con acuse de éxito (HTTP 200) sin duplicar el registro en la base de datos.
* **Criterio de Validación:** Envío reiterado de un mismo payload 10 veces consecutivas; el conteo final de registros en BD debe ser exactamente 1.
* **Fuente o Evidencia:** [[Offline_First]] | [[Componentes_Aplicacion]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Condiciona la arquitectura del backend para procesar con total seguridad la cola de eventos emitida por [[RF020]].

---

### Categoría 5: Usabilidad y Ergonomía Operativa

#### RNF014 — Usabilidad y Ergonomía Operativa para Campo (máximo 3 toques)
* **ID:** `RNF014`
* **Caso de Prueba Asociado:** `CP-RNF-USA-01`
* **Nombre:** Usabilidad y Ergonomía Operativa para Campo.
* **Categoría:** Usabilidad y Ergonomía de Software.
* **Descripción:** La interfaz de la aplicación móvil del conductor debe estar diseñada ergonómicamente para su utilización en campo, permitiendo completar cualquier flujo operativo crítico (iniciar despacho, marcar llegada, confirmar entrega o registrar causal de no entrega) en un **máximo de 3 toques de pantalla**, con botones de alto contraste y tamaño mínimo accesible. Sin formularios complejos de incidencias.
* **Objetivo de Negocio:** Minimizar las distracciones del conductor en cabina, acelerar la atención física en andén y punto de destino y asegurar la rápida adopción por transportistas de cualquier nivel de alfabetización digital.
* **Módulo o Componente Afectado:** Frontend App Nativa (Capa UI y Diseño de Interacción).
* **Condición o Métrica:**
  - Máximo 3 interacciones táctiles requeridas desde la pantalla principal para completar un registro operativo.
  - Dimensiones mínimas de botones interactivos de 48x48 dp (según estándares Material Design / WCAG).
  - Paleta de colores con contraste mínimo de 4.5:1 legible bajo luz solar intensa en carretera.
* **Criterio de Validación:** Evaluación de usabilidad con conductores midiendo tiempos de tarea en campo bajo simulación operativa real.
* **Fuente o Evidencia:** [[Funciones_Movil]] | [[Flujo_Conductor]].
* **Estado:** Propuesta Técnica.
* **Nota de Estado:** El límite de máximo 3 toques y las dimensiones mínimas de 48x48 dp constituyen una propuesta técnica de diseño ergonómico para cabina y andén.
* **Análisis Crítico:** Condiciona la ergonomía visual de los requerimientos [[RF010]], [[RF012]], [[RF013]], [[RF015]], [[RF016]] y [[RF017]].

---

#### RNF015 — Tiempo de Respuesta en Consulta de Trazabilidad (< 2 segundos)
* **ID:** `RNF015`
* **Caso de Prueba Asociado:** `CP-RNF-PERF-03`
* **Nombre:** Tiempo de Respuesta en Consulta de Trazabilidad.
* **Categoría:** Rendimiento (Tiempo de Consulta Web).
* **Descripción:** El módulo de consulta de trazabilidad y resumen de despachos en la plataforma Web (`RF023`) debe devolver la cronología completa del traslado, paradas y coordenadas GPS en un tiempo no mayor a **2.0 segundos** tras introducir el código de despacho o placa de la unidad.
* **Objetivo de Negocio:** Permitir a los supervisores, jefes y operadores de SAC responder de forma inmediata sobre el estado y ubicación de cualquier carga en ruta durante la atención de llamadas.
* **Módulo o Componente Afectado:** Base de Datos (Índices B-Tree en `codigo_despacho` y `placa_vehiculo`), Endpoint de Consulta de Trazabilidad.
* **Condición o Métrica:** Tiempo total de respuesta de la API y renderizado final en pantalla <= 2.0 segundos sobre un histórico de al menos 1,000,000 de registros de despachos.
* **Criterio de Validación:** Pruebas de carga sobre base de datos poblada con 1 millón de despachos históricos; el tiempo de respuesta debe mantenerse inferior a 2 s.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Flujo_Web]].
* **Estado:** Propuesta Técnica.
* **Nota de Estado:** El umbral de tiempo de respuesta < 2.0 s en consulta indexada es una propuesta técnica de rendimiento de base de datos relacional.
* **Análisis Crítico:** Condiciona directamente la efectividad del requerimiento funcional unificado [[RF023]].

---

### Categoría 6: Compatibilidad e Interoperabilidad

#### RNF016 — Compatibilidad de Plataforma Cliente (App Nativa Android y Consola Web)
* **ID:** `RNF016`
* **Caso de Prueba Asociado:** `CP-RNF-COMP-01`
* **Nombre:** Compatibilidad de Plataforma Cliente (App Nativa Android y Consola Web).
* **Categoría:** Compatibilidad y Plataformas de Cliente.
* **Descripción:** El sistema debe garantizar compatibilidad operativa diferenciada y estricta para cada tipo de cliente del ecosistema Y-Trace:
  1. **Aplicación Móvil para Conductores:** Debe operar exclusivamente como **App Nativa para dispositivos Android** (versión 8.0 Oreo o superior / API Level 26+), aprovechando los servicios nativos de geolocalización de Google Play Services / Android Location APIs y almacenamiento local SQLite (Room). Las plataformas iOS, navegadores móviles y modalidad PWA quedan formalmente excluidas del alcance.
  2. **Consola Web Administrativa y Operativa:** Debe operar de forma óptima en **navegadores web modernos de escritorio** compatibles con los estándares HTML5, CSS3 y ECMAScript 2020+ (específicamente Google Chrome versión 90+, Microsoft Edge versión 90+ y Mozilla Firefox versión 88+), sin requerir plugins propietarios ni software cliente adicional.
* **Objetivo de Negocio:** Maximizar la estabilidad técnica del rastreo satelital concentrando el soporte móvil en el parque homogéneo de terminales Android de los transportistas, mientras se provee acceso universal y sin fricción para el personal de control logístico desde cualquier estación de trabajo corporativa.
* **Módulo o Componente Afectado:** Core App Nativa Android (Foreground Service, SQLite Room), Frontend Consola Web (HTML5/CSS3/JS).
* **Condición o Métrica:**
  - **Móvil:** 100% de funcionalidades operativas (GPS satelital en segundo plano, persistencia SQLite, sincronización FIFO) en dispositivos con Android 8.0+.
  - **Web:** 100% de consolas operativas (monitoreo en tiempo real, trazabilidad, reportes y administración) validadas y sin distorsiones en Google Chrome (90+), Microsoft Edge (90+) y Mozilla Firefox (88+).
* **Criterio de Validación:** Pruebas de compatibilidad sobre terminales físicos Android representativos (Samsung, Xiaomi, Motorola) y pruebas cruzadas de renderizado y ejecución en los tres navegadores web de escritorio definidos.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[Funciones_Movil]] | [[Funciones_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Establece una delimitación de soporte clara y auditable que protege la inversión de desarrollo y simplifica el ciclo de vida del soporte técnico.

---

#### RNF017 — Estandarización de Formatos de Intercambio JSON Canónico
* **ID:** `RNF017`
* **Caso de Prueba Asociado:** `CP-RNF-INT-03`
* **Nombre:** Estandarización de Formatos de Intercambio JSON Canónico.
* **Categoría:** Interoperabilidad de Sistemas.
* **Descripción:** Los eventos y resúmenes de integración se estructuran mediante payloads JSON conforme al esquema canónico definido para la interoperabilidad con el Bus corporativo de Yanbal con codificación UTF-8.
* **Objetivo de Negocio:** Garantizar la interoperabilidad fluida y desacoplada con el middleware corporativo de Yanbal ([[Bus_Integracion]]).
* **Módulo o Componente Afectado:** Conector al Bus de Integración, Serializadores JSON de Backend.
* **Condición o Métrica:** 100% de los mensajes de integración formateados en JSON conforme al esquema canónico establecido para los eventos del despacho.
* **Criterio de Validación:** Validación de esquemas JSON Schema en los eventos publicados hacia el Bus de Integración.
* **Fuente o Evidencia:** [[Bus_Integracion]] | [[Integracion_Bus_Eventos]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Gobierna la serialización de datos para la interoperabilidad con el Bus en [[RF025]] y [[RF026]].

---

### Categoría 7: Calidad del Dato, Continuidad, Gobernanza y Auditoría

#### RNF018 — Retención y Eliminación de Registros Operativos (24 meses)
* **ID:** `RNF018`
* **Caso de Prueba Asociado:** `CP-RNF-LEG-01`
* **Nombre:** Retención y Eliminación de Registros Operativos.
* **Categoría:** Seguridad y Gobernanza de Datos (Cumplimiento de Retención).
* **Descripción:** Los registros operativos de eventos y coordenadas GPS satelitales deben conservarse durante el plazo definido por la política corporativa de retención de Yanbal (establecido preliminarmente en 24 meses) y eliminarse o anonimizarse irreversiblemente de forma automatizada en la base de datos relacional una vez vencido dicho plazo, constituyendo la única excepción programada a la inmutabilidad transaccional definida en [[RNF005]]. Sin almacenamiento de fotos ni Object Storage.
* **Objetivo de Negocio:** Cumplir con las directrices de gobernanza de la información y optimización de costos de almacenamiento en base de datos.
* **Módulo o Componente Afectado:** Base de Datos Relacional, Job de Purga y Archivado Automático.
* **Condición o Métrica:** 100% de los registros de telemetría y eventos cuya antigüedad supere los 24 meses deben ser purgados físicamente en el ciclo semanal programado.
* **Criterio de Validación:** Ejecución auditable del job de purga automática en ambiente de pruebas sobre registros simulados con antigüedad > 24 meses.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[RNF005]].
* **Estado:** Pendiente de validación.
* **Nota de Regla Pendiente de Validación (RN-PV-04):** El mecanismo de depuración automatizada y su articulación con RNF005 quedan confirmados; el plazo de 24 meses es propuesta preliminar sujeta a ratificación formal por Legal Yanbal.
* **Análisis Crítico:** Gobierna la expiración y purga programada de las tablas históricas transaccionales en perfecta armonía con RNF005.

---

#### RNF019 — Recuperación ante Desastres (RPO <= 15 min, RTO <= 4 h)
* **ID:** `RNF019`
* **Caso de Prueba Asociado:** `CP-RNF-DIS-01`
* **Nombre:** Recuperación ante Desastres (RPO y RTO).
* **Categoría:** Confiabilidad (Disponibilidad y Recuperación ante Desastres).
* **Descripción:** El sistema debe definir y garantizar métricas estrictas de Objetivo de Punto de Recuperación (RPO) y Objetivo de Tiempo de Recuperación (RTO) ante un escenario de caída catastrófica o pérdida total del centro de datos o infraestructura principal en la nube.
* **Objetivo de Negocio:** Garantizar la continuidad de las operaciones de distribución de Yanbal a nivel nacional, minimizando el impacto comercial y la pérdida de transacciones logísticas ante desastres.
* **Módulo o Componente Afectado:** Infraestructura Cloud, Base de Datos (replicación continua y snapshots), API de Ingesta.
* **Condición o Métrica:**
  - **RPO (Recovery Point Objective):** <= 15 minutos (pérdida máxima tolerable de telemetría y eventos).
  - **RTO (Recovery Time Objective):** <= 4 horas (tiempo máximo tolerable para restaurar la operatividad completa del sistema en infraestructura secundaria).
* **Criterio de Validación:** Ejecución semestral de simulacro de recuperación ante desastres (*DR drill*) con conmutación por error hacia zona secundaria, cronometrando tiempos y consistencia.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | [[RNF010]].
* **Estado:** Pendiente de validación.
* **Nota de Regla Pendiente de Validación (RN-PV-02):** Valores propuestos por arquitectura, pendientes de ratificación en SLA formal con TI de Yanbal.
* **Análisis Crítico:** Condiciona la arquitectura física multizona y la política de copias de seguridad de Y-Trace.

---

#### RNF020 — Precisión Mínima de Geolocalización en Dispositivos Android (<= 50 m)
* **ID:** `RNF020`
* **Caso de Prueba Asociado:** `CP-RNF-GEO-01`
* **Nombre:** Precisión Mínima de Geolocalización en Dispositivos Android.
* **Categoría:** Calidad del Dato y Rendimiento (Precisión Geoespacial).
* **Descripción:** Las coordenadas GPS capturadas mediante los servicios de geolocalización de Android deben presentar un radio de precisión (*accuracy*) reportado por el hardware no mayor a **50 metros** en condiciones de cielo abierto. Aquellas lecturas satelitales con imprecisión mayor a 50 metros no se descartan, sino que se catalogan y persisten con bandera de `"baja confiabilidad"`.
* **Objetivo de Negocio:** Asegurar la calidad y confiabilidad del dato satelital que actúa como evidencia geoespacial de arribo ([[RF015]]) y auditoría de ruta ([[RF014]]).
* **Módulo o Componente Afectado:** Módulo Móvil de Telemetría GPS (App Nativa Android), Backend de Ingesta GPS.
* **Condición o Métrica:**
  - Al menos el 90% de las lecturas capturadas bajo cielo abierto presentan `accuracy <= 50 m`.
  - 100% de las lecturas con `accuracy > 50 m` etiquetadas con atributo `baja_confiabilidad = true`.
* **Criterio de Validación:** Pruebas de campo comprobando que el dato se persiste y el backend asigna el indicador de confiabilidad correspondiente.
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] | [[RF014]] | [[RF015]].
* **Estado:** Propuesta Técnica.
* **Nota de Estado:** El umbral de precisión satelital <= 50 metros y la etiqueta de baja confiabilidad constituyen una propuesta técnica de ingeniería geoespacial en Android.
* **Análisis Crítico:** Define el umbral métrico de confiabilidad de la lectura satelital sin descartar paquetes ni interrumpir la operación del conductor.

---

#### RNF021 — Accesibilidad Web para Consola Administrativa (WCAG 2.1 Nivel AA)
* **ID:** `RNF021`
* **Caso de Prueba Asociado:** `CP-RNF-ACC-01`
* **Nombre:** Accesibilidad Web para Consola Administrativa.
* **Categoría:** Usabilidad y Accesibilidad Web.
* **Descripción:** La plataforma Web de Y-Trace (utilizada por Supervisores de Distribución, Jefes de Distribución y Operadores SAC) debe construirse en cumplimiento del estándar internacional de accesibilidad **WCAG 2.1 Nivel AA**, asegurando ratios de contraste adecuados, compatibilidad con lectores de pantalla, navegación completa por teclado y semántica HTML5 accesible. Sin pantallas de incidencias.
* **Objetivo de Negocio:** Garantizar condiciones ergonómicas óptimas para jornadas continuas de supervisión de 8 horas y accesibilidad para colaboradores con discapacidades visuales o motrices.
* **Módulo o Componente Afectado:** Interfaz web completa (Despachos Disponibles, Monitoreo de Despachos, Trazabilidad, Auditoría y Dashboard).
* **Condición o Métrica:**
  - Relación de contraste de texto normal >= 4.5:1 y componentes gráficos >= 3:1.
  - Navegación completa por teclado accesible con foco visible en el 100% de componentes interactivos.
  - Puntaje de accesibilidad >= 90 / 100 en auditorías automatizadas (Google Lighthouse / axe-core).
* **Criterio de Validación:** Evaluación mediante Lighthouse/axe DevTools y pruebas manuales con teclado y lectores de pantalla.
* **Fuente o Evidencia:** WCAG 2.1 Nivel AA (W3C) | [[Funciones_Web]] | Complemento de [[RNF014]].
* **Estado:** Pendiente de validación.
* **Nota de Regla Pendiente de Validación (RN-PV-03):** Parámetro técnico definido, pendiente de validación de presupuesto/cronograma adicional con Yanbal.
* **Análisis Crítico:** Asegura inclusión digital y reduce la fatiga visual de los operadores en la Torre de Control.

---

#### RNF022 — Inmutabilidad de la Bitácora de Auditoría Administrativa
* **ID:** `RNF022`
* **Caso de Prueba Asociado:** `CP-RNF-INT-04`
* **Nombre:** Inmutabilidad de la Bitácora de Auditoría Administrativa.
* **Categoría:** Seguridad e Integridad de Datos.
* **Descripción:** Los registros de la bitácora de auditoría administrativa generados por [[RF006]] deben almacenarse bajo una arquitectura estricta de **solo anexión (append-only)** a nivel de base de datos relacional, prohibiendo de forma irrevocable operaciones de actualización (`UPDATE`) o eliminación (`DELETE`) mediante restricciones de esquema, triggers y roles de base de datos sin permisos destructivos.
* **Objetivo de Negocio:** Garantizar la validez legal, técnica y forense de las auditorías internas de Yanbal, previniendo que administradores o actores maliciosos puedan encubrir acciones fraudulentas borrando rastros.
* **Módulo o Componente Afectado:** Base de Datos Relacional (esquema `audit_logs`), Capa de Persistencia Backend.
* **Condición o Métrica:** Cero tolerancia a modificaciones o eliminaciones en tablas de auditoría (100% de rechazo inmediato por el motor de base de datos ante sentencias `UPDATE` o `DELETE`).
* **Criterio de Validación:** Ejecución deliberada de comandos SQL `UPDATE` y `DELETE` sobre la tabla de auditoría con la cuenta de servicio de la aplicación; el gestor relacional debe abortar la transacción con error de violación de permisos o excepción de trigger.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[RF006]] | Simetría con la inmutabilidad de eventos de [[RNF005]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Asegura que la bitácora de auditoría web ([[RF006]]) permanezca inalterable ante cualquier intento de manipulación en base de datos.
