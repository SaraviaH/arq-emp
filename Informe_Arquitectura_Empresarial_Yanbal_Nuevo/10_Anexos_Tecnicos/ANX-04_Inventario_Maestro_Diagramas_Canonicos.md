# ANEXO 4: INVENTARIO MAESTRO Y CATÁLOGO TÉCNICO DE DIAGRAMAS CANÓNICOS (DIAG-01 A DIAG-11)

> **Ubicación oficial:** `10_Anexos_Tecnicos/ANX-04_Inventario_Maestro_Diagramas_Canonicos.md`  
> **Documento maestro articulado:** [`ANEXOS_GENERALES_PROYECTO.md`](ANEXOS_GENERALES_PROYECTO.md)  
> **Fuentes de verdad arquitectónica:** Fases A a H de TOGAF ADM y catálogos `README_DIAGRAMAS` de cada fase.  
> **Norma técnica:** OMG Unified Modeling Language (UML) 2.5.1 / TOGAF Standard 10th Edition.  
> **Propósito:** Consolidar en un catálogo unificado el inventario oficial de los once diagramas canónicos del proyecto (`DIAG-01` a `DIAG-11`), especificando su tipo formal de modelo, medio de representación (Mermaid integrado en markdown para `DIAG-01` y código PlantUML editable `.puml` para `DIAG-02` a `DIAG-11`), propósito metodológico, componentes modelados y trazabilidad con los capítulos del informe.

---

## 1. Cadena de Modelado Visual del Proyecto (DIAG-01 a DIAG-11)

El modelado visual del proyecto de arquitectura empresarial de Yanbal Perú no incluye diagramas aislados ni decorativos. Cada modelo corresponde a un artefacto formal de TOGAF ADM estructurado según OMG UML 2.5.1, cubriendo desde la conceptualización estratégica hasta la gobernanza continua del cambio:

