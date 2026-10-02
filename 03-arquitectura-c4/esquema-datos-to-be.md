# Esquema de datos TO-BE — contrato preliminar

**Estado:** propuesta de diseño para Corte 2, no listas desplegadas ni contrato aprobado por el cliente. **Revisión:** 2 de octubre de 2026.

Complementa el [C2 y flujo de conciliación](informe.md#4-arquitectura-objetivo-to-be). Los códigos C01–C06 identifican responsabilidades lógicas; no todos son aplicaciones independientes. Las listas y la biblioteca residirían en SharePoint institucional. No se publica contenido de nómina ni directorio real.

## 1. Convenciones y llaves

- `ID`: identificador nativo numérico de cada elemento de SharePoint. Se usa para búsquedas y relaciones entre listas; no es el ID del empleado.
- `IdEmpleado`, `IdPosicion` y `Extension`: **texto**, preservando ceros iniciales. No convertirlos a número. La naturaleza y el formato exactos del ID que trae la nómina deben verificarse con la cliente.
- `Lookup`: referencia al ID de un elemento de otra lista del mismo sitio. No equivale a una clave foránea SQL: las reglas compuestas, unicidad entre campos y transiciones deben comprobarse en formularios/procedimientos y flujos.
- Todas las listas conservan `Created`, `Modified`, `Author` y `Editor` nativos, con historial de versiones habilitado y probado. No se presume que esto sustituya una auditoría institucional.
- `Sí` en obligatorio significa requerido en el registro; `Cond.` exige la condición indicada. Campos de una etapa posterior no bloquean crear un registro preliminar, pero sí avanzar de estado.
- Claves técnicas únicas propuestas: `ClaveDirectorio`, `ClaveConciliacion` y `ClaveSolicitud`. Deben ser estables y permitir reintentos sin duplicar elementos. No generar una clave nueva en cada reintento.
- Unidad y cargo no identifican a una persona. El correo solo se usa como llave transitoria si hay una única coincidencia en ambas fuentes. Los casos ambiguos quedan fuera de la actualización.
- Definir índices y vistas filtradas sobre las llaves y estados usados en búsquedas, y probar consulta/conciliación con el volumen declarado (~6.400 registros). No asumir que una vista sin filtros o una carga parcial representa la lista completa.

## 2. C01 — biblioteca `EntradasNomina`

Archivo fuente restringido, sin replicar salarios u otros campos no necesarios en las listas operativas.

| Columna | Tipo SharePoint | Obligatorio | Regla |
|---|---|---|---|
| ID | Nativo | Sí | Identifica el elemento de biblioteca |
| IdLote | Texto único | Sí | Identificador estable del archivo y corte; una recarga conserva relación con el lote previo |
| PeriodoFuente | Texto | Sí | `AAAA-MM`; periodo del contenido, no fecha de descarga |
| FechaRecepcion | Fecha/hora | Sí | Permite medir latencia de la fuente |
| Origen | Texto | Sí | Fuente autorizada de Desarrollo Humano |
| EstadoValidacion | Elección | Sí | Recibida / Rechazada / Lista para conciliar |
| ConteoFuente | Número entero | Cond. | Requerido para declarar Lista para conciliar; comprobar completitud, no asumir 6.400 como nómina exacta |
| ObservacionValidacion | Varias líneas | Cond. | Obligatoria si se rechaza o cambia el esquema |

Campos mínimos del contrato de entrada para Power Query: `IdEmpleado`, correo institucional, cargo, unidad y periodo. `IdPosicion` es requerido para evaluar vacancia por posición, pero **su disponibilidad no está confirmada**. Nombre solo cuando sea necesario para consulta/validación autorizada. Rechazar columnas obligatorias ausentes, errores de tipo o duplicados incompatibles con la granularidad acordada. Campos adicionales presentes en el archivo no se trasladan automáticamente al directorio.

## 3. C03 — lista `DirectorioExtensiones`

Granularidad propuesta: una asignación de extensión a una posición, con titular opcional. El modelo admite varias posiciones por persona, pero esa regla debe validarse. Si el directorio no aporta llave de posición, se conserva un ID técnico y se bloquea la clasificación automática de vacantes.

| Columna | Tipo | Obligatorio | Regla / relación |
|---|---|---|---|
| ID | Nativo | Sí | Clave del registro del directorio |
| ClaveDirectorio | Texto único | Sí | Clave técnica estable; no depende del nombre o cargo |
| IdPosicion | Texto | Cond. | Requerido para afirmar vacancia por posición |
| IdEmpleado | Texto | Cond. | Requerido para titular activo; vacío en posición vacante confirmada |
| NombreTitular | Texto | Cond. | Solo titular y consulta autorizada |
| CorreoInstitucional | Texto | Cond. | Titular activo; normalizado y validado; no correo personal |
| Unidad | Lookup a Unidades | Sí | Unidad canónica |
| CargoRegla | Lookup a CargosElegibilidad | Sí | Debe corresponder a la unidad elegida |
| Extension | Texto | Cond. | Requerida cuando hay asignación técnica confirmada; no inventarla en un ingreso |
| EstadoPosicion | Elección | Sí | Ocupada / Vacancia por confirmar / Vacante confirmada / Por validar |
| EstadoExtension | Elección | Sí | Asignada / Por aprovisionar / Por liberar / Liberada / Por validar |
| Tematicas | Varias líneas | No | Información operativa mínima para orientar llamadas |
| UltimaSolicitud | Lookup a SolicitudesAprovisionamiento | No | Evidencia de la última actuación técnica aplicada |
| PeriodoUltimaConciliacion | Texto | No | `AAAA-MM`; nunca sustituye fecha efectiva de Tecnología |

Validar unicidad de `IdPosicion` cuando exista y de la asignación activa de `Extension`, según reglas que confirme Tecnología (extensiones compartidas no se presumen ni se descartan). Más de una coincidencia por empleado se resuelve con posición; sin ella, se registra excepción. La vista de gestoras no muestra ID del empleado ni enlaces a nómina; muestra los campos operativos autorizados. Ocultar una columna en una vista no es seguridad: si deben restringirse campos, publicar una proyección operativa en un recurso con permisos propios y comprobar que la fuente completa no sea accesible.

## 4. C04 — catálogos maestros

| Lista | Campos y tipos | Obligatorios | Llaves / reglas |
|---|---|---|---|
| Unidades | ID nativo; CodigoUnidad texto único; Nombre texto; Activa Sí/No | Todos | Una unidad canónica por código institucional |
| EquivalenciasUnidades | ID nativo; ClaveAlias texto única; SistemaOrigen elección; Alias texto; Unidad lookup a Unidades | Todos | `ClaveAlias = origen + alias normalizado`; un alias ambiguo no se resuelve solo |
| CargosElegibilidad | ID nativo; ClaveRegla texto única; CodigoCargo texto; NombreCargo texto; Unidad lookup a Unidades; TieneDerechoExtension elección; EvidenciaRegla hipervínculo | Todos excepto EvidenciaRegla, que es condicional | Elegibilidad: Sí / No / Por validar. Evidencia obligatoria para Sí/No; la regla por cargo/unidad requiere aprobación del cliente |

No se asigna derecho a extensión por analogía con cargos similares. Un código de catálogo propuesto no se presenta como código institucional ya aprobado.

## 5. C05 — lista `Novedades`

| Columna | Tipo | Obligatorio | Regla / relación |
|---|---|---|---|
| ID | Nativo | Sí | Clave de novedad |
| ClaveConciliacion | Texto único | Sí | Lote + registro/llave + tipo de diferencia; idempotencia entre reintentos del mismo lote |
| LoteFuente | Lookup al elemento de EntradasNomina | Cond. | Obligatorio para origen Conciliación |
| Origen | Elección | Sí | Conciliación / Tecnología / Corrección manual |
| RegistroDirectorio | Lookup a DirectorioExtensiones | No | Puede estar vacío en un ingreso aún no incorporado |
| IdEmpleado | Texto | Cond. | Necesario para aprobar movimiento de titular; no exigible en excepción Sin llave |
| IdPosicion | Texto | Cond. | Necesario para aprobar vacancia por posición |
| Tipo | Elección | Sí | Ingreso / Retiro por confirmar / Cambio de cargo / Cambio de unidad / Vacancia por confirmar / Sin llave / No clasificable |
| ResumenDiferencia | Varias líneas | Sí | Solo campos operativos comparados y valores anterior/propuesto necesarios, sin salarios |
| Estado | Elección | Sí | Detectada / En revisión / Aprobada / Descartada / Aplicada |
| Responsable | Persona institucional | Sí | Responsable de revisión |
| ValidadoPor | Persona institucional | Cond. | Obligatorio para Aprobada, Descartada o Aplicada |
| FechaValidacion | Fecha/hora | Cond. | Obligatoria junto con ValidadoPor |
| MotivoDecision | Varias líneas | Cond. | Obligatorio para aprobar, descartar o corregir |

`Aplicada` exige evidencia del cambio al directorio y, cuando requiere intervención técnica, una solicitud exitosa. El historial conserva el valor anterior; no se sobreescribe para ocultar una diferencia.

## 6. C06 — lista `SolicitudesAprovisionamiento`

| Columna | Tipo | Obligatorio | Regla / relación |
|---|---|---|---|
| ID | Nativo | Sí | Identificador que acompaña toda respuesta |
| ClaveSolicitud | Texto único | Sí | Clave estable por actuación solicitada; evita duplicados por reintento |
| Novedad | Lookup a Novedades | Cond. | Obligatoria para origen Conciliación |
| RegistroDirectorio | Lookup a DirectorioExtensiones | No | Se completa antes de aplicar al directorio; puede faltar en un ingreso |
| Origen | Elección | Sí | Experiencia y Servicio / Tecnología |
| TipoAccion | Elección | Sí | Aprovisionar / Liberar / Corregir asignación |
| IdEmpleado | Texto | Cond. | Titular objetivo cuando aplica; sin nómina adjunta |
| IdPosicion | Texto | No | Contexto de la asignación, cuando está disponible |
| Extension | Texto | Cond. | Obligatoria para Liberar y para respuesta Aprovisionada/Liberada |
| Estado | Elección | Sí | Creada / Enviada a Tecnología / En gestión / Requiere información / Aprovisionada / Liberada / Rechazada / Vencida / Escalada |
| Solicitante | Persona institucional | Sí | Autor de la solicitud |
| ResponsableTecnologia | Persona o grupo institucional | Sí | Asignación nominativa o grupo autorizado |
| FechaEnvio | Fecha/hora | Cond. | Requerida al pasar a Enviada a Tecnología |
| FechaObjetivo | Fecha/hora | No | Solo tras acordar SLA; sin esta fecha no se evalúa vencimiento automáticamente |
| ConfirmadoPor | Persona institucional | Cond. | Obligatorio en respuesta técnica final, verificado contra el grupo autorizado |
| FechaRespuesta | Fecha/hora | Cond. | Obligatoria con respuesta técnica |
| ObservacionRespuesta | Varias líneas | Cond. | Obligatoria en rechazo, solicitud de información o corrección |
| ResultadoAplicacion | Elección | Sí | Pendiente / Aplicado / Requiere revisión / Error |
| ValidadoPorUnidad | Persona institucional | Cond. | Obligatorio si Tecnología inicia el aviso o hay discrepancia |

La respuesta exitosa no prueba que el dato ya se aplicó: `ResultadoAplicacion` solo pasa a Aplicado al completar el cambio y registrar evidencia. Si falla el flujo, conserva la respuesta técnica y alerta. Antes de escribir, comprobar ID de solicitud, estado vigente, identidad autorizada, titular/extensión compatibles y ausencia de una solicitud posterior contradictoria. Una respuesta tardía o duplicada no sobreescribe una asignación nueva. La actualización automática se limita a casos ya aprobados y coherentes; los demás quedan Requiere revisión.

## 7. Relaciones y publicación

| Origen | Destino | Cardinalidad lógica | Control |
|---|---|---|---|
| Unidades | EquivalenciasUnidades / CargosElegibilidad / Directorio | 1:N | Unidad activa y equivalencia no ambigua |
| CargosElegibilidad | Directorio | 1:N | Cargo/unidad y elegibilidad validados |
| EntradasNomina | Novedades de conciliación | 1:N | Lote íntegro y corte identificado |
| Directorio | Novedades / Solicitudes | 1:N opcional al crear | Resolver referencia antes de aplicar |
| Novedades | Solicitudes | 1:N | Justificar cada actuación; no duplicar la misma acción |
| Solicitudes | Directorio.UltimaSolicitud | N:1 a lo largo del tiempo | Solo la última respuesta válida aplicada al registro |

Power Query **lee y prepara** datos. En el diseño base, la responsable revisa las excepciones y las registra por formulario/edición controlada en Lists, incluyendo las llaves de idempotencia. No se atribuye a Power Query una escritura directa en SharePoint. Una importación automatizada futura exige validar licencias, conectores, formato y controles; no forma parte de la solución base.

## 8. Vacancia y aceptación pendiente

Una posición vacante y una extensión liberada son hechos distintos. La ausencia de un empleado en un archivo de nómina puede deberse a un corte incompleto, cambio de posición o error. Solo genera Retiro/Vacancia **por confirmar**, nunca vacancia confirmada ni liberación automática.

Para confirmar una vacante se necesita llave estable de posición, comprobación de completitud del corte y una fuente/decisión autorizada de la unidad. Si esos datos no existen, el caso queda No clasificable o Por validar. Tecnología confirma por separado el estado técnico de la extensión.

Antes del piloto deben validarse: columnas reales, granularidad, llaves de persona/posición, extensiones compartidas, catálogos, derecho a extensión, estados y transiciones, responsables y SLA, permisos efectivos por dato, retención y evidencia de recuperación. Pruebas mínimas con datos sintéticos: ceros iniciales; correo duplicado; dos posiciones por empleado; nómina incompleta; vacancia sin ID; respuesta tardía; reintento duplicado; rechazo; cambio exitoso con fallo posterior de notificación.
