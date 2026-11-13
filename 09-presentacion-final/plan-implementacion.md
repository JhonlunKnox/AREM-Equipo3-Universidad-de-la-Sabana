# Plan de implementacion y gestion del cambio

## Proposito

Llevar la arquitectura desde el directorio manual en Excel hasta un proceso institucional, trazable y operable por la unidad. El plan prioriza primero las condiciones que evitan construir una solucion que el tenant, la regulacion o el equipo no puedan sostener.

## Principios de secuenciacion

- Ninguna fase usa datos reales antes de validar permisos, finalidad, retencion y responsables.
- El directorio gobernado se construye antes que los flujos que lo actualizan.
- La conciliacion se prueba con excepciones y validacion humana antes de automatizar cambios.
- El ciclo con Tecnologia se cierra sin integrar el PBX; la respuesta autenticada es la evidencia administrativa.
- Cada fase deja un resultado que puede comprobar una persona fuera del equipo tecnico.

## Roadmap

| Fase | Horizonte | Work Package | Que se implementa | Riesgos y brechas que atiende | Esfuerzo | Responsable sugerido | Puerta de salida |
|---|---|---|---|---|---|---|---|
| 0. Habilitacion y control | Semanas 0 a 4 | WP0 | Inventario de permisos y licencias; validacion DLP; base juridica y retencion; grupo de Tecnologia; roles; datos ficticios; criterios de piloto; registro inicial de ADR | R-01, R-07, R-11, R-13, R-14; G08 y controles no validados | Medio | Dueño del proceso con Tecnologia, Seguridad y Proteccion de Datos | Acta de viabilidad con responsables, riesgos abiertos, limites del piloto y decision go o no-go |
| 1. Directorio gobernado | Meses 1 y 2 | WP1 / S1 | Sitio institucional; lista DirectorioExtensiones; catalogos; grupos de acceso; historial; migracion controlada; contingencia y recuperacion | R-03, R-06, R-09, R-11, R-13; G02, G06, G07, G09, G10, G12 | Medio | Experiencia y Servicio con administrador SharePoint | Gestoras consultan una lista de solo lectura; prueba de acceso y restauracion aprobada |
| 2. Conciliacion por excepciones | Meses 2 a 4 | WP2 / S2 | Flujo con conectores estandar; validacion de esquema; llave estable; equivalencias; deteccion de excepciones; idempotencia; revision humana; monitoreo | R-05, R-06, R-07, R-08, R-15, R-16; G01, G05, G08, G11, G13, G14 | Alto | Propietario del proceso y equipo Power Platform | Tres ciclos ficticios, incluido uno fallido, producen excepciones sin modificar datos ambiguos |
| 3. Aprovisionamiento trazable y transferencia | Meses 4 a 6 | WP3 / S3 | Lista de solicitudes; estados; notificacion minima; respuesta autenticada; SLA; vencimiento; escalamiento; tablero operativo; manual y capacitacion | R-02, R-04, R-08, R-12; G03, G04 | Medio | Experiencia y Servicio con analista de Tecnologia | Solicitud de prueba completa creacion, gestion, confirmacion, cierre y escalamiento; unidad opera sin el equipo academico |

Los horizontes son estimaciones de orden y dependencia. La Universidad debe confirmar capacidad, calendario y disponibilidad antes de convertirlos en fechas comprometidas.

## Detalle de fases

### Fase 0 Habilitacion y control

**Objetivo.** Resolver las condiciones que determinan si la arquitectura puede pilotearse de forma segura.

**Actividades.**

1. Nombrar dueño del proceso, propietario operativo del sitio, propietario y copropietario de flujos, y representantes de Tecnologia, Seguridad y Proteccion de Datos.
2. Confirmar licencias asignadas, conectores permitidos, capacidad de crear sitio y listas, politicas DLP y restricciones del tenant.
3. Documentar finalidad, campos minimos, base juridica, perfiles de acceso, retencion y eliminacion.
4. Acordar el grupo de Tecnologia que recibira solicitudes y el plazo objetivo de respuesta.
5. Preparar un conjunto de prueba ficticio con duplicados, llaves faltantes, cambio de estructura, reintentos, respuesta tardia y rechazo.
6. Aprobar las ADR existentes y el criterio para nuevas excepciones.

