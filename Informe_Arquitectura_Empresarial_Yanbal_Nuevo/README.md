# Nuevo Informe de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)

Este repositorio contiene el desarrollo completamente nuevo del informe de Arquitectura Empresarial para el caso de **Yanbal Perú**, elaborado bajo el marco **TOGAF ADM**, los principios de **Ingeniería de Requerimientos ISO/IEC/IEEE 29148** y los estándares de modelado **OMG UML**.

---

## 1. Enfoque del Proyecto: Plan de Mejora Arquitectónica

El proyecto aborda un **plan de mejora arquitectónica** orientado a resolver el desfase existente en la visibilidad y actualización de los estados de los pedidos durante el proceso de despacho de Yanbal Perú hacia sus distribuidoras y destinos empresariales correspondientes en el territorio nacional (`[AC]`, `[TO 23:16–25:10]`).

- **Problemática central documentada:** Retraso referido en la entrevista como *«en promedio de dos horas»* (`[TO 23:44]`) y *«hasta dos horas»* (`[TO 24:09; s2 0:49]`) entre la ocurrencia física de los eventos de distribución y su disponibilidad en los sistemas de seguimiento, con una meta preliminar del negocio de reducir dicho desfase a un máximo de **30 minutos** (`[TO 23:29; s2 0:57–1:07]`).
- **Concepto provisional de solución:** **Mecanismo de Trazabilidad del Despacho (`MTD`)**, concebido como una capacidad arquitectónica («caja negra» bajo análisis) para mejorar la captura, validación, correlación, propagación y disponibilidad de los eventos relevantes de picking, packing, despacho, transporte y recepción empresarial, integrándose con el ecosistema existente sin presuponer la construcción de una aplicación aislada ni la existencia de una flota propia con GPS continuo.
- **Alcance operativo B2B:** Comprende desde la preparación, picking, packing y despacho en las instalaciones de Lima hasta la entrega y recepción del despacho en las distribuidoras y sedes o destinos empresariales correspondientes en el Perú (`[AC]`), excluyendo el reparto capilar posterior que realizan las distribuidoras hacia las consumidoras finales.

---

## 2. Reglas de Rigor Epistémico y Control de Consistencia

Todo el contenido del informe distingue explícitamente cuatro niveles de evidencia:
1. **`[Fuente Pública]`**: Información corporativa pública verificable de Unique S.A. / Yanbal Perú.
2. **`[TO mm:ss]` / `[AC]`**: Evidencia empírica procedente de la entrevista al Ing. Joao Condorpusa Mendoza (`[TO]`) y de su aclaración complementaria (`[AC]`).
3. **`[Hipótesis Analítica]`**: Inferencia técnica razonada cuando las fuentes describen un síntoma pero no el mecanismo técnico interno.
4. **`ASM-XX` / `DEC-XX`**: Supuestos, discrepancias de fuente, incertidumbres técnicas abiertas y decisiones arquitectónicas pendientes de validación formal.

> **Regla sobre aprobación de requisitos:** El hecho de que una necesidad operativa esté respaldada por la entrevista (`[TO]`) o la aclaración (`[AC]`) acredita su **evidencia de origen**, pero no implica que Yanbal Perú haya aprobado formalmente la especificación técnica del requisito. En consecuencia, todos los requisitos iniciales se registran con estado **`Pendiente de validación`** o **`Propuesto`**.

---

## 3. Navegación del Repositorio (Etapa 1, Fase A y Fase B)

- **[`INDICE_GENERAL.md`](INDICE_GENERAL.md)**: Estructura maestra de 9 apartados (57 subapartados) y clasificación de entregables APF2 vs. Proyecto Final (`ASM-ACAD-01`).
- **Registros Maestros (`00_Referencias_Glosario_y_Decisiones/`):**
  - **[`00_1_Glosario_Canonico_y_Nomenclatura.md`](00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md)**
  - **[`00_2_Catalogo_Maestro_Actores_y_Sistemas.md`](00_Referencias_Glosario_y_Decisiones/00_2_Catalogo_Maestro_Actores_y_Sistemas.md)**
  - **[`00_3_Registro_Supuestos_Incertidumbres_y_Decisiones.md`](00_Referencias_Glosario_y_Decisiones/00_3_Registro_Supuestos_Incertidumbres_y_Decisiones.md)**
- **Apartado 1 — Fase Preliminar (`01_Fase_Preliminar/`):**
  - **[`01_Fase_Preliminar_Completa.md`](01_Fase_Preliminar/01_Fase_Preliminar_Completa.md)**: Desarrollo completo de los subapartados `1.1` a `1.5`.
  - **[`01_5_Matriz_Maestra_Requerimientos.md`](01_Fase_Preliminar/01_5_Matriz_Maestra_Requerimientos.md)**: Fuente única de la Matriz Maestra de Requerimientos (24 registros × 15 campos) con trazabilidad progresiva actualizada hasta la Fase B.
- **Apartado 2 — Fase A: Visión de Arquitectura (`02_Fase_A_Vision_de_Arquitectura/`):**
  - **[`02_Fase_A_Vision_Arquitectura_Completa.md`](02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md)**: Desarrollo completo de los subapartados `2.1` a `2.8` (`OBJ-01`..`08`, `FCE-01`..`06`, `KPI-01`..`08`, `RSK-01`..`06` y visión conceptual `DIAG-01`).
- **Apartado 3 — Fase B: Arquitectura de Negocio (`03_Fase_B_Arquitectura_de_Negocio/`):**
  - **[`03_Fase_B_Arquitectura_Negocio_Completa.md`](03_Fase_B_Arquitectura_de_Negocio/03_Fase_B_Arquitectura_Negocio_Completa.md)**: Desarrollo completo de los subapartados `3.1` a `3.8` (`CUN-01`..`06`, procesos AS-IS y TO-BE, servicios `SNB-01`..`06` y análisis de brechas `GAP-01`..`08`).
  - **[`03_Fase_B_Arquitectura_de_Negocio/diagramas/`](03_Fase_B_Arquitectura_de_Negocio/diagramas/README_DIAGRAMAS_FASE_B.md)**: Código fuente UML textual y editable en PlantUML (`.puml`, UML 2.5) de los cinco diagramas canónicos de referencia (`DIAG-02` a `DIAG-06`).

