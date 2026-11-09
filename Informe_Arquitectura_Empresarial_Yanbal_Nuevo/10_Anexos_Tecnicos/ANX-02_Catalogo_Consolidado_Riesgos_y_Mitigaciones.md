# ANEXO 2: CATÁLOGO CONSOLIDADO DE RIESGOS DE TRANSFORMACIÓN Y OPERACIÓN ARQUITECTÓNICA

> **Ubicación oficial:** `10_Anexos_Tecnicos/ANX-02_Catalogo_Consolidado_Riesgos_y_Mitigaciones.md`  
> **Documento maestro articulado:** [`ANEXOS_GENERALES_PROYECTO.md`](ANEXOS_GENERALES_PROYECTO.md)  
> **Fuentes de verdad arquitectónica:** Fases A, E, F y H de TOGAF ADM, y [`00_3_Registro_Supuestos_Incertidumbres_y_Decisiones.md`](../00_Referencias_Glosario_y_Decisiones/00_3_Registro_Supuestos_Incertidumbres_y_Decisiones.md).  
> **Propósito:** Centralizar en un marco analítico unificado la totalidad de riesgos arquitectónicos, operacionales, de migración y de cambio identificados a lo largo del ciclo ADM para Yanbal Perú (Unique S.A.), evaluando su probabilidad, impacto, severidad, medidas preventivas, planes de contingencia y responsables canónicos.

---

## 1. Metodología de Evaluación y Criterios de Severidad

La gestión del riesgo en la arquitectura empresarial de Yanbal Perú sigue las directrices de TOGAF ADM y la norma ISO 31000. Los riesgos se evalúan cualitativa y cuantitativamente mediante una escala ordinal de 5 niveles para la Probabilidad ($P$) y el Impacto ($I$):
* **Probabilidad ($P$):** 1 (Muy Baja), 2 (Baja), 3 (Media), 4 (Alta), 5 (Muy Alta).
* **Impacto ($I$):** 1 (Insignificante), 2 (Menor), 3 (Moderado), 4 (Mayor), 5 (Catastrófico).
* **Severidad ($S = P \times I$):**
  * **Crítica ($16 - 25$):** Exige mitigación arquitectónica inmediata y supervisión ejecutiva (`ACT-13`).
  * **Alta ($10 - 15$):** Requiere controles técnicos específicos y verificación en Quality Gates.
  * **Media ($5 - 9$):** Administrada mediante planes operativos y procedimientos estándar.
  * **Baja ($1 - 4$):** Monitoreada de forma rutinaria sin asignación de recursos adicionales.

---

## 2. Matriz Consolidada de Riesgos del Proyecto (14 Registros Oficiales)

A continuación se presenta el catálogo unificado de riesgos, integrando los riesgos estratégicos de la Fase A (`RSK-01` a `RSK-06`), los riesgos de implementación y transición de las Fases E y F (`RSK-07` a `RSK-10`), y los riesgos operacionales de régimen continuo de la Fase H (`RSK-CHG-01` a `RSK-CHG-04`):

