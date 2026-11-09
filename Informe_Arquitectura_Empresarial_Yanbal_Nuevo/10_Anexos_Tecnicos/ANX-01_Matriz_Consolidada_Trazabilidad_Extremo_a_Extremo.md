# ANEXO 1: MATRIZ CONSOLIDADA DE TRAZABILIDAD INTEGRAL EXTREMO A EXTREMO

> **Ubicación oficial:** `10_Anexos_Tecnicos/ANX-01_Matriz_Consolidada_Trazabilidad_Extremo_a_Extremo.md`  
> **Documento maestro articulado:** [`ANEXOS_GENERALES_PROYECTO.md`](ANEXOS_GENERALES_PROYECTO.md)  
> **Fuentes de verdad arquitectónica:** [`01_5_Matriz_Maestra_Requerimientos.md`](../01_Fase_Preliminar/01_5_Matriz_Maestra_Requerimientos.md), Fases Preliminar a H de TOGAF ADM.  
> **Propósito:** Consolidar en una vista única la trazabilidad bidireccional y continua de los 24 Requerimientos Canónicos (`REQ-01` a `REQ-24`) a través de todas las fases del ciclo ADM: desde los Objetivos Estratégicos de Negocio hasta la Supervisión Continua en Régimen Operacional.

---

## 1. Estructura de la Matriz de Trazabilidad Extremo a Extremo

Para asegurar la coherencia arquitectónica y el cumplimiento de las normas ISO/IEC/IEEE 29148 y TOGAF Standard, la matriz articula diez niveles de trazabilidad para cada uno de los 24 requerimientos oficiales:
1. **Identificador y Denominación Oficial (`REQ-XX`):** Código canónico unificado.
2. **Clasificación y Prioridad:** Tipo (Negocio, Funcional, No Funcional, Restricción, Integración, Transición, Arquitectura) y Prioridad unificada (`Alta`, `Media`, `Baja`, según `DEC-01`).
3. **Objetivos de Negocio y KPIs Vinculados:** Trazabilidad a `OBJ-01`..`08` y métricas de desempeño `KPI-01`..`08`.
4. **Procesos y Casos de Uso de Negocio (Fase B):** Vinculación a procesos `PROC-01`..`05` y casos de uso de negocio `CUN-01`..`06`.
5. **Componentes y Entidades de Datos (Fase C):** Asignación a microservicios `CMP-XX-YY`, adaptadores `ADP-XX` y entidades de datos `ENT-XX`.
6. **Infraestructura y Seguridad Tecnológica (Fase D):** Plataforma de ejecución, protocolos, bases de datos y controles de seguridad (`DIAG-07`).
7. **Paquete de Trabajo y Solución Seleccionada (Fase E):** Enlace a paquetes de trabajo `PKG-01` a `PKG-05` (`DIAG-08`).
8. **Planificación de Migración y Arquitectura de Transición (Fase F):** Etapa de despliegue, asignación a `AT-1`, `AT-2` o `TO-BE` y actividades cronograma (`ACT-P*-*`).
9. **Gobernanza de Implementación y Quality Gate (Fase G):** Contrato de Arquitectura (`AC-PKG-XX`), Punto de Control de aceptación (`GATE 1`, `GATE 2` o `GATE 3`) y actores responsables según matriz RACI.
10. **Gobernanza del Cambio y Supervisión en Régimen Continuo (Fase H):** Sensores de monitoreo, telemetría y clasificación de Solicitudes de Cambio (`RFC Tipo 1`, `Tipo 2` o `Tipo 3`, `DIAG-11`).

---

## 2. Matriz Consolidada de los 24 Requerimientos Canónicos

