# 1. TO-BE — Preparación, habilitación y activación

```plantuml
@startuml TO_BE_01_Preparacion_Habilitacion_Activacion

title TO-BE 01 — Preparación, Habilitación y Activación de Despacho (Y-Trace)

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

|#DBEAFE|Sistemas Corporativos (SPY / Driving)|
start
:Sistemas externos reportan despacho preparado y disponible
[DISPONIBLE PARA SEGUIMIENTO];

|#EFF6FF|Supervisor CD Lurín (Web)|
:Consultar despachos disponibles, filtrar y seleccionar despacho [RF007];
:Revisar destino, vehículo y transportista;
:Solicitar "Habilitar seguimiento" [RF007, RF008];

if (¿Despacho cancelado externamente antes\nde activar el seguimiento?) then (sí)

    |#FEF3F7|Plataforma Y-Trace (Backend)|
    :Registrar evento DESPACHO_CANCELADO [RF029];
    :Invalidar código o sesión pendiente [RF033];

    |#DBEAFE|Sistemas Corporativos (Bus ESB)|
    :Publicar evento DESPACHO_CANCELADO al Bus [RF029];

    stop

else (no)

    |#FEF3F7|Plataforma Y-Trace (Backend)|
    :Validar elegibilidad del despacho [RF008];
    :Generar Código Único de Activación de 8 caracteres [RF008];
    :Asociar código al despacho;
    :Transicionar despacho a HABILITADO [RF008];

    |#EFF6FF|Supervisor CD Lurín (Web)|
    :Recibir código y comunicarlo al conductor en andén;

endif


|#F1F5F9|Conductor (PWA Android)|

repeat

    :Abrir PWA e ingresar Código de Activación [RF010];

    |#FEF3F7|Plataforma Y-Trace (Backend)|

    if (¿Código válido y vigente?) then (sí)

        :Validar código y emitir sesión operativa [RF011];

        |#F1F5F9|Conductor (PWA Android)|
        :Recibir sesión operativa y descargar hoja de ruta [RF011, RF012];

    else (no)

        :Recibir rechazo de activación;

        if (¿Alcanza 5 intentos fallidos consecutivos?) then (sí)

            :Invalidar y bloquear código [RF032];

            |#EFF6FF|Supervisor CD Lurín (Web)|
            :Recibir alerta de bloqueo;
            :Verificar presencialmente al conductor;
            :Solicitar generación de nuevo código [RF008];

            |#FEF3F7|Plataforma Y-Trace (Backend)|
            :Generar nuevo Código Único de Activación [RF008];

            |#EFF6FF|Supervisor CD Lurín (Web)|
            :Comunicar nuevo código al conductor;

            |#F1F5F9|Conductor (PWA Android)|
            :Reintentar activación con nuevo código;

        else (no)

            |#F1F5F9|Conductor (PWA Android)|
            :Mostrar error de código;
            :Permitir nuevo intento;

        endif

    endif

repeat while (¿Sesión operativa establecida?) is (no) not (sí)

:Mostrar sesión operativa establecida;

stop

@enduml
```

# 2. TO-BE — Salida, traslado, monitoreo e incidencias

Este recoge el bloque más grande del diagrama original: EN_RUTA, GPS, offline, monitoreo e incidencias.

