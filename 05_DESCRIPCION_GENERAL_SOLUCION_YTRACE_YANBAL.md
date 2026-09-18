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

Y-Trace se integra dentro del ecosistema existente de Yanbal y respeta la frontera de responsabilidades de cada sistema. El despacho **existe antes de ingresar a Y-Trace**: es preparado por los sistemas logísticos upstream, los cuales proporcionan la información operativa disponible.

| Sistema / Componente | Relación con Y-Trace |
|---|---|
| **SPY / WMS** | Proporciona el contexto de despachos ya preparados y consolidados físicamente en el andén del Centro de Distribución (CD Lurín). |
| **Driving / TMS** | Aporta la información operativa relacionada con zonificación regional, destino departamental, transportista y vehículo asignado según el proceso corporativo. |
| **Bus de Integración / ESB** | Medio corporativo para publicar y recibir eventos canónicos de trazabilidad dentro del SLA $\le$ 30 min. |
| **SAP R/3 / ERP** | Recibe información de hitos y resumen de trazabilidad para los procesos corporativos posteriores (liquidación de fletes externa). |
| **Salesforce / CRM** | Recibe la actualización oportuna de entrega para brindar visibilidad a los canales comerciales y de atención al cliente. |
| **Cloud Storage** | Conserva fotografías de respaldo cifradas (AES-256) cuando el conductor decide capturarlas. |

Y-Trace **no crea despachos, no asigna rutas ni asigna vehículos**; su función inicia cuando los sistemas externos reportan un despacho preparado y disponible para seguimiento.

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

La plataforma Web será la **Torre de Control Logístico de Y-Trace**. Es la interfaz utilizada por el personal interno autorizado de Yanbal. Su función principal es **controlar y consultar el seguimiento de los despachos**, no planificar logísticamente el transporte ni crear órdenes.

Se utilizará desde computadoras o tabletas mediante un navegador corporativo y centralizará la supervisión operativa, la auditoría y la administración del acceso.

```text
                 PLATAFORMA WEB Y-TRACE
                          │
      ┌───────────────────┼────────────────────┐
      │                   │                    │
      ▼                   ▼                    ▼
  OPERACIÓN          TRAZABILIDAD          GESTIÓN
      │                   │                    │
      │                   │                    ├─ Usuarios
      │                   │                    ├─ Roles
      │                   │                    └─ Seguridad
      │                   │
      │                   ├─ Buscador
      │                   └─ Evidencias
      │
      ├─ Despachos
      ├─ Habilitación
      ├─ Monitoreo
      ├─ Incidencias
      └─ Llegadas / destino
                          │
                          ▼
                  INDICADORES / AUDITORÍA
```

La Web trabajará con autenticación mediante usuario y contraseña y aplicará control de acceso basado en roles (RBAC). Cada usuario verá solamente las operaciones y módulos permitidos por su perfil.

## 7.2 Seguridad de acceso Web

El acceso Web tendrá como características principales:

- autenticación mediante usuario y contraseña;
- autorización mediante RBAC;
- sesiones protegidas con expiración por inactividad tras 15 minutos (`RF005`);
- posibilidad de revocar sesiones;
- comunicaciones cifradas mediante HTTPS/TLS;
- bitácora de auditoría inmutable bajo modelo *append-only* (`RF006`, `RNF023`).

## 7.3 Estructura Oficial del Menú Web

Para organizar las funciones técnicas por contexto funcional sin saturar la navegación, el menú de la Torre de Control se estructura oficialmente así:

```text
Inicio

Operación
 ├── Despachos
 ├── Monitoreo
 ├── Incidencias
 └── Llegadas / Destino

Trazabilidad
 └── Buscador

Evidencias

Indicadores y Reportes

Auditoría

Integraciones

Administración
 ├── Usuarios
 ├── Roles
 └── Seguridad
```

---

# 8. Apartados de la Plataforma Web

## 8.1 Inicio / Dashboard operativo

Será la vista principal de la Torre de Control y permitirá obtener una visión resumida de la operación en tiempo real.

Podrá mostrar información como:

- despachos activos en seguimiento;
- vehículos actualmente en ruta (`EN_RUTA`);
- despachos arribados a destino (`EN_DESTINO`);
- incidencias activas en atención (`CON_INCIDENCIA`);
- operaciones próximas a superar la ventana de control de 60 minutos en destino;
- accesos directos a la grilla operativa y al buscador de trazabilidad.

## 8.2 Seguridad y acceso

