# Descripción General de la Solución Y-Trace — Yanbal Perú

## 1. Identificación del proyecto

**Nombre del proyecto:** Y-Trace  
**Nombre completo:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú  
**Alcance:** Distribución completa entre puntos de distribución (B2B), desde el Centro de Distribución de Lurín hasta agencias, centros de distribución secundarios y otros puntos de destino.  
**Propósito principal:** Proporcionar trazabilidad operativa de extremo a extremo sobre despachos que ya fueron creados y preparados en los sistemas corporativos de Yanbal, permitiendo habilitar su seguimiento, controlar su salida, registrar y consultar su recorrido mediante telemetría GPS, controlar los cambios de estado, registrar la llegada y el resultado de entrega, calcular tiempos de operación y consultar posteriormente el resumen de trazabilidad.

> **Importante:** Y-Trace no reemplaza los sistemas corporativos que crean, preparan, programan o gestionan logísticamente los despachos. Su función comienza cuando existe un despacho disponible para seguimiento y termina cuando la operación queda formalmente cerrada o cancelada y su trazabilidad queda registrada.

---

## 2. ¿De qué tratará Y-Trace?

Y-Trace será una solución compuesta por dos partes principales que trabajan de manera integrada:

1. **Plataforma Web:** utilizada por el personal autorizado de Yanbal para administrar el acceso, consultar despachos disponibles, habilitar el seguimiento, monitorear operaciones activas, ejecutar cancelaciones forzadas cuando exista una comunicación externa, consultar la trazabilidad y analizar indicadores.

2. **Aplicación móvil nativa Android:** utilizada por el conductor del socio logístico para activar el despacho mediante un código temporal, consultar la información necesaria del viaje, iniciar el traslado, transmitir la posición GPS, operar sin conectividad temporal, registrar la llegada mediante geocerca, confirmar o registrar el resultado de la entrega y finalizar el seguimiento.

El sistema mantiene una única trazabilidad del despacho. La Web funciona como la **Torre de Control**, mientras que la App Nativa Android constituye el **punto operativo móvil del conductor**.

La aplicación móvil **no es una PWA**. Es una aplicación nativa Android destinada al dispositivo móvil utilizado por el conductor, con servicios de ejecución en segundo plano para sostener el tracking GPS durante la operación.

El alcance es estrictamente **B2B punto a punto**. No se contempla reparto domiciliario B2C, gestión de ventas, picking, inventarios ni planificación logística.

---

## 3. Problema que busca resolver

La solución está orientada a mejorar el control y visibilidad del traslado nacional de cargas consolidadas entre puntos de distribución. El sistema busca evitar que la organización dependa exclusivamente de llamadas telefónicas o información dispersa para saber:

- qué despacho está preparado para salir;
- qué unidad realizará el traslado;
- cuándo se activó el seguimiento;
- cuándo salió realmente del Centro de Distribución;
- dónde se encontraba la unidad durante el recorrido;
- qué estados ha alcanzado el despacho;
- cuándo ingresó al radio del punto de destino;
- cuándo se produjo el resultado de entrega o no entrega;
- cuánto tiempo tomó el traslado;
- cuánto tiempo transcurrió entre los principales hitos; y
- cuál fue el desempeño de los transportistas.

Por ello, Y-Trace centraliza la información operativa y genera una línea de tiempo verificable de cada despacho mediante registros de eventos, posiciones GPS, estados y marcas temporales.

---

## 4. Frontera de la solución

### 4.1 Lo que sí hará Y-Trace

Y-Trace realizará principalmente las siguientes actividades:

- consultar despachos existentes y elegibles para seguimiento;
- habilitar el seguimiento mediante un Código Único de Activación;
- generar y validar la activación del conductor en la App Nativa Android;
- crear y mantener una sesión operativa móvil vinculada al despacho, vehículo, conductor y dispositivo;
- conservar localmente la sesión operativa y los eventos necesarios cuando no exista conectividad;
- permitir la reconexión automática utilizando la sesión operativa previamente establecida;
- mostrar al conductor los datos operativos necesarios del despacho y destino;
- registrar el inicio formal del traslado;
- capturar telemetría GPS durante la operación en ruta;
- registrar eventos y coordenadas con sus correspondientes fechas y horas;
- detectar automáticamente la entrada del vehículo en el radio configurado del punto de destino y cambiar el estado a `EN_DESTINO`;
- registrar el resultado de entrega como `ENTREGADO` o `NO_ENTREGADO`;
- sincronizar automáticamente los eventos pendientes cuando se restablezca la conectividad;
- mostrar el estado de sincronización de los eventos almacenados localmente;
- monitorear el estado y la última posición registrada de los despachos activos desde la Torre de Control;
- permitir al Supervisor ejecutar una **Cancelación Forzada del Seguimiento** cuando reciba una comunicación externa;
- revocar la sesión y detener el tracking cuando un despacho sea finalizado o cancelado;
- invalidar el Código de Activación para impedir su reutilización;
- consultar la trazabilidad histórica del despacho;
- consultar el resumen del recorrido, sus tiempos y sus principales eventos;
- calcular indicadores operativos y de rendimiento; y
- publicar eventos y resúmenes de trazabilidad mediante el Bus de Integración corporativo.

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
- gestionar, investigar o atender incidencias operativas;
- recibir reportes de incidencias mediante formularios dentro de la aplicación;
- enviar alertas específicas de incidencias desde la App;
- almacenar fotografías, imágenes o POD multimedia;
- gestionar almacenamiento de archivos multimedia;
- liquidar económicamente los fletes; ni
- sustituir los sistemas corporativos existentes responsables de dichas actividades.

