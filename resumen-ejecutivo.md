# Resumen Ejecutivo

**Cliente:** Jefatura de Cultura de Innovación y Servicio — Universidad de La Sabana
**Proyecto:** Actualización del Directorio de Extensiones Telefónicas
**Documento dirigido a:** el negocio. No requiere conocimientos técnicos para leerse.

---

## 1. La situación hoy

Cuando alguien llama a la Universidad, una gestora de servicio contesta, entiende qué necesita esa persona y la transfiere al área correspondiente. Para saber a dónde transferir, consulta un directorio de extensiones telefónicas.

Ese directorio se mantiene manualmente. Cada mes llega la nómina —con retraso de un mes— y hay que compararla, registro por registro, contra el directorio para detectar quién entró a la Universidad, quién salió y quién cambió de cargo. Después hay que coordinar con la Dirección de Tecnología para pedir que asignen o liberen las extensiones correspondientes.

**El esfuerzo es desproporcionado frente al resultado:**

| | |
|---|---|
| Registros que hay que revisar cada ciclo | ~6.400 |
| Novedades reales que aparecen al mes | 5 a 10 |
| Proporción | Cerca de **800 registros revisados por cada novedad encontrada** |
| Duración de la revisión conjunta con Tecnología | 2 a 3 horas |
| Frecuencia con que debería actualizarse | Mensual |
| Frecuencia con que se logra en la práctica | Cada 2 o 3 meses |

Además de lo anterior, hay dos obstáculos concretos:

**El cruce actual no funciona de forma confiable.** La cliente reportó dificultades con las búsquedas de Excel. La causa exacta debe reproducirse con una muestra autorizada: no se presupone que conservar los identificadores como texto sea un error. La nómina contiene ID y correo; ambas fuentes necesitan llaves compatibles y validación de duplicados.

**Tecnología no avisa cuando termina.** Cuando se pide una extensión nueva, el registro queda marcado como "pendiente por aprovisionar". Tecnología la asigna y llama al colaborador, pero no informa de vuelta a la unidad. La marca se queda ahí indefinidamente y las gestoras no saben si ya pueden transferir llamadas a esa persona.

---

## 2. Por qué importa

El costo no lo asume la unidad: lo asume quien llama.

Cuando el directorio está desactualizado, la llamada se transfiere a un área que no corresponde, o a una extensión donde ya no trabaja nadie y simplemente nadie contesta. Un estudiante, un aspirante o un visitante externo tiene la experiencia de que la Universidad no supo atenderlo.

Hoy esas fallas no se registran ni se miden, por lo que la unidad no tiene visibilidad de cuántas veces ocurren.

---

## 3. Lo que proponemos

### Que el cruce reduzca la revisión a excepciones

La responsable cargará el insumo mensual y ejecutará la actualización guiada del libro de Excel con Power Query. El cruce producirá un reporte de excepciones para revisión humana:

- **Ingresó** una persona que por su cargo debería tener extensión
- **Se retiró** una persona que tenía extensión asignada
- **Cambió de cargo o de unidad** alguien ya registrado
- **Posición posiblemente vacante**, cuando exista una llave de posición y una fuente suficiente para comprobarlo. Una ausencia en la nómina no prueba por sí sola una vacante ni autoriza liberar una extensión.

Los registros sin cambios no aparecen como novedades. Los casos sin llave, ambiguos o con cambios inesperados se separan para revisión. Los 5 a 10 cambios mensuales son el volumen habitual declarado, no un límite del reporte.

### Que exista un canal de ida y vuelta con Tecnología

Después de validar el reporte, la responsable registrará las novedades y solicitudes en Lists. Power Query no publicará directamente en las listas en el diseño base. Power Automate notificará a Tecnología y registrará su respuesta autenticada. La confirmación actualizará el estado administrativo únicamente si corresponde a la solicitud vigente y cumple las reglas de validación; los casos ambiguos conservarán revisión humana.

Tecnología también podrá registrar una asignación o liberación que conozca antes de la siguiente nómina, para que la unidad la valide sin esperar al cierre mensual. El PBX continuará siendo operado por Tecnología, sin integración automática.

Los recordatorios y el escalamiento serán automáticos una vez se acuerde el plazo de atención con Tecnología. Ese plazo aún no está confirmado.

### Que el directorio sea información institucional

Hoy el directorio es un archivo alojado en OneDrive. La titularidad técnica, el historial disponible y los mecanismos de recuperación deben comprobarse. La cliente relató un daño accidental que obligó a reconstruirlo; esto no demuestra que OneDrive carezca de versionamiento.

Proponemos moverlo a un espacio institucional donde tenga estructura definida, permisos claros, registro de quién cambió qué y cuándo, y consulta cómoda para las gestoras desde el computador o el celular.

