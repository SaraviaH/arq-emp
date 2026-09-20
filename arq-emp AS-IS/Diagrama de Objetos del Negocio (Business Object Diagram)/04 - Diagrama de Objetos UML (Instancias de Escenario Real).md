# Diagrama de Objetos UML — Escenario Real de Distribución (Snapshot en Tiempo $T$)

> **Proyecto:** Diseño e Implementación de Arquitectura Empresarial — Caso Yanbal Perú  
> **Área Operativa:** Cadena de Distribución y Transporte  
> **Marco Metodológico:** UML 2.5 / Estándar UTP APF1 (§ 3.3, ítem 5 y Sesión 2)  
> **Archivo de Modelado:** [[Diagrama_Objetos_Instancias_Escenario_Real.puml]]  
> **Diagrama de Clases Base:** [[01 - Diagrama de Clases de Dominio (Nivel Dominio)]] | [[Diagrama_Clases_Dominio.puml]]

---

## 1. Fundamentación Metodológica: Diferencia entre MON y Diagrama de Objetos

En el marco de la ingeniería de software y la guía rectora de la UTP, coexisten dos perspectivas de modelado que han sido desarrolladas para asegurar máxima exhaustividad:

1. **Modelo de Objetos del Negocio (MON / BOM de RUP):** Desarrollado en [[01 - Modelo de Objetos del Negocio (MON)]], describe la colaboración estructural de clases del negocio tipificadas como *«business worker»* (roles humanos) y *«business entity»* (documentos y conceptos) para realizar los CUN.
2. **Diagrama de Objetos UML (Instance Diagram):** Requerido explícitamente en el **ítem 5 del APF1** (*«Modelado de instancias concretas que ilustren el flujo en un escenario real del negocio»*) y en la **Sesión 2** (*«Representa instancias de clases capturando un estado específico en un instante de tiempo»*).

Este documento presenta el **Diagrama de Objetos UML formal**, el cual plasma una "fotografía" o captura instantánea (*snapshot*) del sistema en un momento específico de la operación, exhibiendo objetos reales con valores de datos concretos y enlaces de asociación activos.

---

## 2. Descripción del Escenario de Negocio Modelado

El escenario modelado corresponde a un caso de distribución y entrega a sede autorizada que ilustra el ciclo de vida de un despacho y la mitigación de la brecha de información de entrega:

- **Instante del Snapshot:** Viernes 18 de septiembre de 2026 a las **16:45:00 horas**.
- **Contexto Operativo:**
  1. La orden comercial consolidada asociada a la cuenta `CONS-74892` (`María Elena Torres Flores`) es preparada en un bulto de carga mono SKU (`Formato 3`).
  2. La carga egresa formalmente del Centro de Distribución de Lurín a las 08:30 horas bajo el registro de despacho `DSP-LUR-2026-0918-04`, asignado al operador **Olva Courier** bajo modalidad terrestre troncal/urbana (`Ruta Lima Centro-Sur`).
  3. Al arribar a la **Agencia de Distribución / Sede Autorizada Lima Sur** a las 16:45 horas, la carga es recibida y verificada formalmente por el **Personal Autorizado en Sede**, **Carlos Torres Flores** (DNI `45892147`, Encargado de Recepción en Sede), quien consigna la firma y conformidad en el acta física/digital.
  4. El transportista registra la confirmación y la identidad del personal autorizado en su terminal móvil, pasando el estado a **`Entregado`** y encolando la transacción para su sincronización central.

---

## 3. Catálogo de Instancias y Valores del Escenario

```mermaid
graph TD
    subgraph OBJETOS ["Instancias Concretas en Tiempo T = 16:45h"]
        CON["<b>consultora_maria : Consultora</b><br/>codigoConsultora = 'CONS-74892'<br/>nombreTitular = 'María Elena Torres Flores'<br/>telefono = '+51 987654321'"]
        PED["<b>ped_84920 : Pedido</b><br/>numeroPedido = 'PED-2026-84920'<br/>estadoPedido = 'Entregado'<br/>promesaEntrega = '24 horas'<br/>fechaDespacho = '2026-09-18 08:30'<br/>fechaEntregaEstimada = '2026-09-19'"]
        BUL["<b>bulto_01 : Bulto</b><br/>formatoCaja = 'Formato 3'<br/>estadoFisico = 'Íntegro'"]
        DES["<b>despacho_cd : RegistroDespacho</b><br/>codigoDespacho = 'DSP-LUR-2026-0918-04'<br/>fechaHoraSalida = '2026-09-18 08:30'<br/>supervisor = 'Carlos Quispe (Sup. CD)'"]
        SOC["<b>socio_olva : SocioLogistico</b><br/>ruc = '20100686810'<br/>razonSocial = 'OLVA COURIER S.A.C.'<br/>nombreComercial = 'Olva Express'"]
        MOD["<b>mod_terrestre : TransporteTerrestre</b><br/>tipoModalidad = 'Terrestre Urbano'<br/>tiempoEstimado = '24 horas'<br/>rutaTroncal = 'Ruta Lima Centro-Sur (R-14)'"]
        DIR["<b>dir_sede_destino : DireccionEntrega</b><br/>departamento = 'Lima'<br/>provincia = 'Lima'<br/>distrito = 'San Borja'<br/>tipoCiudad = 'Principal'<br/>direccion = 'Agencia de Distribución / Sede Autorizada Lima Sur'"]
        ENT["<b>reg_entrega_ok : RegistroEntrega</b><br/>codigoEntrega = 'ENT-2026-0918-992'<br/>fechaHoraEntrega = '2026-09-18 16:45:00'<br/>firmaConformidad = true"]
        REC["<b>receptor_carlos : PersonalAutorizado</b><br/>nombresReceptor = 'Carlos Torres Flores'<br/>numeroDocumento = '45892147'<br/>parentescoRelacion = 'Encargado de Recepción en Sede'"]
    end

    CON -->|emite orden| PED
    PED *-->|contiene| BUL
    PED -->|egresa vía| DES
    PED -->|asignado a| DIR
    SOC -->|traslada| PED
    PED -->|canalizado por| MOD
    PED -->|concluye en| ENT
    ENT -->|consignado por| REC
```