```
+----------------------------------------------------------------------------------------------------+
|                         MAPA CONSOLIDADO DE MODELOS CANÓNICOS (DIAG-01 A DIAG-11)                  |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [FASE A: VISIÓN DE ARQUITECTURA]                                                                  |
|  * DIAG-01: Visión Conceptual de la Solución (MTD | Diagrama Mermaid integrado en cuerpo Fase A)    |
|                                                                                                    |
|  [FASE B: ARQUITECTURA DE NEGOCIO]                                                                 |
|  * DIAG-02: Diagrama UML de Casos de Uso del Negocio B2B (CUN-01 a CUN-06 | PlantUML)              |
|  * DIAG-03: Diagrama UML de Actividades AS-IS (Línea Base Operativa | PlantUML)                      |
|  * DIAG-04: Diagrama de Actividades TO-BE (Flujo Operativo Detallado de Lurín a Ruta | PlantUML)     |
|  * DIAG-05: Diagrama de Secuencia AS-IS (Interacciones Actuales y Desfase de ~2 h | PlantUML)       |
|  * DIAG-06: Diagrama de Secuencia TO-BE (Intercambio Reactivo de Eventos <= 30 min | PlantUML)      |
|                                                                                                    |
|  [FASE D: ARQUITECTURA TECNOLÓGICA]                                                                |
|  * DIAG-07: Diagrama de Despliegue Físico y Topología de Red (Kubernetes, WAF, mTLS, TLS 1.3)     |
|                                                                                                    |
|  [FASE E: OPORTUNIDADES Y SOLUCIONES]                                                              |
|  * DIAG-08: Arquitectura de Solución Seleccionada y Mapa de Paquetes de Trabajo (PKG-01..05)       |
|                                                                                                    |
|  [FASE F: PLANIFICACIÓN DE LA MIGRACIÓN]                                                           |
|  * DIAG-09: Roadmap Estratégico de Implementación y Arquitecturas de Transición (AT-1, AT-2, TO-BE)|
|                                                                                                    |
|  [FASE G: GOBERNANZA DE LA IMPLEMENTACIÓN]                                                         |
|  * DIAG-10: Marco de Gobernanza de Implementación, Contratos de Arquitectura y Quality Gates       |
|                                                                                                    |
|  [FASE H: GESTIÓN DEL CAMBIO DE ARQUITECTURA]                                                      |
|  * DIAG-11: Marco de Gestión Continua del Cambio de Arquitectura y Ciclo Evolutivo ADM             |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 2. Fichas Técnicas Consolidadas de los Once Diagramas Canónicos

### Ficha Técnica `DIAG-01`
* **Identificador:** `DIAG-01`
* **Título Oficial:** Diagrama de Visión Conceptual de la Solución (Mecanismo de Trazabilidad del Despacho — MTD).
* **Fase TOGAF:** Fase A (Visión de Arquitectura). Apartado: `2.7.5`.
* **Tipo de Modelo:** Diagrama de Arquitectura Conceptual / Vista de Alto Nivel.
* **Tecnología y Ubicación del Modelo:** Código textual Mermaid (`flowchart TD`) integrado directamente en el cuerpo capitular de la Fase A: [`02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md` — Apartado 2.7.5](../02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md#275-diagrama-conceptual-de-la-visión-de-la-solución-to-be-diag-01). *(No requiere archivo `.puml` externo, garantizando unidad documental e inmutabilidad).*
* **Propósito Metodológico:** Representar la frontera de la solución propuesta como capacidad ("caja negra"), articulando las responsabilidades canónicas `MTD-R01` a `MTD-R06` frente a los sistemas corporativos (`SIS-01` a `SIS-10`), los socios logísticos (`ACT-07`) y el canal de atención al cliente (`SIS-09`), delimitando estrictamente el alcance B2B.

---

### Ficha Técnica `DIAG-02`
* **Identificador:** `DIAG-02`
* **Título Oficial:** Diagrama UML de Casos de Uso del Negocio: Preparación, Despacho y Trazabilidad B2B (CUN-01 a CUN-06).
* **Fase TOGAF:** Fase B (Arquitectura de Negocio). Apartado: `3.3.4`.
* **Tipo de Modelo:** Diagrama de Casos de Uso del Negocio (OMG UML 2.5.1).
* **Archivo Fuente PlantUML:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-02_Casos_Uso_Negocio.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-02_Casos_Uso_Negocio.puml)
* **Propósito Metodológico:** Representar formalmente los límites del proceso B2B y las interacciones entre los actores canónicos (`ACT-03` a `ACT-10`) y los seis casos de uso de negocio (`CUN-01` a `CUN-06`), modelando las extensiones para incidencias y retorno hacia Almacén (`CUN-06`).

---

### Ficha Técnica `DIAG-03`
* **Identificador:** `DIAG-03`
* **Título Oficial:** Diagrama UML de Actividades del Proceso de Negocio Actual (AS-IS): Preparación, Despacho, Transporte B2B y Logística Inversa.
* **Fase TOGAF:** Fase B (Arquitectura de Negocio). Apartados: `3.3` y `3.5.1`.
* **Tipo de Modelo:** Diagrama de Actividades con Calles de Responsabilidad (OMG UML 2.5.1 - *Swimlanes*).
* **Archivo Fuente PlantUML:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-03_Actividades_AS_IS.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-03_Actividades_AS_IS.puml)
* **Propósito Metodológico:** Modelar con exactitud la línea base operativa del proceso actual documentado, reflejando el desfase de hasta ~2 horas en ruta, la falta de marcas $T_0/T_1$ y el circuito actual de retorno físico de mermas e incidencias hacia Almacén Lurín (`ACT-03`).

---

### Ficha Técnica `DIAG-04`
* **Identificador:** `DIAG-04`
* **Título Oficial:** Diagrama de Actividades del Proceso Propuesto TO-BE.
* **Fase TOGAF:** Fase B (Arquitectura de Negocio). Apartado: `3.5`.
* **Tipo de Modelo:** Diagrama de Actividades OMG UML 2.5 con Calles de Responsabilidad (*Swimlanes*).
* **Archivo Fuente PlantUML:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-04_Actividades_TO_BE.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-04_Actividades_TO_BE.puml)
* **Propósito Metodológico:** Detallar paso a paso las actividades ejecutadas por cada rol canónico (`ACT-03`, `ACT-04`, `ACT-05`, `ACT-06`, `ACT-07`, `ACT-09`), mostrando los puntos de bifurcación ante incidentes de transporte, contingencias *Store-and-Forward* y recepción física en destino.

---

### Ficha Técnica `DIAG-05`
* **Identificador:** `DIAG-05`
* **Título Oficial:** Diagrama de Secuencia del Proceso Central AS-IS.
* **Fase TOGAF:** Fase B (Arquitectura de Negocio). Apartado: `3.6`.
* **Tipo de Modelo:** Diagrama de Secuencia OMG UML 2.5.
* **Archivo Fuente PlantUML:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-05_Secuencia_AS_IS.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-05_Secuencia_AS_IS.puml)
* **Propósito Metodológico:** Demostrar temporalmente las interacciones desconectadas entre sistemas en la situación actual, evidenciando cómo la falta de acoplamiento del bus corporativo con las plataformas de los transportistas genera el retraso de hasta ~2 horas en la visibilidad de `CELLFORCE` (`SIS-09`).

---

### Ficha Técnica `DIAG-06`
* **Identificador:** `DIAG-06`
* **Título Oficial:** Diagrama de Secuencia del Proceso Central TO-BE.
* **Fase TOGAF:** Fase B (Arquitectura de Negocio). Apartado: `3.6`.
* **Tipo de Modelo:** Diagrama de Secuencia OMG UML 2.5.
* **Archivo Fuente PlantUML:** [`03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-06_Secuencia_TO_BE.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-06_Secuencia_TO_BE.puml)
* **Propósito Metodológico:** Modelar el flujo reactivo de intercambio de eventos gobernado por CloudEvents v1.0, evidenciando el estampado determinístico de $T_1$ por `CMP-AUD-04`, el cómputo de latencia $\Delta T \le 30\text{ min}$ y la propagación inmediata hacia el portal comercial.

