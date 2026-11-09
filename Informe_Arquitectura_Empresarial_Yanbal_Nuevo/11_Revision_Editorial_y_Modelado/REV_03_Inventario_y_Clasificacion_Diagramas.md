# INVENTARIO Y CLASIFICACIÓN DE DIAGRAMAS UML 2.5.1
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Evaluación de Tipología, Categoría y Ubicación Documental de Modelos Existentes

---

## 1. Introducción y Marco de Clasificación UML 2.5.1

El presente documento constituye la entrega actualizada de **REV_03: Inventario y Clasificación de Diagramas UML 2.5.1**, desarrollada para evaluar la totalidad de los modelos visuales existentes en el proyecto de Arquitectura Empresarial de Yanbal Perú (Unique S.A.), determinando con rigor técnico qué tipo de diagrama UML representa cada modelo, su categoría formal y en qué apartado documental corresponde incorporarlo.

### 1.1. Principio Metodológico de Clasificación Factual
- **Estándar Exclusivo:** OMG Unified Modeling Language (UML) versión 2.5.1.
- **Sin Notaciones Ajenas:** Queda descartada la inclusión de diagramas adicionales, modelos Mermaid alternativos o extensiones fuera del estándar UML oficial.
- **Clasificación por Naturaleza Real:** La tipología no se determina por semejanza visual o por el nombre del archivo `.puml`, sino por la semántica real de lo que se está modelando.

### 1.2. Taxonomía de Diagramas en OMG UML 2.5.1
1. **Diagramas Estructurales:** Representan los elementos estáticos del sistema y sus relaciones (Clases, Objetos, Componentes, Despliegue, Paquetes, Estructura Compuesta, Perfiles).
2. **Diagramas de Comportamiento:** Representan la dinámica funcional y la evolución temporal de los sistemas o procesos (Casos de Uso, Actividades, Máquina de Estados).
3. **Diagramas de Interacción:** Subcategoría de comportamiento enfocada en el intercambio cronológico de mensajes entre elementos (Secuencia, Comunicación, Tiempos, Vista General de Interacción).

---

## 2. Tabla Sintética de Clasificación y Ubicación Documental

Conforme a la estructura solicitada, a continuación se presenta la tabla ejecutiva de clasificación para la totalidad de diagramas canónicos del proyecto:

| ID | Título Recomendado | Tipo UML (OMG 2.5.1) | Categoría UML | Apartado donde debe ubicarse |
| :---: | :--- | :---: | :---: | :--- |
| **DIAG-01** | Visión Conceptual de la Capacidad MTD y Límites del Sistema | Estructura Compuesta / Paquetes *(Limitación UML)* | Estructural *(con limitaciones)* | **Capítulo 3 (Fase A), Apartado 3.4** |
| **DIAG-02** | Casos de Uso del Negocio B2B para Despacho y Trazabilidad | Casos de Uso | Comportamiento | **Capítulo 4 (Fase B), Apartado 4.1** |
| **DIAG-03** | Proceso Operativo de Despacho Actual AS-IS | Actividades (con Particiones / Swimlanes) | Comportamiento | **Capítulo 4 (Fase B), Apartado 4.2** |
| **DIAG-04** | Proceso Operativo de Despacho Propuesto TO-BE | Actividades (con Particiones / Swimlanes) | Comportamiento | **Capítulo 4 (Fase B), Apartado 4.3** |
| **DIAG-05** | Interacción Temporal y Desfases en la Línea Base AS-IS | Secuencia | Interacción | **Capítulo 5 (Fase C), Apartado 5.3** |
| **DIAG-06** | Coreografía Reactiva de Eventos y Notificación TO-BE | Secuencia | Interacción | **Capítulo 5 (Fase C), Apartado 5.3** |
| **DIAG-07** | Despliegue de la Arquitectura Tecnológica TO-BE | Despliegue | Estructural | **Capítulo 6 (Fase D), Apartado 6.2** |
| **DIAG-08** | Arquitectura de Solución y Mapa de Paquetes de Trabajo | Componentes (en Paquetes) | Estructural | **Capítulo 7 (Fase E), Apartado 7.3** |
| **DIAG-09** | Roadmap Estratégico de Transición (Horizonte S1 a S24) | Máquina de Estados | Comportamiento | **Capítulo 8 (Fase F), Apartado 7.2** |
| **DIAG-10** | Marco de Gobernanza, Contratos AC y Puertas de Calidad | Actividades / Paquetes | Comportamiento / Estructural | **Capítulo 9 (Fase G), Apartado 8.3** |
| **DIAG-11** | Circuito de Gestión del Cambio y Ciclo Evolutivo ADM | Actividades | Comportamiento | **Capítulo 10 (Fase H), Apartado 9.3** |

