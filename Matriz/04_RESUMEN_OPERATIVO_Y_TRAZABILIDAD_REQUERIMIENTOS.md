# Resumen Operativo de la Plataforma Web, Aplicación Móvil y Trazabilidad con Requerimientos: Y-Trace

> **Carpeta:** `detalles`  
> **Documento:** `04_RESUMEN_OPERATIVO_Y_TRAZABILIDAD_REQUERIMIENTOS.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Propósito:** Explicación técnica y funcional exhaustiva sobre cómo funciona la plataforma Web, cómo opera la aplicación móvil App Nativa en Android del conductor, qué roles intervienen en cada punto, cuál es el flujo operativo integral extremo a extremo (*end-to-end*) bajo el alcance de **despachos de distribución completa entre puntos de distribución (B2B)**, y su correspondencia y comparación formal con los **28 Requerimientos Funcionales (RF001 a RF028)** y **22 Requerimientos No Funcionales (RNF001 a RNF022)** del sistema (**50 requerimientos consolidados en total**).  
>  
> 🔗 **Documentos Relacionados del Proyecto:**  
> - [[01_MATRIZ_DE_REQUERIMIENTOS]] — Matriz maestra unificada (50 requerimientos consolidados)  
> - [[02_REQUERIMIENTOS_FUNCIONALES]] — Especificación técnica detallada de los 28 RF activos (RF001 a RF028)  
> - [[03_REQUERIMIENTOS_NO_FUNCIONALES]] — Especificación técnica detallada de los 22 RNF activos (RNF001 a RNF022)  
> - [[Acceso_Y_Seguridad]] — Directriz rectora de acceso dual y gobernanza de credenciales  
> - [[MAPA_MAESTRO_DEL_PROYECTO]] — Nodo central del grafo de conocimiento  

---

## 1. Cómo Funcionará la Plataforma Web de Y-Trace

### 1.1 Naturaleza y Propósito Arquitectónico
La plataforma Web de **Y-Trace** es una aplicación enriquecida Single Page Application (SPA construida en React / TypeScript o JavaScript moderno) diseñada para operar en estaciones de trabajo y navegadores corporativos de escritorio (Google Chrome v90+) dentro de la red institucional de Yanbal.

Actúa como la **Torre de Control Logístico y Módulo Central de Administración** del sistema, centralizando el monitoreo y seguimiento operativo de despachos basado en la última telemetría GPS disponible de los vehículos en carretera mediante una grilla operativa con semaforización visual limpia, la consulta de despachos ya existentes y disponibles para seguimiento desde el Centro de Distribución Lurín hacia agencias y puntos de distribución, la generación de códigos de activación efímeros para habilitar el seguimiento del conductor en andén, la ejecución de cancelaciones forzadas ante contingencias externas y la atención ágil de consultas de trazabilidad de viajes.

Y-Trace se concentra exclusivamente en el seguimiento y trazabilidad de despachos ya existentes. **No crea, programa, asigna, prepara, libera ni cancela logísticamente los despachos; esas actividades corresponden a sistemas y procesos externos (SPY, Driving, SAP).**

### 1.2 Mecanismo de Acceso y Seguridad Perimetral
A diferencia de la aplicación del conductor, la plataforma Web **requiere obligatoriamente autenticación tradicional basada en credenciales** (`Usuario + Contraseña`), respaldada por:
* **Canales Cifrados:** Comunicación exclusiva bajo protocolo HTTPS sobre TLS 1.3 ([[RNF003]]).
* **Protección de Credenciales Web:** Autenticación web mediante usuario y contraseña, con contraseñas almacenadas usando BCrypt con factor de costo >= 12 y comunicación protegida mediante TLS 1.3 ([[RNF003]]). Decisión arquitectónica de autenticación local con Spring Security sin integración con Azure AD / Entra ID.
* **Control de Acceso Basado en Roles (RBAC):** Restricción estricta de menús, vistas, APIs y operaciones según el rol institucional asignado ([[RF004]], [[RNF002]]).
* **Gobernanza de Sesiones:** Emisión de tokens de acceso firmados (JWT), expiración forzada por inactividad a los **15 minutos** (propuesta técnica) y capacidad de revocación remota (*kill session*) ante sospecha de vulneración ([[RF005]], [[RNF004]]).
* **Bitácora Inmutable de Auditoría Administrativa:** Registro no modificable (*append-only* en BD) de todos los inicios de sesión, intentos fallidos, cambios de roles y cancelaciones forzadas ([[RF006]], [[RNF022]]).

### 1.3 Roles que Intervienen en la Plataforma Web

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                           ROLES DE LA PLATAFORMA WEB                             │
├──────────────────────┬──────────────────────┬──────────────────┬─────────────────┤
│ Administrador        │ Supervisor de        │ Jefe de          │ Operador SAC /  │
│ Principal            │ Distribución         │ Distribución     │ Monitoreo       │
│ (Gobernanza y        │ (Torre de Control y  │ (Control de KPIs │ (Trazabilidad y │
│  Seguridad RBAC)     │  Operación Diaria)   │  y Auditoría)    │  Soporte)       │
└──────────────────────┴──────────────────────┴──────────────────┴─────────────────┘
```

