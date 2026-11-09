# Índice General del Informe de Arquitectura Empresarial (Entregable 2 - TOGAF ADM)
Proyecto: Arquitectura de Integración para la Trazabilidad y Distribución en Yanbal Perú

# 1. Fase Preliminar [Conservar de V1 - Versión Ejecutiva Compacta]
- 1.1. Alcance y contexto de la arquitectura organizacional (Unique S.A. / Yanbal)
- 1.2. Principios de arquitectura empresarial (Alineación con interoperabilidad e integración)
- 1.3. Roles y responsabilidades de los involucrados en la cadena logística
- 1.4. Capacidades y recursos para el desarrollo de la arquitectura (Activos y sistemas)
- 1.5. Gestión de requerimientos de arquitectura (Reorientados a capacidades de integración)

# 2. FASE A: VISIÓN DE ARQUITECTURA [Conservar y Reenfocar a Solución Integrada]
- 2.1. Visión y objetivos estratégicos del proyecto
- 2.2. Alcance, limitaciones y restricciones (In-Scope de distribución vs Out-of-Scope)
- 2.3. Metas de negocio y objetivos SMART (Reducción de latencia de 2h a <= 30 min)
- 2.4. Factores críticos de éxito (FCE)
- 2.5. Indicadores clave de rendimiento (KPIs estratégicos)
- 2.6. Actores interesados y sus necesidades (Stakeholder Map Matrix)
- 2.7. Visión de la solución propuesta (Concepto de solución integrada en el ecosistema Yanbal) [Ajuste de Enfoque]
- 2.8. Entregables y cronograma de desarrollo de la arquitectura

# 3. FASE B: ARQUITECTURA DE NEGOCIO [Conservar Base y Desarrollar Núcleo TO-BE]
- 3.1. Objetivos y capacidades del negocio (Mapa de Capacidades de Distribución)
- 3.2. Roles y responsabilidades en el proceso de distribución
- 3.3. Modelado del proceso actual (AS-IS) (Línea base resumida: CUN-01..05 y cuellos de botella)
- 3.4. Diseño del proceso optimizado (TO-BE) (Distribución con trazabilidad integrada) [A Desarrollar]
- 3.5. Diagrama de actividades del proceso propuesto (UML TO-BE con swimlanes) [Obligatorio APF2]
- 3.6. Diagrama de secuencia del proceso central (Interacción actores y sistemas) [Obligatorio APF2]
- 3.7. Servicios y funciones del negocio (Alineación a capacidades del negocio) [Ajuste Conceptual]
- 3.8. Análisis de brechas de la arquitectura de negocio (Gap Analysis de Procesos) [A Desarrollar]

# 4. FASE C: ARQUITECTURA DE SISTEMAS DE INFORMACIÓN [Reestructuración Metodológica Crítica]
- 4.1. Arquitectura de datos
  - 4.1.1. Identificación de entidades y objetos de negocio de distribución
  - 4.1.2. Modelo lógico de datos corporativo (Modelo conceptual/lógico de dominio)
  - 4.1.3. Diccionario de datos y matriz de relaciones principales
  - 4.1.4. Gestión, calidad, integridad y seguridad de los datos
  - 4.1.5. Estrategia de intercambio y sincronización de datos vía Bus Corporativo [Ajuste de Enfoque]
- 4.2. Arquitectura de aplicaciones [Reorientación hacia Ecosistema Integrado]
  - 4.2.1. Inventario de aplicaciones actuales de Yanbal (SAP R3, SPY, Maya, DRIVING, NSDG, etc.)
  - 4.2.2. Componentes y servicios de aplicación propuestos (Servicios de Trazabilidad e Integración) [Reemplaza CUS/BCE]
  - 4.2.3. Diagrama de componentes UML (Orquestación Bus, adaptadores y sistemas legados) [Obligatorio APF2]
  - 4.2.4. Integración e interoperabilidad entre aplicaciones (Patrones SOA / Eventos / APIs) [Núcleo de la Solución]
  - 4.2.5. Matriz de trazabilidad entre requerimientos de negocio y componentes de software

