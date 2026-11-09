# AUDITORÍA INTEGRAL DE RIESGOS DE ARQUITECTURA
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Evaluación Metodológica de 14 Riesgos Canónicos (RSK-01 a RSK-10 y RSK-CHG-01 a RSK-CHG-04)

---

## 1. Introducción y Marco Metodológico de Auditoría de Riesgos

El presente documento constituye la entrega actualizada de **REV_05: Auditoría Integral de Riesgos de Arquitectura**, elaborada para garantizar la consistencia, trazabilidad y rigor conceptual de la totalidad de eventos de riesgo identificados en el proyecto de Arquitectura Empresarial de Yanbal Perú (Unique S.A.).

### 1.1. Alcance de la Auditoría
Se audita la totalidad de los catorce riesgos canónicos documentados a lo largo del ciclo TOGAF ADM:
- **Fase A (Riesgos Estratégicos y de Contexto):** `RSK-01` a `RSK-06`.
- **Fases E y F (Riesgos de Ingeniería, Integración y Transición):** `RSK-07` a `RSK-10`.
- **Fase H (Riesgos de Gobernanza Continua y Gestión del Cambio):** `RSK-CHG-01` a `RSK-CHG-04`.

### 1.2. Justificación de la Escala Propia del Proyecto ($5 \times 5$)
La valoración no adopta probabilidades estadísticas abstractas no sustentadas, sino una **escala ordinal propia definida para el proyecto**, alineada con el estándar **ISO 31000:2018** y las directrices de TOGAF ADM:

1. **Probabilidad de Ocurrencia ($P$):** Escala cualitativo-ordinal de 1 a 5:
   - **1 (Muy Baja):** Evento altamente improbable en el contexto de Yanbal Perú; condiciones de ocurrencia excepcionales.
   - **2 (Baja):** Escenario poco frecuente; antecedentes aislados en operaciones similares.
   - **3 (Media):** Evento factible bajo condiciones normales de despacho y transporte interprovincial.
   - **4 (Alta):** Evento altamente probable o recurrente en la operación actual (Línea Base AS-IS).
   - **5 (Muy Alta):** Certeza operativa si no se implementa una intervención arquitectónica formal.
2. **Impacto Operacional y de Negocio ($I$):** Escala cualitativo-ordinal de 1 a 5:
   - **1 (Insignificante):** Desviación menor absorbible por la operación sin impacto en Consultoras ni costos perceptibles.
   - **2 (Menor):** Afectación localizada que no compromete los plazos prometidos de entrega.
   - **3 (Moderado):** Retraso de hasta 30 minutos en visibilidad o incremento puntual de consultas al Call Center.
   - **4 (Mayor):** Incumplimiento del SLA objetivo ($\le 30\text{ min}$), retraso $> 60\text{ min}$ en seguimiento o afectación a múltiples zonas.
   - **5 (Catastrófico / Crítico):** Paralización del despacho nacional, bloqueo transaccional de `SAP R3` o quiebre del canal B2B.
3. **Severidad del Riesgo ($S = P \times I$):**
   - **Baja ($1 - 4$):** Monitoreo rutinario sin asignación presupuestal extraordinaria.
   - **Media ($5 - 9$):** Mitigación mediante procedimientos operativos estándar y controles preventivos en diseño.
   - **Alta ($10 - 15$):** Mitigación técnica obligatoria con verificación vinculante en Quality Gates.
   - **Crítica ($16 - 25$):** Máxima prioridad; veto técnico del Architecture Board y planes de contingencia activos.

### 1.3. Regla Metodológica sobre el Estado de los Riesgos
> **Principio de Honestidad Académica:**  
> Como la arquitectura empresarial se encuentra en fase de **diseño y planificación de la migración (Fases F/G)**, los controles técnicos diseñados en los paquetes `PKG-01..05` aún no han sido desplegados físicamente en producción. Por ende:
> - **Riesgo Inherente ($P_{inh} \times I_{inh}$):** Evaluación en la línea base en ausencia de la arquitectura propuesta.
> - **Riesgo Residual Estimado / Meta ($P_{res} \times I_{res}$):** Nivel proyectado que se alcanzará una vez que los paquetes de trabajo sean certificados formalmente en las Puertas de Calidad (Gate 1, Gate 2, Gate 3).
> - **Riesgo Residual Observado:** No disponible; sujeto a verificación post-rollout en Fase H.
> - **PROHIBIDO calificar riesgos como "mitigados en producción"**: Su estado se define con precisión como **«Planificado / En Diseño de Mitigación»** o **«Bajo Control de Diseño (Gate X)»**.

---

## 2. Matriz Auditada de los 14 Riesgos Canónicos