#### 1. Administrador Principal del Sistema
* **Responsabilidad:** Gobernanza absoluta de la seguridad informática, cuentas y perfiles de usuarios de la plataforma Y-Trace.
* **Funciones Clave:**
  - Alta, edición, suspensión y desactivación de cuentas web autorizadas ([[RF002]]).
  - Asignación y revocación de roles RBAC bajo el principio de mínimo privilegio ([[RF003]]).
  - Consulta y exportación de la bitácora inmutable de auditoría web ([[RF006]]).
  - Supervisión y control de intentos de activación móvil bloqueados tras 5 fallos consecutivos ([[RF027]]).
* **Segregación de Funciones:** No tiene privilegios para crear despachos ni asignar vehículos a rutas (evita colusión y fraude).

#### 2. Supervisor de Distribución
* **Responsabilidad:** Control de la operación diaria de traslado de carga desde el Centro de Distribución (CD Lurín).
* **Funciones Clave:**
  - **Consulta de Despachos Disponibles para Seguimiento:** Visualiza los despachos completos que ya existen y fueron preparados por SPY/WMS, puestos a disposición en andén para iniciar su seguimiento ([[RF007]]).
  - **Generación de Códigos Únicos de Activación:** Genera un Código de Activación efímero de 8 caracteres alfanuméricos asociado al despacho disponible y lo entrega al conductor en andén ([[RF008]]).
  - **Monitoreo en Grilla Operativa en Vivo (Torre de Control):** Visualiza la flota y el avance de los viajes en una grilla estructurada con semáforo limpio según la última telemetría GPS recibida ([[RF022]]), desplegando: código de despacho, placa del vehículo, conductor, destino, estado con semáforo (Verde: `EN_RUTA`, Amarillo: `EN_DESTINO`, Gris: `FINALIZADO`/`DESPACHO_CANCELADO`), última coordenada GPS, hora de última actualización y antigüedad de telemetría:
    - 🟢 *Verde:* Vehículo en ruta de traslado emitiendo telemetría normal (`EN_RUTA`).
    - 🟡 *Amarillo:* Vehículo arribado al perímetro de la agencia o punto receptor (`EN_DESTINO`).
    - ⚪ *Gris:* Vehículo con seguimiento culminado (`FINALIZADO`) o cancelado forzadamente (`DESPACHO_CANCELADO`).
  - **Cancelación Forzada del Seguimiento del Despacho:** Ante contingencias viales externas insalvables comunicadas por telefonía u otros canales fuera de Y-Trace, ejecuta la cancelación forzada del seguimiento desde la Web, registrando motivo formal en bitácora inmutable, deteniendo de inmediato el GPS del móvil, revocando la sesión móvil, invalidando el código y generando el evento `DESPACHO_CANCELADO` cuya publicación al Bus corresponde a [[RF025]] ([[RF009]]).

#### 3. Jefe de Distribución / Gerencia de Operaciones
* **Responsabilidad:** Gestión estratégica del nivel de servicio de transporte, cumplimiento de contratos de transportistas y evaluación de indicadores logísticos.
* **Funciones Clave:**
  - **Tablero de Indicadores Logísticos (Dashboard Ejecutivo):** Visualización de métricas consolidadas de tiempos de traslado (*Lead Time*), tasa de entregas conformes y latencia ([[RF024]]), con exclusión estricta de despachos cancelados en Lead Time y contabilizados en una métrica separada de cancelación, y exportación de reportes exclusivamente en formato Excel (.xlsx).
  - **Monitoreo de SLA de Integración:** Control del tiempo transcurrido desde la recepción del evento en backend hasta su aceptación por el Bus corporativo (SLA único: máximo 30 minutos, [[RNF006]]).
  - **Auditoría de Desempeño por Transportista:** Análisis comparativo del desempeño por empresa de transporte contratada como dimensión analítica de filtrado y reporte en el Dashboard ([[RF024]]).

#### 4. Operador SAC / Soporte Logístico
* **Responsabilidad:** Atención y resolución de consultas operativas sobre el estado y ubicación de las cargas en tránsito.
* **Funciones Clave:**
  - **Consulta de Trazabilidad y Resumen del Despacho:** Consulta instantánea por código alfanumérico de despacho o placa vehicular ([[RF023]]).
  - **Línea de Tiempo Integral:** Visualización del historial cronológico completo en menos de 2.0 segundos (disponibilidad, activación, salida del CD, puntos GPS de paso en ruta, llegada a destino, resultado de entrega o cancelación forzada).
  - Resuelve consultas operativas y valida recepciones de inmediato sin depender de fotos pesadas ni llamadas redundantes a conductores.

