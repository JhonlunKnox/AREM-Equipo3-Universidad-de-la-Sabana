# Procedimiento de actualizacion y revision de la arquitectura

## Proposito y alcance

Este procedimiento se aplica despues de aprobar la arquitectura objetivo. Cubre cambios a datos, procesos, aplicaciones, automatizaciones, permisos, integraciones, estandares y riesgos. No reemplaza el procedimiento institucional de incidentes, cambios de TI o proteccion de datos.

## Disparadores

| Categoria | Disparador concreto | Ejemplo del proyecto | Prioridad inicial |
|---|---|---|---|
| Regulacion o politica | Cambio en ley, politica institucional, clasificacion o retencion | Nueva instruccion sobre tratamiento del Id Empleado o conservacion de nomina | Alta |
| Seguridad | Incidente, hallazgo Alto o Critico, acceso indebido o control fallido | Enlace expone biblioteca de nomina; respuesta de Tecnologia no esta autenticada | Urgente |
| Plataforma | Cambio de licencia, conector, DLP, limite o disponibilidad | Una accion de Power Automate deja de estar permitida | Alta |
| Proveedor | Cambio material de Microsoft 365 o del soporte institucional | Deprecacion de una accion o cambio de propiedad de flujo | Media |
| Datos | Cambio de esquema, llave, taxonomia o volumen | Nomina cambia encabezados; aparece duplicidad de Id Empleado | Alta |
| Negocio | Cambio de proceso, responsable, SLA o alcance | Tecnologia crea un nuevo canal de aprovisionamiento | Media |
| Demanda | Aumento significativo de registros, solicitudes o frecuencia | El directorio o las solicitudes superan el comportamiento probado | Media |
| Desempeno de gobierno | Metrica bajo umbral o decisiones vencidas | M1 menor a 90% o M3 mayor a 10 dias habiles | Media |
| Roadmap | Una fase necesita desviarse del TO-BE | El tenant no permite el conector propuesto | Alta |

## Clasificacion del cambio

| Tipo | Criterio | Ruta |
|---|---|---|
| Estandar | Reversible, probado, sin cambio de principio, riesgo ni dato | Aprobacion operativa y registro en bitacora |
| Arquitectonico | Cambia un principio, estandar, integracion, llave, propietario, control o decision costosa de revertir | ADR y dictamen del comite |
| Urgente | Responde a incidente o riesgo de daño inmediato | Contencion inmediata y ADR retrospectiva dentro de 5 dias habiles |
| Mayor | Cambia alcance, objetivo, plataforma o tratamiento de datos | ADR, reevaluacion de riesgos y aprobaciones institucionales correspondientes |

## Flujo

1. **Registrar.** El proponente crea una solicitud con disparador, alcance, urgencia, evidencia, riesgos y responsable.
2. **Triage en dos dias habiles.** El arquitecto y el dueño del proceso clasifican el cambio y determinan si requiere ADR.
3. **Analizar.** Se identifican alternativas, principios afectados, controles, costo, reversibilidad, impacto sobre datos y dependencias.
4. **Consultar.** Tecnologia, Seguridad o Proteccion de Datos participan cuando el cambio toca su competencia.
5. **Decidir.** El comite acepta, rechaza, devuelve para ajuste o aprueba de forma temporal con fecha de expiracion.
6. **Planear.** Se define piloto, pruebas, respaldo, comunicacion, capacitacion, ventana y criterio de reversa.
7. **Implementar.** El responsable ejecuta en el entorno aprobado y conserva evidencia.
8. **Verificar.** Se compara el resultado con la ADR, el TO-BE, los riesgos y las metricas.
9. **Cerrar.** Se actualizan vistas, matriz de riesgos, inventario, manuales y estado de la ADR.

## Tiempos objetivo

| Actividad | Tiempo objetivo |
|---|---:|
| Triage | 2 dias habiles |
| Dictamen de cambio arquitectonico | 10 dias habiles |
| Escalamiento de decision sin respuesta | Dia habil 10 |
| Contencion de incidente | Inmediata segun procedimiento institucional |
| ADR retrospectiva de cambio urgente | 5 dias habiles despues de estabilizar |

El tiempo de dictamen alimenta M3. Los dias se cuentan de lunes a viernes; los festivos requieren el calendario institucional si se automatiza el calculo.

