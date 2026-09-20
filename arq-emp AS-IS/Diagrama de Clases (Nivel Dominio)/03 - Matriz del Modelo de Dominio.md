# Matriz del Modelo de Dominio (AS-IS)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Marco Metodológico:** RUP (*Rational Unified Process*) — Trazabilidad de Entidades de Dominio vs. Casos de Uso del Negocio  
>
> **Navegación del Módulo de Dominio:**  
> [[01 - Diagrama de Clases de Dominio (Nivel Dominio)]] | [[02 - Diccionario de Datos del Modelo de Dominio]] | [[03 - Matriz del Modelo de Dominio]]  
> **Enlaces a los CUN:**  
> [[01 - Diagrama General de Casos de Uso del Negocio (CUN)]] | [[02 - Tabla de Roles y Actividades del Negocio]] | [[03 - Especificación de Casos de Uso del Negocio]] | [[04 - Trazabilidad de Casos de Uso del Negocio]]  
> **Enlaces a Requerimientos:**  
> [[01 - Matriz Consolidada de Requerimientos]] | [[02 - Especificación de Requerimientos Funcionales]]

---

## 1. Propósito de la Matriz

La **Matriz del Modelo de Dominio** establece la relación bidireccional estricta entre las **entidades del negocio** identificadas en el [[01 - Diagrama de Clases de Dominio (Nivel Dominio)]], los **Casos de Uso del Negocio (CUN)** aprobados, las **actividades operativas AS-IS** (`ACT-01` a `ACT-14`) y los **Requerimientos Funcionales** de la solución arquitectónica.

Esta matriz demuestra que cada concepto modelado participa activamente en los procesos vigentes de distribución y que no existen entidades huérfanas ni inventadas.

---

## 2. Matriz de Entidades vs. Casos de Uso del Negocio (CUN)

A continuación se detalla la intervención de cada entidad conceptual en los 5 CUN, indicando además su ciclo de vida conceptual de negocio:
- **C (Crear / Registrar):** El proceso instancia o formaliza los datos de la entidad.
- **R (Leer / Consultar):** El proceso consulta, verifica o visualiza la información de la entidad.
- **U (Actualizar):** El proceso modifica atributos o estados de la entidad.
- **X (Cierre / Conclusión):** El proceso formaliza la finalización del ciclo de vida operativo de la entidad.

