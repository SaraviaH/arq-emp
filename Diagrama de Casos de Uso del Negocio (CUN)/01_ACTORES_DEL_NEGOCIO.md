# Actores del Negocio: Distribución B2B Yanbal Perú

> **Carpeta:** `detalles/Diagrama de Casos de Uso del Negocio (CUN)`  
> **Documento:** `01_ACTORES_DEL_NEGOCIO.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Marco Metodológico:** Rational Unified Process (RUP) — Modelado del Negocio  
>  
> 🔗 **Documentos Relacionados:**  
> - [[00_INDICE_Y_MODELO_GENERAL_CUN]] — Modelo general y diagrama macro  
> - [[07_MATRIZ_TRAZABILIDAD_CUN_VS_RF]] — Matriz de trazabilidad con requerimientos de software  

---

## 1. Definición y Clasificación de Actores del Negocio en RUP

En el marco de **Rational Unified Process (RUP)** y la práctica docente de **Arquitectura Empresarial (Sesión 4 UTP)**, los **Actores del Negocio (`<<business actor>>`)** representan roles externos a la organización (socios transportistas o agencias destinatarias) o roles operativos internos de la empresa que intervienen en los procesos del negocio ejecutando tareas o recibiendo sus resultados de valor.

> **Nota Aclaratoria:** El **Administrador Principal** es un rol estrictamente informático y de administración del sistema (gestión de cuentas web y perfiles RBAC), por lo que **no constituye un Actor del Negocio logístico** (`<<business actor>>`) ni participa directamente en los flujos de los Casos de Uso del Negocio (CUN).

```mermaid
graph TD
    subgraph CLASIFICACION_ACTORES ["Gobernanza de Actores del Negocio"]
        EXT["<b>1. Actores Externos del Negocio</b><br/>(Interactúan con el proceso desde fuera de la jerarquía directa de Yanbal)"]
        INT["<b>2. Actores Internos del Negocio</b><br/>(Roles operativos y tácticos de la organización Yanbal)"]
    end

    EXT --> A1["ACT-NEG-01: Socio Logístico / Conductor"]
    EXT --> A2["ACT-NEG-02: Punto de Destino / Agencia Receptora"]
    INT --> A3["ACT-NEG-03: Supervisor de Distribución"]
    INT --> A4["ACT-NEG-04: Jefe de Distribución"]
    INT --> A5["ACT-NEG-05: Operador SAC / Soporte Logístico"]
