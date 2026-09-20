# Sílabo: Diseño e Implementación de Arquitectura Empresarial

> **Código de asignatura:** 100000S06I  
> **Período académico:** 2026 - Ciclo 2 Agosto  
> **Institución:** Universidad Tecnológica del Perú (UTP)  
> **Carrera:** Ingeniería de Sistemas e Informática  
> **Créditos:** 3 | **Horas semanales:** 4 | **Modalidad:** Presencial  
> **Documento origen:** `DISEÑOEIMPLEMENTACIÓNDEARQUITECTURAEMPRESARIAL_undefined.pdf`

---

## 1. Fundamentación y Sumilla

### 1.1. Fundamentación
La implementación de arquitecturas empresariales es una competencia clave para alinear los sistemas de información con las estrategias organizacionales. El curso proporciona los conceptos y herramientas necesarias para diseñar, evaluar y gestionar arquitecturas empresariales bajo estándares reconocidos (como TOGAF), analizando fortalezas y debilidades de la empresa y facilitando la transformación digital sostenible.

### 1.2. Sumilla
Curso de naturaleza práctica que aborda:
1. **Fundamentos de Arquitectura Empresarial:** Gestión de requerimientos y modelado del negocio (BPM, RUP, UML).
2. **Marco TOGAF y Método ADM:** Fases para la planificación y gobernanza de arquitecturas empresariales, modelos de madurez y gestión de servicios TI.
3. **Arquitectura Orientada a Servicios (SOA):** Implementación de Web Services (SOAP, RESTful) y microservicios en entornos empresariales.

---

## 2. Logro General y Específicos de Aprendizaje

### Logro General
> Al finalizar el curso, el estudiante implementa arquitecturas empresariales aplicando principios de gestión de requerimientos, el marco de referencia **TOGAF** y tecnologías orientadas a servicios (**SOA**) para alinear las tecnologías de información con los objetivos estratégicos de las organizaciones.

### Unidades de Aprendizaje

| Unidad | Nombre | Semanas | Logro Específico |
| :---: | :--- | :---: | :--- |
| **U1** | **Gestión de Requerimientos** | Semanas 1 a 6 | Modela requerimientos de negocio aplicando gestión de proyectos, modelado de negocio e ingeniería de requerimientos para la documentación estructurada de soluciones de software. |
| **U2** | **TOGAF (Arquitectura Empresarial)** | Semanas 7 a 14 | Aplica el marco de referencia TOGAF para el diseño y gobernanza de arquitecturas empresariales, considerando sus fases, componentes y herramientas tecnológicas. |
| **U3** | **SOA (Arquitectura Orientada a Servicios)** | Semanas 15 a 18 | Diseña arquitecturas orientadas a servicios (SOA) que integran servicios SOAP, RESTful y microservicios para garantizar la interoperabilidad entre sistemas empresariales. |

---

## 3. Sistema de Evaluación Oficial

### Fórmula de Calificación
$$\text{Promedio Final} = 25\%(\text{APF1}) + 25\%(\text{APF2}) + 10\%(\text{PA}) + 40\%(\text{PROY})$$

### Desglose de Evaluaciones

| Sigla | Descripción | Semana | Peso | Modalidad | Alcance temático |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **APF1** | **Avance de Proyecto Final 1** | **Semana 7** | **25%** | Grupal | Contexto empresarial, Fase Preliminar, Gestión de Requerimientos y Modelado UML inicial (Unidad 1). |
| **APF2** | **Avance de Proyecto Final 2** | **Semana 15** | **25%** | Grupal | TOGAF Fases A y B (Visión y Negocio), análisis de brechas e integración UML técnica (Componentes y Secuencia). |
| **PA** | **Participación en Clase** | **Semana 17** | **10%** | Individual | Intervenciones, talleres y actividades prácticas continuas. |
| **PROY** | **Proyecto Final y Sustentación** | **Semana 18** | **40%** | Grupal | Fase C (Datos y Aplicaciones), diseño SOA / Microservicios, entrega del documento consolidado completo y sustentación ejecutiva presencial. |

