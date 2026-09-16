# Descripción General de la Solución Y-Trace — Yanbal Perú

## 1. Identificación del proyecto

**Nombre del proyecto:** Y-Trace  
**Nombre completo:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú  
**Alcance:** Distribución completa entre puntos de distribución (B2B), desde el Centro de Distribución de Lurín hasta agencias, centros de distribución secundarios y otros puntos de destino.  
**Propósito principal:** Proporcionar trazabilidad operativa de extremo a extremo sobre despachos que ya fueron creados y preparados en los sistemas corporativos de Yanbal, permitiendo controlar su salida, seguir el traslado, gestionar incidencias, registrar la llegada y entrega, conservar evidencias y explotar posteriormente la información para auditoría y gestión.

> **Importante:** Y-Trace no reemplaza los sistemas corporativos que crean, preparan, programan o gestionan logísticamente los despachos. Su función comienza cuando existe un despacho disponible para seguimiento y termina cuando la operación queda formalmente cerrada y trazable.

---

## 2. ¿De qué tratará Y-Trace?

Y-Trace será una solución compuesta por dos partes principales que trabajan de manera integrada:

1. **Plataforma Web:** utilizada por el personal autorizado de Yanbal para administrar el acceso, controlar la operación, habilitar el seguimiento de despachos, monitorear vehículos, atender incidencias, consultar la trazabilidad y analizar indicadores.
2. **Aplicación móvil PWA Android:** utilizada por el conductor del socio logístico para activar el viaje mediante un código temporal, consultar la información necesaria del despacho, iniciar el traslado, transmitir la posición GPS, reportar incidencias, registrar la llegada, confirmar o rechazar la entrega y finalizar el seguimiento.

El sistema mantiene una única trazabilidad del despacho. La Web funciona como la **Torre de Control**, mientras que la PWA constituye el **punto operativo móvil del conductor**.

El alcance es estrictamente **B2B punto a punto**. No se contempla reparto domiciliario a consultoras, familiares o clientes residenciales, ni la gestión de ventas, picking o inventarios.

---

## 3. Problema que busca resolver

La solución está orientada a mejorar el control y visibilidad del traslado nacional de cargas consolidadas entre puntos de distribución. El sistema busca evitar que la organización dependa exclusivamente de llamadas telefónicas o información dispersa para saber:

- qué despacho está preparado para salir;
- qué unidad está realizando el traslado;
- cuándo salió realmente del Centro de Distribución;
- dónde se encontraba la unidad durante el recorrido;
- si ocurrió una incidencia en carretera;
- cuándo llegó al destino;
- si la carga fue efectivamente entregada o rechazada;
- qué evidencias respaldan los principales hitos;
- cuánto tiempo tomó la operación; y
- cuál fue el desempeño de los transportistas.

Por ello, Y-Trace centraliza la información operativa y genera una línea de tiempo verificable de cada despacho.

---

## 4. Frontera de la solución

### 4.1 Lo que sí hará Y-Trace

Y-Trace realizará principalmente las siguientes actividades:

- consultar despachos existentes y elegibles para seguimiento;
- habilitar el seguimiento mediante un Código Único de Activación;
- permitir la activación móvil del conductor;
- crear y mantener una sesión operativa móvil vinculada al viaje;
- mostrar al conductor los datos operativos necesarios del despacho y destino;
- registrar el inicio formal del traslado;
- capturar telemetría GPS durante la operación en ruta;
- conservar eventos localmente cuando no exista conectividad;
- sincronizar posteriormente los eventos pendientes;
- mostrar la posición de los vehículos en la Torre de Control;
- generar y atender alertas por incidencias;
- registrar la llegada al destino;
- distinguir llegada física de entrega efectiva;
- confirmar una entrega o registrar una no entrega/rechazo;
- almacenar evidencias operativas y fotografías complementarias opcionales;
- cerrar el despacho y revocar la sesión móvil;
- publicar eventos y resúmenes de trazabilidad hacia el Bus de Integración;
- consultar la trazabilidad histórica;
- auditar evidencias y eventos; y
- calcular indicadores de rendimiento logístico y exportarlos a Excel.

### 4.2 Lo que no hará Y-Trace

Y-Trace no será responsable de:

- crear despachos logísticos;
- programar despachos;
- asignar logísticamente vehículos o rutas;
- preparar, consolidar o hacer picking de la carga;
- gestionar inventarios;
- gestionar ventas o pedidos comerciales;
- realizar reparto minorista B2C;
- administrar entregas domiciliarias a consultoras o clientes residenciales;
- liquidar económicamente los fletes; ni
- sustituir los sistemas corporativos existentes que son responsables de esas actividades.