---

### Ficha Técnica `DIAG-07`
* **Identificador:** `DIAG-07`
* **Título Oficial:** Diagrama de Arquitectura Tecnológica y Despliegue Físico TO-BE.
* **Fase TOGAF:** Fase D (Arquitectura Tecnológica). Apartado: `5.6`.
* **Tipo de Modelo:** Diagrama de Despliegue OMG UML 2.5.
* **Archivo Fuente PlantUML:** [`05_Fase_D_Arquitectura_Tecnologica/diagramas/DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml`](../05_Fase_D_Arquitectura_Tecnologica/diagramas/DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml)
* **Propósito Metodológico:** Representar la topología física y lógica de la infraestructura: clúster de Kubernetes en VPC corporativa, microservicios Core, bases de datos PostgreSQL particionadas, colas RabbitMQ en clúster, API Gateway perimetral con WAF, terminación TLS 1.3/mTLS y canal seguro de solo lectura con `SAP R3`.

---

### Ficha Técnica `DIAG-08`
* **Identificador:** `DIAG-08`
* **Título Oficial:** Diagrama de Arquitectura de Solución Seleccionada y Mapa de Paquetes de Trabajo.
* **Fase TOGAF:** Fase E (Oportunidades y Soluciones). Apartado: `6.5`.
* **Tipo de Modelo:** Diagrama de Componentes y Paquetes de Solución TOGAF.
* **Archivo Fuente PlantUML:** [`06_Fase_E_Oportunidades_y_Soluciones/diagramas/DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml`](../06_Fase_E_Oportunidades_y_Soluciones/diagramas/DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml)
* **Propósito Metodológico:** Modelar la alternativa arquitectónica seleccionada (Integración Basada en Eventos sobre Bus Corporativo) y su descomposición estructurada en los cinco Paquetes de Trabajo canónicos (`PKG-01` a `PKG-05`), vinculándolos a las brechas resueltas.

---

### Ficha Técnica `DIAG-09`
* **Identificador:** `DIAG-09`
* **Título Oficial:** Diagrama del Roadmap Estratégico de Implementación y Arquitecturas de Transición.
* **Fase TOGAF:** Fase F (Planificación de la Migración). Apartados: `7.1` y `7.3`.
* **Tipo de Modelo:** Diagrama de Estado y Roadmap Temporalizado de Transición.
* **Archivo Fuente PlantUML:** [`07_Fase_F_Planificacion_de_Migracion/diagramas/DIAG-09_Roadmap_Estrategico_Implementacion.puml`](../07_Fase_F_Planificacion_de_Migracion/diagramas/DIAG-09_Roadmap_Estrategico_Implementacion.puml)
* **Propósito Metodológico:** Visualizar la secuencia de ejecución de los cinco paquetes de trabajo a lo largo de las 24 semanas referenciales (S1 a S24), delimitando las Arquitecturas de Transición (AT-1 y AT-2), el hito TO-BE y los Quality Gates vinculantes.

---

### Ficha Técnica `DIAG-10`
* **Identificador:** `DIAG-10`
* **Título Oficial:** Marco de Gobernanza de la Implementación, Contratos y Control de Conformidad.
* **Fase TOGAF:** Fase G (Gobernanza de la Implementación). Apartados: `8.2` a `8.6`.
* **Tipo de Modelo:** Diagrama de Estructura Organizacional y Proceso de Gobernanza.
* **Archivo Fuente PlantUML:** [`08_Fase_G_Gobierno_de_Implementacion/diagramas/DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml`](../08_Fase_G_Gobierno_de_Implementacion/diagramas/DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml)
* **Propósito Metodológico:** Modelar las interacciones formales del Architecture Board, la emisión de Contratos de Arquitectura (`AC-PKG-01` a `AC-PKG-05`), los tres Puntos de Control (*Quality Gates* 1, 2 y 3) y el circuito de gestión y remediación de dispensas (*Architecture Waivers*).