---

## 3. Análisis Individual de Denominación, Naturaleza y Ubicación

A continuación se audita cada diagrama determinando su concordancia semántica, la pertinencia de su título y su adecuada integración documental:

### 3.1. DIAG-01
- **Identificador Canónico:** `DIAG-01`
- **Título Oficial Actual:** Diagrama Conceptual de la Visión de la Solución Propuesta (Capacidad MTD)
- **Título Recomendado:** *Visión Conceptual de la Capacidad MTD y Límites del Sistema (DIAG-01)*
- **Tipo UML:** **Estructura Compuesta** (o **Diagrama de Paquetes**).  
  *Limitación Metodológica:* El modelo original fue concebido en Mermaid como un diagrama de contexto/flujo de bloques. UML no posee una notación pura de "Visión de Contexto de Negocio"; la aproximación más rigurosa en UML 2.5.1 es un **Diagrama de Estructura Compuesta** (modelando puertos y conectores entre los subsistemas de Almacén, MTD y Canales) o un **Diagrama de Paquetes** que agrupe las áreas funcionales.
- **Categoría UML:** Estructural.
- **Ubicación Documental:** Capítulo 3 (Fase A: Visión de Arquitectura), Apartado 3.4.
- **Pertinencia de Ubicación:** Es plenamente adecuada. Comunica a los directivos la frontera funcional del MTD sin entrar en detalles de diseño de software. Debe permanecer en el cuerpo principal del capítulo.

### 3.2. DIAG-02
- **Identificador Canónico:** `DIAG-02`
- **Título Oficial Actual:** Diagrama UML de Casos de Uso del Negocio: Preparación, Despacho y Trazabilidad B2B (CUN-01 a CUN-06)
- **Título Recomendado:** *Casos de Uso del Negocio B2B para Despacho y Trazabilidad (DIAG-02)*
- **Tipo UML:** **Casos de Uso (Use Case Diagram)**.
- **Categoría UML:** Comportamiento.
- **Ubicación Documental:** Capítulo 4 (Fase B: Arquitectura de Negocio), Apartado 4.1.
- **Pertinencia de Ubicación:** Es el lugar idóneo para abrir la Arquitectura de Negocio, delimitando las interacciones entre los actores (`ACT-01..08`) y los casos de uso (`CUN-01..06`). Debe permanecer en el cuerpo principal.

### 3.3. DIAG-03
- **Identificador Canónico:** `DIAG-03`
- **Título Oficial Actual:** Diagrama UML de Actividades del Proceso de Negocio Actual (AS-IS): Preparación, Despacho, Transporte B2B y Logística Inversa
- **Título Recomendado:** *Proceso Operativo de Despacho Actual AS-IS (DIAG-03)*
- **Tipo UML:** **Actividades (Activity Diagram with Partitions / Swimlanes)**.
- **Categoría UML:** Comportamiento.
- **Ubicación Documental:** Capítulo 4 (Fase B: Arquitectura de Negocio), Apartado 4.2.
- **Pertinencia de Ubicación:** Esencial para fundamentar la línea base del negocio. Modela la secuencia de tareas a través de 8 carriles organizacionales y evidencia visualmente el desfase de $\sim 2\text{ horas}$ en transporte. Debe permanecer en el cuerpo principal.

