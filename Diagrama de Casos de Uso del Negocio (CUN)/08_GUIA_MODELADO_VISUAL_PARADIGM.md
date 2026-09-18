# Guía Práctica de Modelado en Visual Paradigm (Sesión 4 UTP / Metodología RUP)

> **Carpeta:** `detalles/Diagrama de Casos de Uso del Negocio (CUN)`  
> **Documento:** `08_GUIA_MODELADO_VISUAL_PARADIGM.md`  
> **Proyecto Oficial:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Herramienta CASE:** Visual Paradigm Community Edition (o Standard)  
> **Referencia Curricular:** UTP — Diseño e Implementación de Arquitectura Empresarial (Sesión 4: Modelado del Negocio)  
>  
> 🔗 **Documentos Vinculados:**  
> - [[00_INDICE_Y_MODELO_GENERAL_CUN]] — Modelo general y diagrama macro  
> - [[01_ACTORES_DEL_NEGOCIO]] — Catálogo de actores del negocio  

---

## 1. Configuración del Proyecto y Diagrama en Visual Paradigm

Para reproducir fielmente el modelo de Casos de Uso del Negocio estructurado para **Y-Trace** en **Visual Paradigm** (tal como se ilustra en las diapositivas 5, 7 y 8 de la clase), sigue los siguientes pasos:

### Paso 1: Creación del Diagrama
1. Abre **Visual Paradigm Community Edition**.
2. En la barra de herramientas superior, selecciona la pestaña **Diagram** $\rightarrow$ **New**.
3. En el buscador de diagramas, escribe `Use Case Diagram` y haz clic en **Next**.
4. Nombra el diagrama formalmente como:  
   `CUN_Distribucion_B2B_Yanbal_YTrace`
5. (Opcional) En el panel izquierdo (*Diagram Navigator* / *Model Explorer*), crea un paquete organizador llamado `Modelo del Negocio (CUN)` y arrastra el diagrama dentro.

---

## 2. Modelado de los Actores del Negocio (`<<business actor>>`)

En la metodología RUP, el actor del negocio se distingue del actor de software porque posee una barra inclinada sobre el pecho o el estereotipo explícito `<<business actor>>`:

1. Selecciona la herramienta **Actor** en la barra lateral (*Diagram Toolbar*).
2. Haz clic en el lienzo y crea los **5 actores del negocio**:
   - `Socio Logístico / Conductor`
   - `Punto de Destino / Agencia Receptora`
   - `Supervisor de Distribución`
   - `Jefe de Distribución`
   - `Operador SAC / Soporte Logístico`
3. **Aplicación del Estereotipo RUP:**
   - Haz clic derecho sobre el actor $\rightarrow$ selecciona **Stereotypes** $\rightarrow$ **Edit Stereotypes...**
   - Si existe en la lista de perfiles RUP, selecciona `business actor`.
   - Si estás en la versión *Community* básica, escribe manualmente en el campo *Stereotype*: `business actor`.
   - Visual Paradigm mostrará automáticamente la etiqueta `<<business actor>>` sobre el nombre del actor y, según la versión, modificará el ícono con la banda diagonal RUP.

---

## 3. Modelado de los Casos de Uso del Negocio (`<<business use case>>`)

1. Selecciona la herramienta **Use Case** (óvalo) en la barra de herramientas.
2. Dibuja y nombra los **5 procesos del negocio**:
   - `CUN-01: Despacho y Salida de Carga en Centro de Distribución`
   - `CUN-02: Traslado Interprovincial y Monitoreo de Carga en Tránsito`
   - `CUN-03: Gestión de Incidencias y Contingencias Viales en Ruta`
   - `CUN-04: Entrega y Recepción de Carga en Punto de Destino`
   - `CUN-05: Auditoría de Trazabilidad y Rendimiento de Distribución`
3. **Aplicación del Estereotipo RUP de Negocio:**
   - Haz clic derecho sobre el óvalo $\rightarrow$ selecciona **Stereotypes** $\rightarrow$ **Edit Stereotypes...**
   - Escribe o selecciona: `business use case`.
   - En Visual Paradigm, esto colocará el estereotipo `<<business use case>>` sobre el óvalo y trazará una **línea diagonal interior** que cruza el óvalo (notación oficial RUP mostrada en la diapositiva 5).

---

## 4. Trazado de Relaciones de Asociación

En este nivel macro del negocio, se utilizan exclusivamente líneas de **Asociación** (`Association` sólida sin flechas direccionales ni saturación de includes/extends, siguiendo exactamente el ejemplo de la diapositiva 7):

1. Selecciona la herramienta **Association** en el *Toolbar*.
2. Une cada Actor con sus Casos de Uso del Negocio según la siguiente matriz:

| Actor del Negocio | Conectar con (Asociación Sólida): |
| :--- | :--- |
| **Supervisor de Distribución** | $\rightarrow$ `CUN-01`, `CUN-02`, `CUN-03`, `CUN-04` |
| **Socio Logístico / Conductor** | $\rightarrow$ `CUN-01`, `CUN-02`, `CUN-03`, `CUN-04` |
| **Punto de Destino / Agencia Receptora** | $\rightarrow$ `CUN-04` |
| **Jefe de Distribución** | $\rightarrow$ `CUN-05` |
| **Operador SAC / Soporte Logístico** | $\rightarrow$ `CUN-05` |

---

## 5. Delimitación Visual del Límite del Negocio (*Business Boundary*)

Para una presentación formal de Arquitectura Empresarial:
1. Selecciona el elemento **System / Boundary** (rectángulo contenedor en el *Toolbar*).
2. Enmarca los 5 óvalos de casos de uso dentro del rectángulo.
3. Coloca como título del contenedor:  
   `Negocio: Cadena de Distribución y Trazabilidad B2B (Yanbal Perú)`
4. Deja los 5 actores situados **fuera** del rectángulo del límite del negocio:
   - A la izquierda: Actores operativos de salida y tránsito (`Supervisor de Distribución`, `Socio Logístico / Conductor`).
   - A la derecha: Actores de recepción, control y soporte (`Punto de Destino / Agencia`, `Jefe de Distribución`, `Operador SAC`).

---

## 6. Verificación de Árbol en *Diagram Navigator*

En el panel lateral izquierdo (*Diagram Navigator* / *Model Explorer*), la jerarquía debe lucir organizada idéntica a la diapositiva 7 de la clase:

```
📁 CUN_Distribucion_B2B_Yanbal_YTrace
├── 👤 Socio Logístico / Conductor
├── 👤 Punto de Destino / Agencia Receptora
├── 👤 Supervisor de Distribución
├── 👤 Jefe de Distribución
├── 👤 Operador SAC / Soporte Logístico
├── 🔵 CUN-01: Despacho y Salida de Carga en Centro de Distribución
├── 🔵 CUN-02: Traslado Interprovincial y Monitoreo de Carga en Tránsito
├── 🔵 CUN-03: Gestión de Incidencias y Contingencias Viales en Ruta
├── 🔵 CUN-04: Entrega y Recepción de Carga en Punto de Destino
└── 🔵 CUN-05: Auditoría de Trazabilidad y Rendimiento de Distribución
```

---

## 7. Exportación para Entrega de Laboratorio

1. Menú **Project** $\rightarrow$ **Export** $\rightarrow$ **Active Diagram as Image...**
2. Formato recomendado: **PNG** o **SVG** con resolución de 300 DPI para máxima nitidez en el informe académico.
3. Copia la imagen exportada a la carpeta de tu entrega o insértala directamente en tu reporte de Arquitectura Empresarial.
