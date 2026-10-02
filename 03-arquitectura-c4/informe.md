# Informe — Arquitectura de Aplicaciones (C4)

**Fase:** Information Systems Architecture — Aplicaciones · TOGAF ADM · Corte 2  
**Modelos:** [C1 — Contexto](c1-contexto-final.drawio) · [C2 — Contenedores](c2-contenedores-final.drawio)

**Complementos:** [Esquema de datos TO-BE](esquema-datos-to-be.md) · [Vista integrada ArchiMate](../04-infraestructura/vista-integrada-archimate.drawio)

## 1. Decisión arquitectónica

La arquitectura objetivo no introduce una aplicación desarrollada a medida. Organiza el proceso sobre servicios de Microsoft 365 ya disponibles para la Universidad y separa cuatro responsabilidades que hoy están mezcladas en un único archivo:

1. **Recepción y conservación del insumo:** biblioteca institucional de SharePoint para la nómina mensual.
2. **Preparación y conciliación:** libro controlado de Excel con Power Query, operado mediante actualización guiada.
3. **Fuente institucional del directorio:** Microsoft Lists/SharePoint con esquema, permisos e historial.
4. **Gestión del ciclo de aprovisionamiento:** lista de solicitudes y flujos de Power Automate con notificación, confirmación y escalamiento.

La propuesta base usa únicamente conectores estándar. La automatización total de la transformación de la nómina se mantiene como opción condicionada a la validación de licencias, políticas del tenant y capacidades reales del archivo fuente. El diseño no asume conectores premium, RPA, gateways ni desarrollos locales.

## 2. Alcance y trazabilidad

Este trabajo toma como entrada:

- Los procesos y bloqueos documentados en [`01-bpmn/`](../01-bpmn/informe.md).
- Las entidades, llaves y problemas de calidad documentados en [`02-modelo-informacion/`](../02-modelo-informacion/informe.md).
- Los requerimientos RQ1 a RQ4 definidos en [`00-preliminary-vision/vision.md`](../00-preliminary-vision/vision.md).

El levantamiento y el cuestionario de seguimiento con Johanna Molina confirmaron que la nómina contiene `Id Empleado` y correo institucional, el directorio está en OneDrive y tiene aproximadamente 6.400 filas, y el volumen habitual es de 5 a 10 cambios mensuales. La nómina se publica a mes vencido, aproximadamente el día 15, y es el único insumo de movimientos de personal; Tecnología aporta por separado el estado de aprovisionamiento de las extensiones. La coordinación actual con José Roberto ocurre por Teams o en reunión.

La cliente definió dos capacidades objetivo: (1) detectar mediante el cruce ingresos, retiros, cambios de cargo y posiciones vacantes, con revisión humana de casos ambiguos, y (2) permitir notificaciones en ambos sentidos entre Experiencia y Servicio y Tecnología. También autorizó el uso académico del nombre de la Universidad y del área, y estableció que la solución debe permanecer dentro de la suite Microsoft. Indicó que dispone de licenciamiento ordinario; cualquier función o conector premium debe comprobarse antes de incluirlo.

Quedan fuera del alcance:

- Modificar el sistema de Desarrollo Humano.
- Integrarse directamente con la plataforma PBX.
- Aprovisionar o liberar extensiones desde la solución.
- Publicar datos reales en GitHub.
- Confirmar licencias, permisos o políticas internas sin evidencia del administrador del tenant.

## 3. Arquitectura de aplicaciones AS-IS

### 3.1. Inventario