## Roles

| Rol | Responsabilidad |
|---|---|
| Proponente | Describe el problema y aporta evidencia, no una solucion predeterminada |
| Dueño del proceso | Define prioridad y acepta impacto operativo o riesgo de negocio |
| Arquitecto responsable | Verifica principios, alternativas, trade-offs y coherencia de vistas |
| Tecnologia | Valida viabilidad, soporte, licencias, plataforma e integraciones |
| Seguridad y Proteccion de Datos | Evalua acceso, DLP, datos personales, retencion e incidentes |
| Implementador | Ejecuta pruebas, respaldo, cambio y reversa |
| Responsable operativo | Acepta la operacion y comprueba el resultado visible |

## Reglas del registro ADR

- Estados permitidos: Propuesta, Aceptada, Rechazada o Reemplazada por ADR-XXX.
- Una ADR aceptada no se edita para cambiar su sentido. Una decision nueva la reemplaza.
- Toda alternativa descartada conserva la razon del descarte.
- Las consecuencias incluyen beneficio, costo, deuda y trade-off.
- Las fechas historicas no registradas se declaran como no disponibles. No se inventan para completar M3.
- Una ADR vive junto a la arquitectura que gobierna y enlaza riesgos, Work Packages y vistas.

## Registro de decisiones existentes

### ADR-001 Microsoft Lists como fuente unica del directorio

| Campo | Contenido |
|---|---|
| Estado | Aceptada en el diseño objetivo; pendiente de aprobacion institucional para implantacion |
| Fecha historica | La decision aparece en los Talleres 3 y 7. La fecha exacta del dictamen no quedo registrada; se formaliza en este ADR el 6 de noviembre de 2026 |
| Contexto | El directorio actual es un Excel en OneDrive que mezcla consulta, edicion, estados y fuente de verdad. Su titularidad y recuperacion deben confirmarse |
| Problema | El archivo no ofrece un esquema gobernado del proceso, contiene un estado pendiente sin salida y permite copias o cambios que no se vinculan a una solicitud |
| Decision | Usar Microsoft Lists o una lista de SharePoint dentro de un sitio institucional como fuente unica del directorio; separar biblioteca de nomina, catalogos, novedades y solicitudes |
| Alternativas consideradas | Mantener Excel en OneDrive: descartada porque conserva fragilidad, copias paralelas y gobierno insuficiente. Dataverse o base a medida: descartada por licencias, complejidad y aprobacion. Lista institucional: elegida por encajar en Microsoft 365 y aportar estructura, permisos e historial |
| Consecuencias positivas | Fuente institucional, roles por grupo, historial, estados controlados y mejor consulta |
| Consecuencias negativas | SharePoint Lists no equivale a una base relacional; exige validaciones adicionales, gobierno de permisos y pruebas de volumen. La migracion y los catalogos requieren trabajo inicial |
| Trade-off | Se sacrifica flexibilidad del archivo y algunas restricciones relacionales a cambio de trazabilidad, continuidad y operabilidad dentro de la plataforma existente |
| Trazabilidad | P1, P2, P3 y P4; WP1; riesgos R-03, R-06, R-09, R-11 y R-13; gaps G02, G06, G07, G09, G10 y G12 |

### ADR-002 Power Automate para la conciliacion objetivo

| Campo | Contenido |
|---|---|
| Estado | Aceptada por la matriz de decision del Taller 7; conectores, DLP y licencias pendientes de validacion |
| Fecha historica | Decision documentada en el Taller 7; fecha exacta no registrada. Se formaliza en este ADR el 6 de noviembre de 2026 |
| Contexto | El diseño del Taller 3 proponia Excel y Power Query con ejecucion guiada. El Taller 7 evaluo alternativas y selecciono Power Automate con SharePoint para producir excepciones dentro del tenant |
| Problema | Mantener ambas opciones como arquitectura objetivo crea dos diseños incompatibles para el mismo paso, dificulta soporte y deja la ejecucion dependiente del equipo de una persona |
| Decision | Definir Power Automate con conectores estandar y recursos de SharePoint como arquitectura objetivo de conciliacion. Power Query queda como prototipo o contingencia manual controlada |
| Alternativas consideradas | Revision manual: descartada por 6.400 registros y periodicidad incumplida. Power Query como objetivo: viable y mas simple, pero exige ejecucion del libro y no resuelve por si solo el ciclo de estados. Desarrollo propio: descartado por restricciones institucionales. Power Automate: elegido por orquestacion, trazabilidad y ejecucion en el tenant |
| Consecuencias positivas | Proceso programable, monitoreo central, registro de fallas y menor dependencia del endpoint de una persona |
| Consecuencias negativas | Depende de licencias, conectores, politicas DLP, propietarios y limites del servicio. Configurar idempotencia y manejo de errores agrega complejidad |
| Trade-off | Se sacrifica simplicidad local y facilidad de prototipo a cambio de continuidad institucional, trazabilidad y operacion automatizada |
| Trazabilidad | P1, P2, P5 y P6; WP0 y WP2; riesgos R-05, R-06, R-07, R-08 y R-14; gaps G01, G05, G08, G11, G13 y G14 |