---

## 5. Sistemas externos con los que se relaciona

Y-Trace se integra dentro del ecosistema existente de Yanbal y respeta la frontera de responsabilidades de cada sistema.

| Sistema / Componente | Relación con Y-Trace |
|---|---|
| **SPY / WMS** | Proporciona el contexto de despachos ya preparados y consolidados en el Centro de Distribución. |
| **Driving / TMS** | Aporta la información relacionada con zonificación, destino y transporte asignado según el proceso corporativo. |
| **Bus de Integración / ESB** | Medio corporativo para publicar y recibir eventos relevantes de trazabilidad. |
| **SAP R/3 / ERP** | Recibe información de hitos de transporte para los procesos corporativos posteriores. |
| **Salesforce / sistemas de trazabilidad** | Puede recibir información relacionada con arribo y recepción conforme. |
| **Cloud Storage** | Conserva fotografías de respaldo cuando el conductor decide capturarlas. |

Y-Trace no asume las responsabilidades funcionales propias de estos sistemas.

---

# 6. Actores que intervienen en la solución

## 6.1 Actores del negocio

El modelo del negocio identifica cinco actores principales:

| Actor | Tipo | Función principal |
|---|---|---|
| **Socio Logístico / Conductor** | Externo | Traslada físicamente la carga, utiliza la PWA, reporta eventos, registra llegada y formaliza la entrega. |
| **Punto de Destino / Agencia Receptora** | Externo | Recibe físicamente la carga, inspecciona su estado y determina la conformidad o rechazo. |
| **Supervisor de Distribución** | Interno | Controla la salida, habilita el seguimiento, monitorea los viajes y atiende incidencias. |
| **Jefe de Distribución** | Interno | Analiza el desempeño de la distribución, SLAs, Lead Time y rendimiento de transportistas. |
| **Operador SAC / Soporte Logístico** | Interno | Consulta la trazabilidad y utiliza las evidencias para responder consultas operativas. |

Los cinco actores se relacionan con los cinco Casos de Uso del Negocio definidos para Y-Trace.

## 6.2 Rol adicional de administración de plataforma

La solución Web incorpora además al **Administrador Principal del Sistema**.

Este rol es responsable de la gobernanza técnica y de seguridad de la plataforma: administra cuentas Web, asigna roles, controla la seguridad de acceso y consulta las bitácoras de auditoría.

> **Distinción importante:** el Administrador Principal es un **rol de administración de la plataforma Web**. No debe confundirse con el Actor del Negocio “Supervisor de Distribución” ni agregarse artificialmente como actor de los CUN logísticos si no participa en esos procesos.

---

# 7. ¿Cómo funcionará la Plataforma Web?

## 7.1 Concepto general

La plataforma Web será la **Torre de Control Logístico de Y-Trace**. Se utilizará desde computadoras o tabletas mediante un navegador corporativo y centralizará la supervisión y administración del seguimiento.

La Web trabajará con autenticación mediante usuario y contraseña y aplicará control de acceso basado en roles. Cada usuario verá solamente las operaciones y módulos permitidos por su perfil.

La plataforma deberá mantener una bitácora de auditoría de las operaciones relevantes para permitir seguimiento posterior de las acciones administrativas y operativas.

## 7.2 Seguridad de acceso Web

El acceso Web tendrá como características principales:

- autenticación mediante usuario y contraseña;
- autorización mediante RBAC;
- sesiones protegidas;
- expiración por inactividad;
- posibilidad de revocar sesiones;
- comunicaciones cifradas mediante HTTPS/TLS;
- bitácora de auditoría inmutable para los eventos definidos por el sistema.

---

# 8. Apartados de la Plataforma Web

## 8.1 Inicio / Dashboard operativo

Será la vista principal de la Torre de Control y permitirá obtener una visión resumida de la operación.

Podrá mostrar información como:

- despachos activos;
- vehículos actualmente en ruta;
- despachos en destino;
- incidencias activas;
- operaciones próximas a superar la ventana de atención en destino;
- estados generales de los viajes; y
- accesos rápidos a las principales funciones operativas.

El contenido exacto visible dependerá del rol del usuario.

## 8.2 Seguridad y acceso

Apartado orientado a la administración y protección del acceso a la plataforma.

Funciones principales:

- inicio de sesión;
- cierre de sesión;
- control de sesión;
- autorización por roles;
- bloqueo y control de accesos no autorizados;
- consulta de eventos de seguridad cuando corresponda.

## 8.3 Gestión de usuarios

Apartado utilizado principalmente por el Administrador Principal.

Funciones:

- crear cuentas Web autorizadas;
- editar cuentas;
- suspender o desactivar cuentas;
- asignar roles;
- modificar roles;
- controlar el ciclo de vida de los usuarios.

El Administrador no utilizará este módulo para crear despachos ni para administrar la logística del viaje.

## 8.4 Consulta y habilitación de despachos

Es uno de los apartados operativos centrales del Supervisor de Distribución.

Permitirá:

- consultar los despachos que llegan desde los sistemas corporativos;
- filtrar los despachos disponibles para seguimiento;
- revisar la información necesaria antes de habilitar el seguimiento;
- generar el Código Único de Activación;
- registrar el hito interno de control previo;
- gestionar, cuando proceda, un código de recuperación para un viaje ya iniciado.

La generación del código **no crea el despacho** y **no asigna una ruta nueva**. Solamente habilita el seguimiento de un despacho que ya existe.

## 8.5 Monitoreo de flota

Será la grilla operativa de seguimiento con semaforización visual de la Torre de Control.

Permitirá visualizar los vehículos según la última telemetría disponible y su estado operativo.

Semáforo principal:

- **Verde:** `EN_RUTA`.
- **Amarillo:** `EN_DESTINO`.
- **Rojo:** `CON_INCIDENCIA`.

Además de la posición, el Supervisor podrá consultar información asociada al viaje y detectar anomalías o demoras.

## 8.6 Gestión de incidencias

Apartado desde el cual el Supervisor recibirá y atenderá los reportes generados desde la PWA.

Funciones principales:

- recibir alertas;
- identificar el despacho afectado;
- conocer el tipo de incidencia;
- visualizar la ubicación asociada cuando esté disponible;
- registrar notas de contingencia;
- coordinar asistencia vial o mecánica;
- realizar seguimiento de la recuperación;
- aplicar un cierre administrativo forzado cuando exista una causa justificada y documentada.

## 8.7 Control de llegada y ventana de destino

La Web permitirá al Supervisor visualizar cuándo un vehículo pasó a `EN_DESTINO`.

La regla definida para el proyecto diferencia claramente dos hechos:

1. `EN_DESTINO` demuestra llegada o presencia física en el punto de destino.
2. `ENTREGADO` demuestra que la entrega fue formalmente confirmada.

La llegada no debe convertirse automáticamente en entrega.

Cuando aplique la regla de ventana operativa, la Web podrá generar una alerta cuando transcurran 60 minutos desde `EN_DESTINO` sin resolver la entrega.

## 8.8 Trazabilidad / Buscador

Este módulo será utilizado principalmente por el Operador SAC y también por perfiles de supervisión autorizados.

La búsqueda podrá realizarse por:

- código de despacho; o
- placa de la unidad vehicular.

El resultado mostrará una línea de tiempo integral, incluyendo los principales hitos del viaje:

`Disponibilidad → Activación → Salida → Eventos en ruta → Llegada → Entrega/No Entrega → Cierre`

Si el despacho fue cancelado antes de la salida, la trazabilidad deberá reflejar el estado `CANCELADO` junto con la causal y datos de auditoría definidos.

## 8.9 Evidencias de entrega / POD

Permitirá consultar las evidencias relacionadas con la operación.

Podrá presentar:

- fecha y hora del evento;
- coordenadas GPS asociadas;
- resultado de entrega;
- información de rechazo cuando corresponda; y
- enlace seguro temporal a fotografías complementarias almacenadas en Cloud Storage, si existen.

La fotografía es complementaria: no sustituye los eventos operativos ni bloquea por sí sola el cierre formal.

## 8.10 Auditoría

Apartado destinado a supervisar y revisar el historial de acciones y eventos relevantes.

Permitirá analizar:

- accesos;
- intentos fallidos;
- cambios administrativos;
- acciones de supervisión;
- eventos de activación;
- operaciones de cierre;
- eventos de seguridad; y
- evidencias de trazabilidad.

Los registros de auditoría se consideran inmutables bajo los requerimientos definidos.

## 8.11 Dashboard ejecutivo e indicadores

Este apartado será utilizado principalmente por el Jefe de Distribución.

Permitirá analizar indicadores como:

- **Lead Time** del traslado;
- tasa de entregas conformes;
- latencia de integración;
- cancelaciones en una métrica independiente;
- desempeño comparativo entre empresas transportistas.