Apartado orientado a la administración y protección del acceso a la plataforma.

Funciones principales:

- inicio de sesión con credenciales corporativas;
- cierre de sesión y control de inactividad;
- autorización según roles asignados;
- bloqueo preventivo tras intentos fallidos y registro en bitácora de auditoría.

## 8.3 Gestión de usuarios

Apartado utilizado exclusivamente por el Administrador Principal para la gobernanza del sistema.

Funciones:

- crear, editar, suspender y desactivar cuentas Web autorizadas;
- asignar y modificar roles (Supervisor, Jefe de Distribución, Operador SAC, Administrador);
- controlar el ciclo de vida de los usuarios.

El Administrador no utilizará este módulo para crear despachos ni para administrar la logística del viaje.

## 8.4 Contexto de "Despachos": Consulta, Selección y Habilitación de Seguimiento

Este apartado es el punto neurálgico donde el Supervisor de Distribución vincula un despacho existente con la operación de seguimiento de Y-Trace.

### A. Flujo de Generación del Código
El Supervisor **no inventa ni escribe manualmente el código**. El flujo operativo y técnico es el siguiente:

```text
Supervisor consulta despachos disponibles
        ↓
Filtra / busca por destino, fecha o transportista
        ↓
Selecciona despacho y revisa datos de transporte
        ↓
Solicita "Habilitar seguimiento"
        ↓
Backend Y-Trace valida elegibilidad:
  • Despacho existente y en estado DISPONIBLE
  • Sin código de activación activo incompatible
        ↓
Backend genera automáticamente código criptoseguro:
  • Cadena alfanumérica de 8 caracteres (ej. TRC-82F4)
  • Verifica unicidad activa en base de datos
        ↓
Backend asocia unívocamente:
  Código → Despacho → Vehículo → Conductor → Sesión Móvil
        ↓
Backend transiciona despacho a HABILITADO
        ↓
Web despliega código para que el Supervisor lo entregue al conductor
```

El código **no crea el despacho, no asigna una ruta y no asigna el vehículo**. Solamente habilita el seguimiento digital de un despacho que ya existe.

### B. Maqueta Conceptual de la Interfaz Web

**1. Vista Principal: DESPACHOS DISPONIBLES**
```text
+---------------------------------------------------------------------------------------+
| TORRE DE CONTROL Y-TRACE  >  OPERACIÓN  >  DESPACHOS DISPONIBLES                      |
+---------------------------------------------------------------------------------------+
| [Filtros: Destino [Todos    v]  Transportista [Todos    v]  Fecha [Hoy       v]]      |
+---------------------------------------------------------------------------------------+
| Código Despacho | Destino   | Vehículo | Conductor   | Estado      | Acción           |
|-----------------|-----------|----------|-------------|-------------|------------------|
| D-00125         | Arequipa  | ABC-123  | J. Pérez    | Disponible  | [ Ver Detalle ]  |
| D-00126         | Cusco     | BCD-456  | M. López    | Disponible  | [ Ver Detalle ]  |
| D-00127         | Trujillo  | CDE-789  | R. Gómez    | Disponible  | [ Ver Detalle ]  |
+---------------------------------------------------------------------------------------+
```

**2. Vista Detalle del Despacho**
```text
+---------------------------------------------------------------------------------------+
| DETALLE DE DESPACHO: D-00125                                                          |
+---------------------------------------------------------------------------------------+
| Origen:               Centro de Distribución Lurín (Lima)                             |
| Destino:              Agencia Departamental Arequipa                                  |
| Transportista:        Transportes Andinos S.A.C.                                      |
| Vehículo / Placa:     ABC-123                                                         |
| Conductor Asignado:   Juan Pérez Mendoza                                              |
| Carga Preparada:      42 bultos consolidados (Guía Remisión: 001-049281)              |
| Estado Operativo:     DISPONIBLE PARA SEGUIMIENTO                                     |
|                                                                                       |
|                       [ HABILITAR SEGUIMIENTO ]      [ Volver ]                       |
+---------------------------------------------------------------------------------------+
```

**3. Resultado Modal: CÓDIGO DE ACTIVACIÓN EMITIDO**
```text
+---------------------------------------------------------------------------------------+
| CÓDIGO DE ACTIVACIÓN GENERADO                                                     [X] |
+---------------------------------------------------------------------------------------+
|                                                                                       |
|                                     TRC-82F4                                          |
|                                                                                       |
| Despacho Asociado:    D-00125                                                         |
| Destino:              Agencia Arequipa (ABC-123 / J. Pérez)                           |
| Estado del Despacho:  HABILITADO                                                      |
| Vigencia:             Un solo uso para inicio de viaje                                |
|                                                                                       |
|                [ Copiar Código ]       [ Imprimir Ticket / Comunicar ]                |
+---------------------------------------------------------------------------------------+
```

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

