# Catálogo y Registro Oficial de Diagramas — Fase E: Oportunidades y Soluciones (`DIAG-08`)

> **Capítulo asociado:** [`06_Fase_E_Oportunidades_y_Soluciones_Completa.md`](../06_Fase_E_Oportunidades_y_Soluciones_Completa.md)  
> **Nomenclatura canónica:** [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md) *(Prefijo `DIAG-XX`; `DIAG-08` asignado a la Arquitectura de Solución y Mapa de Paquetes de Trabajo de la Fase E)*  
> **Norma técnica:** OMG UML 2.5.1 / TOGAF Standard Phase E (Opportunities and Solutions)

---

## 1. Inventario Único de Archivos Fuente UML Textuales y Editables (`.puml`)

La documentación escrita de [`06_Fase_E_Oportunidades_y_Soluciones_Completa.md`](../06_Fase_E_Oportunidades_y_Soluciones_Completa.md) constituye la explicación autosuficiente y prioritaria de las oportunidades, soluciones y evaluación de alternativas. El archivo PlantUML (`.puml`) listado a continuación representa la **versión canónica editable** del modelo arquitectónico de la solución seleccionada y su articulación con los Paquetes de Trabajo:

| ID | Título Oficial | Tipo de Modelo | Propósito Metodológico | Apartados de Referencia | Archivo Fuente Textual Editable |
| :---: | :--- | :---: | :--- | :---: | :--- |
| **`DIAG-08`** | Diagrama de Arquitectura de Solución Seleccionada y Mapa de Paquetes de Trabajo | Arquitectura de Solución / Roadmap | Integrar visualmente los componentes de la solución seleccionada (EDA/SOA híbrida sobre el Bus `SIS-05`) y mapear su construcción modular hacia los cinco Paquetes de Trabajo canónicos (`PKG-01` a `PKG-05`), respetando el aislamiento de `SAP R3` (`REQ-RES-002`) y la resiliencia *Store-and-Forward* (`ASM-16`, `RSK-06`). | `6.3` y `6.5` | [`DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml`](DIAG-08_Arquitectura_Solucion_y_Paquetes_Trabajo.puml) |

---

## 2. Reglas de Rigor Metodológico y Criterios Arquitectónicos Aplicados

1. **Alineación con la Alternativa Seleccionada:** Modela con precisión la Alternativa 1 (Arquitectura de Integración Orientada a Eventos y Servicios) sustentada en la evaluación multicriterio de la sección `6.4`.
2. **Trazabilidad hacia los Paquetes de Trabajo:** Muestra la relación directa entre los componentes funcionales (`CMP-01` a `CMP-06`), los adaptadores (`ADP-XX`) y los paquetes de trabajo `PKG-01` a `PKG-05`.
3. **Respeto a Restricciones Vinculantes:**
   * Mantiene explícito el aislamiento de `SAP R3` (`REQ-RES-002`), donde el MTD no escribe en la base de datos del ERP y canaliza avisos de retorno hacia la consola de Almacén Lurín (`ACT-03`).
   * Representa la resiliencia de transporte tercerizado mediante almacenamiento offline en dispositivos móviles (*Store-and-Forward*), sin asumir flota propia con GPS continuo (`REQ-RES-004`, `ASM-05`, `ASM-16`).