| Aplicación o artefacto | Propietario | Función actual | Interacción | Limitación principal |
|---|---|---|---|---|
| Sistema de Desarrollo Humano | Desarrollo Humano | Origina la nómina institucional | Exportación mensual a Excel | La solución no controla su formato ni su latencia |
| Archivo de nómina | Desarrollo Humano / Experiencia y Servicio | Insumo para identificar novedades | Descarga manual | Cruce reportado como fallido, causa técnica por verificar; entrega mes vencido |
| Directorio de extensiones en Excel | Experiencia y Servicio | Fuente consultada por las gestoras | Edición manual; lectura desde OneDrive | Sin esquema tipado, llave estable ni flujo de estados |
| OneDrive | Titularidad técnica por confirmar | Almacena y comparte el directorio | Enlace de solo lectura | El activo permanece ligado a un espacio de trabajo cuya continuidad debe validarse |
| Microsoft Teams | Experiencia y Servicio / Tecnología | Coordina reuniones y solicitudes | Mensajes y reuniones | La solicitud no queda como registro estructurado |
| Outlook | Universidad | Comunicación institucional | Correos eventuales | No constituye fuente de verdad del proceso |
| PBX | Dirección de Tecnología | Mantiene el estado real de las extensiones | Operación manual por Tecnología | Sin integración ni retorno hacia el directorio |

### 3.2. Hallazgos del C1 AS-IS

- El “sistema” de gestión del directorio es una combinación de archivos y coordinación humana, no una aplicación con límites y responsabilidades definidos.
- El archivo del directorio cumple al mismo tiempo los roles de base de datos, interfaz de consulta, registro de estados y producto final.
- La nómina entra al proceso como archivo, sin contrato de datos ni validación automática.
- Los cargos no identifican de manera única a una persona: una misma unidad puede tener varias posiciones con el mismo nombre, por lo que el cruce no puede depender solo de unidad y cargo.
- Teams transporta conversaciones, pero no conserva el estado completo de una solicitud.
- El PBX contiene la verdad técnica sobre la extensión, pero esa verdad no regresa de manera estructurada al proceso administrativo.

## 4. Arquitectura objetivo TO-BE

### 4.1. Principios de diseño

| ID | Principio | Aplicación en el diseño |
|---|---|---|
| P1 | Usar tecnología corporativa existente | SharePoint, Lists, Excel, Power Query, Power Automate, Teams y Outlook |
| P2 | Operación mantenible por la unidad | No se propone código propio ni infraestructura local |
| P3 | Fuente única de verdad | El directorio vigente reside en una lista institucional |
| P4 | Estado explícito y trazable | Cada solicitud tiene identificador, responsable, fecha y transición de estado |
| P5 | Mínimo privilegio | Gestoras consultan; Experiencia y Servicio administra; Tecnología responde solicitudes asignadas |
| P6 | Falla visible | Los errores de flujo, solicitudes vencidas y registros sin llave generan una excepción revisable |

### 4.2. Catálogo de contenedores

| ID | Contenedor | Tecnología | Responsabilidad | Datos principales |
|---|---|---|---|---|
| C01 | Biblioteca de entrada | SharePoint Online | Recibir y versionar la nómina mensual | Archivo de nómina y metadatos de carga |
| C02 | Libro de conciliación | Excel + Power Query | Normalizar llaves, cruzar fuentes y producir excepciones | Nómina preparada, directorio y catálogo de unidades |
| C03 | Directorio institucional | Microsoft Lists / lista de SharePoint | Mantener la versión vigente consultada por las gestoras | Empleado, unidad, cargo, extensión, estado y temáticas |
| C04 | Catálogos maestros | Microsoft Lists | Administrar equivalencias de unidades y regla de cargos con extensión | Unidades, alias, cargos y elegibilidad |
| C05 | Registro de novedades | Microsoft Lists | Conservar cada diferencia detectada y su decisión | Tipo de novedad, evidencia, estado y responsable |
| C06 | Solicitudes de aprovisionamiento | Microsoft Lists | Controlar el ciclo con Tecnología | Solicitud, extensión, fechas, estado, confirmación y observaciones |
| C07 | Automatización | Power Automate | Notificar, solicitar confirmación, actualizar estados y escalar vencimientos | Eventos y estados de C05 y C06 |
| C08 | Interfaz de trabajo | Teams + Outlook | Entregar avisos y permitir responder aprobaciones o confirmaciones | Notificaciones y respuestas institucionales |
| C09 | Identidad | Microsoft Entra ID | Autenticar usuarios y soportar los grupos de acceso | Identidades y grupos institucionales |

