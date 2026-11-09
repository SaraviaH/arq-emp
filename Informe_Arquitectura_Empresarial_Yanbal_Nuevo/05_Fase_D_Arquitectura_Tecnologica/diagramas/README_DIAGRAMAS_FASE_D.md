# Catálogo y Registro Oficial de Diagramas — Fase D: Arquitectura Tecnológica (`DIAG-07`)

> **Capítulo asociado:** [`05_Fase_D_Arquitectura_Tecnologica_Completa.md`](../05_Fase_D_Arquitectura_Tecnologica_Completa.md)  
> **Nomenclatura canónica:** [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md) *(Prefijo `DIAG-XX`; `DIAG-07` asignado al modelo de despliegue de la Fase D)*  
> **Catálogo maestro de actores y sistemas:** [`00_2_Catalogo_Maestro_Actores_y_Sistemas.md`](../../00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md)  
> **Norma técnica:** OMG Unified Modeling Language (OMG UML) versión 2.5.1 — Diagramas de Despliegue (*Deployment Diagrams*)

---

## 1. Inventario Único de Archivos Fuente UML Textuales y Editables (`.puml`)

La documentación escrita de [`05_Fase_D_Arquitectura_Tecnologica_Completa.md`](../05_Fase_D_Arquitectura_Tecnologica_Completa.md) constituye la explicación autosuficiente y prioritaria de la arquitectura tecnológica. El archivo PlantUML (`.puml`) listado a continuación representa la **versión canónica editable** del modelo de despliegue tecnológico objetivo (TO-BE), manteniendo correspondencia rigurosa con los sistemas existentes (`SIS-01` a `SIS-10`), los componentes lógicos del MTD (`CMP-ING-01` a `CMP-NOT-06`), los adaptadores (`ADP-SAP`, `ADP-SPY`, `ADP-LOG`, `ADP-CRM`), los actores logísticos (`ACT-03`, `ACT-04`, `ACT-05`, `ACT-06`, `ACT-07`, `ACT-08`, `ACT-09`, `ACT-11`, `ACT-12`) y las restricciones de contorno vinculantes:

| ID | Título Oficial | Tipo UML 2.5.1 | Propósito Metodológico | Apartado de Referencia | Archivo Fuente Textual Editable |
| :---: | :--- | :---: | :--- | :---: | :--- |
| **`DIAG-07`** | Diagrama UML de Despliegue de la Arquitectura Tecnológica Objetivo (TO-BE): Topología Híbrida On-Premise Lurín, Nube Corporativa / MTD, Socios Logísticos y Destinatarios B2B | Despliegue (*Deployment Diagram*) | Modelar la infraestructura de cómputo, redes, nodos físicos y virtuales, servicios de middleware y canales de comunicación seguros que dan soporte a los procesos de negocio y componentes de información, respetando el aislamiento de SAP R3 (`REQ-RES-002`) y la tercerización de flota sin GPS continuo (`REQ-RES-004`). | `5.6` | [`DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml`](DIAG-07_Despliegue_Arquitectura_Tecnologica_TO_BE.puml) |

---

## 2. Reglas de Rigor Metodológico y Criterios Arquitectónicos Aplicados

1. **Alineación con TOGAF ADM:** Modela la vista de infraestructura tecnológica que soporta la Arquitectura de Aplicaciones y de Datos desarrollada en la Fase C.
2. **Neutralidad Tecnológica y Distinción de Propuesta (`DEC-02`):** La plataforma híbrida y los componentes del MTD se expresan como una capacidad arquitectónica propuesta, diferenciando plataformas existentes frente a componentes propuestos sujetos a evaluación.
3. **Respeto a la Restricción Crítica de SAP R3 (`REQ-RES-002`):** El ERP SAP R3 (`SIS-01`) se aloja exclusivamente en el nodo on-premise de Lurín, operado por Almacén (`ACT-03`). Ningún flujo tecnológico del MTD posee acceso de escritura a la base de datos de SAP R3.
4. **Respeto a la Operación de Transporte Tercerizado (`REQ-RES-004`, `ASM-05`):** La infraestructura externa de transporte pertenece a socios logísticos asociados (`ACT-07`). No se incluyen servidores de telemetría GPS continua de flota vehicular propia.
5. **Tolerancia a Conectividad Intermitente (*Store-and-Forward* — `ASM-16`, `RSK-06`):** Los dispositivos móviles de transportistas incorporan buffers de almacenamiento local cifrado para capturar eventos y sellos $T_0$ de forma offline, sincronizando ráfagas al recuperar cobertura celular en rutas troncales interprovinciales.
6. **Seguridad y Perímetro Corporativo (`REQ-RNF-003`, `OBJ-07`):** Todo tráfico entrante desde redes externas o socios logísticos atraviesa un API Gateway corporativo con terminación TLS 1.3, autenticación mutua (mTLS) o tokens OAuth 2.0 / JWT, validado por Seguridad Informática (`ACT-11`).

