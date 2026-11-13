# Preguntas de defensa

## 1 Por que Power Automate si el diseño anterior usaba Power Query

El Taller 7 evaluo alternativas y selecciono Power Automate con SharePoint para la arquitectura objetivo. Esa opcion ejecuta el flujo dentro del tenant, conserva historial de ejecuciones y reduce la dependencia del equipo de una persona. Power Query sigue siendo util como prototipo o contingencia manual. La decision queda formalizada en ADR-002 para eliminar la contradiccion entre vistas.

**Evidencia:** integracion de vistas, decision del Taller 7 y procedimiento de cambios.

## 2 Como evitan que el flujo cambie a la persona equivocada

El Id Empleado se conserva como texto y se convierte en llave estable despues de una migracion inicial controlada. Correo se usa solo si existe una coincidencia unica. Duplicados, ausencia de llave, taxonomias no mapeadas o cambios masivos detienen la aplicacion y crean una excepcion para revision humana. Los reintentos usan una clave idempotente.

**Evidencia:** modelo de informacion, contrato de datos, STRIDE T-02 y plan de implementacion.

## 3 Por que no integran directamente el PBX

El PBX esta fuera del alcance, es administrado por Tecnologia y no se ha confirmado una interfaz aprobada. Una integracion directa aumentaria permisos, costo, plazo y riesgo. ADR-003 mantiene la actuacion tecnica en Tecnologia y exige una respuesta autenticada ligada a una solicitud. El trade-off es depender de una confirmacion humana, compensado con vencimiento y escalamiento.

## 4 La arquitectura elimina el desfase de 45 dias

No. La nomina llega mes vencido y es el unico insumo de movimientos de personal. La automatizacion reduce el esfuerzo de conciliacion, pero no puede adelantar la fuente. El canal con Tecnologia compensa parcialmente ingresos o asignaciones conocidos antes del corte. La mejora estructural requeriria que Desarrollo Humano reporte novedades de forma mas oportuna.

## 5 Como saben que Microsoft 365 permite esta solucion

Todavia no se afirma. La fase cero confirma permisos, licencias, conectores, DLP, propietarios y retencion antes de construir. Si el tenant no permite el diseño, el comite registra una nueva ADR y evalua alternativas. La presentacion propone un piloto condicionado, no un despliegue aprobado.

## 6 Que pasa si Power Automate falla

El flujo debe alertar, conservar el ultimo estado valido y permitir contingencia manual. Cada flujo tiene propietario y copropietario institucional. Se prueban cambio de esquema, ausencia de archivo, reintento y falla de notificacion. Una falla silenciosa no es conforme con P6.

## 7 Como cumplen la Ley 1581

La arquitectura aplica finalidad, minimizacion, acceso restringido, trazabilidad y separacion de la nomina. Sin embargo, no declara cumplimiento definitivo. La Universidad debe confirmar base juridica, roles, retencion, atencion de derechos y procedimiento de incidentes. Antes de datos reales, Seguridad y Proteccion de Datos deben aprobar los controles.

## 8 Por que Lists y no Dataverse o una base de datos

Lists encaja con la plataforma existente, el volumen declarado y la capacidad de mantenimiento de la unidad. Dataverse o una base a medida ofrecen controles relacionales superiores, pero agregan licenciamiento, complejidad y aprobaciones. El costo aceptado es implementar validaciones que Lists no impone por si sola. Si el volumen o las relaciones superan las pruebas, la decision se revisa mediante ADR.

## 9 Que riesgo aceptan de forma explicita

Se acepta la interrupcion temporal de Microsoft 365 con procedimiento manual y reintento, porque construir infraestructura paralela no es proporcional al proceso mensual. Tambien se acepta el desfase estructural de la nomina como restriccion externa, con compensacion parcial por el canal de Tecnologia. Ambas aceptaciones se revisan si cambia el impacto o la frecuencia.

## 10 Como evitan que el gobierno bloquee al equipo

M3 mide los dias habiles entre propuesta y dictamen. El umbral es 10 dias habiles y ninguna decision debe superar 15 dias sin estado y responsable. Los cambios estandar siguen una ruta operativa; solo las decisiones costosas, dificiles de revertir o que cambian principios requieren ADR y comite completo.

## 11 Que demuestra que la gestion del cambio funciono

Cada fase deja un resultado visible que comprueba alguien fuera de TI. Una gestora prueba la consulta, la profesional valida las excepciones y el dueño del proceso verifica el cierre de una solicitud. La operacion se acepta solo si la unidad ejecuta un ciclo sin el equipo academico.

## 12 Por que la matriz tiene riesgos de negocio y gobierno

El mayor peligro no es solo tecnico. La solucion puede fracasar si Tecnologia no responde, si la unidad mantiene el Excel paralelo, si el comite tarda demasiado o si no existe base juridica. La guia exige siete dominios y trazabilidad a los talleres previos. Por eso la matriz integra Negocio, Procesos, Datos, Aplicaciones, Infraestructura, Seguridad y Gobierno TI.

