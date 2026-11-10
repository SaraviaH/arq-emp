# `DIAG-01.1` — Visión de Contexto e Interoperabilidad E2E de la Capacidad Propuesta `MTD`

- **ID del Diagrama Derivado:** `DIAG-01.1`
- **Nombre del Diagrama Derivado:** Visión de Contexto e Interoperabilidad E2E de la Capacidad Propuesta `MTD` (`S1`, `S2`, `S3`, `S4`)
- **Tipo de Diagrama:** **Diagrama de Estructura Compuesta / Diagrama de Paquetes y Flujo Conceptual (OMG UML 2.5.1 — Categoría Estructural)**
- **Diagrama General del que Procede:** `DIAG-01` — *Diagrama Conceptual de la Visión de la Solución Propuesta (Capacidad `MTD`)* ([`02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md` — Apartado 2.7.5](../../02_Fase_A_Vision_de_Arquitectura/02_Fase_A_Vision_Arquitectura_Completa.md#275-diagrama-conceptual-de-la-visión-de-la-solución-to-be-diag-01))
- **Elementos Originales Incluidos:** Subgrafos `S1`, `S2` (frontera de contexto), `S3` y `S4`; nodos `A1`, `A2`, `A3`, `A4`, `B1`, `B2`, `C5`, `C6`, `D1`, `E1`, `E2`, `E3`, `E4`.

```mermaid
flowchart TD
    subgraph S1["1. GENERACIÓN, ALMACÉN Y PREPARACIÓN (CD Lurín)"]
        A1["Frente Comercial (Agrupación conceptual)<br>Maya (SIS-03) -> ZapCommerce (SIS-04, ASM-08)"] -->|"Pedidos vía Bus (SIS-05)"| A2["Gestor de Picking in-house<br>SPY (SIS-02)"]
        A3["Almacén PT - ERP<br>SAP R3 (SIS-01 - Exclusivo ACT-03)"] -->|"Sincronización stock liberado /<br>Cajas Master (Código UA)"| A2
        A2 -->|"Pedido preparado (1 de 8 cajas)<br>(Número de Pedido)"| A4["Zona de Despacho (ACT-05)<br>Zonificación a 24 Departamentos"]
    end

    subgraph S2["2. CAPACIDAD ARQUITECTÓNICA PROPUESTA (MTD - DEC-02)"]
        A2 -->|"Evento: En preparación (T0, ASM-15)"| B1["Mecanismo de Trazabilidad del Despacho (MTD)<br>(Capacidad propuesta, no app independiente aprobada)"]
        A4 -->|"Evento: Pedido despachado (T0, ASM-15)"| B1
        B2["Socios Logísticos (ACT-07) y Sistemas Transporte/Trackeo<br>NSDG (SIS-06) / ENSDY (SIS-07) / DRIVING (SIS-08)<br>(Separados y sujetos a ASM-02, ASM-07, ASM-11)"] -->|"Eventos: En ruta / Entregado /<br>Incidencia-Retorno (T0)"| B1
        B1 --- C5["MTD-R05 Propagación y Disponibilidad Consulta"]
        B1 --- C6["MTD-R06 Trazabilidad Retorno y Notificación"]
    end

    subgraph S3["3. INTEROPERABILIDAD DESACOPLADA"]
        C5 -->|"Eventos de Estado Correlacionados"| D1["Bus de Integración<br>(SIS-05, ASM-08)"]
        C6 -->|"Evento de Incidencia / Retorno"| D1
    end

    subgraph S4["4. CONSULTA Y NOTIFICACIÓN A ACTORES (SIN ESCRITURA DIRECTA EN SAP R3)"]
        D1 -->|"Disponibilidad para consulta (<= 30 min)"| E1["Servicio al Cliente (ACT-08)<br>CELLFORCE (SIS-09, ASM-06)"]
        D1 -->|"Visibilidad entrega B2B (ASM-01)"| E2["Destinatarios Empresariales B2B (ACT-09)<br>(Distribuidoras y sedes en Perú)"]
        D1 -->|"Consulta y monitoreo de estados"| E3["Área de Distribución (ACT-06)<br>(Seguimiento del Despacho)"]
        D1 -.->|"Solo Notificación al Actor Responsable<br>(Sin interfaz de escritura MTD -> SAP R3, REQ-RES-002)"| E4["Área de Almacén PT (ACT-03)<br>(Recepción física y operación propia de SAP R3)"]
    end

    classDef proc fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef mtd fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef bus fill:#fff3e0,stroke:#f57c00,stroke-width:2px;
    classDef cons fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;

    class A1,A2,A3,A4,B2 proc;
    class B1,C5,C6 mtd;
    class D1 bus;
    class E1,E2,E3,E4 cons;
```