| Objeto (Instancia : Clase) | Atributos y Valores Asignados en el Snapshot | Rol en la Demostración del Negocio |
|:---|:---|:---|
| **`ped_84920 : Pedido`** | • `numeroPedido`: `"PED-2026-84920"`<br/>• `estadoPedido`: `"Entregado"`<br/>• `promesaEntrega`: `"24 horas"`<br/>• `fechaDespacho`: `2026-09-18 08:30:00`<br/>• `fechaEntregaEstimada`: `2026-09-19` | Instancia eje del proceso; refleja el cambio de estado a `"Entregado"` cumpliendo holgadamente el SLA de 24 horas. |
| **`bulto_01 : Bulto`** | • `formatoCaja`: `"Formato 3 (Mediana)"`<br/>• `estadoFisico`: `"Íntegro"` | Unidad física de empaque; valida la condición no deteriorada del producto al momento de la entrega en puerta. |
| **`despacho_cd : RegistroDespacho`** | • `codigoDespacho`: `"DSP-LUR-2026-0918-04"`<br/>• `fechaHoraSalida`: `2026-09-18 08:30:00`<br/>• `supervisorDespacho`: `"Carlos Quispe (Sup. CD)"`<br/>• `conformidadCarga`: `true` | Comprueba la salida formal autorizada en el muelle de Lurín y el inicio de la custodia del tercero. |
| **`socio_olva : SocioLogistico`** | • `ruc`: `"20100686810"`<br/>• `razonSocial`: `"OLVA COURIER S.A.C."`<br/>• `nombreComercial`: `"Olva Express"`<br/>• `contactoOperativo`: `"despachos.lima@olva.com.pe"` | Operador contratado que asume la responsabilidad operativa y captura la información en campo. |
| **`mod_terrestre : TransporteTerrestre`** | • `tipoModalidad`: `"Terrestre Urbano"`<br/>• `tiempoTransitoEstimado`: `"24 horas"`<br/>• `rutaTroncal`: `"Ruta Lima Centro-Sur (R-14)"` | Especialización de la modalidad; concreta el medio de transporte correspondiente a Lima Metropolitana. |
| **`dir_sede_destino : DireccionEntrega`** | • `departamento`: `"Lima"`<br/>• `provincia`: `"Lima"`<br/>• `distrito`: `"San Borja"`<br/>• `tipoCiudad`: `"Principal"`<br/>• `direccionDetallada`: `"Agencia de Distribución / Sede Autorizada Lima Sur"` | Entidad territorial que sustenta la promesa de entrega y destino formal B2B. |
| **`consultora_maria : Consultora`** | • `codigoConsultora`: `"CONS-74892"`<br/>• `nombreTitular`: `"María Elena Torres Flores"`<br/>• `telefono`: `"+51 987654321"`<br/>• `correoElectronico`: `"m.torres@gmail.com"` | Cuenta comercial/distribuidora vinculada al lote de despacho. |
| **`reg_entrega_ok : RegistroEntrega`** | • `codigoEntrega`: `"ENT-2026-0918-992"`<br/>• `fechaHoraEntrega`: `2026-09-18 16:45:00`<br/>• `firmaConformidad`: `true` | Certificación transaccional capturada en campo que alimenta la sincronización posterior. |
| **`receptor_carlos : PersonalAutorizado`** | • `nombresReceptor`: `"Carlos Torres Flores"`<br/>• `numeroDocumento`: `"45892147"`<br/>• `parentescoRelacion`: `"Encargado de Recepción en Sede"` | **Personal Autorizado en Sede:** acredita formalmente la recepción institucional de la carga en la sede de destino, consignando la conformidad. |

---

## 4. Trazabilidad frente al Diagrama de Clases de Dominio

El diagrama de objetos es una instanciación exacta de la estructura definida en el [[Diagrama_Clases_Dominio.puml]]:
1. **Conformidad de Tipos y Clases:** Cada objeto corresponde biunívocamente a una clase definida en el modelo conceptual (incluyendo las subclases concretas `TransporteTerrestre` y `PersonalAutorizado`).
2. **Multiplicidad Respetada:** 
   - La relación `Pedido (1) *-- (1..*) Bulto` se satisface con el objeto `bulto_01`.
   - La relación `RegistroEntrega (1) -- (1) Receptor` se satisface con el enlace entre `reg_entrega_ok` y `receptor_carlos`.
   - La relación `Consultora (1) -- (1..*) Pedido` se satisface con `consultora_maria` y `ped_84920`.

---
