# 1. TO-BE 01 — Preparación, Habilitación y Activación

Este diagrama modela el proceso de control previo en andén, emisión del Código Único de Activación efímero y vinculación de la sesión operativa móvil en la App Nativa Android (soporte a **CUN-01**).

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
:Sistemas corporativos ponen a disposición
despacho preparado en andén
[DISPONIBLE PARA SEGUIMIENTO];

|#EFF6FF|Supervisor de Distribución (Web)|
:Consultar despachos preparados en andén [RF007];
:Verificar destino, bultos y transportista asignado;
:Solicitar habilitación de seguimiento en Web [RF007, RF008];

if (¿Despacho detenido por cancelación externa previa a salida?) then (sí)

    |#FEF3F7|Plataforma Y-Trace (Backend)|
    :Procesar procedimiento corporativo externo de cancelación;
    :Invalidar código efímero o sesión pendiente [RF028];
    :Registrar estado correspondiente en el sistema;

    stop

else (no)

    |#FEF3F7|Plataforma Y-Trace (Backend)|
    :Validar elegibilidad del despacho [RF008];
    :Registrar hito interno de control previo;
    :Generar Código Único de Activación (8 caracteres) [RF008];
    :Asociar código al despacho;
    :Transicionar despacho a HABILITADO [RF008];

    |#EFF6FF|Supervisor de Distribución (Web)|
    :Visualizar código en pantalla y comunicarlo
    al conductor junto con la guía física;

endif

|#F1F5F9|Conductor (App Nativa Android)|

repeat

    :Abrir App Nativa Android e ingresar
    Código Único de Activación recibido [RF010];

    |#FEF3F7|Plataforma Y-Trace (Backend)|

    if (¿Código válido y vigente?) then (sí)

        :Validar código y emitir sesión operativa temporal [RF010];

        |#F1F5F9|Conductor (App Nativa Android)|
        :Recibir sesión operativa y descargar
        información del despacho y destino [RF010, RF011, RF012];

    else (no)

        :Rechazar activación e incrementar
        contador de intentos fallidos [RF027];

        if (¿Alcanza 5 intentos fallidos consecutivos?) then (sí)

            :Invalidar y bloquear definitivamente el código [RF027];

            |#EFF6FF|Supervisor de Distribución (Web)|
            :Verificar presencialmente la identidad
            del conductor en andén;

            if (¿Corresponde generar nueva habilitación?) then (sí)

                :Solicitar nueva habilitación mediante
                el procedimiento operativo establecido [RF008];

                |#FEF3F7|Plataforma Y-Trace (Backend)|
                :Generar nuevo Código Único de Activación [RF008];

                |#EFF6FF|Supervisor de Distribución (Web)|
                :Comunicar nuevo código al conductor;

                |#F1F5F9|Conductor (App Nativa Android)|
                :Reintentar activación con nuevo código;

            else (no)

                :Resolver situación mediante proceso
                operativo externo del CD o transportista;
                stop

            endif

        else (no)

            |#F1F5F9|Conductor (App Nativa Android)|
            :Mostrar error de código en pantalla;
            :Permitir nuevo intento;

        endif

    endif

repeat while (¿Sesión operativa establecida?) is (no) not (sí)

:Mostrar sesión operativa establecida
y datos operativos de ruta [RF012];

stop

@enduml
```

---

# 2. TO-BE 02 — Salida, Traslado, Monitoreo y Contingencias

Este diagrama modela el inicio de viaje (`EN_RUTA`), la telemetría periódica GPS en segundo plano, la persistencia offline (SQLite Room), la supervisión en grilla web y la resolución de contingencias viales comunicadas por telefonía externa, incluyendo la Cancelación Forzada del Seguimiento (`RF009`) cuando el viaje no puede continuar (soporte a **CUN-02** y **CUN-03**).

```plantuml
@startuml TO_BE_02_Traslado_Monitoreo_Contingencias

title TO-BE 02 — Salida, Traslado, Monitoreo y Contingencias (Y-Trace)

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

|#F1F5F9|Conductor (App Nativa Android)|
start
:Verificar precintos de seguridad y estiba;
:Presionar "Iniciar Despacho" en App Móvil [RF013];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Registrar hito formal de salida física con GPS y hora [RF013];
:Transicionar estado del despacho a EN_RUTA [RF013];
:Activar captura periódica de telemetría GPS [RF014];
:Publicar evento EN_RUTA al Bus corporativo
(SLA de integración <= 30 min) [RF025, RNF006];

|#F1F5F9|Conductor (App Nativa Android)|

