# Arquitectura Empresarial — Universidad de La Sabana

**Equipo:** Adictos al Azucar · Juan Pablo Luna Zuleta, Alejandro Riveros, Martín Ortega
**Curso:** Arquitectura Empresarial — Universidad de La Sabana
**Cliente:** Jefatura de Cultura de Innovación y Servicio — Dirección de Desarrollo Estratégico

---

## En una frase

Rediseñamos cómo la Jefatura de Cultura de Innovación y Servicio mantiene actualizado su directorio de extensiones telefónicas, para que las llamadas de estudiantes, aspirantes y visitantes lleguen siempre a la persona correcta.

---

## El problema

Las gestoras que atienden el teléfono de la Universidad usan un directorio de extensiones para redirigir cada llamada al área competente. Ese directorio se mantiene a mano: cada mes hay que comparar cerca de 6.400 registros contra la nómina para encontrar entre 5 y 10 novedades, y luego coordinar con Tecnología en reuniones que muchas veces se cancelan.

El resultado es que el directorio se actualiza cada dos o tres meses en lugar de cada mes. Cuando eso pasa, la llamada se transfiere a un área equivocada, o a una extensión donde ya no trabaja nadie y nadie contesta.

---

## Lo que proponemos

- Que Power Query compare la nómina y el directorio después de una actualización guiada por la responsable, para revisar excepciones clasificadas en lugar de miles de filas. La publicación de novedades y los cambios sensibles requieren validación humana.
- Que exista un canal directo con Tecnología para pedir una extensión y, sobre todo, para que Tecnología avise de vuelta cuando ya quedó lista. Hoy ese aviso de retorno no existe.
- Que el directorio deje de ser un archivo suelto y pase a ser una fuente de información institucional, con estructura, permisos e historial de cambios.
- Usar la suite Microsoft existente y conectores estándar. La implantación depende de confirmar licencias efectivas, permisos, políticas DLP y aprobación institucional; el equipo no ha verificado esas condiciones en el tenant.

---

## Cómo se implementa

Ver el [Resumen Ejecutivo](resumen-ejecutivo.md) — ahí está el detalle de beneficios esperados, fases de implementación y tiempos, en un solo documento pensado para el negocio, no para el equipo técnico.

---

## Si quiere ver el detalle técnico completo

Todo el análisis que sustenta esta propuesta está documentado carpeta por carpeta, siguiendo el método usado durante el proyecto:

| Carpeta | Qué contiene | Estado |
|---|---|---|
| `00-preliminary-vision/` | Contexto del cliente y visión de la solución | ✅ Corte 1 |
| `01-bpmn/` | Cómo funciona hoy el proceso de negocio analizado | ✅ Corte 1 |
| `02-modelo-informacion/` | Qué información maneja el negocio y cómo fluye | ✅ Corte 1 |
| [`03-arquitectura-c4/`](03-arquitectura-c4/informe.md) | C4 AS-IS/TO-BE y esquema de datos objetivo | 🟡 Corte 2 — preparado; sustentación pendiente |
| [`04-infraestructura/`](04-infraestructura/informe.md) | Infraestructura y vista integrada ArchiMate | 🟡 Corte 2 — preparado; sustentación pendiente |
| [`05-seguridad-stride/`](05-seguridad-stride/informe.md) | Amenazas, priorización y riesgo residual proyectado | 🟡 Corte 2 — preparado; sustentación pendiente |
| [`06-normatividad/`](06-normatividad/informe.md) | Checklist normativo y brechas por validar | 🟡 Corte 2 — preparado; sustentación pendiente |
| `07-opportunities-solutions/` | La solución propuesta y qué brechas cierra | 🔜 Corte 3 |
| `08-integracion-vistas/` | Cómo se conecta todo lo anterior en una sola arquitectura | 🔜 Corte 3 |
| `09-presentacion-final/` | Presentación ejecutiva, plan de implementación y gobernanza | 🔜 Corte 3 |

### Alcance académico y validación del cliente

Según el [cronograma confirmado del profesor](https://github.com/CesarAVegaF312/AREM-Proyecto-Cliente#4-cronograma-del-semestre-confirmado), el Corte 2 comprende las carpetas **03, 04, 05 y 06**, con sustentación el **3 de octubre de 2026**. La carpeta **07** se trabaja después de esos diagnósticos: su taller está previsto para el 10 de octubre y su entrega para el Corte 3. Esta secuencia actualiza la distribución del enunciado inicial; no se omite 07 por falta de análisis.

El estado académico de la tabla no equivale a aprobación del cliente, despliegue o cumplimiento institucional. Esas validaciones siguen abiertas. Última revisión del repositorio: **2 de octubre de 2026**.

Artefactos complementarios: [esquema de datos TO-BE](03-arquitectura-c4/esquema-datos-to-be.md), [vista ArchiMate](04-infraestructura/vista-integrada-archimate.drawio) y [criterios de priorización STRIDE](05-seguridad-stride/criterios-priorizacion.md).

---

## Nota sobre los datos

Este repositorio **no contiene** el directorio de extensiones ni archivos de nómina. Ambos incluyen datos personales de colaboradores de la Universidad y su tratamiento se rige por la Ley 1581 de 2012. Los modelos y ejemplos publicados aquí usan datos ficticios o estructuras sin contenido real.

El [`.gitignore`](.gitignore) excluye carpetas de datos reales y archivos de nómina identificables por su nombre. Esto no detecta todos los datos sensibles ni retira archivos ya versionados: antes de cada publicación se revisan archivos, capturas y metadatos. La restricción se refiere a datos operativos del cliente, no a los créditos o contactos académicos del equipo.

---

## Contacto

Juan Pablo Luna Zuleta — jplz39333@gmail.com — GitHub: [JhonlunKnox](https://github.com/JhonlunKnox)
