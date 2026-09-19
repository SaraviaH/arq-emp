# Catálogo Oficial de Requerimientos Funcionales: Y-Trace

> **Carpeta:** `detalles`  
> **Documento:** `02_REQUERIMIENTOS_FUNCIONALES.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Propósito:** Especificación exhaustiva, estructurada y crítica de todos los Requerimientos Funcionales (RF) de la solución bajo el alcance exclusivo de distribución completa entre puntos de distribución (B2B), justificando su condición funcional frente a restricciones técnicas, reglas de negocio y sistemas externos.  
> **Estado:** [CONSOLIDADO OFICIAL — ALCANCE B2B PUNTO A PUNTO CONGELADO (28 RF)]  
>  
> 🔗 **Documentos Vinculados:**  
> - Matriz Maestra Unificada: [[01_MATRIZ_DE_REQUERIMIENTOS]]  
> - Requerimientos No Funcionales: [[03_REQUERIMIENTOS_NO_FUNCIONALES]]  
> - Resumen Operativo y Trazabilidad: [[04_RESUMEN_OPERATIVO_Y_TRAZABILIDAD_REQUERIMIENTOS]]  
> - Nodo Central del Proyecto: [[MAPA_MAESTRO_DEL_PROYECTO]]  
> - Marco de Seguridad y Acceso: [[Acceso_Y_Seguridad]]  

---

## 1. Marco Metodológico y Definición del Requerimiento Funcional

En la arquitectura de software de Y-Trace, un **Requerimiento Funcional (RF)** responde estrictamente a la pregunta:

> **"¿Qué debe hacer el sistema Y-Trace?"**

Representa un servicio, comportamiento, flujo de datos o transformación que el sistema proporciona a un actor humano o a un sistema externo para satisfacer una necesidad operativa comprobada en el traslado de carga de Yanbal Perú.

### 1.1 Criterios de Demarcación Rigurosa:
1. **Diferencia frente a un RNF:** Un RF define *la acción o capacidad operativa* (ej. capturar coordenadas GPS, activar un despacho, registrar una entrega), mientras que el RNF define *la restricción o criterio de calidad* bajo el cual debe ejecutarse dicha acción (ej. latencia máxima de 30 minutos al Bus, disponibilidad del 99.5%, consumo de batería menor al 15%).
2. **Diferencia frente a una Regla de Negocio (RN):** Una regla de negocio es una directriz operativa preexistente (ej. *"Un despacho no puede finalizarse sin haber resuelto la entrega en destino"*); el requerimiento funcional es *el mecanismo de software* que implementa y hace cumplir dicha política.
3. **Diferencia frente a una Decisión Técnica o de Arquitectura:** Un RF declara la necesidad operativa sin amarrarse prematuramente a librerías específicas (ej. *"Guardar transacciones localmente ante pérdida de red"* es el RF; el uso concreto de SQLite (Room) en Android es la decisión técnica).
4. **Respeto a la Frontera del Sistema:** Se excluyen taxativamente las responsabilidades de los sistemas corporativos consolidados de Yanbal ([[SAP_R3]], [[SPY]], [[Maya]], [[SAP_Commerce]], [[Driving]], [[NSDG]], [[Salesforce]]).

### 1.2 Delimitación de Alcance Funcional: Distribución Completa Punto a Punto (B2B)
El alcance de Y-Trace modela conceptualmente el siguiente ciclo operativo:
```
Origen (CD Lurín) 
   → Despacho Completo (Carga ya consolidada y preparada en SPY/WMS corporativo)
   → Habilitación y Generación de Código Único (RF007, RF008)
   → Activación Móvil en App Nativa Android (RF010, RF011 - Sesión Persistente)
   → Inicio de Traslado e inicio de telemetría (RF013, RF014 - Estado EN_RUTA)
   → Traslado en Ruta (Muestreo GPS en segundo plano cada 10 min, offline en SQLite)
   → Punto de Destino (Agencia o Centro de Distribución Secundario)
   → Llegada por Geocerca (RF015 - Entrada al radio configurado → Estado EN_DESTINO)
   → Resolución Manual de Entrega (RF016: ENTREGADO / RF017: NO_ENTREGADO con causal tipificada)
   → Trazabilidad Verificable (Datos estructurados GPS, fechas/horas, estados, causales; sin fotos)
   → Cierre de Operación (RF018 - Estado FINALIZADO → Revocación inmediata de sesión móvil)
   → Ruta Alternativa de Contingencia: Cancelación Forzada (RF009 - Estado DESPACHO_CANCELADO)