Si el despacho fue cancelado antes de la salida, la trazabilidad deberá reflejar el estado `DESPACHO_CANCELADO` junto con la causal y datos de auditoría definidos.

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

La aplicación móvil será una **Progressive Web App (PWA)** desarrollada exclusivamente para dispositivos con sistema operativo **Android 8.0 o superior** (`RNF010`).

El conductor **no utilizará cuentas permanentes ni contraseñas** para iniciar el seguimiento. En su lugar, el acceso está gobernado por el **Código Único de Activación de 8 caracteres alfanuméricos** (ej. `TRC-82F4`) generado desde la Web por el Supervisor.

La PWA puede abrirse desde el navegador Chrome en Android o agregarse como acceso directo en la pantalla de inicio con soporte de Service Workers para funcionamiento autónomo.

```text
             PWA ANDROID Y-TRACE
                     │
                     ▼
                ACTIVACIÓN
                     │
                     ▼
             DATOS DEL DESPACHO
                     │
                     ▼
                  EN_RUTA
               ┌─────┼─────┐
               │     │     │
               ▼     ▼     ▼
              GPS  OFFLINE INCIDENCIA
               │     │     │
               └─────┼─────┘
                     ▼
                EN_DESTINO
                     │
                ┌────┴────┐
                ▼         ▼
            ENTREGADO  NO_ENTREGADO
                │         │
                └────┬────┘
                     ▼
              FOTO OPCIONAL
                     │
                     ▼
                 FINALIZAR
                     │
                     ▼
                FINALIZADO
```

## 9.2 Acceso y Activación mediante Código

El flujo de handoff en andén se ejecuta bajo el siguiente protocolo:

1. El Supervisor revisa el despacho existente y presiona *"Habilitar seguimiento"*.
2. El backend genera el código único de 8 caracteres (`RF008`).
3. El Supervisor entrega la carga física estibada y comunica el código al conductor.
4. El conductor abre la PWA en su smartphone Android e ingresa el código (`RF010`).
5. El backend valida el código (vigente, unívoco, despacho elegible y menos de 5 intentos fallidos acumulados, `RF032`).
6. El backend emite un token JWT de sesión operativa móvil efímera (`RF011`).
7. La PWA descarga la hoja de ruta y datos de destino a la base de datos local `IndexedDB` (`RF012`).

El código **no crea el viaje**; únicamente habilita la captura digital de trazabilidad sobre el despacho existente.

## 9.3 Protección Anti Fuerza Bruta y Código de Recuperación

- **Control de Intentos (`RF032`):** La PWA y el backend bloquean automáticamente el código tras **5 intentos fallidos consecutivos**, invalidándolo y emitiendo una alerta a la Torre de Control Web para verificación presencial.
- **Código de Recuperación en Ruta (`RF008`, `RF011`):** Ante rotura, robo o descarga total del smartphone Android durante un despacho `EN_RUTA`, el Supervisor puede emitir excepcionalmente un *Código de Recuperación* de un solo uso. Al ingresarlo en un nuevo terminal Android, se revoca la sesión previa y se restaura el viaje en curso sin pérdida del histórico y sin crear un nuevo despacho.

---

# 10. Apartados y Ciclo de Vida de la PWA Android

La PWA se estructura en **11 apartados operacionales** que guían al conductor paso a paso:

### 1. Pantalla de Activación
- Campo de entrada de 8 caracteres alfanuméricos: `[________]`.
- Botón `[ Activar Despacho ]`.
- Control y conteo visual de intentos restantes (máximo 5 intentos antes de bloqueo).

### 2. Información del Despacho / Hoja de Ruta (`RF012`)
Tras la activación, despliega la información operativa requerida para el viaje:
- Código de despacho (ej. `D-00125`).
- Punto de destino y dirección de la sede o agencia regional.
- Indicaciones u observaciones de ruta.
- Estado operativo actual (`HABILITADO`).

### 3. Inicio del Despacho (`RF013`)
- Botón prominente: `[ Iniciar Despacho ]`.
- Al pulsarlo, el despacho transiciona a `EN_RUTA`, se estampa fecha, hora y coordenada GPS de salida en CD Lurín, y se arranca el servicio de telemetría en segundo plano.