---

## 2. Cómo Funcionará la Aplicación Móvil (App Nativa Android del Conductor)

### 2.1 Naturaleza y Portabilidad Tecnológica: Exclusividad Android Nativa
La aplicación móvil de **Y-Trace** está implementada como una **Aplicación Nativa Android (App Nativa)** orientada exclusivamente a dispositivos móviles con sistema operativo Android (versión 8.0 Oreo o superior, [[RNF016]]).

* **Despliegue Controlado (APK):** La App Nativa se instala directamente en el dispositivo Android del transportista, garantizando los permisos necesarios para ejecución en segundo plano (Foreground Services) sin depender de navegadores web.
* **Compatibilidad Exclusiva Android:** Diseñada y optimizada estrictamente para Android 8.0+ ([[RNF016]]). La captura de ubicación GPS se realiza mediante los servicios nativos de geolocalización de Android. **No se contempla compatibilidad con iOS ni modalidad PWA en el alcance del proyecto.**
* **Cero Costo en Hardware Propietario:** Aprovecha los smartphones estándar Android de los transportistas terceros contratados por Yanbal.

### 2.2 Mecanismo de Acceso y Seguridad Operativa: CERO Contraseñas
La aplicación móvil **prohíbe el uso de cuentas de usuario permanentes y contraseñas tradicionales**. Su acceso se gobierna mediante:
1. **Código Único de Activación:** Al momento de la estiba en el CD Lurín, el conductor recibe un código alfanumérico efímero de **8 caracteres** (ej. `TRC-82F4`), generado al habilitar el seguimiento ([[RF008]]).
2. **Activación de Jornada:** El conductor abre la App Nativa e ingresa dicho código ([[RF010]]). El backend valida que el despacho exista, esté habilitado y que el código no haya sido consumido.
3. **Control Anti-Fuerza Bruta:** El sistema limita los intentos fallidos a un máximo de 5; al registrarse el quinto fallo consecutivo, bloquea e invalida el código de forma irrevocable ([[RF027]]).
4. **Sesión Operativa Persistente:** El backend emite un token de sesión operativa vinculado al viaje ([[RF011]]). La sesión sobrevive a pérdidas de cobertura celular o reinicios de la app, reconectándose automáticamente sin exigir al conductor volver a digitar el código.
5. **Revocación Inmediata al Cierre:** Al pasar a `FINALIZADO` ([[RF018]]) o `DESPACHO_CANCELADO` ([[RF009]]), el token es revocado inmediatamente, la App Nativa purga los datos temporales del viaje de SQLite (Room) y regresa a su pantalla inicial de activación por código.

---

## 3. Flujo Operativo Extremo a Extremo (End-to-End)

```
+───────────────────────────────────────────────────────────────────────────────────────────────────+
│                                 FLUJO INTEGRADO EXTREMO A EXTREMO                                 │
+───────────────────┬───────────────────┬───────────────────┬───────────────────┬───────────────────+
│ 1. Preparación    │ 2. Consulta y     │ 3. Activación     │ 4. Traslado en    │ 5. Llegada por    │
│    externa en CD  │    Habilitación   │    Móvil Android  │    Ruta (GPS      │    Geocerca y     │
│   (WMS SPY)       │    (Portal Web)   │   (App Nativa)    │    cada 10 min)   │    Resolución     │
+───────────────────┴───────────────────┴───────────────────┴───────────────────┴───────────────────+
                                                                          │
                                                                          ▼
                                        +───────────────────┬───────────────────┬───────────────────+
                                        │ 6. Publicación    │ 7. Cierre Formal  │ 8. Consulta de    │
                                        │    Bus ESB        │    y Revocación   │    Trazabilidad   │
                                        │ (SLA <= 30 min)   │    de Sesión      │    y Dashboard    │
                                        +───────────────────┴───────────────────┴───────────────────+
```

### Etapa 1: Preparación Externa en Centro de Distribución
* El Centro de Distribución de Yanbal en Lurín consolida y prepara la carga B2B en el sistema corporativo **SPY**.
* El TMS corporativo (**Driving**) define el punto de destino de distribución.
* La carga preparada queda disponible en andén de salida y es consultada por Y-Trace ([[RF007]]). Y-Trace no crea ni modifica el despacho.

### Etapa 2: Consulta de Despachos Disponibles y Habilitación en Portal Web
* El **Supervisor de Distribución** accede a la plataforma Web con sus credenciales corporativas (RBAC, [[RF001]]).
* En la vista de andén, consulta los despachos completos disponibles para seguimiento ([[RF007]]).
* Selecciona el despacho y solicita la **Generación del Código Único de Activación** ([[RF008]]). El sistema registra el hito interno de control previo, emite la cadena criptosegura de 8 caracteres alfanuméricos y transiciona el despacho a `HABILITADO`.