| Código | Denominación y Descripción del Riesgo | Fase de Origen | P | I | Severidad ($P \times I$) | Medida de Mitigación Preventiva | Plan de Contingencia / Recuperación | Actor Responsable Canónico | Indicadores y Objetivos Vinculados | Estado Actual |
| :---: | :--- | :---: | :---: | :---: | :---: | :--- | :--- | :---: | :---: | :---: |
| **`RSK-01`** | **Imposibilidad de medir línea base fiable por falta de marcas temporales duales.**<br>Carencia de sellos $T_0$ y $T_1$ en los sistemas legados, impidiendo calcular la latencia real histórica. | Fase A | 4 | 4 | **16 (Crítica)** | Muestreo controlado manual durante la Fase Preliminar; despliegue prioritario de `CMP-AUD-04` en AT-1. | Utilizar como referencia la evidencia cualitativa de la entrevista (~2 horas) sin asumir un número cerrado (`ASM-03`). | `ACT-12` (Arquitecto)<br>`ACT-06` (Distribución) | `OBJ-01`, `OBJ-02`<br>`KPI-01`, `KPI-02` | Mitigado en AT-1 |
| **`RSK-02`** | **Desfase originado en el reporte de transportistas externos fuera del control de Yanbal.**<br>La latencia no reside en los sistemas internos sino en la demora de captura de las empresas tercerizadas. | Fase A | 4 | 4 | **16 (Crítica)** | Involucrar a los transportistas (`ACT-07`) en el Comité de Interoperabilidad; diseñar APIs REST abiertas y Portal Web. | Cláusulas de SLA vinculantes en contratos comerciales; incentivos operativos por reporte oportuno. | `ACT-06` (Distribución)<br>`ACT-07` (Transporte) | `OBJ-02`<br>`KPI-01`, `KPI-02` | Controlado en AT-2 / TO-BE |
| **`RSK-03`** | **Diseño arquitectónico basado en supuestos erróneos sobre `NSDG`, `ENSDY` y `DRIVING`.**<br>Duplicidad de adaptadores o conexiones erróneas por ambigüedad de nombres en transcripción. | Fase A | 3 | 4 | **12 (Alta)** | Mantener identidades separadas (`SIS-06`, `SIS-07`, `SIS-08`) hasta validación oficial (`ASM-02`, `ASM-07`, `ASM-11`). | Arquitectura modular de adaptadores que permita conmutar endpoints sin alterar el Core MTD. | `ACT-12` (Arquitecto)<br>`ACT-16` (Proveedores TI) | `OBJ-03`, `OBJ-08`<br>`KPI-03`, `KPI-04` | Mitigado en Fase C/D |
| **`RSK-04`** | **Rechazo o retraso en la certificación de Seguridad Informática de interfaces con terceros.**<br>Bloqueo de despliegues perimetrales por políticas estrictas de ciberseguridad corporativa. | Fase A | 3 | 4 | **12 (Alta)** | Integrar al CISO (`ACT-11`) en el Architecture Board; adoptar estándares rigurosos (TLS 1.3, mTLS, JWT RS256). | Pruebas de penetración preliminares en Staging y escaneos SAST/DAST continuos en pipeline CI/CD. | `ACT-11` (CISO)<br>`ACT-14` (QA) | `OBJ-07`<br>`KPI-06`, `REQ-RNF-003` | Mitigado en Gate 1 |
| **`RSK-05`** | **Alcance B2B mal delimitado que contamine el diseño con lógica de última milla B2C.**<br>Presión para incluir reparto a consultoras/consumidoras, sobrecargando la solución. | Fase A | 3 | 5 | **15 (Alta)** | Regla vinculante de alcance (`REQ-RES-001`, `DEC-04`); validación estricta de destinos empresariales B2B. | Filtro a nivel de API Gateway que rechace transacciones con IDs de destino no pertenecientes a distribuidoras. | `ACT-13` (Dirección)<br>`ACT-12` (Arquitecto) | `OBJ-04`<br>`REQ-RES-001` | Cerrado (Veto formal) |
| **`RSK-06`** | **Intermitencia de conectividad de datos en rutas remotas terrestres y bimodales.**<br>Zonas de sombra celular en selva y sierra que impiden la transmisión inmediata de eventos. | Fase A | 4 | 3 | **12 (Alta)** | Exigir mecanismo *Store-and-Forward* en aplicativos móviles de transportistas (`CRT-TRA-03` derivado de `REQ-RES-004` y `REQ-RNF-001`, `ASM-16`). | Reintento automático con backoff exponencial al recuperar señal; tolerancia técnica operativa $\ge 95\%$ en piloto. | `ACT-07` (Transporte)<br>`ACT-12` (Arquitecto) | `OBJ-02`, `OBJ-08`<br>`KPI-01`, `KPI-02` | Mitigado en Gate 2 |
| **`RSK-07`** | **Contención transaccional o degradación de rendimiento en `SAP R3` por lecturas del MTD.**<br>Consultas de sincronización que bloqueen tablas maestras durante el picking en Lurín. | Fase E/F | 2 | 5 | **10 (Alta)** | Lectura exclusiva sobre tablas intermedias de staging (`ZTRK_DESPACHO_STG`) y vistas indexadas desacopladas. | Desconexión preventiva automática de `ADP-SAP` ante incremento del tiempo de respuesta en Lurín. | `ACT-03` (Almacén)<br>`ACT-12` (Arquitecto) | `OBJ-03`<br>`REQ-RES-002` | Mitigado en Gate 1 |
| **`RSK-08`** | **Sobrecarga o saturación en el Bus de Integración (`SIS-05`) durante picos de campaña.**<br>Volúmenes masivos de pedidos que retrasen la distribución de eventos hacia los satélites. | Fase E/F | 3 | 4 | **12 (Alta)** | Despliegue de colas de eventos asíncronas dedicadas (RabbitMQ clúster) con autoscaling horizontal. | Buffer en colas de persistencia temporal; descarte ordenado de eventos duplicados mediante clave `UUIDv4`. | `ACT-12` (Arquitecto)<br>`ACT-13` (Gerente TI) | `OBJ-07`<br>`KPI-06`, `REQ-RNF-002` | Mitigado en AT-1 |
| **`RSK-09`** | **Desfase semántico y desalineación de esquemas JSON durante el onboarding de transportistas.**<br>Payloads heterogéneos enviados por transportistas que causen errores en `CMP-ING-01`. | Fase E/F | 3 | 3 | **9 (Media)** | Publicación de esquemas JSON Schema draft-07 y especificación OpenAPI 3.0 con kit de integración en Postman. | Validación sintáctica automática en API Gateway; derivación de malformados a cola Dead Letter Queue (DLQ). | `ACT-14` (QA)<br>`ACT-16` (TI Transportistas) | `OBJ-08`<br>`KPI-07`, `REQ-INT-003` | Mitigado en Gate 2 |
| **`RSK-10`** | **Resistencia operativa de transportistas a utilizar interfaces digitales de reporte.**<br>Choferes que omiten el registro de entrega o postergan la carga manual de eventos. | Fase E/F | 3 | 4 | **12 (Alta)** | Portal Web de Contingencia optimizado para dispositivos móviles con interfaz intuitiva y carga simplificada. | Supervisión activa por Supervisores de Ruta (`ACT-04`); condicionamiento de liquidación de fletes al reporte. | `ACT-06` (Distribución)<br>`ACT-07` (Transporte) | `OBJ-04`<br>`KPI-04` | Mitigado en AT-2 |
| **`RSK-CHG-01`** | **Ruptura de interoperabilidad con transportistas por cambios no coordinados en APIs.**<br>Modificaciones intempestivas en esquemas de eventos que corten el flujo de trazabilidad. | Fase H | 3 | 4 | **12 (Alta)** | Política de gestión de cambios (RFC Tipo 2); versionado semántico de APIs y deprecación obligatoria de 6 meses. | Pruebas de contrato automatizadas (*Contract Testing* con Pact); entorno Sandbox de homologación permanente. | `ACT-12` (Arquitecto)<br>`ACT-16` (TI Transportistas) | `OBJ-08`<br>`KPI-02` | Controlado en Fase H |
| **`RSK-CHG-02`** | **Contención de recursos en base de datos de Lurín por ajustes ineficientes de consultas.**<br>Nuevas vistas o consultas analíticas mal diseñadas que degraden el rendimiento transaccional. | Fase H | 2 | 4 | **8 (Media)** | Revisión obligatoria de sentencias SQL por el Administrador de Base de Datos; pruebas de carga previas. | Veto técnico vinculante de `ACT-03` (Almacén); apagado inmediato del proceso de consulta. | `ACT-03` (Almacén)<br>`ACT-12` (Arquitecto) | `OBJ-03`<br>`REQ-RES-002` | Controlado en Fase H |
| **`RSK-CHG-03`** | **Fatiga de cambios en empresas de transporte por modificaciones continuas de sistemas.**<br>Desinterés o demoras de transportistas para adaptarse a nuevas versiones de la plataforma. | Fase H | 3 | 3 | **9 (Media)** | Convocatorias previas en el Comité de Integración B2B; boletines técnicos con 30 días de anticipación. | Soporte asistido por la PMO (`ACT-15`) y sesiones de capacitación técnica a enlaces de transportistas. | `ACT-06` (Distribución)<br>`ACT-15` (PMO) | `OBJ-08`<br>`KPI-04` | Controlado en Fase H |
| **`RSK-CHG-04`** | **Divergencia entre la documentación de arquitectura y la realidad en producción (*Shadow Architecture*).**<br>Parches directos o atajos operativos no reflejados en el Repositorio de Arquitectura (EAR). | Fase H | 3 | 4 | **12 (Alta)** | Disciplina GitOps (ArgoCD); sincronización obligatoria del EAR antes de dar por cerrada cualquier RFC. | Auditorías semestrales de arquitectura física vs. modelos PlantUML; congelamiento de credenciales directas. | `ACT-12` (Arquitecto)<br>`ACT-14` (QA) | Transversal a<br>todos los `OBJ` | Controlado en Fase H |

