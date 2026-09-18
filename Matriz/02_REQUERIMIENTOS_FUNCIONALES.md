# Catálogo Oficial de Requerimientos Funcionales: Y-Trace

> **Carpeta:** `detalles`  
> **Documento:** `02_REQUERIMIENTOS_FUNCIONALES.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Propósito:** Especificación exhaustiva, estructurada y crítica de todos los Requerimientos Funcionales (RF) de la solución bajo el alcance exclusivo de distribución completa entre puntos de distribución (B2B), justificando su condición funcional frente a restricciones técnicas, reglas de negocio y sistemas externos.  
> **Estado:** [CONSOLIDADO OFICIAL — ALCANCE B2B PUNTO A PUNTO]  
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
1. **Diferencia frente a un RNF:** Un RF define *la acción o capacidad operativa* (ej. capturar coordenadas GPS, activar un despacho, registrar una entrega), mientras que el RNF define *la restricción o criterio de calidad* bajo el cual debe ejecutarse dicha acción (ej. latencia máxima de 30 minutos, disponibilidad del 99.5%, consumo de batería menor al 15%).
2. **Diferencia frente a una Regla de Negocio (RN):** Una regla de negocio es una directriz preexistente (ej. *"Un despacho no puede cerrarse sin haber resuelto la entrega en destino"*); el requerimiento funcional es *el mecanismo de software* que implementa y hace cumplir dicha política.
3. **Diferencia frente a una Decisión Técnica o de Arquitectura:** Un RF declara la necesidad operativa sin amarrarse prematuramente a librerías específicas (ej. *"Guardar transacciones localmente ante pérdida de red"* es el RF; el uso concreto de IndexedDB es la decisión técnica).
4. **Respeto a la Frontera del Sistema:** Se excluyen taxativamente las responsabilidades de los sistemas corporativos consolidados de Yanbal ([[SAP_R3]], [[SPY]], [[Maya]], [[SAP_Commerce]], [[Driving]], [[NSDG]], [[Salesforce]]).

### 1.2 Delimitación de Alcance Funcional: Distribución Completa Punto a Punto (B2B)
El alcance de Y-Trace modela conceptualmente el siguiente ciclo:
```
Origen (CD Lurín) 
   → Despacho Completo (Carga ya consolidada y preparada)
   → Vehículo + Conductor (Catálogo corporativo de transporte)
   → Traslado en Ruta (Telemetría satelital periódica en Android)
   → Punto de Destino (Agencia / Centro de distribución secundario)
   → Llegada a Destino (Confirmación "Llegué a destino" → Estado EN_DESTINO con fecha, hora y GPS)
   → Confirmación de Entrega (Recepción de carga completa → Estado ENTREGADO)
   → Evidencia Operativa (registro del evento + fecha/hora + ubicación GPS cuando esté disponible + evidencia fotográfica complementaria opcional)
   → Cierre de Operación (Estado FINALIZADO → Revocación inmediata de código y sesión móvil)
