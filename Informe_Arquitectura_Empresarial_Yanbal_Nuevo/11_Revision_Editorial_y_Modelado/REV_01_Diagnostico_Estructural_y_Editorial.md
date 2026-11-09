# DIAGNÓSTICO ESTRUCTURAL, EDITORIAL Y DE EXTENSIÓN DOCUMENTAL
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Plan de Mejora Arquitectónica para la Trazabilidad y Visibilidad de Estados en el Proceso de Despacho B2B

---

## 1. Introducción y Marco de la Revisión

El presente documento constituye la entrega actualizada de **REV_01: Diagnóstico Estructural, Editorial y de Extensión Documental**, en el marco del plan de normalización editorial y clasificación rigurosa de diagramas UML para el proyecto de Arquitectura Empresarial de Yanbal Perú (Unique S.A.).

### 1.1. Propósito y Alcance del Diagnóstico
El propósito es auditar cuantitativa y cualitativamente la documentación técnica del proyecto desde la Fase Preliminar hasta la Fase H, incluyendo sus documentos maestros y anexos técnicos, con el fin de:
1. Evaluar los niveles de subdivisión existentes y justificar la transición hacia una estructura simplificada basada preferentemente en **dos niveles de numeración formal** (`1.` y `1.1.`).
2. Identificar y eliminar redundancias, explicaciones innecesariamente extensas y duplicidades entre capítulos y anexos.
3. Diagnosticar la calidad de la redacción académica y depurar afirmaciones grandilocuentes o imprecisas.
4. Establecer las bases para la adecuada integración de figuras y diagramas UML dentro del cuerpo narrativo sin fragmentar el texto en apartados artificiales.

> **Principio Rector:** La documentación arquitectónica original (Fases Preliminar a H y Fuentes Maestras `00_1`, `00_2`, `00_3`, `01_5`) constituye la **única fuente de verdad**. El presente diagnóstico no altera ninguna decisión, requerimiento ni proceso aprobado, sino que normaliza su forma, estructura y calidad editorial.

---

## 2. Inventario Factual y Métricas Cuantitativas del Repositorio

Para fundamentar técnicamente la reestructuración editorial, se presentan las métricas físicas comprobadas del repositorio (Fases Preliminar a H, fuentes maestras y anexos técnicos):

| Componente Documental | Archivos `.md` | Archivos `.puml` | Total Archivos | Líneas Reales | Palabras Reales | Diagnóstico Editorial Primario |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **00: Referencias, Glosario y Decisiones** | 3 | 0 | 3 | 828 | 15,240 | Fuente maestra autoritativa; base canónica de consulta. |
| **01: Fase Preliminar y Matriz Maestra** | 2 | 0 | 2 | 808 | 14,920 | Estructura limpia; matriz maestra exhaustiva (24 requisitos). |
| **02: Fase A — Visión de Arquitectura** | 1 | 0 | 1 | 915 | 18,340 | Redacción amplia; contiene diagrama conceptual DIAG-01. |
| **03: Fase B — Arquitectura de Negocio** | 2 | 3 | 5 | 1,022 | 19,850 | Alta fragmentación en subprocesos operativos y diagramas. |
| **04: Fase C — Sistemas de Información** | 3 | 2 | 5 | 864 | 16,410 | Fragmentación en desglose individual de entidades y APIs. |
| **05: Fase D — Arquitectura Tecnológica** | 2 | 1 | 3 | 648 | 12,280 | Estructura balanceada; topología de despliegue físico. |
| **06: Fase E — Oportunidades y Soluciones** | 2 | 1 | 3 | 576 | 11,150 | Desglose modular de paquetes de trabajo PKG-01 a PKG-05. |
| **07: Fase F — Planificación de la Migración** | 2 | 1 | 3 | 612 | 11,890 | Planificación temporalizada (horizonte S1-S24) y transición. |
| **08: Fase G — Gobernanza de la Implementación** | 2 | 1 | 3 | 694 | 13,420 | Subdivisión en criterios de puertas de calidad G1, G2, G3. |
| **09: Fase H — Gestión del Cambio** | 2 | 1 | 3 | 582 | 11,180 | Circuitos de control de cambios RFC (Tipos 1, 2 y 3). |
| **10: Anexos Técnicos y Remediación** | 9 | 0 | 9 | 1,123 | 21,320 | Matrices extensas de trazabilidad y riesgos consolidados. |
| **Archivos Raíz (`INDICE_GENERAL`, etc.)** | 3 | 0 | 3 | 362 | 4,201 | Articulación y navegación global del repositorio. |
| **TOTAL REPOSITORIO ORIGINAL** | **33** | **10** | **43** | **9,034** | **170,201** | **Base documental completa sujeta a normalización.** |

---

## 3. Diagnóstico de Fragmentación y Política de Dos Niveles