### 4. Operación en Ruta: Telemetría GPS en Segundo Plano (`RF014`)
- El conductor **no tiene que presionar ningún botón para enviar su ubicación**.
- El servicio en segundo plano invoca la API de geolocalización de Android y muestrea coordenadas automáticamente **cada 10 minutos** durante todo el trayecto interprovincial (24 horas a 7 días).
- Los puntos capturados se asocian de forma indivisible a la sesión activa y al identificador del despacho.

### 5. Reporte Formal de Incidencias en Ruta (`RF019`, `RF026`)
- Botón accesible: `[ Reportar Incidencia ]`.
- Formulario modal: selección de tipo de contingencia (avería mecánica, siniestro vial, bloqueo carretero, desvío climático), descripción obligatoria y captura automática de coordenadas GPS actuales.
- Permite adjuntar fotografía complementaria si las condiciones de seguridad lo permiten (`RF018`).
- Al enviar, dispara una alerta prioritaria con alarma sonora y visual en la Torre de Control Web (`RF026`).

### 6. Indicador Visual de Estado de Sincronización Local (`RF023`)
- Ubicado permanentemente en la cabecera (*header*) de la aplicación:
  - 🟢 **Sincronizado:** Cola local vacía, datos al día en el servidor central.
  - 🟡 **12 eventos pendientes:** Indica que existen transacciones almacenadas localmente a la espera de cobertura celular.
- **Clarificación de Capacidad Offline:** La aplicación debe soportar **como mínimo una cola local de 500 eventos operativos** en `IndexedDB` (`RF021`, `RNF013`) durante ausencias prolongadas de señal celular, sin pérdida ni degradación funcional. Los 500 eventos representan un **criterio mínimo de prueba y aceptación operativa**, no un límite máximo o techo de almacenamiento de la aplicación.
- Al recuperar conectividad, el servicio ejecuta sincronización automática FIFO en segundo plano (`RF022`).

### 7. Llegada a Destino (`RF015`)
- Protocolo híbrido: detección perimétrica automática al cruzar la geocerca de 500 metros del destino o mediante el botón manual de respaldo `[ Llegué a Destino ]`.
- Registra atómicamente el evento `EN_DESTINO` con fecha, hora y coordenadas GPS.
- *Regla de Negocio:* `EN_DESTINO` **no equivale a entrega**. Solo acredita la presencia física en el almacén e inicia la ventana de control de 60 minutos (`RF026`, `RN-PV-01`).

### 8. Confirmar Entrega Conforme (`RF016`)
- Tras la descarga física y la conformidad del receptor en destino:
- Botón de acción consciente obligatoria: `[ Confirmar Entrega ]`.
- Transiciona formalmente a `ENTREGADO`.

### 9. Registrar No Entrega / Rechazo (`RF017`)
- En caso de rechazo total, daño severo o imposibilidad física de recepción:
- Selector de causales tipificadas (mercadería dañada, discrepancia de bultos, agencia cerrada, rechazo de receptor) y campo de fundamentación.
- Transiciona formalmente a `NO_ENTREGADO`.

### 10. Evidencia Fotográfica Complementaria (`RF018`)
- Captura de foto de la guía sellada o de la causal de rechazo.
- **Carácter Opcional y No Bloqueante:** La fotografía es una evidencia de respaldo complementaria; su omisión (por falta de cámara, batería o condiciones lumínicas) **no bloquea la confirmación de entrega ni el registro de rechazo**. La evidencia base reside en las estampas atómicas de GPS, fecha y hora.

### 11. Finalizar Despacho y Cese de Telemetría (`RF020`, `RF033`, `RNF001`)
- Botón `[ Finalizar Despacho ]`, el cual solo se habilita si se cumplen dos condiciones concurrentes:
  1. El despacho se encuentra en estado `ENTREGADO` o `NO_ENTREGADO`.
  2. La cola local de sincronización en `IndexedDB` está completamente vacía (indicador en verde).
- Al ejecutarse:
  1. Transiciona a `FINALIZADO`.
  2. El backend revoca inmediatamente el token JWT de sesión móvil.
  3. El código de activación se extingue y queda invalidado de forma permanente.
  4. La PWA purga los datos temporales del viaje de la memoria local.
  5. Cesa mandatoriamente todo muestreo y captura de telemetría GPS (`RNF001`).
  6. La aplicación regresa a la pantalla inicial de activación.

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

El flujo integral de Y-Trace responde a la secuencia oficial:

```text
1. Preparación externa
   SPY/WMS prepara carga en andén + Driving/TMS asigna transporte
           ↓
2. Despacho disponible para seguimiento
   Supervisor consulta en Torre Web, filtra y selecciona despacho
           ↓
3. Solicitud de habilitación
   Supervisor verifica datos y solicita "Habilitar seguimiento"
           ↓
4. Generación automática en Backend
   Backend valida elegibilidad, genera código (ej. TRC-82F4) y pasa a HABILITADO
           ↓
5. Comunicación del código
   Supervisor comunica código al conductor en andén de CD Lurín
           ↓
6. Activación móvil
   Conductor ingresa código en PWA Android (< 5 intentos)
           ↓
7. Emisión de sesión y descarga local
   Backend emite token efímero y PWA guarda hoja de ruta en IndexedDB
           ↓
8. Inicio de despacho
   Conductor presiona “Iniciar Despacho” → pasa a EN_RUTA
           ↓
9. Bucle de traslado y monitoreo continuo
   PWA captura telemetría GPS cada 10 min en segundo plano (IndexedDB en sombra vial)
           ↓
10. Incidencia vial (solo si ocurre contingencia)
    Conductor reporta con GPS/foto → Supervisor atiende alarma en Torre Web
    (Mitigable: auxilio vial o código de recuperación | Siniestro total: cierre forzado)
           ↓
11. Llegada a instalaciones de destino
    Geocerca o botón “Llegué a Destino” → pasa a EN_DESTINO (ventana de 60 min)
           ↓
12. Inspección y recepción
    Punto de Destino / Receptor inspecciona precintos y bultos
           ↓
13. Formalización consciente del resultado
    ENTREGADO (confirmación conforme)  o  NO_ENTREGADO (rechazo con causal tipificada)
           ↓
14. Respaldo complementario
    Fotografía opcional de guía sellada o rechazo (evidencia no bloqueante)
           ↓
15. Cierre formal
    Conductor presiona “Finalizar Despacho” con cola local vacía
           ↓
16. FINALIZADO
    Revocación de sesión, extinción de código, purga local y cese mandatorio de GPS
           ↓
17. Propagación corporativa y analítica
    Resumen al Bus ESB (SLA ≤ 30 min) → SAP/Salesforce → KPIs en Dashboard Web
```

---

# 12. Estados principales del despacho

El seguimiento se apoya en los **estados operacionales oficiales** del sistema:

| Estado Oficial | Evento Detonante | Actor Responsable | Significado Operativo y Efecto Técnico |
|---|---|:---:|---|
| **DISPONIBLE PARA SEGUIMIENTO** | Carga preparada comunicada por SPY/Driving. | Sistemas Externos | El despacho existe y está listo para ser consultado en la Torre Web. |
| **HABILITADO** | Backend genera código de activación exitosamente. | Backend Y-Trace | Código único emitido (ej. `TRC-82F4`) y registrado el hito de control previo. |
| **EN_RUTA** | Conductor pulsa *"Iniciar Despacho"*. | Conductor PWA | Salida física de CD Lurín; inicia captura periódica de GPS cada 10 min y publicación al Bus. |
| **CON_INCIDENCIA** | Conductor reporta anomalía o siniestro vial. | Conductor PWA | **Solo durante contingencia activa**; alarma sonora/visual en Torre Web y notificación al Bus. |
| **EN_DESTINO** | Geocerca perimétrica o botón manual de llegada. | Sistema / Conductor | Llegada física al perímetro de destino; inicia ventana de control de 60 min. **No equivale a entrega.** |
| **ENTREGADO** | Conductor pulsa conscientemente *"Confirmar Entrega"*. | Conductor PWA | Entrega conforme verificada con coordenadas y hora atómica; foto de respaldo opcional. |
| **NO_ENTREGADO** | Conductor selecciona causal tipificada de rechazo. | Conductor PWA | Entrega frustrada registrada formalmente con sustento inmutable en bitácora. |
| **FINALIZADO** | Conductor pulsa *"Finalizar Despacho"* o Supervisor fuerza cierre. | Conductor / Supervisor | Cierre formal del seguimiento; revocación inmediata de sesión móvil, extinción de código y cese mandatorio de telemetría GPS. |
| **DESPACHO_CANCELADO** | Evento externo cancela el despacho antes de la salida física. | Sistemas Externos | Rama independiente desde `DISPONIBLE PARA SEGUIMIENTO`; revoca código asignado y registra justificación administrativa en auditoría. |

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