### 4.3. Flujo mensual de conciliación

1. Desarrollo Humano publica la nómina y la responsable coloca una copia en la biblioteca de entrada.
2. SharePoint registra fecha, cargador y versión del archivo.
3. La responsable actualiza el libro de Power Query.
4. Power Query conserva `Id Empleado` como texto, limpia espacios, normaliza correo y aplica el catálogo de unidades.
5. La consulta compara la nómina preparada contra el directorio por `Id Empleado`. Durante la transición inicial usa correo institucional y exige revisión de coincidencias ambiguas.
6. El resultado se limita a excepciones: ingreso, retiro por confirmar, cambio de cargo, cambio de unidad, vacancia por confirmar, falta de llave o diferencia no clasificable. Una posición vacante no equivale a una extensión liberada; sin llave de posición y evidencia suficiente no se confirma vacancia ni se ordena liberación.
7. La responsable registra las excepciones en Lists mediante publicación controlada, con decisión y responsable. Power Query no escribe directamente en las listas en el diseño base. Las aprobadas que requieren actuación técnica crean una solicitud; las ambiguas conservan el estado de revisión.
8. La actualización del directorio ocurre después de la validación humana correspondiente, no por una escritura automática sin control.

### 4.4. Flujo de aprovisionamiento

1. Una solicitud nueva recibe un identificador único y estado `Creada`.
2. Power Automate la asigna al grupo de aprovisionamiento y envía una notificación por Teams/Outlook.
3. Tecnología responde `Aprovisionada`, `Liberada`, `Requiere información` o `Rechazada`, con observación obligatoria cuando aplique.
4. El flujo registra usuario y fecha de respuesta.
5. Una respuesta exitosa actualiza el estado administrativo solo si la solicitud sigue vigente, la identidad está autorizada y titular/extensión son coherentes. Una respuesta tardía, duplicada o contradictoria no sobreescribe una asignación posterior; queda para revisión. Se distingue respuesta técnica de aplicación efectiva del cambio.
6. Las solicitudes sin respuesta dentro del plazo acordado se marcan `Vencida` y se escalan. El plazo no se fija en este documento porque debe acordarse con Tecnología.

Este flujo implementa la comunicación bidireccional solicitada por la cliente: Experiencia y Servicio reporta la novedad y Tecnología devuelve el resultado del aprovisionamiento o liberación sin depender de una reunión.

Tecnología también puede iniciar un aviso de asignación/liberación antes del corte mensual. Se registra con origen Tecnología y la unidad valida su incorporación al directorio. No se presume que el PBX envíe ese aviso automáticamente.

### 4.5. Estados propuestos

```text
Creada -> Enviada a Tecnología -> En gestión -> Aprovisionada/Liberada
                      |                |
                      |                +-> Requiere información -> En gestión
                      +-> Rechazada
                      +-> Vencida -> Escalada -> En gestión
```

Ninguna solicitud puede permanecer en un estado “pendiente” sin responsable, fecha de última actuación y siguiente acción definida.

### 4.6. Contrato de datos

El [esquema TO-BE](esquema-datos-to-be.md) detalla columnas tipadas, campos obligatorios, claves y relaciones de C01 y C03–C06. Diferencia empleado, posición y extensión; propone llaves estables para reintentos y reglas de transición. Las relaciones Lookup y las vistas de SharePoint no sustituyen todas las restricciones de una base relacional ni la seguridad por campo: se requieren validaciones y pruebas de acceso. El contrato es preliminar hasta confirmar el archivo real y los catálogos del cliente.

## 5. Matriz aplicaciones versus procesos