| Entidad de Dominio | CUN-01<br/>Despachar | CUN-02<br/>Trasladar | CUN-03<br/>Entregar | CUN-04<br/>Entrega Fallida / Retorno | CUN-05<br/>Consultar Trazabilidad | Actividades AS-IS Involucradas | Requerimientos Trazados | Rol Responsable en el Negocio |
|:---|:---:|:---:|:---:|:---:|:---:|:---|:---:|:---|
| **`Pedido`** | **C / U** | **U** | **U / X** | **U / X** | **R** | `ACT-01` a `ACT-14` | RF-01 a RF-12 | Supervisor de Despacho / Socio Logístico / Consultora / Agente |
| **`Bulto`** | **R / U** | **R** | **X** | **U / X** | — | `ACT-01`, `ACT-05`, `ACT-06`, `ACT-07`, `ACT-08`, `ACT-10`, `ACT-11` | RF-01, RF-04, RF-05, RF-06, RF-08, RF-09 | Supervisor de Despacho / Socio Logístico |
| **`RegistroDespacho`** | **C** | **R** | — | — | **R** | `ACT-04`, `ACT-05` | RF-03, RF-04, RF-11 | Supervisor de Zona de Despacho |
| **`Consultora`** | **R** | **R** | **R** | — | **R** | `ACT-01`, `ACT-02`, `ACT-09`, `ACT-13` | RF-01, RF-07, RF-11 | Consultora / Supervisor / Agente Atención |
| **`DireccionEntrega`** | **R** | **R** | **R** | **R** | **R** | `ACT-02`, `ACT-03`, `ACT-04`, `ACT-07`, `ACT-08`, `ACT-10` | RF-01, RF-02, RF-03, RF-05, RF-06, RF-08 | Supervisor de Despacho / Socio Logístico |
| **`SocioLogistico`** | **R / U** | **R** | **R** | **R** | **R** | `ACT-03`, `ACT-05`, `ACT-06`, `ACT-07`, `ACT-08`, `ACT-10`, `ACT-11` | RF-02, RF-04, RF-05, RF-06, RF-08, RF-09 | Socio Logístico / Supervisor de Despacho |
| **`ModalidadTransporte`** *(Terrestre / Bimodal / Aérea)* | **C / R** | **R** | — | — | **R** | `ACT-03`, `ACT-04`, `ACT-06`, `ACT-07`, `ACT-13` | RF-02, RF-03, RF-05, RF-11 | Supervisor de Despacho / Socio Logístico |
| **`RegistroEntrega`** | — | — | **C** | — | **R** | `ACT-08`, `ACT-09`, `ACT-14` | RF-06, RF-07, RF-12 | Socio Logístico / Transportista |
| **`Receptor`** *(ConsultoraTitular / PersonalAutorizado)* | — | — | **C** | — | **R** | `ACT-09`, `ACT-14` | RF-07, RF-12 | Socio Logístico / Consultora / Personal Autorizado |
| **`IncidenciaEntrega`** | — | — | — | **C** | **R** | `ACT-10`, `ACT-13` | RF-08, RF-11 | Socio Logístico / Transportista |
| **`OrdenRetorno`** | — | — | — | **C / X** | **R** | `ACT-11`, `ACT-13` | RF-09, RF-11 | Socio Logístico / Transportista |

---

## 3. Detalle de Intervención de las Entidades por Caso de Uso del Negocio

### 3.1. CUN-01: Despachar Pedidos desde Centro de Distribución
* **Objetivo del CUN:** Clasificar los pedidos empacados por destino geográfico nacional, vincular socio logístico, asignar modalidad de transporte, asociar la promesa de entrega y formalizar la salida material en muelle.
* **Entidades Intervinientes:**
  - `Pedido`: Cambia de estado a *"Despachado"* (`ACT-05`), asociando la promesa estimada de entrega (`ACT-04`).
  - `Bulto`: Se verifica la integridad física de las cajas (Formatos 1 a 8) recibidas de picking (`ACT-01`).
  - `RegistroDespacho`: Se genera formalmente como constancia de transferencia de custodia al transportista (`ACT-05`).
  - `Consultora` y `DireccionEntrega`: Se leen para clasificar el pedido según departamento (24 departamentos), provincia y distrito (`ACT-02`).
  - `SocioLogistico`: Es seleccionado y vinculado formalmente al despacho de la ruta (`ACT-03`).
  - `ModalidadTransporte`: Se determina si el pedido viajará vía terrestre, bimodal o aérea según la dificultad de acceso (`ACT-03`).

### 3.2. CUN-02: Trasladar Pedidos hacia Destino Nacional
* **Objetivo del CUN:** Asumir la custodia de la carga consolidada, cargar las unidades de transporte e iniciar el traslado por la red logística hacia el domicilio de destino.
* **Entidades Intervinientes:**
  - `Pedido`: Cambia de estado a *"En Ruta"* (`ACT-06`) y se mantiene en tránsito durante el trayecto (`ACT-07`).
  - `Bulto`: Es manipulado físicamente durante la estiba y transporte.
  - `SocioLogistico`: Conduce y custodia los bultos en todo el trayecto interdepartamental.
  - `ModalidadTransporte`: Rige la ruta física recorrida (carretera troncal, transporte combinado bimodal o vuelo comercial).
  - `DireccionEntrega`: Determina el itinerario y destino final del vehículo de transporte.

