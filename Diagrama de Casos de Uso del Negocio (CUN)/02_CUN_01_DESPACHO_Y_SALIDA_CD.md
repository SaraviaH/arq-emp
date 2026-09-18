# CUN-01: Despacho y Salida de Carga en Centro de Distribución

> **Carpeta:** `detalles/Diagrama de Casos de Uso del Negocio (CUN)`  
> **Documento:** `02_CUN_01_DESPACHO_Y_SALIDA_CD.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Marco Metodológico:** Rational Unified Process (RUP) — Especificación de Casos de Uso del Negocio  
>  
> 🔗 **Documentos Vinculados:**  
> - [[00_INDICE_Y_MODELO_GENERAL_CUN]] — Modelo general de casos de uso del negocio  
> - [[01_ACTORES_DEL_NEGOCIO]] — Catálogo y fichas de actores del negocio  
> - [[07_MATRIZ_TRAZABILIDAD_CUN_VS_RF]] — Matriz de trazabilidad con requerimientos de software  

---

## 1. Ficha de Identificación del Proceso

| Atributo                 | Detalle                                                                                                                                                                                                                             |
| :----------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Identificador:**       | **CUN-01**                                                                                                                                                                                                                          |
| **Nombre del Proceso:**  | **Despacho y Salida de Carga en Centro de Distribución**                                                                                                                                                                            |
| **Estereotipo RUP:**     | `<<business use case>>`                                                                                                                                                                                                             |
| **Área Responsable:**    | Gerencia de Supply Chain — Área de Distribución y Transporte (CD Lurín)                                                                                                                                                             |
| **Alcance Operativo:**   | Distribución troncal completa B2B (Andén de Salida CD Lurín $\rightarrow$ Activación y Salida Física)                                                                                                                               |
| **Objetivo de Negocio:** | Formalizar el control previo en andén, la transferencia formal de custodia de la carga consolidada al transportista tercero y la habilitación operativa del seguimiento para garantizar el inicio seguro del viaje interprovincial. |

---

## 2. Actores del Negocio Participantes

```mermaid
flowchart LR
    SUP["fa:fa-user-tie Supervisor de Distribución<br/>(Iniciador / Control Andén)"]
    SL["fa:fa-truck Socio Logístico / Conductor<br/>(Participante / Transportista)"]
    CUN01(["((CUN-01))<br/><b>Despacho y Salida de Carga<br/>en Centro de Distribución</b>"])

    SUP ---|Inicia control y habilita código| CUN01
    SL ---|Recibe carga y confirma salida| CUN01
```

* **Actor Iniciador:** **Supervisor de Distribución (Interno)**. Controla el andén de salida, verifica que la carga consolidada esté lista para viaje y habilita el seguimiento generando el Código Único de Activación.
* **Actor Secundario / Participante:** **Socio Logístico / Conductor (Externo)**. Se presenta en andén, recibe la estiba de los bultos/pallets, activa la unidad en su PWA Android con el código recibido y declara formalmente la salida de planta.

---

## 3. Precondiciones del Negocio

1. El Centro de Distribución de Yanbal (CD Lurín) ha completado la consolidación, cubicaje y rotulación de los bultos y pallets en el sistema WMS (SPY).
2. El sistema TMS corporativo (Driving) ha establecido la zonificación geográfica, punto de destino de distribución y la empresa transportista asignada.
3. El despacho preparado se encuentra físicamente posicionado en el andén de carga y su registro ha sido comunicado a la Torre de Control Logístico mediante el Bus de Integración corporativo.
4. El Conductor asignado por el transportista tercero se encuentra presente en el andén con la unidad vehicular inspeccionada y un dispositivo móvil Android (versión 8.0+ con Google Chrome).

---

## 4. Flujo Básico de Actividades del Negocio (Happy Path)

```mermaid
sequenceDiagram
    autonumber
    actor SUP as Supervisor de Distribución
    actor COND as Conductor (Socio Logístico)
    participant SIS as Plataforma Y-Trace (Web / Móvil)
    participant EXT as Sistemas Corporativos (SPY / Bus)

    EXT->>SIS: Reporta despacho consolidado disponible en andén
    SUP->>SIS: Consulta despachos disponibles y valida elegibilidad
    SUP->>SIS: Solicita habilitación de seguimiento (Backend genera Código de Activación)
    SIS-->>SUP: Emite Código Único de 8 caracteres y registra Hito de Control Previo
    SUP->>COND: Entrega física de la carga y comunica el Código de Activación
    COND->>SIS: Abre PWA en Android e ingresa Código de Activación
    SIS-->>COND: Valida código, emite sesión operativa y descarga datos de ruta
    COND->>SUP: Concluye estiba, verifica precintos y presiona "Iniciar Despacho"
    SIS-->>SIS: Transiciona a EN_RUTA e inicia muestreo GPS en segundo plano
    SIS->>EXT: Publica evento formal de salida EN_RUTA al Bus (SLA <= 30 min)
