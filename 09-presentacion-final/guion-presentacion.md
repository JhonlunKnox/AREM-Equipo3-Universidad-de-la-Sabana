# Guion de presentacion

**Duracion objetivo:** 9 minutos 40 segundos  
**Maximo del taller:** 10 minutos  
**Regla de ensayo:** si una seccion excede su tiempo, se recorta contenido. No se aumenta la velocidad de habla.

## Distribucion sugerida

| Diapositiva | Tema | Tiempo | Acumulado | Presentador sugerido |
|---:|---|---:|---:|---|
| 1 | Apertura | 0:25 | 0:25 | Integrante 1 |
| 2 | Acto 1 Problema | 1:05 | 1:30 | Integrante 1 |
| 3 | Acto 2 Causa arquitectonica | 1:10 | 2:40 | Integrante 1 |
| 4 | Acto 2 Riesgos | 1:00 | 3:40 | Integrante 2 |
| 5 | Acto 3 Solucion | 1:20 | 5:00 | Integrante 2 |
| 6 | Decisiones y trade-offs | 1:00 | 6:00 | Integrante 2 |
| 7 | Acto 4 Roadmap y cambio | 1:40 | 7:40 | Integrante 3 |
| 8 | Gobernanza y metricas | 1:15 | 8:55 | Integrante 3 |
| 9 | Cierre | 0:45 | 9:40 | Integrante 3 |

Los nombres de presentador deben acordarse en el ensayo. Todos deben poder responder preguntas de cualquier seccion.

## Diapositiva 1 Apertura - 25 segundos

> Buenos dias. Somos el Equipo 3 del curso. Trabajamos con la Jefatura de Cultura de Innovacion y Servicio de la Universidad de La Sabana. La propuesta convierte el directorio de extensiones, hoy mantenido manualmente, en una fuente institucional con conciliacion por excepciones y un ciclo trazable con Tecnologia. Mostraremos el problema, la causa, la solucion y como implementarla sin prometer controles que aun no estan aprobados.

**Transicion:** Empezamos por el efecto que ve quien llama.

## Diapositiva 2 Acto 1 Problema - 1 minuto 5 segundos

> Una gestora contesta, identifica la necesidad y consulta el directorio. Si el registro esta desactualizado, la llamada llega al area equivocada o a una extension sin responsable. Para mantener el directorio, la unidad revisa cerca de 6.400 registros para encontrar entre 5 y 10 novedades. La revision conjunta tarda entre dos y tres horas y el ciclo, que deberia ser mensual, termina ocurriendo cada dos o tres meses. Ademas, las fallas de transferencia no se registran, asi que el impacto sobre el usuario no tiene linea base.
>
> Hay un limite que debemos reconocer desde el inicio. La nomina llega mes vencido y puede tener hasta 45 dias de desfase. La arquitectura reduce esfuerzo y cierra el ciclo con Tecnologia, pero no puede crear informacion que aun no existe.

**Enfatizar:** 6.400 registros frente a 5 a 10 novedades.  
**Transicion:** El problema no esta en la llamada. Esta en la forma de mantener la informacion.

## Diapositiva 3 Acto 2 Causa arquitectonica - 1 minuto 10 segundos

> El AS-IS mezcla cuatro responsabilidades en un archivo y en conversaciones. Primero, la nomina y el directorio no comparten una llave estable: el Id Empleado solo aparece en la nomina y correo es una llave transitoria. Segundo, las taxonomias de unidad y cargo no tienen una correspondencia aprobada. Tercero, el Excel funciona a la vez como base, interfaz, estado y resultado. Cuarto, Tecnologia aprovisiona la extension pero no devuelve una confirmacion estructurada.
>
> Eso produce dos bloqueos. La responsable revisa miles de filas porque no puede confiar en el cruce, y el estado pendiente no tiene una transicion de salida. Por eso automatizar solo una parte no recupera la periodicidad mensual.

**Transicion:** Estos hallazgos se consolidaron en una matriz que cubre siete dominios.

## Diapositiva 4 Acto 2 Riesgos - 1 minuto

> La matriz no vuelve a inventar riesgos. Consolida BPMN, informacion, infraestructura, STRIDE, normatividad y las brechas del Taller 7. Los riesgos criticos se concentran en cuatro temas: acceso excesivo a la nomina, una llave o respuesta que actualice a la persona equivocada, controles legales o de retencion no aprobados y dependencias del tenant que pueden impedir el despliegue.
>
> Tambien registramos riesgos de negocio y proceso: que Tecnologia no tenga un SLA de respuesta, que la unidad mantenga el Excel paralelo o que el ciclo mensual deje de ejecutarse. Aceptamos dos limites de forma explicita: una interrupcion temporal de Microsoft 365 con contingencia manual, y el desfase estructural de la nomina con una compensacion parcial mediante el canal de Tecnologia.

**Transicion:** La solucion responde a estos riesgos en paquetes dependientes, no como una lista de herramientas.

## Diapositiva 5 Acto 3 Solucion - 1 minuto 20 segundos

