# Catálogo y Registro Oficial de Diagramas — Fase H: Gestión del Cambio de Arquitectura (`DIAG-11`)

> **Capítulo asociado:** [`09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md`](../09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md)  
> **Nomenclatura canónica:** [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md) *(Prefijo `DIAG-XX`; `DIAG-11` asignado al Marco de Gestión Continua del Cambio de Arquitectura y Ciclo Evolutivo ADM)*  
> **Norma técnica:** OMG UML 2.5.1 / TOGAF Standard Phase H (Architecture Change Management)

---

## 1. Inventario Único de Archivos Fuente UML Textuales y Editables (`.puml`)

La documentación escrita de [`09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md`](../09_Fase_H_Gestion_del_Cambio_de_Arquitectura.md) constituye la base conceptual y operativa de la gestión del cambio arquitectónico en régimen operacional. El archivo PlantUML (`.puml`) listado a continuación representa la **versión canónica editable** del modelo de cambio y evolución continua:

| ID | Título Oficial | Tipo de Modelo | Propósito Metodológico | Apartados de Referencia | Archivo Fuente Textual Editable |
| :---: | :--- | :--- :--- | :--- | :---: | :--- |
| **`DIAG-11`** | Marco de Gestión Continua del Cambio de Arquitectura y Ciclo Evolutivo ADM | Diagrama de Flujo y Proceso de Gobernanza (UML / TOGAF Phase H) | Modelar formalmente la interacción entre los sensores de monitoreo continuo de arquitectura, el circuito de Solicitudes de Cambio (RFC), la clasificación tripartita (Tipos 1, 2 y 3), las autoridades de aprobación, la sincronización del Repositorio de Arquitectura Empresarial (EAR) y las condiciones de disparo de un nuevo ciclo ADM. | `9.2`, `9.4`, `9.5`, `9.6`, `9.7` y `9.9` | [`DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml`](DIAG-11_Gestion_del_Cambio_y_Ciclo_Evolutivo_ADM.puml) |

---

## 2. Reglas de Rigor Metodológico y Criterios Arquitectónicos Aplicados

1. **Alineación con TOGAF ADM (Fase H):** Establece el marco operativo continuo mediante el cual Yanbal Perú (Unique S.A.) asegura que la arquitectura empresarial TO-BE implementada evolucione de manera controlada, respondiendo a los cambios del entorno de negocio, las variaciones tecnológicas y las demandas de integración sin perder su integridad ni degradar los SLAs de despacho B2B.
2. **Supervisión Proactiva Basada en Sensores:** Conecta la gestión del cambio con la telemetría en tiempo real (`CMP-AUD-04`, Prometheus/Grafana), la auditoría de base de datos de `SAP R3` (blindaje de cero escrituras externas `REQ-RES-002`), la consistencia de eventos (`CMP-VAL-03`) y la satisfacción del cliente en `CELLFORCE` (`SIS-09`).
3. **Clasificación Tripartita Rigurosa de RFCs:**
   * **RFC Tipo 1 (Mantenimiento Operativo y Menor):** Cambios que no alteran esquemas públicos ni contratos de arquitectura. Aprobados por el Comité Técnico de Cambios (`ACT-12`, `ACT-14`, `ACT-15`).
   * **RFC Tipo 2 (Modificación Incremental de Arquitectura):** Nuevos transportistas tercerizados (`ACT-07`), adición de campos opcionales retrocompatibles en CloudEvents o ajustes en adaptadores (`ADP-SPY`, `ADP-CRM`). Aprobados formalmente por el *Architecture Board* y consolidados en la versión v1.x del Repositorio de Arquitectura.
   * **RFC Tipo 3 (Cambio Fundamental / Re-arquitectura):** Modificaciones estructurales que rompen los supuestos y restricciones base (e.g., migración de `SAP R3` a SAP S/4HANA, ampliación a distribución B2C hacia consumidoras finales, o adopción de flota propia con telemática propietaria). Disparan formalmente una nueva iteración de TOGAF ADM mediante una nueva *Request for Architecture Work* (RAW).
4. **Sincronización Continua del Repositorio de Arquitectura Empresarial (EAR):** Cada cambio aprobado actualiza obligatoriamente los modelos PlantUML, catálogos maestros (`00_1`, `00_2`, `00_3`), matrices de trazabilidad (`REQ-01` a `REQ-24`) y contratos vigentes (`AC-PKG-01` a `AC-PKG-05`), garantizando que la documentación nunca diverja de la realidad productiva.