---

### Ficha Técnica `DIAG-11`
* **Identificador:** `DIAG-11`
* **Título Oficial:** Marco de Gestión Continua del Cambio de Arquitectura y Ciclo Evolutivo ADM.
* **Fase TOGAF:** Fase H (Gestión del Cambio de Arquitectura). Apartados: `9.2`, `9.4` y `9.9`.
* **Tipo de Modelo:** Diagrama de Flujo y Proceso de Cambio Continuo.
* **Archivo Fuente PlantUML:** [`09_Fase_H_Gestion_del_Cambio/diagramas/DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml`](../09_Fase_H_Gestion_del_Cambio/diagramas/DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml)
* **Propósito Metodológico:** Modelar los sensores de monitoreo de telemetría y seguridad, el flujo de vida de una RFC (`RFC-ARCH-XXXX`), la clasificación en Tipos 1, 2 y 3, la sincronización del Repositorio EA y los disparadores para el reinicio formal del ciclo TOGAF ADM.

---

## 3. Matriz de Resumen y Trazabilidad de Diagramas

| ID | Título Oficial | Fase TOGAF | Tipo de Modelo | Código / Ubicación Fuente | Calidad / Gate Asociado |
| :---: | :--- | :---: | :---: | :--- | :---: |
| **`DIAG-01`** | Visión Conceptual de la Solución (MTD) | Fase A | Arquitectura Conceptual | [Mermaid en Fase A (2.7.5)](../02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md#275-diagrama-conceptual-de-la-visión-de-la-solución-to-be-diag-01) | Hito de Visión Aprobada |
| **`DIAG-02`** | Casos de Uso del Negocio B2B (CUN-01..06) | Fase B | UML Casos de Uso (2.5.1) | [`DIAG-02_Casos_Uso_Negocio.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-02_Casos_Uso_Negocio.puml) | Definición Frontera B2B |
| **`DIAG-03`** | Actividades del Negocio AS-IS | Fase B | UML Actividades (Swimlanes) | [`DIAG-03_Actividades_AS_IS.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-03_Actividades_AS_IS.puml) | Línea Base Operativa |
| **`DIAG-04`** | Actividades del Negocio TO-BE | Fase B | UML Actividades (Swimlanes) | [`DIAG-04_Actividades_TO_BE.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-04_Actividades_TO_BE.puml) | Flujo Operativo Lurín-Ruta |
| **`DIAG-05`** | Diagrama de Secuencia AS-IS | Fase B | UML Secuencia (2.5.1) | [`DIAG-05_Secuencia_AS_IS.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-05_Secuencia_AS_IS.puml) | Análisis de Desfase (~2 h) |
| **`DIAG-06`** | Diagrama de Secuencia TO-BE | Fase B | UML Secuencia (2.5.1) | [`DIAG-06_Secuencia_TO_BE.puml`](../03_Fase_B_Arquitectura_de_Negocio/diagramas/DIAG-06_Secuencia_TO_BE.puml) | Intercambio Reactivo ($\le 30\text{ min}$) |
| **`DIAG-07`** | Despliegue Tecnológico TO-BE | Fase D | UML Despliegue (2.5.1) | [`DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml`](../05_Fase_D_Arquitectura_Tecnologica/diagramas/DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml) | Topología K8s y Red |
| **`DIAG-08`** | Arquitectura de Solución y Paquetes | Fase E | TOGAF / Componentes | [`DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml`](../06_Fase_E_Oportunidades_y_Soluciones/diagramas/DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml) | Solución `PKG-01..05` |
| **`DIAG-09`** | Roadmap Estratégico y Transiciones | Fase F | Roadmap de Estado | [`DIAG-09_Roadmap_Estrategico_Implementacion.puml`](../07_Fase_F_Planificacion_de_Migracion/diagramas/DIAG-09_Roadmap_Estrategico_Implementacion.puml) | Plan S1..S24 (`AT-1`, `AT-2`) |
| **`DIAG-10`** | Gobernanza de Implementación | Fase G | Proceso de Gobernanza | [`DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml`](../08_Fase_G_Gobierno_de_Implementacion/diagramas/DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml) | Control Gates 1, 2 y 3 |
| **`DIAG-11`** | Gestión del Cambio y Ciclo ADM | Fase H | Proceso y Ciclo ADM | [`DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml`](../09_Fase_H_Gestion_del_Cambio/diagramas/DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml) | Supervisión y Re-arquitectura |