```plantuml
@startuml TO_BE_02_Traslado_Monitoreo_Incidencias

title TO-BE 02 — Salida, Traslado, Monitoreo e Incidencias (Y-Trace)

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

|#F1F5F9|Conductor (PWA Android)|
start
:Verificar precintos de seguridad;
:Presionar "Iniciar Despacho" [RF013];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Transicionar estado a EN_RUTA [RF013];
:Iniciar telemetría GPS periódica [RF014];
:Publicar evento EN_RUTA al Bus\n(SLA de integración <= 30 min) [RF029];

|#F1F5F9|Conductor (PWA Android)|

repeat

    :Avanzar en recorrido hacia destino;

    :Capturar muestreo GPS en segundo plano cada 10 min [RF014];

    if (¿Dispone de cobertura celular?) then (no)

        :Guardar coordenadas y eventos en IndexedDB [RF021];
        :Mostrar contador de cola pendiente [RF023];

    else (sí)

        :Transmitir lote de telemetría al servidor central [RF022];

    endif

    |#EFF6FF|Supervisor CD Lurín (Web)|
    :Visualizar último avance disponible\nen grilla operativa [RF024];

    note right
        Verde = EN_RUTA
        La grilla muestra el último dato
        recibido por la plataforma.
    end note

    |#F1F5F9|Conductor (PWA Android)|

    if (¿Ocurre anomalía vial o siniestro en ruta?) then (sí)

        :Detener unidad en zona segura;
        :Registrar incidencia con GPS [RF019];

        if (¿Condiciones permiten fotografía?) then (sí)
            :Adjuntar fotografía complementaria opcional [RF018];
        else (no)
            :Continuar sin fotografía;
        endif

        :Enviar alerta prioritaria de incidencia [RF019];

        |#FEF3F7|Plataforma Y-Trace (Backend)|
        :Transicionar despacho a CON_INCIDENCIA [RF019];

        |#EFF6FF|Supervisor CD Lurín (Web)|
        :Mostrar alarma sonora y visual\n(Rojo: CON_INCIDENCIA) [RF026];

        |#FEF3F7|Plataforma Y-Trace (Backend)|
        :Publicar notificación de contingencia al Bus\n[RF030];

        |#EFF6FF|Supervisor CD Lurín (Web)|

        if (¿Contingencia impide continuar el viaje?) then (sí)

            :Ejecutar Cierre Forzado con justificación [RF009];

            |#FEF3F7|Plataforma Y-Trace (Backend)|
            :Transicionar despacho a FINALIZADO [RF009];
            :Revocar sesión móvil activa y código [RF033];
            :Transmitir resumen consolidado del despacho [RF031];

            |#DBEAFE|Sistemas Corporativos (Bus ESB)|
            :Derivar caso al proceso corporativo externo\nde logística inversa y reposición;

            stop

        else (no)

            if (¿Falla de smartphone Android?) then (sí)

                |#F1F5F9|Conductor (PWA Android)|
                :Contactar al Supervisor por canal de auxilio;

                |#EFF6FF|Supervisor CD Lurín (Web)|
                :Validar identidad;
                :Solicitar código de recuperación [RF008];

                |#FEF3F7|Plataforma Y-Trace (Backend)|
                :Generar Código de Recuperación\nde un solo uso [RF008];

                |#F1F5F9|Conductor (PWA Android)|
                :Ingresar código en nuevo smartphone Android [RF011];

                |#FEF3F7|Plataforma Y-Trace (Backend)|
                :Revocar token anterior;
                :Asociar nuevo dispositivo al mismo despacho;
                :Conservar histórico del viaje [RF011];

            else (otra contingencia mitigable)

                |#EFF6FF|Supervisor CD Lurín (Web)|
                :Coordinar auxilio vial o atención\nde la contingencia;

                |#F1F5F9|Conductor (PWA Android)|
                :Continuar operación una vez resuelta\nla contingencia;

            endif

            |#FEF3F7|Plataforma Y-Trace (Backend)|
            :Restablecer estado a EN_RUTA;

            |#F1F5F9|Conductor (PWA Android)|
            :Reanudar ciclo de traslado y monitoreo;

        endif

    else (no)

        :Continuar recorrido normal;

    endif

repeat while (¿Ha arribado al perímetro del punto de destino?) is (no) not (sí)

:Fin de etapa de traslado;
:Continuar con proceso de llegada a destino;

stop

@enduml
```

# 3. TO-BE — Llegada y entrega

Aquí se concentra EN_DESTINO, inspección, entrega conforme/no conforme y evidencia. El EN_DESTINO no implica entrega.