| ID Requerimiento | Clasificación y Prioridad | Objetivos y KPIs | Negocio (Fase B) | Sistemas / Componentes (Fase C) | Tecnología y Seguridad (Fase D) | Paquete (Fase E) | Migración y Transición (Fase F) | Gobernanza y Gates (Fase G) | Supervisión y Cambio (Fase H) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`REQ-NEG-001`** | Negocio<br>**Alta** | `OBJ-01`<br>`KPI-01` | `PROC-04`<br>`CUN-01` | `CMP-ING-01`<br>`CMP-COR-02`<br>`ENT-04` | Event Broker (RabbitMQ/Kafka)<br>REST / JSON<br>TLS 1.3 | `PKG-02`<br>Core MTD | `AT-1`<br>`ACT-P2-01`<br>Semanas S5–S10 | `AC-PKG-02`<br>**GATE 1**<br>`ACT-12` (R), `ACT-06` (A) | Monitoreo Prometheus<br>$\Delta T \le 30\text{ min}$<br>RFC Tipo 2 |
| **`REQ-NEG-002`** | Negocio<br>**Alta** | `OBJ-02`<br>`KPI-01`, `KPI-02` | `PROC-04`<br>`CUN-02` | `CMP-AUD-04`<br>`CMP-ING-01`<br>`ENT-05` | Ingesta Asíncrona<br>Marcas duales $T_0/T_1$<br>ISO 8601 UTC-5 | `PKG-02`<br>`PKG-05` | `AT-1` / `TO-BE`<br>`ACT-P2-01`, `ACT-P5-02`<br>S5–S10 / S17–S24 | `AC-PKG-02`<br>`AC-PKG-05`<br>**GATE 1** / **GATE 3** | Alerta preventiva si $> 25\text{ min}$<br>Telemetría en vivo<br>RFC Tipo 2 |
| **`REQ-NEG-003`** | Negocio<br>**Media** | `OBJ-05`<br>`KPI-05` | `PROC-05`<br>`CUN-05` | `CMP-PUB-05`<br>`ADP-CRM`<br>`SIS-09` (`CELLFORCE`) | WebSocket / Push API<br>Reverse Proxy WAF<br>Cache Redis | `PKG-02`<br>`PKG-03` | `AT-2`<br>`ACT-P3-03`<br>Semanas S11–S16 | `AC-PKG-03`<br>**GATE 2**<br>`ACT-12` (R), `ACT-08` (C) | Tasa de quejas en CRM<br>Uptime de consultas<br>RFC Tipo 1 |
| **`REQ-NEG-004`** | Negocio<br>**Media** | `OBJ-06`<br>`KPI-03` | `PROC-05`<br>`CUN-06` | `CMP-NOT-06`<br>`ENT-08`<br>`ACT-03` (Almacén) | Cola dedicada de excepciones<br>Mensajería asíncrona<br>Sin escrituras en SAP | `PKG-05`<br>(Core en `PKG-02`) | `TO-BE`<br>`ACT-P5-01`<br>Semanas S17–S19 | `AC-PKG-05`<br>**GATE 3**<br>`ACT-03` (A), `ACT-06` (C) | Trazabilidad de retornos<br>Control de mermas<br>RFC Tipo 2 |
| **`REQ-FUN-001`** | Funcional<br>**Alta** | `OBJ-01`<br>`KPI-01` | `PROC-04`<br>`CUN-01` | `CMP-ING-01`<br>`CMP-VAL-03`<br>`ENT-04` | API Gateway B2B<br>CloudEvents v1.0<br>mTLS / JWT | `PKG-02`<br>`PKG-04` | `AT-1` / `AT-2`<br>`ACT-P2-01`, `ACT-P4-01`<br>S5–S10 / S11–S18 | `AC-PKG-02`<br>`AC-PKG-04`<br>**GATE 1** / **GATE 2** | Rechazo de malformados<br>Inspección de schema<br>RFC Tipo 2 |
| **`REQ-FUN-002`** | Funcional<br>**Alta** | `OBJ-03`<br>`KPI-03` | `PROC-04`<br>`CUN-02` | `CMP-COR-02`<br>`CMP-VAL-03`<br>`ENT-05` | Motor reactivo Quarkus/Spring<br>PostgreSQL particionado<br>Transacciones ACID | `PKG-02`<br>Core MTD | `AT-1`<br>`ACT-P2-02`<br>Semanas S7–S12 | `AC-PKG-02`<br>**GATE 1**<br>`ACT-14` (R), `ACT-12` (A) | Cero saltos causales<br>DLQ de transiciones erróneas<br>RFC Tipo 2 |
| **`REQ-FUN-003`** | Funcional<br>**Media** | `OBJ-04`<br>`KPI-04` | `PROC-04`<br>`CUN-04` | `CMP-ING-01`<br>`ENT-07`<br>`ACT-10` (Receptor) | Payload JSON extendido<br>DNI/Firma/Foto receptor<br>Cifrado AES-256 | `PKG-01`<br>`PKG-04` | `AT-2`<br>`ACT-P4-02`<br>Semanas S13–S18 | `AC-PKG-04`<br>**GATE 2**<br>`ACT-06` (A), `ACT-07` (R) | Verificación B2B (`ASM-01`)<br>Validación receptores<br>RFC Tipo 2 |
| **`REQ-FUN-004`** | Funcional<br>**Alta** | `OBJ-02`<br>`KPI-01` | `PROC-04`<br>`CUN-03` | `CMP-AUD-04`<br>`ENT-06`<br>`SIS-03` | Timestamping determinístico<br>NTP corporativo sincronizado<br>Persistencia inmutable | `PKG-02`<br>`PKG-05` | `AT-1` / `TO-BE`<br>`ACT-P2-01`, `ACT-P5-02`<br>S5–S10 / S17–S24 | `AC-PKG-02`<br>`AC-PKG-05`<br>**GATE 1** / **GATE 3** | Auditoría estricta de $T_1$<br>Prohibido alterar fórmula<br>RFC Tipo 2 (Alto Impacto) |
| **`REQ-FUN-005`** | Funcional<br>**Media** | `OBJ-06`<br>`KPI-03` | `PROC-05`<br>`CUN-06` | `CMP-NOT-06`<br>`ENT-08`<br>`ADP-SAP` (Staging) | Consola web para Lurín<br>Aviso en bus asíncrono<br>Ajuste físico en Garita | `PKG-05`<br>(Core en `PKG-02`) | `TO-BE`<br>`ACT-P5-01`<br>Semanas S17–S19 | `AC-PKG-05`<br>**GATE 3**<br>`ACT-03` (A - Veto) | Registro de incidencias<br>Inspección de bulto<br>RFC Tipo 2 |
| **`REQ-RNF-001`** | No Funcional<br>**Alta** | `OBJ-02`<br>`KPI-01`, `KPI-02` | `PROC-04`<br>`CUN-02` | `CMP-AUD-04`<br>`CMP-ING-01`<br>`ADP-LOG` | OpenTelemetry distribuido<br>Prometheus / Grafana<br>SLA $\le 30\text{ min}$ | `PKG-02`<br>`PKG-05` | `AT-1` / `TO-BE`<br>`ACT-P2-01`, `ACT-P5-02`<br>S5–S10 / S17–S24 | `AC-PKG-02`<br>`AC-PKG-05`<br>**GATE 1** / **GATE 3** | Reportes de degradación<br>Meta diseño vs. basal<br>Disparador RFC Tipo 3 |
| **`REQ-RNF-002`** | No Funcional<br>**Alta** | `OBJ-07`<br>`KPI-06` | Transversal<br>a procesos | `CMP-ING-01`<br>`CMP-COR-02`<br>`Cluster K8s` | Clúster Kubernetes Multi-AZ<br>Autoscaling (HPA)<br>RabbitMQ en clúster | `PKG-02`<br>Infraestructura | `AT-1`<br>`ACT-P2-03`<br>Semanas S9–S12 | `AC-PKG-02`<br>**GATE 1**<br>`ACT-13` (A), `ACT-12` (R) | Uptime mensual $\ge 99.5\%$<br>Health checks automáticos<br>RFC Tipo 1 |
| **`REQ-RNF-003`** | No Funcional<br>**Alta** | `OBJ-07`<br>`KPI-06` | Transversal<br>a procesos | API Gateway<br>Reverse Proxy<br>WAF Perimetral | TLS 1.3 / mTLS obligatorio<br>Tokens JWT (RS256)<br>Escaneo SAST/DAST | `PKG-02`<br>`PKG-04` | `AT-1` / `AT-2`<br>`ACT-P2-03`, `ACT-P4-01`<br>S9–S12 / S11–S18 | `AC-PKG-02`<br>`AC-PKG-04`<br>**GATE 1** / **GATE 2**<br>`ACT-11` (A - Veto) | Auditoría de vulnerabilidades<br>Cero fallas críticas<br>Veto por CISO |
| **`REQ-RNF-004`** | No Funcional<br>**Media** | `OBJ-04`<br>`FCE-05`<br>`KPI-04` | `PROC-02` a `05`<br>`CUN-01..03`, `05`<br>Sin doble digitación | Interfaces existentes<br>`SPY`, `NSDG`, `CELLFORCE`<br>Portal Contingencia | UI responsiva ergonómica<br>Autenticación única (SSO)<br>Terminales RF Almacén | `PKG-02`<br>`PKG-04` | `AT-2` / `TO-BE`<br>`ACT-P4-03`<br>S13–S24 | `AC-PKG-04`<br>**GATE 2**<br>`ACT-06` (A), `ACT-04` (C) | Tasa de errores operativos<br>Tiempo medio de captura en garita<br>RFC Tipo 1 |
| **`REQ-RES-001`** | Restricción<br>**Alta** | `OBJ-04`<br>`DEC-04` | `PROC-01` a `05`<br>Lurín a Sedes | Frontera MTD<br>Lurín a 24 Deptos.<br>Excluye B2C | Validación en Gateway de IDs<br>de distribuidoras/sedes B2B<br>Filtro de rutas corporativas | Transversal a<br>todos (`PKG-01..05`) | `AT-1`, `AT-2`, `TO-BE`<br>Permanente S1–S24 | Gobernanza continua<br>**GATE 1, 2 y 3**<br>`ACT-13` (A), `ACT-12` (R) | Prohibido desviar a B2C<br>Causal de rechazo de RFC<br>Disparador nuevo ADM |
| **`REQ-RES-002`** | Restricción<br>**Alta** | `OBJ-03`<br>`PAE-04` | `PROC-01`, `PROC-03`<br>Almacén Lurín | `ADP-SAP`<br>`SIS-01` (`SAP R3`)<br>Solo Lectura / Staging | DB Views indexadas<br>Tablas intermedias de paso<br>Cero permisos INSERT/UPDATE | `PKG-03`<br>Adaptador SAP | `AT-1`<br>`ACT-P3-01`<br>Semanas S5–S10 | `AC-PKG-03`<br>**GATE 1**<br>`ACT-03` (A - Veto Absoluto) | Auditoría de trazas SQL<br>Alerta roja ante escritura<br>Veto de Almacén |
| **`REQ-RES-003`** | Restricción<br>**Alta** | `OBJ-08`<br>`PAE-05` | Transversal<br>a integración | `ADP-SAP`, `ADP-SPY`<br>`ADP-CRM`, `ADP-LOG`<br>`SIS-05` (Bus) | Reutilización de APIs existentes<br>Adaptadores modulares<br>Sin reescritura de core legacy | `PKG-03`<br>`PKG-04` | `AT-1` / `AT-2`<br>`ACT-P3-01..03`, `ACT-P4-01`<br>S5–S16 / S11–S18 | `AC-PKG-03`<br>`AC-PKG-04`<br>**GATE 1** / **GATE 2** | Preservación de inversiones<br>Monitoreo de adaptadores<br>RFC Tipo 2 |
| **`REQ-RES-004`** | Restricción<br>**Alta** | `OBJ-01`<br>`ASM-05` | `PROC-04`<br>Ruta Externa | `ADP-LOG`<br>Portal Web MTD<br>`ACT-07` (Terceros) | Interoperabilidad abierta REST<br>Carga web de contingencia<br>Sin hardware GPS propietario | `PKG-04`<br>Pasarela B2B | `AT-2`<br>`ACT-P4-01`, `ACT-P4-02`<br>Semanas S11–S18 | `AC-PKG-04`<br>**GATE 2**<br>`ACT-06` (A), `ACT-07` (C) | Prohibido exigir hardware<br>Homologación en Sandbox<br>RFC Tipo 2 |
| **`REQ-INT-001`** | Integración<br>**Alta** | `OBJ-01`<br>`KPI-01` | `PROC-01`, `PROC-03`<br>Lurín Despacho | `ADP-SAP`<br>`ADP-SPY`<br>`SIS-01`, `SIS-02` | Consultas no bloqueantes<br>Extracción batch ligero ($\le 15\text{ min}$)<br>Idempotencia por UA/Pedido | `PKG-03`<br>Integración | `AT-1`<br>`ACT-P3-01`, `ACT-P3-02`<br>Semanas S5–S10 | `AC-PKG-03`<br>**GATE 1**<br>`ACT-03` (A), `ACT-12` (R) | Control de tiempos de extracción<br>Cero locks en Lurín<br>RFC Tipo 2 |
| **`REQ-INT-002`** | Integración<br>**Media** | `OBJ-05`<br>`KPI-05` | `PROC-05`<br>Servicio Cliente | `ADP-CRM`<br>`CMP-PUB-05`<br>`SIS-09` (`CELLFORCE`) | Webhook / API REST CRM<br>Colas de despacho asíncrono<br>Resiliencia ante caída de CRM | `PKG-03`<br>Integración CRM | `AT-2`<br>`ACT-P3-03`<br>Semanas S11–S16 | `AC-PKG-03`<br>**GATE 2**<br>`ACT-08` (C), `ACT-12` (R) | Sincronización en $\le 5\text{ seg}$<br>Retención en cola si cae CRM<br>RFC Tipo 1 |
| **`REQ-INT-003`** | Integración<br>**Alta** | `OBJ-01`<br>`KPI-02` | `PROC-04`<br>Transportistas | `ADP-LOG`<br>`CMP-ING-01`<br>`ACT-07` (Sistemas) | Contratos OpenAPI 3.0<br>CloudEvents v1.0 JSON<br>Ambiente Sandbox de pruebas | `PKG-01`<br>`PKG-04` | `AT-2`<br>`ACT-P1-01`, `ACT-P4-01`<br>S1–S4 / S11–S18 | `AC-PKG-01`<br>`AC-PKG-04`<br>**GATE 2** | Pruebas de compatibilidad<br>Deprecación con 6 meses aviso<br>RFC Tipo 2 |
| **`REQ-INT-004`** | Integración<br>**Alta** | `OBJ-04`<br>`OBJ-08`<br>`KPI-04` | Transversal<br>`PROC-01` a `05`<br>`CUN-01..03`, `05` | `ADP-SAP`, `ADP-SPY`<br>`ADP-CRM`, `ADP-LOG`<br>`SIS-01` a `SIS-10` | Reutilización de APIs legadas<br>Conectores desacoplados (ACL)<br>OpenAPI 3.0 / CloudEvents | `PKG-01`<br>`PKG-03`<br>`PKG-04` | `AT-1` / `AT-2`<br>`ACT-P1-01`, `ACT-P3-01..03`<br>S1–S18 | `AC-PKG-01`<br>`AC-PKG-03`<br>`AC-PKG-04`<br>**GATE 1** y **GATE 2** | Compatibilidad de versiones de API<br>Cero roturas de integración<br>RFC Tipo 2 |
| **`REQ-ARQ-001`** | Arquitectura<br>**Alta** | `OBJ-03`<br>`OBJ-08` | Transversal<br>`SIS-05` (Bus) | `Bus de Integración`<br>Event Broker MTD<br>Conectores desacoplados | Arquitectura Event-Driven (EDA)<br>Colas asíncronas RabbitMQ<br>Desacoplamiento temporal | `PKG-02`<br>`PKG-03` | `AT-1`<br>`ACT-P2-01`, `ACT-P3-01`<br>S5–S12 | `AC-PKG-02`<br>`AC-PKG-03`<br>**GATE 1** | Tolerancia a fallos de bus<br>DLQ para mensajes huérfanos<br>RFC Tipo 2 |
| **`REQ-ARQ-002`** | Arquitectura<br>**Alta** | `OBJ-01`<br>`OBJ-03` | `PROC-01` a `04`<br>Picking a Ruta | `CMP-COR-02`<br>`ENT-01`, `ENT-02`<br>`ENT-04` | Correlación UA - Caja - Pedido<br>Clave única UUIDv4<br>Estructura jerárquica | `PKG-02`<br>Core MTD | `AT-1`<br>`ACT-P2-02`<br>Semanas S7–S12 | `AC-PKG-02`<br>**GATE 1**<br>`ACT-12` (A), `ACT-04` (C) | Integridad referencial<br>Control de bultos parciales<br>RFC Tipo 2 |
| **`REQ-ARQ-003`** | Arquitectura<br>**Media** | `OBJ-08`<br>`FCE-04` | Operaciones<br>Nacionales | API Gateway B2B<br>Reverse Proxy WAF<br>Multi-transportista | Normalización de payloads<br>Reglas de transformación adaptativa<br>Aislamiento de protocolos de terceros | `PKG-04`<br>Pasarela B2B | `AT-2`<br>`ACT-P4-01`<br>S11–S18 | `AC-PKG-04`<br>**GATE 2**<br>`ACT-12` (A), `ACT-16` (R) | Adaptador universal de ingesta<br>Nuevos operadores sin tocar Core<br>RFC Tipo 2 |