| Proceso | RH | SharePoint entrada | Power Query | Directorio Lists | Novedades | Solicitudes | Power Automate | Teams/Outlook | PBX |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Publicar nómina | R | C | — | — | — | — | — | — | — |
| Preparar insumo | — | R | R | C | — | — | C | — | — |
| Conciliar fuentes | — | C | R | C | C | — | — | — | — |
| Validar novedades | — | C | C | C | R | C | C | C | — |
| Solicitar aprovisionamiento | — | — | — | C | C | R | R | R | — |
| Ejecutar aprovisionamiento | — | — | — | — | — | C | C | C | R |
| Confirmar resultado | — | — | — | C | C | R | R | R | C |
| Consultar directorio | — | — | — | R | — | — | — | C | — |

**R:** responsabilidad principal. **C:** consulta, entrada o apoyo.

## 6. Trazabilidad de requerimientos

| Requerimiento | Contenedores que lo realizan | Evidencia esperada |
|---|---|---|
| RQ1 — Conciliación y reporte de novedades | C01, C02, C03, C04, C05 | Ejecución de prueba con datos ficticios y reporte de excepciones |
| RQ2 — Solicitud estructurada a Tecnología | C06, C07, C08 | Solicitud con ID, estado, responsable y notificación |
| RQ3 — Confirmación de retorno | C06, C07, C08 | Respuesta registrada y transición automática de estado |
| RQ4 — Llave estable | C02, C03 | `Id Empleado` conservado como texto y control de registros sin llave |

## 7. Requerimientos no funcionales

| Atributo | Criterio propuesto |
|---|---|
| Seguridad | Acceso por grupos institucionales y mínimo privilegio |
| Auditabilidad | Versiones de listas y archivos; ID de solicitud; usuario y fecha de cada transición |
| Mantenibilidad | Catálogos editables por la unidad, sin modificar consultas o flujos para cambios ordinarios |
| Portabilidad | Exportación de listas y documentación de configuración |
| Disponibilidad | Proceso mensual; fallas de automatización generan alerta y no borran el estado anterior |
| Calidad del dato | Llave como texto, validación de duplicados, equivalencias de unidad y estados controlados |
| Privacidad | Sin datos reales en GitHub; acceso operativo limitado a personal autorizado |

## 8. Decisiones y alternativas

| Decisión | Alternativa descartada | Razón |
|---|---|---|
| Lists/SharePoint como fuente institucional | Excel en OneDrive | Lists aporta esquema, permisos, historial y continuidad institucional |
| Power Query para el cruce base | Revisión manual de 6.400 registros | Reduce la revisión a excepciones sin introducir software externo |
| Flujo de estados en lista | Mensajes y reuniones sin registro | Permite seguimiento, vencimiento y confirmación |
| Conectores estándar | Conectores premium o desarrollo propio | Respeta la restricción de licenciamiento y aprobación |
| Validación humana antes de cambios sensibles | Actualización ciega del directorio | Evita propagar errores de calidad o coincidencias ambiguas |

## 9. Validaciones pendientes con el cliente

- Permiso para crear un sitio, biblioteca y listas de SharePoint.
- Licencias efectivas de las personas que crearán, poseerán y ejecutarán los flujos.
- Políticas DLP del tenant y clasificación permitida de los conectores.
- Autorización para incorporar `Id Empleado` al directorio; su presencia en la nómina ya fue confirmada.
- Catálogo oficial de unidades y regla de cargos con derecho a extensión.
- Grupo institucional de Tecnología que reemplazará o formalizará la coordinación actual por Teams/reunión con José Roberto.
- Plazo de atención y ruta de escalamiento.
- Compatibilidad real del archivo de nómina con el libro de Power Query.

Hasta completar estas validaciones, el modelo es una **arquitectura objetivo propuesta**, no una descripción de componentes ya implantados.

## 10. Conclusión

La arquitectura separa datos, transformación y coordinación. El cambio principal no es tecnológico: convierte un archivo en OneDrive y una conversación informal en un proceso institucional con fuente única, excepciones revisables y estados trazables. La titularidad y recuperación del espacio actual aún deben comprobarse. El diseño conserva la intervención humana donde existe incertidumbre y automatiza únicamente los pasos repetibles y verificables.