```

---

## 2. Fichas Descriptivas de Actores del Negocio

### 2.1 Actores Externos

#### ACT-NEG-01: Socio Logístico / Conductor
* **Estereotipo RUP:** `<<business actor>>`
* **Tipo:** Externo (Contratista de transporte tercero).
* **Descripción Institucional:**
  Representa tanto a las empresas transportistas homologadas por Yanbal como a los conductores asignados a las unidades de carga pesada o mediana (furgones, camiones interprovinciales) para realizar el traslado troncal e interdepartamental.
* **Objetivo de Negocio:**
  Recibir la carga en andén, trasladarla cumpliendo los estándares de seguridad vial y los tiempos de entrega pactados (*Lead Time*), y formalizar la entrega en el punto de distribución.
* **Responsabilidades en el Proceso:**
  1. Recibir físicamente los pallets o bultos precintados en el CD Lurín y validar su correspondencia con la guía de remisión física.
  2. Activar la unidad en la App Nativa Android mediante el Código de Activación efímero de 8 caracteres entregado por el Supervisor.
  3. Ejecutar el traslado interprovincial portando el dispositivo móvil que emite telemetría satelital periódica en segundo plano.
  4. Comunicar de inmediato contingencias viales, desperfectos mecánicos o bloqueos en carretera a la Torre de Control mediante telefonía o canales operativos externos (fuera de Y-Trace).
  5. Presentar la unidad en el punto de destino para la descarga e inspección física, y registrar la confirmación manual consciente (`ENTREGADO` o `NO_ENTREGADO` con causal tipificada) y el cierre formal (`FINALIZADO`) en la App Nativa Android mediante datos estructurados (GPS y timestamp), sin capturar fotografías ni POD multimedia.
* **Información que Recibe:**
  - Código Único de Activación (8 caracteres).
  - Datos operativos del despacho: código de viaje, punto de destino y observaciones de ruta.
* **Información que Genera:**
  - Confirmación de inicio de despacho (`EN_RUTA`).
  - Muestreo periódico de telemetría GPS en segundo plano durante el traslado.
  - Registro de llegada acreditado por geocerca (`EN_DESTINO`).
  - Confirmación manual de entrega (`ENTREGADO` o `NO_ENTREGADO` con motivo tipificado) y solicitud de cierre (`FINALIZADO`).

---

#### ACT-NEG-02: Punto de Destino / Agencia Receptora
* **Estereotipo RUP:** `<<business actor>>`
* **Tipo:** Externo / Destinatario B2B (Agencia comercial, almacén intermedio o centro secundario departamental).
* **Descripción Institucional:**
  Representa al encargado de recepción de mercadería en las sedes regionales o puntos de distribución de Yanbal ubicados a lo largo del territorio nacional.
* **Objetivo de Negocio:**
  Recepcionar formalmente la carga consolidada, verificar su integridad física exterior y otorgar la conformidad física de recepción para permitir el abastecimiento regional.
* **Responsabilidades en el Proceso:**
  1. Recibir físicamente al conductor del socio logístico a su arribo a las instalaciones de la agencia o almacén.
  2. Realizar la inspección visual exterior de bultos, precintos de seguridad y pallets.
  3. Validar las cantidades declaradas en los manifiestos físicos de transporte.
  4. Otorgar la conformidad física de recepción suscribiendo con firma y sello la guía de remisión física, o emitir las observaciones y causales formales de rechazo físico en caso de anomalías severas o arribo fuera de horario.
  > [!NOTE]
  > El Punto de Destino participa exclusivamente en la recepción física e inspección de la carga; no interactúa directamente con el software Y-Trace ni registra la entrega en el sistema. El registro en la aplicación móvil es efectuado por el Conductor conforme al requerimiento oficial RF016.
* **Información que Recibe:**
  - Carga física consolidada y bultos rotulados.
  - Manifiesto físico / guía de remisión física de transporte.
* **Información que Genera:**
  - Dictamen de conformidad física (o rechazo tipificado) suscrito con sello y firma física en comprobante de recepción.

---

### 2.2 Actores Internos (Trabajadores del Negocio)

#### ACT-NEG-03: Supervisor de Distribución
* **Estereotipo RUP:** `<<business actor>>`
* **Tipo:** Interno (Nivel de Gestión Operativa / Torre de Control CD Lurín).
* **Descripción Institucional:**
  Colaborador interno de Yanbal Perú encargado del control diario de las operaciones de salida y monitoreo de la flota desde la Torre de Control Logístico.
* **Objetivo de Negocio:**
  Garantizar el despacho fluido de la carga desde el andén de Lurín, mantener visibilidad permanente de los vehículos en carretera y ejecutar la cancelación forzada del seguimiento cuando una contingencia insalvable lo amerite.
* **Responsabilidades en el Proceso:**
  1. Inspeccionar en andén los despachos consolidados puestos a disposición para seguimiento por los sistemas corporativos (SPY/Driving).
  2. Generar el Código Único de Activación efímero de 8 caracteres (registrando el hito interno de control previo) y transferir formalmente la custodia física de la carga al conductor.
  3. Supervisar en tiempo real el avance geográfico, estado y tiempos de ciclo de la flota en la grilla operativa interactiva de la Torre de Control interna de Yanbal.
  4. Recibir comunicaciones telefónicas externas del conductor o base de transporte ante contingencias viales ocurridas en carretera.
  5. Evaluar la viabilidad del viaje y, si la contingencia imposibilita de forma definitiva continuar el traslado, ejecutar la Cancelación Forzada del Seguimiento en la plataforma Web (`RF009`), seleccionando obligatoriamente la causal tipificada y registrando la justificación en la bitácora inmutable (`RF006`).
* **Información que Recibe:**
  - Despachos preparados disponibles para seguimiento puestos a disposición en andén.
  - Telemetría GPS periódica transmitida por los vehículos en ruta.
  - Comunicaciones telefónicas externas sobre contingencias viales en carretera.
* **Información que Genera:**
  - Código Único de Activación de 8 caracteres y registro del hito interno de control previo.
  - Ejecución de Cancelación Forzada del Seguimiento con causal tipificada y justificación auditada en bitácora inmutable.

---

#### ACT-NEG-04: Jefe de Distribución
* **Estereotipo RUP:** `<<business actor>>`
* **Tipo:** Interno (Nivel Táctico / Estratégico de Supply Chain).
* **Descripción Institucional:**
  Máximo responsable del área de Distribución y Transporte de Yanbal Perú.
* **Objetivo de Negocio:**
  Evaluar estratégicamente el nivel de servicio de la red de transporte nacional, supervisar el cumplimiento de compromisos operativos y analizar comparativamente el desempeño entre empresas transportistas.
* **Responsabilidades en el Proceso:**
  1. Evaluar periódicamente los indicadores de rendimiento logístico (*Lead Time*, cumplimiento de entregas conformes, tasa de puntualidad y latencia de integración con el Bus corporativo) a través del Dashboard ejecutivo (`RF024`).
  2. Monitorear la métrica analítica separada de despachos cancelados administrativamente.
  3. Filtrar y comparar objetivamente el desempeño operativo entre las distintas empresas de transporte asociadas para la supervisión y control de la flota.
  4. Exportar reportes analíticos consolidados exclusivamente en formato Excel (.xlsx) para el análisis operativo y soporte de decisiones del área.
* **Información que Recibe:**
  - Tablero ejecutivo de indicadores (Dashboard) con métricas consolidadas, filtros por período, ruta y transportista.
  - Historial consolidado de trazabilidad y tiempos de ciclo.
* **Información que Genera:**
  - Reportes analíticos consolidados oficiales en formato Excel (.xlsx).
  - Evaluaciones de desempeño de servicio para la Dirección de Operaciones.

---

#### ACT-NEG-05: Operador SAC / Soporte Logístico
* **Estereotipo RUP:** `<<business actor>>`
* **Tipo:** Interno (Servicio al Cliente y Trazabilidad Operacional).
* **Descripción Institucional:**
  Agente de soporte interno encargado de responder requerimientos y consultas de trazabilidad sobre despachos en tránsito o culminados.
* **Objetivo de Negocio:**
  Resolver consultas e inquietudes de clientes internos (agencias comerciales, fuerza de ventas) en el primer contacto, disponiendo de información histórica de trazabilidad fidedigna e inmediata.
* **Responsabilidades en el Proceso:**
  1. Realizar búsquedas ágiles de despachos mediante código de viaje o placa vehicular en el buscador web de Y-Trace (`RF023`).
  2. Examinar la línea de tiempo completa (*Timeline*) del despacho (cronología de hitos, estados, marcas de tiempo y coordenadas GPS) en menos de 2 segundos.
  3. Resolver tickets de consulta, corroborar arribos y desvirtuar discrepancias de entrega con base en datos estructurados y certificados de auditoría interna.
* **Información que Recibe:**
  - Solicitudes de consulta o reclamos por estado de despachos.
  - Cronología histórica completa, marcas de tiempo y coordenadas GPS atómicas en plataforma Web.
* **Información que Genera:**
  - Respuestas fundamentadas a consultas operativas de trazabilidad para clientes internos.

---

## 3. Matriz de Atribuciones y Flujo de Intercambio de Información

| Actor del Negocio | Rol Institucional | Entradas de Información | Salidas / Entregables | Herramienta Operativa |
| :--- | :--- | :--- | :--- | :--- |
| **Socio Logístico / Conductor** | Transportista tercero de carga pesada | Manifiesto de carga, Código de Activación de 8 caracteres | Telemetría GPS en ruta, registro de llegada, confirmación de entrega o causal de rechazo | App Nativa Android (Y-Trace Móvil) |
| **Punto de Destino / Agencia** | Receptor físico B2B en destino | Carga consolidada física, guía de remisión física | Dictamen de conformidad física, sello/firma física de recepción | Manifiesto físico / Inspección visual exterior |
| **Supervisor de Distribución** | Control operativo de andén y flota | Despachos disponibles en andén, telemetría de vehículos, llamadas de contingencia | Código de activación, hito de control previo, Cancelación Forzada justificada | Plataforma Web (Torre de Control) |
| **Jefe de Distribución** | Dirección táctica y control de SLAs | Métricas consolidadas de tiempos, entregas conformes y latencia | Reportes analíticos consolidados en Excel (.xlsx), análisis comparativo | Plataforma Web (Dashboard Ejecutivo) |
| **Operador SAC / Soporte** | Atención de consultas de trazabilidad | Requerimientos de seguimiento de agencias / áreas comerciales | Información fidedigna de trazabilidad, resolución de consultas | Plataforma Web (Buscador de Trazabilidad) |

---

## 4. Gobernanza y Segregación de Responsabilidades

Para asegurar la integridad del negocio y prevenir colusión o distorsiones operativas:
1. **Segregación Operativa en Andén:** El Supervisor de Distribución solo puede habilitar el seguimiento de despachos ya preparados y validados externamente por el WMS SPY; no puede crear despachos ficticios ni realizar asignaciones logísticas de flota dentro de Y-Trace.
2. **Segregación en Ruta:** El Conductor no puede modificar los datos del despacho, ni reprogramar rutas, ni alterar las coordenadas GPS capturadas de forma nativa por el dispositivo Android. Las contingencias se reportan por vía telefónica externa.
3. **Segregación en Destino:** El arribo a la geocerca (`EN_DESTINO`) es una evidencia geoespacial que no sustituye la confirmación manual consciente de entrega (`ENTREGADO`), garantizando que la presencia del vehículo no acredite automáticamente la descarga conforme. La certificación se realiza mediante datos estructurados sin captura de fotos ni POD multimedia.