# 5. FASE D: ARQUITECTURA TECNOLÓGICA [Conservar Estructura y Desarrollar Despliegue Híbrido]
- 5.1. Infraestructura tecnológica actual (Sistemas on-premise Lurín, terminales RF, redes móviles)
- 5.2. Infraestructura tecnológica propuesta (Plataforma híbrida del Bus de Integración y API Gateway) [A Desarrollar]
- 5.3. Componentes de hardware, software de base, red y comunicaciones
- 5.4. Servicios de infraestructura, middleware y esquemas de alojamiento (Híbrido / Cloud) [A Desarrollar]
- 5.5. Seguridad técnica, cifrado (TLS 1.3), disponibilidad y rendimiento
- 5.6. Diagrama de despliegue UML (Topología física: Nube, Servidores Corporativos, Nodos Móviles) [Obligatorio APF2]
- 5.7. Justificación técnica y económica de las tecnologías propuestas [A Desarrollar]

# 6. FASE E: OPORTUNIDADES Y SOLUCIONES [Conservar Estructura y Desarrollar Matriz de Brechas]
- 6.1. Identificación de oportunidades de mejora a partir del diagnóstico
- 6.2. Matriz formal de análisis de brechas consolidada (Gap Analysis B, C y D) [Obligatorio APF2]
- 6.3. Identificación y definición de paquetes de trabajo (Work Packages) [A Desarrollar]
- 6.4. Evaluación de alternativas de solución (Sustentación de Solución Integrada vs Sistema Aislado) [A Desarrollar]
- 6.5. Solución arquitectónica seleccionada (Arquitectura de Integración Orientada a Eventos y Servicios)
- 6.6. Beneficios esperados para Yanbal (Métricas de impacto cualitativo y cuantitativo)

# 7. FASE F: PLANIFICACIÓN DE LA MIGRACIÓN [Conservar Estructura y Desarrollar Roadmap]
- 7.1. Estrategia de transición y definición de Arquitecturas de Transición (Transición 1 y 2) [A Desarrollar]
- 7.2. Priorización multicriterio de proyectos (Costo-Beneficio-Riesgo) [A Desarrollar]
- 7.3. Roadmap estratégico de implementación temporalizado [Obligatorio APF2]
- 7.4. Cronograma estimado de actividades por fases
- 7.5. Asignación de recursos, responsabilidades y gestión de dependencias
- 7.6. Matriz de riesgos de transformación empresarial y planes de mitigación [A Desarrollar]
- 7.7. Plan de validación, pruebas de carga y sincronización de datos maestros

# 8. FASE G: GOBIERNO DE LA IMPLEMENTACIÓN [Conservar Estructura y Desarrollar Contrato de Arq.]
- 8.1. Modelo de gobernanza de arquitectura y estructura del Architecture Board de Yanbal [A Desarrollar]
- 8.2. Roles, responsabilidades y matriz RACI de supervisión
- 8.3. Estándares técnicos y Contrato de Arquitectura (Cláusulas obligatorias de diseño) [Obligatorio APF2]
- 8.4. Revisiones y listas de chequeo de cumplimiento arquitectónico (Compliance Reviews) [A Desarrollar]
- 8.5. Gestión de calidad, validación por seguridad informática corporativa y pruebas de estrés
- 8.6. Procedimiento para el tratamiento de no conformidades, dispensas y desviaciones técnicas

# 9. FASE H: GESTIÓN DEL CAMBIO DE ARQUITECTURA [Conservar Estructura y Desarrollar Procedimiento RFC]
- 9.1. Procedimiento formal para la gestión de solicitudes de cambio (RFC) [Obligatorio APF2]
- 9.2. Monitoreo del desempeño y cumplimiento del SLA de tracking (<= 30 min)
- 9.3. Criterios para la gestión de cambios Tipo 1 (Mantenimiento y parches operativos)
- 9.4. Criterios de evaluación para cambios Tipo 2 (Incrementales) y Tipo 3 (Re-arquitectura)
- 9.5. Estrategia de mejora continua y evolución tecnológica corporativa

# ANEXOS TÉCNICOS [NUEVO - Reubicación Estratégica de Activos V1]
- Anexo A: Especificación de Casos de Uso del Sistema (CUS-01 a CUS-12) [Heredado V1]
- Anexo B: Modelos de Clases de Análisis (BCE) y Clases de Diseño [Heredado V1]
- Anexo C: Glosario de Términos Técnicos y Trazabilidad de Entrevistas