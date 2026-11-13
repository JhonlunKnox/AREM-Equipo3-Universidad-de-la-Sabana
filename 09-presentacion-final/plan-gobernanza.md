# Plan de gobernanza de la implementacion

## Objetivo

Asegurar que cada fase del roadmap respete la arquitectura objetivo, los riesgos aceptados y las restricciones institucionales. El gobierno debe resolver decisiones con rapidez y producir evidencia, no convertirse en una reunion adicional sin efecto.

## Marco de decisiones

La cadena de gobierno es:

**Objetivos del cliente -> principios -> estandares -> ADR -> implementacion -> metricas**

Una fase no se declara conforme solo porque funciona. Debe realizar el Work Package aprobado, cumplir los controles asociados y dejar evidencia de operacion por la unidad.

## Evolucion de principios arquitectonicos

| ID | Estado frente a la Vision | Principio | Declaracion | Evidencia del proyecto | Implicacion concreta |
|---|---|---|---|---|---|
| P1 | Reescrito | Plataforma corporativa primero | Las capacidades nuevas usan servicios institucionales de Microsoft 365 y conectores estandar, sujetos a permisos y DLP aprobados | Restriccion C1-C2; C4 e infraestructura; decision T7 | Se rechaza software externo, instalacion local o conector premium sin ADR y aprobacion del tenant |
| P2 | Confirmado | Operabilidad por la unidad | La unidad debe ejecutar, corregir y recuperar el proceso sin depender del equipo academico | Expectativa del cliente; criterio de exito de la Vision | Se rechaza una solucion que requiera codigo propio, conocimiento no transferido o una cuenta del equipo academico |
| P3 | Reescrito | Fuente unica con trazabilidad | El directorio vigente y su ciclo de cambio viven en recursos institucionales con esquema, roles, versiones y evidencia | Q5-Q8; N3-N6; S1 del Taller 7 | Se rechazan copias operativas paralelas, estados en mensajes y cambios sin solicitud o autor identificado |
| P4 | Nuevo | Privacidad y minimo privilegio desde el diseño | La nomina se separa del directorio y cada actor accede solo a los campos necesarios | STRIDE I-01, I-02, E-03; Ley 1581 y checklist normativo | Se rechaza adjuntar la nomina a solicitudes, heredar permisos amplios o publicar datos reales en repositorios |
| P5 | Nuevo | Automatizar solo lo verificable | Los pasos repetibles se automatizan; las coincidencias ambiguas y cambios sensibles conservan validacion humana | C4, T-02 y decision de validacion humana | Se rechaza una actualizacion ciega por correo, cargo, unidad o ausencia en un corte |
| P6 | Nuevo | Falla visible y recuperable | Una falla debe generar alerta, conservar el ultimo estado valido y ofrecer una contingencia documentada | D-01, D-02, RT4 y controles de continuidad | Se rechaza un flujo que falle en silencio o deje una solicitud sin responsable y siguiente accion |

El borrador original no se retira por completo. P1 y P3 se precisan para que puedan negar opciones concretas. P4 a P6 aparecen porque los talleres de seguridad, normatividad e integracion mostraron vacios que la Vision inicial no cubria.

## Estandares tecnologicos