> **Regla de contingencia:** si ocurre una situación operativa externa que requiera detener el seguimiento, el conductor la comunica mediante el canal establecido por Yanbal fuera de Y-Trace. El personal autorizado recibe dicha comunicación y, cuando corresponda, ejecuta desde la Web una **Cancelación Forzada del Seguimiento**. Y-Trace registra la cancelación y ejecuta sus efectos técnicos, pero no administra la incidencia que originó la decisión.

---

## 5. Sistemas externos con los que se relaciona

Y-Trace se integra dentro del ecosistema existente de Yanbal y respeta la frontera de responsabilidades de cada sistema. El despacho **existe antes de ingresar a Y-Trace**: es preparado por los sistemas logísticos upstream, que proporcionan la información operativa disponible.

| Sistema / Componente         | Relación con Y-Trace                                                                                                                         |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **SPY / WMS**                | Proporciona el contexto de despachos ya preparados y consolidados físicamente en el Centro de Distribución (CD Lurín).                       |
| **Driving / TMS**            | Aporta la información operativa relacionada con destino, transportista y vehículo asignado según el proceso corporativo.                     |
| **Bus de Integración / ESB** | Medio corporativo para publicar y recibir eventos canónicos de trazabilidad.                                                                 |
| **SAP R/3 / ERP**            | Participa en los procesos corporativos posteriores que correspondan y puede informar eventos externos que afecten el estado de la operación. |
| **Salesforce / CRM**         | Recibe la información de entrega y trazabilidad que corresponda para proporcionar visibilidad a los canales comerciales y de atención.       |

Y-Trace **no crea despachos, no planifica rutas y no asigna logísticamente vehículos**; su función inicia cuando los sistemas externos reportan un despacho preparado y disponible para seguimiento.

La integración con el Bus pertenece a la solución como mecanismo de intercambio de eventos y resúmenes, mientras que el Bus continúa siendo un componente corporativo externo a Y-Trace.

---

# 6. Actores que intervienen en la solución

## 6.1 Actores del negocio

El modelo del negocio identifica cinco actores principales:

| Actor                                    | Tipo    | Función principal                                                                                                                                                                           |
| ---------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Socio Logístico / Conductor**          | Externo | Traslada físicamente la carga, utiliza la App Nativa Android, activa el seguimiento, inicia el viaje, alimenta la telemetría GPS, registra el resultado de entrega y finaliza la operación. |
| **Punto de Destino / Agencia Receptora** | Externo | Recibe físicamente la carga y determina la conformidad o rechazo de la recepción.                                                                                                           |
| **Supervisor de Distribución**           | Interno | Consulta despachos disponibles, habilita el seguimiento, monitorea las operaciones y ejecuta cancelaciones forzadas cuando corresponde.                                                     |
| **Jefe de Distribución**                 | Interno | Analiza indicadores, tiempos de operación, cumplimiento y rendimiento de transportistas.                                                                                                    |
| **Operador SAC / Soporte Logístico**     | Interno | Consulta la trazabilidad y el resumen del seguimiento para atender consultas operativas.                                                                                                    |

Los cinco actores se relacionan con los procesos de negocio definidos para Y-Trace.

## 6.2 Rol adicional de administración de plataforma

La solución Web incorpora además al **Administrador Principal del Sistema**.

Este rol es responsable de la gobernanza técnica y de seguridad de la plataforma: administra cuentas Web, asigna roles, controla el acceso y consulta las bitácoras de auditoría.

> **Distinción importante:** el Administrador Principal es un rol de administración de plataforma. No debe confundirse con el Supervisor de Distribución ni asumir automáticamente las funciones operativas de seguimiento.

---

# 7. ¿Cómo funcionará la Plataforma Web?

## 7.1 Concepto general

La plataforma Web será la **Torre de Control de Y-Trace**. Es la interfaz utilizada por el personal interno autorizado de Yanbal.

Su función principal es **controlar, consultar y supervisar el seguimiento de los despachos**, no planificar logísticamente el transporte ni crear órdenes.

Se utilizará desde computadoras o tabletas mediante un navegador corporativo y centralizará:

- seguridad y administración de usuarios;
- consulta y habilitación de despachos;
- monitoreo de operaciones activas;
- cancelación forzada del seguimiento;
- consulta de trazabilidad;
- indicadores y reportes; e
- información del estado de integración.

### 7.2 Vista funcional general

```text
                         PLATAFORMA WEB Y-TRACE
                                  │
             ┌────────────────────┼────────────────────┐
             │                    │                    │
             ▼                    ▼                    ▼
         OPERACIÓN           TRAZABILIDAD          GESTIÓN
             │                    │                    │
             ├─ Despachos         ├─ Buscador           ├─ Usuarios
             ├─ Habilitación      ├─ Línea de tiempo    ├─ Roles
             ├─ Monitoreo         ├─ GPS / estados      └─ Seguridad
             ├─ Llegadas          └─ Resumen
             └─ Cancelación
                  forzada
                                  │
                                  ▼
                        INDICADORES / INTEGRACIÓN
```