### 3.4. DIAG-04
- **Identificador Canónico:** `DIAG-04`
- **Título Oficial Actual:** Diagrama UML de Actividades del Proceso de Negocio Propuesto (TO-BE): Trazabilidad Reactiva B2B con Capacidad MTD
- **Título Recomendado:** *Proceso Operativo de Despacho Propuesto TO-BE (DIAG-04)*
- **Tipo UML:** **Actividades (Activity Diagram with Partitions / Swimlanes)**.
- **Categoría UML:** Comportamiento.
- **Ubicación Documental:** Capítulo 4 (Fase B: Arquitectura de Negocio), Apartado 4.3.
- **Pertinencia de Ubicación:** Permite comparar inmediatamente la mejora operativa frente a DIAG-03, introduciendo las marcas $T_0$, la notificación $T_1$ y el mecanismo *Store-and-Forward*. Debe permanecer en el cuerpo principal.

### 3.5. DIAG-05
- **Identificador Canónico:** `DIAG-05`
- **Título Oficial Actual:** Diagrama UML de Secuencia: Interacción Temporal y Latencias en la Línea Base (AS-IS)
- **Título Recomendado:** *Interacción Temporal y Desfases en la Línea Base AS-IS (DIAG-05)*
- **Tipo UML:** **Secuencia (Sequence Diagram)**.
- **Categoría UML:** Interacción.
- **Ubicación Documental:** Capítulo 5 (Fase C: Sistemas de Información), Apartado 5.3.  
  *(Nota de Ubicación: En el repositorio físico el archivo .puml reside en la carpeta de diagramas de Fase B, pero metodológicamente modela el intercambio de mensajes y latencias entre sistemas de información `SIS-01` a `SIS-09`, por lo que su ubicación natural en el informe es la Fase C).*
- **Pertinencia de Ubicación:** Es adecuada en el apartado 5.3 para contrastar el acoplamiento batch frente a la solución reactiva. Debe permanecer en el cuerpo principal.

### 3.6. DIAG-06
- **Identificador Canónico:** `DIAG-06`
- **Título Oficial Actual:** Diagrama UML de Secuencia: Coreografía Reactiva de Eventos y Notificación TO-BE con MTD
- **Título Recomendado:** *Coreografía Reactiva de Eventos y Notificación TO-BE (DIAG-06)*
- **Tipo UML:** **Secuencia (Sequence Diagram)**.
- **Categoría UML:** Interacción.
- **Ubicación Documental:** Capítulo 5 (Fase C: Sistemas de Información), Apartado 5.3.
- **Pertinencia de Ubicación:** Complementa a DIAG-05 en el mismo apartado, demostrando la reducción de latencia ($\le 30\text{ min}$) mediante el fragmento `alt` y los microservicios `CMP-ING-01..NOT-06`. Debe permanecer en el cuerpo principal.

### 3.7. DIAG-07
- **Identificador Canónico:** `DIAG-07`
- **Título Oficial Actual:** Diagrama UML de Despliegue de la Arquitectura Tecnológica Objetivo (TO-BE)
- **Título Recomendado:** *Despliegue de la Arquitectura Tecnológica TO-BE (DIAG-07)*
- **Tipo UML:** **Despliegue (Deployment Diagram)**.
- **Categoría UML:** Estructural.
- **Ubicación Documental:** Capítulo 6 (Fase D: Arquitectura Tecnológica), Apartado 6.2.
- **Pertinencia de Ubicación:** Es la figura central de la Fase D. Modela los nodos de cómputo on-premise en CD Lurín, el servidor de aplicaciones MTD, el clúster del Bus corporativo `SIS-05`, la DMZ perimetral y los terminales en ruta. Debe permanecer en el cuerpo principal.

### 3.8. DIAG-08
- **Identificador Canónico:** `DIAG-08`
- **Título Oficial Actual:** Diagrama de Arquitectura de Solución Seleccionada y Mapa de Paquetes de Trabajo (Fase E)
- **Título Recomendado:** *Arquitectura de Solución y Mapa de Paquetes de Trabajo (DIAG-08)*
- **Tipo UML:** **Componentes (Component Diagram estructurado en Paquetes)**.
- **Categoría UML:** Estructural.
- **Ubicación Documental:** Capítulo 7 (Fase E: Oportunidades y Soluciones), Apartado 7.3.
- **Pertinencia de Ubicación:** Permite vincular los componentes de software (`CMP-*`, `ADP-*`) con los cinco paquetes de trabajo contractuales (`PKG-01..05`). Debe permanecer en el cuerpo principal.

