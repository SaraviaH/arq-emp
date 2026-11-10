# SÍNTESIS DE OBSERVACIONES, PREGUNTAS FRECUENTES Y CRITERIOS DE CALIFICACIÓN DEL DOCENTE EN EXPOSICIÓN
## Proyecto de Arquitectura Empresarial — Yanbal Perú (Unique S.A.)
### Inteligencia Académica Extraída de la Evaluación Docente en Clase (Sin modificar archivos existentes)

> **Propósito del Documento:** Conservar únicamente la información más importante sobre **qué exige, qué pregunta y qué corrige el profesor** durante la sustentación y revisión del proyecto (Fases Preliminar y A hasta la H), y cómo está preparado nuestro informe de Yanbal Perú frente a cada punto.  
> **Restricción Cumplida:** Ningún archivo existente del proyecto ha sido editado ni modificado.

---

## 1. Hallazgo Clave sobre el Alcance Evaluado (Confirmación sobre `ASM-ACAD-01`)

- Durante la exposición, los grupos presentaron y fueron evaluados sobre el **ciclo completo de TOGAF ADM**:
  - **Fase Preliminar y Fase A** (Principios, Diagnóstico, Visión de Arquitectura, Objetivos Estratégicos, KPIs, Stakeholders).
  - **Fase B** (Mapa de Capacidades, Casos de Uso de Negocio, Diagramas de Actividades AS-IS y TO-BE, Diagramas de Secuencia).
  - **Fase C** (Modelo Lógico de Datos, Inventario de Aplicaciones AS-IS/TO-BE, Diagrama de Componentes en Paquetes).
  - **Fase D** (Arquitectura Tecnológica y Diagrama de Despliegue).
  - **Fase E** (Análisis de Brechas, Matriz de Valor/Riesgo, Dependencias y Paquetes de Trabajo).
  - **Fase F** (Arquitecturas de Transición: AS-IS $\rightarrow$ Transición 1 $\rightarrow$ Transición 2 $\rightarrow$ TO-BE, y Hoja de Ruta por Hitos).
  - **Fase G** (Gobernanza de la Implementación, Contratos de Arquitectura, Comité y Rol del Arquitecto).
  - **Fase H** (Gestión del Cambio de Arquitectura, Solicitudes RFC Tipo 1, Tipo 2 y Tipo 3).

---

## 2. Lo Más Importante que Pide y Pregunta el Profesor por Fase

### A. Fase Preliminar y Fase A: Visión de Arquitectura
1. **No confundir "Visión de la Empresa" con "Visión de la Arquitectura" *(Min. 5:20 – 6:02, 10:27)*:**
   - **Qué reclama el profesor:** *"¿Dónde está la visión? La visión de la arquitectura, no la visión de la empresa. Si no está la visión de arquitectura, no vamos a poder garantizar los demás pasos."*
   - **Qué debe mostrarse sí o sí:** El diagrama y declaración de la **Visión de la Arquitectura (en Yanbal: `DIAG-01` — Visión Conceptual de la Capacidad `MTD`)**, mostrando cómo interactúan los actores, la lógica de negocio, las aplicaciones, los datos, la infraestructura y la seguridad.
2. **Principios de Arquitectura por Dominio *(Min. 8:04 – 8:16, 13:18 – 13:45)*:**
   - Explicar claramente los principios rectores divididos en: **Negocio, Datos, Aplicaciones, Tecnología y Seguridad** (disponibilidad, escalabilidad, usabilidad, control de accesos por roles y protección de datos).
3. **Diagnóstico AS-IS, Objetivos Estratégicos y KPIs *(Min. 6:06 – 6:50, 15:10)*:**
   - El profesor pregunta directamente: *"¿Cuáles son los objetivos estratégicos?"* y exige que estén expresados con **métricas y KPIs porcentuales o de tiempo** (ej. en Yanbal: reducir el desfase de actualización de estados de ~2 horas a $\le 30\text{ minutos}$).
4. **Matriz de Stakeholders / Interesados *(Min. 7:02 – 7:53)*:**
   - Debe incluir columnas claras: **Actor / Interesado, Tipo (Interno/Externo), Rol, Interés Principal, Nivel de Influencia (Alta/Media/Baja) y Preocupación Principal**.

---