### ADR-003 Confirmacion humana autenticada sin integracion directa con el PBX

| Campo | Contenido |
|---|---|
| Estado | Aceptada en el alcance de los Talleres 0, 3 y 7 |
| Fecha historica | La decision esta implicita desde la Vision y se formaliza en este ADR el 6 de noviembre de 2026 |
| Contexto | El PBX es administrado por Tecnologia y esta fuera del alcance. La unidad necesita saber si una extension fue aprovisionada o liberada |
| Problema | Sin retorno, el estado pendiente no tiene salida. Integrar el PBX aumentaria permisos, costo, plazo y riesgo mas alla del proyecto |
| Decision | Mantener la operacion tecnica del PBX en Tecnologia y registrar su respuesta autenticada en una solicitud. El flujo actualiza el estado administrativo solo si la solicitud sigue vigente y la respuesta es coherente |
| Alternativas consideradas | Integracion directa con PBX: descartada por alcance, seguridad y falta de interfaz confirmada. Continuar con Teams o reuniones: descartada por falta de estado y evidencia. Confirmacion estructurada: elegida como limite viable |
| Consecuencias positivas | Cierra el ciclo, conserva responsabilidad de Tecnologia y evita exponer la plataforma telefonica |
| Consecuencias negativas | La arquitectura depende de que una persona responda correctamente. El sistema no puede comprobar por si solo el estado tecnico del PBX y debe escalar silencios o contradicciones |
| Trade-off | Se sacrifica verificacion tecnica automatica a cambio de menor alcance, menor riesgo de integracion y viabilidad institucional |
| Trazabilidad | P3, P4, P5 y P6; WP3; riesgos R-02, R-04, R-08 y R-12; gaps G03 y G04 |

## Plantilla para ADR futuras

| Campo | Contenido requerido |
|---|---|
| ID y titulo | ADR-XXX y decision en una frase |
| Estado | Propuesta, Aceptada, Rechazada o Reemplazada por ADR-XXX |
| Fecha de propuesta | Fecha real de registro |
| Fecha de dictamen | Fecha real o Pendiente |
| Disparador | Evento concreto que obliga a decidir |
| Contexto y problema | Situacion actual y daño de no decidir |
| Decision | Opcion elegida y alcance |
| Alternativas | Opciones evaluadas y razon de descarte |
| Consecuencias | Positivas, negativas, deuda y costo operativo |
| Trade-off | Atributo que se sacrifica y atributo priorizado |
| Riesgos y controles | IDs de la matriz y cambio de nivel esperado |
| Trazabilidad | Principios, estandares, Work Packages y vistas afectados |
| Aprobadores | Roles y evidencia del dictamen |

## Revision periodica completa

Cada seis meses, y siempre despues de una fase mayor, el comite revisa:

1. Objetivos, stakeholders, restricciones y supuestos.
2. Arquitectura real frente a los Plateaus de la vista de Migracion.
3. Matriz de riesgos, controles y aceptaciones vigentes.
4. Principios, estandares y ADR reemplazadas.
5. Metricas M1, M2 y M3.
6. Inventario de aplicaciones, datos, propietarios, conexiones y grupos.
7. Cambios de regulacion, licencia, DLP, plataforma o proveedor.
8. Documentacion, contingencia, formacion y capacidad de recuperacion.

La revision produce un acta, una matriz de riesgos actualizada y, si existe una diferencia material entre lo aprobado y lo real, un nuevo Gap y Work Package.

