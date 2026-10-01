
# Informe — Modelo de Información

> Fase Information Systems Architecture (Datos) — TOGAF ADM · Corte 1 (AS-IS)
> Archivos del modelo: [`modelo-final-er.drawio`](modelo-final-er.drawio) · [`diagrama-contexto-final.drawio`](diagrama-contexto-final.drawio)

---

## 1. Alcance

Se modela la información que soporta el proceso de mantenimiento del directorio de extensiones telefónicas: las dos fuentes de datos que intervienen, sus entidades, la relación entre ellas y los flujos de información que las conectan.

---

## 2. Fuentes de datos

| Fuente | Origen | Formato | Periodicidad | Volumen |
|---|---|---|---|---|
| **Nómina institucional** | Sistema de Desarrollo Humano | Export a hoja de cálculo | Mensual, mes vencido (≈ día 15) | Planta completa |
| **Directorio de extensiones** | Mantenido manualmente | Hoja de cálculo en OneDrive | Actualización manual | ≈ 6.400 registros |
| **Plataforma de telefonía (PBX)** | Dirección de Tecnología | Sistema propio | — | Sin integración |

---

## 3. Catálogo de entidades

| Entidad | Atributos principales | Fuente | Observación |
|---|---|---|---|
| **Registro de nómina** | Id Empleado, Apellidos y Nombres, Departamento, Establecimiento, Nombre puesto/cargo, ID Posición, Dedicación, Estado HR, Clasificación empleado, Relación Organizativa, Código Directivo, Correo-E | Sistema de Desarrollo Humano | Corte a fin de mes |
| **Registro de directorio** | Unidad, Apellidos y Nombres, Cargo, Extensión, Correo, Temática, Estado | Hoja de cálculo | Sin esquema tipado ni restricciones |
| **Empleado** | Id Empleado, Correo institucional, Nombres | Ambas (parcialmente) | El Id solo existe en nómina |
| **Extensión** | Número, Estado de aprovisionamiento | PBX (fuera de alcance) | Estado no visible para la unidad |
| **Unidad** | Nombre | Implícita en ambas | **No existe catálogo maestro** |
| **Cargo** | Nombre, ¿requiere extensión? | Implícita en ambas | **Sin catálogo de derecho a extensión** |
| **Posición** | ID Posición | Nómina | Identifica la posición, no a la persona |
| **Temática** | Descripción | Directorio | Texto libre no normalizado |
| **Estado de registro** | Vigente · Pendiente por aprovisionar | Directorio | Estado sin transición de salida |

---

## 4. Análisis de llaves

La conciliación entre nómina y directorio requiere un atributo común que identifique unívocamente a una persona. El análisis de las llaves candidatas arroja lo siguiente:

| Llave candidata | En nómina | En directorio | Estabilidad | Viabilidad |
|---|---|---|---|---|
| **Id Empleado** | Sí | **No** | Alta (inmutable) | Requiere propagación previa al directorio |
| **Correo institucional** | Sí | Sí | Media | **Única llave de cruce disponible hoy** |
| **Cargo + Unidad** | Sí | Sí | Baja | **No unívoca** — cardinalidad 1:N con Empleado |
| **Apellidos y Nombres** | Sí | Sí | Baja | Homónimos, tildes, orden variable |

### 4.1. Hallazgo central

**El `Id Empleado` es la llave natural e inmutable del modelo, pero existe únicamente en la nómina.** Nunca fue incorporado al directorio, cuya construcción manual se apoyó en los datos que resultaban visibles para el operador: unidad, nombre, cargo, extensión, correo.

La consecuencia es que la única correspondencia directa entre ambas fuentes es el **correo institucional**, un atributo de estabilidad media —cambia por corrección de nombre, cambio de estado civil o duplicidad de homónimos.

### 4.2. Por qué `Cargo + Unidad` no funciona

La combinación de cargo y unidad no identifica a una persona porque su cardinalidad respecto de Empleado es **1:N**. El cliente ilustró el caso: una misma facultad puede tener seis secretarias auxiliares, todas con idéntico cargo e idéntica unidad.

Al usar esta combinación como criterio de cruce, la conciliación deja de ser un emparejamiento determinístico y se convierte en un ejercicio de aproximación, con riesgo de asociar una extensión al titular equivocado. **Esta es la causa raíz técnica de que el proceso deba ejecutarse manualmente.**

### 4.3. Recomendación

Propagar el `Id Empleado` al directorio mediante un cruce inicial único por correo institucional, e incorporarlo en adelante como llave permanente. A partir del segundo ciclo, la conciliación se realizaría sobre un atributo inmutable.

---

## 5. Problemas de calidad del dato

