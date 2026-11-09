# Catálogo y Registro Oficial de Diagramas — Fase B: Arquitectura de Negocio (`DIAG-02` a `DIAG-06`)

> **Capítulo asociado:** [`03_Fase_B_Arquitectura_Negocio_Completa.md`](../03_Fase_B_Arquitectura_Negocio_Completa.md)  
> **Nomenclatura canónica:** [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md) *(Prefijo `DIAG-XX`; `DIAG-01` asignado al esquema conceptual de la Fase A en `2.7.5`)*  
> **Catálogo maestro de actores y sistemas:** [`00_2_Catalogo_Maestro_Actores_y_Sistemas.md`](../../00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md)

---

## 1. Inventario Único de Archivos Fuente UML Textuales y Editables (`.puml`)

La documentación escrita de [`03_Fase_B_Arquitectura_Negocio_Completa.md`](../03_Fase_B_Arquitectura_Negocio_Completa.md) constituye la explicación autosuficiente y prioritaria del funcionamiento del negocio. Los cinco archivos PlantUML (`.puml`, estándar OMG UML 2.5.1) que se listan a continuación constituyen las **únicas versiones canónicas** de los diagramas de referencia de la Fase B en código fuente textual y editable, manteniendo correspondencia exacta con los actores (`ACT-XX`), sistemas (`SIS-XX`), casos de uso (`CUN-01` a `CUN-06`), estados del pedido y responsabilidades propuestas del `MTD` (`MTD-R01` a `MTD-R06`):

| ID | Título Oficial | Tipo UML 2.5 | Objetivo Metodológico | Apartado de Referencia | Archivo Fuente Textual Editable |
| :---: | :--- | :---: | :--- | :---: | :--- |
| **`DIAG-02`** | Diagrama UML de Casos de Uso del Negocio: Preparación, Despacho y Trazabilidad B2B (`CUN-01` a `CUN-06`) | Casos de Uso | Representar mediante asociaciones simples (`--`) las interacciones de los actores internos y externos confirmados (y `ACT-10` condicionado a `ASM-01`) con `CUN-01` a `CUN-06`, utilizando `<<extend>>` únicamente entre casos de uso (`CUN-06` extiende a `CUN-03` y `CUN-04`). | `3.3.4` *(y `3.4`)* | [`DIAG-02_Casos_Uso_Negocio.puml`](DIAG-02_Casos_Uso_Negocio.puml) |
| **`DIAG-03`** | Diagrama UML de Actividades del Proceso de Negocio Actual (AS-IS): Preparación, Despacho, Transporte B2B y Logística Inversa | Actividades (*Swimlanes*) | Modelar exclusivamente la operación actual respaldada por `[TO]` y `[AC]` (sin elementos del `MTD`), reflejando la secuencia de cinco estados, la pérdida de visibilidad de hasta ~2 horas y las dos ramas de incidencia y logística inversa hacia Almacén. | `3.3` y `3.5.1` | [`DIAG-03_Actividades_AS_IS.puml`](DIAG-03_Actividades_AS_IS.puml) |
| **`DIAG-04`** | Diagrama UML de Actividades del Proceso de Negocio Objetivo Propuesto (TO-BE): Despacho B2B con Capacidad Propuesta `MTD` ($\Delta T \le 30\text{ min}$) | Actividades (*Swimlanes*) | Representar la propuesta de proceso objetivo (`DEC-02`), distinguiendo taxativamente lo `[Confirmado]` de lo `[Propuesto MTD-R01..R06]` y lo `[Condicionado ASM-01]`, acotando la notificación de retorno a `ACT-03` sin escritura directa en `SAP R3` (`REQ-RES-002`). | `3.4` y `3.5.2` | [`DIAG-04_Actividades_TO_BE.puml`](DIAG-04_Actividades_TO_BE.puml) |
| **`DIAG-05`** | Diagrama UML de Secuencia del Flujo Actual (AS-IS): Despacho, Actualización Diferida de Estados (~2 h), Consulta y Logística Inversa | Secuencia | Mostrar en orden cronológico los intercambios conceptuales actuales con identificadores canónicos (`ACT-XX`, `SIS-XX`), sin inventar APIs ni conexiones directas no confirmadas (`ASM-11`), reflejando la ventana de desfase de ~2 horas y la atención de `ACT-08` en `CELLFORCE`. | `3.3` y `3.6.1` | [`DIAG-05_Secuencia_AS_IS.puml`](DIAG-05_Secuencia_AS_IS.puml) |
| **`DIAG-06`** | Diagrama UML de Secuencia del Flujo Objetivo Propuesto (TO-BE): Captura, Correlación, Propagación ($\Delta T \le 30\text{ min}$) y Notificación de Retorno vía `SIS-05` y `MTD` | Secuencia | Contrastar los intercambios confirmados frente a los propuestos para `MTD` (`DEC-02`) enrutando coherentemente todos los eventos inter-sistemas a través del `Bus de Integración` (`SIS-05`, `ASM-08`), sin inventar protocolos y sin escrituras directas sobre `SAP R3`. | `3.4` y `3.6.2` | [`DIAG-06_Secuencia_TO_BE.puml`](DIAG-06_Secuencia_TO_BE.puml) |

---

## 2. Reglas de Rigor Metodológico y UML Verificadas

1. **Notación UML 2.5 estricta:** En `DIAG-02` no se emplea ningún estereotipo `<<include>>` ni `<<extend>>` entre actores y casos de uso; toda relación actor–caso de uso es una asociación simple (`--`) y `<<extend>>` conecta únicamente casos de uso entre sí.
2. **Separación AS-IS vs. Propuesta TO-BE:** `DIAG-03` y `DIAG-05` representan exclusivamente la operación actual sin introducir el `MTD`; `DIAG-04` y `DIAG-06` presentan el TO-BE como propuesta pendiente de validación (`DEC-02`), distinguiendo elementos confirmados, propuestos y condicionados (`ASM-01`).
3. **Mediación coherente del `Bus de Integración` (`SIS-05`) y fronteras de sistemas:** Ningún diagrama inventa APIs, endpoints ni conexiones directas no confirmadas (`ASM-11`), `SIS-01: SAP R3` es operado exclusivamente por `ACT-03` (`REQ-RES-002`) y no se presupone flota propia con GPS continuo (`REQ-RES-004`).
