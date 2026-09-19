# Matriz de Trazabilidad: Casos de Uso del Negocio (CUN) vs. Requerimientos Funcionales (RF)

> **Carpeta:** `detalles/Diagrama de Casos de Uso del Negocio (CUN)`  
> **Documento:** `07_MATRIZ_TRAZABILIDAD_CUN_VS_RF.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Marco Metodológico:** Rational Unified Process (RUP) — Ingeniería de Requerimientos y Modelado de Procesos  
>  
> 🔗 **Documentos Vinculados:**  
> - [[00_INDICE_Y_MODELO_GENERAL_CUN]] — Modelo general y diagrama macro  
> - [[01_MATRIZ_DE_REQUERIMIENTOS]] — Matriz maestra unificada (28 RF activos)  
> - [[02_REQUERIMIENTOS_FUNCIONALES]] — Especificación detallada de RFs (RF001 a RF028)  

---

## 1. Justificación de la Correspondencia CUN $\leftrightarrow$ RF

En la disciplina de Arquitectura Empresarial, los **Casos de Uso del Negocio (CUN)** definen los procesos operacionales de la empresa (independientemente de la tecnología), mientras que los **Requerimientos Funcionales (RF)** especifican las capacidades de software que el sistema computacional proveerá para automatizar, controlar y dar soporte a dichos procesos.

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                               RELACIÓN DE AGREGACIÓN DE VALOR                           │
├──────────────────────────────────────┬──────────────────────────────────────────────────┤
│ 5 Casos de Uso del Negocio (CUN)     │ 28 Requerimientos Funcionales Activos (RF)       │
├──────────────────────────────────────┼──────────────────────────────────────────────────┤
│ Definen los macro-procesos de valor: │ Son los bloques y capacidades de software que    │
│ • CUN-01: Despacho y Salida CD       │ materializan las reglas, transiciones de estado, │
│ • CUN-02: Traslado y Monitoreo       │ telemetría satelital, persistencia offline,      │
│ • CUN-03: Cancelación Forzada        │ seguridad y la integración con el Bus            │
│ • CUN-04: Entrega y Recepción        │ corporativo dentro de los SLAs establecidos.     │
│ • CUN-05: Trazabilidad y Rendimiento │                                                  │
└──────────────────────────────────────┴──────────────────────────────────────────────────┘
```

---

## 2. Matriz de Cobertura Bidireccional

A continuación se presenta el mapeo exhaustivo de los **28 Requerimientos Funcionales oficiales** del sistema Y-Trace clasificados según el Caso de Uso del Negocio al cual dan soporte técnico directo:

| ID Req. | Nombre del Requerimiento Funcional Oficial | Módulo del Sistema Y-Trace | CUN que le da Soporte | Justificación del Rol en el Negocio |
| :---: | :--- | :--- | :---: | :--- |
| **RF001** | Autenticación de Usuarios en la Plataforma Web | Seguridad y Acceso Web | **Transversal** | Provee acceso seguro y controlado a la Torre de Control y gestión web. |
| **RF002** | Gestión de Cuentas de Usuarios Web | Administración de Usuarios | **Transversal** | Garantiza la gobernanza centralizada de cuentas autorizadas en Yanbal. |
| **RF003** | Asignación y Gestión de Roles de Usuarios Web | Administración de Usuarios | **Transversal** | Aplica segregación de funciones entre Administrador, Supervisor, Jefe y SAC. |
| **RF004** | Control de Acceso Basado en Roles (RBAC) | Seguridad y Acceso Web | **Transversal** | Restringe menús y transacciones al perfil funcional del colaborador. |
| **RF005** | Gestión del Ciclo de Vida de la Sesión Web | Seguridad y Acceso Web | **Transversal** | Protege terminales desatendidas mediante expiración de sesión por inactividad. |
| **RF006** | Registro de Bitácora de Auditoría de Acciones Web | Auditoría y Seguridad | **Transversal / CUN-03** | Garantiza trazabilidad pericial y justificación inmutable en la cancelación forzada. |
| **RF007** | Consulta de Despachos Disponibles para Seguimiento | Consulta de Despachos | **CUN-01** | Permite al Supervisor verificar despachos preparados por SPY/WMS listos para seguimiento. |
| **RF008** | Generación de Código Único de Activación para Seguimiento | Códigos y Activación | **CUN-01** | Habilita el seguimiento en andén emitiendo el código efímero de 8 caracteres y registrando hito previo. |
| **RF009** | Cancelación Forzada del Seguimiento del Despacho | Consulta y Habilitación | **CUN-03** | Permite cancelar formalmente el seguimiento ante contingencias externas insalvables con registro en bitácora. |
| **RF010** | Activación de la Operación Móvil mediante Código Único | Móvil de Activación (App Nativa) | **CUN-01** | Habilita la jornada operativa del transportista tercero sin requerir cuentas ni contraseñas. |
| **RF011** | Establecimiento y Mantenimiento de Sesión Operativa Móvil | Móvil de Activación (App Nativa) | **CUN-01 / CUN-02** | Mantiene la sesión operativa persistente ante caídas de red y reconexión automática. |
| **RF012** | Visualización de Información del Despacho y Destino | Móvil de Ruta (App Nativa) | **CUN-01** | Brinda al conductor los datos del punto de destino y hoja de ruta en memoria local. |
| **RF013** | Registro de Inicio de Traslado del Despacho (`EN_RUTA`) | Móvil de Ruta (App Nativa) | **CUN-01** | Registra el hito formal de salida física con fecha, hora y coordenadas GPS iniciales. |
| **RF014** | Captura Periódica de Telemetría GPS durante el Traslado | Móvil de Telemetría GPS | **CUN-02** | Captura coordenadas cada 10 min en segundo plano en Android durante `EN_RUTA`. |
| **RF015** | Registro de Llegada al Punto de Destino por Geocerca | Móvil de Llegada (App Nativa) | **CUN-04** | Acredita presencia física atómica en destino (`EN_DESTINO`) sin validar entrega de carga. |
| **RF016** | Confirmación de Recepción / Entrega del Despacho Completo | Móvil de Entrega (App Nativa) | **CUN-04** | Exige la acción manual consciente del conductor para formalizar `ENTREGADO` sin fotos. |
| **RF017** | Registro de No Entrega o Rechazo de Despacho en Destino | Móvil de Entrega (App Nativa) | **CUN-04** | Registra el rechazo de carga bajo causales tipificadas con GPS y timestamp sin fotos. |
| **RF018** | Finalización y Cierre del Seguimiento del Despacho | Móvil de Cierre (App Nativa) | **CUN-04** | Concluye el seguimiento operativo (`FINALIZADO`), apaga GPS y revoca sesión móvil. |
| **RF019** | Almacenamiento Local Offline en Android (SQLite Room) | Persistencia Local (App Nativa) | **CUN-02** | Garantiza la operación ininterrumpida en carreteras sin cobertura (SQLite Room). |
| **RF020** | Sincronización Automática en Segundo Plano (FIFO) | Sincronización Móvil | **CUN-02** | Transmite eventos retenidos en orden cronológico estricto al reconectar con idempotencia. |
| **RF021** | Indicador Visual de Estado de Sincronización Local | Sincronización Móvil | **CUN-02** | Brinda retroalimentación visual al conductor sobre datos retenidos en cola local. |
| **RF022** | Monitoreo Operativo en Torre de Control Web | Monitoreo de Despachos | **CUN-02** | Grilla interna con despacho, placa, conductor, destino, estado, última posición y tiempo de viaje. |
| **RF023** | Consulta de Trazabilidad y Resumen del Despacho | Consulta de Trazabilidad | **CUN-05** | Despliega en < 2 s la cronología completa, coordenadas y tiempos por código o placa. |
| **RF024** | Visualización de Indicadores Clave de Desempeño Logístico (KPIs) | Indicadores (Dashboard) | **CUN-05** | Tablero ejecutivo de Lead Time, puntualidad, entregas conformes y filtros por transportista con exportación Excel (.xlsx). |
| **RF025** | Publicación de Eventos de Despacho al Bus de Integración | Integración Backend | **CUN-02 / CUN-03 / CUN-04** | Publica estados en formato JSON al Bus de Yanbal en SLA $\le$ 30 min. |
| **RF026** | Envío de Resumen de Trazabilidad del Despacho al Bus | Integración Backend | **CUN-03 / CUN-04** | Transmite la síntesis consolidada de trazabilidad al cerrar o cancelar el seguimiento del viaje. |
| **RF027** | Control de Intentos y Bloqueo Anti-Fuerza Bruta del Código | Seguridad Móvil (App Nativa) | **CUN-01** | Invalida de forma definitiva el código efímero ante 5 intentos fallidos consecutivos en andén. |
| **RF028** | Unicidad, Vigencia y Caducidad del Código de Activación | Códigos y Activación | **CUN-01 / CUN-03 / CUN-04** | Asegura el cese total de emisión y revocación al pasar a `FINALIZADO` o `DESPACHO_CANCELADO`. |

---

## 3. Cuadro Resumen de Cobertura por Caso de Uso del Negocio

```
┌───────────────────────────────────────────────────────────────────┬──────────────┐
│ Caso de Uso del Negocio (CUN)                                     │ Cantidad RFs │
├───────────────────────────────────────────────────────────────────┼──────────────┤
│ CUN-01: Despacho y Salida de Carga en Centro de Distribución       │ 7 RFs        │
│ CUN-02: Traslado y Monitoreo de Carga en Tránsito                 │ 7 RFs        │
│ CUN-03: Cancelación Forzada del Seguimiento                       │ 5 RFs        │
│ CUN-04: Entrega y Recepción de Carga en Punto de Destino          │ 7 RFs        │
│ CUN-05: Consulta de Trazabilidad y Análisis de Rendimiento        │ 2 RFs        │
│ Transversales (Seguridad Perimetral, RBAC y Auditoría Web)        │ 6 RFs        │
├───────────────────────────────────────────────────────────────────┼──────────────┤
│ Total de Requerimientos Funcionales Oficiales Consolidados        │ 28 RF Activos│
└───────────────────────────────────────────────────────────────────┴──────────────┘
```

> [!NOTE]
> La suma de RFs por rubro en el cuadro contiene requerimientos de arquitectura compartida entre fases (ej. `RF011` en CUN-01 y CUN-02; `RF025`, `RF026` y `RF028` en CUN-03 y CUN-04; `RF006` transversal y de soporte a la justificación de CUN-03), garantizando que el **100% de los 28 Requerimientos Funcionales oficiales del proyecto Y-Trace** cuentan con trazabilidad directa y justificación de negocio.
