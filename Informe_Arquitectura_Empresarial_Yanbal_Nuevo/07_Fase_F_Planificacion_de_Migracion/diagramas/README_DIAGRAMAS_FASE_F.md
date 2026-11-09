# Catálogo y Registro Oficial de Diagramas — Fase F: Planificación de la Migración (`DIAG-09`)

> **Capítulo asociado:** [`07_Fase_F_Planificacion_de_la_Migracion.md`](../07_Fase_F_Planificacion_de_la_Migracion.md)  
> **Nomenclatura canónica:** [`00_1_Glosario_Canonico_y_Nomenclatura.md`](../../00_Referencias_Glosario_y_Decisiones/00_1_Glosario_Canonico_y_Nomenclatura.md) *(Prefijo `DIAG-XX`; `DIAG-09` asignado al Roadmap Estratégico de Implementación de la Fase F)*  
> **Norma técnica:** OMG UML 2.5.1 / TOGAF Standard Phase F (Migration Planning)

---

## 1. Inventario Único de Archivos Fuente UML Textuales y Editables (`.puml`)

La documentación escrita de [`07_Fase_F_Planificacion_de_la_Migracion.md`](../07_Fase_F_Planificacion_de_la_Migracion.md) constituye la explicación autosuficiente y prioritaria de la planificación de migración, la priorización multicriterio de proyectos y los planes de validación. El archivo PlantUML (`.puml`) listado a continuación representa la **versión canónica editable** del modelo de hoja de ruta estratégica:

| ID | Título Oficial | Tipo de Modelo | Propósito Metodológico | Apartados de Referencia | Archivo Fuente Textual Editable |
| :---: | :--- | :---: | :--- | :---: | :--- |
| **`DIAG-09`** | Diagrama del Roadmap Estratégico de Implementación y Arquitecturas de Transición | Roadmap de Transición / Estado | Modelar la progresión gradual desde la Línea Base AS-IS hacia la Arquitectura Objetivo (TO-BE), delimitando las dos Arquitecturas de Transición intermedias (AT-1 y AT-2), los Puntos de Control (Gates) y el encadenamiento de los cinco Paquetes de Trabajo (`PKG-01` a `PKG-05`). | `7.1` y `7.3` | [`DIAG-09_Roadmap_Estrategico_Implementacion.puml`](DIAG-09_Roadmap_Estrategico_Implementacion.puml) |

---

## 2. Reglas de Rigor Metodológico y Criterios Arquitectónicos Aplicados

1. **Alineación con TOGAF ADM (Fase F):** Modela formalmente las fases de evolución gradual justificadas a partir de la auditoría de dependencias de los paquetes de trabajo.
2. **Definición Rigurosa de Estados Intermedios:**
   * **AT-1 (Infraestructura Base, Gobernanza y Núcleo Interno):** Habilita la plataforma de microservicios MTD, el API Gateway y los conectores internos (`SPY` y `SAP R3` solo lectura), asegurando el control sobre el tramo de Lurín.
   * **AT-2 (Integración Externa de Transporte y Piloto en Ruta):** Habilita la ingesta segura desde los socios logísticos (`ACT-07`), el mecanismo offline *Store-and-Forward* y la sincronización temprana hacia `CELLFORCE` (`SIS-09`).
   * **TO-BE (Cobertura Nacional y Gobierno Pleno):** Despliegue en los 24 departamentos, gestión estructurada de retornos por logística inversa y monitoreo telemático de SLAs en tiempo real ($\le 30\text{ min}$).
3. **Puntos de Control y Criterios de Puerta (*Gates*):** Establece condiciones objetivas de salida técnica (certificación de seguridad, cero escrituras sobre `SAP R3`, verificación empírica de latencia en piloto) antes de avanzar al siguiente estado.
4. **Naturaleza Referencial del Cronograma:** Todos los periodos se expresan en semanas relativas de planificación (S1 a S24), reconociendo explícitamente que el calendario corporativo formal está pendiente de validación por Yanbal Perú (`ASM-14`).