### Etapa 3: Activación Móvil en App Nativa Android
* El Supervisor comunica el código al conductor en andén.
* El **Conductor** abre la App Nativa en su smartphone Android e ingresa el código ([[RF010]]).
* El sistema valida el código y establece la **sesión operativa persistente** ([[RF011]]), descargando a la base de datos local SQLite (Room) la información del despacho y punto de destino ([[RF012]], [[RF019]]).

### Etapa 4: Traslado en Ruta y Supervisión en Torre de Control
* El conductor pulsa *"Iniciar Despacho"*, registrando formalmente el hito de salida del CD Lurín con fecha, hora y coordenadas GPS iniciales atómicas, transicionando el estado a `EN_RUTA` ([[RF013]]).
* La App Nativa arranca la telemetría GPS periódica en segundo plano cada 10 minutos exclusivamente durante `EN_RUTA` ([[RF014]]).
* El evento `EN_RUTA` se encola localmente en SQLite (Room) ([[RF019]]) y se transmite al backend vía FIFO ([[RF020]]), siendo publicado al Bus corporativo mediante el servicio de integración ([[RF025]]) dentro del SLA de 30 minutos ([[RNF006]]).
* En la plataforma Web, el **Supervisor** visualiza el despacho en verde (`EN_RUTA`) en la grilla operativa de la Torre de Control ([[RF022]]), monitoreando placa, conductor, destino, última posición GPS, estampa temporal y antigüedad del último reporte.

### Etapa 5: Llegada al Destino y Resolución de Entrega
* **Llegada por Geocerca (`EN_DESTINO`):** Al ingresar el GPS al radio configurado del destino, el despacho pasa automáticamente a `EN_DESTINO` ([[RF015]]), capturando fecha, hora y coordenadas GPS atómicas. **Este evento NO confirma la entrega.**
* **Confirmación Manual de Entrega (`ENTREGADO`):** El conductor presiona conscientemente *"Confirmar Entrega Conforme"* ([[RF016]]), registrando de forma atómica fecha, hora y ubicación GPS. **Sin fotografías ni comprobantes multimedia (POD).**
* **Registro de No Entrega (`NO_ENTREGADO`):** Si el local está cerrado o se rechaza la carga, el conductor selecciona la causal tipificada correspondiente del catálogo cerrado ([[RF017]]), registrando coordenadas GPS y estampa de tiempo atómica. **Sin fotografías.**

### Etapa 6: Publicación de Eventos al Bus Corporativo (SLA $\le$ 30 min)
* Cada evento operativo (`EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`, `DESPACHO_CANCELADO`) se publica de forma asíncrona hacia el **Bus de Integración de Yanbal** en formato JSON canónico estandarizado exclusivamente a través del módulo de integración ([[RF025]], [[RNF017]]).
* El Bus recibe y acepta cada evento elegible dentro del **SLA único de 30 minutos como máximo** desde su recepción en el backend de Y-Trace ([[RNF006]]), alimentando a SAP R/3 y Salesforce y reduciendo el retraso histórico de 2 horas. El Bus es un componente externo de middleware corporativo.

### Etapa 7: Cierre Formal del Despacho y Revocación de Sesión
* Tras resolver la entrega (`ENTREGADO` o `NO_ENTREGADO`) y con la cola local de sincronización vacía ([[RF021]]), el conductor presiona *"Finalizar Despacho"* ([[RF018]]).
* El despacho transiciona a `FINALIZADO`, concluyendo el seguimiento y la trazabilidad operativa en Y-Trace (sin liquidar hojas de traslado ni fletes contables). El backend revoca de inmediato la sesión móvil y caduca el código de activación ([[RF028]]). La App Nativa apaga el GPS y retorna a su pantalla inicial de activación.
* El sistema transmite el resumen consolidado de trazabilidad al Bus corporativo ([[RF026]]).

### Etapa 8: Explotación en Trazabilidad y Control Gerencial
* Ante reclamos o consultas de distribución, operadores SAC o supervisores ingresan el código de despacho o placa en el buscador web de Y-Trace ([[RF023]]), desplegando en menos de 2.0 segundos ([[RNF015]]) la línea de tiempo completa, coordenadas GPS de paradas y resultado final o cancelación forzada.
* La Jefatura de Distribución evalúa el desempeño en el Dashboard ejecutivo ([[RF024]]), analizando Lead Time, puntualidad, entregas conformes y cancelaciones separadas, con filtros por transportista y exportación de reportes exclusivamente en formato Excel (.xlsx).

---

## 4. Matriz de Trazabilidad con Requerimientos Funcionales (RF001 a RF028)

A continuación se detalla la correspondencia formal de los **28 Requerimientos Funcionales oficiales**:

| Módulo / Fase del Flujo | Requerimiento Funcional Oficial | Código | Implementación Técnica y Operativa en Y-Trace |
|---|---|:---:|---|
| **Seguridad Web** | Autenticación de Usuarios en la Plataforma Web | **RF001** | Personal Yanbal inicia sesión en Portal Web con usuario y contraseña corporativa sobre HTTPS/TLS 1.3. |
| **Seguridad Web** | Gestión de Cuentas de Usuarios Web | **RF002** | El Administrador Principal gestiona altas, bajas y suspensiones de cuentas de operadores web. |
| **Seguridad Web** | Asignación y Gestión de Roles de Usuarios Web | **RF003** | El Administrador Principal asocia roles (`Admin`, `Supervisor`, `Jefe`, `SAC`) a cada usuario web. |
| **Seguridad Web** | Control de Acceso Basado en Roles (RBAC) | **RF004** | Los menús y endpoints se restringen según el rol activo del usuario autenticado (HTTP 403 ante infracciones). |
| **Seguridad Web** | Gestión del Ciclo de Vida de la Sesión Web | **RF005** | La sesión web expira automáticamente a los 15 minutos de inactividad o mediante revocación remota. |
| **Seguridad Web** | Registro de Bitácora de Auditoría de Acciones Web | **RF006** | Registro inmutable append-only de transacciones administrativas críticas en la base de datos. |
| **Consulta y Habilitación** | Consulta de Despachos Disponibles para Seguimiento | **RF007** | El Supervisor consulta y filtra despachos completos ya preparados por SPY/WMS externos disponibles para seguimiento. |
| **Códigos y Activación** | Generación de Código Único de Activación para Seguimiento | **RF008** | El backend genera el Código de Activación de 8 caracteres para un despacho disponible y lo asocia al viaje. |
| **Control de Seguimiento** | Cancelación Forzada del Seguimiento del Despacho | **RF009** | El Supervisor cancela forzadamente el seguimiento ante contingencia externa comunicada fuera de la app: transiciona a `DESPACHO_CANCELADO`, detiene GPS móvil, revoca sesión e invalida código (publicación al Bus delegada a RF025). |
| **Operación Móvil Android** | Activación de la Operación Móvil mediante Código Único | **RF010** | El Conductor digita el código de 8 caracteres en la App Nativa Android para vincular el dispositivo al despacho. |
| **Operación Móvil Android** | Establecimiento y Mantenimiento de Sesión Operativa Móvil | **RF011** | Sesión móvil persistente que sobrevive a pérdidas de cobertura celular y reconecta automáticamente sin código. |
| **Operación Móvil Android** | Visualización de Información Operativa del Despacho y Destino | **RF012** | La App Nativa presenta código de despacho, punto de destino de distribución, dirección y observaciones operativas en modo offline, excluyendo catálogo, inventario y datos B2C. |
| **Operación Móvil Android** | Registro de Inicio de Traslado del Despacho (`EN_RUTA`) | **RF013** | El Conductor pulsa "Iniciar Despacho" registrando formalmente el hito de salida con fecha, hora y coordenadas GPS iniciales; cambia a `EN_RUTA` y arranca la telemetría periódica GPS. |
| **Operación Móvil Android** | Captura Periódica de Telemetría GPS durante el Traslado | **RF014** | Muestreo en segundo plano cada 10 minutos exclusivamente durante el estado `EN_RUTA` en Android. |
| **Operación Móvil Android** | Registro de Llegada al Punto de Destino por Geocerca | **RF015** | Entrada al radio perimétrico transiciona automáticamente a `EN_DESTINO` con GPS y hora atómicos; no confirma entrega. |
| **Operación Móvil Android** | Confirmación de Recepción / Entrega del Despacho Completo | **RF016** | Acción manual consciente del conductor que marca `ENTREGADO` con captura atómica de fecha/hora y GPS; sin fotos. |
| **Operación Móvil Android** | Registro de No Entrega o Rechazo de Despacho en Destino | **RF017** | Registro manual con causal tipificada (local cerrado, rechazo) con GPS y hora, marcando `NO_ENTREGADO`; sin fotos. |
| **Operación Móvil Android** | Finalización y Cierre del Seguimiento del Despacho | **RF018** | Concluye el seguimiento y la trazabilidad operativa tras resolver la entrega y vaciar la cola local, pasando a `FINALIZADO` y revocando la sesión móvil (sin liquidar hojas de traslado ni fletes). |
| **Offline y Resiliencia** | Almacenamiento Local Offline en Android (SQLite Room) | **RF019** | Almacena eventos y coordenadas satelitales en SQLite (Room) local ante pérdida de red celular; sin fotos. |
| **Offline y Resiliencia** | Sincronización Automática en Segundo Plano (FIFO) | **RF020** | Transmisión por lotes ordenados cronológicamente al detectar red celular con backend idempotente. |
| **Offline y Resiliencia** | Indicador Visual de Estado de Sincronización Local | **RF021** | Semáforo visual en la App Nativa: verde (sincronizado) o amarillo con contador numérico de eventos en cola. |
| **Monitoreo y Control Web** | Monitoreo Operativo en Torre de Control Web | **RF022** | Torre de Control web con grilla operativa que despliega despacho, vehículo/placa, conductor, destino, estado con semáforo (Verde `EN_RUTA`, Amarillo `EN_DESTINO`, Gris `FINALIZADO`/`CANCELADO`), última posición GPS, estampa temporal y antigüedad de telemetría. |
| **Trazabilidad Web** | Consulta de Trazabilidad y Resumen del Despacho | **RF023** | Consulta unificada por código o placa que muestra cronología completa, coordenadas GPS, duración y resultado final o cancelación en < 2 s. Sin término "evidencias". |
| **Indicadores y KPIs** | Indicadores y Reportes de Gestión (Dashboard) | **RF024** | Tablero ejecutivo con Lead Time, puntualidad, entregas conformes, cancelaciones separadas y análisis comparativo por transportista; exportación en Excel (.xlsx). |
| **Integración ESB** | Publicación de Eventos de Despacho al Bus de Integración | **RF025** | Publicación JSON de `EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO` y `DESPACHO_CANCELADO` en SLA $\le$ 30 min. |
| **Integración ESB** | Envío de Resumen de Trazabilidad del Despacho al Bus | **RF026** | Envío del consolidado final de seguimiento al Bus corporativo tras `FINALIZADO` o `DESPACHO_CANCELADO`. |
| **Seguridad Móvil** | Control de Intentos y Bloqueo Anti-Fuerza Bruta del Código | **RF027** | Bloqueo preventivo autónomo e invalidación irrevocable del código de activación al registrar 5 intentos fallidos consecutivos en la App Nativa Android. |
| **Seguridad Móvil** | Unicidad, Vigencia y Caducidad del Código de Activación | **RF028** | Garantiza unicidad activa y revoca código y sesión tan pronto el despacho pasa a `FINALIZADO` o `DESPACHO_CANCELADO`. |

