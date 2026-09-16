# Matriz de Trazabilidad: Casos de Uso del Negocio (CUN) vs. Requerimientos Funcionales (RF)

> **Carpeta:** `detalles/Diagrama de Casos de Uso del Negocio (CUN)`  
> **Documento:** `07_MATRIZ_TRAZABILIDAD_CUN_VS_RF.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Marco Metodológico:** Rational Unified Process (RUP) — Ingeniería de Requerimientos y Modelado de Procesos  
>  
> 🔗 **Documentos Vinculados:**  
> - [[00_INDICE_Y_MODELO_GENERAL_CUN]] — Modelo general y diagrama macro  
> - [[01_MATRIZ_DE_REQUERIMIENTOS]] — Matriz maestra unificada (34 RF activos)  
> - [[02_REQUERIMIENTOS_FUNCIONALES]] — Especificación detallada de RFs  

---

## 1. Justificación de la Correspondencia CUN $\leftrightarrow$ RF

En la disciplina de Arquitectura Empresarial, los **Casos de Uso del Negocio (CUN)** definen los procesos operacionales de la empresa (independientemente de la tecnología), mientras que los **Requerimientos Funcionales (RF)** especifican las capacidades de software que el sistema computacional proveerá para automatizar, controlar y dar soporte a dichos procesos.

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                               RELACIÓN DE AGREGACIÓN DE VALOR                           │
├──────────────────────────────────────┬──────────────────────────────────────────────────┤
│ 5 Casos de Uso del Negocio (CUN)     │ 34 Requerimientos Funcionales Activos (RF)       │
├──────────────────────────────────────┼──────────────────────────────────────────────────┤
│ Definen los macro-procesos de valor: │ Son los bloques y servicios de software que      │
│ • CUN-01: Despacho y Salida CD       │ materializan las reglas, transiciones de estado, │
│ • CUN-02: Traslado y Monitoreo       │ telemetría, seguridad perimetral e integración   │
│ • CUN-03: Gestión de Incidencias     │ que permiten a los actores ejecutar los CUN.     │
│ • CUN-04: Entrega y Recepción        │                                                  │
│ • CUN-05: Auditoría y Rendimiento    │                                                  │
└──────────────────────────────────────┴──────────────────────────────────────────────────┘
```

---

## 2. Matriz de Cobertura Bidireccional

A continuación se presenta el mapeo exhaustivo de los 34 Requerimientos Funcionales activos del sistema Y-Trace clasificados según el Caso de Uso del Negocio al cual dan soporte técnico directo:

| ID Req. | Nombre del Requerimiento Funcional | Módulo del Sistema Y-Trace | CUN que le da Soporte | Justificación del Rol en el Negocio |
| :---: | :--- | :--- | :---: | :--- |
| **RF001** | Autenticación de usuarios web mediante credenciales | Seguridad y Acceso Web | **Transversal** | Provee acceso seguro y controlado a la Torre de Control y gestión web. |
| **RF002** | Gestión del ciclo de vida de usuarios web | Administración de Usuarios | **Transversal** | Garantiza la gobernanza centralizada de cuentas autorizadas en Yanbal. |
| **RF003** | Asignación y cambio de roles institucionales | Administración de Usuarios | **Transversal** | Aplica segregación de funciones entre Administrador, Supervisor, Jefe y SAC. |
| **RF004** | Control de acceso basado en roles (RBAC) | Seguridad y Acceso Web | **Transversal** | Restringe menús y transacciones al perfil funcional del colaborador. |
| **RF005** | Control del ciclo de vida de la sesión web | Seguridad y Acceso Web | **Transversal** | Protege terminales desatendidas mediante expiración a los 15 minutos. |
| **RF006** | Registro en bitácora inmutable de auditoría web | Auditoría y Seguridad | **Transversal** | Garantiza trazabilidad pericial y forense sobre cambios administrativos. |
| **RF007** | Consulta y filtrado de despachos en andén | Consulta y Habilitación | **CUN-01** | Permite al Supervisor verificar las cargas puestas a disposición por el CD. |
| **RF008** | Generación de Código Único de Activación | Códigos y Activación | **CUN-01 / CUN-03** | Habilita el seguimiento en andén (CUN-01) o la recuperación en ruta (CUN-03). |
| **RF009** | Cierre forzado del despacho en contingencia | Consulta y Habilitación | **CUN-03** | Permite liquidar administrativamente viajes interrumpidos por siniestro total. |
| **RF010** | Activación móvil del despacho mediante código | Móvil de Activación (PWA) | **CUN-01** | Habilita la jornada operativa del transportista tercero sin usar contraseñas. |
| **RF011** | Sesión operativa móvil y reemplazo de token | Móvil de Activación (PWA) | **CUN-01 / CUN-03** | Asocia el dispositivo al despacho y soporta recuperación sin perder histórico. |
| **RF012** | Visualización de datos de despacho y destino | Móvil de Ruta (PWA) | **CUN-01** | Brinda al conductor la hoja de ruta y destino B2B en memoria local. |
| **RF013** | Cambio de estado a `EN_RUTA` e inicio de GPS | Móvil de Ruta (PWA) | **CUN-01** | Marca la salida formal de planta y dispara la telemetría periódica. |
| **RF014** | Muestreo periódico de coordenadas GPS en ruta | Móvil de Telemetría GPS | **CUN-02** | Captura coordenadas cada 10 min en segundo plano en Android. |
| **RF015** | Registro de llegada a destino (geocerca/manual) | Móvil de Llegada (PWA) | **CUN-04** | Acredita presencia física atómica en destino (`EN_DESTINO`) sin validar entrega. |
| **RF016** | Confirmación manual de entrega y recepción | Móvil de Entrega (PWA) | **CUN-04** | Exige la acción manual consciente para formalizar el estado `ENTREGADO`. |
| **RF017** | Registro de despacho no entregado o rechazado | Móvil de Entrega (PWA) | **CUN-04** | Sustenta técnicamente el rechazo de carga bajo causales tipificadas. |
| **RF018** | Gestión de evidencias y fotografía complementaria | Móvil de Evidencias (PWA) | **CUN-03 / CUN-04** | Asocia GPS/hora e imagen segura opcional sin bloquear la operación. |
| **RF019** | Envío de reporte de incidencia en ruta | Móvil de Incidencias (PWA) | **CUN-03** | Permite al transportista clasificar y notificar siniestros viales en carretera. |
| **RF020** | Cierre de despacho y finalización de sesión | Móvil de Cierre (PWA) | **CUN-04** | Liquida el viaje (`FINALIZADO`) y revoca los accesos móviles de forma inmediata. |
| **RF021** | Almacenamiento local offline en dispositivo | Persistencia Local (PWA) | **CUN-02** | Garantiza la operación ininterrumpida en carreteras sin cobertura (IndexedDB). |
| **RF022** | Sincronización automática FIFO al recuperar red | Sincronización Móvil | **CUN-02** | Transmite eventos retenidos en orden cronológico estricto al reconectar. |
| **RF023** | Semáforo visual del estado de sincronización | Sincronización Móvil | **CUN-02** | Brinda retroalimentación visual al conductor sobre datos retenidos en cola. |
| **RF024** | Consulta de avance y detalle de despachos | Monitoreo de Despachos | **CUN-02** | Habilita la grilla operativa con semaforización de tiempos de traslado, origen, destino y avance. |
| **RF025** | Visualización de evidencias y enlace seguro | Auditoría de Entregas | **CUN-05** | Permite auditar coordenadas de entrega y fotos seguras en Cloud Storage. |
| **RF026** | Alerta visual y sonora por incidencias y ventana | Gestión de Incidencias | **CUN-03 / CUN-04** | Notifica siniestros en ruta o demoras de 60 min en andén de destino. |
| **RF027** | Búsqueda por despacho y timeline histórico | Consulta de Trazabilidad | **CUN-05** | Despliega en menos de 2 seg la cronología completa de hitos para SAC. |
| **RF028** | Tablero ejecutivo de KPIs y exportación Excel | Indicadores (Dashboard) | **CUN-05** | Calcula Lead Time, puntualidad y latencia, con reportes en Excel. |
| **RF029** | Publicación de eventos de estado al Bus (ESB) | Integración Backend | **CUN-02 / CUN-04** | Comunica estados en formato JSON al Bus de Yanbal en SLA $\le$ 30 min. |
| **RF030** | Publicación de eventos de siniestro al Bus (ESB) | Integración Backend | **CUN-03** | Alerta contingencias graves a los sistemas centrales (SAP / Salesforce). |
| **RF031** | Envío de resumen de trazabilidad del despacho | Integración Backend | **CUN-04** | Transmite la síntesis de tiempos y kilometraje al cerrar el viaje. |
| **RF032** | Bloqueo por 5 intentos fallidos de activación | Seguridad Móvil (PWA) | **CUN-01** | Invalida el código ante ataques de adivinación o fuerza bruta en andén. |
| **RF033** | Revocación inmediata de código y sesión al cierre | Códigos y Activación | **CUN-04** | Asegura el cese total de transmisión satelital al finalizar el despacho. |
| **RF034** | Aislamiento de datos entre empresas transportistas | Seguridad y Roles Web | **CUN-05** | Impide que contratistas terceros visualicen operaciones de competidores. |

---

## 3. Cuadro Resumen de Cobertura por Caso de Uso del Negocio

```
┌───────────────────────────────────────────────────────────────────┬──────────────┐
│ Caso de Uso del Negocio (CUN)                                     │ Cantidad RFs │
├───────────────────────────────────────────────────────────────────┼──────────────┤
│ CUN-01: Despacho y Salida de Carga en Centro de Distribución       │ 7 RFs        │
│ CUN-02: Traslado Interprovincial y Monitoreo de Carga en Tránsito │ 6 RFs        │
│ CUN-03: Gestión de Incidencias y Contingencias Viales en Ruta     │ 7 RFs        │
│ CUN-04: Entrega y Recepción de Carga en Punto de Destino          │ 9 RFs        │
│ CUN-05: Auditoría de Trazabilidad y Rendimiento de Distribución   │ 4 RFs        │
│ Transversales (Seguridad Perimetral, RBAC y Auditoría Web)        │ 6 RFs        │
├───────────────────────────────────────────────────────────────────┼──────────────┤
│ Total de Requerimientos Funcionales Activos Cubiertos             │ 34 RF Activos│
└───────────────────────────────────────────────────────────────────┴──────────────┘
```

> [!NOTE]
> La suma analítica presenta algunos RFs compartidos de forma complementaria (ej. `RF008` en CUN-01 y CUN-03; `RF011` en CUN-01 y CUN-03; `RF018` en CUN-03 y CUN-04; `RF026` en CUN-03 y CUN-04; `RF029` en CUN-02 y CUN-04), garantizando que el **100% de los 34 Requerimientos Funcionales activos del proyecto Y-Trace** cuentan con trazabilidad directa y justificación de negocio.