```plantuml
@startuml TO_BE_03_Llegada_Entrega

title TO-BE 03 — Llegada, Recepción y Entrega (Y-Trace)

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

|#F1F5F9|Conductor (PWA Android)|
start
:Detectar llegada por geocerca o presionar\n"Llegué a Destino" [RF015];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Registrar EN_DESTINO con GPS y timestamp [RF015];
:Publicar evento EN_DESTINO al Bus [RF029];
:Iniciar cómputo de ventana operativa de 60 minutos [RF026];

note right
La geocerca acredita únicamente
la llegada física.

No confirma la entrega.
end note

|#ECFDF5|Punto de Destino / Receptor|
:Inspeccionar bultos, precintos y guía física;

if (¿Carga recibida conforme?) then (sí)

    :Sellar y firmar comprobante de recepción;

    |#F1F5F9|Conductor (PWA Android)|
    :Presionar "Confirmar Entrega" [RF016];
    :Registrar ENTREGADO con GPS y timestamp [RF016];

else (no)

    |#ECFDF5|Punto de Destino / Receptor|
    :Emitir dictamen formal de rechazo con causal;

    |#F1F5F9|Conductor (PWA Android)|
    :Seleccionar causal;
    :Registrar NO_ENTREGADO [RF017];

endif


|#FEF3F7|Plataforma Y-Trace (Backend)|

if (¿Se alcanza la ventana de 60 minutos\nsin ENTREGADO ni NO_ENTREGADO?) then (sí)

    |#EFF6FF|Supervisor CD Lurín (Web)|
    :Recibir y atender alerta de timeout [RF026];

    |#FEF3F7|Plataforma Y-Trace (Backend)|
    :Mantener seguimiento hasta registrar\nla resolución operativa;

else (no)

    :Continuar procesamiento normal;

endif


|#F1F5F9|Conductor (PWA Android)|

if (¿Condiciones permiten capturar fotografía?) then (sí)

    :Capturar fotografía complementaria opcional [RF018];

else (no)

    :Continuar sin fotografía;

endif

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Persistir evento operativo de forma inmutable [RNF006];

if (¿Existe fotografía?) then (sí)

    :Alojar fotografía en Cloud Storage [RNF005];

else (no)

endif

if (¿Resolución ENTREGADO?) then (sí)

    :Publicar evento ENTREGADO al Bus [RF029];

else (NO_ENTREGADO)

    :Publicar evento NO_ENTREGADO al Bus [RF029];

endif

:Fin de etapa de entrega;

stop

@enduml
```

# 4. TO-BE — Cierre, integración y control

Esta parte contiene FINALIZADO, revocación, sincronización corporativa y KPIs. RF028 es el que contiene la exportación Excel; RF034 corresponde al aislamiento por empresa transportista.

```plantuml
@startuml TO_BE_04_Cierre_Integracion_KPIs

title TO-BE 04 — Cierre, Integración Corporativa y Control Gerencial (Y-Trace)

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

|#F1F5F9|Conductor (PWA Android)|
start
:Recibir resolución ENTREGADO o NO_ENTREGADO;
:Verificar que la cola local esté vacía [RF020];
:Presionar "Finalizar Despacho" [RF020];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Validar resolución final del despacho [RF020];
:Transicionar a FINALIZADO [RF020];
:Revocar inmediatamente token de sesión [RF033];
:Extinguir código de activación [RF033];

|#F1F5F9|Conductor (PWA Android)|
:Purgar datos temporales locales;
:Cesar captura y transmisión de GPS [RNF001];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Transmitir resumen consolidado de trazabilidad\nal Bus [RF031];

|#DBEAFE|Sistemas Corporativos\n(Bus ESB / SAP / Salesforce)|
:Sincronizar hitos y resumen de trazabilidad\ncon los sistemas corporativos;

if (¿Entrega NO_ENTREGADO?) then (sí)

    :Derivar el caso al proceso corporativo externo\nde logística inversa si requiere retorno;

else (ENTREGADO)

    :Mantener trazabilidad de entrega conforme;

endif


|#F5F3FF|Jefe de Distribución\n(Dashboard Web / KPIs)|

:Aplicar aislamiento de información según\nempresa transportista [RF034];

:Consultar indicadores de Lead Time,\npuntualidad y latencia [RF028];

:Evaluar desempeño de transporte;

:Exportar reporte oficial en Excel [RF028];

stop

@enduml
```

La relación entre los cuatro queda así:

```text
TO-BE 01
Preparación / Habilitación / Activación
        │
        ▼
SESIÓN OPERATIVA ESTABLECIDA
        │
        ▼
TO-BE 02
Salida / Traslado / Monitoreo / Incidencias
        │
        ▼
EN_DESTINO
        │
        ▼
TO-BE 03
Llegada / Recepción / Entrega
        │
        ▼
ENTREGADO o NO_ENTREGADO
        │
        ▼
TO-BE 04
Cierre / Integración / KPIs
        │
        ▼
FINALIZADO
```
