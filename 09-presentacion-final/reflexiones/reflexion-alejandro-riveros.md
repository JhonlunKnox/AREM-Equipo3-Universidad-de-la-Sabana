# Reflexión individual — Alejandro Riveros

**Curso:** Arquitectura Empresarial — Universidad de La Sabana · **Equipo 3**
**Cliente:** Jefatura de Cultura de Innovación y Servicio

Lo que más me deja este proyecto es haber entendido que diseñar un estado futuro implica elegir y aceptar costos, no solo proponer herramientas. Cuando empezamos el TO-BE, mi instinto era buscar la solución más automatizada posible, incluso integrarnos con el PBX para que la extensión se verificara sola. Con el análisis del equipo vi que esa opción aumentaba permisos, plazos y riesgos sobre un sistema que ni siquiera está en nuestro alcance.

Aunque el equipo trabajó en conjunto en todas las fases, yo me enfoqué más en la arquitectura objetivo: el C4, la infraestructura, el análisis de amenazas con STRIDE y la matriz de brechas y riesgos. El reto más grande fue reconciliar vistas que se habían construido en momentos distintos. El C4 proponía Power Query y el Taller 7 había seleccionado Power Automate con SharePoint. En lugar de cambiar un diagrama para que coincidiera con el otro, aprendimos a registrar la decisión en una ADR, con alternativas y con el costo que aceptábamos. Esa forma de trabajar me pareció una de las herramientas más útiles del curso.

También cambió mi idea sobre la seguridad. Antes la veía como una revisión al final; aquí, el riesgo de exponer la nómina, la identidad de quien confirma una extensión y la retención de los registros cambiaron el diseño desde el principio: separar la nómina del directorio, enviar notificaciones con un enlace protegido y no con el archivo, y exigir una respuesta autenticada de Tecnología. De los 16 riesgos de la matriz, 11 eran críticos al inicio, y la mayoría no eran técnicos sino de permisos, responsables y aprobaciones.

Si repitiera el proyecto, haría el análisis de riesgos en paralelo con el diseño del TO-BE y no después, porque varias decisiones habrían salido antes. Evitaría asumir que una licencia o un conector existe sin confirmarlo. Para una siguiente fase considero indispensable probar la conciliación con datos ficticios que incluyan duplicados, cambios de estructura y respuestas tardías antes de pensar en producción.