---

### 2.2. Criterios Técnicos Derivados y Condiciones de Transición (Quality Gates)

Los siguientes parámetros corresponden a condiciones técnicas de transición y criterios de aceptación derivados para los *Quality Gates*, los cuales operan subordinados a los 24 requerimientos canónicos oficiales sin constituir requerimientos independientes del catálogo maestro:

* **`CRT-TRA-01` (Mapeo Semántico y Sandbox de Integración):** Certificación de equivalencias en esquemas JSON Schema y validación en entorno Sandbox durante Semanas S1–S4 (`PKG-01`). Criterio derivado de `REQ-INT-004` y `REQ-ARQ-001`.
* **`CRT-TRA-02` (Kit de Integración B2B y Postman Collections):** Certificación técnica previa de operadores de transporte en ambiente Sandbox (`PKG-04`, Semanas S11–S14). Criterio derivado de `REQ-INT-003` y `REQ-RES-004`.
* **`CRT-TRA-03` (Tolerancia Store-and-Forward ante Desconexión):** Retención local en SQLite de apps de transportistas para rutas remotas sin cobertura (`PKG-04`, Semanas S13–S18). Criterio derivado de `REQ-RNF-001` y `REQ-RES-004` (`ASM-16`).
* **`CRT-TRA-04` (Portal Web de Contingencia Operativa):** Interfaz web protegida con 2FA para carga manual de eventos de excepción (`PKG-04`, Semanas S15–S18). Criterio derivado de `REQ-RES-004` y `REQ-RNF-004`.
* **`CRT-TRA-05` (Batería Integral de Pruebas de Carga y Simulación):** Pruebas de estrés a 100 req/s, simulacro de corte de bus y certificación de no regresión en Staging (`PKG-01..05`). Criterio derivado de `REQ-RNF-002` y `REQ-ARQ-003`.