---

## 5. Matriz de Trazabilidad con Requerimientos No Funcionales (RNF001 a RNF022)

A continuación se detalla la correspondencia formal de las **22 restricciones de calidad de software (ISO/IEC 25010)**, categorizadas según su estado de certeza (*Confirmado*, *Propuesta Técnica* o *Pendiente de Validación*):

| Categoría de Calidad | Requerimiento No Funcional Oficial | Código | Estado de Certeza | Mecanismo Arquitectónico de Cumplimiento | Métrica o Criterio de Aceptación |
|---|---|:---:|:---:|---|---|
| **Seguridad** | Seguridad en el Control de Acceso Dual (Web vs. Móvil) | **RNF001** | *Confirmado* | Autenticación web obligatoria y rechazo estricto de transmisiones móviles sin token de sesión activo. | 100% de peticiones no autorizadas rechazadas con HTTP 401/403. |
| **Seguridad** | Autorización Estricta y Mínimo Privilegio (RBAC) | **RNF002** | *Confirmado* | Filtrado de navegación en frontend reforzado con validación estricta de roles en cada endpoint del backend. | Cero tolerancia a elevación de privilegios no autorizados. |
| **Seguridad** | Protección y Gestión de Credenciales Web | **RNF003** | *Confirmado* | Almacenamiento mediante BCrypt (costo >= 12) y transmisión exclusiva bajo HTTPS/TLS 1.3 (autenticación local sin Azure AD). | 100% de contraseñas con BCrypt; prohibido texto plano o HTTP. |
| **Seguridad** | Gobernanza y Caducidad de Sesiones Web y Móviles | **RNF004** | *Propuesta Técnica* | Expiración web a los 15 min de inactividad; revocación inmediata de sesión móvil al finalizar o cancelar. | Sesión móvil revocada dentro de los 5 s tras cierre o cancelación. |
| **Integridad** | Inmutabilidad e Integridad de Eventos y Telemetría Operativa | **RNF005** | *Confirmado* | Tablas transaccionales bajo esquema append-only estricto (rechazo de UPDATE/DELETE) preservando `captured_at`. Armonizado con RNF018: no se permite borrado manual ni operativo; la única supresión es la purga administrativa al cumplir la política legal de retención. | Cero modificaciones o eliminaciones sobre eventos y coordenadas operativas en ciclo activo. |
| **Rendimiento** | Latencia Máxima de Sincronización hacia el Bus | **RNF006** | *Confirmado* | Cola asíncrona hacia el Bus ESB con publicación y aceptación dentro de un SLA máximo de 30 minutos desde la ingesta. | 100% de eventos elegibles aceptados por el Bus en $\le 30$ min. |
| **Rendimiento** | Tiempo de Respuesta de la Interfaz Móvil App Nativa | **RNF007** | *Propuesta Técnica* | Guardado local optimizado en SQLite (Room) desacoplado de la latencia de red celular; sin fotos pesadas. | Respuesta y confirmación táctil en pantalla móvil en < 500 ms. |
| **Eficiencia** | Consumo Eficiente de Batería del Dispositivo Móvil | **RNF008** | *Propuesta Técnica* | Muestreo periódico de GPS cada 10 minutos únicamente durante el estado `EN_RUTA` mediante Foreground Service. | Consumo acumulado menor al 15% de batería en 8 h en Android. |
| **Eficiencia** | Consumo Optimizado de Datos Móviles Celulares | **RNF009** | *Propuesta Técnica* | Payloads JSON minificados y telemetría comprimida. Erradicación total de tráfico multimedia. | Tráfico celular total menor a 50 MB diarios por transportista. |
| **Disponibilidad** | Disponibilidad Operativa de Plataforma Web y Backend | **RNF010** | *Pendiente de Validación* | Arquitectura cloud de alta disponibilidad con réplicas sin estado y balanceadores de carga. | Disponibilidad >= 99.5% de lunes a sábado de 06:00 a 21:00 h. |
| **Escalabilidad** | Capacidad y Escalabilidad Concurrente | **RNF011** | *Propuesta Técnica* | Procesamiento concurrente de al menos 500 conductores y 50,000 eventos diarios con degradación de latencia < 10%. | Soporte verificado de 500 conductores en paralelo y 50k eventos/día. |
| **Resiliencia** | Capacidad de Persistencia Local Offline en Dispositivo | **RNF012** | *Propuesta Técnica* | Almacenamiento local en SQLite (Room) dimensionado para retener al menos 500 eventos operativos y coordenadas. | Retención íntegra de >= 500 eventos en modo offline sin corrupción. |
| **Confiabilidad** | Idempotencia y Garantía de Entrega en Sincronización | **RNF013** | *Confirmado* | Identificadores únicos UUIDv4 generados en el móvil utilizados para deduplicar reintentos en el backend. | Cero duplicación de transacciones ante reintentos de red inestable. |
| **Usabilidad** | Usabilidad y Ergonomía Operativa para Campo | **RNF014** | *Propuesta Técnica* | Diseño UI móvil con botones táctiles >= 48x48 dp, alto contraste solar y flujos en máximo 3 toques. Sin incidencias. | Completar cualquier registro operativo en <= 3 toques de pantalla. |
| **Rendimiento** | Tiempo de Respuesta en Consulta de Trazabilidad | **RNF015** | *Propuesta Técnica* | Índices B-Tree optimizados en PostgreSQL por `codigo_despacho` y `placa_vehiculo` en la base de datos central. | Despliegue de trazabilidad y resumen en pantalla en < 2.0 s. |
| **Compatibilidad** | Compatibilidad de Plataforma Cliente (App Nativa Android y Consola Web) | **RNF016** | *Confirmado* | App Nativa Android (8.0+) para conductor y navegadores web modernos (Chrome 90+, Edge 90+, Firefox 88+) para consola de control. | 100% de operatividad en Android 8.0+ y navegadores web de escritorio definidos; iOS/PWA excluidos. |
| **Interoperabilidad**| Estandarización de Formatos JSON Canónico | **RNF017** | *Confirmado* | Payloads JSON estructurados conforme al esquema canónico corporativo para interoperabilidad con el Bus. | Validación de estructura JSON Schema en eventos hacia el Bus. |
| **Gobernanza** | Retención y Eliminación de Registros Operativos | **RNF018** | *Pendiente de Validación* | Job programado de purga de coordenadas y eventos al cumplir 24 meses en la base de datos relacional. Armonizado con RNF005: la inmutabilidad rige la vida útil activa; este job cumple el ciclo de vida legal del dato. | 100% de registros > 24 meses purgados en ciclo automático semanal. |
| **Continuidad** | Recuperación ante Desastres (RPO y RTO) | **RNF019** | *Pendiente de Validación* | Replicación continua de base de datos y snapshots multizona para conmutación hacia infraestructura secundaria. | RPO <= 15 minutos; RTO <= 4 horas en simulacro validado. |
| **Calidad del Dato**| Precisión Mínima de Geolocalización en Android | **RNF020** | *Propuesta Técnica* | Validación del radio de precisión satelital (accuracy <= 50 m); lecturas mayores marcadas como de baja confiabilidad. | 90% con accuracy <= 50 m; lecturas imprecisas señalizadas sin descarte. |
| **Accesibilidad** | Accesibilidad Web para Consola Administrativa | **RNF021** | *Pendiente de Validación* | Contraste >= 4.5:1, navegación por teclado y semántica HTML5 accesible bajo WCAG 2.1 AA. Sin módulo de incidencias. | Puntaje Lighthouse/axe >= 90 en consola web corporativa. |
| **Integridad** | Inmutabilidad de Bitácora de Auditoría Administrativa | **RNF022** | *Confirmado* | Modelo append-only estricto en tabla `audit_logs` (RF006), rechazando UPDATE y DELETE a nivel de motor relacional. | 100% de intentos de modificación o borrado bloqueados por motor de BD. |