### 3.9. DIAG-09
- **Identificador Canónico:** `DIAG-09`
- **Título Oficial Actual:** Diagrama del Roadmap Estratégico de Implementación y Arquitecturas de Transición
- **Título Recomendado:** *Roadmap Estratégico de Transición — Horizonte S1 a S24 (DIAG-09)*
- **Tipo UML:** **Máquina de Estados (State Machine Diagram)**.  
  *Justificación:* El modelo representa formalmente la evolución del ecosistema empresarial a través de estados arquitectónicos discretos (Estado Inicial AS-IS $\rightarrow$ Transición AT-1 $\rightarrow$ Transición AT-2 $\rightarrow$ Estado Objetivo TO-BE), donde cada cambio de estado está gobernado por condiciones de guarda estrictas (Quality Gates).
- **Categoría UML:** Comportamiento.
- **Ubicación Documental:** Capítulo 8 (Fase F: Planificación de la Migración), Apartado 7.2.
- **Pertinencia de Ubicación:** Es la pieza clave de la Fase F. Debe reflejar rigurosamente el **horizonte temporal de 24 semanas (S1 a S24)** y los tres Gates. Debe permanecer en el cuerpo principal.

### 3.10. DIAG-10
- **Identificador Canónico:** `DIAG-10`
- **Título Oficial Actual:** Marco de Gobernanza de la Implementación, Contratos y Control de Conformidad
- **Título Recomendado:** *Marco de Gobernanza de la Implementación y Puertas de Calidad (DIAG-10)*
- **Tipo UML:** **Diagrama de Actividades / Diagrama de Paquetes**.  
  *Justificación:* Estructuralmente agrupa los paquetes de contratos (`AC-PKG-01..05`) y comportamentalmente modela el circuito de evaluación de conformidad a través de las **tres Puertas de Calidad Canónicas (Gate 1, Gate 2, Gate 3)** y la gestión de dispensas (`WVR-*`).
- **Categoría UML:** Comportamiento / Estructural.
- **Ubicación Documental:** Capítulo 9 (Fase G: Gobernanza de la Implementación), Apartado 8.3.
- **Pertinencia de Ubicación:** Es el núcleo visual de la Fase G. Debe permanecer en el cuerpo principal.

### 3.11. DIAG-11
- **Identificador Canónico:** `DIAG-11`
- **Título Oficial Actual:** Marco de Gestión Continua del Cambio de Arquitectura y Ciclo Evolutivo ADM
- **Título Recomendado:** *Gestión Continua del Cambio y Ciclo Evolutivo ADM (DIAG-11)*
- **Tipo UML:** **Actividades (Activity Diagram)**.
- **Categoría UML:** Comportamiento.
- **Ubicación Documental:** Capítulo 10 (Fase H: Gestión del Cambio de Arquitectura), Apartado 9.3.
- **Pertinencia de Ubicación:** Modela el procedimiento decisional ante Solicitudes de Cambio (RFC Tipos 1, 2 y 3) y la reactivación de bucles ADM. Debe permanecer en el cuerpo principal.

---

## 4. Conclusión del Inventario y Dictamen de Ubicación

1. **Permanencia en Cuerpo Principal:** Los once diagramas canónicos (`DIAG-01` a `DIAG-11`) deben figurar directamente dentro de sus capítulos correspondientes en el Volumen 1, ya que representan la justificación gráfica indispensable de cada fase TOGAF ADM.
2. **Rol del Volumen 2 (Anexos):** En el Anexo `ANX-04` del Volumen 2 se consolidará el inventario técnico maestro y el histórico documental de los modelos, evitando sobrecargar el cuerpo narrativo del informe principal.
3. **Cero Modelos Huérfanos:** No se autoriza la inclusión de modelos auxiliares no aprobados (como las propuestas `DIAG-C1` y `DIAG-C2`), garantizando absoluta fidelidad a las fuentes originales.