La Web trabajará con autenticación mediante usuario y contraseña y aplicará control de acceso basado en roles (RBAC). Cada usuario verá solamente las operaciones y módulos permitidos por su perfil.

## 7.3 Seguridad de acceso Web

El acceso Web tendrá como características principales:

- autenticación mediante usuario y contraseña;
- autorización mediante RBAC;
- sesiones protegidas con expiración por inactividad;
- posibilidad de revocar sesiones;
- comunicaciones cifradas mediante HTTPS/TLS; y
- bitácora de auditoría inmutable bajo modelo *append-only*.

## 7.4 Estructura del menú Web

```text
Inicio

Operación
 ├── Despachos
 ├── Monitoreo
 ├── Llegadas / Destino
 └── Cancelación de Seguimiento

Trazabilidad
 └── Consulta y Resumen

Indicadores y Reportes

Auditoría

Integraciones

Administración
 ├── Usuarios
 ├── Roles
 └── Seguridad
```

No existe un módulo de incidencias ni un módulo de evidencias multimedia.

---

# 8. Apartados de la Plataforma Web

## 8.1 Inicio / Dashboard operativo

Será la vista principal de la Torre de Control y permitirá obtener una visión resumida de la operación activa.

Podrá mostrar:

- despachos habilitados o en seguimiento;
- vehículos actualmente en `EN_RUTA`;
- despachos en `EN_DESTINO`;
- despachos recientemente finalizados;
- despachos cancelados;
- tiempos relevantes de operación; y
- accesos directos al monitoreo y al buscador de trazabilidad.

## 8.2 Seguridad y acceso

Apartado orientado a la administración y protección del acceso a la plataforma.

Funciones principales:

- inicio y cierre de sesión;
- control de inactividad;
- autorización según roles;
- gestión de bloqueos por intentos fallidos; y
- registro de acciones de seguridad.

## 8.3 Gestión de usuarios

Apartado utilizado exclusivamente por el Administrador Principal.

Funciones:

- crear, editar, suspender y desactivar cuentas Web;
- asignar y modificar roles;
- controlar el ciclo de vida de los usuarios.

El Administrador no utilizará este módulo para crear despachos ni para administrar la logística del viaje.

## 8.4 Despachos: Consulta, Selección y Habilitación de Seguimiento

Este apartado es el punto en el que el Supervisor vincula un despacho existente con el seguimiento de Y-Trace.

### Flujo de habilitación

```text
Supervisor consulta despachos disponibles
        ↓
Filtra / busca el despacho
        ↓
Selecciona el despacho
        ↓
Solicita "Habilitar seguimiento"
        ↓
Backend valida:
  • despacho existente
  • elegibilidad para seguimiento
  • ausencia de código activo incompatible
        ↓
Backend genera código único
        ↓
Asocia:
  Código → Despacho → Vehículo → Conductor → Sesión
        ↓
Despacho pasa a HABILITADO
        ↓
Web muestra el código al Supervisor
```

El código **no crea el despacho, no asigna una ruta y no asigna el vehículo**. Solamente habilita el seguimiento digital de una operación que ya existe.

## 8.5 Monitoreo operativo

Será la grilla de seguimiento de la Torre de Control.

Permitirá consultar:

- código del despacho;
- vehículo;
- conductor;
- destino;
- estado actual;
- última coordenada registrada;
- hora de última actualización; y
- tiempo transcurrido desde el último evento.

Semáforo principal:

- **Verde:** `EN_RUTA`.
- **Amarillo:** `EN_DESTINO`.
- **Gris:** `FINALIZADO` o `DESPACHO_CANCELADO`.

No existe el estado `CON_INCIDENCIA`.

## 8.6 Cancelación Forzada del Seguimiento

No constituye un módulo de gestión de incidencias. Es una función de **control operativo excepcional**.

El flujo es:

```text
Situación operativa externa
        ↓
Conductor comunica por canal externo
        ↓
Personal responsable recibe la comunicación
        ↓
Supervisor ejecuta "Cancelación Forzada"
        ↓
Y-Trace registra motivo mínimo
        ↓
Despacho → DESPACHO_CANCELADO
        ↓
Revoca sesión móvil
        ↓
Invalida código
        ↓
Bloquea nuevos envíos del despacho
        ↓
Detiene el tracking en la App
        ↓
La App sale a pantalla inicial
        ↓
Y-Trace publica DESPACHO_CANCELADO al Bus
```

Y-Trace registra únicamente la información necesaria para la trazabilidad de la cancelación. No administra la incidencia externa ni mantiene un expediente de contingencia.

## 8.7 Control de llegada y destino

Y-Trace diferencia claramente:

`EN_RUTA` = el vehículo se encuentra en traslado.

`EN_DESTINO` = el GPS del dispositivo entra al radio configurado del punto de destino.

`ENTREGADO` = el conductor confirma la recepción conforme.

`NO_ENTREGADO` = el conductor registra que la entrega no se concretó.

El cambio a `EN_DESTINO` se produce automáticamente cuando el sistema detecta la entrada al radio de geocerca definido para el destino.

La proximidad a la geocerca **no equivale a entrega**.

## 8.8 Trazabilidad / Consulta y Resumen del Seguimiento

Este es el módulo destinado a consultar el historial del despacho.

La búsqueda podrá realizarse por:

- código de despacho; o
- placa vehicular.

