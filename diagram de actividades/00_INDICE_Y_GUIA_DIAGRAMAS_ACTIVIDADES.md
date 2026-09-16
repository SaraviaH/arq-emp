# Índice y Guía Maestra: Diagramas de Actividades AS-IS (Modelado de Procesos Actuales)

> **Carpeta:** `diagram de actividades/`  
> **Proyecto:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Curso:** Diseño e Implementación de Arquitectura Empresarial (UTP)  
> **Entregable Oficial:** Avance de Proyecto Final 1 (APF1 — Semanas 6 a 7) — Peso: 25%  
> **Estándar:** UML 2.5 / RUP / TOGAF Fase Preliminar  

---

## 1. Justificación Académica y Requisitos del APF1

En el Capítulo 3.3, numeral 3 de la **Guía Oficial de Propuesta de Proyecto Final (UTP)** se establece como entregable obligatorio:

> *«3. **Diagramas de Actividades AS-IS:**  
>    - Flujo de los procesos de negocio actuales enfocados en el problema crítico a resolver.  
>    - Flujo general de los procesos de negocio actuales.»*

Esta carpeta contiene los **dos diagramas oficiales exigidos**, estructurados rigurosamente con **particiones (*swimlanes*)**, condiciones de guarda y apego estricto a la realidad del negocio:

```mermaid
graph TD
    subgraph ENTREGABLE_APF1 ["Diagramas de Actividades AS-IS Requeridos"]
        D1["<b>01. Flujo Enfocado en el Problema Crítico</b><br/>• Zoom del proceso actual de trazabilidad (6 Carriles)<br/>• Desfase promedio de aproximadamente 2 horas<br/>• Margen de desconocimiento y consultas a SAC"]
        D2["<b>02. Flujo General de Negocio Actual</b><br/>• Proceso completo actual de distribución (5 Carriles)<br/>• Despacho, traslado, entrega y logística inversa"]
    end
```

> [!IMPORTANT]
> **Criterios Metodológicos Fundamentales:**  
> 1. **Conservación de Ambos Modelos:** El **Diagrama 01** realiza un *zoom* sobre la brecha de trazabilidad de 2 horas; el **Diagrama 02** documenta el macro-proceso completo de distribución física que opera hoy en la empresa.
> 2. **Pureza Estricta del AS-IS:** Ninguno de los dos diagramas incorpora requerimientos ni funcionalidades futuras de Y-Trace (como código de activación, PWA, GPS de la solución ni sincronización offline). Dichos elementos corresponden exclusivamente al modelo **TO-BE**.
> 3. **Integridad del Proceso:** No se elimina ninguna actividad del proceso actual solo porque no exista como requerimiento funcional. Los 34 RF describen el sistema futuro; el AS-IS describe el proceso que realmente ocurre en el negocio.
> 4. **El Problema Central:** Yanbal **sí tiene tracking** (plataformas como Drivin y ENSDY); el problema radica en que la información **presenta un desfase promedio de 2 horas en actualizarse**, generando un margen de desconocimiento temporal que Y-Trace resolverá reduciendo dicho desfase a $\le 30$ minutos.

---

## 2. Mapa de Documentos de esta Carpeta

| Documento | Descripción y Enfoque | Formatos Incluidos |
| :--- | :--- | :--- |
| **[[01_DIAGRAMA_ACTIVIDADES_AS_IS_PROBLEMA_CRITICO]]** | Zoom del proceso actual enfocado en el problema de trazabilidad: despacho $\rightarrow$ traslado $\rightarrow$ entrega física $\rightarrow$ registro $\rightarrow$ desfase promedio de 2 horas $\rightarrow$ consulta del solicitante a SAC $\rightarrow$ dificultad para responder $\rightarrow$ actualización posterior a ENTREGADO. | Mermaid nativo + Código PlantUML con 6 *Swimlanes* + Análisis de ineficiencias del AS-IS. |
| **[[02_DIAGRAMA_ACTIVIDADES_AS_IS_GENERAL]]** | Proceso completo actual del negocio de distribución: preparación comercial $\rightarrow$ picking/consolidación $\rightarrow$ zonificación $\rightarrow$ despacho $\rightarrow$ verificación $\rightarrow$ entrega de custodia $\rightarrow$ transporte (24 departamentos) $\rightarrow$ incidencias y logística inversa $\rightarrow$ entrega $\rightarrow$ conformidad $\rightarrow$ cierre. | Mermaid nativo + Código PlantUML con 5 *Swimlanes* + Tabla de secuencia de sistemas corporativos. |

---

## 3. Instrucciones para Dibujar o Importar en Visual Paradigm

Para incorporar estos diagramas en el informe final en formato PDF del **APF1**:

1. Abre **Visual Paradigm**.
2. Selecciona **Diagram** $\rightarrow$ **New** $\rightarrow$ **Activity Diagram**.
3. En la barra de herramientas lateral, selecciona la herramienta **Swimlane** (horizontal o vertical).
4. Crea los carriles según la tabla de particiones documentada en cada archivo:
   - **Para el problema crítico (6 carriles):** *Supervisor CD (Lurín)*, *Conductor / Socio Logístico*, *Punto de Destino / Receptor Autorizado*, *Sistema de Tracking y Consulta*, *Solicitante / Destinatario*, *Servicio al Cliente (SAC)*.
   - **Para el flujo general (5 carriles):** *Centro de Distribución (Lurín)*, *Supervisor de Despacho*, *Conductor / Socio Logístico*, *Punto de Destino / Receptor*, *Administración y Cierre Operativo*.
5. Dibuja las actividades (rectángulos redondeados), decisiones (rombos) y transiciones.
6. Alternativamente, si cuentas con el plugin o visualizador de PlantUML, copia directamente el bloque `@startuml ... @enduml` incluido en cada archivo.

---

## 4. Trazabilidad: De los Problemas AS-IS a la Propuesta TO-BE

Los diagramas AS-IS justifican la necesidad del software sin mezclar ambos niveles:
- Las **demoras en conocer el avance y contingencias en ruta** justifican que el TO-BE capture telemetría periódica con soporte offline.
- El **desfase promedio de 2 horas en el tracking actual** justifica que el TO-BE publique eventos al Bus corporativo en un tiempo $\le$ 30 minutos (**RF029**).
- El **proceso de incidencias y logística inversa** demuestra la importancia de contar con un canal estandarizado de reportes tipificados.
- La **falta de confirmación inmediata en destino** justifica incorporar confirmación consciente con evidencias fotográficas.
- La **dificultad para responder con certeza en SAC** justifica un buscador indexado de timeline histórico que recupere la traza en $<$ 2 segundos.