### 3.3. CUN-03: Entregar Carga en Sede Autorizada de Destino (Camino Principal)
* **Objetivo del CUN:** Realizar la entrega física de los bultos en la sede o agencia autorizada de destino, verificar la identidad de quien recibe y formalizar la constancia de entrega.
* **Entidades Intervinientes:**
  - `Pedido`: Cambia de estado al valor terminal *"Entregado"* (`ACT-08`).
  - `Bulto`: Se entrega materialmente al receptor en la sede o agencia de destino.
  - `RegistroEntrega`: Se genera por el transportista consignando fecha, hora y conformidad (`ACT-08`).
  - `Receptor`: Se registra la persona física que recibe (`ACT-09`), especializándose en:
    - `ConsultoraTitular`: Si la recepción la efectuó el titular de la cuenta o despacho.
    - `PersonalAutorizado`: Si recibió el personal o persona autorizada designada en la sede/agencia de destino.

### 3.4. CUN-04: Gestionar Entrega Fallida y Retorno por Logística Inversa (Camino Alternativo)
* **Objetivo del CUN:** Registrar la imposibilidad de completar la entrega física por incidencias tipificadas (retraso, pérdida o daño) y formalizar el inicio de retorno al CD en caso de contar con bulto físico.
* **Entidades Intervinientes:**
  - `Pedido`: Cambia de estado al valor terminal de ruta *"Entrega Fallida"* (`ACT-10`).
  - `IncidenciaEntrega`: Se genera tipificando la causal exacta: *"Retraso"*, *"Pérdida"* o *"Daño"* (`ACT-10`).
  - `OrdenRetorno`: Se genera formalmente **únicamente** si la incidencia presenta bulto físico disponible (`disponibilidadBulto = true`, en causales de retraso o daño) para amparar el flete de retorno al almacén (`ACT-11`). En caso de pérdida, no se instancia (`0`).
  - `Bulto`: En caso de daño o retraso, es reembarcado hacia el Centro de Distribución.

### 3.5. CUN-05: Consultar Trazabilidad y Situación del Pedido
* **Objetivo del CUN:** Atender consultas sobre el estado del pedido, promesa de entrega y receptor real disponible, tanto por la consultora titular (portal web) como por agentes de atención (Salesforce).
* **Entidades Intervinientes:**
  - `Pedido`: Se consulta su identificador (`numeroPedido`), su estado actual (`estadoPedido`) y promesa (`promesaEntrega`) (`ACT-13`).
  - `Consultora`: Proporciona su `codigoConsultora` como criterio alternativo de búsqueda (`ACT-13`).
  - `RegistroEntrega` y `Receptor`: Permiten visualizar los datos disponibles de la persona que recibió el paquete (`ACT-14`), reflejando la información sincronizada por `ACT-12`.
  - `IncidenciaEntrega` y `OrdenRetorno`: Si el pedido sufrió una contingencia, permiten consultar el motivo tipificado y la situación del retorno.

---

## 4. Consistencia y Correspondencia de la Arquitectura

Los tres documentos del módulo de clases de dominio guardan correspondencia exacta:
1. **Mismos Nombres y Atributos:** Todas las entidades (`Pedido`, `Bulto`, `RegistroDespacho`, `Consultora`, `DireccionEntrega`, `SocioLogistico`, `ModalidadTransporte`, `TransporteTerrestre`, `TransporteBimodal`, `TransporteAereo`, `RegistroEntrega`, `Receptor`, `ConsultoraTitular`, `PersonalAutorizado`, `IncidenciaEntrega`, `OrdenRetorno`) y sus respectivos atributos mantienen idéntica nomenclatura técnica y conceptual en el [[Diagrama_Clases_Dominio.puml]], en el [[02 - Diccionario de Datos del Modelo de Dominio]] y en la presente matriz.
2. **Sin Inconsistencias con los CUN:** Respeta que la sincronización técnica (`ACT-12` / `RF-10`) es transversal y no una entidad estática aislada, que el personal / persona autorizada receptora es física presencial (`ACT-09`), y que la logística inversa concluye formalmente en el inicio del retorno sin incluir procesos internos de almacén.