El resultado mostrará la línea de tiempo:

```text
Disponibilidad
    ↓
Habilitación
    ↓
Activación
    ↓
Inicio de traslado
    ↓
Eventos GPS / estados
    ↓
EN_DESTINO
    ↓
ENTREGADO / NO_ENTREGADO
    ↓
FINALIZADO
```

Cuando exista una cancelación forzada, la línea de tiempo mostrará:

```text
EN_RUTA → DESPACHO_CANCELADO
```

junto con la fecha, hora y motivo mínimo registrado.

El resumen debe permitir reconstruir el seguimiento mediante:

- posiciones GPS;
- estados;
- fechas y horas;
- duración total;
- duración entre hitos;
- última ubicación registrada;
- resultado de la entrega; y
- cancelación, si corresponde.

No se manejan fotografías, imágenes ni archivos POD.

## 8.9 Auditoría

Apartado destinado a supervisar el historial de acciones y eventos relevantes.

Podrá consultar:

- accesos;
- intentos fallidos;
- cambios administrativos;
- activaciones;
- cancelaciones forzadas;
- finalizaciones; y
- eventos de seguridad.

Los registros de auditoría serán inmutables bajo el modelo definido.

## 8.10 Dashboard ejecutivo e indicadores

Este apartado será utilizado principalmente por el Jefe de Distribución.

Permitirá analizar:

- **Lead Time** del traslado;
- tiempo entre hitos;
- entregas y no entregas;
- latencia de integración;
- cancelaciones como métrica independiente; y
- desempeño comparativo entre empresas transportistas.

Los reportes consolidados podrán exportarse en **Excel (.xlsx)**.

## 8.11 Integraciones y estado de eventos

Este apartado permitirá visualizar el estado de los eventos que deben ser comunicados al Bus de Integración.

Y-Trace controlará el cumplimiento del SLA definido para la publicación y aceptación de eventos desde su recepción en el backend.

---

# 9. ¿Cómo funcionará la aplicación móvil?

## 9.1 Concepto general

La aplicación móvil será una **Aplicación Nativa Android**, no una PWA.

Estará orientada al smartphone utilizado por el conductor y permitirá:

- activar un despacho mediante código;
- consultar sus datos operativos;
- iniciar el traslado;
- capturar telemetría GPS;
- trabajar temporalmente sin conectividad;
- sincronizar automáticamente los eventos pendientes;
- detectar la llegada mediante geocerca;
- registrar el resultado de la entrega; y
- finalizar el seguimiento.

La aplicación utilizará servicios de ejecución en segundo plano para mantener la captura de ubicación durante la operación.

## 9.2 Acceso mediante Código de Activación

El conductor no utilizará una cuenta permanente ni una contraseña corporativa.

El flujo será:

```text
Supervisor habilita seguimiento
        ↓
Backend genera código único
        ↓
Conductor recibe código
        ↓
Conductor ingresa código
        ↓
Backend valida el código
        ↓
Se crea la sesión operativa
        ↓
El código queda consumido para esa activación
```

Una vez establecida la sesión, el código **no podrá utilizarse nuevamente para iniciar otra sesión sobre ese despacho mientras exista una sesión operativa activa**.

## 9.3 Sesión operativa persistente ante pérdida de Internet

Esta es una característica central de la aplicación.

Una vez realizada correctamente la activación, la App Nativa conserva de forma segura la información necesaria de la sesión operativa y del despacho.

La pérdida de Internet **no debe cerrar ni romper el enlace operativo**.

```text
Activación correcta
        ↓
Sesión operativa establecida
        ↓
Token / referencia de sesión persistida localmente
        ↓
Internet se pierde
        ↓
La sesión local continúa
        ↓
GPS y eventos se almacenan localmente
        ↓
Internet vuelve
        ↓
La App intenta reconectarse automáticamente
        ↓
Backend valida la sesión
        ↓
Continúa el mismo despacho
```

Por tanto, el conductor **no debe volver a introducir el Código de Activación simplemente porque perdió conectividad**.

La sesión permanece válida mientras el despacho continúe activo y hasta que el sistema la revoque por finalización o cancelación.

## 9.4 Telemetría GPS

Cuando el despacho se encuentra en `EN_RUTA`, la App Nativa:

- obtiene la ubicación mediante los servicios de geolocalización de Android;
- registra posiciones periódicamente;
- asocia cada posición al despacho y a la sesión operativa; y
- conserva localmente los registros pendientes cuando no hay conexión.

El tracking se mantiene activo mientras el despacho siga operativo.

## 9.5 Llegada automática mediante geocerca

La llegada al destino no requiere que el conductor informe manualmente que se encuentra cerca.

El sistema utilizará un radio configurado alrededor del punto de destino.

```text
EN_RUTA
   ↓
GPS detecta entrada en radio del destino
   ↓
EN_DESTINO
   ↓
Se registra:
   • fecha
   • hora
   • coordenada
   • despacho
   • sesión
```

`EN_DESTINO` representa proximidad/presencia en la zona de destino y no constituye por sí misma la confirmación de entrega.

## 9.6 Confirmación de entrega

Una vez que físicamente se complete la recepción, el conductor podrá realizar la acción:

**“Confirmar Entrega”**

El sistema registra el evento y cambia a:

`ENTREGADO`

Si la entrega no se concreta, el conductor registrará:

`NO_ENTREGADO`

según las causales y reglas operativas definidas.

## 9.7 Finalización

Después de obtener un resultado final válido (`ENTREGADO` o `NO_ENTREGADO`) y completar la sincronización pendiente, el conductor podrá finalizar el despacho.

Al ejecutarse:

1. el despacho pasa a `FINALIZADO`;
2. el backend revoca la sesión móvil;
3. el Código de Activación queda invalidado;
4. se detiene el tracking GPS;
5. la App abandona el contexto del viaje; y
6. la aplicación vuelve a la pantalla inicial.

Después de `FINALIZADO`, el dispositivo no debe seguir enviando datos correspondientes a ese viaje.

## 9.8 Cancelación Forzada durante la ruta

Si durante `EN_RUTA` ocurre una situación externa que requiera interrumpir el seguimiento:

```text
Situación externa
      ↓
Comunicación fuera de Y-Trace
      ↓
Supervisor recibe la indicación
      ↓
Cancelación Forzada en Web
      ↓
DESPACHO_CANCELADO
      ↓
Revocación de sesión
      ↓
Código invalidado
      ↓
Tracking detenido
      ↓
Bloqueo de nuevos eventos
      ↓
Salida de la App
```

Si el dispositivo está conectado, la aplicación recibe la revocación y aplica el cierre.

Si el dispositivo está temporalmente sin conectividad, la revocación queda registrada en el backend; al recuperar conexión, la App valida el estado del despacho, detecta que la sesión ya no es válida y realiza el cierre local correspondiente.

## 9.9 Funcionamiento Offline y sincronización

La App Nativa utiliza una estrategia **Offline-First**.

Los eventos operativos y coordenadas GPS se almacenan localmente mientras no exista conexión.

La cola local conserva los eventos pendientes en orden cronológico.

Cuando vuelve la conectividad:

```text
Conectividad restaurada
        ↓
Validación de sesión
        ↓
Envío de eventos pendientes
        ↓
Backend procesa e identifica duplicados
        ↓
Confirmación de recepción
        ↓
El evento deja de estar pendiente
```

La pérdida temporal de Internet no implica pérdida del recorrido ni necesidad de una nueva activación.

---

# 10. Apartados y ciclo de vida de la App Nativa Android

La App Nativa se estructura alrededor del ciclo operativo del despacho:

### 1. Activación

- Campo para ingresar el Código de Activación.
- Validación del código.
- Control de intentos fallidos.

### 2. Información del despacho

- Código de despacho.
- Destino.
- Dirección.
- Datos operativos necesarios para el viaje.
- Estado actual.

### 3. Inicio del despacho

- Botón **“Iniciar Despacho”**.
- Cambio a `EN_RUTA`.
- Registro de fecha, hora y ubicación inicial.
- Inicio del tracking GPS.

### 4. Tracking GPS

- Ejecución automática en segundo plano.
- Asociación de cada coordenada al despacho y sesión.
- Persistencia local ante ausencia de conectividad.

### 5. Estado de sincronización

- Indicador de sincronización.
- Cantidad de eventos pendientes.
- Sincronización automática al regresar la conectividad.

### 6. Llegada a destino

- Detección automática mediante geocerca.
- Cambio a `EN_DESTINO`.
- Registro de fecha, hora y posición.

### 7. Resultado de entrega

- **Confirmar Entrega** → `ENTREGADO`.
- **Registrar No Entrega** → `NO_ENTREGADO`.

### 8. Finalización

- Verificación de resultado final.
- Verificación de eventos sincronizados.
- Cambio a `FINALIZADO`.
- Revocación de sesión.
- Invalidación del código.
- Cese de tracking.
- Retorno a pantalla inicial.

### 9. Cancelación forzada

No es una acción originada por el conductor dentro de la app.

La cancelación se origina en la Web por personal autorizado después de una comunicación externa.

---

# 11. Flujo completo de la operación

El flujo integral de Y-Trace responde a la siguiente secuencia:

```text
1. Preparación externa
   SPY/WMS prepara la carga y los sistemas externos aportan el contexto del despacho
                         ↓

2. Despacho disponible para seguimiento
   Supervisor consulta el despacho en la Torre Web
                         ↓

3. Habilitación
   Supervisor selecciona "Habilitar seguimiento"
                         ↓

4. Generación automática del código
   Backend genera el Código de Activación
                         ↓

5. Activación móvil
   Conductor ingresa el código en la App Nativa Android
                         ↓

6. Sesión operativa
   Backend crea la sesión y la App conserva la referencia localmente
                         ↓

7. Inicio del traslado
   Conductor presiona "Iniciar Despacho"
                         ↓

8. EN_RUTA
   Inicia tracking GPS y registro de eventos
                         ↓

9. Pérdida de conectividad, si ocurre
   La sesión permanece asociada
   GPS/eventos se conservan localmente
                         ↓

10. Reconexión
    App valida la misma sesión y sincroniza eventos pendientes
                         ↓

11. Llegada al destino
    GPS entra en el radio configurado
                         ↓

12. EN_DESTINO
    Se registra fecha, hora y coordenada
                         ↓

13. Resultado
    ENTREGADO o NO_ENTREGADO
                         ↓

14. Finalización
    Se verifica sincronización pendiente y se cierra el viaje
                         ↓

15. FINALIZADO
    Sesión revocada + código invalidado + tracking detenido
                         ↓

16. Resumen e integración
    Trazabilidad histórica + indicadores + publicación de eventos al Bus
```