> La arquitectura objetivo tiene cuatro paquetes. WP0 habilita el piloto: permisos, licencias, DLP, base juridica, retencion y responsables. WP1 crea el directorio gobernado en SharePoint o Lists, con catalogos, grupos e historial. WP2 ejecuta la conciliacion con Power Automate y produce excepciones. Los casos ambiguos conservan validacion humana. WP3 crea solicitudes trazables hacia Tecnologia, con estado, respuesta autenticada, vencimiento y escalamiento.
>
> Todo permanece dentro de Microsoft 365 con conectores estandar, y eso ya lo comprobamos con la cliente. El PBX no se integra y sigue bajo Tecnologia. La nomina se separa del directorio y las notificaciones llevan un ID y un enlace protegido, no el archivo completo.

**Enfatizar:** WP1 es prerequisito de WP2 y WP3.  
**Transicion:** Dos decisiones explican por que elegimos esta realizacion y no otra.

## Diapositiva 6 Decisiones y trade-offs - 1 minuto

> La primera ADR reemplaza el Excel por Lists como fuente unica. Aceptamos que Lists no es una base relacional completa y exige validaciones, a cambio de continuidad, permisos e historial institucional.
>
> La segunda ADR resuelve una tension entre talleres. El C4 proponia Power Query, pero el Taller 7 selecciono Power Automate con SharePoint. Power Query queda como contingencia o prototipo. Sacrificamos simplicidad local a cambio de ejecucion en el tenant, monitoreo y menor dependencia del equipo de una persona.
>
> La tercera ADR mantiene el PBX fuera del alcance y usa una confirmacion humana autenticada. Sacrificamos verificacion tecnica automatica para mantener una solucion viable y segura.

**Transicion:** Estas decisiones se implementan en fases con resultados que el negocio puede ver.

## Diapositiva 7 Acto 4 Roadmap y gestion del cambio - 1 minuto 40 segundos

> La fase cero dura hasta cuatro semanas y decide si existe un camino seguro hacia el piloto. La fase uno, durante los meses uno y dos, crea el directorio gobernado. La fase dos, entre los meses dos y cuatro, prueba la conciliacion por excepciones con datos ficticios y escenarios de error. La fase tres, entre los meses cuatro y seis, cierra el ciclo con Tecnologia y transfiere la operacion.
>
> El cambio no es solo tecnico. La profesional de Experiencia y Servicio puede mantener el Excel paralelo por seguridad. Las gestoras pueden seguir usando enlaces antiguos. Tecnologia puede responder por fuera del flujo para evitar trabajo administrativo. Por eso cada fase incluye comunicacion y capacitacion, y deja un resultado visible. Una gestora comprueba el directorio de solo lectura. La responsable valida las excepciones. El dueño del proceso comprueba que una solicitud se cierra o escala.
>
> Una fase no avanza por porcentaje completado. Avanza cuando supera su puerta de calidad y alguien fuera de TI confirma el resultado.

**Transicion:** El gobierno mantiene esas puertas despues de que termine el proyecto academico.

## Diapositiva 8 Gobernanza y metricas - 1 minuto 15 segundos

> Proponemos un comite ligero con el dueño del proceso, el arquitecto responsable, Tecnologia y Seguridad o Proteccion de Datos cuando corresponda. Se reune quincenalmente durante el piloto y mensualmente al iniciar operacion. Toda excepcion a un estandar requiere una ADR.
>
> Tres metricas muestran si funciona. M1 mide ciclos mensuales cerrados sobre ciclos programados y actua por debajo de 90 por ciento. M2 mide cambios sensibles con solicitud, validacion, autor y fechas, y su umbral es 100 por ciento. M3 mide al propio comite: dias habiles entre propuesta y dictamen. No inventamos una linea base para M3; empieza con este registro y actua si supera 10 dias habiles.

**Transicion:** Cerramos con la decision que pedimos al cliente y al panel.

## Diapositiva 9 Cierre - 45 segundos

> Recomendamos autorizar un piloto limitado con datos ficticios, no una implantacion directa. Parte de la fase cero ya avanzo: la cliente comprobo en su cuenta que Power Automate tiene los conectores de SharePoint, Excel Online, Outlook y Teams sin licencia premium, y que puede crear sitios de SharePoint. Antes de datos reales faltan DLP, base juridica, retencion, grupo de Tecnologia y propietarios de los flujos.
>
> La propuesta reduce la revision a excepciones, cierra el estado pendiente y deja reglas para cambiar la arquitectura despues. Su limite principal sigue siendo la nomina mes vencido. La decision responsable es comprobar primero que la Universidad puede gobernar y sostener la solucion. Con eso cerramos y quedamos atentos a sus preguntas.

## Ensayo

- Ensayo 1: leer el guion y medir cada seccion.
- Ensayo 2: presentar sin leer, manteniendo los mensajes principales.
- Ensayo 3: intercambiar una pregunta de defensa entre integrantes.
- Ensayo final: usar las diapositivas, detener al llegar a 9:40 y recortar cualquier exceso.