| Categoria | Estandar | Razon | Principio y objetivo que lo sustentan | Puerta de excepcion |
|---|---|---|---|---|
| Plataforma | SharePoint Online y Microsoft Lists para biblioteca, directorio, catalogos, novedades y solicitudes | Aporta estructura, permisos e historial dentro de la plataforma institucional | P1 y P3; G1 y G4 | ADR con analisis de migracion, propiedad, seguridad y sostenibilidad |
| Automatizacion | Power Automate con conectores estandar aprobados por DLP | Ejecuta conciliacion y estados dentro del tenant sin codigo local | P1, P2 y P6; G2 y G4 | ADR si se requiere premium, RPA o desarrollo propio |
| Identidad | Microsoft Entra ID y grupos institucionales por rol | Permite autenticar y separar consulta, custodia, Tecnologia y administracion | P4; cumplimiento y trazabilidad | ADR y aprobacion de Seguridad para cualquier identidad no institucional |
| Datos | Id Empleado e Id Posicion como texto; claves tecnicas idempotentes; catalogos versionados | Evita perdida de ceros, ambiguedad y duplicados por reintento | P3 y P5; G1 | ADR para cambiar la llave o aceptar correspondencia probabilistica |
| Integracion | Solicitudes mediante Lists y notificaciones por Teams o Outlook con ID y enlace | Cierra el ciclo con Tecnologia sin exponer la nomina ni integrar el PBX | P3 y P4; G4 | ADR para integrar el PBX o enviar datos adicionales |
| Ciclo de entrega | Entorno de pruebas con datos ficticios, puertas de fase y restauracion verificada | Reduce el riesgo de cambiar produccion sin evidencia | P5 y P6 | ADR para omitir una puerta por urgencia, con expiracion y compensacion |

## Estructura de gobierno

### Comite de Arquitectura ligero

| Rol | Responsabilidad | Participacion minima |
|---|---|---|
| Dueño del proceso - Jefatura de Cultura de Innovacion y Servicio | Prioridad, valor, aceptacion operativa y riesgo de negocio | Obligatoria para cerrar fases y aceptar riesgos |
| Arquitecto responsable o delegado tecnico | Coherencia con TO-BE, ADR, integracion de vistas y deuda aceptada | Obligatoria en toda decision arquitectonica |
| Representante de Tecnologia o Power Platform | Viabilidad del tenant, licencias, conectores, propiedad y soporte | Obligatoria cuando cambia plataforma o automatizacion |
| Seguridad y Proteccion de Datos | Acceso, DLP, finalidad, retencion e incidentes | Obligatoria antes de datos reales y para riesgos altos de seguridad |
| Responsable operativo | Usabilidad, prueba del ciclo y transferencia | Obligatoria para aceptar la operacion de cada fase |

El comite puede reunirse con tres miembros cuando la decision no afecta datos personales, seguridad o plataforma. Las decisiones que si los afectan no se toman sin el representante competente.

## Cadencia

- **Piloto:** seguimiento quincenal de 45 minutos y puerta de calidad al cierre de cada fase.
- **Operacion inicial:** reunion mensual durante los primeros tres meses.
- **Operacion estable:** revision trimestral de metricas y revision completa semestral.
- **Urgencia:** sesion extraordinaria dentro de cinco dias habiles para incidente, cambio regulatorio o falla que comprometa datos.

## Criterio de conformidad

Una fase cierra cuando cumple todos estos puntos:

1. Realiza el Work Package y el Plateau definidos en la vista de Migracion.
2. Cumple los principios y estandares, o tiene una excepcion aprobada mediante ADR.
3. Mitiga los riesgos asignados hasta el nivel residual previsto o registra aceptacion por el dueño competente.
4. Supera pruebas funcionales, de acceso, trazabilidad, error y recuperacion.
5. Deja documentacion, propietario, copropietario y contingencia.
6. Una persona fuera del equipo tecnico comprueba el resultado visible.

## Escalamiento

| Situacion | Accion | Quien decide | Plazo objetivo |
|---|---|---|---|
| Desviacion menor sin efecto sobre principios ni riesgos | Registrar ajuste en el acta de fase | Arquitecto y responsable operativo | 5 dias habiles |
| Excepcion a un estandar o cambio dificil de revertir | Crear ADR y evaluar alternativas | Comite de Arquitectura | 10 dias habiles |
| Riesgo residual Alto o Critico | No pasar a datos reales; definir control adicional o aceptacion formal | Dueño del proceso con Seguridad o Proteccion de Datos | Antes de la puerta de fase |
| Incidente de seguridad o exposicion | Detener automatizacion, conservar evidencia y activar procedimiento institucional | Seguridad y dueño del proceso | Inmediato |
| Bloqueo del tenant o licenciamiento | Replantear el Work Package y registrar ADR | Tecnologia y comite | Antes de construir alternativa |