### Rama excepcional de cancelación

```text
EN_RUTA
   ↓
Situación externa comunicada fuera de Y-Trace
   ↓
Supervisor ejecuta Cancelación Forzada
   ↓
DESPACHO_CANCELADO
   ↓
Revocación de sesión
   ↓
Código invalidado
   ↓
Tracking detenido
   ↓
Bloqueo de nuevos eventos
   ↓
Salida de la App
   ↓
Publicación de DESPACHO_CANCELADO al Bus
```

---

# 12. Estados principales del despacho

El seguimiento se apoya en los siguientes estados operacionales:

| Estado | Evento detonante | Actor / origen | Significado |
|---|---|---|---|
| **DISPONIBLE PARA SEGUIMIENTO** | Despacho preparado por sistemas externos | Sistemas externos | El despacho existe y está disponible para ser habilitado en Y-Trace. |
| **HABILITADO** | Generación exitosa del código | Backend Y-Trace | El despacho queda habilitado para activación móvil. |
| **EN_RUTA** | Conductor pulsa “Iniciar Despacho” | Conductor / App | Comienza el traslado y el tracking GPS. |
| **EN_DESTINO** | GPS entra en el radio configurado | Sistema | El vehículo se encuentra dentro de la zona de destino. No equivale a entrega. |
| **ENTREGADO** | Confirmación de entrega | Conductor / App | El despacho fue recibido conforme. |
| **NO_ENTREGADO** | Registro del resultado de no entrega | Conductor / App | La entrega no se concretó. |
| **FINALIZADO** | Cierre del seguimiento | Conductor / Supervisor | Se cierra la operación, se revoca la sesión y cesa el tracking. |
| **DESPACHO_CANCELADO** | Cancelación Forzada o cancelación externa procesada | Supervisor / Sistema externo | El seguimiento se interrumpe y el despacho queda fuera de operación activa en Y-Trace. |

No existe el estado `CON_INCIDENCIA`.

---

# 13. Responsabilidades de cada rol

## Administrador Principal

Maneja la plataforma desde el punto de vista de seguridad y gobernanza.

**Puede:**

- administrar cuentas Web;
- asignar y retirar roles;
- controlar accesos;
- consultar bitácoras;
- gestionar aspectos de seguridad.

**No puede:**

- crear despachos;
- asignar rutas logísticas;
- sustituir al Supervisor en las operaciones diarias.

## Supervisor de Distribución

Es el responsable operativo de la Torre de Control.

**Puede:**

- consultar despachos disponibles;
- revisar su elegibilidad;
- generar códigos de activación;
- monitorear operaciones;
- revisar trazabilidad;
- ejecutar Cancelación Forzada del Seguimiento cuando corresponda;
- supervisar llegada y cierre.

**No gestiona incidencias dentro de Y-Trace.**

## Jefe de Distribución

Es el responsable táctico y estratégico del rendimiento.

**Puede:**

- analizar KPIs;
- revisar Lead Time;
- analizar tiempos entre hitos;
- analizar entregas y no entregas;
- revisar latencia de integración;
- comparar transportistas;
- consultar información consolidada;
- exportar reportes.

## Operador SAC / Soporte Logístico

Es el usuario orientado a consultas.

**Puede:**

- buscar despachos por código o placa;
- consultar la línea de tiempo;
- consultar el resumen del seguimiento;
- revisar posiciones y estados;
- responder consultas operativas con base en la trazabilidad registrada.

## Socio Logístico / Conductor

Es el responsable de ejecutar físicamente el traslado y alimentar la operación móvil.

**Puede:**

- activar el despacho mediante código;
- consultar información del viaje;
- iniciar el despacho;
- generar telemetría GPS;
- operar temporalmente sin conectividad;
- reconectarse automáticamente mediante la sesión operativa vigente;
- registrar la entrega o no entrega;
- finalizar el seguimiento.

**No puede:**

- crear despachos;
- modificar datos maestros;
- reprogramar la logística;
- reutilizar un código activo;
- alterar artificialmente las coordenadas registradas.

## Punto de Destino / Agencia Receptora

Es el receptor físico de la carga.

**Participa en:**

- recepción de los bultos;
- inspección física;
- conformidad o rechazo;
- recepción documental según el proceso físico correspondiente.

---

# 14. Casos de Uso del Negocio de Y-Trace

El modelo del negocio puede agruparse en cinco macroprocesos:

## CUN-01 — Habilitación y Salida del Despacho

Controla la consulta del despacho disponible, la habilitación del seguimiento y la activación del conductor para iniciar la operación.

## CUN-02 — Traslado y Monitoreo de la Carga

Representa el traslado físico nacional y el seguimiento de la unidad mediante telemetría GPS.

## CUN-03 — Llegada y Entrega en Punto de Destino

Representa la llegada al radio del destino, el cambio a `EN_DESTINO`, la confirmación de entrega o la no entrega y el cierre.

## CUN-04 — Control Excepcional del Seguimiento

Representa la intervención del Supervisor para cancelar forzosamente el seguimiento después de una comunicación externa que requiere interrumpir la operación.

## CUN-05 — Consulta y Análisis de Trazabilidad

