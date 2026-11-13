# Respuesta a la revision docente — puntos 2 y 7

## Punto 2 — Matriz de riesgos

Se agrega [matriz-riesgos.xlsx](matriz-riesgos.xlsx), con las hojas `Resumen`, `Matriz de riesgos` y `Escalas y fuentes`. La matriz tiene 16 escenarios en siete dominios: Negocio, Procesos, Datos, Aplicaciones, Infraestructura, Seguridad y Gobierno TI.

Cada escenario contiene causa, efecto, impacto, probabilidad, valor inicial por formula, tratamiento, control, responsable, Work Package, riesgo residual y estado de evidencia. El archivo diferencia los riesgos aceptados **como propuesta** de cualquier autorizacion institucional real.

**Correcciones efectuadas directamente en el XLSX (hoja Matriz de riesgos):**

- `K7` (R-03): la revisión del ciclo mensual se vincula a `M1`, no a `M2`.
- `L8` (R-04): estrategia principal `Reducir`; la transición obligatoria y el escalamiento permanecen en los controles.
- `L11` (R-07): `Reducir` en vez de `Transferir`, porque el soporte y la contingencia no transfieren contractualmente el riesgo.
- `L17` (R-13): estrategia principal `Reducir`, asociada al gate de conformidad y a la validación de controles antes de cualquier dato real.

## Punto 7 — Vistas finales ArchiMate

Se publican ambos modelos en formato fuente `.drawio` y vista `.png`:

1. [Motivacion](vistas-finales/archimate-motivacion.drawio) y [PNG](vistas-finales/archimate-motivacion.png): drivers, goals, principles, requirements/constraints y los WP.
2. [Implementacion y Migracion](vistas-finales/archimate-migracion.drawio) y [PNG](vistas-finales/archimate-migracion.png): Plateaus, Gaps y WP0–WP3 asociados al roadmap.

El modelo visual de motivación muestra los cuatro principios fundacionales P1–P4; el [plan de gobernanza](plan-gobernanza.md) complementa esos principios con P5 (Automatizar solo lo verificable) y P6 (Falla visible y recuperable). Se conservan las relaciones y la notación del archivo editable para mantener las imágenes y los diagramas coherentes.

## Verificacion adicional

Para el comite: las fases son estimaciones y no fechas de implementacion aprobadas. La presentacion se debe exportar a PDF y se deben validar sus rótulos/tiempos de ensayo contra el guion antes de la entrega definitiva.
