# Reflexión individual — Juan Pablo Luna Zuleta

**Curso:** Arquitectura Empresarial — Universidad de La Sabana · **Equipo 3**
**Cliente:** Jefatura de Cultura de Innovación y Servicio

El principal aprendizaje que me deja el proyecto es que una arquitectura empresarial no se demuestra por la cantidad de diagramas, sino por la coherencia entre el problema del negocio, las decisiones de diseño y la capacidad real de llevarlas a operación. Al inicio yo veía el caso como un problema de Excel: un archivo grande y desordenado que había que automatizar. El levantamiento del AS-IS me hizo cambiar esa lectura.

Trabajamos los tres de forma conjunta en todos los talleres, pero donde más me involucré fue en entender y modelar la situación actual: la ficha de caracterización, el BPMN y el modelo de información, y luego en mantener el repositorio coherente de corte a corte. Lo difícil no fue dibujar el proceso, sino aceptar lo que mostraba. Cuando cruzamos el modelo de información con el BPMN vimos que el Id Empleado solo existe en la nómina y que el correo es una llave transitoria. Ahí entendí que, sin una llave estable, cualquier automatización solo produce errores más rápido. Esa conclusión terminó guiando la conciliación por excepciones y la cuarentena de casos ambiguos.

También aprendí a separar lo que la arquitectura puede resolver de lo que no. La nómina llega mes vencido, con hasta 45 días de desfase, y ningún diseño cambia eso. Decirlo de forma explícita en la presentación me pareció más honesto y más útil para la cliente que prometer un directorio siempre al día.

Otra lección fue el valor de la documentación como herramienta de trabajo y no como requisito. Mantener la trazabilidad entre talleres nos permitió detectar contradicciones, como la diferencia entre Power Query en el C4 y Power Automate en el Taller 7, y resolverlas con una ADR en lugar de dejarlas escondidas.

Si repitiera el proyecto, validaría antes con la cliente las condiciones del tenant de Microsoft 365. Esa verificación llegó tarde y varias decisiones quedaron condicionadas mientras tanto. Para una siguiente fase considero indispensable ejecutar la fase cero completa: permisos, DLP, base jurídica y responsables, antes de tocar cualquier dato real.
