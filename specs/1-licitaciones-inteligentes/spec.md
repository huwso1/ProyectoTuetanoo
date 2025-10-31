# Especificación: Licitaciones Inteligentes

Short name: 1-licitaciones-inteligentes
Created: 2025-10-24

## Resumen breve
Centralizar búsqueda y seguimiento de procesos contractuales (SECOP I y II) y proveer una "calificación inteligente" de compatibilidad entre empresas y pliegos (0–100%). Ofrecer herramientas para acelerar la calificación (checklists visuales, alertas de RUP expirado), colaboración (búsqueda de usuarios para uniones temporales), asistentes básicos de investigación de mercado y presupuesto, plantillas de documentos y una API para integración.

## Contexto y diferenciador
A diferencia de agregadores pasivos, la plataforma actuará proactivamente con IA para evaluación de compatibilidad, sugerencias y colaboración, posicionándose como socio accesible para MiPyme y actores gubernamentales en economía popular. Modelo freemium actualizado: acceso básico gratuito para MiPymes con búsquedas ilimitadas; funciones IA avanzadas sujetas a límites o planes pagos.

## Clarifications

### Session 2025-10-24
- Q: Política de PII y almacenamiento → A: Almacenar PII necesaria cifrada (email, teléfono, NIT, etc)
- Q: Política de retención de PII → A: Retener hasta solicitud explícita de eliminación
- Q: Política freemium (límites) → A: Sin límite de búsqueda; funciones IA limitadas para usuarios gratuitos
- Q: Objetivos de SLO para endpoints y búsquedas → A: Endpoints críticos <3s

## Alcance (qué incluye)
- Ingesta y centralización de búsquedas en SECOP I y II.
- Motor de comparación que produce una calificación de compatibilidad (0–100%), advertencias y sugerencias accionables.
- Panel de seguimiento de procesos contractuales con checklists visuales de documentos faltantes/completados.
- Chats de IA simples y plantillas de documentos reutilizables; búsqueda de usuarios para conformar uniones temporales.
- Asistente básico para investigación de mercado y estimación de presupuesto.
- API pública para consulta de calificaciones y estado de procesos.
- Esquema de almacenamiento de datos mínimo (Usuario, Empresa, Proyecto, Documentos) según boceto provisto.

## Fuera de alcance (qué no incluye en esta fase)
- Integraciones avanzadas pagas con ERPs de clientes (solo API básica en fase 1).
- Automatizaciones legales complejas o generación de propuestas 100% personalizadas.

## Actores
- MiPyme emprendedora (usuario primario, recursos limitados, experiencia baja).
- Consultor freelance (usuario secundario, análisis rápido para clientes).
- Administrador (moderación, gestión de acceso, verificación de RUP si procede).
- Sistema SECOP (fuente de datos externa).

## Resultados clave (outcomes)
- Reducir descalificaciones en procesos en un 80% (comparado con línea base) mediante checklists y alertas.
- Acelerar la calificación de horas a minutos (p. ej. reducir tiempo medio de evaluación por pliego de 4h a <10 min en la experiencia asistida).
- Aumentar wins de MiPymes en 50% vía incentivos visuales y mejora de preparación (medible por tasa de adjudicación entre usuarios activos vs baseline).

## Escenarios de usuario y pruebas (acceptance scenarios)
1. Búsqueda y calificación (MiPyme)
   - Usuario busca por UNSPSC o ubicación; sistema muestra lista de procesos y una calificación de compatibilidad (0–100%).
   - Aceptación: Al seleccionar un proceso, el sistema muestra checklist de elegibilidad y marca claramente los requisitos que faltan.

2. Análisis rápido (Consultor)
   - Consultor sube/pega partes de un pliego; sistema devuelve resumen de puntos críticos, compatibilidad estimada y sugerencias en <5 min.
   - Aceptación: Resumen incluye lista de incumplimientos y al menos 3 sugerencias accionables.