### Sin salirse de lo que la Universidad ya tiene

La propuesta usa la suite Microsoft existente y conectores estándar, sin desarrollo a medida ni servidores propios. La cliente indicó que dispone de licenciamiento ordinario, pero no se han confirmado permisos para crear sitios/listas, licencias de los propietarios de los flujos, políticas DLP ni aprobación de Seguridad. Las revisiones o comités que exija la Universidad siguen aplicando.

Esto responde directamente a lo que ocurrió en un semestre anterior, cuando un desarrollo entregado por otro equipo nunca pudo implementarse.

---

## 4. Beneficios esperados

| Situación actual | Situación propuesta |
|---|---|
| Revisión manual de 6.400 registros | Cruce guiado y revisión de excepciones; volumen habitual estimado de 5 a 10 cambios |
| Reunión de 2 a 3 horas con Tecnología | Validación breve del reporte |
| Solicitudes verbales sin registro | Solicitudes estructuradas y trazables |
| Sin confirmación de Tecnología | Respuesta autenticada y actualización controlada del estado |
| Marca "pendiente" indefinida | Vencimiento y recordatorio automático |
| Actualización cada 2 o 3 meses | Actualización mensual sostenida |
| Archivo en OneDrive con continuidad por confirmar | Fuente institucional con esquema, roles e historial verificado |

---

## 5. Lo que esta solución no resuelve

Conviene ser explícitos.

La nómina llega con un mes de retraso y es el único insumo disponible. Eso significa que siempre habrá un desfase de hasta 45 días entre el momento en que alguien cambia de cargo y el momento en que la unidad se entera. Ninguna automatización puede eliminar ese retraso: la información simplemente no existe antes.

Lo que sí se compensa parcialmente es el caso de los ingresos nuevos, porque el canal con Tecnología captura esa novedad en el momento en que ocurre, sin esperar a la nómina.

La recomendación de fondo —que Desarrollo Humano reporte las novedades directamente a la unidad— es un cambio de coordinación entre áreas, no un cambio técnico, y queda planteado como oportunidad de mejora.

---

## 6. Cómo se implementa

### Fase 1 — Preparar la información
Verificar el esquema y hacer compatibles las llaves de ambas fuentes, conservando los identificadores como texto, y completar el directorio con el ID que la cliente confirmó en la nómina. Confirmar qué registros del directorio ya cuentan con ese ID. El emparejamiento inicial por correo exige unicidad y revisión de ambiguos, no se basa únicamente en unidad y cargo.

### Fase 2 — Trasladar el directorio
Migrar el directorio a un espacio institucional con estructura definida y permisos por rol. Las gestoras mantienen la consulta; la edición queda controlada y registrada.

### Fase 3 — Automatizar el cruce
Configurar el libro de conciliación con actualización guiada, validación de entrada y reporte de excepciones. La publicación en Lists será controlada por la responsable; la automatización completa queda condicionada a pruebas y licencias.

### Fase 4 — Conectar con Tecnología
Habilitar el canal de solicitud y el de confirmación de retorno, junto con el recordatorio automático para las solicitudes que se queden sin respuesta.

### Fase 5 — Entregar y acompañar
Documento de configuración paso a paso, acompañamiento en la puesta en marcha y verificación de que la unidad puede operar y mantener la solución por su cuenta.

---

## 7. Qué necesitamos del cliente

- Confirmación de los permisos disponibles en las herramientas de Microsoft 365.
- Una muestra ficticia o anonimizada autorizada, o revisión en el entorno del cliente, para reproducir el problema del cruce. La nómina con datos reales no se publica en GitHub.
- Aclaración de la correspondencia entre las categorías de unidad que maneja la nómina y las que maneja el directorio.
- El criterio documentado de qué cargos tienen derecho a extensión telefónica.
- Validación de este resumen y del diseño objetivo preliminar ya elaborado para Corte 2, antes del piloto o implantación.
- Confirmación de la llave de posición, el criterio de vacancia y los estados/plazos de respuesta de Tecnología.
- Revisión de roles, clasificación de datos, base jurídica y retención con los responsables institucionales.

---

## 8. Criterio de éxito

El proyecto será exitoso si, tres meses después de terminado, la unidad sigue usando la solución sin necesidad de nuestra intervención.

No buscamos entregar el sistema más sofisticado posible, sino el que la unidad pueda operar y mantener por sí misma dentro de las herramientas que ya tiene.

---

*Diseño propuesto para Corte 2, pendiente de validación e implantación institucional. Las cifras corresponden a lo declarado por el cliente durante el levantamiento de información. Última actualización: 2 de octubre de 2026.*