### B. Fase B: Arquitectura de Negocio
1. **Comparación Explícita entre AS-IS y TO-BE *(Min. 9:45 – 10:55, 12:14 – 13:15)*:**
   - **Qué pregunta el profesor:** *"Ese es el TO-BE. ¿Cuál es el AS-IS? ¿Qué han mejorado? ¿Cuál es la diferencia? ¿Qué cajitas han agregado ahora en este diagrama que no estaban en el AS-IS?"*
   - **Regla de oro:** Siempre presentar el **AS-IS antes del TO-BE** y resaltar visualmente (con color distinto o etiqueta) **qué actividades o casos de uso son nuevos o mejorados en el TO-BE** (en nuestro proyecto Yanbal ya están diferenciados en verde `#E8F5E9` con `[Propuesto MTD-R01..R06]` frente a `[Confirmado]`).
2. **No confundir Caso de Uso de Negocio con Caso de Uso de Sistema, ni con Diagrama de Actividades *(Min. 8:55 – 9:15, 10:19 – 10:22, 13:27 – 15:02)*:**
   - **Qué corrige el profesor:** Cuestiona cuando en Fase B ponen casos de uso de sistema (*"Iniciar sesión"*) en lugar de **Casos de Uso de Negocio**, o cuando los actores del diagrama no coinciden con lo expuesto.
3. **Los Diagramas deben "conversar" con el Mapa de Capacidades y Procesos *(Min. 15:35 – 15:58)*:**
   - **Qué reclama el profesor:** *"Esos son los procesos / capacidades... Pero tampoco conversa con el diagrama. ¿Cómo se relaciona esto con su diagrama?"*
   - **Regla de oro:** Los nombres de capacidades (`CAP-01..06`), procesos (`PROC-01..05`), casos de uso (`CUN-01..06`) y actores (`ACT-01..10`) deben ser **exactamente los mismos** en el texto, las tablas y los diagramas.
4. **Coherencia Cronológica en los Diagramas de Secuencia *(Min. 16:57 – 20:39)*:**
   - El profesor revisa línea por línea el orden temporal de las flechas (ej. *"¿Primero registra o primero paga? ¿Por qué envía confirmación si aún no terminó el paso previo?"*) y pide no mezclar objetos de negocio con acciones puras de base de datos sin sentido funcional.

---

### C. Fase C: Arquitectura de Sistemas de Información (Datos y Aplicaciones)
1. **Separar claramente Capa de Datos vs. Inventario de Aplicaciones *(Min. 20:14 – 20:45, 28:09 – 29:15)*:**
   - **Qué pregunta el profesor:** *"¿Cuántas aplicaciones tenía la organización en el AS-IS y cuántas va a tener al final? ¿Dónde está el inventario de aplicaciones? ¿Esas 9 entidades van a ser tus aplicaciones?"*
   - **Regla de oro:**
     - En **Arquitectura de Datos (`4.1`)**: Mostrar el Modelo Lógico de Datos (PK, FK, campos `UNIQUE`, campos `NULL` opcionales), explicar **qué entidades o estados nuevos se agregaron respecto al AS-IS** (en Yanbal: `ENT-04 Linea_Picking_Trazable`, `ENT-07 Evento_Trazabilidad_Canonica` con marcas $T_0/T_1$, `ENT-08`, `ENT-09`) y destacar el **componente/registro de Auditoría** *(Min. 20:45 – 20:56: "Una decisión: tiene que tener auditoría")*.
     - En **Arquitectura de Aplicaciones (`4.2`)**: Mostrar el **Inventario de Aplicaciones Actuales (`SIS-01` a `SIS-10`)** y los **Componentes Propuestos (`CMP-ING-01` a `CMP-NOT-06` y adaptadores `ADP-*`)**.
2. **Prohibido justificar motores de BD o tecnologías diciendo "porque es gratis" o "porque es pequeña" *(Min. 18:37 – 19:26, 0:22 – 0:55)*:**
   - **Advertencia textual del profesor:** *"¿Por qué recomiendan uno gratis? Ustedes son los arquitectos, ustedes tienen que decidir lo que le va a hacer bien a la organización. Así sea pequeño o grande, la solución siempre es mirar a futuro."*
   - **Regla de oro:** Justificar la base de datos y tecnología exclusivamente por **robustez, escalabilidad, soporte transaccional, particionamiento de datos históricos, alta disponibilidad y seguridad**.
3. **Diagrama de Componentes organizado en Paquetes/Capas *(Min. 21:50 – 22:06, 24:29 – 24:55)*:**
   - Explicar cómo se dividen los paquetes (Ingesta, Negocio/Correlación, Adaptadores, Persistencia, Salida/Bus) y cómo interactúan secuencialmente.