**No se hace.** No se carga nomina real, no se promete automatizacion total y no se abre acceso al equipo academico.

### Fase 1 Directorio gobernado

**Objetivo.** Crear la base estable que necesitan los demas paquetes.

**Actividades.**

1. Configurar el sitio y las listas con columnas tipadas y claves estables.
2. Crear grupos separados para consulta, custodia, Tecnologia y administracion.
3. Habilitar historial y comprobar restauracion con datos ficticios.
4. Validar catalogos de unidades, equivalencias y cargos elegibles.
5. Migrar un subconjunto ficticio y conciliar conteos antes y despues.
6. Publicar vistas de consulta de solo lectura para las gestoras.

**Criterio de conformidad.** La lista realiza S1 y reemplaza el Excel como fuente objetivo. No existen permisos heredados que expongan la biblioteca de nomina a las gestoras.

### Fase 2 Conciliacion por excepciones

**Objetivo.** Sustituir la revision de miles de filas por una lista auditable de diferencias.

**Actividades.**

1. Validar el contrato de entrada y detener el proceso ante columnas faltantes, duplicados, conteo anomalo o periodo incorrecto.
2. Conservar Id Empleado como texto y usar correo solo para una migracion inicial sin ambiguedad.
3. Aplicar equivalencias de unidad y reglas de elegibilidad aprobadas.
4. Generar novedades clasificadas sin publicar cambios directamente.
5. Exigir validacion humana para retiros, vacancias, cambios de titular y coincidencias dudosas.
6. Probar idempotencia, reintentos y restauracion del ultimo estado valido.
7. Alertar fallas y registrar responsable, fecha y evidencia.

**Criterio de conformidad.** Ninguna excepcion ambigua modifica el directorio. El mismo lote ejecutado dos veces no duplica novedades ni solicitudes.

### Fase 3 Aprovisionamiento trazable y transferencia

**Objetivo.** Cerrar el estado pendiente y entregar la operacion a la unidad.

**Actividades.**

1. Configurar estados y transiciones de la solicitud.
2. Enviar avisos con ID y enlace protegido, sin adjuntar la nomina.
3. Verificar identidad y grupo autorizado de quien responde.
4. Impedir que una respuesta duplicada, tardia o contradictoria sobrescriba una asignacion posterior.
5. Implementar vencimiento, recordatorio y escalamiento cuando exista un SLA aprobado.
6. Crear una vista operativa de solicitudes abiertas, vencidas y cerradas.
7. Capacitar, observar un ciclo y ejecutar una prueba de continuidad sin el equipo academico.

**Criterio de conformidad.** Cada solicitud cerrada conserva autor, fechas, resultado tecnico y evidencia de aplicacion. Una solicitud abierta siempre tiene responsable y siguiente accion.

## Gestion del cambio con las personas