---

## 6. Conclusión y Síntesis de la Comparación

La sincronización de la arquitectura y la documentación demuestra una **alineación rigurosa, no redundante y libre de contradicciones** entre el modelo operativo de Y-Trace y sus requerimientos congelados (**50 requerimientos consolidados: 28 RF y 22 RNF**):

1. **La Plataforma Web** cumple con la totalidad de los requerimientos de gobernanza y seguridad (**RF001 - RF006**), consulta de despachos existentes en origen (**RF007**), generación de códigos únicos efímeros (**RF008**), cancelación forzada del seguimiento ante contingencias externas (**RF009**), monitoreo en grilla operativa en vivo Torre de Control (**RF022**), consulta unificada de trazabilidad y resumen sin término "evidencias" (**RF023**) y tablero ejecutivo de indicadores logísticos con comparación por transportista en Excel (**RF024**), asegurando atributos de rendimiento (**RNF015**), seguridad RBAC (**RNF002**), disponibilidad (**RNF010**) e inmutabilidad de bitácora (**RNF022**).
2. **La Aplicación Móvil (App Nativa Android)** viabiliza el traslado seguro en carretera (**RF010 - RF018**) bajo un modelo sin contraseñas basado en códigos efímeros con bloqueo anti-fuerza bruta autónomo (**RF027**), sesión móvil persistente ante pérdidas de cobertura (**RF011**), detección automática de llegada física por radio de geocerca (`EN_DESTINO`, **RF015**), confirmación consciente manual de entrega (`ENTREGADO`, **RF016**) y registro de no entrega con causal tipificada (`NO_ENTREGADO`, **RF017**) basados estrictamente en datos geoespaciales y temporales sin fotografías ni POD, soportado por una arquitectura *Offline-First* con SQLite (Room) y Foreground Service (**RF019 - RF021**), cumpliendo los atributos de ergonomía en cabina (**RNF014**), calidad del dato satelital (**RNF020**), bajo consumo de batería y datos celulares (**RNF008, RNF009**), respuesta ultrarrápida local (**RNF007**), tolerancia a fallos (**RNF012, RNF013**) y compatibilidad exclusiva con Android (**RNF016**).
3. **El Ecosistema Corporativo de Yanbal** recibe oportunamente los eventos de estado e hitos operativos a través del Bus de Integración (**RF025, RF026**), reduciendo de forma contundente el desfase histórico de 2 horas a un **SLA único de integración de 30 minutos como máximo**, garantizando interoperabilidad canónica (**RNF017**) e inmutabilidad estricta de telemetría y eventos (**RNF005**).