repeat

    :Avanzar en recorrido por la red vial nacional;

    :Capturar muestreo GPS en segundo plano
    a intervalos de 10 minutos [RF014, RNF008];

    if (¿Dispone de cobertura celular?) then (no)

        :Almacenar coordenadas y eventos localmente
        en SQLite (Room) [RF019];
        :Mostrar indicador de conectividad y contador
        de cola pendiente en pantalla [RF021];

    else (sí)

        :Transmitir lote de telemetría retenida
        en orden cronológico estricto (FIFO) [RF020];

    endif

    |#EFF6FF|Supervisor de Distribución (Web)|
    :Visualizar último avance y estado de la unidad
    en grilla operativa interactiva [RF022];

    note right
        Verde = EN_RUTA
        La grilla interna muestra última
        posición, tiempo y placa.
    end note

    |#F1F5F9|Conductor (App Nativa Android)|

    if (¿Ocurre contingencia vial o siniestro en ruta?) then (sí)

        :Detener unidad en zona segura de la berma;
        :Comunicar contingencia vía telefónica
        a la Torre de Control (fuera de Y-Trace);

        |#EFF6FF|Supervisor de Distribución (Web)|
        :Recibir llamada telefónica y evaluar
        la gravedad de la situación en carretera;

        if (¿Contingencia insalvable o imposibilidad de continuar viaje?) then (sí)

            :Ejecutar Cancelación Forzada del Seguimiento [RF009];
            :Seleccionar causal tipificada y registrar
            justificación en bitácora inmutable [RF006];

            |#FEF3F7|Plataforma Y-Trace (Backend)|
            :Registrar evento y causal en bitácora [RF006];
            :Transicionar despacho a DESPACHO_CANCELADO [RF009];
            :Revocar token de sesión móvil e invalidar
            definitivamente el Código de Activación [RF028];
            :Cesar inmediatamente captura y transmisión GPS [RF009];
            :Publicar evento DESPACHO_CANCELADO y resumen
            consolidado al Bus corporativo [RF025, RF026];

            |#EFF6FF|Supervisor de Distribución (Web)|
            :Visualizar despacho cancelado en la grilla web [RF022];

            |#DBEAFE|Sistemas Corporativos (Bus ESB)|
            :Sincronizar cancelación con SAP / Salesforce;
            :Derivar caso a procesos corporativos externos
            de reposición, reclamos o retorno físico;

            stop

        else (contingencia vial mitigable externamente)

            |#EFF6FF|Supervisor de Distribución (Web)|
            :Coordinar auxilio vial, asistencia mecánica
            o despeje de ruta fuera de Y-Trace;

            |#F1F5F9|Conductor (App Nativa Android)|
            :Superar contingencia física externa;
            :Reanudar marcha en carretera;
            :Mantener transmisión periódica de telemetría GPS [RF014];

        endif

    else (no)

        :Continuar recorrido normal hacia destino;

    endif

repeat while (¿Ha arribado al radio del punto de destino?) is (no) not (sí)

:Fin de etapa de traslado en carretera;
:Continuar con proceso de llegada a destino;

stop

@enduml
```

---

# 3. TO-BE 03 — Llegada, Recepción y Entrega

Este diagrama modela la acreditación de arribo por geocerca (`EN_DESTINO`), la inspección física en destino y la confirmación manual consciente de `ENTREGADO` o `NO_ENTREGADO` por el conductor (soporte a **CUN-04**).

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

|#F1F5F9|Conductor (App Nativa Android)|
start
:Ingresar al radio del punto de destino;
:Detectar llegada automáticamente por geocerca [RF015];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Registrar estado EN_DESTINO con coordenadas GPS
y timestamp atómico [RF015];
:Publicar evento EN_DESTINO al Bus corporativo [RF025];

note right
    La geocerca acredita únicamente
    la llegada física del móvil.
    NO confirma la entrega de mercadería.
end note

|#ECFDF5|Punto de Destino / Agencia Receptora|
:Recibir físicamente al conductor e inspeccionar
exterior de bultos, precintos numerados y guía;

if (¿Carga recibida conforme?) then (sí)

    :Sellar y firmar comprobante físico de recepción;

    |#F1F5F9|Conductor (App Nativa Android)|
    :Presionar conscientemente botón "Confirmar Entrega" [RF016];
    :Registrar estado ENTREGADO con GPS y timestamp [RF016];

else (no)

    |#ECFDF5|Punto de Destino / Agencia Receptora|
    :Emitir dictamen formal de rechazo con causal tipificada;

    |#F1F5F9|Conductor (App Nativa Android)|
    :Presionar "Registrar No Entrega" en la App [RF017];
    :Seleccionar causal tipificada de no entrega [RF017];
    :Registrar estado NO_ENTREGADO con GPS y timestamp [RF017];

endif

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Persistir evento operativo de forma inmutable [RNF005];

if (¿Resolución ENTREGADO?) then (sí)

    :Publicar evento ENTREGADO al Bus
    (SLA de integración <= 30 min) [RF025, RNF006];

else (NO_ENTREGADO)

    :Publicar evento NO_ENTREGADO al Bus
    (SLA de integración <= 30 min) [RF025, RNF006];

endif

:Fin de etapa de recepción y entrega física;
:Continuar con proceso de cierre y finalización;

stop

@enduml
```