| Grupo afectado | Que pierde o teme | Resistencia esperada | Poder e interes | Comunicacion | Capacitacion | Momento | Responsable |
|---|---|---|---|---|---|---|---|
| Profesional de Experiencia y Servicio | Perder control del archivo conocido; asumir responsabilidad sobre flujos y excepciones | Mantener el Excel paralelo o evitar cerrar excepciones complejas | Alto / Alto | Demostracion con sus casos reales y acuerdo de limites | Taller practico de carga, validacion, correccion y contingencia | Desde Fase 0; refuerzo en Fases 2 y 3 | Arquitecto responsable y administrador Power Platform |
| Gestoras de servicio | Que la nueva consulta sea mas lenta o cambie su rutina | Seguir usando enlaces o copias antiguas | Medio / Alto | Mostrar que la busqueda conserva los campos que usan | Sesion de 45 minutos sobre consulta, filtros y reporte de un error | Antes de cerrar Fase 1 | Profesional de Experiencia y Servicio |
| Analista o grupo de aprovisionamiento de Tecnologia | Recibir mas trabajo administrativo o duplicado | Responder por fuera del flujo y mantener Teams como canal informal | Alto / Alto | Acordar datos minimos, estados, SLA y criterios de rechazo | Prueba guiada de respuesta, solicitud de informacion y cierre | Diseño en Fase 0; piloto en Fase 3 | Dueño del proceso y representante de Tecnologia |
| Administrador Microsoft 365 y Seguridad | Perder control sobre conectores, datos y permisos | Bloquear el piloto si recibe el diseño al final | Alto / Medio | Revision temprana de arquitectura, DLP y matriz de acceso | No requiere entrenamiento de uso; requiere lista de verificacion tecnica | Fase 0 y puertas de cada fase | Arquitecto responsable |
| Jefe de Cultura de Innovacion y Servicio | Invertir tiempo sin evidencia de mejora | Priorizar solo la entrega tecnica y omitir adopcion | Alto / Medio | Informe de una pagina por fase con resultado comprobable y riesgo residual | Lectura de metricas, decisiones y escalamiento | Cierre de cada fase | Dueño del proceso |
| Desarrollo Humano | Que se atribuyan errores al archivo fuente o se solicite modificar su sistema | No participar en la definicion del contrato de datos | Medio / Medio | Separar la limitacion de 45 dias de los errores de calidad y pedir solo campos necesarios | Validacion puntual del contrato y de los codigos de unidad o posicion | Fase 0 y pruebas de Fase 2 | Dueño del proceso |

## Resultados visibles por fase

| Fase | Resultado visible | Quien lo comprueba fuera del equipo tecnico | Evidencia |
|---|---|---|---|
| Habilitacion y control | Se conoce si el piloto es viable y bajo que condiciones | Jefe de la unidad y Proteccion de Datos | Acta con decisiones, responsables y riesgos abiertos |
| Directorio gobernado | Las gestoras consultan una fuente institucional sin capacidad de editar | Una gestora designada | Prueba de acceso y consulta; captura sin datos reales |
| Conciliacion por excepciones | Un ciclo ficticio produce solo las novedades y detiene ambiguedades | Profesional de Experiencia y Servicio | Reporte de prueba y decision sobre cada excepcion |
| Aprovisionamiento trazable | Una solicitud recibe respuesta o escalamiento y deja el directorio en estado coherente | Dueño del proceso | Registro completo de la solicitud y verificacion de cierre |

## Plan de comunicacion

| Hito | Mensaje | Audiencia | Canal | Responsable |
|---|---|---|---|---|
| Inicio | Problema, alcance, limites y decisiones que necesitan aprobacion | Unidad, Tecnologia, Seguridad y Proteccion de Datos | Reunion de 45 minutos y acta | Dueño del proceso |
| Puerta de fase | Resultado visible, riesgos abiertos y decision de continuar | Comite de Arquitectura | Reunion quincenal del piloto | Arquitecto responsable |
| Cambio de forma de trabajo | Que deja de hacerse, que se hace ahora y que pasa si falla | Usuarios de la fase | Demostracion y guia breve | Responsable operativo |
| Cierre del piloto | Resultados, metricas, riesgos residuales y soporte | Direccion y usuarios | Informe de cierre | Dueño del proceso |

## Pruebas de aceptacion

1. Llave con ceros iniciales se conserva como texto.
2. Correo duplicado o sin coincidencia no genera aplicacion automatica.
3. Cambio de estructura detiene el lote y conserva el directorio anterior.
4. Reintento del mismo lote no duplica novedades.
5. Respuesta tardia no sobreescribe una solicitud posterior.
6. Usuario de consulta no puede ver la nomina ni editar el directorio.
7. Notificacion no contiene datos excesivos.
8. Falla del flujo genera alerta y permite seguir con procedimiento manual.
9. Propietario alterno puede operar el flujo si falta el propietario principal.
10. La unidad ejecuta un ciclo documentado sin el equipo academico.

## Dependencias y limites

- La nomina sigue llegando mes vencido. El plan no elimina ese desfase.
- El PBX permanece fuera del alcance y sin integracion directa.
- Los valores de SLA, retencion y umbrales de cambio masivo requieren aprobacion institucional.
- Si el tenant no permite la arquitectura objetivo, el comite debe registrar una nueva ADR. No se adopta silenciosamente una tecnologia distinta.