---

### D. Fase D: Arquitectura Tecnológica y Diagrama de Despliegue
1. **Identificar correctamente el nombre técnico del diagrama: "Diagrama de Despliegue" *(Min. 24:00 – 24:22)*:**
   - El profesor pregunta directamente: *"¿Cómo se llama ese diagrama? ¿Componentes o Despliegue?"* $\rightarrow$ La respuesta exacta es **Diagrama de Despliegue UML (*Deployment Diagram*)**.
2. **Coherencia estricta si se propone Arquitectura Híbrida (On-Premise + Cloud) *(Min. 22:46 – 25:43, 26:35 – 27:31, 4:10 – 4:38)*:**
   - **Qué cuestiona el profesor:** *"Si dicen despliegue híbrido, ¿dónde está el híbrido en el diagrama? Si es híbrido tienes algo local y algo en la nube. ¿Por qué es importante la nube? ¿Dónde están las PCs de los usuarios y dónde están los servidores? ¿Cómo se conecta con el exterior (Firewall / HTTPS)?"*
   - **Cómo responder con nuestro `DIAG-07` de Yanbal:**
     - **Por qué es Híbrido:** Porque el ERP crítico `SAP R3 (SIS-01)`, el BWMS `SPY (SIS-02)` y las terminales RF (`SIS-10`) operan **On-Premise en la planta y almacén de Lurín (`Node_Lurin`)**, mientras que la pasarela perimetral (`API Gateway` en DMZ), el `Bus de Integración (SIS-05)` y el clúster de microservicios `MTD` operan en la **Nube Corporativa (`Node_Cloud`)** para garantizar escalabilidad elástica en cierres de campaña, alta disponibilidad ($\ge 99.5\%$) y recepción segura (HTTPS / TLS 1.3 / mTLS / WAF) desde los sistemas externos de los socios logísticos (`Node_Logistics`).

---

### E. Fase E y Fase F: Paquetes de Trabajo, Dependencias, Transición y Roadmap
1. **Orden de Priorización y Dependencias entre Paquetes de Trabajo *(Min. 2:02 – 3:20, 26:18 – 26:52)*:**
   - **Qué pregunta el profesor:** *"¿Cuál paquete va primero y por qué? ¿Qué paquete depende de otro?"*
   - **Qué destacar:** Explicar la **Matriz de Priorización (Valor de negocio, Riesgo técnico, Criticidad)** y las **dependencias técnicas** (ej. en Yanbal: `PKG-01` [Contratos y API Gateway] va primero porque `PKG-02` [Microservicios MTD] y `PKG-03` [Adaptadores internos] dependen de él; luego `PKG-04` [Socios Logísticos y Store-and-Forward] y finalmente `PKG-05` [Telemetría SLA y Retornos]).
2. **Diagrama de Arquitecturas de Transición y Hoja de Ruta por Hitos *(Min. 3:21 – 4:07, 27:00 – 27:58)*:**
   - Mostrar claramente la evolución gradual: **Estado Inicial `AS-IS` $\rightarrow$ `Transición 1 (AT-1)` $\rightarrow$ `Transición 2 (AT-2)` $\rightarrow$ `Arquitectura Objetivo (TO-BE)`**, indicando la duración de cada fase, los hitos (`M1`, `M2`, `M3` / `Gate 1`, `Gate 2`, `Gate 3`) y los **Contratos de Arquitectura** resultantes.

---

### F. Fase G: Gobernanza de la Implementación
1. **Pregunta Trampa del Profesor: "¿Dónde está el Arquitecto en el Comité que aprueba?" *(Min. 5:19 – 5:35, 28:22 – 29:03)*:**
   - **Qué corrigió severamente el profesor a ambos grupos:**
     - *"¿Quién valida que los controles se cumplan? — El Comité Directivo. — ¿Y quién es el Comité Directivo? ¿Dónde está el arquitecto? Ahí faltaría un rol de arquitecto, ¿no? Porque el arquitecto es el que aprueba la arquitectura, el gobernante."*
   - **Ventaja en nuestro informe de Yanbal (`DIAG-10` y Apartado `8.2`):** En nuestro proyecto, el **Architecture Board (Consejo de Arquitectura)** ya incluye explícitamente a **`ACT-12: Arquitecto Líder (Dirección Técnica)`** junto a Gerencia de TI (`ACT-13`), Oficial de Seguridad CISO (`ACT-11`), Jefatura de Distribución (`ACT-06`) y Jefatura de Almacén (`ACT-03`). Es fundamental mencionar explícitamente al **Arquitecto Líder (`ACT-12`)** al explicar la Fase G.