```
Y-Trace **no** es un sistema de picking, inventario, ventas ni de reparto minorista domiciliario (B2C) de cajas a consultoras. Las cajas ya existen y están rotuladas en los sistemas correspondientes; Y-Trace se concentra exclusivamente en su traslado, seguimiento y trazabilidad operativa.
Y-Trace tampoco crea, programa, asigna, libera ni cancela logísticamente los despachos. Esas actividades pertenecen a los sistemas y procesos externos; el sistema inicia su participación cuando existe un despacho disponible para seguimiento.

---

### 1.3 Definición del Hito Interno de Control Previo

El **hito interno de control previo** es un registro interno de Y-Trace que acredita que un despacho ya existente, recibido desde los sistemas externos, ha quedado **habilitado para iniciar su seguimiento en Y-Trace**. Este hito no crea, programa, asigna, prepara, libera ni autoriza logísticamente el despacho para su salida. Su finalidad es controlar la transición desde la disponibilidad del despacho hacia la activación móvil y conservar trazabilidad interna. El hito se registra cuando el Supervisor solicita la generación del Código de Activación mediante [[RF008]] y el sistema valida la elegibilidad del despacho. No se publica al Bus mediante [[RF029]].

### 1.4 Modelo de Evidencias Operativas

En Y-Trace la **evidencia operativa es obligatoria a nivel de evento**, pero no se limita a una fotografía. Cada evento relevante debe conservar, según disponibilidad del dispositivo y del contexto operativo, su identificador de despacho, tipo de evento, fecha y hora de captura, actor o sesión operativa y ubicación GPS cuando esté disponible.

La **fotografía es una evidencia complementaria opcional** para los eventos `ENTREGADO`, `NO_ENTREGADO` e incidencias. No debe ser una condición bloqueante para registrar el evento, porque una colisión, avería, urgencia o daño del dispositivo puede impedir tomarla. Cuando exista una fotografía, se almacena en Cloud Storage y se conserva en la base de datos únicamente su referencia segura.

La ausencia de fotografía **no invalida** un evento que ya cuenta con su registro operativo y demás evidencias disponibles.

---

## 2. Catálogo Detallado de Requerimientos Funcionales (RF001 – RF034)

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
* **Entradas:** Identificador de usuario (correo corporativo o usuario de red), contraseña en texto plano introducida por el usuario.
* **Resultado Esperado:** Usuario autenticado con sesión iniciada y redirección al panel correspondiente a su rol, o mensaje de rechazo de credenciales.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Separación Dual de Mecanismos de Acceso).
* **Dependencias:** Ninguna (punto de entrada a la plataforma Web).
* **Criterios de Validación:** Ingreso exitoso con credenciales válidas; bloqueo tras 5 intentos fallidos consecutivos; rechazo inmediato ante cuentas inactivas.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 1 y 4) | [[Usuarios_Web]] (Sección 3).
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
* **Resultado Esperado:** Cuenta creada o modificada en el repositorio central de usuarios, con envío de notificación de credenciales provisionales al usuario.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Mínimo Privilegio).
* **Dependencias:** [[RF001]].
* **Criterios de Validación:** Operación restringida exclusivamente al rol Administrador Principal; bloqueo automático ante correos duplicados.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 2) | [[Funciones_Web]] (Sección 1.1).
* **Estado:** Confirmado.
* **Análisis Crítico:** Capacidad administrativa que garantiza la administración centralizada de identidades sin intervención de personal operativo de transporte.

---

#### RF003 — Asignación y Gestión de Roles de Usuarios Web
* **ID:** `RF003`
* **Caso de Prueba Asociado:** `CP-ROL-01`
* **Nombre:** Asignación y Gestión de Roles de Usuarios Web.
* **Descripción:** El sistema debe permitir al Administrador Principal asignar o revocar uno o más de los 4 roles canónicos web (`Administrador Principal`, `Supervisor de Distribución`, `Jefe de Distribución`, `Operador SAC`) a los usuarios autorizados.
* **Objetivo de Negocio:** Mantener una estricta segregación funcional de responsabilidades y aplicar el principio de mínimo privilegio en Yanbal.
* **Actor:** Administrador Principal del Sistema.
* **Módulo:** Módulo de Administración de Usuarios.
* **Qué debe hacer el sistema:** Permitir seleccionar un usuario y asociarle su rol correspondiente, recalculando de forma inmediata su matriz de permisos y registrando la modificación en la bitácora de auditoría.
* **Entradas:** Identificador de usuario (`usuario_id`), rol seleccionado (`rol_id`).
* **Resultado Esperado:** Perfil del usuario actualizado con sus nuevos privilegios operativos en la plataforma Web.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Mínimo Privilegio).
* **Dependencias:** [[RF002]].
* **Criterios de Validación:** Un usuario sin rol no puede acceder a ningún módulo operativo; la reasignación de rol se refleja en la siguiente petición autenticada.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 3) | [[Usuarios_Web]] (Sección 1).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RF de gobernanza RBAC. Evita que personal no facultado acceda a funciones críticas como la habilitación de seguimiento o reportes estratégicos.

---

#### RF004 — Control de Acceso Basado en Roles (RBAC) en Vistas y Operaciones Web
* **ID:** `RF004`
* **Caso de Prueba Asociado:** `CP-RBAC-01`
* **Nombre:** Control de Acceso Basado en Roles (RBAC) en Vistas y Operaciones Web.
* **Descripción:** El sistema debe restringir la navegación, menús visuales y la ejecución de endpoints y operaciones de la plataforma Web según el rol asignado al usuario autenticado.
* **Objetivo de Negocio:** Prevenir la ejecución indebida de transacciones logísticas o de configuración por personal no facultado.
* **Actor:** Sistema Backend / Todos los usuarios web.
* **Módulo:** Módulo de Seguridad y Acceso Web.
* **Qué debe hacer el sistema:** Interceptar cada solicitud HTTP y renderizado en cliente; validar si el rol del token de sesión posee permisos para la ruta o acción invocada; en caso de carecer de privilegios, retornar HTTP 403 Forbidden y denegar el despliegue.
* **Entradas:** Token de sesión del usuario, ruta u operación invocada.
* **Resultado Esperado:** Acceso permitido a recursos autorizados y bloqueo inmediato con mensaje explicativo ante rutas restringidas.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Mínimo Privilegio).
* **Dependencias:** [[RF001]], [[RF003]].
* **Criterios de Validación:** Un Operador SAC no puede invocar endpoints de creación de despachos; un Supervisor no puede invocar endpoints de administración de cuentas.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 3) | [[Componentes_Aplicacion]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Define la aplicación práctica del RBAC en la arquitectura de software web, asegurando consistencia entre el front-end y el back-end.

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
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Separación Dual de Mecanismos de Acceso).
* **Dependencias:** [[RF001]].
* **Criterios de Validación:** Toda llamada tras 15 minutos de inactividad debe recibir un HTTP 401; la revocación administrativa invalida el token de inmediato.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 5) | [[Usuarios_Web]] (Sección 4).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es un RF de seguridad y control de sesiones que protege la plataforma web corporativa contra secuestro de sesiones (*session hijacking*).

---

#### RF006 — Registro de Bitácora de Auditoría de Acciones Administrativas Web
* **ID:** `RF006`
* **Caso de Prueba Asociado:** `CP-AUD-01`
* **Nombre:** Registro de Bitácora de Auditoría de Acciones Administrativas Web.
* **Descripción:** El sistema debe registrar en una bitácora inmutable todas las acciones administrativas críticas ejecutadas en la plataforma Web (altas, bajas, modificaciones de cuentas, cambios de roles, bloqueos, cierres forzados e intentos fallidos de autenticación).
* **Objetivo de Negocio:** Proveer trazabilidad de auditoría informática para cumplimiento de seguridad de la información y revisiones periciales.
* **Actor:** Sistema Backend / Administrador Principal.
* **Módulo:** Módulo de Auditoría y Seguridad.
* **Qué debe hacer el sistema:** Capturar de forma atómica en cada evento administrativo: `usuario_ejecutor_id`, `tipo_evento`, `tabla_afectada`, `valores_anteriores`, `valores_nuevos`, `ip_origen`, `user_agent` y estampa de tiempo UTC; almacenar el registro bajo esquema de solo anexión (*append-only*).
* **Entradas:** Eventos del sistema resultantes de acciones de usuarios web.
* **Resultado Esperado:** Registro consolidado en la tabla de bitácora, accesible en modo solo lectura mediante la consola de auditoría para el Administrador.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Mínimo Privilegio).
* **Dependencias:** [[RF001]], [[RF002]], [[RF003]].
* **Criterios de Validación:** Cero posibilidad de modificar o borrar entradas de bitácora; toda acción crítica genera un registro inmediato.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 4) | [[Funciones_Web]] (Sección 1.1).
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el requerimiento funcional que alimenta la auditoría perimetral web; su inmutabilidad formal está respaldada por el RNF023.

---

### Grupo 2: Módulo de Consulta y Habilitación de Seguimiento en Origen

#### RF007 — Consulta de Cargas y Despachos Preparados en CD Origen
* **ID:** `RF007`
* **Caso de Prueba Asociado:** `CP-DESP-01`
* **Nombre:** Consulta de Cargas y Despachos Preparados en CD Origen.
* **Descripción:** El sistema debe permitir al Supervisor de Distribución consultar y filtrar la lista de despachos completos ya existentes, recibidos desde los sistemas externos y disponibles para iniciar su seguimiento hacia los puntos de destino.
* **Objetivo de Negocio:** Proveer visibilidad inmediata de los despachos completos ya preparados por los sistemas externos y disponibles para iniciar su seguimiento.
* **Actor:** Supervisor de Distribución.
* **Módulo:** Módulo de Gestión de Despachos.
* **Qué debe hacer el sistema:** Consultar los despachos completos puestos a disposición por los sistemas externos (incluido SPY vía Bus), desplegar su información operativa disponible y ofrecer filtros por fecha, destino y transportista. La consulta no crea, asigna, programa ni autoriza logísticamente el despacho.
* **Entradas:** Parámetros de filtrado (rango de fechas, punto de destino, transportista).
* **Resultado Esperado:** Tabla de despachos completos existentes y disponibles para habilitar su seguimiento.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Seguimiento y Trazabilidad).
* **Dependencias:** Disponibilidad del flujo de entrada de cargas desde los sistemas upstream mediante [[Bus_Integracion]]. No depende de RF029, que corresponde a la publicación de eventos desde Y-Trace hacia el Bus.
* **Criterios de Validación:** No se listan cargas ya despachadas; visualización clara de la carga consolidada en andén sin desglose de pedidos minoristas individuales.
* **Fuente o Evidencia:** [[Funciones_Web]] (Sección 1.2) | [[Flujo_Web]] | [[SPY]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Alineado al alcance B2B: Y-Trace no interactúa con cajas minoristas de catálogo de venta directa, sino con la unidad de carga preparada para traslado punto a punto.

---

#### RF008 — Generación de Código Único de Activación para Seguimiento
* **ID:** `RF008`
* **Caso de Prueba Asociado:** `CP-COD-01`
* **Nombre:** Generación de Código Único de Activación para Seguimiento.
* **Descripción:** El sistema debe generar un Código Único de Activación alfanumérico asociado a un despacho ya existente y disponible para seguimiento. El código será entregado al conductor para que pueda ingresar a la PWA e iniciar el registro del seguimiento. Ante contingencia de dispositivo durante un despacho `EN_RUTA`, podrá generarse un código de recuperación de un solo uso, bajo autorización del Supervisor, sin crear ni modificar el despacho.
* **Objetivo de Negocio:** Habilitar de manera segura y temporal el acceso del conductor al seguimiento digital del despacho sin recurrir a cuentas fijas ni contraseñas permanentes.
* **Actor:** Supervisor de Distribución / Sistema Backend.
* **Módulo:** Módulo de Códigos y Activación.
* **Qué debe hacer el sistema:** Recibir la solicitud sobre un despacho disponible para seguimiento, verificar que exista y sea elegible para seguimiento, registrar el hito interno de control previo, generar una cadena pseudoaleatoria criptosegura de 8 caracteres alfanuméricos, verificar unicidad activa, asociarla al despacho y presentarla en pantalla para su entrega al conductor. En contingencia de dispositivo durante `EN_RUTA`, permitir al Supervisor solicitar un código de recuperación de un solo uso, revocar el token anterior y registrar la justificación sin crear un nuevo despacho.
* **Entradas:** Identificador del despacho disponible para seguimiento; o solicitud extraordinaria de recuperación de dispositivo para un despacho `EN_RUTA`, con justificación y confirmación del Supervisor.
* **Resultado Esperado:** Despacho existente con hito interno de control previo y Código de Activación vigente, listo para ser utilizado por el conductor; en contingencia, nuevo código de recuperación de un solo uso asociado al mismo despacho, con token anterior revocado y trazabilidad histórica intacta.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Separación Dual de Mecanismos de Acceso).
* **Dependencias:** [[RF007]].
* **Criterios de Validación:** El código solo puede generarse para un despacho existente y elegible para seguimiento; al generarlo correctamente se registra el hito interno de control previo. Cada código activo es unívoco. Un código de recuperación solo puede emitirse para un despacho `EN_RUTA`, requiere autorización del Supervisor, es de un solo uso y no modifica el histórico del viaje. Y-Trace no crea, programa, asigna ni cancela el despacho.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 1 y 6) | [[Funciones_Web]] | [[Flujo_Principal]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el mecanismo que conecta un despacho ya existente con la operación móvil de seguimiento. No constituye una función de programación, asignación ni autorización de salida logística.

---

#### RF009 — Cierre Administrativo Forzado de Despacho en Contingencia
* **ID:** `RF009`
* **Caso de Prueba Asociado:** `CP-DESP-04`
* **Nombre:** Cierre Administrativo Forzado de Despacho en Contingencia.
* **Descripción:** El sistema debe permitir al Supervisor de Distribución forzar administrativamente el cierre del seguimiento de un despacho en ruta bajo justificación formal registrada en bitácora ante eventos de siniestro mayor o fuerza mayor.
* **Objetivo de Negocio:** Resolver administrativamente seguimientos interrumpidos por pérdida total, confiscación o desastres naturales donde el conductor no pueda operar la PWA.
* **Actor:** Supervisor de Distribución.
* **Módulo:** Módulo de Gestión de Despachos.
* **Qué debe hacer el sistema:** Solicitar confirmación con contraseña del Supervisor, exigir la descripción detallada de la causal de fuerza mayor, forzar el cierre del seguimiento en `FINALIZADO`, revocar la sesión móvil asociada cuando exista y generar el evento administrativo de cierre para su publicación mediante [[RF029]]. El resumen consolidado de trazabilidad correspondiente se gestiona mediante [[RF031]].
* **Entradas:** Identificador de despacho, justificación detallada y credenciales de confirmación del supervisor.
* **Resultado Esperado:** Despacho cerrado administrativamente, sesión móvil revocada y notificación de evento extraordinario emitida.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Condiciones de Cierre).
* **Dependencias:** [[RF008]], [[RF011]], [[RF029]], [[RF033]].
* **Criterios de Validación:** Requiere perfil de Supervisor o superior; genera entrada crítica en la bitácora inmutable de auditoría ([[RF006]]).
* **Fuente o Evidencia:** [[Ciclo_De_Vida_Del_Despacho]] (Sección 1) | [[Funciones_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Mecanismo de contingencia para evitar que el seguimiento quede permanentemente abierto cuando el conductor no puede completar el cierre desde la PWA.

---

### Grupo 3: Módulo Móvil de Operación en Ruta, Llegada y Entrega (PWA Android)

#### RF010 — Activación de la Operación Móvil mediante Código Único
* **ID:** `RF010`
* **Caso de Prueba Asociado:** `CP-MOB-01`
* **Nombre:** Activación de la Operación Móvil mediante Código Único.
* **Descripción:** El sistema debe permitir al conductor activar la operación móvil del despacho habilitado para seguimiento ingresando el Código Único de Activación en la aplicación PWA en Android, sin solicitar usuario ni contraseña.
* **Objetivo de Negocio:** Proveer un mecanismo de inicio de jornada ágil y sin fricciones para transportistas terceros en el andén de Lurín.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Activación (PWA Android).
* **Qué debe hacer el sistema:** Solicitar el código de 8 caracteres en la pantalla inicial de la PWA, transmitirlo al backend mediante canal seguro TLS 1.3, validar existencia, elegibilidad para activación, vigencia y no consumo del código, y descargar la información del despacho a la persistencia local del dispositivo.
* **Entradas:** Código alfanumérico digitado por el conductor, huella digital del dispositivo Android.
* **Resultado Esperado:** PWA habilitada con la hoja de ruta y despacho activo desplegado en pantalla.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Acceso Móvil sin Contraseñas).
* **Dependencias:** [[RF008]].
* **Criterios de Validación:** En el flujo normal se rechazan códigos erróneos, expirados, ya consumidos o asociados a despachos no elegibles para seguimiento. En contingencia, un código de recuperación solo es válido para el despacho `EN_RUTA` indicado por el Supervisor, es de un solo uso y revoca el token anterior. El bloqueo por fuerza bruta se aplica a ambos tipos de código ([[RF032]]).
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 1 y 6) | [[Funciones_Movil]] (Sección 1).
* **Estado:** Confirmado.
* **Análisis Crítico:** Punto de entrada del transportista a la operativa digital, diseñado para una curva de aprendizaje mínima y alta tolerancia a rotación de conductores.

---

#### RF011 — Establecimiento y Mantenimiento de Sesión Operativa Móvil
* **ID:** `RF011`
* **Caso de Prueba Asociado:** `CP-MOB-02`
* **Nombre:** Establecimiento y Mantenimiento de Sesión Operativa Móvil.
* **Descripción:** El sistema debe establecer y mantener una sesión operativa móvil local vinculada al despacho, vehículo, conductor y dispositivo Android mientras el viaje esté activo, revocándola automáticamente al concluir el despacho. Debe permitir el reemplazo controlado del dispositivo en contingencia, sin crear un nuevo despacho ni alterar los eventos históricos.
* **Objetivo de Negocio:** Asociar de manera unívoca cada coordenada GPS y evento de entrega al despacho correspondiente, impidiendo emisiones no autorizadas tras el cierre.
* **Actor:** Sistema Backend / PWA Android.
* **Módulo:** Módulo Móvil de Activación (PWA Android).
* **Qué debe hacer el sistema:** Tras la validación exitosa del Código de Activación ([[RF010]]), generar un token de sesión operativa efímero firmado criptográficamente; almacenarlo en memoria/IndexedDB del navegador Android; inyectarlo en cada payload de telemetría y evento; y revocarlo al pasar el despacho a `FINALIZADO` o `DESPACHO_CANCELADO`. Ante una recuperación autorizada de dispositivo en `EN_RUTA`, revocar el token anterior, emitir el nuevo token para el mismo despacho y conservar todos los eventos históricos.
* **Entradas:** Validación conforme de activación de despacho.
* **Resultado Esperado:** Contexto de sesión operativo activo y unívocamente asociado al despacho; en recuperación, nuevo contexto de sesión para el dispositivo sustituto sin reiniciar el viaje.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Condición Mandatoria de Transmisión).
* **Dependencias:** [[RF010]].
* **Criterios de Validación:** La PWA no permite operar sin sesión activa; el backend descarta tokens revocados, expirados o pertenecientes a despachos en estado `FINALIZADO`/`DESPACHO_CANCELADO` (HTTP 403). En recuperación, solo un token nuevo y vigente puede emitir telemetría después de revocarse el anterior.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 6 y 7) | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Garantiza la inviolabilidad de las sesiones móviles temporales y asegura que el conductor no transmita datos fuera del horario y ámbito del viaje autorizado.

---

#### RF012 — Visualización de Información del Despacho y Punto de Destino
* **ID:** `RF012`
* **Caso de Prueba Asociado:** `CP-MOB-03`
* **Nombre:** Visualización de Información del Despacho y Punto de Destino.
* **Descripción:** El sistema debe presentar al conductor en la interfaz de la PWA la información operativa del despacho habilitado para seguimiento: código de despacho, punto de destino de distribución (centro o agencia), dirección de llegada, referencias geográficas, observaciones de ruta y estado actual.
* **Objetivo de Negocio:** Guiar la ejecución física del traslado de la carga hacia la instalación correcta sin sobrecargar al conductor con datos comerciales confidenciales.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Ruta (PWA Android).
* **Qué debe hacer el sistema:** Renderizar en la pantalla principal del móvil el resumen del despacho de distribución: origen (CD Lurín), destino (agencia/punto de distribución), horario planificado, volumen/bultos y notas de traslado.
* **Entradas:** Datos del despacho sincronizados en almacenamiento local IndexedDB.
* **Resultado Esperado:** Pantalla limpia y de alto contraste con la información necesaria para el traslado intercentros.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Mínimo Privilegio).
* **Dependencias:** [[RF010]], [[RF011]].
* **Criterios de Validación:** La PWA no muestra precios, nombres de consultoras residenciales ni datos B2C; la información está disponible tanto con red como en modo fuera de línea.
* **Fuente o Evidencia:** [[Funciones_Movil]] (Sección 1.2) | [[Despacho_Como_Unidad_Logistica]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Refleja con fidelidad el alcance B2B: el conductor traslada carga entre instalaciones de distribución y solo requiere datos del punto de llegada y condiciones de ruta.

---

#### RF013 — Registro de Inicio de Traslado del Despacho
* **ID:** `RF013`
* **Caso de Prueba Asociado:** `CP-MOB-04`
* **Nombre:** Registro de Inicio de Traslado del Despacho.
* **Descripción:** El sistema debe permitir al conductor registrar el inicio del traslado de la carga mediante el botón "Iniciar Despacho", cambiando el estado a `EN_RUTA` cuando la sesión móvil sea válida y el despacho esté habilitado para seguimiento.
* **Objetivo de Negocio:** Marcar el hito formal de salida física del vehículo con la carga desde el CD Lurín e iniciar el seguimiento de viaje.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Ruta (PWA Android).
* **Qué debe hacer el sistema:** Verificar que el despacho esté habilitado para seguimiento y que la sesión móvil sea válida; capturar la pulsación del botón, obtener la coordenada GPS actual, estampar fecha y hora, transicionar el estado a `EN_RUTA`, persistirlo localmente, iniciar el servicio de muestreo GPS en segundo plano y transmitir la actualización al backend.
* **Entradas:** Pulsación en "Iniciar Despacho", coordenadas GPS de salida.
* **Resultado Esperado:** Despacho en estado `EN_RUTA`, tracking GPS activo y evento de salida registrado localmente y enviado al backend/Bus cuando exista conectividad.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Condición Mandatoria de Transmisión).
* **Dependencias:** [[RF010]], [[RF011]].
* **Criterios de Validación:** El botón solo es accionable si existe una sesión móvil válida y el despacho está habilitado para seguimiento; tras pulsarlo se deshabilita para evitar dobles disparos.
* **Fuente o Evidencia:** [[Flujo_Conductor]] | [[Ciclo_De_Vida_Del_Despacho]] (Sección 1).
* **Estado:** Confirmado.
* **Análisis Crítico:** Marca la frontera temporal entre la estiba en andén de salida y el inicio del traslado en carretera.

---

#### RF014 — Captura Periódica de Telemetría GPS durante el Traslado
* **ID:** `RF014`
* **Caso de Prueba Asociado:** `CP-GPS-01`
* **Nombre:** Captura Periódica de Telemetría GPS durante el Traslado.
* **Descripción:** El sistema debe capturar periódicamente coordenadas GPS en segundo plano desde el dispositivo Android durante el estado `EN_RUTA`, registrando latitud, longitud, precisión y estampa de tiempo.
* **Objetivo de Negocio:** Proveer la traza histórica del recorrido y la última ubicación conocida del camión para supervisión basada en la última telemetría disponible en la Torre de Control.
* **Actor:** Sistema Móvil (PWA Android) / Conductor.
* **Módulo:** Módulo Móvil de Telemetría GPS.
* **Qué debe hacer el sistema:** Invocar la API de geolocalización de Android con un muestreo operativo optimizado de un punto cada 10 minutos mientras el despacho permanezca en `EN_RUTA`; los cambios de estado e incidencias críticas se capturan de inmediato. Los puntos se empaquetan junto con el token de sesión operativa, se encolan localmente y se transmiten al backend cuando existe conectividad.
* **Entradas:** Señal satelital GPS obtenida mediante servicios de geolocalización de Android.
* **Resultado Esperado:** Secuencia de puntos de telemetría georreferenciados asociados al despacho activo.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Georreferenciación de Arribo, Condición Mandatoria de Transmisión).
* **Dependencias:** [[RF013]].
* **Criterios de Validación:** La captura GPS se detiene inmediatamente si el despacho pasa a estado `FINALIZADO`; no consume más del 15% de batería en 8 horas (RNF009).
* **Fuente o Evidencia:** [[Funcionamiento_GPS]] | [[Restricciones_Tecnicas]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el núcleo funcional del tracking en ruta. Se ejecuta en segundo plano bajo las restricciones de optimización energética de Android.

---

#### RF015 — Registro de Llegada al Punto de Destino por Geocerca o Registro Manual
* **ID:** `RF015`
* **Caso de Prueba Asociado:** `CP-ENT-01`
* **Nombre:** Registro de Llegada al Punto de Destino por Geocerca o Registro Manual.
* **Descripción:** El sistema debe registrar el arribo físico del vehículo al punto de distribución mediante detección automática de ingreso a geocerca perimétrica (o mediante pulsación manual del conductor del botón "Llegué a Destino" en la PWA), cambiando el estado a `EN_DESTINO` y capturando de forma atómica fecha, hora y ubicación GPS exacta. **Este cambio de estado solo acredita la llegada física y NO confirma la entrega de la carga (articulado con RN-PV-01).**
* **Objetivo de Negocio:** Auditar la presencia física del vehículo en el punto de distribución y registrar el hito exacto de arribo sirviendo la coordenada GPS como evidencia geoespacial directa.
* **Actor:** Conductor / Sistema Móvil.
* **Módulo:** Módulo Móvil de Llegada (PWA Android).
* **Qué debe hacer el sistema:** Al detectar el cruce de geocerca en destino o al presionar "Llegué a Destino", capturar inmediatamente la coordenada GPS del dispositivo Android, generar la estampa de tiempo del evento, cambiar el estado del despacho a `EN_DESTINO`, persistir localmente y transmitir la actualización al backend cuando exista conectividad.
* **Entradas:** Detección de geocerca satelital o pulsación manual en "Llegué a Destino", coordenadas GPS del arribo.
* **Resultado Esperado:** Despacho transicionado a estado `EN_DESTINO`, con coordenadas y hora de llegada auditadas; el sistema habilita el formulario de entrega y comienza a computar la ventana operativa de 60 minutos definida en RN-PV-01; esta ventana es independiente del SLA de integración de 30 minutos hacia el Bus.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Georreferenciación de Arribo, RN-PV-01).
* **Dependencias:** [[RF013]], [[RF014]].
* **Criterios de Validación:** La geocerca o el botón manual acreditan exclusivamente la llegada física (`EN_DESTINO`); no sustituyen la confirmación formal de entrega ni liberan la carga sin la acción explícita de RF016.
* **Fuente o Evidencia:** [[Eventos_E_Incidencias]] | [[Funciones_Movil]] | Directriz de Alcance Y-Trace.
* **Estado:** Confirmado.
* **Análisis Crítico:** Reemplaza el anterior concepto de "en puerta residencial", formalizando la llegada física a la instalación receptora y delimitando con precisión que la presencia física precede pero no sustituye a la entrega.

---

#### RF016 — Confirmación de Recepción / Entrega del Despacho Completo
* **ID:** `RF016`
* **Caso de Prueba Asociado:** `CP-ENT-02`
* **Nombre:** Confirmación de Recepción / Entrega del Despacho Completo en Destino.
* **Descripción:** El sistema debe exigir al conductor registrar manualmente la confirmación de recepción y entrega del despacho completo en el punto de destino, registrando fecha, hora y ubicación GPS y transicionando el estado del despacho a `ENTREGADO`. La evidencia fotográfica de respaldo es complementaria y opcional ([[RF018]]) y nunca condiciona la confirmación de entrega.
* **Objetivo de Negocio:** Formalizar la culminación exitosa y auditada de la entrega de la carga en destino sin fricciones innecesarias pero con respaldo documental y visual inmutable.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Entrega (PWA Android).
* **Qué debe hacer el sistema:** Presentar pantalla de confirmación de entrega del despacho completo ofreciendo la asociación de evidencia operativa y, cuando corresponda, fotografía complementaria opcional ([[RF018]]); al pulsar "Confirmar Entrega", capturar coordenadas GPS y timestamp atómico, marcar el despacho como `ENTREGADO`, persistir el evento localmente y encolarlo para su transmisión al backend y al Bus corporativo cuando exista conectividad.
* **Entradas:** Acción consciente de confirmación manual de entrega en la PWA Android, evidencias operativas disponibles ([[RF018]]), coordenadas GPS.
* **Resultado Esperado:** Evento `ENTREGADO` registrado de forma inmutable con fecha, hora, ubicación GPS y evidencia operativa disponible; cuando exista fotografía de respaldo, esta queda almacenada en Cloud Storage y vinculada mediante una referencia segura. La ventana de alerta de 60 min queda cerrada y el despacho queda listo para liquidación y cierre.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Georreferenciación de Arribo, Condiciones de Cierre, RN-PV-01).
* **Dependencias:** [[RF015]], [[RF018]].
* **Criterios de Validación:** No se exige DNI residencial ni firmas de terceros particulares. La acción manual del conductor es obligatoria; la evidencia del evento se registra con los datos operativos disponibles y la fotografía es opcional.
* **Fuente o Evidencia:** [[Despacho_Como_Unidad_Logistica]] | [[Ciclo_De_Vida_Del_Despacho]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Modela la recepción de la carga completa en el punto de distribución, garantizando que ninguna geocerca cierre la entrega automáticamente sin la acción manual de confirmación del transportista.

---

#### RF017 — Registro de No Entrega o Rechazo de Despacho en Destino
* **ID:** `RF017`
* **Caso de Prueba Asociado:** `CP-ENT-03`
* **Nombre:** Registro de No Entrega o Rechazo de Despacho en Destino.
* **Descripción:** El sistema debe permitir al conductor registrar la no entrega o rechazo del despacho en el punto de destino cuando existan impedimentos operativos tipificados, registrando causal, estampa de tiempo y coordenadas GPS, cambiando el estado a `NO_ENTREGADO`.
* **Objetivo de Negocio:** Sustentar formalmente los motivos por los cuales una carga no pudo ser recepcionada en la agencia o punto de destino (local cerrado, rechazo formal por discrepancia externa, imposibilidad física de acceso).
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Entrega (PWA Android).
* **Qué debe hacer el sistema:** Desplegar catálogo de motivos tipificados de no entrega en destino (`PUNTO_CERRADO`, `RECHAZO_POR_DISCREPANCIA`, `ACCESO_BLOQUEADO`, `FUERZA_MAYOR`); requerir selección de causal y descripción complementaria; capturar coordenadas GPS y marcar el despacho como `NO_ENTREGADO`.
* **Entradas:** Selección de causal tipificada, descripción breve, coordenadas GPS del arribo.
* **Resultado Esperado:** Evento `NO_ENTREGADO` registrado con causal justificada y coordenadas del intento de entrega.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Georreferenciación de Arribo, Condiciones de Cierre).
* **Dependencias:** [[RF015]].
* **Criterios de Validación:** No se permite registrar el fallo sin seleccionar un motivo del catálogo; se descarta la obligación de fotos de fachadas de casas familiares.
* **Fuente o Evidencia:** [[Eventos_E_Incidencias]] | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Adapta el concepto de fallo de entrega al ámbito de distribución intercentros, asegurando que las contingencias en puntos receptores queden tipificadas.

---

#### RF018 — Gestión de Evidencias Operativas del Despacho
* **ID:** `RF018`
* **Caso de Prueba Asociado:** `CP-EVI-01`
* **Nombre:** Gestión de Evidencias Operativas del Despacho.
* **Descripción:** El sistema debe permitir registrar y asociar al despacho las evidencias generadas durante su operación, incluyendo tipo de evento, fecha y hora, actor o sesión operativa, ubicación GPS cuando esté disponible y, de forma complementaria, una fotografía opcional.
* **Objetivo de Negocio:** Garantizar la trazabilidad de los hechos ocurridos durante el traslado y la recepción sin depender exclusivamente de una evidencia fotográfica.
* **Actor:** Conductor / Sistema.
* **Módulo:** Módulo Móvil de Evidencias (PWA Android).
* **Qué debe hacer el sistema:** Generar automáticamente el registro base de evidencia para los eventos relevantes y permitir adjuntar una fotografía complementaria cuando las condiciones operativas lo permitan. La foto se comprime localmente, se conserva como archivo externo en Cloud Storage y la base de datos mantiene únicamente su identificador y referencia segura.
* **Entradas:** Evento operativo, fecha y hora de captura, contexto de despacho, sesión o actor, coordenadas GPS cuando estén disponibles y fotografía opcional.
* **Resultado Esperado:** Evidencia operativa inmutable asociada al despacho; si existe fotografía, queda vinculada mediante una referencia segura a Cloud Storage.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla B2B de Evidencia Operativa).
* **Dependencias:** [[RF015]], [[RF016]], [[RF017]], [[RF019]].
* **Criterios de Validación:** Todo evento operativo relevante conserva su registro base; la ausencia de fotografía no bloquea `ENTREGADO`, `NO_ENTREGADO` ni una incidencia; cuando exista foto, no se almacena como BLOB en la base de datos.
* **Fuente o Evidencia:** [[Restricciones_Tecnicas]] | Directriz de Evidencias Y-Trace.
* **Estado:** Confirmado.
* **Análisis Crítico:** Convierte la evidencia en un conjunto de datos operativos y deja la fotografía como respaldo complementario, evitando que una emergencia o daño del dispositivo invalide la trazabilidad del hecho.

---

#### RF019 — Reporte de Incidencias en Ruta durante el Traslado
* **ID:** `RF019`
* **Caso de Prueba Asociado:** `CP-INC-01`
* **Nombre:** Reporte de Incidencias en Ruta durante el Traslado.
* **Descripción:** El sistema debe permitir al conductor reportar siniestros o percances viales tipificados ocurridos durante el traslado hacia el destino, registrando tipo de incidencia, descripción y ubicación GPS cuando esté disponible, con fotografía complementaria opcional.
* **Objetivo de Negocio:** Alertar oportunamente contingencias en carretera que comprometan la integridad de la carga, la seguridad del conductor o los tiempos de llegada planificados.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Incidencias (PWA Android).
* **Qué debe hacer el sistema:** Presentar menú rápido de reporte de imprevistos con tipos tipificados (`FALLA_MECANICA`, `ACCIDENTE_TRANSITO`, `BLOQUEO_VIA`, `ASALTO_ROBO`, `CLIMA_ADVERSO`); capturar coordenadas GPS de forma atómica, permitir adjuntar foto opcional y transmitir la alerta con prioridad alta al backend.
* **Entradas:** Selección del tipo de incidencia, descripción, fotografía opcional, coordenadas GPS.
* **Resultado Esperado:** Incidencia registrada en la PWA, transmitida al backend y reflejada de inmediato en la consola de la Torre de Control con alerta sonora y visual.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Tipificación y Alerta de Incidencias).
* **Dependencias:** [[RF013]].
* **Criterios de Validación:** La incidencia no interrumpe el tracking GPS si el vehículo continúa en movimiento; genera notificación prioritaria al Bus corporativo ([[RF030]]).
* **Fuente o Evidencia:** [[Eventos_E_Incidencias]] | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Otorga un canal de auxilio y reporte inmediato desde cabina, permitiendo a la Torre de Control activar protocolos de contingencia logística.

---

#### RF020 — Finalización y Cierre del Despacho con Revocación de Sesión
* **ID:** `RF020`
* **Caso de Prueba Asociado:** `CP-MOB-05`
* **Nombre:** Finalización y Cierre del Despacho con Revocación de Sesión.
* **Descripción:** El sistema debe permitir al conductor finalizar formalmente el despacho una vez resuelta la entrega en destino (`ENTREGADO` o `NO_ENTREGADO`) y verificado el vaciado de la cola local de sincronización, pasando el estado a `FINALIZADO` y revocando inmediatamente la sesión operativa móvil y el código de activación.
* **Objetivo de Negocio:** Liquidar formalmente la hoja de traslado y cesar de forma terminante la emisión de telemetría y eventos desde el dispositivo móvil.
* **Actor:** Conductor / Sistema Backend.
* **Módulo:** Módulo Móvil de Cierre (PWA Android).
* **Qué debe hacer el sistema:** Al presionar "Finalizar Despacho", verificar que el despacho tenga resolución final (`ENTREGADO` o `NO_ENTREGADO`) y que la cola local de IndexedDB esté vacía; transicionar el estado a `FINALIZADO`; ordenar al backend la revocación inmediata del token de sesión; purgar los datos temporales locales en el móvil y apagar el tracking GPS.
* **Entradas:** Acción de finalización por el conductor en la PWA Android.
* **Resultado Esperado:** Despacho en estado `FINALIZADO`, sesión móvil revocada y aplicación PWA retornada a la pantalla inicial de activación.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Condiciones de Cierre, Condición Mandatoria de Transmisión).
* **Dependencias:** [[RF016]] o [[RF017]], [[RF022]].
* **Criterios de Validación:** No se puede finalizar si restan eventos pendientes por sincronizar en la cola local; tras finalizar, cualquier petición móvil con ese token es rechazada (HTTP 403).
* **Fuente o Evidencia:** [[Ciclo_De_Vida_Del_Despacho]] (Sección 1) | [[Flujo_Conductor]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Marca el término de la responsabilidad operativa sobre el viaje, garantizando que el conductor no mantenga sesiones activas fuera de la jornada de despacho.

---

### Grupo 4: Módulo Móvil Offline-First y Sincronización en Android

#### RF021 — Almacenamiento Local Offline en Dispositivo Android (IndexedDB)
* **ID:** `RF021`
* **Caso de Prueba Asociado:** `CP-OFF-01`
* **Nombre:** Almacenamiento Local Offline en Dispositivo Android.
* **Descripción:** El sistema debe almacenar localmente en el dispositivo Android estados, coordenadas GPS, incidencias y fotos opcionales cuando se interrumpa la cobertura celular en ruta (arquitectura *Offline-First*).
* **Objetivo de Negocio:** Garantizar la continuidad operativa ininterrumpida en carreteras interprovinciales o tramos montañosos sin señal de telecomunicaciones.
* **Actor:** Sistema Móvil (PWA Android).
* **Módulo:** Módulo Móvil de Persistencia Local.
* **Qué debe hacer el sistema:** Persistir cada acción del conductor (llegada, entrega, foto, incidencia) y cada punto GPS en la base de datos local IndexedDB del navegador Android en menos de 500 ms, sin depender de la disponibilidad de red celular.
* **Entradas:** Transacciones operativas y paquetes de telemetría GPS generados en el móvil.
* **Resultado Esperado:** Registro inmediato en almacenamiento local con estampa temporal de captura (`captured_at`) y asignación de identificador único UUIDv4.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Consistencia y Persistencia Offline-First).
* **Dependencias:** [[RF011]].
* **Criterios de Validación:** La aplicación opera con normalidad con el dispositivo en "Modo Avión", reteniendo al menos 500 eventos sin degradación funcional ni pérdida de datos (umbral mínimo de prueba y aceptación, no límite máximo de almacenamiento) (RNF013).
* **Fuente o Evidencia:** [[Offline_First]] | [[Restricciones_Tecnicas]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Es el requerimiento arquitectónico que permite la viabilidad técnica de Y-Trace en la accidentada geografía de carreteras peruanas.

---

#### RF022 — Sincronización Automática en Segundo Plano (FIFO)
* **ID:** `RF022`
* **Caso de Prueba Asociado:** `CP-OFF-02`
* **Nombre:** Sincronización Automática en Segundo Plano.
* **Descripción:** El sistema debe sincronizar automáticamente en orden cronológico estricto (FIFO) los eventos y telemetría retenidos localmente tan pronto se restablezca la conectividad de red celular o Wi-Fi.
* **Objetivo de Negocio:** Evitar la pérdida de datos y actualizar el backend y la Torre de Control sin exigir acciones manuales complejas al transportista.
* **Actor:** Sistema Móvil (Service Worker en Android) / Sistema Backend.
* **Módulo:** Módulo Móvil de Sincronización.
* **Qué debe hacer el sistema:** Registrar el estado de red y eventos de Background Sync; al detectar conectividad activa, leer la cola de IndexedDB por despacho en orden cronológico y transmitir los registros mediante un endpoint de ingesta por lotes (`POST /api/v1/sync/batch`). Cada lote debe enviarse en orden de `captured_at`, procesarse secuencialmente en backend y confirmarse de forma idempotente; solo los registros aceptados se eliminan de la cola local.
* **Entradas:** Detección de conectividad de red activa (4G/5G/Wi-Fi) y lote ordenado de eventos pendientes por despacho.
* **Resultado Esperado:** Cola local vaciada progresivamente y servidor central actualizado con los hechos en el orden en que ocurrieron en campo.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Consistencia y Persistencia Offline-First).
* **Dependencias:** [[RF021]].
* **Criterios de Validación:** El backend procesa cada lote respetando el orden por `captured_at`, conserva la estampa original, evita carreras entre eventos del mismo despacho y descarta duplicados mediante UUIDv4 (RNF014).
* **Fuente o Evidencia:** [[Offline_First]] (Sección 3 y 4) | [[Componentes_Aplicacion]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Automatiza el flujo de subida de información y garantiza la consistencia eventual entre el móvil y la base de datos central.

---

#### RF023 — Indicador Visual de Estado de Sincronización Local
* **ID:** `RF023`
* **Caso de Prueba Asociado:** `CP-OFF-03`
* **Nombre:** Indicador Visual de Estado de Sincronización Local.
* **Descripción:** El sistema debe presentar al conductor en la interfaz de la PWA un indicador visual permanente del estado de sincronización (verde: al día; amarillo: contador de transacciones pendientes en cola).
* **Objetivo de Negocio:** Brindar retroalimentación visual al transportista sobre la persistencia y subida efectiva de sus datos antes de abandonar zonas de cobertura o finalizar el viaje.
* **Actor:** Conductor.
* **Módulo:** Módulo Móvil de Sincronización.
* **Qué debe hacer el sistema:** Consultar dinámicamente el conteo de elementos en la cola de IndexedDB; si es 0, desplegar icono verde ("Sincronizado"); si es mayor a 0, desplegar icono amarillo con el número de elementos pendientes (ej. "Cola: 3").
* **Entradas:** Conteo de registros pendientes en la cola local de IndexedDB.
* **Resultado Esperado:** Indicador visual permanente en la cabecera de la PWA que actualiza su estado cuando cambia el contenido de la cola local.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Consistencia y Persistencia Offline-First).
* **Dependencias:** [[RF021]], [[RF022]].
* **Criterios de Validación:** Si el conductor intenta finalizar el despacho con eventos en cola, el sistema le advierte que debe esperar la subida completa ([[RF020]]).
* **Fuente o Evidencia:** [[Offline_First]] | [[Funciones_Movil]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Elemento clave de usabilidad que previene liquidaciones truncadas por pérdidas de conexión.

---

### Grupo 5: Módulo Web de Monitoreo, Auditoría y Control Operativo

#### RF024 — Consulta y Seguimiento Operativo de Despachos en la Web
* **ID:** `RF024`
* **Caso de Prueba Asociado:** `CP-MON-01`
* **Nombre:** Consulta y Seguimiento Operativo de Despachos en la Web.
* **Descripción:** El sistema debe permitir al Supervisor y Jefe de Distribución consultar el avance y detalle de los despachos activos y cerrados mediante una grilla operativa estructurada, visualizando tiempos de traslado origen-destino, tiempos de permanencia y estado general del viaje con semaforización visual (Verde: `EN_RUTA`; Amarillo: `EN_DESTINO`; Rojo: `CON_INCIDENCIA`).
* **Objetivo de Negocio:** Habilitar el control operacional y seguimiento de productividad de las rutas troncales e interurbanas en tiempo real.
* **Actor:** Supervisor de Distribución / Jefe de Distribución.
* **Módulo:** Módulo Web de Monitoreo de Despachos.
* **Qué debe hacer el sistema:** Proveer tabla interactiva con listado de despachos, desplegando: código, origen, punto de destino, empresa transportista, conductor, placa, hora de salida, hora de llegada estimada y estado; con opciones de filtrado multicriterio y semaforización visual.
* **Entradas:** Criterios de búsqueda (rango de fechas, transportista, estado del despacho, destino).
* **Resultado Esperado:** Listado dinámico filtrado con opciones de exportación y enlace directo al detalle del viaje.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Seguimiento y Trazabilidad).
* **Dependencias:** [[RF013]], [[RF015]].
* **Criterios de Validación:** Respeta el aislamiento multitransportista ([[RF034]]); tiempos de respuesta inferiores a 2 segundos (RNF016).
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Proceso_TOBE]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Permite monitorear el cumplimiento de horarios y la eficiencia del transporte entre sedes sin depender de representaciones cartográficas complejas.

---

#### RF025 — Consulta y Auditoría de Evidencias de Entrega en la Web
* **ID:** `RF025`
* **Caso de Prueba Asociado:** `CP-EVI-02`
* **Nombre:** Consulta y Auditoría de Evidencias de Entrega en la Web.
* **Descripción:** El sistema debe permitir al personal autorizado consultar en la plataforma Web las evidencias asociadas a la entrega del despacho (coordenadas GPS de llegada y entrega cuando estén disponibles, estampa de tiempo y fotografía complementaria opcional alojada en Cloud Storage).
* **Objetivo de Negocio:** Sustentar técnicamente la entrega y recepción de la carga ante auditorías logísticas y liquidaciones con transportistas.
* **Actor:** Supervisor de Distribución / Jefe de Distribución.
* **Módulo:** Módulo Web de Auditoría de Entregas.
* **Qué debe hacer el sistema:** En la vista de detalle del despacho entregado, desplegar la ubicación GPS exacta del arribo, la estampa temporal certificada y, si existe fotografía de respaldo, generar una URL prefirmada temporal (15 min) para su visualización segura desde Cloud Storage.
* **Entradas:** Identificador de despacho entregado (`despacho_id`).
* **Resultado Esperado:** Panel de auditoría con la evidencia geoespacial y visual del despacho disponible para el supervisor.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Georreferenciación de Arribo).
* **Dependencias:** [[RF016]], [[RF018]].
* **Criterios de Validación:** Cero exposición de URLs públicas permanentes de buckets; si no se tomó foto opcional, el panel muestra la evidencia satelital GPS como comprobante conforme.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Despacho_Como_Unidad_Logistica]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Adapta la auditoría a la distribución B2B, erradicando datos personales residenciales y protegiendo los archivos en almacenamiento seguro en la nube.

---

#### RF026 — Gestión y Atención de Alertas de Incidencias en Ruta y Timeout en Destino
* **ID:** `RF026`
* **Caso de Prueba Asociado:** `CP-INC-02`
* **Nombre:** Gestión y Atención de Alertas de Incidencias en Ruta y Timeout en Destino.
* **Descripción:** El sistema debe emitir alertas visuales y sonoras inmediatas en la plataforma Web ante incidencias reportadas en carretera y ante el vencimiento de la ventana operativa de 60 minutos en estado `EN_DESTINO` sin confirmación de entrega (condición de implementación sujeta a la ratificación de RN-PV-01), permitiendo al Supervisor gestionar estados (`REPORTADA`, `EN_ATENCION`, `RESUELTA`), coordinar asistencia o ejecutar el cierre administrativo forzado justificado ([[RF009]]).
* **Objetivo de Negocio:** Activar oportunamente protocolos de asistencia vial y resolver demoras críticas en destino para evitar bloqueos operativos o viajes indefinidamente abiertos.
* **Actor:** Supervisor de Distribución.
* **Módulo:** Módulo Web de Gestión de Incidencias.
* **Qué debe hacer el sistema:** Recibir notificación vía WebSockets ante siniestro vial ([[RF019]]) o ante timeout del backend cuando corresponda. En el móvil, [[RF015]]/[[RF023]] pueden mostrar recordatorios locales durante la ventana de destino, sin crear registros repetitivos. El backend calcula la ventana de 60 minutos a partir del evento de llegada recibido y genera la alerta cuando el despacho continúa sin `ENTREGADO` ni `NO_ENTREGADO`. La consola Web muestra el banner, tono audible, ubicación, bitácora y opciones de contacto o cierre forzado [[RF009]].
* **Entradas:** Evento de incidencia móvil ([[RF019]]), alerta automática del timer de 60 min en destino (RN-PV-01), notas de seguimiento del supervisor.
* **Resultado Esperado:** Alerta atendida y registrada en bitácora; contingencia resuelta o viaje cerrado administrativamente mediante [[RF009]].
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Tipificación y Alerta de Incidencias, RN-PV-01).
* **Dependencias:** [[RF009]], [[RF015]], [[RF019]].
* **Criterios de Validación:** La alerta no puede ser descartada sin que el supervisor registre una causal tipificada o acción en bitácora inmutable.
* **Condición de Implementación:** RF026 está confirmado como requerimiento funcional. La ventana operativa de 60 minutos en `EN_DESTINO` constituye una condición de implementación sujeta a la ratificación de RN-PV-01 con el área de Operaciones.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Eventos_E_Incidencias]] | Protocolo Híbrido RN-PV-01.
* **Estado:** Confirmado.
* **Análisis Crítico:** Unifica bajo el mismo canal reactivo de WebSockets las alertas de carretera y las contingencias por demoras de descarga en los puntos de destino.

---

#### RF027 — Consulta Rápida de Trazabilidad de Despachos
* **ID:** `RF027`
* **Caso de Prueba Asociado:** `CP-TRAC-01`
* **Nombre:** Consulta Rápida de Trazabilidad de Despachos.
* **Descripción:** El sistema debe proveer un buscador rápido por código de despacho o unidad vehicular que presente la cronología completa del traslado (salida de CD, paso por puntos de control en ruta, llegada a destino, entrega y evidencias asociadas).
* **Objetivo de Negocio:** Resolver consultas de seguimiento operacional, auditoría y reclamos de transporte en el primer contacto.
* **Actor:** Supervisor de Distribución / Operador SAC / Jefe de Distribución.
* **Módulo:** Módulo Web de Consulta de Trazabilidad.
* **Qué debe hacer el sistema:** Recibir el código de despacho o placa en una barra de búsqueda indexada; devolver en menos de 2 segundos la línea de tiempo completa del viaje con sus hitos operativos (`DISPONIBLE`, `EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`, `FINALIZADO` y, cuando corresponda, `DESPACHO_CANCELADO` informado externamente) y, cuando exista, la causal y actor de cancelación.
* **Entradas:** Código alfanumérico del despacho o placa vehicular.
* **Resultado Esperado:** Línea de tiempo visual consolidada del traslado con estampas de tiempo y referencias geográficas.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Seguimiento y Trazabilidad).
* **Dependencias:** [[RF013]], [[RF015]], [[RF016]].
* **Criterios de Validación:** Búsqueda indexada sobre histórico de despachos con tiempo de respuesta inferior a 2.0 s (RNF016). No busca por DNI residencial de clientes.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Flujo_Web]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Reorienta el buscador de trazabilidad al código de despacho completo entre sedes, eliminando la búsqueda minorista de pedidos comerciales.

---

#### RF028 — Indicadores y Reportes de Gestión
* **ID:** `RF028`
* **Caso de Prueba Asociado:** `CP-KPI-01`
* **Nombre:** Indicadores y Reportes de Gestión.
* **Descripción:** El sistema debe presentar un tablero ejecutivo de indicadores logísticos con métricas de tiempos de traslado (Lead Time), tasa de entregas conformes y latencia, permitiendo consultar y filtrar la información mediante los criterios disponibles en la vista operativa y exportar los resultados en formato Excel. La evaluación podrá realizarse por empresa transportista y período, manteniendo la exclusión de despachos cancelados de las métricas de Lead Time y puntualidad y contabilizándolos en una métrica separada de cancelación.
* **Objetivo de Negocio:** Permitir el análisis estratégico del nivel de servicio de transporte, la evaluación de contratos de transportistas y la toma de decisiones basada en datos confiables.
* **Actor:** Jefe de Distribución / Gerencia de Operaciones.
* **Módulo:** Módulo Web de Indicadores (Dashboard).
* **Qué debe hacer el sistema:** Calcular y graficar indicadores agregados por empresa transportista y período; permitir consultar y filtrar la información mediante los criterios disponibles en la vista operativa y exportar reportes exclusivamente en formato Excel.
* **Entradas:** Filtros de período (semana, mes, campaña), empresa transportista y criterios de consulta de la vista operativa.
* **Resultado Esperado:** Tablero visual con los indicadores logísticos (Lead Time, entregas conformes y latencia) y exportación exclusivamente en formato Excel.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Cálculo de Indicadores Operativos).
* **Dependencias:** [[RF020]], [[RF024]], [[RF029]].
* **Criterios de Validación:** Datos calculados a partir de eventos inmutables consolidados en la base de datos central. Exportación de reportes disponible exclusivamente en formato Excel sin incluir despachos cancelados en Lead Time ni puntualidad.
* **Fuente o Evidencia:** [[Funciones_Web]] | [[Oportunidad_de_Mejora]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Proporciona a la gerencia logística de Yanbal las métricas clave para evaluar la eficiencia de las empresas transportistas asociadas y exportar resultados exclusivamente a Excel.

---

### Grupo 6: Módulo Backend de Integración Corporativa (ESB)

#### RF029 — Publicación de Eventos de Despacho al Bus de Integración
* **ID:** `RF029`
* **Caso de Prueba Asociado:** `CP-INT-01`
* **Nombre:** Publicación de Eventos de Despacho al Bus de Integración.
* **Descripción:** El sistema debe publicar al Bus de Integración corporativo, en formato JSON estandarizado, los eventos externos del ciclo operativo que correspondan: `EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO` y `DESPACHO_CANCELADO`. El hito interno de control previo se conserva para auditoría y no se publica mediante RF029.
* **Objetivo de Negocio:** Mantener actualizados los sistemas centrales de Yanbal con un SLA de integración de 30 minutos como máximo (reduciendo el atraso histórico de 2 horas).
* **Actor:** Sistema Backend / Bus de Integración (ESB).
* **Módulo:** Módulo Backend de Integración (ESB).
* **Qué debe hacer el sistema:** Al registrarse uno de los eventos integrables del ciclo operativo, construir un payload JSON con esquema canónico (`despacho_id`, `estado`, `timestamp_utc`, `latitud`, `longitud`, `causal` cuando corresponda), encolarlo en el broker de mensajería y transmitirlo al Bus corporativo de forma asíncrona. El hito interno de control previo permanece como hito interno de auditoría y no se publica por este requerimiento.
* **Entradas:** Eventos de estado y eventos administrativos integrables validados en la base de datos (`EN_RUTA`, `EN_DESTINO`, `ENTREGADO`, `NO_ENTREGADO`, `DESPACHO_CANCELADO`).
* **Resultado Esperado:** Mensaje JSON recibido y confirmado por el Bus de Integración de Yanbal.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Integración al Bus: SLA máximo de 30 minutos).
* **Dependencias:** [[RF013]], [[RF015]], [[RF016]], [[RF017]].
* **Criterios de Validación:** Reintento automático con retroceso exponencial ante fallas temporales del Bus; estricto cumplimiento del formato JSON canónico (RNF018).
* **Fuente o Evidencia:** [[Bus_Integracion]] | [[Integracion_Bus_Eventos]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Requisito medular de interoperabilidad para sincronizar la visibilidad logística con el resto del ecosistema tecnológico de Yanbal.

---

#### RF030 — Publicación de Alertas de Incidencias en Ruta al Bus de Integración
* **ID:** `RF030`
* **Caso de Prueba Asociado:** `CP-INT-02`
* **Nombre:** Publicación de Alertas de Incidencias en Ruta al Bus de Integración.
* **Descripción:** El sistema debe publicar notificaciones de incidencias graves en ruta hacia el Bus de Integración corporativo para conocimiento preventivo de sistemas y áreas operativas centrales.
* **Objetivo de Negocio:** Notificar a las áreas de planificación y operaciones sobre retrasos forzados o siniestros viales que comprometen el arribo de la carga.
* **Actor:** Sistema Backend / Bus de Integración (ESB).
* **Módulo:** Módulo Backend de Integración (ESB).
* **Qué debe hacer el sistema:** Al persistirse una incidencia de severidad alta ([[RF019]]), estructurar un payload de alerta con tipo de contingencia, despacho afectado, coordenadas y hora, incorporarlo a la cola de integración y publicarlo en el tópico correspondiente del Bus corporativo conforme al SLA global de 30 minutos.
* **Entradas:** Registro de incidencia reportada por el conductor.
* **Resultado Esperado:** Evento de contingencia publicado en el Bus y consumido por sistemas interesados.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Tipificación y Alerta de Incidencias).
* **Dependencias:** [[RF019]].
* **Criterios de Validación:** El evento se incorpora a la cola de salida y debe ser aceptado por el Bus dentro del SLA único `<= 30 min`, contado desde su recepción en el backend.
* **Fuente o Evidencia:** [[Integracion_Bus_Eventos]] | [[Eventos_E_Incidencias]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Habilita la proactividad institucional frente a incidentes mayores en carreteras.

---

#### RF031 — Envío de Resumen de Trazabilidad del Despacho al Bus
* **ID:** `RF031`
* **Caso de Prueba Asociado:** `CP-INT-03`
* **Nombre:** Envío de Resumen de Trazabilidad del Despacho al Bus.
* **Descripción:** El sistema debe transmitir el resumen consolidado de trazabilidad del despacho al Bus de Integración tras su finalización formal en campo.
* **Objetivo de Negocio:** Permitir disponer de la información final de seguimiento para los procesos posteriores de los sistemas externos.
* **Actor:** Sistema Backend / Bus de Integración (ESB).
* **Módulo:** Módulo Backend de Integración (ESB).
* **Qué debe hacer el sistema:** Al transicionar el despacho a `FINALIZADO`, ya sea mediante el cierre operativo del conductor ([[RF020]]) o el cierre administrativo forzado del Supervisor ([[RF009]]), consolidar la información disponible del seguimiento (hora de inicio, eventos relevantes, estado final, incidencias y kilometraje aproximado cuando esté disponible), generar el mensaje canónico de trazabilidad y enviarlo al Bus corporativo.
* **Entradas:** Evento de cierre formal del despacho (`FINALIZADO`), generado por [[RF020]] o [[RF009]].
* **Resultado Esperado:** Mensaje de trazabilidad transmitido con acuse de recibo conforme.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Condiciones de Cierre).
* **Dependencias:** [[RF020]], [[RF009]].
* **Criterios de Validación:** Solo se transmite después de la finalización formal del despacho. Si el cierre proviene de [[RF009]], se conserva la causal administrativa y la referencia a la bitácora inmutable; la revocación de la sesión se ejecuta cuando exista una sesión activa.
* **Fuente o Evidencia:** [[Integracion_Bus_Eventos]] | [[Ciclo_De_Vida_Del_Despacho]].
* **Estado:** Confirmado.
* **Análisis Crítico:** Entrega información consolidada de trazabilidad a los sistemas externos; cualquier liquidación o conciliación posterior corresponde a dichos sistemas.

---

### Grupo 7: Módulo de Seguridad Móvil y Aislamiento Multitransportista

#### RF032 — Control de Intentos y Bloqueo del Código de Activación Móvil
* **ID:** `RF032`
* **Caso de Prueba Asociado:** `CP-MOB-SEC-01`
* **Nombre:** Control de Intentos y Bloqueo del Código de Activación Móvil.
* **Descripción:** El sistema debe limitar los intentos fallidos de ingreso del código de activación móvil (RF010) a un máximo de 5 intentos, bloqueando e invalidando el código al registrar el quinto fallo consecutivo y alertando al Supervisor.
* **Objetivo de Negocio:** Evitar ataques de fuerza bruta o adivinación sobre el espacio de códigos alfanuméricos de 8 caracteres asignados en andén.
* **Actor:** Conductor / Sistema Backend / Supervisor de Distribución.
* **Módulo:** Módulo Móvil de Activación (PWA Android) + Backend de Seguridad.
* **Qué debe hacer el sistema:** Contar los intentos fallidos por código o identificador de dispositivo; al alcanzar 5 fallos consecutivos, invalidar el código vigente, registrar un evento de alerta de seguridad y notificar al Supervisor para su eventual regeneración justificada.
* **Entradas:** Código ingresado en la PWA, identificador de dispositivo Android.
* **Resultado Esperado:** Código bloqueado tras el quinto intento fallido, con mensaje disuasivo en la PWA y alerta en la consola web del Supervisor. Cualquier intento posterior queda rechazado.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Separación Dual de Mecanismos de Acceso).
* **Dependencias:** [[RF008]], [[RF010]].
* **Criterios de Validación:** Al alcanzar 5 intentos fallidos consecutivos, el código queda invalidado; cualquier intento posterior, incluido el sexto, debe retornar HTTP 403 Forbidden alertando al Supervisor.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 3 y 4).
* **Estado:** Confirmado.
* **Análisis Crítico:** Cierra la asimetría de seguridad móvil frente al bloqueo de cuentas web de RF001, protegiendo los despachos contra accesos maliciosos automatizados.

---

#### RF033 — Unicidad, Vigencia y Caducidad del Código de Activación Móvil
* **ID:** `RF033`
* **Caso de Prueba Asociado:** `CP-COD-02`
* **Nombre:** Unicidad, Vigencia y Caducidad del Código de Activación Móvil.
* **Descripción:** El sistema debe garantizar unicidad activa simultánea a cada Código de Activación generado en RF008 y revocar automáticamente su validez y la de la sesión operativa cuando el despacho llegue a su finalización (`FINALIZADO`) o sea cancelado (`DESPACHO_CANCELADO`) por un sistema externo antes de iniciar el seguimiento.
* **Objetivo de Negocio:** Reducir la ventana de exposición temporal y garantizar que ningún código ni sesión móvil permanezca activo indefinidamente tras la entrega.
* **Actor:** Supervisor de Distribución / Sistema Backend.
* **Módulo:** Módulo de Códigos y Activación.
* **Qué debe hacer el sistema:** Al generar el código en RF008, asegurar que no colisione con ningún otro código activo en el sistema; al marcarse el despacho como `FINALIZADO` o `DESPACHO_CANCELADO`, invalidar inmediatamente el código en la base de datos, impidiendo cualquier canje posterior.
* **Entradas:** Petición de generación de código autorizada por el Supervisor; evento de finalización del seguimiento o cancelación informada externamente.
* **Resultado Esperado:** Código con ciclo de vida estrictamente acotado a la duración del viaje autorizado; rechazo ante cualquier intento de uso tras el cierre.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Acceso Móvil sin Contraseñas, Condición Mandatoria de Transmisión).
* **Dependencias:** [[RF008]], [[RF011]], [[RF020]].
* **Criterios de Validación:** Un código de un seguimiento finalizado o un despacho cancelado externamente no puede ser reutilizado; la base de datos impone restricción de unicidad activa y cualquier intento posterior de canje es rechazado.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] (Sección 3 y 6).
* **Estado:** Confirmado.
* **Nota de Regla Pendiente de Validación (RN-PV-01):** Articulado con la regla híbrida de llegada y entrega (RN-PV-01): la geocerca automática solo marca `EN_DESTINO`, la entrega `ENTREGADO` requiere acción manual del conductor y fotografía opcional ([[RF018]]), y ante la ventana operativa de 60 minutos sin confirmación se dispara alerta al Supervisor ([[RF026]]). Al pasar a `FINALIZADO` (vía conductor [[RF020]] o Supervisor [[RF009]]) o a `DESPACHO_CANCELADO` antes de iniciar el seguimiento, el código y, cuando corresponda, la sesión se revocan de inmediato ([[RF033]]). El SLA de integración de 30 minutos queda definido para este proyecto; la ventana operativa de 60 minutos en destino es una regla de atención independiente y no modifica el SLA de integración.
* **Análisis Crítico:** Garantiza que los códigos alfanuméricos sean secretos efímeros y no credenciales permanentes, cumpliendo con la directriz de seguridad de la arquitectura.

---

#### RF034 — Segmentación de Datos Operativos por Empresa Transportista
* **ID:** `RF034`
* **Caso de Prueba Asociado:** `CP-RBAC-02`
* **Nombre:** Segmentación de Datos Operativos por Empresa Transportista (Aislamiento Multitransportista).
* **Descripción:** El sistema debe restringir la visibilidad de despachos, hojas de ruta, conductores, unidades vehiculares y evidencias a los Supervisores y Jefes de Distribución según la(s) empresa(s) de transporte que tengan asignadas en su perfil web.
* **Objetivo de Negocio:** Evitar que una empresa contratista tercera de transporte visualice, filtre o acceda a la productividad, rutas, incidencias o despachos atendidos por otra empresa transportista competidora.
* **Actor:** Administrador Principal / Supervisor de Distribución / Jefe de Distribución / Sistema Backend.
* **Módulo:** Módulo de Administración de Usuarios + Módulo de Seguridad y Acceso Web.
* **Qué debe hacer el sistema:** Asociar a cada usuario web con rol operativo una o varias entidades `EmpresaTransportista` autorizadas (o comodín `TODAS` para supervisores internos directos de Yanbal). En todas las consultas a nivel de base de datos, API y vistas operativas, aplicar obligatoriamente un filtro por `empresa_transportista_id`.
* **Entradas:** Contexto del usuario autenticado (`usuario_id`, roles, lista de empresas asignadas).
* **Resultado Esperado:** Tableros, listados de monitoreo, reportes y búsquedas filtrados estrictamente por el ámbito de transporte autorizado del usuario.
* **Reglas de Negocio Relacionadas:** [[Reglas_De_Negocio]] (Regla de Confidencialidad Operativa).
* **Dependencias:** [[RF003]], [[RF004]].
* **Criterios de Validación:** Un Supervisor de la Empresa "A" recibe HTTP 403 Forbidden al intentar consultar un despacho perteneciente a la Empresa "B" vía interfaz o API.
* **Fuente o Evidencia:** [[Acceso_Y_Seguridad]] | Arquitectura de Aislamiento Multitenant Lógico.
* **Estado:** Derivado.
* **Nota de Regla Pendiente de Validación (RN-PV-05):** El mecanismo de aislamiento está basado estrictamente en la(s) empresa(s) de transporte asignadas (filtro por `empresa_transportista_id`). Se mantiene como Derivado —no Confirmado— hasta la ratificación formal de las reglas de asignación y modelo comercial con el área de Operaciones.
* **Análisis Crítico:** Protege la confidencialidad comercial entre proveedores logísticos externos contratados por Yanbal en el esquema de flotas tercerizadas.

---

## 3. Análisis Crítico de Candidatos Descartados, Reformulados o Fuera de Alcance

Para asegurar la máxima rigurosidad conceptual y delimitar con total exactitud la frontera de Y-Trace, a continuación se detallan los elementos evaluados que **NO fueron catalogados como Requerimientos Funcionales de Y-Trace** o que fueron depurados de versiones anteriores:

### 3.1 Justificación Formal de Requerimientos Eliminados o Descartados
1. **Eliminación de Entrega Domiciliaria a Familiares y Parentesco (ex-RF019):**
   - *Motivo de Exclusión:* El alcance oficial de Y-Trace está centrado en **despachos de distribución completa entre puntos de distribución (B2B)**. La figura de entrega a familiares directos (cónyuge, hijo, padre) y el registro de grados de parentesco pertenecen al reparto domiciliario minorista de venta directa (B2C), ajeno a la operación de traslado entre instalaciones logísticas.
2. **Reformulación de Validación Algorítmica de Geocerca en Llegada (ex-RF035):**
   - *Motivo de Exclusión:* Se eliminó el motor de validación rígido en backend basado en polígonos, tolerancias estrictas y bloqueo de la operación por distancia. Se mantiene en [[RF015]] una geocerca perimétrica simple como mecanismo auxiliar para detectar el arribo, con el botón manual "Llegué a Destino" como alternativa operativa. La geocerca nunca bloquea la operación ni confirma por sí sola la entrega. La coordenada GPS capturada constituye la evidencia geoespacial del arribo.
3. **Eliminación de Exigencia de DNI Residencial y Firmas de Personas Naturales:**
   - *Motivo de Exclusión:* En despachos completos intercentros, la recepción de la carga la efectúa el personal del punto de destino. Exigir DNI de 8 dígitos de personas naturales era una reminiscencia de entregas a domicilio que distorsionaba el alcance de recepción logística.
4. **Eliminación de Fotografías de Fachada Residencial:**
   - *Motivo de Exclusión:* La fotografía de fachada de casa familiar fue descartada por pertenecer al reparto residencial B2C. En Y-Trace, la evidencia operativa se registra para los eventos del despacho y la **evidencia fotográfica es complementaria y opcional** para `ENTREGADO`, `NO_ENTREGADO` e incidencias; cuando existe, se aloja en Cloud Storage externo sin saturar la base de datos relacional.
5. **Eliminación del Monitoreo Cartográfico de Flota en Ruta en Mapa Interactivo (ex-RF026):**
   - *Motivo de Exclusión:* Se eliminó la capa cartográfica interactiva pesada (Leaflet/Mapbox) para concentrar la Torre de Control en la Grilla Operativa de Despachos ([[RF024]]) y la Línea de Tiempo ([[RF027]]), simplificando la arquitectura y reduciendo la complejidad del frontend sin perder el seguimiento satelital de la flota.
6. **Eliminación de la Bitácora de Auditoría de Intentos Móviles (ex-RF037):**
   - *Motivo de Exclusión:* La seguridad perimetral móvil se consolidó en el bloqueo automático anti-fuerza bruta a los 5 intentos fallidos ([[RF032]]), prescindiendo de la persistencia forense de huellas de dispositivos e IPs móviles en la base de datos central.

### 3.2 Procesos Trasladados Fuera de Alcance (Pertenecen a Sistemas Existentes)
1. **Gestión de Stock, Lotes y Picking Unitario de Mercancía:**
   - *Por qué NO es RF de Y-Trace:* Es responsabilidad exclusiva de **[[SAP_R3]]** y del WMS **[[SPY]]** en el CD de Lurín. Y-Trace inicia cuando la carga ya está preparada y lista en andén.
2. **Cálculo Heurístico y Optimización de Rutas (VRP):**
   - *Por qué NO es RF de Y-Trace:* Corresponde al software TMS **[[Driving]]**. Y-Trace recibe la ruta y secuencia planificada para monitorear su ejecución física real.
3. **Catálogo Comercial, Ventas y Gestión de Clientes/Consultoras:**
   - *Por qué NO es RF de Y-Trace:* Pertenece a **[[Maya]]** y **[[SAP_Commerce]]**. Y-Trace no administra pedidos comerciales individuales ni perfiles de consultoras.
4. **Administración y Maestro de Conductores y Vehículos:**
   - *Por qué NO es RF de Y-Trace:* Y-Trace no da de alta conductores ni empadrona vehículos; esa información maestra reside en los sistemas centrales de flota de Yanbal y se consume como catálogo de solo lectura.
5. **Logística Inversa Física, Devoluciones y Dictamen de Calidad:**
   - *Por qué NO es RF de Y-Trace:* El reingreso físico por esclusas y la inspección pericial ocurren tras el retorno del vehículo en CD, fuera de última milla. Y-Trace se limita a registrar el estado del despacho.
6. **Liquidación Tarifaria de Fletes y Pagos por Kilómetro a Transportistas:**
   - *Por qué NO es RF de Y-Trace:* Corresponde a los contratos de transporte gestionados mediante **[[NSDG]]** y la gerencia de finanzas de Yanbal.

### 3.3 Reglas de Negocio Aisladas de la Lógica Funcional
1. **Regla de Negocio Pendiente de Validación (RN-PV-01: Protocolo Híbrido de Llegada, Confirmación de Entrega y Ventana de Alerta en Destino):**
   - *Clasificación:* Se mantiene como **Regla de Negocio Pendiente de Validación con Operaciones de Yanbal**. La lógica técnica queda resuelta: (a) geocerca automática solo cambia a `EN_DESTINO` (arribo físico, sin validar entrega); (b) confirmación de `ENTREGADO` exige acción manual del conductor y fotografía complementaria opcional ([[RF016]], [[RF018]]); (c) ventana de 60 minutos sin confirmación dispara alerta automática al Supervisor en consola web ([[RF026]]), quien decide contactar al conductor o ejecutar cierre forzado con causal tipificada ([[RF009]]). Aun con la solución técnica cerrada, se mantiene formalmente en estado "Pendiente de validación" hasta la ratificación formal de Operaciones de Yanbal.
2. **"Almacenar fotografías en Cloud Storage y enlaces en base de datos":**
   - *Clasificación:* Es una **Decisión de Arquitectura y Persistencia**. El RF es [[RF018]] (gestión de evidencia operativa con fotografía complementaria opcional); el almacenamiento en buckets S3/GCS y la indexación de URLs en PostgreSQL corresponde al diseño técnico de persistencia.

---

## 4. Exclusiones Explícitas del Alcance (Frontera del Sistema)

Para garantizar la absoluta integridad conceptual y operativa de Y-Trace, se delimitan y consolidan formalmente en esta sección las exclusiones taxativas del proyecto:

1. **No reparto domiciliario minorista (B2C):** Y-Trace no realiza ni gestiona distribución de última milla hacia consumidores finales ni domicilios residenciales.
2. **No entrega a consultoras/familiares con DNI residencial:** Queda formalmente excluida toda captura de grado de parentesco, DNI residencial de personas naturales y firmas de recepción en hogares familiares.
3. **No gestión de despachos unitarios — solo despachos de carga completa punto a punto:** El alcance operacional está restringido estrictamente a despachos de carga completa intercentros (Centro de Distribución Lurín → Punto de Distribución / Agencia receptora). No incluye picking, preparación de pedidos, stock ni bultos unitarios sueltos.
4. **No soporte iOS:** La aplicación móvil para conductores está desarrollada y homologada de manera exclusiva sobre la plataforma Android (versión 8.0 Oreo o superior con Google Chrome v90+). Queda expresamente fuera de alcance el soporte, desarrollo o compatibilidad con el ecosistema iOS / Apple.