La información podrá filtrarse por criterios disponibles como período y empresa transportista, respetando las restricciones de acceso correspondientes.

Los reportes consolidados se exportarán en **Excel (.xlsx)**.

## 8.12 Administración de integraciones y estado de eventos

Este apartado será principalmente de soporte técnico/operativo y permitirá visualizar el estado de los eventos que deben ser comunicados al Bus de Integración.

Y-Trace debe controlar el cumplimiento del SLA definido para la publicación y aceptación de eventos: máximo 30 minutos desde que el evento es recibido por el backend y entra al proceso de integración.

---

# 9. ¿Cómo funcionará la aplicación móvil?

## 9.1 Concepto general

La aplicación móvil será una **Progressive Web App (PWA)** destinada exclusivamente a **Android**.

El conductor no utilizará una cuenta permanente con usuario y contraseña para comenzar el viaje. En su lugar, recibirá un **Código Único de Activación de 8 caracteres** generado desde la Web por el Supervisor.

La PWA podrá abrirse mediante un enlace seguro o QR y podrá añadirse a la pantalla de inicio del dispositivo.

## 9.2 Acceso mediante código

El flujo será:

1. El Supervisor revisa un despacho disponible para seguimiento.
2. El Supervisor genera un código de activación.
3. El conductor recibe el código junto con la información operativa correspondiente.
4. El conductor abre la PWA en su teléfono Android.
5. Ingresa el código de 8 caracteres.
6. El backend valida que el código sea válido, vigente, no consumido y corresponda a un despacho elegible.
7. El sistema establece una sesión operativa móvil vinculada al despacho y al dispositivo.
8. La PWA descarga los datos necesarios del despacho y destino para operar localmente.

El código no crea el viaje; solamente habilita la operación digital sobre un despacho existente.

## 9.3 Protección contra intentos no autorizados

El sistema limitará los intentos incorrectos del código. Al alcanzar cinco fallos consecutivos, el código será bloqueado e invalidado y se generará una alerta para el Supervisor.

Durante una contingencia de dispositivo en un despacho `EN_RUTA`, el Supervisor podrá generar un código de recuperación autorizado para el mismo viaje. Ese mecanismo reemplazará la sesión anterior sin crear un nuevo despacho ni destruir el histórico.

---

# 10. Apartados principales de la PWA Android

## 10.1 Pantalla de activación

Es la pantalla inicial de la aplicación.

Contendrá principalmente:

- campo para el Código Único de Activación;
- acción para activar el despacho;
- mensajes de validación;
- información sobre errores de código;
- estado de bloqueo si se superan los intentos permitidos.

## 10.2 Datos del despacho / Hoja de ruta

Después de activar correctamente el viaje, el conductor podrá consultar:

- código del despacho;
- punto de destino;
- dirección de llegada;
- observaciones de ruta;
- estado actual de la operación.

La información se conservará localmente para soportar el funcionamiento sin conectividad.

## 10.3 Inicio del despacho

El conductor realizará la acción **“Iniciar Despacho”** después de completar la actividad física de salida.

Al ejecutarla correctamente:

- el despacho cambia a `EN_RUTA`;
- se registra fecha y hora;
- comienza la captura periódica de GPS;
- inicia formalmente la etapa de traslado monitorizado.

La PWA no decide la ruta ni asigna vehículos.

## 10.4 Monitoreo GPS

Durante `EN_RUTA`, la aplicación captura coordenadas GPS periódicamente.

El diseño actual establece un muestreo de referencia de **cada 10 minutos** durante el traslado, condicionado por el viaje activo y las capacidades del dispositivo Android.

La información asociada al GPS se vincula a la sesión y al despacho para impedir que una posición quede asociada a otro viaje.

## 10.5 Gestión offline

La operación debe continuar aun cuando el conductor atraviese zonas sin cobertura celular.

Para ello la PWA utiliza almacenamiento local mediante IndexedDB.

Cuando no hay conexión:

- los eventos se guardan localmente;
- las coordenadas se conservan;
- las incidencias se conservan;
- las fotografías opcionales se pueden conservar localmente;
- los registros se acumulan en una cola de sincronización.

Cuando vuelve la conectividad, el sistema sincroniza los registros pendientes en orden cronológico.

La interfaz mostrará un indicador de estado de sincronización y, cuando corresponda, el número de eventos pendientes.

## 10.6 Reportar incidencia

El conductor podrá abrir el módulo de incidencias cuando ocurra una contingencia, por ejemplo:

- avería mecánica;
- accidente o siniestro;
- bloqueo de carretera;
- otra contingencia tipificada por el sistema.

El flujo será:

1. detener la unidad en condiciones seguras;
2. seleccionar el tipo de incidencia;
3. ingresar una descripción;
4. capturar una fotografía complementaria cuando las condiciones lo permitan;
5. registrar la incidencia incluyendo fecha, hora y ubicación cuando esté disponible;
6. enviar la alerta al backend/Torre de Control.

## 10.7 Llegué a Destino

Al arribar al destino, la PWA podrá registrar el evento `EN_DESTINO`.

El modelo establecido contempla un enfoque híbrido:

- detección automática mediante geocerca cuando corresponda;
- botón manual **“Llegué a Destino”** como mecanismo de respaldo.

El evento registra fecha, hora y coordenadas GPS cuando estén disponibles.

> **No confirma la entrega.** Llegar físicamente al destino y entregar la carga son eventos distintos.

## 10.8 Confirmar entrega

Cuando la carga haya sido recibida conforme, el conductor realizará la acción manual **“Confirmar Entrega”**.

Con esa acción el sistema registra formalmente el resultado de entrega y transiciona a `ENTREGADO`.

La recepción física comprende la participación del Punto de Destino / Agencia, que inspecciona la carga y determina su conformidad.

## 10.9 Registrar no entrega / rechazo

Cuando la carga no pueda entregarse o sea rechazada, el conductor seleccionará una causal tipificada y registrará el resultado como `NO_ENTREGADO`.

La operación debe conservar la información suficiente para explicar por qué la entrega no concluyó normalmente.

## 10.10 Evidencia fotográfica complementaria

La fotografía puede utilizarse como respaldo adicional de una incidencia o entrega.

Es importante que la solución no dependa de una fotografía para determinar todos los estados. La evidencia base está constituida por los eventos, marcas de tiempo y datos de ubicación correspondientes; la fotografía es un complemento opcional.

## 10.11 Finalizar despacho

Después de resolver la entrega como `ENTREGADO` o `NO_ENTREGADO`, el conductor podrá seleccionar **“Finalizar Despacho”**.

El backend verificará que:

- el despacho tenga un resultado final válido;
- no existan eventos pendientes de sincronización que deban ser enviados;
- se pueda cerrar formalmente el seguimiento.

Entonces:

1. el despacho pasa a `FINALIZADO`;
2. se revoca la sesión móvil;
3. se invalida el código de activación;
4. se eliminan los datos temporales del viaje del dispositivo;
5. se apaga la emisión de tracking GPS;
6. la aplicación regresa a la pantalla inicial de activación.

Después de `FINALIZADO`, el dispositivo no debe continuar transmitiendo eventos o coordenadas de ese viaje.

---

# 11. Flujo completo de la operación

El flujo integral de Y-Trace puede entenderse de la siguiente forma:

```text
1. Preparación externa
   SPY / sistemas corporativos
           ↓
2. Despacho disponible para seguimiento
   Supervisor consulta en Web
           ↓
3. Habilitación
   Supervisor genera Código de Activación
           ↓
4. Activación móvil
   Conductor ingresa código en PWA Android
           ↓
5. Inicio de despacho
   Conductor presiona “Iniciar Despacho”
           ↓
6. Traslado
   Estado EN_RUTA + GPS periódico
           ↓
7. Incidencia (solo si ocurre)
   Conductor reporta → Supervisor atiende
           ↓
8. Llegada
   Geocerca / “Llegué a Destino”
   Estado EN_DESTINO
           ↓
9. Recepción
   Punto de Destino inspecciona la carga
           ↓
10. Resultado
    ENTREGADO  /  NO_ENTREGADO
           ↓
11. Evidencia complementaria
    Foto opcional + evidencias operativas
           ↓
12. Cierre
    Conductor “Finalizar Despacho”
           ↓
13. FINALIZADO
    Revocación de sesión + cese de GPS
           ↓
14. Explotación de información
    Trazabilidad + Auditoría + KPIs + Integraciones
```

---

# 12. Estados principales del despacho

El seguimiento se apoya en estados que permiten representar la evolución de la operación.