## Metricas de gobierno

### M1 Cumplimiento del ciclo mensual

| Campo | Definicion |
|---|---|
| Que mide | Si P2 Operabilidad por la unidad y el objetivo G2 se cumplen |
| Formula | Ciclos de conciliacion ejecutados y cerrados antes de la fecha acordada / ciclos programados x 100 |
| Fuente existente | Hoy: calendario mensual, archivo de nomina y fecha de actualizacion del directorio. TO-BE: historial de lotes y novedades |
| Linea base | Frecuencia real declarada: un ciclo cada 2 o 3 meses. Equivale a aproximadamente 33% a 50% de los ciclos mensuales; debe confirmarse con historial |
| Umbral | Actuar si es menor a 90% en una ventana movil de tres meses o si se omite un ciclo sin decision registrada |
| Responsable | Dueño del proceso |
| Accion | Analizar causa, reasignar responsable, activar contingencia y registrar riesgo si se repite |

### M2 Cambios sensibles con trazabilidad completa

| Campo | Definicion |
|---|---|
| Que mide | Si P3 Fuente unica con trazabilidad y P5 Automatizar solo lo verificable se cumplen |
| Formula | Cambios de titular, extension o estado con solicitud, validacion, autor y fechas completas / total de cambios sensibles aplicados x 100 |
| Fuente existente | Hoy: directorio y comunicaciones de Teams o reuniones, aunque no forman un registro completo. TO-BE: listas de Novedades y Solicitudes e historial de versiones |
| Linea base | No calculable con evidencia actual: el proceso no conserva un registro estructurado de cada cambio. Se establece durante el primer ciclo instrumentado |
| Umbral | 100%. Cualquier cambio sensible sin evidencia abre una no conformidad y revisa accesos |
| Responsable | Profesional de Experiencia y Servicio |
| Accion | Suspender automatizacion del caso, reconstruir evidencia y revisar la transicion o permiso que lo permitio |

### M3 Tiempo de dictamen del comite

| Campo | Definicion |
|---|---|
| Que mide | Si el propio gobierno resuelve decisiones sin bloquear al negocio |
| Formula | Suma de dias habiles entre Fecha de propuesta y Fecha de dictamen de ADR o excepciones / numero de decisiones con dictamen |
| Fuente existente | Registro de ADR en el procedimiento de cambios y actas del comite |
| Linea base | No existe. El registro comienza con este plan y no se inventa un valor historico |
| Umbral | Actuar si el promedio supera 10 dias habiles o si una decision supera 15 dias sin estado y responsable |
| Responsable | Secretario del comite o arquitecto responsable |
| Accion | Simplificar quorum por tipo de decision, delegar decisiones menores y revisar causas de espera |

## Tablero minimo de seguimiento

| Tema | Indicador | Estado esperado | Evidencia |
|---|---|---|---|
| Roadmap | Fases cerradas con puerta aprobada | Ninguna fase cerrada solo por porcentaje de avance | Acta y checklist de conformidad |
| Riesgos | Riesgos residuales Altos o Criticos | Cero sin aceptacion o plan activo | Matriz actualizada |
| Decisiones | ADR propuestas sin dictamen | Ninguna supera 15 dias habiles | Registro de ADR |
| Operacion | M1 y M2 | M1 igual o superior a 90%; M2 igual a 100% | Historial de lotes, solicitudes y versiones |
| Adopcion | Usuarios formados y resultado visible | Responsable operativo ejecuta sin el equipo academico | Evidencia de prueba y aceptacion |

## Evidencia y archivo

- Las ADR viven en el repositorio y no se reescriben para cambiar su sentido.
- Las actas registran asistentes, decision, riesgos, condiciones y fecha.
- La matriz de riesgos se revisa al cerrar cada fase y en la revision semestral.
- Capturas y datos de prueba no deben contener datos reales.
- La aprobacion del cliente o de la Universidad no se presume. Se adjunta solo si la persona autorizada la entrega.