---

## 7. Exclusiones Explícitas del Alcance (Frontera del Sistema)

Para asegurar la coherencia arquitectónica y evitar desvíos funcionales, se consolidan formalmente las siguientes exclusiones taxativas:

1. **No reparto domiciliario minorista (B2C):** Y-Trace no atiende distribución capilar a cliente final, hogares particulares ni cobranza contra entrega.
2. **No entrega a consultoras/familiares con DNI residencial:** Queda formalmente excluida toda captura de vínculos de parentesco, DNI residencial de personas naturales y firmas de recepción en domicilios familiares.
3. **No gestión de despachos unitarios — solo despachos de carga completa punto a punto:** La operación cubre exclusivamente traslados de carga completa intercentros (Centro de Distribución Lurín → Punto de Distribución / Agencia receptora). No incluye picking, preparación de pedidos, stock ni bultos unitarios sueltos.
4. **No soporte iOS ni modalidad PWA:** La aplicación móvil para conductores opera exclusivamente sobre dispositivos Android (versión 8.0 Oreo o superior) como App Nativa. El ecosistema iOS / Apple y el formato PWA quedan formalmente fuera del alcance del proyecto.
5. **No gestión de incidencias dentro del aplicativo:** Sin botón "Reportar Incidencia", sin formularios móviles, sin tipificación de siniestros en carretera y sin estado `CON_INCIDENCIA`. Toda contingencia se comunica al 100% fuera del sistema vía telefónica externa.
6. **No captura ni almacenamiento de fotografías, imágenes o POD:** La trazabilidad se sustenta exclusivamente en datos estructurados verificables (coordenadas GPS satelitales, marcas de tiempo inmutables `captured_at`, estados y causales tipificadas).
7. **No gestión logística upstream ni liquidación documental:** Y-Trace no crea, programa, asigna, prepara, libera ni cancela logísticamente los despachos, ni liquida hojas de traslado, viajes o fletes. Recibe despachos existentes desde sistemas externos y concentra su función en seguimiento, telemetría y publicación de eventos hacia el Bus.
8. **No aislamiento multi-tenant por empresa transportista:** Y-Trace es una plataforma de supervisión centralizada interna de Yanbal. No es un sistema multi-inquilino de cara a empresas de transporte externas ni restringe la visibilidad entre empresas transportistas; el análisis por transportista se gestiona como dimensión y filtro analítico en el Dashboard corporativo ([[RF024]]).