| ID | Problema | Evidencia | Efecto |
|---|---|---|---|
| **Q1** | `Id Empleado` almacenado como texto con ceros a la izquierda (`00000349945`) | Muestra del archivo de nómina | Las funciones de búsqueda fallan aunque el dato coincida |
| **Q2** | El archivo de nómina no admite cruce por funciones de búsqueda | Declarado por el cliente | Obliga a la revisión manual completa |
| **Q3** | Doble taxonomía organizacional: `Departamento` y `Establecimiento` en nómina frente a `Unidad` en directorio, sin tabla de correspondencia | Muestra del archivo de nómina | Falsos positivos recurrentes al detectar cambios de unidad |
| **Q4** | Columna `Unidad` duplicada en el directorio «por temas de filtro» | Declarado por el cliente | Síntoma de falta de normalización |
| **Q5** | Ausencia de restricciones de esquema: el archivo fue corrompido por transposición de filas y columnas | Incidente reportado por el cliente | Obligó a reconstruir el documento y a restringir la edición |
| **Q6** | Sin trazabilidad de cambios: no se registra quién modificó qué ni cuándo | Estructura actual del archivo | Imposible auditar el estado del directorio |
| **Q7** | La regla que determina qué cargos tienen derecho a extensión es conocimiento tácito | No documentada; reside en Desarrollo Humano | No verificable ni automatizable |
| **Q8** | El directorio reside en OneDrive; la titularidad técnica del espacio está por confirmar | Ubicación confirmada por el cliente | Riesgo de continuidad si el activo depende de un espacio individual |

### 5.1. Nota sobre Q1 y Q2

El problema Q2 —la imposibilidad de usar funciones de búsqueda— tiene su origen más probable en Q1. Un identificador almacenado como texto con ceros a la izquierda no coincide con el mismo valor almacenado como número, aunque visualmente sean idénticos. Es un comportamiento habitual en exportaciones de sistemas de recursos humanos.

Se recomienda verificar adicionalmente la presencia de espacios no separables (`U+00A0`), encabezados en múltiples filas y columnas vacías intermedias, defectos frecuentes en este tipo de archivos.

---

## 6. Modelo entidad-relación

### 6.1. Relaciones principales

| Entidad A | Cardinalidad | Entidad B | Relación |
|---|---|---|---|
| Empleado | 1 : N | Registro de nómina | Aparece en (uno por corte mensual) |
| Empleado | 1 : 0..1 | Registro de directorio | Figura en |
| Unidad | 1 : N | Registro de directorio | Agrupa |
| Unidad | 1 : N | Posición | Contiene |
| Cargo | 1 : N | Posición | Tipifica |
| Posición | 1 : 0..1 | Extensión | Tiene asignada |
| Registro de directorio | N : 1 | Estado de registro | Posee |
| Registro de directorio | N : M | Temática | Atiende |
| Departamento (nómina) | N : 0..1 | Unidad (directorio) | **Corresponde a — sin mapeo definido** |
| Establecimiento (nómina) | N : 0..1 | Unidad (directorio) | **Corresponde a — sin mapeo definido** |

### 6.2. Observación sobre las taxonomías

Las dos últimas relaciones son el punto débil del modelo. La nómina maneja dos niveles organizacionales (`Departamento` y `Establecimiento`) mientras el directorio maneja uno solo (`Unidad`), y no existe una tabla que declare la correspondencia entre ellos.

Sin ese mapeo, cualquier comparación automática de unidad produciría falsos positivos sistemáticos: toda persona cuya unidad se denomine de forma distinta en cada fuente aparecería como «cambió de unidad» en cada ciclo. Se requiere construir un **catálogo maestro de unidades** con sus alias.

---

## 7. Flujos de información

| # | Flujo | Origen | Destino | Medio | Frecuencia |
|---|---|---|---|---|---|
| 1 | Exportación de nómina | Sistema de Desarrollo Humano | Archivo de nómina | Descarga manual | Mensual, mes vencido |
| 2 | Conciliación | Archivo de nómina | Directorio de extensiones | **Manual, registro por registro** | Bimensual–trimestral |
| 3 | Consulta operativa | Directorio de extensiones | Gestoras de servicio | OneDrive, solo lectura | Continua |
| 4 | Estado de extensiones | Plataforma de telefonía | Directorio | **Sin integración** | — |
| 5 | Reporte de novedades | — | — | **No existe como artefacto** | — |

### 7.1. Observaciones

- **No existe integración entre la plataforma de telefonía y el directorio.** El estado real de una extensión solo se conoce por comunicación verbal con la Dirección de Tecnología.
- **No existe un artefacto intermedio de novedades.** La detección y la decisión ocurren simultáneamente durante la revisión manual, lo que impide auditar qué se detectó y qué se decidió en cada ciclo.
- **El directorio es simultáneamente fuente de verdad y producto final**, sin capa intermedia de validación.

---

## 8. Conclusiones

1. La ausencia de una llave común entre las dos fuentes es la causa raíz técnica de que la conciliación sea manual. Todo lo demás se deriva de ahí.
2. Los problemas de calidad Q1 a Q4 son corregibles con transformación de datos y no requieren cambiar de plataforma.
3. Los problemas Q5 a Q8 son de gobierno del dato y sí requieren mover el directorio a un almacén con esquema tipado, permisos y trazabilidad.
4. La falta de un catálogo maestro de unidades es un prerrequisito no evidente: sin él, la automatización del cruce generaría más ruido que señal.
5. La latencia de hasta 45 días es inherente al ciclo de la fuente y no se corrige con tecnología.