| Estado | Significado |
|---|---|
| **Disponible para seguimiento** | Existe un despacho externo preparado y comunicado a Y-Trace para iniciar su seguimiento. |
| **EN_RUTA** | El despacho salió formalmente y el seguimiento móvil/GPS está activo. |
| **CON_INCIDENCIA** | Existe una incidencia operativa crítica activa durante el traslado. |
| **EN_DESTINO** | El vehículo llegó al punto de destino; este estado no implica que la carga ya haya sido entregada. |
| **ENTREGADO** | La entrega fue confirmada manualmente. |
| **NO_ENTREGADO** | La entrega no pudo completarse o fue rechazada bajo una causal tipificada. |
| **FINALIZADO** | El seguimiento del despacho fue cerrado formalmente y la sesión móvil fue revocada. |
| **CANCELADO** | El despacho fue cancelado antes de la salida según el evento/proceso externo y reglas de auditoría correspondientes. |

---

# 13. Responsabilidades de cada rol

## Administrador Principal

Maneja la plataforma desde el punto de vista de seguridad y gobernanza.

**Puede:**

- administrar cuentas Web;
- asignar y retirar roles;
- supervisar controles de acceso;
- consultar bitácoras de auditoría;
- revisar eventos de seguridad relacionados con activaciones bloqueadas.

**No puede:**

- crear despachos;
- asignar rutas logísticas;
- asumir las funciones operativas del Supervisor por el simple hecho de ser administrador.

## Supervisor de Distribución

Es el responsable operativo de la Torre de Control.

**Puede:**

- consultar despachos disponibles;
- revisar su elegibilidad para seguimiento;
- generar códigos de activación;
- monitorear la flota;
- atender incidencias;
- supervisar la llegada y ventanas de destino;
- generar un código de recuperación autorizado cuando corresponda;
- ejecutar cierres administrativos forzados cuando exista una causa justificada;
- consultar la trazabilidad y evidencias según sus permisos.

## Jefe de Distribución

Es el responsable táctico y estratégico del rendimiento de la distribución.

**Puede:**

- analizar KPIs;
- revisar Lead Time;
- revisar entregas conformes;
- analizar latencia de integración;
- comparar el rendimiento de transportistas;
- consultar información consolidada;
- exportar reportes en Excel.

## Operador SAC / Soporte Logístico

Es el usuario orientado a las consultas de trazabilidad.

**Puede:**

- buscar despachos por código o placa;
- consultar la línea de tiempo;
- revisar el estado del viaje;
- consultar evidencias;
- responder consultas de las áreas internas y puntos de distribución;
- utilizar la trazabilidad como sustento frente a discrepancias.

## Socio Logístico / Conductor

Es el responsable de ejecutar físicamente el traslado y alimentar la operación móvil.

**Puede:**

- activar un despacho mediante código;
- consultar información del viaje;
- iniciar el despacho;
- generar telemetría GPS;
- reportar incidencias;
- registrar llegada;
- confirmar entrega;
- registrar no entrega/rechazo;
- adjuntar fotografías complementarias;
- finalizar el seguimiento.

**No puede:**

- modificar libremente los datos maestros del despacho;
- crear despachos;
- reprogramar logísticamente la ruta;
- alterar artificialmente las coordenadas registradas por el dispositivo.

## Punto de Destino / Agencia Receptora

Es el receptor físico de la carga.

**Participa en:**

- recepción de los bultos/pallets;
- inspección física;
- verificación de precintos e integridad;
- conformidad o rechazo de la carga;
- firma o sello de los documentos físicos de recepción según el proceso establecido.

Este actor no controla la logística interna de Y-Trace; su función principal es la recepción física del despacho en destino.

---

# 14. Cinco Casos de Uso del Negocio de Y-Trace

El modelo del negocio se divide en cinco macroprocesos:

## CUN-01 — Despacho y Salida de Carga en Centro de Distribución

Controla la salida de la carga desde CD Lurín, la verificación previa, el traspaso de custodia y la habilitación del seguimiento.

**Actores principales:** Supervisor de Distribución y Socio Logístico / Conductor.

## CUN-02 — Traslado Interprovincial y Monitoreo de Carga en Tránsito

Representa el traslado físico nacional y el monitoreo de la unidad mediante telemetría GPS.

**Actores principales:** Socio Logístico / Conductor y Supervisor de Distribución.

## CUN-03 — Gestión de Incidencias y Contingencias Viales en Ruta

Representa la detección, reporte, atención y recuperación ante incidencias del traslado.

**Actores principales:** Socio Logístico / Conductor y Supervisor de Distribución.

## CUN-04 — Entrega y Recepción de Carga en Punto de Destino

Representa el arribo, inspección, recepción conforme o rechazo y cierre formal del viaje.

**Actores principales:** Socio Logístico / Conductor, Punto de Destino / Agencia y Supervisor de Distribución.