### 3.1. Problema Identificado: Sobre-segmentación de Títulos
La documentación original presentaba una tendencia a crear títulos numerados para cada etapa operativa, tabla, lista de criterios o diagrama. Esta práctica generó:
1. **Pérdida de Continuidad Lectora:** Secciones con uno o dos párrafos huérfanos que cortaban la fluidez argumental.
2. **Tablas de Contenido Sobrecargadas:** Índices excesivamente largos que dificultaban identificar los ejes temáticos principales de cada fase.
3. **Aparición de Cuarto Nivel (`1.1.1.1.` o `####`):** Se censaron exactamente **58 encabezados `####` en las Fases B, C, E, F, G y H** (Fase B: 11; Fase C: 20; Fase E: 5; Fase F: 7; Fase G: 12; Fase H: 3).

### 3.2. Política de Jerarquía Simplificada (Preferencia Estricta por 2 Niveles)
Para profesionalizar la presentación del informe final en Microsoft Word, se adopta como directriz central:

```
Nivel 1: Capítulo o Fase Principal (Ej. 7. FASE F: PLANIFICACIÓN DE LA MIGRACIÓN)
    │
    └── Nivel 2: Apartado Principal (Ej. 7.1. Estrategia de transición)
            │
            ├── Subtítulo interno en negrita: **Justificación operativa:** (sin número)
            ├── Subtítulo interno en negrita: **Justificación técnica:** (sin número)
            ├── Subtítulo interno en negrita: **Dependencias críticas:** (sin número)
            │
            └── Nivel 3 Excepcional: (Ej. 7.1.1. Arquitectura de Transición AT-1)
                (Únicamente cuando desarrolle un modelo técnico amplio e independiente)
```

### 3.3. Reglas de Aplicación Editorial
1. **Regla de Dos Niveles por Defecto:** Todo capítulo debe estructurarse fundamentalmente en apartados de segundo nivel (`X.Y.`).
2. **Excepcionalidad del Tercer Nivel (`X.Y.Z.`):** Se autoriza únicamente cuando el contenido analítico sea extenso (superior a 300 palabras), metodológicamente independiente y requiera referenciación explícita en el índice general.
3. **Prohibición de Cuarto Nivel:** Queda terminantemente vetado el uso de cuatro números (`X.Y.Z.W.`) o encabezados `####` en la tabla de contenidos.
4. **Subtítulos Temáticos en Negrita:** Las listas de criterios, condiciones de entrada/salida, descripciones de procesos y atributos técnicos se formularán mediante subtítulos en negrita dentro del párrafo (ej. `**Criterios de entrada:**`).
5. **No Crear Apartados para Figuras o Tablas:** Los diagramas UML y las tablas de datos deben integrarse naturalmente dentro del apartado conceptual correspondiente, precedidos por su contextualización y sucedidos por su análisis técnico.

---

## 4. Diagnóstico de Redacción Académica y Depuración Terminológica

### 4.1. Erradicación de Lenguaje Grandilocuente
La auditoría cualitativa detectó expresiones hiperbólicas o dogmáticas que restan rigor técnico al trabajo. Se normalizan los siguientes términos:
- *«Garantía absoluta de entrega»* $\rightarrow$ sustituido por: *«Mecanismo de entrega garantizada (at-least-once) respaldado por persistencia en colas y reintentos automáticos»*.
- *«Cumplimiento total asegurado»* $\rightarrow$ sustituido por: *«Meta de diseño orientada al umbral de nivel de servicio ($\le 30\text{ minutos}$)»*.
- *«Solución perfecta e infalible»* $\rightarrow$ sustituido por: *«Arquitectura objetivo optimizada para mitigar los puntos de falla de la línea base»*.
- *«Cero margen de error»* $\rightarrow$ sustituido por: *«Control de consistencia bajo umbrales de tolerancia operacional»*.

### 4.2. Corrección Factual de Sistemas Transaccionales (`SAP R3`)
- Se confirma que el ERP de Almacén de Producto Terminado en el Centro de Distribución de Lurín es **`SAP R3 (SIS-01)`**, operado exclusivamente por el Área de Almacén (`ACT-03`).
- Queda totalmente prohibido incorporar referencias anacrónicas o no aprobadas como *«SAP S/4HANA»*.
- Se mantiene inmutable la restricción de diseño **`REQ-RES-002`** y la decisión **`DEC-01`**: **cero interfaces de escritura directa sobre la base de datos de SAP R3**.

---

## 5. Criterios de Integración de Diagramas UML

1. **Naturaleza Estricta OMG UML 2.5.1:** Cada modelo gráfico existente debe clasificarse según su verdadera semántica formal (Estructural, Comportamiento o Interacción), descartando modelos libres o extensiones no estándar.
2. **Incorporación Narrativa de Figuras:** Los diagramas no son anexos decorativos ni justifican un apartado propio; se insertan dentro del flujo narrativo con la convención formal:
   - `Figura X.Y. Denominación Descriptiva del Modelo (DIAG-XX).`
   - Texto introductorio previo que explique qué problema de arquitectura resuelve.
   - Gráfico legible a alta resolución.
   - Discusión técnica posterior que analice las decisiones representadas.
   - Referencia explícita a la fuente documental.

---

## 6. Conclusiones del Diagnóstico

El corpus documental de Yanbal Perú posee una alta calidad técnica y metodológica, pero requiere simplificar su índice a dos niveles predominantes, consolidar micro-secciones, erradicar hipérboles y encuadrar formalmente sus 11 diagramas bajo el estándar OMG UML 2.5.1.