---

### G. Fase H: Gestión del Cambio de Arquitectura
1. **Clasificación en 3 Tipos de Cambios (RFC Tipo 1, Tipo 2 y Tipo 3) y Flujo de Aprobación *(Min. 6:26 – 9:24, 29:11 – 29:45)*:**
   - El profesor espera escuchar:
     1. Cómo se solicita un cambio formal mediante una **RFC (*Request for Architecture Change*)**.
     2. Los **3 niveles de cambio** con ejemplos aplicados al caso:
        - **Tipo 1 (Cambio Menor / Mantenimiento Operativo):** No altera contratos ni arquitectura (ej. ajuste de dashboard o parche interno); lo aprueba el Comité Técnico.
        - **Tipo 2 (Cambio Incremental / Evolutivo):** Afecta conectores o agrega un nuevo socio logístico (`ACT-07`) manteniendo la arquitectura base; requiere aprobación del *Architecture Board* y actualización de contratos `AC-PKG-XX`.
        - **Tipo 3 (Cambio Estratégico / Re-arquitectura):** Rompe supuestos base (ej. migrar `SAP R3` a S/4HANA o abrir reparto B2C de última milla); requiere aprobación de la Dirección y **dispara un nuevo ciclo TOGAF ADM**.

---

## 3. Lista de Chequeo Rápida para el Informe y la Exposición

| Punto Crítico Evaluado por el Docente | Estado en el Informe Yanbal Perú | Referencia en nuestro Repositorio |
| :--- | :---: | :--- |
| ¿Existe una **Visión de la Arquitectura** distinta de la visión de la empresa? | **Cumplido** | Fase A (`2.7` y `DIAG-01`) |
| ¿Están los **Principios de Arquitectura** divididos en Negocio, Datos, Apps, Tecnología y Seguridad? | **Cumplido** | Fase Preliminar (`1.2`) |
| ¿Están los **Objetivos Estratégicos** respaldados con **KPIs** medibles? | **Cumplido** | Fase A (`2.3` y `2.5`: `OBJ-01..08`, `KPI-01..08`) |
| ¿Está la **Matriz de Stakeholders** con Rol, Interés, Influencia y Preocupación? | **Cumplido** | Fase A (`2.6`) |
| ¿Se puede comparar visualmente qué cambió entre **AS-IS y TO-BE** en Fase B? | **Cumplido** | Fase B (`DIAG-03` vs `DIAG-04` y `DIAG-05` vs `DIAG-06`) |
| ¿"Conversan" los procesos/capacidades con los Casos de Uso y Actividades? | **Cumplido** | Fase B (`CAP-01..06`, `PROC-01..05`, `CUN-01..06`) |
| ¿Están separados el **Modelo Lógico de Datos** (con Auditoría) y el **Inventario de Aplicaciones**? | **Cumplido** | Fase C (`4.1` Datos con `ENT-07` y `4.2` Aplicaciones `SIS-01..10`) |
| ¿Se justifica la tecnología por **escalabilidad/seguridad** y NO por "ser gratis"? | **Cumplido** | Fase D (`5.2`, `5.5`, `5.8`) |
| ¿Muestra el **Diagrama de Despliegue** claramente la parte **On-Premise** y la parte **Cloud** (Híbrida)? | **Cumplido** | Fase D (`5.6` y `DIAG-07` / `DIAG-07.1..07.3`) |
| ¿Están los **Paquetes de Trabajo** priorizados con sus **dependencias**? | **Cumplido** | Fase E (`6.3`, `DIAG-08.2`) y Fase F (`7.2`, `7.5`) |
| ¿Está el **Diagrama de Transición** (`AS-IS` $\rightarrow$ `AT-1` $\rightarrow$ `AT-2` $\rightarrow$ `TO-BE`) y Hoja de Ruta por Hitos? | **Cumplido** | Fase F (`7.1`, `7.3`, `DIAG-09`) |
| ¿Está explícitamente el **Arquitecto (`ACT-12`)** en el Comité de Gobernanza (*Architecture Board*)? | **Cumplido** | Fase G (`8.2`, `DIAG-10`) |
| ¿Están definidos los **3 tipos de cambio (RFC Tipo 1, 2 y 3)** y quién los aprueba? | **Cumplido** | Fase H (`9.4`, `9.6`, `DIAG-11`) |