## CUN-05 — Auditoría de Trazabilidad y Rendimiento de Distribución

Representa la explotación posterior de los datos para auditoría, consultas y evaluación del desempeño logístico.

**Actores principales:** Jefe de Distribución, Operador SAC / Soporte y Supervisor de Distribución.

---

# 15. Arquitectura funcional simplificada

```text
                         ECOSISTEMA CORPORATIVO YANBAL
          ┌─────────────────────────────────────────────────────┐
          │ SPY / Driving / SAP / Salesforce / Bus de Integración│
          └───────────────────────┬─────────────────────────────┘
                                  │
                                  ▼
                  ┌──────────────────────────────┐
                  │        BACKEND Y-TRACE        │
                  │                              │
                  │ - Seguridad / RBAC           │
                  │ - Despachos / estados        │
                  │ - Sesiones móviles           │
                  │ - Telemetría                 │
                  │ - Incidencias                │
                  │ - Evidencias                 │
                  │ - Trazabilidad               │
                  │ - Integración ESB             │
                  │ - Auditoría                  │
                  └──────────────┬───────────────┘
                                 │
                 ┌───────────────┴────────────────┐
                 │                                │
                 ▼                                ▼
       ┌─────────────────────┐          ┌────────────────────────┐
       │      WEB Y-TRACE    │          │ PWA ANDROID CONDUCTOR  │
       │   Torre de Control  │          │                        │
       │                     │          │ - Activación           │
       │ - Administración    │          │ - Datos del viaje      │
       │ - Despachos         │          │ - Inicio               │
       │ - Monitoreo         │          │ - GPS                  │
       │ - Incidencias       │          │ - Incidencias          │
       │ - Trazabilidad      │          │ - Llegada              │
       │ - Evidencias        │          │ - Entrega              │
       │ - Auditoría         │          │ - Evidencias           │
       │ - Dashboard         │          │ - Cierre               │
       └─────────────────────┘          │ - Offline / Sync       │
                                        └────────────────────────┘
```

---

# 16. Principios de funcionamiento importantes

## 16.1 El código de activación no es una contraseña

El conductor no recibe una cuenta permanente para navegar por la plataforma corporativa. Recibe un código efímero asociado a una operación concreta.

## 16.2 El código no crea un despacho

El despacho debe existir previamente en el ecosistema corporativo y ser elegible para seguimiento. Y-Trace solamente habilita el seguimiento de esa operación.

## 16.3 Llegada no significa entrega

`EN_DESTINO` representa presencia física en el destino. `ENTREGADO` requiere la confirmación manual de la entrega.

## 16.4 La fotografía no es obligatoria para cerrar

La foto es evidencia complementaria. El sistema debe conservar los eventos de trazabilidad aunque no se capture una fotografía.

## 16.5 Sin cobertura no significa pérdida del evento

La PWA trabaja con una estrategia Offline-First. Los eventos pueden almacenarse localmente y enviarse posteriormente cuando vuelva la conectividad.

## 16.6 El cierre termina la sesión móvil

Al llegar a `FINALIZADO`, la sesión y el código quedan revocados y el dispositivo deja de enviar telemetría del viaje terminado.

## 16.7 El sistema debe conservar la trazabilidad

Cada operación relevante debe quedar asociada a un despacho y registrada con sus datos temporales y de contexto necesarios para consultas y auditoría.

---

# 17. Resumen de la experiencia de uso

## Para el Supervisor de Distribución

El Supervisor entra a la Web, consulta los despachos preparados que están disponibles para seguimiento, selecciona el despacho correcto, genera el código y lo entrega al conductor. Posteriormente observa el viaje en la Torre de Control, recibe las incidencias y supervisa la llegada y el cierre.

## Para el Conductor

El Conductor abre la PWA Android, introduce el código recibido, revisa el despacho, realiza físicamente la salida y presiona **“Iniciar Despacho”**. Durante el viaje la PWA registra GPS y conserva los eventos incluso sin cobertura. Si ocurre un problema, registra una incidencia. Al llegar al destino, registra `EN_DESTINO`; después de la recepción, confirma `ENTREGADO` o registra `NO_ENTREGADO`. Finalmente presiona **“Finalizar Despacho”** y la aplicación cierra la sesión del viaje.

## Para el Punto de Destino / Agencia

El receptor recibe físicamente la carga, inspecciona los bultos y determina la conformidad o rechazo. Esa actividad física es la que permite completar la etapa de recepción.

