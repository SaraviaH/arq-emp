# Diagrama de Actividades TO-BE: Resolución del Problema Crítico de Trazabilidad

> **Ubicación:** `diagram de actividades to-be/diagrama critico tobe/diagrama critico tobe.md`  
> **Curso:** Diseño e Implementación de Arquitectura Empresarial (UTP) — Entregable APF1 (Semana 6 - 7)  
> **Proyecto:** Sistema Web y Móvil para la Gestión y Trazabilidad de Despachos y Entregas de Yanbal Perú (Y-Trace)  
> **Enfoque:** Resolución directa del desfase crítico de ~2 horas (problemas PR-03, PR-04, PR-05) y soporte ágil a SAC mediante los requerimientos oficiales.  
> **Estándar:** UML 2.5 / Metodología RUP con Particiones (*Swimlanes*) oficiales.

---

```plantuml
@startuml Diagrama_Actividades_TO_BE_Yanbal_Problema_Critico

title TO-BE — Resolución del Problema Crítico de Trazabilidad (Y-Trace)

skinparam shadowing false
skinparam defaultFontName Arial
skinparam defaultFontSize 11

skinparam activity {
    BackgroundColor #F8FAFC
    BorderColor #1E293B
    BorderThickness 1.5
    FontSize 11
}

skinparam partition {
    BackgroundColor #FFFFFF
    BorderColor #475569
    FontColor #0F172A
    FontStyle bold
    FontSize 12
}

' ============================================================
' 1. LLEGADA Y REGISTRO DE ENTREGA (CUN-04)
' ============================================================

|#EFF6FF|Conductor (Socio Logístico / App Nativa)|

start
:Arribar al radio del punto de destino;
:Detectar llegada automáticamente
por geocerca [RF015];

|#FEF3F7|Plataforma Y-Trace (Backend)|

:Registrar evento EN_DESTINO con
GPS y timestamp atómico [RF015];
:Publicar hito EN_DESTINO al Bus [RF025];

|#ECFDF5|Punto de Destino / Agencia Receptora|

:Inspeccionar integridad exterior de la carga;
:Cotejar bultos, precintos numerados y guía física;

if (¿Carga recibida conforme?) then (sí)

    :Sellar y firmar comprobante físico de recepción;

    |#EFF6FF|Conductor (Socio Logístico / App Nativa)|

    :Confirmar conscientemente ENTREGADO [RF016];
    :Registrar coordenadas GPS y timestamp [RF016];

else (no)

    :Emitir dictamen formal de rechazo con causal tipificada;

    |#EFF6FF|Conductor (Socio Logístico / App Nativa)|

    :Seleccionar motivo tipificado de rechazo;
    :Registrar NO_ENTREGADO con GPS y timestamp [RF017];

endif

' ============================================================
' 2. PERSISTENCIA OFFLINE Y SINCRONIZACIÓN RESILIENTE
' ============================================================

|#EFF6FF|Conductor (Socio Logístico / App Nativa)|

:Persistir evento de entrega localmente
en SQLite (Room) [RF019];
:Transmitir evento en orden cronológico estricto (FIFO)
al detectar conectividad celular [RF020];

|#FEF3F7|Plataforma Y-Trace (Backend)|

:Validar token de sesión y procesar evento
con idempotencia [RNF013];
:Persistir evento de forma inmutable [RNF005];
:Generar payload JSON canónico del evento [RNF017];

' ============================================================
' 3. INTEGRACIÓN CORPORATIVA (RESOLUCIÓN DEL DESFASE CRÍTICO)
' ============================================================

|#DBEAFE|Bus Corporativo / Sistemas Empresariales|

:Recibir y aceptar evento dentro del SLA
de integración <= 30 minutos [RF025, RNF006];

:Actualizar hito de transporte en
los sistemas corporativos (SAP / Salesforce);

:Actualizar estatus de entrega para
visibilidad y consulta comercial;

note right
Objetivo del Proyecto:
Reducir el desfase crítico de
~2 horas a un máximo de 30 minutos
entre la recepción en Y-Trace y su
aceptación formal en el Bus [RNF006].
end note

' ============================================================
' 4. CONSULTA OPERACIONAL Y SOPORTE INMEDIATO SAC (CUN-05)
' ============================================================

|#F5F3FF|Operador SAC / Soporte Logístico (Web Y-Trace)|

:Recibir consulta operacional o requerimiento
de información de agencia comercial;

:Acceder a Consulta de Trazabilidad y Resumen [RF023];
:Ingresar código de despacho o placa vehicular;

:Desplegar línea de tiempo completa (*Timeline*)
en menos de 2.0 segundos [RF023, RNF015];

:Verificar cronología, estados, coordenadas GPS,
marcas de tiempo y causales tipificadas [RF023];

:Proporcionar respuesta inmediata y fundamentada
al solicitante con sustento en datos certificados;

stop

@enduml
```

---

## 2. Coherencia con el Alcance y Requerimientos

1. **Alineación de Particiones y Actores:** Utiliza exclusivamente los actores del negocio participantes (`Conductor`, `Punto de Destino / Agencia Receptora`, `Operador SAC / Soporte Logístico`) y las particiones de soporte para los sistemas y componentes tecnológicos (`Plataforma Y-Trace (Backend)` y `Bus Corporativo / Sistemas Empresariales`), eliminando la partición no oficial de *"Solicitante / Destinatario"* y preservando la concordancia con los actores oficiales definidos en el modelo CUN.
2. **Citas RNF Sincronizadas:**
   - **`RNF006`:** Latencia máxima de integración al Bus ($\le$ 30 minutos), resolviendo la brecha de 2 horas.
   - **`RNF017`:** Estandarización de formato de intercambio en JSON Canónico.
   - **`RNF015`:** Tiempo de respuesta en consulta web de trazabilidad ($< 2.0$ segundos).
   - **`RNF005`:** Inmutabilidad absoluta en la base de datos de eventos de despacho.
   - **`RNF013`:** Procesamiento idempotente de eventos en backend.
3. **Frontera B2B Respetada:** Conductor realiza la confirmación manual consciente en la app móvil; Punto de Destino efectúa la inspección física y firma del documento físico, sin fotografías ni POD multimedia.
