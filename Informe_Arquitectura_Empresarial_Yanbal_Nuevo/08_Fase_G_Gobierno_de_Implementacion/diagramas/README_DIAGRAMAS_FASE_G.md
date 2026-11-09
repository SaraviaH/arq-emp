# Catálogo y Registro Oficial de Diagramas — Fase G: Gobernanza de la Implementación (`DIAG-10`)

> **Capítulo asociado:** [`08_Fase_G_Gobernanza_de_la_Implementacion.md`](../08_Fase_G_Gobernanza_de_la_Implementacion.md)  
> **Nomenclatura canónica:** [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md) *(Prefijo `DIAG-XX`; `DIAG-10` asignado al Marco de Gobernanza de la Implementación y Control de Conformidad Arquitectónica)*  
> **Norma técnica:** OMG UML 2.5.1 / TOGAF Standard Phase G (Implementation Governance)

---

## 1. Inventario Único de Archivos Fuente UML Textuales y Editables (`.puml`)

La documentación escrita de [`08_Fase_G_Gobernanza_de_la_Implementacion.md`](../08_Fase_G_Gobernanza_de_la_Implementacion.md) constituye la explicación autosuficiente y prioritaria de los órganos de gobierno, los contratos de arquitectura, los puntos de control de conformidad (*Quality Gates*), el flujo de gestión de dispensas y el ciclo de vida de transición operativa. El archivo PlantUML (`.puml`) listado a continuación representa la **versión canónica editable** del modelo de gobernanza:

| ID | Título Oficial | Tipo de Modelo | Propósito Metodológico | Apartados de Referencia | Archivo Fuente Textual Editable |
| :---: | :--- | :--- :--- | :--- | :---: | :--- |
| **`DIAG-10`** | Marco de Gobernanza de la Implementación, Contratos y Control de Conformidad Arquitectónica | Diagrama de Estructura y Flujo de Gobernanza (UML / TOGAF Phase G) | Modelar formalmente la interacción entre el Consejo de Arquitectura (*Architecture Board*), los Contratos de Arquitectura para los cinco paquetes (`PKG-01` a `PKG-05`), los tres Puntos de Control de Conformidad (*Quality Gates* para AT-1, AT-2 y TO-BE), el circuito de dispensas (*Architecture Waivers*) y el traspaso operacional a la Fase H. | `8.2`, `8.3`, `8.4`, `8.5`, `8.6` y `8.8` | [`DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml`](DIAG-10_Gobernanza_Implementacion_y_Conformidad.puml) |

---

## 2. Reglas de Rigor Metodológico y Criterios Arquitectónicos Aplicados

1. **Alineación Estricta con TOGAF ADM (Fase G):** Define los mecanismos institucionales mediante los cuales Yanbal Perú (Unique S.A.) gobierna, audita y valida la implementación técnica de la arquitectura empresarial sin interferir en la gestión interna de desarrollo de software, pero garantizando el apego riguroso a la arquitectura de destino aprobada.
2. **Institucionalización del Consejo de Arquitectura (*Architecture Board*):** Integra formalmente a los actores canónicos de decisión: Gerencia de TI (`ACT-13`), Arquitecto Líder (`ACT-12`), CISO / Seguridad de la Información (`ACT-11`), Jefatura de Operaciones Logísticas Lurín (`ACT-06`) y Jefatura de Almacén Lurín (`ACT-03`).
3. **Formalización de Contratos de Arquitectura (*Architecture Contracts*):** Establece contratos vinculantes (`AC-PKG-01` a `AC-PKG-05`) que fijan responsabilidades, componentes de software, requerimientos obligatorios, restricciones ineludibles y criterios de aceptación técnica para cada equipo ejecutor y socio logístico externo (`ACT-07` / `ACT-16`).
4. **Gobierno por Puntos de Control (*Quality Gates*):**
   * **Gate 1 (Semana S12 — Certificación AT-1):** Verificación estricta de cero escrituras en `SAP R3` (`REQ-RES-002`), aislamiento perimetral, pruebas estáticas SAST y estampación de marcas temporales duales ($T_0/T_1$) en el componente de auditoría (`CMP-AUD-04`).
   * **Gate 2 (Semana S18 — Certificación AT-2):** Verificación de interoperabilidad con transportistas piloto (`ACT-07`), mecanismo *Store-and-Forward* ante fallas de conectividad, cumplimiento del SLA en piloto ($\ge 95\%$ como umbral de tolerancia técnica operativa; $\ge 98\%$ como meta analítica de diseño del `KPI-02`) y 100% de consistencia de secuencia de estados (`KPI-03`).
   * **Gate 3 (Semana S24 — Certificación TO-BE):** Cobertura nacional hacia los 24 departamentos, telemetría y observabilidad en tiempo real (`PKG-05`), tablero de alertas de SLA y cumplimiento integral de los 24 requerimientos canónicos (`REQ-01` a `REQ-24`).
5. **Gestión Formal de Dispensas y Excepciones (*Architecture Waivers*):** Procedimiento documentado con plazos perentorios ($\le 6$ semanas) para gestionar desviaciones técnicas sin comprometer los pilares innegociables del proyecto (seguridad perimetral, aislamiento de `SAP R3` y alcance B2B estricto).