## Para el Operador SAC

El Operador busca el despacho por código o placa, consulta el timeline y revisa las evidencias para responder consultas sin necesidad de contactar directamente al conductor para conocer el estado histórico del viaje.

## Para el Jefe de Distribución

El Jefe consulta el Dashboard, analiza el cumplimiento de tiempos y entregas, compara transportistas y obtiene reportes consolidados en Excel.

## Para el Administrador Principal

El Administrador mantiene la plataforma controlada desde el punto de vista de cuentas, roles, seguridad y auditoría, sin asumir la gestión logística de los despachos.

---

# 18. Resultado esperado de la solución

La solución Y-Trace debe permitir que, para cada despacho bajo su alcance, exista una trazabilidad digital completa y verificable:

```text
DESPACHO EXISTENTE
      ↓
DISPONIBLE PARA SEGUIMIENTO
      ↓
HABILITACIÓN POR SUPERVISOR
      ↓
CÓDIGO DE ACTIVACIÓN
      ↓
ACTIVACIÓN PWA
      ↓
EN_RUTA
      ↓
GPS / MONITOREO / INCIDENCIAS
      ↓
EN_DESTINO
      ↓
ENTREGADO o NO_ENTREGADO
      ↓
EVIDENCIAS
      ↓
FINALIZADO
      ↓
AUDITORÍA / TRAZABILIDAD / KPIs / INTEGRACIÓN
```

De esta manera, Web y PWA no funcionan como dos aplicaciones independientes, sino como dos puntos de interacción de una misma solución: **la Web controla y supervisa la operación; la PWA ejecuta y registra la operación desde el vehículo; el backend centraliza, protege, persiste e integra la información.**

---

# 19. Límites y decisiones que deben mantenerse consistentes

Para evitar contradicciones durante el diseño, implementación y modelado posterior, deben conservarse estas decisiones:

1. **Y-Trace es B2B punto a punto.** No se debe reintroducir reparto domiciliario B2C.
2. **El despacho existe antes de entrar a Y-Trace.** La plataforma no lo crea.
3. **El Supervisor habilita seguimiento, no crea logística.**
4. **La PWA es exclusivamente Android.** No se contempla iOS dentro del alcance actual.
5. **La PWA no utiliza contraseñas permanentes.** Se accede mediante códigos efímeros de operación.
6. **La llegada no equivale a entrega.** `EN_DESTINO` y `ENTREGADO` son estados diferentes.
7. **El Conductor confirma manualmente la entrega.**
8. **La fotografía es opcional y complementaria.**
9. **El GPS se detiene al finalizar el despacho.**
10. **La pérdida de conectividad no debe destruir los eventos.**
11. **La trazabilidad debe poder reconstruirse posteriormente.**
12. **Los cancelados se deben tratar de manera independiente en los indicadores de Lead Time y puntualidad.**
13. **Los reportes analíticos se exportan en Excel.**
14. **Los roles Web deben mantenerse segregados.** El Administrador Principal no sustituye al Supervisor de Distribución.
15. **Las integraciones con sistemas corporativos respetan la frontera funcional de cada sistema.**

---

# 20. Referencia respecto a los requerimientos actuales

Esta descripción consolida el estado documental actual del proyecto y sirve como visión general antes de pasar a diagramas, casos de uso del sistema, arquitectura, diseño de base de datos e implementación.

El conjunto documental actual establece **34 Requerimientos Funcionales activos (RF001 a RF034) y 23 Requerimientos No Funcionales (RNF001 a RNF023)**, con RF026 (mapa interactivo) y RF037 (bitácora de auditoría móvil) eliminados del alcance, además de los históricos RF008 y RF010. Los requerimientos funcionales cubren seguridad Web, habilitación de seguimiento, operación móvil, telemetría, incidencias, entrega, resiliencia Offline-First, monitoreo en grilla operativa, trazabilidad, indicadores, integración, seguridad móvil, auditoría y aislamiento entre transportistas.

Los CUN se mantienen como nivel de negocio y no deben convertirse directamente en botones, pantallas o funciones técnicas. Los requerimientos funcionales son las capacidades de software que soportan esos procesos.

---

# 21. Resumen ejecutivo en una sola frase

**Y-Trace es la solución Web y móvil de Yanbal Perú que permite controlar y demostrar, de extremo a extremo, qué ocurre con un despacho B2B ya existente desde su salida en el CD Lurín, durante su traslado y ante cualquier incidencia, hasta su llegada, entrega o no entrega, cierre y posterior auditoría.**
