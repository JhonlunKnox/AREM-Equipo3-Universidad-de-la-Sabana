# Vistas ArchiMate finales

## Vista de Motivacion

La vista conecta los drivers del cliente, sus goals, los principles confirmados por el plan de gobernanza, los requirements y los Work Packages que los realizan. Los elementos de motivacion usan lila y los paquetes de trabajo usan naranja, de acuerdo con la guia de notacion del curso.

Archivos:

- [`archimate-motivacion.drawio`](archimate-motivacion.drawio): fuente editable.
- [`archimate-motivacion.png`](archimate-motivacion.png): exportacion para consulta.

## Vista de Migracion

La vista representa el paso desde el Plateau AS-IS hasta el TO-BE mediante cuatro Work Packages. Cada paquete se asocia con los gaps que cierra y realiza un Plateau estable. Los gaps se conectan mediante Association, y los Work Packages realizan los Plateaus mediante Realization.

Archivos:

- [`archimate-migracion.drawio`](archimate-migracion.drawio): fuente editable.
- [`archimate-migracion.png`](archimate-migracion.png): exportacion para consulta.

## Convenciones

- Motivacion: lila.
- Implementacion y Migracion: naranja.
- Influence: flecha discontinua.
- Realization: linea discontinua con triangulo vacio.
- Association: linea simple sin flecha.

Las vistas usan estereotipos explicitos y atributos `archimateType` dentro del XML de draw.io. Deben abrirse en diagrams.net para editarse.


## Alcance de principios representados

La vista de Motivacion sintetiza cuatro principios fundacionales (P1-P4) para mantener legible la relacion con los objetivos y requisitos. El plan de gobernanza documenta tambien P5 (Automatizar solo lo verificable) y P6 (Falla visible y recuperable) como principios transversales a todos los WP. Las fuentes `.drawio` y `.png` muestran la misma version del modelo.
