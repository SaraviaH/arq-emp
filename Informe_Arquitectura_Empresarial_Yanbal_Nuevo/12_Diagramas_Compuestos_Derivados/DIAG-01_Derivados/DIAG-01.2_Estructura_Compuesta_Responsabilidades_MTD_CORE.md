# `DIAG-01.2` — Descomposición Funcional Interna de Responsabilidades Canónicas del `MTD` (`MTD-R01` a `MTD-R06`)

- **ID del Diagrama Derivado:** `DIAG-01.2`
- **Nombre del Diagrama Derivado:** Descomposición Funcional Interna de Responsabilidades Canónicas del `MTD` (`MTD-R01` a `MTD-R06`)
- **Tipo de Diagrama:** **Diagrama de Estructura Compuesta / Flujo Interno de Capacidad (OMG UML 2.5.1 — Categoría Estructural)**
- **Diagrama General del que Procede:** `DIAG-01` — *Diagrama Conceptual de la Visión de la Solución Propuesta (Capacidad `MTD`)* ([`02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md` — Apartado 2.7.5](../../02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md#275-diagrama-conceptual-de-la-visión-de-la-solución-to-be-diag-01))
- **Elementos Originales Incluidos:** Subgrafo `S2`, subgrafo anidado `MTD_CORE`, subgrafo de salida `S3`; nodos `B1`, `C1`, `C2`, `C3`, `C4`, `C5`, `C6`, `D1`.

```mermaid
flowchart TD
    subgraph S2["2. CAPACIDAD ARQUITECTÓNICA PROPUESTA (MTD - DEC-02)"]
        B1["Mecanismo de Trazabilidad del Despacho (MTD)<br>(Capacidad propuesta, no app independiente aprobada)"]

        subgraph MTD_CORE["Responsabilidades Canónicas Propuestas (MTD-R01 a MTD-R06)"]
            C1["MTD-R01 Captura Estandarizada"] --> C2["MTD-R02 Homologación 5 Estados"]
            C2 --> C3["MTD-R03 Trazabilidad UA - Picking - Pedido<br>(Cardinalidad pendiente ASM-08/11)"]
            C3 --> C4["MTD-R04 Marcas T0/T1 y Medición (<= 30 min)"]
            C4 --> C5["MTD-R05 Propagación y Disponibilidad Consulta"]
            C4 --> C6["MTD-R06 Trazabilidad Retorno y Notificación"]
        end
        B1 --- MTD_CORE
    end

    subgraph S3["3. INTEROPERABILIDAD DESACOPLADA"]
        C5 -->|"Eventos de Estado Correlacionados"| D1["Bus de Integración<br>(SIS-05, ASM-08)"]
        C6 -->|"Evento de Incidencia / Retorno"| D1
    end

    classDef mtd fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef bus fill:#fff3e0,stroke:#f57c00,stroke-width:2px;

    class B1,C1,C2,C3,C4,C5,C6 mtd;
    class D1 bus;
```