---

### 2.3. Criterios Técnicos Derivados de Seguridad y Auditoría Corporativa

* **`CRT-SEG-01` (Seguridad Perimetral y Cifrado):** Canales cifrados TLS 1.3, autenticación mutua mTLS o tokens JWT (RS256) con inspección WAF en el API Gateway B2B. Criterio derivado de `REQ-RNF-003`.
* **`CRT-SEG-02` (Aislamiento de Privilegios y Staging):** Conexión no transaccional de solo lectura `SELECT` y aislamiento absoluto de escritura en `SAP R3`. Criterio derivado de `REQ-RNF-003` y `REQ-RES-002`.
* **`CRT-SEG-03` (Inmutabilidad de Pistas de Auditoría y SLA):** Retención inalterable de trazas durante 12 meses y prohibición estricta de alteración de marcas temporales $T_1$ o cálculo de latencia $\Delta T$. Criterio derivado de `REQ-RNF-003` y `REQ-ARQ-003`.

---

## 3. Síntesis Estadística de la Trazabilidad

* **Total de Requerimientos Canónicos Trazados:** 24 de 24 ($100.0\%$).
* **Distribución Rigurosa por Tipología Oficial (Matriz Maestra `01_5`):**
  * Requerimientos de Negocio (`REQ-NEG-*`): **4 registros** ($16.7\%$).
  * Requerimientos Funcionales (`REQ-FUN-*`): **5 registros** ($20.8\%$).
  * Requerimientos de Integración (`REQ-INT-*`): **4 registros** ($16.7\%$).
  * Requerimientos de Calidad / No Funcionales (`REQ-RNF-*`): **4 registros** ($16.7\%$).
  * Requerimientos de Arquitectura (`REQ-ARQ-*`): **3 registros** ($12.5\%$).
  * Restricciones Ineludibles (`REQ-RES-*`): **4 registros** ($16.7\%$).
  * **Suma Total Exacta:** $4 + 5 + 4 + 4 + 3 + 4 = \mathbf{24\text{ requerimientos}}$ ($100.0\%$).