A continuación se presenta el catálogo consolidado de riesgos, preservando con absoluta fidelidad la identidad histórica y textual de las fuentes originales:

| Código Canónico | Categoría | Evento de Riesgo y Causa Raíz Original | Consecuencia Operacional / Negocio | $P_{inh}$ | $I_{inh}$ | $S_{inh}$ | Paquete / Fase | Responsable Canónico | Control Preventivo Diseñado | Plan de Contingencia / Recuperación | $P_{res}$ (Meta) | $I_{res}$ (Meta) | $S_{res}$ (Meta) | Estado de Gobernanza Actual |
| :---: | :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :--- | :--- | :---: | :---: | :---: | :---: |
| **`RSK-01`** | Operativo / Métrico | **Imposibilidad de medir línea base fiable por falta de marcas temporales duales.** Causa: Sistemas legados no capturan la dupla $T_0$ y $T_1$. | Incertidumbre sobre el desfase real histórico; dificultad para auditar el cumplimiento del SLA de 30 min. | 4 | 4 | **16 (Crítica)** | Fase A / PKG-02 | `ACT-12` (Arquitecto)<br>`ACT-06` (Distribución) | Despliegue de sonda `CMP-AUD-04` en AT-1 para captura estandarizada de marcas duales. | Muestreo manual asistido; tomar como referencia cualitativa el retardo observado de $\sim 2\text{ horas}$ (`ASM-03`). | 1 | 2 | **2 (Baja)** | Diseñado para AT-1 (Gate 1) |
| **`RSK-02`** | Operativo / Terceros | **Desfase originado en el reporte de transportistas externos fuera del control de Yanbal.** Causa: Demora de choferes en consolidar y transmitir las entregas. | Persistencia del desfase de seguimiento a pesar de que el núcleo MTD interno funcione correctamente. | 4 | 4 | **16 (Crítica)** | Fase A / PKG-04 | `ACT-06` (Distribución)<br>`ACT-07` (Transporte) | Diseño de pasarela `ADP-LOG`, APIs REST y Portal Web de Contingencia optimizado para móviles. | Cláusulas contractuales vinculantes con penalidades e incentivos por reporte oportuno. | 2 | 2 | **4 (Baja)** | Diseñado para AT-2 (Gate 2) |
| **`RSK-03`** | Técnico / Arquitectura | **Diseño arquitectónico basado en supuestos erróneos sobre `NSDG`, `ENSDY` y `DRIVING`.** Causa: Ambigüedad en transcripciones de sistemas satélites de transporte. | Construcción de adaptadores redundantes o endpoints fallidos con sobrecostos de desarrollo. | 3 | 4 | **12 (Alta)** | Fase A / PKG-04 | `ACT-12` (Arquitecto)<br>`ACT-16` (Proveedores TI) | Mantener identidades modulares independientes (`SIS-06`, `SIS-07`, `SIS-08`) hasta validación formal (`ASM-02`). | Arquitectura modular desacoplada que permita reconfigurar endpoints sin alterar el core del MTD. | 1 | 2 | **2 (Baja)** | Control de Diseño (Gate 1) |
| **`RSK-04`** | Ciberseguridad | **Rechazo o retraso en la certificación de Seguridad Informática de interfaces con terceros.** Causa: Políticas estrictas de ciberseguridad corporativa. | Bloqueo del pase a producción perimetral por observaciones de seguridad no resueltas. | 3 | 4 | **12 (Alta)** | Fase A / PKG-01 | `ACT-11` (CISO)<br>`ACT-14` (QA) | Inclusión del CISO en el Architecture Board; adopción de TLS 1.3, mTLS y tokens JWT validados. | Entorno de Staging para auditorías de penetración y escaneos SAST/DAST continuos en CI/CD. | 1 | 3 | **3 (Baja)** | Control de Diseño (Gate 1) |
| **`RSK-05`** | Negocio / Alcance | **Alcance B2B mal delimitado que contamine el diseño con lógica de última milla B2C.** Causa: Presión de áreas comerciales para rastrear reparto a consumidor final. | Sobrecarga del MTD, desnaturalización del proyecto B2B y desfase de cronograma. | 3 | 5 | **15 (Alta)** | Fase A / Alcance | `ACT-13` (Gerencia TI)<br>`ACT-12` (Arquitecto) | Veto técnico formal mediante principio `REQ-RES-001` y decisión vinculante `DEC-04`. | Filtrado en API Gateway que descarte cualquier transacción con destinatario no empresarial. | 1 | 4 | **4 (Baja)** | Veto Aprobado (`DEC-04`) |
| **`RSK-06`** | Tecnológico / Red | **Intermitencia de conectividad de datos en rutas remotas terrestres y bimodales.** Causa: Sombras de cobertura celular en carreteras de selva y sierra. | Pérdida o retraso de eventos de ruta transmitidos desde las unidades de transporte. | 4 | 3 | **12 (Alta)** | Fase A / PKG-04 | `ACT-07` (Transporte)<br>`ACT-12` (Arquitecto) | Exigir mecanismo *Store-and-Forward* en aplicativos móviles de transportistas (`ASM-16`, `CRT-TRA-03`). | Reintento automático con algoritmo de retroceso exponencial al recuperar señal 4G/5G. | 2 | 2 | **4 (Baja)** | Diseñado para AT-2 (Gate 2) |
| **`RSK-07`** | Tecnológico / Core | **Contención transaccional o degradación de rendimiento en `SAP R3` por lecturas del MTD.** Causa: Consultas de lectura intensiva sobre tablas maestras de Lurín. | Congelamiento de tareas de picking y facturación; afectación a la operación central de la planta. | 2 | 5 | **10 (Alta)** | Fase E / PKG-03 | `ACT-03` (Almacén)<br>`ACT-12` (Arquitecto) | Principio de aislamiento `REQ-RES-002`: lectura exclusiva sobre tablas desacopladas de staging (`ZTRK_*`). | Cortocircuito automático (Circuit Breaker) que inhabilita `ADP-SAP` ante incremento de latencia en SAP. | 1 | 4 | **4 (Baja)** | Diseñado para AT-1 (Gate 1) |
| **`RSK-08`** | Tecnológico / Capacidad | **Sobrecarga o saturación en el Bus de Integración (`SIS-05`) durante picos de campaña.** Causa: Picos de volumen transaccional en días de cierre comercial. | Encolamiento de eventos, retraso en notificaciones y degradación del bus corporativo. | 3 | 4 | **12 (Alta)** | Fase E / PKG-01 | `ACT-12` (Arquitecto)<br>`ACT-13` (Gerente TI) | Despliegue de colas de eventos asíncronas dedicadas y buffer persistente en disco con alta disponibilidad. | Buffer en colas de persistencia temporal; descarte de eventos duplicados mediante firma SHA-256. | 1 | 3 | **3 (Baja)** | Diseñado para AT-1 (Gate 1) |
| **`RSK-09`** | Técnico / Integración | **Desfase semántico y desalineación de esquemas JSON durante el onboarding de transportistas.** Causa: Cargas útiles heterogéneas enviadas por operadores. | Errores de parseo en `CMP-ING-01`, descarte de eventos legítimos y reprocesamiento manual. | 3 | 3 | **9 (Media)** | Fase E / PKG-01 | `ACT-14` (QA)<br>`ACT-16` (TI Transportistas) | Publicación de esquemas JSON Schema draft-07 formales y especificación OpenAPI 3.0 vinculante. | Validación sintáctica automática en API Gateway; derivación de malformados a cola Dead Letter Queue (DLQ). | 1 | 2 | **2 (Baja)** | Diseñado para AT-2 (Gate 2) |
| **`RSK-10`** | Operativo / Personas | **Resistencia operativa de transportistas a utilizar interfaces digitales de reporte.** Causa: Choferes que omiten el registro o postergan la carga de eventos. | Falta de confirmación de entrega en tiempo oportuno; persistencia de la conciliación física tardía. | 3 | 4 | **12 (Alta)** | Fase E / PKG-04 | `ACT-06` (Distribución)<br>`ACT-07` (Transporte) | Portal Web de Contingencia optimizado para móviles y capacitación técnica presencial. | Supervisión activa por Supervisores de Ruta; condicionamiento de liquidación de fletes al reporte. | 2 | 2 | **4 (Baja)** | Diseñado para AT-2 (Gate 2) |
| **`RSK-CHG-01`** | Gobernanza / Evolutivo | **Ruptura de interoperabilidad con transportistas por cambios no coordinados en APIs.** Causa: Modificación intempestiva de endpoints o payloads externos. | Caída súbita del flujo de trazabilidad en las regiones operadas por el transportista modificado. | 3 | 4 | **12 (Alta)** | Fase H / RFC | `ACT-12` (Arquitecto)<br>`ACT-16` (TI Transportistas) | Política de gobernanza de APIs (RFC Tipo 2), versionado semántico estricto y periodo de gracia de 6 meses. | Pruebas de contrato automatizadas (*Contract Testing* con Pact); entorno Sandbox de homologación. | 1 | 3 | **3 (Baja)** | Protocolizado en Fase H |
| **`RSK-CHG-02`** | Técnico / Rendimiento | **Contención de recursos en base de datos de Lurín por ajustes ineficientes de consultas.** Causa: Nuevas vistas o queries analíticas no indexadas sobre el staging. | Lentitud en la línea de picking del Centro de Distribución y demoras en el despacho de furgones. | 2 | 4 | **8 (Media)** | Fase H / RFC | `ACT-03` (Almacén)<br>`ACT-12` (Arquitecto) | Revisión obligatoria de sentencias SQL por DBA; veto técnico vinculante de `ACT-03` (`REQ-RES-002`). | Plan de apagado inmediato (Kill Switch) para consultas que superen el umbral de 1 segundo. | 1 | 2 | **2 (Baja)** | Protocolizado en Fase H |
| **`RSK-CHG-03`** | Operativo / Terceros | **Fatiga de cambios en empresas de transporte por modificaciones continuas de sistemas.** Causa: Actualizaciones demasiado frecuentes que exijan reconfiguración en ruta. | Falta de adopción de mejoras técnicas, desinterés y demoras en homologaciones de nuevos socios. | 3 | 3 | **9 (Media)** | Fase H / RFC | `ACT-06` (Distribución)<br>`ACT-15` (PMO) | Convocatorias previas en el Comité de Integración B2B; calendario semestral de cambios mayores. | Soporte asistido por la PMO (`ACT-15`) y sesiones de capacitación técnica a enlaces de transportistas. | 1 | 2 | **2 (Baja)** | Protocolizado en Fase H |
| **`RSK-CHG-04`** | Gobernanza / Calidad | **Divergencia entre la documentación de arquitectura y la realidad en producción (*Shadow Architecture*).** Causa: Parches de emergencia no registrados en el repositorio. | Pérdida de control de la arquitectura empresarial, obsolescencia del repositorio y fallas en auditorías. | 3 | 4 | **12 (Alta)** | Fase H / Gobierno | `ACT-12` (Arquitecto)<br>`ACT-14` (QA) | Enfoque GitOps estricto (Infraestructura como Código); sincronización obligatoria del Repositorio (EAR). | Auditorías semestrales de conformidad física vs. modelos de arquitectura; congelamiento de credenciales directas. | 1 | 3 | **3 (Baja)** | Protocolizado en Fase H |

