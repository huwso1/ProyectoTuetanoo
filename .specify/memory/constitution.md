<!--
Sync Impact Report
- Version change: unspecified → 0.1.0
- Modified principles:
  - [PRINCIPLE_1_NAME] → I. Privacidad y Datos Agregados
  - [PRINCIPLE_2_NAME] → II. Simplicidad como Prioridad
  - [PRINCIPLE_3_NAME] → III. Flujo Educativo y Valor Rápido
  - [PRINCIPLE_4_NAME] → IV. Rendimiento (SLAs)
  - [PRINCIPLE_5_NAME] → V. Diseño de UX: Minimizar Clicks
  - (added) → VI. Seguridad de Datos y Cifrado
  - (added) → VII. Encapsulación de Lógica de Negocio
  - (added) → VIII. Manejo Robusto de Errores y Confianza
- Added sections: Requisitos Adicionales y Flujo de Desarrollo
- Removed sections: none
- Templates requiring updates:
  - .specify/templates/plan-template.md ⚠ pending (not found in workspace)
  - .specify/templates/spec-template.md ⚠ pending (not found in workspace)
  - .specify/templates/tasks-template.md ⚠ pending (not found in workspace)
  - .specify/templates/commands/*.md ⚠ pending (not found in workspace)
- Follow-up TODOs:
  - RATIFICATION_DATE: TODO(RATIFICATION_DATE): fecha de ratificación no encontrada en el repositorio; rellenar al ratificar.
  - Revisar y actualizar cualquier plantilla `.specify/templates/*` si aparecen más adelante.
-->

# ProyectoTuetanoo Constitution

## Core Principles

### I. Privacidad y Datos Agregados
Toda analítica debe operar exclusivamente sobre datos agregados y anonimizados. No se permitirá el uso de identificadores personales en análisis ni en reportes; cualquier excepción requiere aprobación documentada y una evaluación de riesgo. Rationale: protege la privacidad de usuarios y reduce riesgos de cumplimiento.

### II. Simplicidad como Prioridad
Se prioriza la simplicidad sobre la densidad de funciones. Las decisiones de diseño deben favorecer soluciones claras, fáciles de mantener y comprensibles por nuevos colaboradores. Evitar acumulación de funcionalidades que no demuestren valor medible.

### III. Flujo Educativo y Valor Rápido
Las experiencias y flujos deben ser educativas y demostrar valor tangible en menos de 60 segundos desde la primera interacción. El primer recorrido del usuario debe enseñar el valor principal del producto sin fricción innecesaria.

### IV. Rendimiento (SLAs)
Endpoints básicos —registro, iniciar sesión, recuperar contraseña y consultas críticas— deben retornar en menos de 2 segundos bajo condiciones normales. Se deben definir SLOs y monitoreo para evaluar cumplimiento, con alertas cuando se alcancen degradaciones.

### V. Diseño de UX: Minimizar Clicks
Cada decisión de UX/UI debe optimizar la cantidad de clicks necesarios para completar tareas clave. El objetivo es reducir la fricción y facilitar el uso: cada optimización de UI deberá medirse contra una métrica de conversión o tiempo a tarea.

### VI. Seguridad de Datos y Cifrado
Todos los datos almacenados considerados sensibles deben ser cifrados en reposo y en tránsito usando estándares reconocidos (p. ej. AES-256, TLS 1.2+). La gestión de claves, rotación y acceso debe estar documentada y controlada por políticas de acceso mínimo.

### VII. Encapsulación de Lógica de Negocio
La lógica de negocio debe residir en servidores y servicios de backend; no debe exponerse en el cliente ni en endpoints públicos sin autenticación y autorización apropiada. Detalles de implementación internos (endpoints internos, contratos privados) deben permanecer ocultos y versionados.

### VIII. Manejo Robusto de Errores y Confianza
El manejo de errores debe ser robusto y explícito: errores tipados, mensajes que no filtren información sensible y rutas de degradación. Las decisiones automatizadas que requieren confianza deberán reportar una medida de confianza y apuntar a una precisión mínima del 80% cuando aplique; si no es posible, hacerlo visible y documentar compensaciones.

## Requisitos Adicionales y Restricciones
- Analítica: exclusivamente agregada y anonimizada por defecto.
- Seguridad: cifrado para datos sensibles, gestión de llaves y auditoría de accesos.
- Rendimiento: SLOs definidos para endpoints críticos; pruebas de carga periódicas.
- Observabilidad: métricas de latencia, errores, tasas de confianza en decisiones automáticas y contadores de clicks por flujo.

## Flujo de Desarrollo y Puertas de Calidad
- Revisión de UX: cualquier cambio de flujo que afecte la cantidad de clicks requiere una revisión de usabilidad y una métrica asociada.
- Pruebas: pruebas unitarias, de integración y pruebas de rendimiento para endpoints críticos.
- Seguridad: chequeos automatizados para cifrado, exposición de datos y gestión de llaves en CI.
- Despliegue: validaciones previas en staging que incluyan latencia y pruebas de regresión de privacidad.

## Governance
La Constitución prevalece sobre prácticas locales y guías de equipo. Cualquier enmienda requiere:
- Apertura de un PR que describa el cambio, la razón y un plan de migración (si aplica).
- Aprobación por parte de al menos un mantenedor principal y un revisor de seguridad/privacidad.
- Pruebas o validaciones concretas para garantizar que la enmienda no rompe SLOs o compromete datos.

Política de versionado:
- MAJOR: Cambios incompatibles en principios (eliminación o redefinición substantiva).
- MINOR: Añadido de principios o expansión material de guías existentes.
- PATCH: Clarificaciones, correcciones de redacción o ajustes menores.

Revisiones y cumplimiento:
- Se realizará una revisión de cumplimiento (audit) trimestral enfocada en privacidad, rendimiento y seguridad.

**Version**: 0.1.0 | **Ratified**: TODO(RATIFICATION_DATE): fecha de ratificación no encontrada | **Last Amended**: 2025-10-24
