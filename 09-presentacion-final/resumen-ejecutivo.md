# Resumen ejecutivo del proyecto

**Cliente:** Jefatura de Cultura de Innovacion y Servicio - Universidad de La Sabana  
**Proyecto:** Actualizacion del directorio de extensiones telefonicas  
**Recomendacion:** implantar la arquitectura por fases, comenzando por las condiciones de gobierno y seguridad que permiten un piloto controlado.

## Decision solicitada

Autorizar un piloto con datos ficticios y un alcance reducido para validar la arquitectura dentro del tenant Microsoft 365. El paso a datos reales queda condicionado a la aprobacion institucional de permisos, licencias, proteccion de datos, retencion y responsables operativos.

## Situacion actual

Las cuatro gestoras que atienden llamadas consultan un directorio de extensiones para transferir al usuario al area correcta. La informacion se mantiene en un archivo de Excel alojado en OneDrive. Cada ciclo exige comparar manualmente cerca de 6.400 registros contra la nomina para detectar entre 5 y 10 novedades. El ciclo deberia ser mensual, pero en la practica ocurre cada dos o tres meses.

Cuando la unidad solicita una extension, Tecnologia la aprovisiona y contacta al colaborador. No existe un retorno estructurado hacia la unidad. El registro puede quedar indefinidamente como pendiente aunque la extension ya funcione. La unidad tampoco mide las transferencias fallidas, de modo que el impacto sobre estudiantes, aspirantes y visitantes no tiene una linea base.

## Causas arquitectonicas

| Causa | Evidencia del proyecto | Efecto |
|---|---|---|
| No existe una llave estable comun entre la nomina y el directorio | El Id Empleado solo aparece en la nomina; correo es la llave transitoria | Cruces ambiguos y riesgo de asignar una extension a la persona equivocada |
| El directorio mezcla dato, interfaz, estados y auditoria en un archivo | C4 AS-IS y modelo de informacion | Edicion fragil, trazabilidad limitada y dependencia del espacio actual |
| Las taxonomias de unidad y cargo no tienen correspondencia aprobada | Modelo de informacion Q3 y Q7 | Falsos positivos en el cruce |
| La coordinacion con Tecnologia ocurre por mensajes o reunion | BPMN B2 y B3 | Solicitudes sin estado completo y ausencia de confirmacion de retorno |
| Los controles del tenant no estan verificados | Infraestructura, STRIDE y normatividad | El diseño no puede declararse desplegable ni conforme todavia |

## Arquitectura recomendada

La solucion usa servicios corporativos de Microsoft 365 y conectores estandar. No introduce servidores propios, software externo ni una integracion directa con el PBX.

1. **Directorio gobernado.** Microsoft Lists o una lista de SharePoint se convierte en la fuente unica. El acceso se separa por roles y se habilita historial de versiones.
2. **Conciliacion por excepciones.** Un flujo de Power Automate valida la estructura, cruza por una llave estable y produce solo novedades. Los casos ambiguos requieren decision humana.
3. **Aprovisionamiento trazable.** Cada solicitud conserva identificador, responsable, fechas, estado, respuesta autenticada y regla de escalamiento.
4. **Gobierno y seguridad.** Los cambios respetan principios, estandares, ADR, minimo privilegio, minimizacion de datos y evidencia de aprobacion.

## Beneficios esperados

| Situacion actual | Estado objetivo | Como se comprobara |
|---|---|---|
| Revision de aproximadamente 6.400 registros | Revision de excepciones | Conteo de registros fuente frente a novedades generadas |
| Ciclo bimensual o trimestral | Ciclo mensual sostenido | Historial de ejecuciones durante tres meses consecutivos |
| Solicitud verbal o por mensaje | Solicitud estructurada | Registro con ID, estado, responsable y fechas |
| Pendiente sin salida | Confirmacion o escalamiento | Cero solicitudes vencidas sin responsable ni siguiente accion |
| Excel como fuente operativa | Lista institucional gobernada | Prueba de acceso por rol, versionamiento y restauracion |
| Fallas de transferencia sin medicion | Registro basico de fallas | Linea base y seguimiento mensual |

Los beneficios son objetivos de validacion, no resultados ya medidos. Las cifras de tiempo y volumen provienen del levantamiento con el cliente.

## Roadmap