* **Criterios Técnicos Derivados Subordinados:**
  * Criterios de Transición y Calidad Operativa (`CRT-TRA-*`): 5 criterios asociados a Gates.
  * Criterios de Seguridad Informática y Auditoría (`CRT-SEG-*`): 3 criterios técnicos derivados de `REQ-RNF-003`.
* **Cobertura por Paquetes de Trabajo (`PKG-01` a `PKG-05`):** $100\%$ de los requerimientos cuentan con asignación unívoca y verificable en las fichas de trabajo.
* **Cobertura por Puntos de Control (*Quality Gates*):**
  * `GATE 1` (Semana S12 — Certificación AT-1): Requerimientos fundacionales certificados (`REQ-NEG-001`, `REQ-NEG-002`, `REQ-FUN-001`, `REQ-FUN-002`, `REQ-FUN-004`, `REQ-INT-001`, `REQ-INT-004`, `REQ-RNF-001..003`, `REQ-ARQ-001..002`, `REQ-RES-001..003`).
  * `GATE 2` (Semana S18 — Certificación AT-2): Requerimientos de interoperabilidad y piloto certificados (`REQ-NEG-003`, `REQ-FUN-003`, `REQ-INT-002`, `REQ-INT-003`, `REQ-INT-004`, `REQ-RNF-003..004`, `REQ-ARQ-003`, `REQ-RES-004`).
  * `GATE 3` (Semana S24 — Certificación TO-BE): Requerimientos de cierre nacional, logística inversa y telemetría certificados (`REQ-NEG-004`, `REQ-FUN-005`, `REQ-RNF-001`, `REQ-RNF-004`).