---

## 3. Matriz de Severidad y Mapa de Calor Comparativo ($5 \times 5$)

A continuación se grafica el análisis cuantitativo del desplazamiento esperado entre la severidad inherente y la severidad residual proyectada:

```
        MATRIZ DE RIESGO INHERENTE                              MATRIZ DE RIESGO RESIDUAL PROYECTADA (META)
  P                                                       P
  5 |       |       |       |  R05  |       |             5 |       |       |       |       |       |
  4 |       |       |  R06  |R01,R02|       |             4 |       |       |       |       |       |
  3 |       |       |R09,C03|R03,R04|       |             3 |       |       |       |       |       |
    |       |       |       |R08,R10|       |             2 |       |R02,R06|       |       |       |
    |       |       |       |C01,C04|       |               |       |R10    |       |       |       |
  2 |       |       |       |  C02  |  R07  |             1 |       |R01,R03|R04,R08|R05,R07|       |
  1 |       |       |       |       |       |               |       |R09,C02|C01,C04|       |       |
    +-------+-------+-------+-------+-------+               |       |C03    |       |       |       |
        1       2       3       4       5   I               +-------+-------+-------+-------+-------+
                                                                1       2       3       4       5   I

Leyenda de Códigos:
R01..R10: RSK-01 a RSK-10
C01..C04: RSK-CHG-01 a RSK-CHG-04
Severidad: [1-4] Baja (Verde) | [5-9] Media (Amarillo) | [10-15] Alta (Naranja) | [16-25] Crítica (Rojo)
```

### 3.1. Síntesis Cuantitativa de la Auditoría
- **Riesgos Inherentes:** 2 Críticos ($14.3\%$), 9 Altos ($64.3\%$) y 3 Medios ($21.4\%$). El $78.6\%$ de los riesgos inherentes supera el umbral de tolerancia operacional.
- **Riesgos Residuales Proyectados (Meta):** El 100% de los riesgos ($14/14$) se proyecta hacia la zona de severidad baja ($S_{res} \le 4$, Verde).
- **Compromiso de Validación:** Ningún riesgo se considera mitigado por decreto; su reducción será formalmente verificada por el Architecture Board durante la evaluación vinculante de los Quality Gates:
  - `Gate 1 (Semana S12):` Verificación de controles para `RSK-01`, `RSK-03`, `RSK-04`, `RSK-07`, `RSK-08`.
  - `Gate 2 (Semana S18):` Verificación de controles para `RSK-02`, `RSK-06`, `RSK-09`, `RSK-10`.
  - `Gate 3 (Semana S24) / Fase H:` Verificación de gobernanza para `RSK-05`, `RSK-CHG-01..04`.