| Fase | Horizonte | Resultado visible |
|---|---|---|
| Habilitacion y control | 0 a 4 semanas | Acta de viabilidad, roles, riesgos y criterios de paso al piloto |
| Directorio gobernado | Meses 1 y 2 | Las gestoras consultan un directorio institucional con acceso de solo lectura |
| Conciliacion por excepciones | Meses 2 a 4 | Un ciclo ficticio identifica novedades sin revisar toda la fuente |
| Aprovisionamiento trazable y transferencia | Meses 4 a 6 | Una solicitud de prueba recorre creacion, respuesta, cierre y escalamiento |

Cada fase tiene una puerta de salida. La siguiente no comienza si la anterior no demuestra los controles asociados.

## Riesgos principales

Los riesgos mas altos son la exposicion de la nomina por permisos excesivos, la asociacion equivocada entre persona y extension, la falta de base juridica o retencion aprobada, una actualizacion incorrecta por respuesta tardia o duplicada y la dependencia de permisos o conectores no verificados. La matriz completa cubre Negocio, Procesos, Datos, Aplicaciones, Infraestructura, Seguridad y Gobierno TI.

El desfase de hasta 45 dias de la nomina se acepta como una limitacion externa. La confirmacion directa de Tecnologia compensa parcialmente los ingresos y movimientos que requieren extension, pero no elimina el retraso de la fuente de Desarrollo Humano.

## Gobierno

Un comite ligero con el dueño del proceso, el arquitecto responsable, Tecnologia y Seguridad o Proteccion de Datos revisa el avance cada dos semanas durante el piloto y mensualmente durante la operacion. Cierra una fase solo cuando existe evidencia de conformidad con el TO-BE y con los controles. Toda excepcion requiere una ADR antes de pasar a produccion.

Las tres metricas iniciales son:

1. Porcentaje de ciclos mensuales ejecutados y cerrados.
2. Porcentaje de cambios sensibles con validacion y trazabilidad completa.
3. Tiempo de dictamen del comite sobre ADR o excepciones.

## Proximos pasos

1. Confirmar el patrocinador y los representantes institucionales que pueden aprobar el piloto.
2. Validar licencias, permisos, DLP, base juridica, retencion y propietarios de los flujos.
3. Acordar con Tecnologia el grupo de respuesta y el plazo objetivo.
4. Preparar datos ficticios que representen duplicados, cambios de estructura, respuestas tardias y reintentos.
5. Ejecutar la fase de habilitacion antes de cargar datos reales.

## Limite de la recomendacion

La arquitectura es una propuesta academica trazada a los Talleres 0 a 8. No sustituye el concepto juridico, la evaluacion de Seguridad Informatica ni la aprobacion del administrador de Microsoft 365. El repositorio no contiene datos operativos del cliente.


## Vistas arquitectonicas finales

La [Vista de Motivacion](vistas-finales/archimate-motivacion.drawio) relaciona drivers, objetivos, principios y requisitos con los paquetes de trabajo. La [Vista de Implementacion y Migracion](vistas-finales/archimate-migracion.drawio) conecta los cuatro paquetes WP0 a WP3 con las brechas priorizadas y los estados AS-IS, piloto y TO-BE. Las fuentes editables y sus exportaciones PNG se incluyen en `vistas-finales/`.

## Hallazgos clave

- **Negocio:** el problema no es unicamente Excel; falta vigencia, conciliacion confiable y confirmacion del aprovisionamiento.
- **Datos y aplicaciones:** la arquitectura objetivo seleccionada en el Taller 7 es Power Automate con SharePoint para la conciliacion por excepciones; Power Query queda como prototipo o contingencia controlada y no como arquitectura objetivo concurrente (ADR-002).
- **Seguridad y cumplimiento:** permisos, base juridica, DLP, retencion y licencias siguen pendientes de comprobacion institucional; el riesgo residual es proyectado, no medido en produccion.
- **Implementacion:** WP0 valida la viabilidad antes de cargar datos reales, seguido del directorio gobernado, la conciliacion y el cierre trazable con Tecnologia.

## Reflexion final

La propuesta prioriza la capacidad de operar y gobernar una solucion realista antes que automatizar sin controles. Las vistas y los planes constituyen un diseño academico: la aprobacion de la Universidad, la evaluacion firmada del cliente y las reflexiones individuales no se presumen completadas.