3. Seguimiento de documentos
   - Usuario agrega documentos; el panel indica qué documentos faltan y envía alertas si RUP está próximo a expirar.
   - Aceptación: El checklist refleja estado por documento y alertas se envían 30/7/1 días antes de vencimiento (configurable).

4. Asistente de investigación y presupuesto
   - Usuario solicita estimación básica; sistema entrega rango de presupuesto y referencias de mercado en formato breve.
   - Aceptación: Respuesta incluye al menos 1 rango numérico y 2 factores de referencia.

5. API
   - Cliente hace llamada autenticada para obtener la calificación de compatibilidad de una empresa vs un pliego.
   - Aceptación: Respuesta JSON contiene score (0–100), lista de advertencias, y array de sugerencias.

## Requisitos funcionales (testables)
RF-1. Centralizar búsquedas SECOP
- Dado que SECOP I/II tienen APIs públicas, cuando un usuario realice una búsqueda por UNSPSC o ubicación, entonces la plataforma debe devolver resultados agregados y normalizados 

RF-2. Calificación de compatibilidad (motor)
- El sistema calculará una compatibilidad entre 0 y 100 basada en: requisitos obligatorios del pliego (pasar/fallar), cumplimiento documental, requisitos de capacidad económica/técnica y factores ponderados configurables.
- Prueba: para un conjunto de 10 pliegos test con datos conocidos, el motor debe producir scores reproducibles y documentar la fórmula usada en la especificación técnica (internamente).

RF-3. Checklist visual y gestión de documentos
- El panel debe mostrar checklist por pliego con estado (faltante/completado) y permitir subir documentos asociados a ítems.
- Prueba: crear 1 proceso de prueba y subir 3 documentos; la UI debe marcar al menos 2 como completados.

RF-4. Alertas de RUP expirado
- El sistema debe detectar RUPs con fecha de expiración y generar alertas configurables (ej. 30/7/1 días antes).
- Prueba: insertar RUP con fecha cercana y verificar que se generen las 3 alertas en el cronograma.

RF-5. Chats y plantillas
- Ofrecer chat de IA simple y repositorio de plantillas llenables por el asistente de IA.
- Prueba: crear y descargar una plantilla; enviar 1 mensaje en chat y comprobar historial.

RF-6. Asistente de investigación y presupuesto
- El asistente entregará un resumen ejecutivo y un rango de presupuesto estimado; las respuestas deben incluir un nivel de confianza (alta/media/baja).
- Prueba: solicitar 5 consultas de ejemplo y verificar formato de respuesta.

RF-7. API pública
- Proveer endpoint autenticado que devuelve score, advertencias y sugerencias; respuestas deben estar en JSON y documentadas.
- Prueba: llamada de ejemplo devuelve los campos esperados y códigos de error claros.

RF-8. Privacidad y consentimiento
- Almacenar únicamente la PII mínima necesaria (por ejemplo: email, teléfono, NIT) cifrada en reposo y en tránsito. El almacenamiento de cualquier PII requiere registro explícito de consentimiento del usuario; los accesos a PII deben registrarse en auditoría y ser trazables.
- Retención: los datos PII se mantienen hasta que el usuario solicite su eliminación explícita. La revocación de consentimiento desactiva el uso operativo y el acceso a la PII, pero no borra automáticamente los registros salvo que el usuario solicite eliminación.
- Prueba: crear usuario que concede y revoca consentimiento y verificar que PII se almacena/cifra correctamente, que la revocación impide acceso futuro, que la eliminación por solicitud borra o anonimiza los datos y que las entradas de auditoría se generan para lecturas y modificaciones.

RF-9. Freemium & Quotas
- Modelo freemium: acceso básico gratuito para MiPymes con sin límite de búsquedas en SECOP; las funciones que dependan de capacidades de IA (p. ej. calificación automatizada avanzada, resúmenes de pliegos generados por IA, asistencia de presupuesto avanzada) estarán limitadas o degradadas para cuentas gratuitas (por ejemplo, acceso a sugerencias básicas en lugar de sugerencias avanzadas). Límites de uso y degradaciones deben ser configurables.
- Prueba: crear cuenta gratuita y verificar que las búsquedas funcionan sin límite pero que intentos de usar funciones IA restringidas reciben una respuesta degradada o mensaje de upsell.