Representa la explotación posterior de los datos para consulta histórica, resumen del recorrido, tiempos, estados e indicadores.

> Los CUN se mantienen como nivel de negocio y no deben convertirse directamente en botones o pantallas. Los requerimientos funcionales describen las capacidades de software que soportan estos procesos.

---

# 15. Arquitectura funcional simplificada

```text
                           ECOSISTEMA CORPORATIVO YANBAL
          ┌────────────────────────────────────────────────────────┐
          │ SPY / Driving / SAP / Salesforce / Bus de Integración │
          └──────────────────────────┬─────────────────────────────┘
                                     │
                                     ▼
                         ┌────────────────────────────┐
                         │       BACKEND Y-TRACE      │
                         │                            │
                         │ - Seguridad / RBAC         │
                         │ - Despachos / estados      │
                         │ - Códigos de activación    │
                         │ - Sesiones móviles         │
                         │ - Telemetría GPS           │
                         │ - Trazabilidad             │
                         │ - Sincronización           │
                         │ - Integración ESB          │
                         │ - Auditoría               │
                         └────────────┬───────────────┘
                                      │
                    ┌─────────────────┴─────────────────┐
                    │                                   │
                    ▼                                   ▼
          ┌──────────────────────┐          ┌──────────────────────────┐
          │      WEB Y-TRACE     │          │ APP NATIVA ANDROID       │
          │   Torre de Control   │          │       CONDUCTOR           │
          │                      │          │                          │
          │ - Administración     │          │ - Activación             │
          │ - Despachos          │          │ - Datos del despacho     │
          │ - Habilitación       │          │ - Inicio                 │
          │ - Monitoreo          │          │ - GPS                    │
          │ - Cancelación        │          │ - Offline / Sync         │
          │ - Trazabilidad       │          │ - Llegada por geocerca   │
          │ - Auditoría          │          │ - Entrega / No Entrega   │
          │ - Dashboard          │          │ - Finalización            │
          │ - Integraciones      │          │ - Sesión persistente      │
          └──────────────────────┘          └──────────────────────────┘
```

---

# 16. Principios de funcionamiento importantes

## 16.1 El Código de Activación no es una contraseña

El conductor no recibe una cuenta permanente. Recibe un código asociado a una operación concreta.

## 16.2 El Código no crea un despacho

El despacho existe previamente en el ecosistema corporativo. Y-Trace solamente habilita su seguimiento.

## 16.3 El código no se reutiliza mientras exista una sesión activa

Una vez activado correctamente el despacho, el código deja de estar disponible para una nueva activación del mismo viaje mientras la sesión operativa continúe vigente.

## 16.4 La sesión operativa sobrevive a la pérdida de Internet

La pérdida de conectividad no debe destruir la relación:

`Dispositivo → Sesión → Despacho`

La App conserva localmente la información necesaria para mantener el contexto y reconectarse cuando exista señal nuevamente.

## 16.5 Llegada no significa entrega

`EN_DESTINO` significa que el GPS detectó la entrada al radio configurado del destino.

`ENTREGADO` requiere una confirmación consciente posterior.

## 16.6 El tracking funciona mientras el despacho esté activo

La telemetría GPS permanece activa durante `EN_RUTA` y continúa hasta que el seguimiento se finaliza o es cancelado.

## 16.7 La cancelación forzada corta el seguimiento

La cancelación forzada es una función excepcional de control. Revoca la sesión, invalida el código, bloquea nuevos envíos y detiene el tracking.

## 16.8 Sin cobertura no significa pérdida del evento

Los eventos y coordenadas pendientes permanecen localmente y son sincronizados cuando la conectividad se restablece.

## 16.9 La trazabilidad debe poder reconstruirse

Cada operación debe conservar sus principales eventos, posiciones, estados, fechas, horas y tiempos para reconstruir posteriormente el recorrido.

## 16.10 Y-Trace no gestiona incidencias ni archivos multimedia

Las incidencias se comunican fuera de Y-Trace y las fotografías, imágenes y POD no forman parte del sistema.

---

# 17. Resumen de la experiencia de uso

## Para el Supervisor de Distribución

El Supervisor entra a la Web, consulta los despachos disponibles para seguimiento, selecciona el despacho correspondiente y habilita la operación generando el Código de Activación. Posteriormente monitorea el viaje y consulta su trazabilidad. Si recibe una comunicación externa que requiere interrumpir el seguimiento, ejecuta una Cancelación Forzada.

## Para el Conductor

El conductor abre la App Nativa Android, introduce el código recibido y establece la sesión operativa. Consulta el despacho y presiona **“Iniciar Despacho”**. Durante el traslado, la aplicación captura GPS automáticamente y conserva los eventos si no existe conectividad.

Cuando el GPS entra en el radio del destino, el sistema registra `EN_DESTINO`. Después de completar la recepción física, el conductor confirma `ENTREGADO` o registra `NO_ENTREGADO`. Finalmente, cierra el seguimiento y la aplicación revoca la sesión.

## Para el Punto de Destino / Agencia

El receptor recibe físicamente la carga y determina la conformidad o rechazo de la recepción.

## Para el Operador SAC

El Operador busca el despacho por código o placa, consulta la línea de tiempo y revisa el resumen del seguimiento para conocer el recorrido, estados y tiempos sin depender de información manual del conductor.

## Para el Jefe de Distribución

