# Inventario Documental: Revisión de la Entrevista Yanbal

> **Navegación:** [[Índice de información]] | [[Transcripción original]] | [[Aclaración adicional del entrevistado]] | [[Matriz de inconsistencias]] | [[Glosario de nombres y términos]] | [[Informe final de revisión]]

---

## 1. Relación de Archivos Revisados

A continuación se detalla el censo completo de los archivos que conforman el espacio documental de la entrevista, clasificados según su tipología, función en el proyecto, estado tras la revisión y mecanismo de custodia:

| N° | Ubicación y Nombre del Archivo | Tipo de Documento | Función en el Proyecto | Estado tras la Revisión | Integridad / Custodia |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **1** | `00 - Fuente/Transcripción original.md` | **Fuente Primaria (Original)** | Transcripción íntegra con marcas de tiempo y ficha técnica. Base inalterable de todo el análisis. | **INTACTO / DE SOLO LECTURA** (Sin modificaciones). | Hash SHA256 verificado: `AB6FC60641C5497835BE2ED188FB9CE857C137AD4656DCEE67C0A634547B1B26` |
| **2** | `00 - Fuente/Aclaración adicional del entrevistado.md` | **Fuente Complementaria (`[AC]`)** | Registro formal de la precisión sobre el problema central de trazabilidad aportada fuera de la entrevista. | **NUEVO ARCHIVO CREADO** | Separado rigurosamente de la transcripción original. |
| **3** | `trascrito.text` | **Fuente Bruta** | Texto continuo de la transcripción automática original. | **INTACTO / PRESERVADO** (Sin modificaciones). | Hash SHA256: `3864A5C82BC05EBA0FB9134F676FC9BC3EC037E6A89F9EFDB3F281ACFE2B2C59` |
| **4** | `Índice de información.md` | **Índice / Mapa de Contenidos (MOC)** | Estructura general, enlaces, flujo de valor, matrices cruzadas y parámetros cuantitativos. | **REESTRUCTURADO Y CORREGIDO** | Foco reorientado al problema central de trazabilidad; enlaces verificados. |
| **5** | `01 - Empresa/Información general de la empresa.md` | **Módulo Temático 01** | Descripción de Yanbal, sector cosméticos, actividad, familias de productos y áreas. | **CORREGIDO Y ALINEADO** | Retiradas áreas no confirmadas; delimitado problema central con `[AC]`. |
| **6** | `02 - Transporte/Transporte y logística.md` | **Módulo Temático 02** | Procesos de tránsito, volumetría, 8 cajas, 24 departamentos, lead times y logística inversa. | **CORREGIDO Y ALINEADO** | Retirada exclusión contradictoria de reparto domiciliario; corregidas citas. |
| **7** | `03 - Producción/Producción.md` | **Módulo Temático 03** | Manufactura, envasado, cajas mono SKU, código UA y 3 estatus en SAP R3. | **CORREGIDO Y ALINEADO** | Errata de tipeo corregida; merma contextualizada sin imputar fallas a manufactura. |
| **8** | `04 - Inventario/Inventario y almacén.md` | **Módulo Temático 04** | Almacén de 15.000 m², ~6.800 posiciones, 2M cajas, SPY, RF, reportes SAP y merma de 6h. | **CORREGIDO Y ALINEADO** | Datos métricos verificados con transcripción; separación de hechos y deducciones. |
| **9** | `05 - Trazabilidad/Trazabilidad.md` | **Módulo Temático 05** | Problema central, llaves de seguimiento (UA, N° Pedido), estatus, promesas y brechas. | **CORREGIDO Y ALINEADO** | Encabezado reenfocado en trazabilidad fábrica $\rightarrow$ distribuidoras/sedes `[AC]`. |
| **10** | `06 - Sistemas/Sistemas y tecnología.md` | **Módulo Temático 06** | Catálogo de 7 sistemas, bus de integración, terminales RF, requerimientos no funcionales. | **CORREGIDO Y ALINEADO** | Nombres textuales aclarados (SPY, NSDG/ENSDY, DRIVING, CELLFORCE); 3 vs 4 clasificado como 🔷. |
| **11** | `07 - Problemas/Problemas y necesidades.md` | **Módulo Temático 07** | Diagnóstico operacional, matriz PR-01 a PR-05 y necesidades NEC-01 a NEC-05. | **CORREGIDO Y ALINEADO** | Retirado el supuesto PR-06 no respaldado; centralidad en trazabilidad de entregas. |
| **12** | `08 - Actores/Actores y responsabilidades.md` | **Módulo Temático 08** | Niveles jerárquicos de roles, fichas de actores confirmados y matriz de intercambio. | **CORREGIDO Y ALINEADO** | Retirado enlace roto; eliminadas áreas ficticias; basada en los 4 roles reales. |
| **13** | `buscar mas informacion.md` | **Nota Externa Referencial** | Apuntes sobre contratación de proveedores de transporte en órdenes de compra. | **DOCUMENTADO Y CONTEXTUALIZADO** | Marcado como nota referencial externa no validada por el entrevistado. |
| **14** | `09 - Revisión documental/Inventario documental.md` | **Entregable de Auditoría** | Relación censal y descriptiva de los documentos. | **NUEVO ARCHIVO CREADO** | Entregable A de la revisión. |
| **15** | `09 - Revisión documental/Matriz de inconsistencias.md` | **Entregable de Auditoría** | Registro tabular de errores detectados, evidencias, correcciones y motivos. | **NUEVO ARCHIVO CREADO** | Entregable B de la revisión. |
| **16** | `09 - Revisión documental/Glosario de nombres y términos.md` | **Entregable de Auditoría** | Términos y entidades normalizados diferenciando confirmados de dudosos. | **NUEVO ARCHIVO CREADO** | Entregable C de la revisión. |
| **17** | `09 - Revisión documental/Informe final de revisión.md` | **Entregable de Auditoría** | Resumen ejecutivo, diagnóstico, resultados, dudas abiertas y criterio de aceptación. | **NUEVO ARCHIVO CREADO** | Entregable F de la revisión. |
| **18** | `09 - Revisión documental/Registro de cambios y reversión.md` | **Entregable de Auditoría** | Procedimiento de control de versiones git, respaldo zip y comandos de restauración. | **NUEVO ARCHIVO CREADO** | Garantía de trazabilidad y reversión. |

---

## 2. Resumen Estadístico del Inventario

- **Total de documentos inspeccionados originales:** 12 archivos (9 subcarpetas temáticas + 3 archivos en raíz).
- **Archivos fuente protegidos inalterados:** 2 (`Transcripción original.md` y `trascrito.text`).
- **Archivos temáticos derivados revisados y reescritos:** 8 (módulos 01 al 08 + Índice + Apuntes).
- **Archivos nuevos creados para gobernanza y auditoría:** 6 (1 aclaración complementaria en 00-Fuente + 5 documentos en 09-Revisión documental).
- **Total de documentos en la carpeta tras la intervención:** 18 archivos.