> [!WARNING]
> **Indicaciones sobre la evaluación:**
> 1. La nota mínima aprobatoria final es **12**.
> 2. En este curso **no aplica examen rezagado**.
> 3. Ninguna nota se reemplaza.
> 4. Los entregables son **acumulativos**: el documento de `PROY` en Semana 18 debe contener todo el proyecto consolidado desde la página 1 con las correcciones integradas.

---

## 4. Cronograma Detallado de Actividades

```mermaid
timeline
    title Cronograma de Unidades y Evaluaciones
    section Unidad 1: Requerimientos
        Semana 01 - 02 : Gestión Proyectos SW : Metodología RUP : Buenas Prácticas
        Semana 03 - 04 : Modelado Negocio (BPM, RUP) : CUN : Dominio : Actividades
        Semana 05 - 06 : Ing. Requerimientos : CMMI REQM/RD : Casos de Uso Sistema
        Semana 07 : EVALUACIÓN APF1 (25%) : Intro Arquitectura Empresarial
    section Unidad 2: TOGAF
        Semana 08 - 09 : TOGAF Preliminar & Fases A, B, C, D (Datos, Apps, Tec)
        Semana 10 - 12 : Fases E, F (Oportunidades, Migración) & G, H (Gobernanza)
        Semana 13 - 14 : Marcos de Trabajo & Metamodelo Tecnológico
        Semana 15 : EVALUACIÓN APF2 (25%) : Intro SOA y Web Services
    section Unidad 3: SOA
        Semana 16 - 17 : Web Services SOAP / WSDL / BPEL : RESTful : Microservicios : EVAL PA (10%)
        Semana 18 : EVALUACIÓN PROYECTO FINAL Y SUSTENTACIÓN (40%)
```

### Detalle Sesión por Sesión (Semanas 1 a 7 - Base de APF1)

- **Semana 1 (Sesión 1 y 2):** Gestión de Proyectos de Desarrollo de Software (buenas prácticas, errores comunes, riesgos). Modelos de proceso (Cascada, Espiral, Ágil). Conceptos clave de RUP: fases, disciplinas y las 6 mejores prácticas.
- **Semana 2 (Sesión 3 y 4):** Modelado del Negocio. Enfoques BPM y RUP. Del negocio al desarrollo de SW (SOA, MDA). Casos de uso de negocio (CUN).
- **Semana 3 (Sesión 5 y 6):** Modelado del Negocio: Modelo de dominio, clases de negocio, modelo de objetos de negocio y diagramas de actividades AS-IS.
- **Semana 4 (Sesión 7 y 8):** Proceso de Ingeniería de Requerimientos. Documentos de requerimientos, validación, revisión, clasificación y elicitación. Evolución de requerimientos.
- **Semana 5 (Sesión 9 y 10):** Niveles CMMI: Nivel 2 CMMI (Gestión de Requerimientos - REQM), Nivel 3 CMMI (Desarrollo de Requerimientos - RD).
- **Semana 6 (Sesión 11 y 12):** Diagrama de Contexto. Especificación estructurada de requerimientos y casos de uso. Administración de requerimientos y medición de tiempos.
- **Semana 7 (Sesión 13 y 14):** Introducción a la Arquitectura Empresarial (tipos de arquitectura: negocio, datos, aplicaciones y tecnología). **Entrega y evaluación de APF1**.

---

## 5. Bibliografía

- **Luis, C., Perla, V., & Humberto, C.:** *Arquitectura del Software.* Cengage Learning Editores SA.
- **Jiménez Partearroyo, M.:** *La gestión informática de la empresa: nuevos modelos de negocio.* RA-MA Editorial.
- **Estrada Hernández, J. A.:** *Modelo para la gestión de tecnologías de información y comunicación en la gestión empresarial.* Editorial Universitaria.
- **Palacios Preciado, M.:** *Modelos de negocio: propuesta de un marco conceptual para centros de productividad.* D - ESAP.