```
Y-Trace **no** es un sistema de picking, inventario, ventas ni de reparto minorista domiciliario (B2C) de cajas a consultoras. Las unidades de carga ya existen y están preparadas en los sistemas correspondientes; Y-Trace se concentra exclusivamente en su seguimiento, telemetría, trazabilidad y publicación de eventos hacia el Bus corporativo.
Y-Trace tampoco crea, programa, asigna, libera ni cancela logísticamente los despachos. Esas actividades pertenecen a los sistemas y procesos externos; el sistema inicia su participación cuando existe un despacho disponible para seguimiento.

### 1.3 Definición del Hito Interno de Control Previo
El **hito interno de control previo** es un registro interno de Y-Trace que acredita que un despacho ya existente, recibido desde los sistemas externos, ha quedado **habilitado para iniciar su seguimiento en Y-Trace**. Este hito no crea, programa, asigna, prepara, libera ni autoriza logísticamente el despacho para su salida física. Su finalidad es controlar la transición desde la disponibilidad del despacho hacia la activación móvil y conservar trazabilidad interna. El hito se registra cuando el Supervisor solicita la generación del Código de Activación mediante [[RF008]] y el sistema valida la elegibilidad del despacho. Es un registro interno de auditoría y no se publica al Bus de Integración mediante [[RF025]].

### 1.4 Modelo de Trazabilidad y Datos Verificables (Cero Fotos / Cero POD)
En Y-Trace la **trazabilidad operativa se sustenta estrictamente en datos transaccionales estructurados e inmutables**:
1. Identificador unívoco del despacho (`despacho_id`).
2. Evento operativo registrado (`HABILITACION_SEGUIMIENTO`, `SALIDA`, `GPS_TRACKING`, `LLEGADA`, `ENTREGA`, `NO_ENTREGA`, `CIERRE`, `CANCELACION`).
3. Marca de tiempo certificada de captura en dispositivo (`captured_at`).
4. Coordenadas de geolocalización satelital (latitud, longitud y precisión horizontal en metros).
5. Identificador de sesión y actor operativo.
6. Causal tipificada de impedimento en caso de no entrega (`NO_ENTREGADO`) o motivo justificado en cancelación forzada (`DESPACHO_CANCELADO`).

Se excluye taxativamente la captura, procesamiento y almacenamiento de fotografías, imágenes, firmas biométricas y comprobantes de entrega (POD) multimedia. La presencia física se acredita mediante la geocerca de destino (`EN_DESTINO`) y la confirmación consciente manual del conductor (`ENTREGADO` o `NO_ENTREGADO`) con coordenadas GPS atómicas.

---

## 2. Catálogo Detallado de Requerimientos Funcionales (RF001 – RF028)

---

### Grupo 1: Módulo de Seguridad, Autenticación y Administración Web

#### RF001 — Autenticación de Usuarios en la Plataforma Web
* **ID:** `RF001`
* **Caso de Prueba Asociado:** `CP-AUTH-01`
* **Nombre:** Autenticación de Usuarios en la Plataforma Web.
* **Descripción:** El sistema debe permitir a los colaboradores autorizados de Yanbal acceder a la plataforma Web mediante la validación de sus credenciales de usuario y contraseña personales.
* **Objetivo de Negocio:** Proteger la información operacional, los datos de los despachos y la configuración del sistema contra accesos no autorizados.
* **Actor:** Administrador Principal, Supervisor de Distribución, Jefe de Distribución, Operador SAC.
* **Módulo:** Módulo de Seguridad y Acceso Web.
* **Qué debe hacer el sistema:** Solicitar credenciales, validar contra el repositorio seguro de usuarios, verificar estado de la cuenta (activo/inactivo), identificar el rol asignado, cargar permisos y emitir el contexto de sesión autenticada. En caso de credenciales inválidas, debe denegar el acceso y registrar el fallo en la bitácora.
* **Entradas:** Identificador de usuario (correo corporativo), contraseña en texto plano introducida por el usuario.
* **Resultado Esperado:** Usuario autenticado con sesión iniciada y redirección al panel correspondiente a su rol, o mensaje de rechazo de credenciales.
* **Reglas de Negocio Relacionadas:** Separación Dual de Mecanismos de Acceso (Web con credenciales vs. Móvil con código efímero).
* **Dependencias:** Ninguna (punto de entrada a la plataforma Web).
* **Criterios de Validación:** Ingreso exitoso con credenciales válidas; bloqueo tras 5 intentos fallidos consecutivos; rechazo inmediato ante cuentas inactivas.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Usuarios_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RF de autenticación web que protege la Torre de Control corporativa. No aplica a la app móvil del conductor, que opera bajo el modelo de códigos efímeros de activación.

---

#### RF002 — Gestión de Cuentas de Usuarios Web
* **ID:** `RF002`
* **Caso de Prueba Asociado:** `CP-USR-01`
* **Nombre:** Gestión de Cuentas de Usuarios Web.
* **Descripción:** El sistema debe permitir al Administrador Principal crear, consultar, actualizar, suspender y desactivar cuentas de usuarios para el acceso a la plataforma Web.
* **Objetivo de Negocio:** Centralizar la gobernanza de identidades de colaboradores autorizados y asegurar el cese inmediato de accesos ante desvinculaciones.
* **Actor:** Administrador Principal del Sistema.
* **Módulo:** Módulo de Administración de Usuarios.
* **Qué debe hacer el sistema:** Presentar formulario de gestión de usuarios, permitir el ingreso de nombres, apellidos, correo corporativo, área operativa y estado; persistir cambios y registrar cada acción en la bitácora de auditoría.
* **Entradas:** Datos de la cuenta de usuario (`nombre`, `correo`, `estado_activo`, `roles_asignados`).
* **Resultado Esperado:** Cuenta creada o modificada en el repositorio central de usuarios, con registro inmutable en bitácora.
* **Reglas de Negocio Relacionadas:** Gobernanza de Cuentas y Mínimo Privilegio.
* **Dependencias:** [[RF001]].
* **Criterios de Validación:** Solo ejecutable por el Administrador Principal; validación de formato de correo corporativo institucional; bloqueo de duplicidad de cuentas.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Funciones_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Impide la creación descentralizada de accesos y garantiza que las identidades web estén estrictamente auditadas.

---

#### RF003 — Asignación y Gestión de Roles de Usuarios Web
* **ID:** `RF003`
* **Caso de Prueba Asociado:** `CP-ROL-01`
* **Nombre:** Asignación y Gestión de Roles de Usuarios Web.
* **Descripción:** El sistema debe permitir al Administrador Principal asignar y modificar los roles de acceso a la plataforma Web entre los perfiles institucionales definidos (`Administrador Principal`, `Supervisor de Distribución`, `Jefe de Distribución`, `Operador SAC`).
* **Objetivo de Negocio:** Garantizar la segregación de funciones dentro de la operación logística de Yanbal.
* **Actor:** Administrador Principal del Sistema.
* **Módulo:** Módulo de Administración de Usuarios.
* **Qué debe hacer el sistema:** Desplegar catálogo de roles oficiales, permitir asociar uno o más roles a una cuenta de usuario web activa y guardar la asignación con registro de auditoría.
* **Entradas:** Identificador de usuario, rol seleccionado.
* **Resultado Esperado:** Rol asignado actualizado y permisos reconfigurados en el siguiente inicio de sesión del usuario.
* **Reglas de Negocio Relacionadas:** Matriz de Segregación de Roles.
* **Dependencias:** [[RF001]], [[RF002]].
* **Criterios de Validación:** Modificación inmediata de permisos; toda reasignación genera un registro obligatorio en la bitácora inmutable ([[RF006]]).
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Usuarios_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Base funcional para hacer cumplir el control de acceso basado en roles (RBAC) a nivel de frontend y backend.

---

#### RF004 — Control de Acceso Basado en Roles (RBAC)
* **ID:** `RF004`
* **Caso de Prueba Asociado:** `CP-RBAC-01`
* **Nombre:** Control de Acceso Basado en Roles (RBAC).
* **Descripción:** El sistema debe restringir la navegación, vistas, menús y ejecución de operaciones en la plataforma Web en función estricta del rol asignado al usuario autenticado.
* **Objetivo de Negocio:** Prevenir que personal operativo acceda a funciones de configuración o que operadores de atención al cliente ejecuten acciones de supervisión o cancelación.
* **Actor:** Sistema Backend / Frontend Web.
* **Módulo:** Módulo de Seguridad y Acceso Web.
* **Qué debe hacer el sistema:** Evaluar el rol en cada renderizado de interfaz (ocultando opciones no autorizadas) y validar obligatoriamente los permisos en cada endpoint del backend, rechazando con código HTTP 403 Forbidden cualquier invocación no autorizada.
* **Entradas:** Token de sesión web del usuario autenticado y recurso/endpoint solicitado.
* **Resultado Esperado:** Acceso concedido si el rol tiene privilegios, o denegación inmediata (HTTP 403) con registro del evento.
* **Reglas de Negocio Relacionadas:** Principio de Mínimo Privilegio.
* **Dependencias:** [[RF001]], [[RF003]].
* **Criterios de Validación:** 100% de endpoints protegidos; imposibilidad de saltar restricciones manipulando la URL del navegador.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Componentes_Aplicacion]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Aplica defensa en profundidad: la restricción visual se complementa con la intercepción estricta a nivel de servicios web.

---

#### RF005 — Gestión del Ciclo de Vida de la Sesión Web
* **ID:** `RF005`
* **Caso de Prueba Asociado:** `CP-SES-01`
* **Nombre:** Gestión del Ciclo de Vida de la Sesión Web.
* **Descripción:** El sistema debe controlar el ciclo de vida de la sesión web autenticada, soportando cierre de sesión voluntario, expiración automática por inactividad tras 15 minutos y revocación forzada remota (*kill session*).
* **Objetivo de Negocio:** Prevenir el uso indebido de sesiones abiertas en estaciones de trabajo desatendidas en el Centro de Distribución u oficinas.
* **Actor:** Usuario autenticado / Administrador Principal / Sistema.
* **Módulo:** Módulo de Seguridad y Acceso Web.
* **Qué debe hacer el sistema:** Emitir token firmado al iniciar sesión, contabilizar tiempo de inactividad del usuario, expirar y destruir la sesión a los 15 minutos sin interacción; permitir al Administrador invalidar tokens activos de cualquier usuario ante sospecha de compromiso.
* **Entradas:** Actividad del usuario (eventos de interfaz) o comando administrativo de revocación forzada.
* **Resultado Esperado:** Redirección automática a la pantalla de login con notificación de cierre de sesión.
* **Reglas de Negocio Relacionadas:** Separación Dual de Mecanismos de Acceso.
* **Dependencias:** [[RF001]].
* **Criterios de Validación:** Toda llamada tras 15 minutos de inactividad debe recibir un HTTP 401; la revocación administrativa invalida el token de inmediato.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Usuarios_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Protege la plataforma web corporativa contra secuestro de sesiones (*session hijacking*).

---

#### RF006 — Registro de Bitácora de Auditoría de Acciones Administrativas Web
* **ID:** `RF006`
* **Caso de Prueba Asociado:** `CP-AUD-01`
* **Nombre:** Registro de Bitácora de Auditoría de Acciones Administrativas Web.
* **Descripción:** El sistema debe registrar en una bitácora inmutable todas las acciones administrativas críticas ejecutadas en la plataforma Web (altas, bajas, modificaciones de cuentas, cambios de roles, bloqueos, cancelaciones forzadas e intentos fallidos de autenticación).
* **Objetivo de Negocio:** Proveer trazabilidad de auditoría informática para cumplimiento de seguridad de la información y revisiones periciales.
* **Actor:** Sistema Backend / Administrador Principal.
* **Módulo:** Módulo de Auditoría y Seguridad.
* **Qué debe hacer el sistema:** Capturar de forma atómica en cada evento administrativo: `usuario_ejecutor_id`, `tipo_evento`, `tabla_afectada`, `valores_anteriores`, `valores_nuevos`, `ip_origen`, `user_agent` y estampa de tiempo UTC; almacenar el registro bajo esquema de solo anexión (*append-only*).
* **Entradas:** Eventos del sistema resultantes de acciones de usuarios web.
* **Resultado Esperado:** Registro consolidado en la tabla de bitácora, accesible en modo solo lectura mediante la consola de auditoría para el Administrador.
* **Reglas de Negocio Relacionadas:** Inmutabilidad de Auditoría.
* **Dependencias:** [[RF001]], [[RF002]], [[RF003]].
* **Criterios de Validación:** Cero posibilidad de modificar o borrar entradas de bitácora; toda acción crítica genera un registro inmediato.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Funciones_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Alimenta la auditoría perimetral web; su inmutabilidad formal está respaldada por el RNF022.

---

### Grupo 2: Módulo de Consulta y Habilitación de Seguimiento en Origen

#### RF007 — Consulta de Despachos Disponibles para Seguimiento
* **ID:** `RF007`
* **Caso de Prueba Asociado:** `CP-DESP-01`
* **Nombre:** Consulta de Despachos Disponibles para Seguimiento.
* **Descripción:** El sistema debe permitir al Supervisor de Distribución consultar y filtrar la lista de despachos completos ya existentes, recibidos desde los sistemas corporativos externos (SPY/WMS vía Bus) y disponibles para iniciar su seguimiento hacia los puntos de destino.
* **Objetivo de Negocio:** Proveer visibilidad inmediata de los despachos completos que ya fueron consolidados y preparados por los sistemas corporativos externos, permitiendo seleccionar cuáles serán habilitados para seguimiento en Y-Trace.
* **Actor:** Supervisor de Distribución.
* **Módulo:** Módulo de Consulta de Despachos Disponibles.
* **Qué debe hacer el sistema:** Consultar los despachos completos puestos a disposición por los sistemas externos, desplegar su información operativa (código de despacho, destino, cantidad de bultos, transportista asignado y horario previsto) y ofrecer filtros por fecha, destino y transportista. **La consulta no crea, asigna, programa, cubicá ni autoriza logísticamente el despacho; Y-Trace no gestiona la carga física.**
* **Entradas:** Parámetros de filtrado (rango de fechas, punto de destino, empresa transportista).
* **Resultado Esperado:** Grilla interactiva de despachos completos existentes y disponibles para habilitar su seguimiento.
* **Reglas de Negocio Relacionadas:** Seguimiento y Trazabilidad de Carga Completa.
* **Dependencias:** Recepción de despachos desde sistemas externos vía Bus corporativo. No depende de RF025 (publicación hacia el Bus).
* **Criterios de Validación:** No se listan despachos ya finalizados ni cancelados; visualización clara de la unidad de despacho B2B sin desglose de pedidos minoristas individuales.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Flujo_Web]] | [[SPY]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Precisión semántica medular: Y-Trace no gestiona cargas en almacén ni crea despachos; consulta despachos que ya existen en el ecosistema corporativo para habilitar su trazabilidad.

---

#### RF008 — Generación de Código Único de Activación para Seguimiento
* **ID:** `RF008`
* **Caso de Prueba Asociado:** `CP-COD-01`
* **Nombre:** Generación de Código Único de Activación para Seguimiento.
* **Descripción:** El sistema debe generar un Código Único de Activación alfanumérico asociado a un despacho ya existente y disponible para seguimiento. El código será comunicado en andén al conductor para que pueda ingresar a la App Nativa Android e iniciar el seguimiento del viaje.
* **Objetivo de Negocio:** Habilitar de manera segura y temporal el acceso del transportista tercero al seguimiento digital del despacho sin recurrir a cuentas corporativas fijas ni contraseñas permanentes.
* **Actor:** Supervisor de Distribución / Sistema Backend.
* **Módulo:** Módulo de Códigos y Activación.
* **Qué debe hacer el sistema:** Recibir la solicitud sobre un despacho disponible, verificar que exista y sea elegible para seguimiento, registrar el hito interno de control previo, generar una cadena pseudoaleatoria criptosegura de 8 caracteres alfanuméricos, verificar unicidad activa, asociarla al despacho, transicionar el estado del despacho a `HABILITADO` y presentar el código en pantalla para su entrega al conductor.
* **Entradas:** Identificador del despacho disponible para seguimiento.
* **Resultado Esperado:** Despacho en estado `HABILITADO` con hito interno de control previo registrado y Código de Activación vigente listo para ser ingresado por el conductor en la App Nativa Android.
* **Reglas de Negocio Relacionadas:** Separación Dual de Mecanismos de Acceso, Unicidad de Código Activo.
* **Dependencias:** [[RF007]].
* **Criterios de Validación:** El código solo puede generarse para un despacho en estado `DISPONIBLE`; el código activo es unívoco; Y-Trace no crea ni modifica los datos de carga del despacho.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Funciones_Web]] | [[Flujo_Principal]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el puente seguro entre el sistema corporativo y la operación móvil de campo, eliminando la fricción de gestionar usuarios para cientos de choferes terceros.

---

#### RF009 — Cancelación Forzada del Seguimiento del Despacho
* **ID:** `RF009`
* **Caso de Prueba Asociado:** `CP-DESP-04`
* **Nombre:** Cancelación Forzada del Seguimiento del Despacho.
* **Descripción:** El sistema debe permitir al Supervisor de Distribución forzar administrativamente la cancelación del seguimiento de un despacho desde la plataforma Web ante contingencias externas insalvables comunicadas fuera del sistema (avería mayor, siniestro vial, bloqueo total de carretera o instrucción operativa externa). El sistema registra el motivo mínimo de cancelación, cambia el estado a `DESPACHO_CANCELADO`, detiene de inmediato el tracking GPS en el móvil, revoca la sesión operativa móvil, invalida el código de activación, desaloja al conductor de la App Nativa hacia la pantalla inicial de activación y genera el evento de estado `DESPACHO_CANCELADO`, cuya publicación al Bus corporativo corresponde exclusivamente a [[RF025]].
* **Objetivo de Negocio:** Proveer un mecanismo de control de seguimiento web que permita cerrar técnicamente y de forma inmediata la emisión de telemetría y el ciclo de vida del viaje cuando la operación física no puede completarse por contingencias externas.
* **Actor:** Supervisor de Distribución.
* **Módulo:** Módulo de Consulta y Habilitación de Seguimiento.
* **Qué debe hacer el sistema:** Presentar modal de cancelación forzada, exigir la selección de motivo tipificado y texto de justificación, solicitar confirmación de seguridad del Supervisor; transicionar el estado del despacho a `DESPACHO_CANCELADO`, registrar la acción en la bitácora inmutable de auditoría ([[RF006]]), ordenar la revocación inmediata del token de sesión móvil, invalidar el código de activación asociado para impedir nuevos envíos de telemetría, expulsar la App Nativa a su pantalla inicial y generar el registro transaccional inmutable del evento `DESPACHO_CANCELADO` disponible para su posterior publicación hacia el Bus de Integración corporativo ([[RF025]]).
* **Entradas:** Identificador de despacho, causal tipificada de cancelación, justificación administrativa y credenciales del supervisor.
* **Resultado Esperado:** Despacho en estado `DESPACHO_CANCELADO`, tracking GPS móvil apagado, token y código invalidados, expulsión de la App Nativa a la pantalla de activación y registro del evento `DESPACHO_CANCELADO` generado.
* **Frontera con el Bus:** Y-Trace ejecuta y registra la cancelación técnica del seguimiento y genera el evento de estado; la publicación asíncrona hacia el Bus corporativo de integración corresponde de forma exclusiva a [[RF025]].
* **Reglas de Negocio Relacionadas:** Condiciones de Cancelación Forzada, Inmutabilidad de Bitácora.
* **Dependencias:** [[RF006]], [[RF008]], [[RF011]].
* **Criterios de Validación:** Requiere perfil de Supervisor o superior; genera entrada crítica obligatoria en bitácora inmutable; rechazo inmediato de cualquier paquete GPS posterior a la cancelación.
* **Fuente o Evidencia:** [[Ciclo_De_Vida_Del_Despacho]] | [[Funciones_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Resuelve limpiamente contingencias viales externas sin requerir módulos de incidencias en el móvil: la contingencia se atiende por canales externos y el Supervisor toma el control del seguimiento desde la Web. La publicación hacia el Bus se delega a RF025 para mantener la responsabilidad única.

---

### Grupo 3: Módulo Móvil de Operación en Ruta, Geolocalización y Entrega (App Nativa Android)

#### RF010 — Activación de la Operación Móvil mediante Código Único
* **ID:** `RF010`
* **Caso de Prueba Asociado:** `CP-MOB-01`
* **Nombre:** Activación de la Operación Móvil mediante Código Único.
* **Descripción:** El sistema debe permitir al conductor activar la operación de seguimiento del despacho en la App Nativa Android mediante el ingreso del Código de Activación alfanumérico emitido en andén.
* **Objetivo de Negocio:** Vincular el dispositivo móvil del transportista con el despacho asignado de forma rápida y sin fricción operativa.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Activación (App Nativa Android).
* **Qué debe hacer el sistema:** Solicitar el código alfanumérico de 8 caracteres, validar su vigencia y estado en el backend; verificar que el código no haya expirado ni alcanzado intentos fallidos; asociar el dispositivo al despacho y emitir el token de sesión operativa móvil.
* **Entradas:** Código de activación de 8 caracteres alfanuméricos ingresado por el conductor.
* **Resultado Esperado:** Código validado, sesión operativa móvil emitida y pantalla de la App Nativa transicionada a la vista de información del despacho ([[RF012]]).
* **Reglas de Negocio Relacionadas:** Acceso Móvil sin Contraseñas, Unicidad de Código Activo.
* **Dependencias:** [[RF008]].
* **Criterios de Validación:** Solo acepta códigos vigentes asociados a despachos habilitados; ante código incorrecto, incrementa el contador de intentos fallidos ([[RF027]]).
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Punto de partida de la experiencia móvil del conductor, garantizando que el transportista no requiera credenciales corporativas permanentes.

---

#### RF011 — Establecimiento y Mantenimiento de Sesión Operativa Móvil
* **ID:** `RF011`
* **Caso de Prueba Asociado:** `CP-MOB-02`
* **Nombre:** Establecimiento y Mantenimiento de Sesión Operativa Móvil.
* **Descripción:** El sistema debe establecer y mantener una sesión operativa móvil local persistente en el dispositivo Android vinculada al despacho, vehículo y conductor, garantizando que la sesión sobreviva a pérdidas de cobertura celular o reinicios de la aplicación, reconectándose automáticamente sin exigir al conductor volver a digitar el código de activación.
* **Objetivo de Negocio:** Garantizar la continuidad operativa ininterrumpida en carreteras sin fricción para el conductor, asociando de forma unívoca cada coordenada y evento al viaje correcto.
* **Actor:** Sistema Backend / App Nativa Android.
* **Módulo:** Módulo Móvil de Activación (App Nativa Android).
* **Qué debe hacer el sistema:** Al activarse el código en RF010, emitir un token de sesión criptográfico con los identificadores del viaje; almacenarlo en el almacenamiento seguro de la App Nativa Android; ante caídas de señal o reinicios de la app, restaurar el contexto de forma transparente sin solicitar reingreso del código; y revocar el token inmediatamente cuando el despacho pase a `FINALIZADO` o `DESPACHO_CANCELADO`.
* **Entradas:** Validación exitosa de activación en RF010 o reconexión automática tras pérdida de señal.
* **Resultado Esperado:** Contexto de sesión móvil mantenido persistentemente durante todo el traslado, cesando automáticamente al cierre o cancelación del viaje.
* **Reglas de Negocio Relacionadas:** Persistencia de Sesión Móvil, Gobernanza de Sesiones.
* **Dependencias:** [[RF010]].
* **Criterios de Validación:** La sesión se mantiene válida durante toda la ruta interprovincial aun en modo avión; al cerrarse el viaje en `FINALIZADO` o cancelarse en `DESPACHO_CANCELADO`, el token es revocado y el dispositivo queda inmediatamente desvinculado.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Resuelve el problema de usabilidad en ruta: caídas temporales de cobertura 4G/3G no interrumpen el seguimiento ni fuerzan al conductor a detenerse a reingresar códigos.

---

#### RF012 — Visualización de Información Operativa del Despacho y Punto de Destino
* **ID:** `RF012`
* **Caso de Prueba Asociado:** `CP-MOB-03`
* **Nombre:** Visualización de Información Operativa del Despacho y Punto de Destino.
* **Descripción:** El sistema debe presentar al conductor en la App Nativa Android la información operativa del despacho: código de despacho, punto de destino de distribución, dirección física de llegada, cantidad de bultos y observaciones operativas de ruta, limitándose estrictamente a datos de traslado B2B sin introducir inventario, catálogo de productos, picking unitario, ventas comerciales ni información residencial B2C.
* **Objetivo de Negocio:** Proveer al transportista una hoja de ruta operativa digital clara y directa para el traslado físico intercentros, libre de datos comerciales confidenciales o ajenos a su función de transporte.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Ruta (App Nativa Android).
* **Qué debe hacer el sistema:** Descargar en memoria local los datos operativos del despacho al activarse la sesión ([[RF010]]) y presentarlos en una interfaz accesible, legible y operable aun sin conexión a internet; la información visualizada se restringe estrictamente a la unidad de despacho y su destino B2B.
* **Entradas:** Contexto de sesión móvil activa.
* **Resultado Esperado:** Pantalla de hoja de ruta disponible offline con información operativa de la carga completa y el destino B2B, sin datos de venta minorista.
* **Reglas de Negocio Relacionadas:** Delimitación de Alcance B2B Punto a Punto.
* **Dependencias:** [[RF010]], [[RF011]].
* **Criterios de Validación:** Visualización instantánea (< 500 ms); información estrictamente limitada a datos operativos del despacho troncal B2B; no introduce inventario, catálogo de productos, picking unitario, ventas comerciales ni información residencial B2C (consultoras/consumidores).
* **Fuente o Evidencia:** [[Funciones_Movil]] | [[Despacho_Como_Unidad_Logistica]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Asegura que el transportista cuente exclusivamente con la información necesaria para el traslado físico sin saturar la aplicación con datos comerciales o de inventario corporativo.

---

#### RF013 — Registro de Inicio de Traslado del Despacho (`EN_RUTA`)
* **ID:** `RF013`
* **Caso de Prueba Asociado:** `CP-MOB-04`
* **Nombre:** Registro de Inicio de Traslado del Despacho (`EN_RUTA`).
* **Descripción:** El sistema debe permitir al conductor registrar el hito formal de salida física del CD Lurín e inicio del traslado mediante la pulsación consciente del botón "Iniciar Despacho" en la App Nativa Android, capturando de forma atómica fecha, hora y coordenadas GPS iniciales de partida, transicionando el estado del despacho a `EN_RUTA` y disparando de forma automática el servicio de telemetría periódica GPS en segundo plano. Genera el evento `EN_RUTA`, cuya publicación al Bus corresponde a [[RF025]].
* **Objetivo de Negocio:** Acreditar con certeza satelital y temporal el hito formal de partida física de la carga desde el CD Lurín y certificar el inicio del traslado interprovincial, activando el rastreo continuo de la unidad.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Ruta (App Nativa Android).
* **Qué debe hacer el sistema:** Verificar que la sesión móvil esté activa y que el estado previo sea `HABILITADO`; al presionar "Iniciar Despacho", capturar de forma atómica la coordenada GPS y la estampa temporal de salida (`captured_at`); cambiar el estado a `EN_RUTA`, persistir el evento en la base de datos local SQLite (Room) ([[RF019]]), iniciar el Foreground Service de telemetría GPS periódica en segundo plano ([[RF014]]) y generar el evento inmutable `EN_RUTA` disponible para su sincronización y posterior publicación hacia el Bus corporativo ([[RF025]]).
* **Entradas:** Acción táctil del conductor en "Iniciar Despacho".
* **Resultado Esperado:** Hito de salida registrado con fecha, hora y ubicación atómica, despacho transicionado a `EN_RUTA`, servicio de rastreo satelital activo en Android y evento generado para sincronización.
* **Frontera con el Bus:** Y-Trace registra el hito de salida física y genera el evento de estado; la transmisión al Bus corporativo de integración se delega en su totalidad a [[RF025]].
* **Reglas de Negocio Relacionadas:** Condiciones de Inicio de Traslado.
* **Dependencias:** [[RF010]], [[RF011]].
* **Criterios de Validación:** No permite iniciar si la sesión no está activa o el código no fue validado; la acción manual del chofer es obligatoria; captura atómica de coordenadas y timestamp.
* **Fuente o Evidencia:** [[Flujo_Conductor]] | [[Ciclo_De_Vida_Del_Despacho]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Marca el hito formal de salida física de planta, certifica la ubicación inicial del viaje y activa la telemetría periódica en segundo plano. La publicación hacia el middleware se realiza de forma desacoplada vía RF025.

---

#### RF014 — Captura y Registro Periódico de Posición GPS en Ruta
* **ID:** `RF014`
* **Caso de Prueba Asociado:** `CP-GPS-01`
* **Nombre:** Captura y Registro Periódico de Posición GPS en Ruta.
* **Descripción:** El sistema debe capturar periódicamente coordenadas GPS en segundo plano desde el dispositivo Android durante el estado `EN_RUTA`, utilizando un intervalo operativo de 10 minutos para equilibrar la fidelidad del rastreo con la autonomía de la batería del smartphone en trayectos largos.
* **Objetivo de Negocio:** Mantener informada a la Torre de Control sobre el avance geográfico de la carga pesada en carreteras nacionales sin intervención humana del conductor.
* **Actor:** Sistema Móvil (App Nativa Android).
* **Módulo:** Módulo Móvil de Telemetría GPS.
* **Qué debe hacer el sistema:** Ejecutar un Foreground Service con notificación persistente que invoque el proveedor de ubicación fusionada (Fused Location Provider) de Android cada 10 minutos; capturar latitud, longitud, precisión horizontal y `captured_at`; persistir localmente y transmitir o encolar para sincronización según cobertura.
* **Entradas:** Señal de satélites GPS y reloj interno del dispositivo Android.
* **Resultado Esperado:** Registro continuo de coordenadas geográficas en ruta a intervalos regulares de 10 minutos.
* **Reglas de Negocio Relacionadas:** Frecuencia de Muestreo de Telemetría.
* **Dependencias:** [[RF013]].
* **Criterios de Validación:** El muestreo se ejecuta estrictamente mientras el estado sea `EN_RUTA`; cesa de inmediato en `EN_DESTINO`, `FINALIZADO` o `DESPACHO_CANCELADO`.
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] | [[Restricciones_Tecnicas]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el motor del seguimiento en carretera, diseñado bajo parámetros de estricta eficiencia energética (RNF008) y de datos móviles (RNF009).

---

#### RF015 — Registro de Llegada al Punto de Destino por Geocerca
* **ID:** `RF015`
* **Caso de Prueba Asociado:** `CP-ENT-01`
* **Nombre:** Registro de Llegada al Punto de Destino por Geocerca.
* **Descripción:** El sistema debe registrar automáticamente el arribo físico del vehículo al punto de distribución mediante la detección de ingreso al radio configurado de la geocerca perimétrica del destino, transicionando el estado del despacho a `EN_DESTINO` y capturando de forma atómica fecha, hora y ubicación GPS exacta. **Este cambio de estado se produce exclusivamente por geocerca automática, solo acredita la presencia física en destino y NO confirma la entrega de la carga.**
* **Objetivo de Negocio:** Auditar con certeza satelital objetiva la llegada física del vehículo al punto de entrega, sirviendo la coordenada GPS como respaldo geoespacial directo y habilitando la etapa de recepción física de la mercancía.
* **Actor:** Sistema Móvil (App Nativa Android).
* **Módulo:** Módulo Móvil de Llegada (App Nativa Android).
* **Qué debe hacer el sistema:** Comparar periódicamente las coordenadas GPS capturadas contra el radio configurado del destino; al detectar la entrada al perímetro de la geocerca, generar de forma atómica el evento `EN_DESTINO`, registrar `captured_at`, latitud y longitud, detener el muestreo periódico de carretera de 10 min, habilitar la pantalla de resolución de entrega en la App Nativa y encolar el evento para su sincronización y publicación al Bus ([[RF025]]).
* **Entradas:** Coordenadas GPS de la unidad y radio de geocerca del destino preconfigurado.
* **Resultado Esperado:** Despacho transicionado automáticamente al estado `EN_DESTINO` por detección perimétrica, con coordenadas y hora de arribo auditadas, y botones de confirmación de entrega habilitados en la App Nativa.
* **Reglas de Negocio Relacionadas:** Georreferenciación Automática de Arribo en Destino.
* **Dependencias:** [[RF013]], [[RF014]].
* **Análisis Crítico:** Desacopla con rigor la presencia física (hecho comprobado por geocerca) de la entrega de la mercancía (hecho que requiere verificación de bultos y precintos).

---

#### RF016 — Confirmación de Recepción / Entrega del Despacho Completo
* **ID:** `RF016`
* **Caso de Prueba Asociado:** `CP-ENT-02`
* **Nombre:** Confirmación de Recepción / Entrega del Despacho Completo en Destino.
* **Descripción:** El sistema debe exigir al conductor registrar manualmente la confirmación de recepción y entrega del despacho completo en el punto de destino (habilitado exclusivamente tras alcanzar el estado `EN_DESTINO`), registrando fecha, hora y ubicación GPS y transicionando el estado del despacho a `ENTREGADO`. **No requiere ni admite captura de fotografías ni comprobantes multimedia (POD).**
* **Objetivo de Negocio:** Formalizar la culminación conforme y auditada de la entrega de la carga completa en el punto de destino, fundamentada en datos estructurados verificables.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Entrega (App Nativa Android).
* **Qué debe hacer el sistema:** Presentar la pantalla de confirmación de entrega (activa únicamente en `EN_DESTINO`); al presionar conscientemente "Confirmar Entrega Conforme", capturar de forma atómica la coordenada GPS y la estampa temporal (`captured_at`), transicionar el despacho a `ENTREGADO`, almacenar el evento inmutable en SQLite (Room) local y encolarlo para su sincronización al backend y publicación al Bus corporativo ([[RF025]]).
* **Entradas:** Acción táctil de confirmación manual de entrega por el conductor y coordenadas GPS del dispositivo.
* **Resultado Esperado:** Evento inmutable `ENTREGADO` registrado con fecha, hora y ubicación GPS. Despacho listo para el cierre formal del viaje.
* **Reglas de Negocio Relacionadas:** Acción Manual Obligatoria de Entrega, Integridad Transaccional.
* **Dependencias:** [[RF015]].
* **Criterios de Validación:** Los botones de entrega están estrictamente bloqueados durante `EN_RUTA`; la acción manual es obligatoria; no se solicita DNI residencial ni captura de fotografías.
* **Fuente o Evidencia:** [[Despacho_Como_Unidad_Logistica]] | [[Ciclo_De_Vida_Del_Despacho]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Elimina la dependencia de fotos pesadas que fallan en zonas de baja señal, confiando la validez de la entrega a la coincidencia atómica de fecha, hora y geolocalización dentro del perímetro de destino.

---

#### RF017 — Registro de No Entrega o Rechazo de Despacho en Destino
* **ID:** `RF017`
* **Caso de Prueba Asociado:** `CP-ENT-03`
* **Nombre:** Registro de No Entrega o Rechazo de Despacho en Destino.
* **Descripción:** El sistema debe permitir al conductor registrar la no entrega o rechazo del despacho en el punto de destino cuando existan impedimentos operativos tipificados, registrando la causal estructurada, estampa de tiempo y coordenadas GPS, cambiando el estado a `NO_ENTREGADO` (acción solo permitida si el estado previo es `EN_DESTINO`). **No requiere captura de fotografías.**
* **Objetivo de Negocio:** Sustentar formalmente y sin ambigüedades los motivos por los cuales una carga no pudo ser recepcionada en la agencia o punto de destino.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Entrega (App Nativa Android).
* **Qué debe hacer el sistema:** Desplegar catálogo cerrado de causales tipificadas de no entrega (`PUNTO_CERRADO`, `RECHAZO_POR_DISCREPANCIA`, `ACCESO_BLOQUEADO`, `FUERZA_MAYOR`); requerir la selección de la causal y un texto breve explicativo; capturar coordenadas GPS y timestamp atómico, transicionar el estado a `NO_ENTREGADO`, guardar localmente en SQLite (Room) y encolar para sincronización y publicación al Bus ([[RF025]]).
* **Entradas:** Selección obligatoria de causal tipificada, texto explicativo y coordenadas GPS del intento de entrega.
* **Resultado Esperado:** Evento `NO_ENTREGADO` registrado con causal justificada y coordenadas auditadas. Despacho listo para cierre formal.
* **Reglas de Negocio Relacionadas:** Tipificación Obligatoria de No Entrega.
* **Dependencias:** [[RF015]].
* **Criterios de Validación:** No se permite registrar el estado sin seleccionar una causal del catálogo; se descarta la obligación de tomar fotos de fachadas o locales cerrados.
* **Fuente o Evidencia:** [[Eventos_E_Incidencias]] | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Estandariza las razones de fallo de entrega intercentros, permitiendo a Yanbal procesar automáticamente las discrepancias operativas.

---

#### RF018 — Finalización y Cierre del Seguimiento del Despacho
* **ID:** `RF018`
* **Caso de Prueba Asociado:** `CP-MOB-05`
* **Nombre:** Finalización y Cierre del Seguimiento del Despacho.
* **Descripción:** El sistema debe permitir al conductor finalizar formalmente el seguimiento operativo del despacho una vez resuelta la entrega en destino (`ENTREGADO` o `NO_ENTREGADO`) y verificado el vaciado de la cola local de sincronización, pasando el estado a `FINALIZADO` y revocando inmediatamente la sesión operativa móvil y el código de activación, sin realizar liquidación de documentos ni de fletes.
* **Objetivo de Negocio:** Finalizar formalmente el seguimiento operativo del despacho y cesar de forma terminante la emisión de telemetría y eventos desde el dispositivo móvil del conductor, garantizando la extinción de credenciales temporales sin ejecutar liquidación documental ni procesos logísticos de transporte.
* **Actor:** Conductor / Sistema Backend.
* **Módulo:** Módulo Móvil de Cierre (App Nativa Android).
* **Qué debe hacer el sistema:** Al presionar "Finalizar Despacho", verificar que el despacho tenga resolución final (`ENTREGADO` o `NO_ENTREGADO`) y que la cola local de SQLite (Room) esté vacía (cero transacciones pendientes); transicionar el estado a `FINALIZADO`; ordenar al backend la revocación inmediata del token de sesión móvil y la caducidad del código de activación; purgar los datos locales temporales del viaje en el móvil, apagar el servicio GPS y regresar la aplicación a su pantalla inicial de activación por código.
* **Entradas:** Acción consciente de finalización por el conductor en la App Nativa Android.
* **Resultado Esperado:** Despacho en estado `FINALIZADO`, sesión móvil revocada, código caducado y App Nativa retornada a la pantalla inicial de activación.
* **Reglas de Negocio Relacionadas:** Condiciones de Cierre Operativo, Vaciado Obligatorio de Cola.
* **Dependencias:** [[RF016]] o [[RF017]], [[RF020]].
* **Criterios de Validación:** No se puede finalizar si restan eventos pendientes por sincronizar en la cola local; tras finalizar, cualquier petición móvil posterior con ese token es rechazada (HTTP 401/403). No realiza liquidación física ni documental de fletes.
* **Fuente o Evidencia:** [[Ciclo_De_Vida_Del_Despacho]] | [[Flujo_Conductor]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Marca el cierre técnico y formal del seguimiento del despacho, no su liquidación documental o administrativa (que corresponde a sistemas externos de Yanbal). Protege la privacidad del conductor al garantizar que la app deje de emitir ubicación GPS al culminar el turno.

---

### Grupo 4: Módulo Móvil Offline-First y Sincronización en Android

#### RF019 — Almacenamiento Local Offline en Dispositivo Android (SQLite (Room))
* **ID:** `RF019`
* **Caso de Prueba Asociado:** `CP-OFF-01`
* **Nombre:** Almacenamiento Local Offline en Dispositivo Android (SQLite (Room)).
* **Descripción:** El sistema debe almacenar localmente en el dispositivo Android coordenadas GPS y eventos de estado cuando se interrumpa la cobertura celular en ruta (arquitectura *Offline-First*), sin almacenar fotografías ni archivos adjuntos.
* **Objetivo de Negocio:** Garantizar la continuidad operativa ininterrumpida en carreteras interprovinciales o zonas geográficas sin conectividad celular.
* **Actor:** Sistema Móvil (App Nativa Android).
* **Módulo:** Módulo Móvil de Persistencia Local.
* **Qué debe hacer el sistema:** Persistir cada acción del conductor (inicio de ruta, llegada, entrega, no entrega) y cada muestra GPS periódica en la base de datos local SQLite (Room) de Android en menos de 500 ms, asignando a cada registro un identificador UUIDv4 y la estampa temporal inmutable de captura (`captured_at`), con independencia absoluta de la señal de red.
* **Entradas:** Eventos de cambio de estado y muestras de telemetría GPS generadas en el móvil.
* **Resultado Esperado:** Persistencia local estructurada e inmediata en la base de datos SQLite del teléfono, lista para su lectura y sincronización posterior.
* **Reglas de Negocio Relacionadas:** Persistencia Local Offline-First.
* **Dependencias:** [[RF011]].
* **Criterios de Validación:** La aplicación opera con normalidad con el dispositivo en "Modo Avión", reteniendo al menos 500 eventos sin degradación funcional ni pérdida de datos (RNF012).
* **Fuente o Evidencia:** [[Offline_First]] | [[Restricciones_Tecnicas]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Pilar de la resiliencia en carretera: el conductor jamás se ve bloqueado por un error de "sin conexión", registrando sus hitos en tiempo y forma.

---

#### RF020 — Sincronización Automática en Segundo Plano (FIFO)
* **ID:** `RF020`
* **Caso de Prueba Asociado:** `CP-OFF-02`
* **Nombre:** Sincronización Automática en Segundo Plano (FIFO).
* **Descripción:** El sistema debe sincronizar automáticamente en orden cronológico estricto (FIFO) los eventos y telemetría retenidos localmente tan pronto se restablezca la conectividad de red celular o Wi-Fi.
* **Objetivo de Negocio:** Evitar la pérdida de información y actualizar la Torre de Control y el backend sin exigir acciones manuales complejas al transportista.
* **Actor:** Sistema Móvil (Foreground Service en Android) / Sistema Backend.
* **Módulo:** Módulo Móvil de Sincronización.
* **Qué debe hacer el sistema:** Monitorear el estado de red de Android; al detectar conectividad activa, leer la cola de SQLite (Room) en orden de `captured_at` y transmitir los registros por lotes mediante el endpoint de ingesta (`POST /api/v1/sync/batch`); el backend procesa secuencialmente los eventos de forma idempotente (basado en UUIDv4) y emite acuse de recibo; únicamente los registros confirmados por el backend son eliminados de la cola local del dispositivo.
* **Entradas:** Detección de conectividad de red activa (4G/3G/Wi-Fi) y lote ordenado de eventos pendientes.
* **Resultado Esperado:** Cola local vaciada progresivamente y base de datos central actualizada respetando la secuencia cronológica real en que ocurrieron los hechos.
* **Reglas de Negocio Relacionadas:** Sincronización Cronológica FIFO e Idempotencia.
* **Dependencias:** [[RF019]].
* **Criterios de Validación:** El backend procesa el lote respetando el orden por `captured_at`, preserva la estampa temporal original y descarta duplicados mediante UUIDv4 (RNF013).
* **Fuente o Evidencia:** [[Offline_First]] | [[Componentes_Aplicacion]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Automatiza el flujo de subida de información y garantiza la consistencia eventual entre el dispositivo móvil y el servidor central.

---

#### RF021 — Indicador Visual de Estado de Sincronización Local
* **ID:** `RF021`
* **Caso de Prueba Asociado:** `CP-OFF-03`
* **Nombre:** Indicador Visual de Estado de Sincronización Local.
* **Descripción:** El sistema debe presentar al conductor en la cabecera de la App Nativa Android un indicador visual permanente del estado de sincronización (verde: al día / sincronizado; amarillo: con contador numérico de transacciones pendientes en cola).
* **Objetivo de Negocio:** Brindar retroalimentación visual clara al transportista sobre la persistencia y subida efectiva de sus datos antes de salir de zona de cobertura o intentar finalizar el viaje.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Sincronización.
* **Qué debe hacer el sistema:** Consultar dinámicamente la cantidad de registros en la cola local de SQLite (Room); si es 0, desplegar icono verde ("Sincronizado"); si es mayor a 0, desplegar icono amarillo con el contador de eventos en cola (ej. "Cola: 4 pendientes").
* **Entradas:** Conteo en tiempo real de registros pendientes en la cola local de SQLite (Room).
* **Resultado Esperado:** Indicador visual permanente en pantalla que se actualiza reactivamente cuando cambian los elementos en cola.
* **Reglas de Negocio Relacionadas:** Visibilidad de Sincronización Local.
* **Dependencias:** [[RF019]], [[RF020]].
* **Criterios de Validación:** Si el conductor intenta finalizar el despacho con eventos en cola, el sistema le muestra un mensaje advirtiéndole que debe esperar a tener red para vaciar la cola ([[RF018]]).
* **Fuente o Evidencia:** [[Offline_First]] | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Elemento clave de usabilidad operativa que previene que los conductores apaguen la app antes de que sus confirmaciones lleguen al servidor.

---

### Grupo 5: Módulo Web de Monitoreo, Trazabilidad y Control Gerencial

#### RF022 — Monitoreo Operativo de Flota en Torre de Control Web
* **ID:** `RF022`
* **Caso de Prueba Asociado:** `CP-MON-01`
* **Nombre:** Monitoreo Operativo de Flota en Torre de Control Web.
* **Descripción:** El sistema debe proveer una grilla operativa de monitoreo en tiempo real para Supervisores y Jefatura de Distribución en la plataforma Web (Torre de Control), visualizando para cada despacho: despacho (código), vehículo/placa, conductor, destino, estado operativo con semáforo visual (Verde: `EN_RUTA`, Amarillo: `EN_DESTINO`, Gris: `FINALIZADO` / `DESPACHO_CANCELADO`), última posición GPS, marca de tiempo / tiempo de actualización y tiempo transcurrido desde el último reporte (antigüedad de telemetría). No contempla estados de incidencia ni mapas cartográficos pesados.
* **Objetivo de Negocio:** Habilitar el control operacional y supervisión activa de la flota en tiempo real con datos objetivos de ubicación, estado y frescura de la telemetría satelital.
* **Actor:** Supervisor de Distribución / Jefe de Distribución.
* **Módulo:** Módulo Web de Monitoreo de Despachos.
* **Qué debe hacer el sistema:** Proveer una tabla dinámica estructurada con actualización periódica que despliegue para cada despacho: código de despacho, vehículo/placa, conductor, destino, estado operativo con semáforo visual de tres estados, última posición GPS, marca de tiempo y tiempo de actualización, y tiempo transcurrido desde el último reporte (antigüedad de telemetría); ofrecer filtros por estado, destino y empresa transportista; y permitir la navegación directa hacia la vista de trazabilidad detallada ([[RF023]]).
* **Entradas:** Criterios de filtrado (rango de fechas, transportista, placa, estado, destino).
* **Resultado Esperado:** Grilla operativa en vivo con los 8 campos operacionales clave, tiempos de respuesta menores a 2 segundos y enlace directo a la trazabilidad unificada.
* **Reglas de Negocio Relacionadas:** Supervisión Operativa en Torre de Control, Frescura de Telemetría.
* **Dependencias:** [[RF013]], [[RF014]], [[RF015]], [[RF016]], [[RF017]], [[RF018]], [[RF009]].
* **Criterios de Validación:** Despliegue correcto de los 8 campos clave por despacho; tiempos de respuesta de consulta inferiores a 2.0 segundos (RNF015); grilla libre de estados `CON_INCIDENCIA`; semáforo tricolor estricto.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Proceso_TOBE]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Torre de Control ligera y enfocada: los supervisores disponen de una grilla operativa de alta legibilidad con el dato exacto de ubicación, estado y antigüedad del reporte sin sobrecargar la red con capas cartográficas pesadas.

---

#### RF023 — Consulta de Trazabilidad y Resumen del Despacho
* **ID:** `RF023`
* **Caso de Prueba Asociado:** `CP-TRAC-01`
* **Nombre:** Consulta de Trazabilidad y Resumen del Despacho.
* **Descripción:** El sistema debe permitir consultar el seguimiento completo de un despacho mediante su código de despacho o placa vehicular, mostrando los principales eventos operativos, coordenadas GPS registradas, cambios de estado, fechas y horas, duración de los hitos y recorrido realizado, incluyendo el resultado final o la cancelación forzada del seguimiento cuando corresponda. **Este requerimiento unifica la consulta integral de trazabilidad, erradica el término "evidencias" y no requiere ni almacena archivos multimedia.**
* **Objetivo de Negocio:** Proveer a Supervisores, Jefatura de Transporte y Operadores de Servicio al Cliente (SAC) visibilidad unificada e inmediata del ciclo de vida completo de un despacho en un único punto de consulta para resolver reclamos y auditorías en el primer contacto.
* **Actor:** Supervisor de Distribución / Operador SAC / Jefe de Distribución.
* **Módulo:** Módulo Web de Consulta de Trazabilidad.
* **Qué debe hacer el sistema:** Proveer barra de búsqueda indexada por código de despacho o placa; al consultar, recuperar en menos de 2.0 segundos y desplegar:
  1. Cabecera con datos del despacho (código, placa, transportista, origen, destino).
  2. Línea de tiempo cronológica con cada hito registrado (`DISPONIBLE`, `HABILITADO`, `EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`, `FINALIZADO` o `DESPACHO_CANCELADO`).
  3. Coordenadas GPS satelitales asociadas a cada evento y traza de telemetría en ruta con estampa de tiempo `captured_at`.
  4. Duración de cada etapa (tiempo transcurrido en CD, tiempo de viaje en ruta, tiempo de permanencia en destino).
  5. Resultado final de entrega con su causal tipificada en caso de `NO_ENTREGADO`, o motivo y usuario ejecutor en caso de `DESPACHO_CANCELADO`.
* **Entradas:** Código alfanumérico del despacho o placa vehicular ingresada en la barra de búsqueda.
* **Resultado Esperado:** Vista de trazabilidad completa y resumen del despacho desplegada en pantalla en menos de 2 segundos, sin enlaces rotos ni dependencias de buckets multimedia.
* **Reglas de Negocio Relacionadas:** Trazabilidad Unificada de Despacho, Inmutabilidad de Registros.
* **Dependencias:** [[RF008]], [[RF009]], [[RF013]], [[RF014]], [[RF015]], [[RF016]], [[RF017]], [[RF018]].
* **Criterios de Validación:** Búsqueda indexada sobre histórico de despachos con tiempo de respuesta inferior a 2.0 s (RNF015); visualización 100% basada en datos estructurados; no busca por DNI residencial ni expone datos B2C.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Flujo_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Requerimiento unificado clave: consolida en una sola capacidad potente la búsqueda, cronología y auditoría de despacho, suprimiendo la duplicidad anterior entre el buscador y la consulta de evidencias multimedia.

---

#### RF024 — Indicadores y Reportes de Gestión (Dashboard Ejecutivo)
* **ID:** `RF024`
* **Caso de Prueba Asociado:** `CP-KPI-01`
* **Nombre:** Indicadores y Reportes de Gestión (Dashboard Ejecutivo).
* **Descripción:** El sistema debe presentar un tablero ejecutivo de indicadores logísticos con métricas de tiempos de traslado (Lead Time), tasa de entregas conformes y latencia de integración, permitiendo consultar y filtrar la información mediante los criterios disponibles en la vista operativa y exportar los resultados exclusivamente en formato Excel (.xlsx). La evaluación podrá realizarse por empresa transportista y período, manteniendo la exclusión de despachos cancelados de las métricas de Lead Time y puntualidad, contabilizándolos en una métrica separada de cancelación.
* **Objetivo de Negocio:** Permitir el análisis estratégico del nivel de servicio de transporte interprovincial, la evaluación objetiva de contratos de transportistas y la toma de decisiones gerenciales basada en datos confiables.
* **Actor:** Jefe de Distribución / Gerencia de Operaciones.
* **Módulo:** Módulo Web de Indicadores (Dashboard).
* **Qué debe hacer el sistema:** Calcular y graficar métricas agregadas por empresa transportista y rango temporal; desplegar tasa de entregas conformes (`ENTREGADO`), tasa de no entregas (`NO_ENTREGADO`), ratio de cancelaciones forzadas (`DESPACHO_CANCELADO`) y Lead Time promedio; proveer botón de descarga que exporte la sábana de datos filtrada exclusivamente en formato Excel (.xlsx).
* **Entradas:** Filtros de período (semana, mes, campaña), empresa transportista y destino.
* **Resultado Esperado:** Tablero visual interactivo con KPIs logísticos y exportación de reportes exclusivamente en formato Excel.
* **Reglas de Negocio Relacionadas:** Regla de Cálculo de Indicadores Operativos (los despachos cancelados se aíslan en métrica propia y no distorsionan el Lead Time).
* **Dependencias:** [[RF018]], [[RF009]], [[RF022]], [[RF023]].
* **Criterios de Validación:** Cálculos basados en eventos inmutables consolidados; exportación disponible exclusivamente en formato Excel (.xlsx); exclusión estricta de cancelados del Lead Time.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Oportunidad_de_Mejora]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Entrega a la gerencia de Yanbal la herramienta de control de gestión contractual de los transportistas terceros.

---

### Grupo 6: Módulo Backend de Integración Corporativa (Bus ESB)

#### RF025 — Publicación de Eventos de Despacho al Bus de Integración
* **ID:** `RF025`
* **Caso de Prueba Asociado:** `CP-INT-01`
* **Nombre:** Publicación de Eventos de Despacho al Bus de Integración.
* **Descripción:** El sistema debe publicar al Bus de Integración corporativo, en formato JSON canónico estandarizado, los eventos operativos del ciclo de vida del despacho: `EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO` y `DESPACHO_CANCELADO`. El hito interno de control previo se conserva como registro de auditoría y no se publica mediante este requerimiento.
* **Objetivo de Negocio:** Mantener sincronizados los sistemas centrales de Yanbal (SAP R/3, Salesforce, sistemas comerciales) con un SLA de integración de 30 minutos como máximo, reduciendo el desfase histórico de 2 horas.
* **Actor:** Sistema Backend / Bus de Integración (ESB).
* **Módulo:** Módulo Backend de Integración (ESB).
* **Qué debe hacer el sistema:** Al persistirse un evento integrable (`EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`, `DESPACHO_CANCELADO`), construir un payload JSON con esquema canónico (`despacho_id`, `estado`, `timestamp_utc`, `latitud`, `longitud`, `causal` cuando aplique), encolarlo en el broker de mensajería interno y transmitirlo al Bus corporativo de forma asíncrona; en caso de fallo de red corporativa, aplicar reintentos con retroceso exponencial garantizando aceptación dentro de los 30 minutos.
* **Entradas:** Eventos de estado operativos inmutables persistidos en la base de datos de Y-Trace.
* **Resultado Esperado:** Mensaje JSON canónico recibido y aceptado con acuse conforme por el Bus corporativo de Yanbal.
* **Frontera con el Bus:** Y-Trace es el publicador de eventos; el Bus es el middleware corporativo externo encargado de distribuir los mensajes a SAP o Salesforce.
* **Reglas de Negocio Relacionadas:** SLA Central de Integración al Bus ($\le 30$ minutos).
* **Dependencias:** [[RF009]], [[RF013]], [[RF015]], [[RF016]], [[RF017]].
* **Criterios de Validación:** El 100% de eventos integrables son aceptados por el Bus en $\le$ 30 min desde su ingesta en el backend (RNF006); estricto cumplimiento del JSON Schema canónico (RNF017).
* **Fuente o Evidencia:** [[Bus_Integracion]] | [[Integracion_Bus_Eventos]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el requerimiento arquitectónico que resuelve la problemática central del negocio identificada en las entrevistas: acortar la visibilidad de 2 horas a un máximo de 30 minutos.

---

#### RF026 — Envío de Resumen de Trazabilidad del Despacho al Bus
* **ID:** `RF026`
* **Caso de Prueba Asociado:** `CP-INT-03`
* **Nombre:** Envío de Resumen de Trazabilidad del Despacho al Bus.
* **Descripción:** El sistema debe transmitir el resumen consolidado de trazabilidad del despacho al Bus de Integración tras su finalización formal (`FINALIZADO`) o cancelación forzada (`DESPACHO_CANCELADO`), sin ejecutar liquidación económica ni logística.
* **Objetivo de Negocio:** Proporcionar a los sistemas externos de Yanbal la información consolidada final del seguimiento necesaria para sus procesos de conciliación, liquidación o reposición industrial.
* **Actor:** Sistema Backend / Bus de Integración (ESB).
* **Módulo:** Módulo Backend de Integración (ESB).
* **Qué debe hacer el sistema:** Al transicionar el despacho a `FINALIZADO` ([[RF018]]) o `DESPACHO_CANCELADO` ([[RF009]]), consolidar los datos finales del viaje (hora de inicio, hora de llegada, resultado de entrega, motivo de cancelación si aplica, cantidad de eventos de telemetría y duración total), generar el mensaje JSON de cierre y publicarlo al Bus corporativo.
* **Entradas:** Evento de cierre formal del despacho (`FINALIZADO` o `DESPACHO_CANCELADO`).
* **Resultado Esperado:** Mensaje consolidado de trazabilidad transmitido al Bus con acuse de recibo conforme.
* **Reglas de Negocio Relacionadas:** Condiciones de Cierre Operativo.
* **Dependencias:** [[RF018]], [[RF009]], [[RF025]].
* **Criterios de Validación:** Solo se transmite tras el cierre formal; no realiza cálculos de tarifas ni pagos a transportistas (responsabilidad de NSDG/SAP).
* **Fuente o Evidencia:** [[Integracion_Bus_Eventos]] | [[Ciclo_De_Vida_Del_Despacho]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Cierra el ciclo de interoperabilidad informando a SAP que el viaje concluyó para que prosigan los procesos administrativos corporativos.

---

### Grupo 7: Módulo de Seguridad y Gobernanza de Activación Móvil

#### RF027 — Control de Intentos y Bloqueo del Código de Activación Móvil
* **ID:** `RF027`
* **Caso de Prueba Asociado:** `CP-MOB-SEC-01`
* **Nombre:** Control de Intentos y Bloqueo del Código de Activación Móvil.
* **Descripción:** El sistema debe limitar los intentos fallidos de ingreso del código de activación móvil ([[RF010]]) a un máximo de 5 intentos consecutivos, bloqueando e invalidando el código de forma irrevocable al registrar el quinto fallo.
* **Objetivo de Negocio:** Prevenir ataques automatizados de fuerza bruta o adivinación sobre el espacio de códigos alfanuméricos de 8 caracteres asignados en andén.
* **Actor:** Conductor / Sistema Móvil / Sistema Backend.
* **Módulo:** Módulo Móvil de Activación (App Nativa Android) + Backend de Seguridad.
* **Qué debe hacer el sistema:** Contabilizar en backend los intentos fallidos asociados al código o identificador de dispositivo; al registrar el quinto fallo consecutivo, transicionar el código al estado `BLOQUEADO` e invalidarlo de forma irrevocable, registrando el evento de bloqueo en la bitácora de auditoría; responder con error HTTP 403 Forbidden y mensaje disuasivo en la App Nativa impidiendo cualquier reintento adicional con dicho código.
* **Entradas:** Código alfanumérico ingresado en la App Nativa Android, identificador del dispositivo.
* **Resultado Esperado:** Código invalidado de forma permanente al quinto fallo consecutivo; rechazo automático de peticiones posteriores sin emisión de alertas en consola Web.
* **Reglas de Negocio Relacionadas:** Separación Dual de Mecanismos de Acceso, Límite de Fuerza Bruta.
* **Dependencias:** [[RF008]], [[RF010]].
* **Criterios de Validación:** Bloqueo inmutable al 5to intento consecutivo; imposibilidad de activar el despacho con el código bloqueado. Sin envío de alertas a supervisores.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Cierra la asimetría de seguridad móvil frente al bloqueo de cuentas web de RF001, protegiendo los despachos contra accesos maliciosos automatizados de forma autónoma.

---

#### RF028 — Unicidad, Vigencia y Caducidad del Código de Activación Móvil
* **ID:** `RF028`
* **Caso de Prueba Asociado:** `CP-COD-02`
* **Nombre:** Unicidad, Vigencia y Caducidad del Código de Activación Móvil.
* **Descripción:** El sistema debe garantizar unicidad activa simultánea a cada Código de Activación generado en [[RF008]] y revocar automáticamente su validez y la de la sesión operativa tan pronto el despacho pase a `FINALIZADO` o `DESPACHO_CANCELADO`.
* **Objetivo de Negocio:** Reducir la ventana de exposición temporal y garantizar que ningún código ni sesión móvil permanezca activo indefinidamente tras la entrega o cancelación.
* **Actor:** Supervisor de Distribución / Sistema Backend.
* **Módulo:** Módulo de Códigos y Activación.
* **Qué debe hacer el sistema:** Al emitir el código en RF008, asegurar unicidad frente a cualquier otro código activo; al transicionar el despacho a `FINALIZADO` ([[RF018]]) o `DESPACHO_CANCELADO` ([[RF009]]), marcar el código como `REVOCADO` o `CONSUMIDO` en base de datos, impidiendo cualquier activación posterior.
* **Entradas:** Solicitud de código autorizada por el Supervisor; evento de finalización o cancelación de seguimiento.
* **Resultado Esperado:** Código con ciclo de vida estrictamente acotado a la duración del viaje autorizado; rechazo de uso posterior.
* **Reglas de Negocio Relacionadas:** Acceso Móvil sin Contraseñas, Gobernanza de Sesiones.
* **Dependencias:** [[RF008]], [[RF011]], [[RF018]], [[RF009]].
* **Criterios de Validación:** Un código de un despacho cerrado o cancelado no puede ser reutilizado; la base de datos impone restricción de unicidad activa.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Garantiza que los códigos alfanuméricos sean secretos efímeros y no credenciales permanentes, cumpliendo con la directriz de seguridad de la arquitectura.

---

## 3. Análisis Crítico de Candidatos Descartados, Reformulados o Fuera de Alcance

Para asegurar la máxima rigurosidad conceptual y delimitar con total exactitud la frontera de Y-Trace, a continuación se detallan los elementos evaluados que **NO fueron catalogados como Requerimientos Funcionales de Y-Trace** o que fueron depurados de versiones anteriores:

### 3.1 Justificación Formal de Requerimientos Eliminados o Descartados
1. **Eliminación Total de la Gestión de Incidencias en Ruta (propuestas preliminares de reporte móvil, alertas sonoras y notificaciones push):**
   - *Motivo de Exclusión:* En la operación real de transporte interprovincial de Yanbal, los siniestros viales graves (accidentes, asaltos, bloqueos de carretera) se comunican de forma inmediata vía llamada telefónica externa a la Torre de Control o policía de carreteras. Introducir formularios de reporte, captura de percances y categorización en la App del conductor resulta riesgoso e inoperante en situaciones de peligro. Por ello, Y-Trace no dispone de módulo móvil de incidencias ni estado `CON_INCIDENCIA`. La gestión de contingencias insalvables se resuelve desde la Web mediante la **Cancelación Forzada del Seguimiento (`RF009`)**, que apaga el GPS y revoca los accesos móviles.
2. **Eliminación de Captura y Almacenamiento de Fotografías y Evidencias Multimedia (ex-RF018, ex-RF026):**
   - *Motivo de Exclusión:* La captura de fotos de bultos o precintos genera sobrecostos de almacenamiento en la nube, drenaje severo de batería y fallas críticas de transmisión en carreteras con señal intermitente. En despachos B2B punto a punto, la prueba de entrega se sustenta de forma sólida y suficiente en la coincidencia atómica de geolocalización satelital (`EN_DESTINO`), estampa temporal inmutable (`captured_at`) y la confirmación manual consciente del transportista (`ENTREGADO`), complementada con la causal tipificada en caso de no entrega (`NO_ENTREGADO`). Se eliminan los servicios de Object Storage y pipelines de compresión de imágenes.
3. **Unificación de la Consulta de Trazabilidad y Supresión del Término "Evidencias" (ex-RF026 y ex-RF027 en RF023):**
   - *Motivo de Exclusión:* Existía una redundancia funcional innecesaria entre la consulta de evidencias web y el buscador de trazabilidad rápida. Ambas capacidades se fusionaron en **`RF023 — Consulta de Trazabilidad y Resumen del Despacho`**, erradicando el término "evidencias" y concentrando el despliegue de toda la traza de eventos, tiempos, estados y coordenadas GPS.
4. **Eliminación de Entrega Domiciliaria a Familiares y Parentesco:**
   - *Motivo de Exclusión:* El alcance oficial de Y-Trace está centrado en **despachos de distribución completa entre puntos de distribución (B2B)**. La figura de entrega a familiares directos (cónyuge, hijo, padre) y el registro de grados de parentesco pertenecen al reparto domiciliario minorista de venta directa (B2C), ajeno a la operación intercentros.
5. **Eliminación de Exigencia de DNI Residencial y Firmas de Personas Naturales:**
   - *Motivo de Exclusión:* En despachos completos intercentros, la recepción de la carga la efectúa el personal del punto de destino. Exigir DNI de 8 dígitos de personas naturales residenciales distorsionaba el alcance de recepción logística.
6. **Eliminación del Monitoreo Cartográfico Pesado en la Grilla Web:**
   - *Motivo de Exclusión:* Se eliminó la capa cartográfica interactiva pesada (Leaflet/Mapbox) para concentrar la Torre de Control en la Grilla Operativa de Despachos ([[RF022]]) y la vista de Trazabilidad ([[RF023]]), simplificando la arquitectura y reduciendo la latencia de interfaz sin perder el control de la flota.
7. **Eliminación del Aislamiento Lógico Multi-tenant por Empresa Transportista (ex-RF029):**
   - *Motivo de Exclusión:* Y-Trace es una plataforma web de uso interno exclusivo de los colaboradores de Yanbal Perú (Supervisores de Distribución, Jefatura, SAC, Administrador), quienes tienen la responsabilidad de fiscalizar la totalidad de los despachos de la flota contratada. Las empresas transportistas terceras no disponen de usuarios ni acceso a la consola Web (los conductores interactúan únicamente con la App Nativa mediante códigos efímeros de 8 caracteres). Establecer una segmentación de datos multi-tenant por contratista sobrepasaba el alcance del proyecto e introducía restricciones artificiales innecesarias. La evaluación comparativa de transportistas, su nivel de cumplimiento y puntualidad se atienden de forma nativa mediante los filtros y métricas del Tablero de Indicadores ([[RF024]]).

### 3.2 Procesos Trasladados Fuera de Alcance (Pertenecen a Sistemas Existentes)
1. **Gestión de Stock, Lotes y Picking Unitario de Mercancía:** Responsabilidad exclusiva de **[[SAP_R3]]** y del WMS **[[SPY]]** en el CD de Lurín. Y-Trace inicia cuando el despacho ya está consolidado y listo en andén.
2. **Cálculo Heurístico y Optimización de Rutas (VRP):** Corresponde al software TMS **[[Driving]]**. Y-Trace recibe el punto de destino para monitorear su ejecución física real.
3. **Catálogo Comercial, Ventas y Gestión de Clientes/Consultoras:** Pertenece a **[[Maya]]** y **[[SAP_Commerce]]**. Y-Trace no administra pedidos comerciales individuales ni perfiles de consultoras.
4. **Administración y Maestro de Conductores y Vehículos:** Y-Trace no da de alta conductores ni empadrona vehículos; esa información maestra reside en los sistemas centrales de flota de Yanbal y se consume como catálogo de solo lectura.
5. **Logística Inversa Física, Devoluciones y Dictamen de Calidad:** El reingreso físico por esclusas y la inspección pericial ocurren tras el retorno del vehículo en CD, fuera de última milla. Y-Trace se limita a registrar el estado del despacho.
6. **Liquidación Tarifaria de Fletes y Pagos a Transportistas:** Corresponde a los contratos de transporte gestionados mediante **[[NSDG]]** y la gerencia de finanzas de Yanbal.

---

## 4. Exclusiones Explícitas del Alcance (Frontera del Sistema)

Para garantizar la absoluta integridad conceptual y operativa de Y-Trace, se consolidan formalmente las siguientes exclusiones taxativas:

1. **No reparto domiciliario minorista (B2C):** Y-Trace no realiza ni gestiona distribución de última milla hacia consumidores finales ni domicilios particulares.
2. **No entrega a consultoras/familiares con DNI residencial:** Queda formalmente excluida toda captura de grado de parentesco, DNI de personas naturales y firmas de recepción en hogares particulares.
3. **No gestión de despachos unitarios — solo despachos de carga completa punto a punto:** El alcance operacional está restringido estrictamente a despachos de carga completa intercentros (Centro de Distribución Lurín → Punto de Distribución / Agencia receptora).
4. **No soporte iOS ni formato PWA:** La aplicación móvil para conductores es exclusivamente nativa para Android (versión 8.0 Oreo o superior). Queda expresamente fuera de alcance el soporte para iOS / Apple y la modalidad de navegador web progresivo (PWA).
5. **No gestión de incidencias dentro del aplicativo:** Sin botones "Reportar Incidencia", formularios móviles, tipificación en ruta ni estado `CON_INCIDENCIA`. Toda contingencia se comunica al 100% fuera del sistema vía telefónica externa.
6. **No captura ni almacenamiento de fotografías, imágenes o POD:** La trazabilidad se sustenta exclusivamente en datos estructurados verificables (coordenadas GPS satelitales, marcas de tiempo inmutables `captured_at`, estados y causales tipificadas).
7. **No gestión logística upstream:** Y-Trace no crea, programa, asigna, prepara, libera ni cancela logísticamente los despachos. Recibe despachos existentes desde sistemas externos y concentra su función en seguimiento, telemetría y publicación de eventos hacia el Bus.
8. **No aislamiento multi-tenant por empresa transportista:** Y-Trace no es una plataforma multi-tenant con segregación de datos por empresa transportista ni perfiles externos para contratistas. Los transportistas no interactúan con la plataforma Web; la consola Web es de uso exclusivo del personal interno de Yanbal, quienes supervisan la totalidad de las operaciones. El análisis por transportista se realiza como dimensión analítica de consulta y exportación en el Tablero de Indicadores ([[RF024]]).