---

## 3. Mapa de Calor de Riesgos Arquitectónicos (Matriz $5 \times 5$)

```
+---+---+---+---+---+---+
| 5 |   |   |   |R05|   |  P = 5 (Muy Alta)
+---+---+---+---+---+---+
| 4 |   |   |R06|R01|   |  P = 4 (Alta)
|   |   |   |   |R02|   |
+---+---+---+---+---+---+
| 3 |   |   |R09|R03|   |  P = 3 (Media)
|   |   |   |C03|R04|   |
|   |   |   |   |R08|   |
|   |   |   |   |R10|   |
|   |   |   |   |C01|   |
|   |   |   |   |C04|   |
+---+---+---+---+---+---+
| 2 |   |   |   |C02|R07|  P = 2 (Baja)
+---+---+---+---+---+---+
| 1 |   |   |   |   |   |  P = 1 (Muy Baja)
+---+---+---+---+---+---+
    | 1 | 2 | 3 | 4 | 5 |  Impacto (I)
```

**Distribución de Severidad Consolidada:**
* **Severidad Crítica ($16 - 25$):** 2 riesgos (`RSK-01`, `RSK-02`), correspondientes a las dependencias de línea base y reporte externo.
* **Severidad Alta ($10 - 15$):** 9 riesgos (`RSK-03`, `RSK-04`, `RSK-05`, `RSK-06`, `RSK-07`, `RSK-08`, `RSK-10`, `RSK-CHG-01`, `RSK-CHG-04`), gestionados activamente mediante Quality Gates y salvaguardas contractuales.
* **Severidad Media ($5 - 9$):** 3 riesgos (`RSK-09`, `RSK-CHG-02`, `RSK-CHG-03`), administrados mediante procedimientos operativos de soporte y monitoreo.
* **Severidad Baja ($1 - 4$):** 0 riesgos.