```

1. **Consulta y Verificación en Andén:** El Supervisor de Distribución accede a la plataforma Web y visualiza la lista de despachos preparados puestos a disposición en andén por el WMS SPY.
2. **Generación de Habilitación Operativa:** El Supervisor selecciona el despacho verificado y solicita la emisión del Código Único de Activación. El sistema registra el **hito interno de control previo** (asegurando trazabilidad interna) y genera un código alfanumérico efímero de 8 caracteres (ej. `TRC-82F4`).
3. **Traspaso de Custodia y Handoff:** El Supervisor entrega el código al Conductor del transportista asociado junto con las guías físicas de remisión y transfiere formalmente la custodia física de los bultos/pallets estibados.
4. **Activación de la Unidad Móvil:** El Conductor accede a la PWA de Y-Trace en su teléfono Android e ingresa el Código de Activación recibido. El sistema valida la elegibilidad del viaje, asocia la huella del dispositivo, emite una sesión operativa temporal y descarga los datos operativos del despacho y punto de destino.
5. **Declaración Formal de Salida (`EN_RUTA`):** Con la carga asegurada y la unidad lista para cruzar la garita de control de Lurín, el Conductor pulsa *"Iniciar Despacho"*.
6. **Inicio de Telemetría y Notificación Corporativa:** El sistema transiciona el estado a `EN_RUTA`, activa la captura periódica de coordenadas GPS en segundo plano y notifica el hito de salida al Bus de Integración de Yanbal para sincronizar a SAP R/3 y Salesforce dentro del SLA de 30 minutos.

---

## 5. Flujos Alternativos y Excepciones del Negocio

### A1: Bloqueo por Intentos Fallidos Consecutivos (Seguridad Anti-Fuerza Bruta)
* **Condición:** El Conductor ingresa erróneamente el código de activación en su teléfono Android.
* **Acción de Negocio:**
  - El sistema permite hasta 4 reintentos.
  - Al registrarse el **quinto intento fallido consecutivo**, el sistema invalida y bloquea automáticamente el código, emitiendo una alerta visual inmediata al Supervisor en la plataforma Web.
  - El Supervisor verifica la identidad presencial del Conductor en andén, anula el código bloqueado y genera un nuevo código de activación previa validación administrativa.

### A2: Cancelación Logística Externa Previa a la Salida
* **Condición:** El Centro de Distribución o el área comercial ordena detener el despacho antes de que la unidad cruce la garita (ej. retención de calidad o cancelación de pedido).
* **Acción de Negocio:**
  - El sistema externo notifica la cancelación a través del Bus de Integración.
  - El sistema Y-Trace registra el evento administrativo `DESPACHO_CANCELADO`, revoca de inmediato la validez del Código de Activación y cancela cualquier sesión móvil pendiente.
  - El Supervisor retira la carga del andén y la devuelve a custodia interna del CD.

### A3: Falla de Dispositivo del Conductor en Andén
* **Condición:** El teléfono del conductor se descarga, rompe o no dispone de conectividad al momento de la estiba en Lurín.
* **Acción de Negocio:**
  - El Conductor reporta el percance al Supervisor de andén.
  - El Supervisor puede reasignar la salida a otro Conductor/dispositivo habilitado de la misma empresa transportista, invalidando el código anterior y emitiendo una nueva habilitación.

---

## 6. Postcondiciones del Negocio

* **Estado de Éxito:**
  - La custodia física de la carga ha sido transferida formalmente a la empresa transportista.
  - El despacho se encuentra en estado `EN_RUTA` emitiendo telemetría satelital periódica.
  - El hito de salida física ha sido notificado al ecosistema Yanbal (SAP / Salesforce) vía Bus de Integración.
* **Estado de Cancelación:**
  - La carga permanece bajo custodia del CD Lurín y el código de activación queda revocado sin emitir datos en ruta.

---

## 7. Reglas de Negocio Vinculadas

* **RN-CUN-01.1 (Carga Preexistente):** Y-Trace no planifica rutas ni consolida bultos; solo gestiona despachos completos previamente preparados por los sistemas corporativos (SPY / Driving).
* **RN-CUN-01.2 (Hito Interno de Control Previo):** La emisión del código de activación genera un hito interno que audita la habilitación del viaje, sin constituir una autorización logística de salida externa ni publicarse al Bus.
* **RN-CUN-01.3 (Cero Contraseñas en Móvil):** Los conductores externos no poseen cuentas ni contraseñas permanentes; su autenticación se realiza exclusivamente mediante códigos de activación efímeros de 8 caracteres vinculados al despacho.
* **RN-CUN-01.4 (Límite Anti-Fuerza Bruta):** Máximo 5 intentos fallidos antes del bloqueo definitivo del código.

---

## 8. Trazabilidad con Requerimientos Funcionales del Sistema (Y-Trace)

Los siguientes requerimientos de software implementan y dan soporte operativo a este Caso de Uso del Negocio:

| ID Requerimiento | Nombre del Requerimiento Funcional            | Rol en el Soporte de CUN-01                                                                            |
| :--------------: | :-------------------------------------------- | :----------------------------------------------------------------------------------------------------- |
|    **RF007**     | Consulta y filtrado de despachos en andén     | Permite al Supervisor visualizar y filtrar las cargas preparadas disponibles para iniciar seguimiento. |
|    **RF008**     | Generación de Código Único de Activación      | Emite el código de 8 caracteres y registra el hito interno de control previo.                          |
|    **RF010**     | Activación móvil del despacho mediante código | Valida el código en la PWA Android del conductor y habilita la jornada.                                |
|    **RF011**     | Establecimiento de sesión operativa móvil     | Asocia el dispositivo Android al despacho sin usar contraseñas permanentes.                            |
|    **RF012**     | Visualización de datos de despacho y destino  | Descarga la información del punto de destino a la memoria local (IndexedDB).                           |
|    **RF013**     | Cambio de estado a `EN_RUTA` e inicio de GPS  | Formaliza el inicio de viaje y activa los sensores de geolocalización.                                 |
|    **RF032**     | Bloqueo por 5 intentos fallidos de activación | Protege el código efímero contra ataques de adivinación o fuerza bruta.                                |