## Success criteria (medibles y verificables)
- SC-1: Descalificaciones reducidas en 80% para usuarios activos vs baseline (medir tasa de rechazo antes/después en cohortes de 3–6 meses).
- SC-2: Tiempo medio de calificación por pliego para experiencia asistida < 10 minutos (meta), con objetivo ideal < 2 minutos en flujos guiados.
- SC-3: Incremento de wins para MiPymes 50% en cohortes con uso de incentivos visuales (medible por tasa de adjudicación anual comparada con baseline).
- SC-4: Latencia percibida en búsquedas endpoints críticos (login, consulta) < 3s bajo condiciones normales (alineado con la constitución).
- SC-5: 100% de los datos sensibles cifrados en reposo; consentimiento registrado para almacenar PII.

## Key entities (datos involucrados)
- Usuario: id, nombre, correo, teléfono (consentimiento para PII), rol, cuenta (freemium/paid).
- Empresa: id, RUP, NIT, descripción, actividad económica (UNSPSC), status.
- Proyecto (Licitación-Usuario): id, referencia SECOP, título, requisitos, fechas, documentos asociados.
- Documento: id, tipo (DID u otro), referencia a licitación y usuario, estado (subido/pendiente), metadatos de consentimiento.
- Pliego/Requirement: estructura extraída del SECOP para la comparación.

## Privacidad, seguridad y cumplimiento
- Almacenar PII mínima necesaria (email, teléfono, NIT, etc) cifrada en reposo y en tránsito; registro de consentimiento obligatorio para almacenar PII.
- Retención: PII retenida hasta solicitud explícita de eliminación por parte del usuario. La revocación de consentimiento desactiva uso operativo y acceso; para eliminar registros se debe procesar la solicitud de eliminación según la política de retención.
- Accesos a PII deben auditarse y ser trazables; políticas de retención y eliminación deben aplicarse.
- Datos de analítica deben ser agregados y anonimizados cuando se usen para informes.

## Assumptions
- SECOP I/II disponen de APIs públicas o endpoints que permiten ingestión automática; si no, el sistema usará scraping con acuerdo legal y rate limits.
- Motor de compatibilidad inicial es heurístico configurable; mejoras de ML pueden añadirse en fases posteriores.
- Freemium limita ciertas funciones, pero no impide flujos críticos de elegibilidad.
- Se dispone de mecanismos de verificación manual para RUPs en casos de disputa.

## Edge cases
- RUP expirado o inválido: generar alertas y marcar riesgo en la calificación.
- Pliegos con requisitos ambiguos: marcar como "requiere revisión humana" y permitir que el consultor adjunte notas.
- Usuarios sin suficiente información: permitir guardar borradores y marcar "incompleto" en checklists.

## Dependencias
- Acceso a SECOP I y II.
- Servicio de almacenamiento cifrado y gestión de llaves.
- Servicio de notificaciones.

## Entregables de la fase 1
- Spec y API pública básica documentada.
- Interfaz de búsqueda y panel de calificación básico.
- Motor de compatibilidad heurístico con reglas configurables.
- Panel de seguimiento de documentos y alertas de RUP.
- Funciones básicas de chat de Inteligencia Artificial y plantillas.

## Acceptance & Ready
- Spec lista para planificación: YES

## Notas y próximos pasos
- Priorizar integración SECOP y motor de compatibilidad.
- Diseñar flujos guiados para MiPymes que reduzcan clicks y expliquen valor en <60s.
- Plan de instrumentación para medir SC-1 a SC-3.

## Archivo
Spec file path: specs/1-licitaciones-inteligentes/spec.md