El Jefe consulta el Dashboard, analiza tiempos, entregas, cancelaciones, latencia y desempeño de transportistas, y obtiene reportes consolidados.

## Para el Administrador Principal

El Administrador mantiene la plataforma controlada desde el punto de vista de cuentas, roles, seguridad y auditoría, sin asumir la gestión logística del despacho.

---

# 18. Resultado esperado de la solución

La solución Y-Trace debe permitir que, para cada despacho bajo su alcance, exista una trazabilidad digital completa:

```text
DESPACHO EXISTENTE
       ↓
DISPONIBLE PARA SEGUIMIENTO
       ↓
HABILITACIÓN POR SUPERVISOR
       ↓
CÓDIGO DE ACTIVACIÓN
       ↓
ACTIVACIÓN APP NATIVA ANDROID
       ↓
SESIÓN OPERATIVA PERSISTENTE
       ↓
EN_RUTA
       ↓
GPS + EVENTOS + ESTADOS
       ↓
EN_DESTINO
       ↓
ENTREGADO / NO_ENTREGADO
       ↓
FINALIZADO
       ↓
RESUMEN DE TRAZABILIDAD
       ↓
INDICADORES / AUDITORÍA / INTEGRACIÓN
```

Ante una cancelación forzada:

```text
EN_RUTA
       ↓
COMUNICACIÓN EXTERNA
       ↓
CANCELACIÓN FORZADA EN WEB
       ↓
DESPACHO_CANCELADO
       ↓
SESIÓN REVOCADA
       ↓
CÓDIGO INVALIDADO
       ↓
TRACKING DETENIDO
       ↓
SALIDA DE LA APP
       ↓
INTEGRACIÓN DEL EVENTO
```

De esta manera, Web y App Nativa no funcionan como dos aplicaciones independientes, sino como dos puntos de interacción de una misma solución: **la Web controla y supervisa la operación; la App Nativa ejecuta y registra la operación desde el vehículo; el backend centraliza, protege, persiste e integra la información de seguimiento.**

---

# 19. Límites y decisiones que deben mantenerse consistentes

Para evitar contradicciones durante el diseño, implementación y modelado posterior, deben conservarse estas decisiones:

1. **Y-Trace es B2B punto a punto.**
2. **El despacho existe antes de entrar a Y-Trace.**
3. **El Supervisor habilita el seguimiento; no crea la operación logística.**
4. **La aplicación móvil es nativa Android; no es una PWA.**
5. **La App no utiliza contraseñas permanentes.**
6. **El Código de Activación se utiliza para establecer la operación y no puede reutilizarse mientras exista una sesión activa.**
7. **La sesión operativa permanece vinculada al despacho aunque se pierda Internet.**
8. **La reconexión no exige volver a introducir el código mientras la sesión siga vigente.**
9. **La llegada a `EN_DESTINO` se determina mediante geocerca/radio del punto de destino.**
10. **`EN_DESTINO` no equivale a `ENTREGADO`.**
11. **El conductor confirma manualmente el resultado de entrega.**
12. **El GPS se detiene cuando el seguimiento termina o es cancelado.**
13. **La Cancelación Forzada se ejecuta desde la Web ante una comunicación externa.**
14. **Y-Trace no gestiona incidencias.**
15. **Y-Trace no captura ni almacena fotografías, imágenes ni POD.**
16. **La trazabilidad se basa en datos: GPS, eventos, estados, fechas, horas y tiempos.**
17. **La pérdida de conectividad no debe destruir los eventos pendientes.**
18. **Los cancelados deben tratarse separadamente en los indicadores que correspondan.**
19. **Los reportes analíticos se exportan en Excel.**
20. **Los roles Web deben mantenerse segregados.**
21. **Las integraciones con los sistemas corporativos respetan la frontera funcional de cada sistema.**

---

# 20. Referencia respecto a los requerimientos

Esta descripción sirve como documento raíz de la solución y como referencia funcional antes de desarrollar los diagramas, casos de uso, arquitectura, diseño de base de datos e implementación.

La numeración definitiva de RF y RNF debe mantenerse sincronizada con la matriz maestra y con los catálogos de requerimientos. Este documento no debe introducir identificadores que no correspondan a esa versión oficial.

La solución se centra en:

- seguridad Web;
- habilitación de seguimiento;
- activación móvil;
- sesión operativa persistente;
- telemetría GPS;
- funcionamiento Offline-First;
- llegada por geocerca;
- entrega/no entrega;
- finalización;
- cancelación forzada;
- consulta y resumen de trazabilidad;
- indicadores;
- auditoría; e
- integración con el Bus corporativo.

Quedan fuera del alcance:

- gestión de incidencias;
- reporte de incidencias desde la App;
- alertas de incidencias;
- fotografías;
- imágenes;
- POD multimedia; y
- almacenamiento de archivos multimedia.

---

# 21. Resumen ejecutivo en una sola frase

**Y-Trace es la solución Web y móvil nativa Android de Yanbal Perú que permite controlar, registrar y consultar de extremo a extremo el seguimiento de un despacho B2B ya existente, desde su habilitación y salida en el CD Lurín, durante su recorrido mediante GPS y cambios de estado, hasta su llegada, entrega, finalización o cancelación forzada, manteniendo la trazabilidad aun ante pérdidas temporales de conectividad e integrando los eventos correspondientes con el ecosistema corporativo.**