---

# 4. TO-BE 04 — Cierre, Integración Corporativa y Trazabilidad

Este diagrama modela la finalización formal (`FINALIZADO`), la revocación de credenciales móviles, la transmisión del resumen consolidado al Bus corporativo y la explotación de trazabilidad por el **Jefe de Distribución** (KPIs/Excel) y el **Operador SAC** (consulta histórica) (soporte a **CUN-05**).

```plantuml
@startuml TO_BE_04_Cierre_Integracion_KPIs

title TO-BE 04 — Cierre, Integración Corporativa y Trazabilidad (Y-Trace)

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

|#F1F5F9|Conductor (App Nativa Android)|
start
:Verificar resolución registrada (ENTREGADO o NO_ENTREGADO);
:Verificar que la cola local esté vacía y sincronizada [RF020];
:Presionar conscientemente botón "Finalizar Despacho" [RF018];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Validar resolución final registrada [RF018];
:Transicionar estado del despacho a FINALIZADO [RF018];
:Revocar inmediatamente token de sesión móvil [RF028];
:Extinguir Código Único de Activación efímero [RF028];

|#F1F5F9|Conductor (App Nativa Android)|
:Purgar datos temporales del viaje de la memoria local [RF018];
:Cesar definitivamente captura y transmisión de GPS [RF018, RNF001];

|#FEF3F7|Plataforma Y-Trace (Backend)|
:Transmitir resumen consolidado de trazabilidad al Bus [RF026];

|#DBEAFE|Sistemas Corporativos (Bus ESB / SAP / Salesforce)|
:Sincronizar hitos finales y resumen consolidado con SAP y Salesforce;

if (¿Despacho concluido como NO_ENTREGADO?) then (sí)

    :Derivar caso al proceso corporativo externo
    de logística inversa si se dispone retorno;

else (ENTREGADO)

    :Mantener registro de entrega conforme
    para conciliación de fletes en SAP;

endif

' ============================================================
' EXPLOTACIÓN DE TRAZABILIDAD Y RENDIMIENTO (CUN-05)
' ============================================================

fork

    |#F5F3FF|Jefe de Distribución (Dashboard Web / KPIs)|

    :Acceder a Dashboard Ejecutivo en plataforma Web;

    :Filtrar indicadores según empresa transportista,
    departamento y rango de fechas [RF024];

    :Consultar métricas de Lead Time, tasa de puntualidad,
    entregas conformes y latencia del Bus [RF024];

    :Analizar métrica separada de despachos cancelados [RF024];

    :Exportar reporte consolidado oficial
    exclusivamente en formato Excel (.xlsx) [RF024];

fork again

    |#FFF1F2|Operador SAC / Soporte Logístico (Web Y-Trace)|

    :Recibir requerimiento o consulta operativa de agencia;

    :Acceder al buscador de trazabilidad en plataforma Web [RF023];

    :Ingresar código de despacho o placa vehicular;

    :Consultar cronología histórica completa (*Timeline*)
    en menos de 2.0 segundos [RF023, RNF015];

    :Verificar estados, marcas de tiempo y coordenadas GPS
    para brindar respuesta inmediata y fidedigna [RF023];

end fork

stop

@enduml
```

---

## 5. Articulación Secuencial y de Control entre Procesos TO-BE

El siguiente esquema ilustra la relación operativa entre los cuatro diagramas, reflejando fielmente la bifurcación hacia `DESPACHO_CANCELADO` ante contingencias insalvables en ruta sin pasar obligatoriamente por entrega:

```text
                    TO-BE 01
      Preparación / Habilitación / Activación
                        │
                        ▼
           SESIÓN OPERATIVA ESTABLECIDA
                        │
                        ▼
                    TO-BE 02
  Salida / Traslado / Monitoreo / Contingencias
                        │
       ┌────────────────┴────────────────────────┐
       │ (Traslado exitoso)                      │ (Contingencia insalvable)
       ▼                                         ▼
   EN_DESTINO                           Cancelación Forzada (RF009)
       │                                         │
       ▼                                         ▼
   TO-BE 03                             DESPACHO_CANCELADO
Llegada / Recepción / Entrega             (Revocación de sesión,
       │                                  cese GPS y resumen al Bus)
       ▼
ENTREGADO o NO_ENTREGADO
       │
       ▼
   TO-BE 04
Cierre / Integración Corporativa
       │
       ├─────────────────────────────────────────┐
       ▼                                         ▼
Jefe de Distribución                       Operador SAC
(Dashboard KPIs / Excel)            (Consulta Histórica < 2 s)
       │
       ▼
   FINALIZADO
```
