# Informe — Análisis de Seguridad STRIDE

**Fase:** Evaluación de seguridad de la arquitectura · Corte 2  
**Matriz:** [tabla-stride-cliente.xlsx](tabla-stride-cliente.xlsx)

## 1. Alcance

El análisis cubre los componentes y flujos definidos en las carpetas [`03-arquitectura-c4/`](../03-arquitectura-c4/) y [`04-infraestructura/`](../04-infraestructura/). Evalúa la arquitectura objetivo propuesta, no una implementación ya desplegada.

Durante el levantamiento, la cliente identificó el directorio como información sensible y la nómina como un insumo de sensibilidad mayor, y condicionó cualquier acceso del equipo a su uso académico restringido. El diseño adopta una postura más conservadora: no publica datos reales en el repositorio y utiliza estructuras o datos ficticios para pruebas y evidencias.

Activos principales:

- Archivo mensual de nómina.
- Directorio institucional de extensiones.
- Catálogos maestros.
- Registros de novedades y solicitudes.
- Flujos y conexiones de Power Automate.
- Notificaciones y respuestas en Teams/Outlook.
- Identidades, grupos y permisos del tenant.
- Historial de versiones y ejecuciones.

## 2. Método

Cada escenario se clasifica con STRIDE:

| Categoría | Pregunta de análisis |
|---|---|
| Spoofing | ¿Alguien puede aparentar ser otra persona o sistema? |
| Tampering | ¿Puede modificarse información o configuración sin autorización? |
| Repudiation | ¿Puede una acción quedar sin evidencia suficiente de autor, fecha o resultado? |
| Information Disclosure | ¿Pueden revelarse datos a quien no los necesita? |
| Denial of Service | ¿Puede impedirse el proceso o dejarlo sin respuesta? |
| Elevation of Privilege | ¿Puede un usuario obtener permisos superiores a los requeridos? |

La matriz usa escalas de probabilidad e impacto de 1 a 5. El puntaje es el producto de ambas:

- 1–4: Bajo.
- 5–9: Medio.
- 10–16: Alto.
- 17–25: Crítico.

Los valores son una evaluación inicial del equipo y deben validarse con Tecnología y Seguridad Informática. No representan una medición institucional oficial.

## 3. Resultado resumido

Los escenarios de mayor atención se concentran en cuatro áreas:

1. **Acceso excesivo al archivo de nómina.** Es el activo con mayor cantidad y sensibilidad de datos. Debe permanecer separado de las vistas operativas del directorio.
2. **Actualización incorrecta del directorio.** Una llave ambigua, un archivo alterado o una transición defectuosa puede asignar una extensión a la persona equivocada.
3. **Suplantación o falta de evidencia en la confirmación.** La respuesta de Tecnología debe estar ligada a una identidad institucional y a una solicitud específica.
4. **Propiedad y continuidad de los flujos.** Una cuenta individual, una conexión vencida o una política DLP puede detener la automatización sin que el usuario lo advierta.

## 4. Controles prioritarios

### 4.1. Identidad y acceso

- Autenticación exclusivamente con Microsoft Entra ID.
- Grupos institucionales separados para consulta, operación y administración.
- Revisión periódica de miembros y propietarios.
- Tecnología solo accede al detalle necesario de cada solicitud, no a la nómina completa.
- La cuenta que posee los flujos debe tener continuidad institucional y co-propietario.

### 4.2. Integridad

- Conservar `Id Empleado` como texto y usarlo como llave después de la migración inicial.
- Validar esquema, columnas obligatorias, duplicados y conteos antes del cruce.
- Detener cambios automáticos cuando exista ambigüedad o volumen inesperado.
- Usar estados controlados y transiciones válidas.
- Registrar la versión anterior del directorio y el identificador de la solicitud que originó el cambio.

### 4.3. Trazabilidad

- Identificador único para novedad y solicitud.
- Usuario y fecha en cada transición.
- Historial de versiones de archivos y listas.
- Historial de ejecuciones de Power Automate y alertas de falla.
- Observación obligatoria para rechazo, corrección o cierre excepcional.

### 4.4. Confidencialidad

- Sitio privado y mínimo privilegio.
- Notificaciones con el mínimo de datos: ID, tipo de acción y enlace al registro protegido.
- Prohibición de copiar la nómina a GitHub, chats no autorizados o cuentas personales.
- Separación entre el repositorio académico y los datos operativos.
- Confirmación de las políticas DLP y retención del tenant.

### 4.5. Disponibilidad

- Alerta cuando un flujo falla.
- Reintentos limitados y manejo explícito de errores.
- Procedimiento manual de contingencia.
- Lista de solicitudes conserva el último estado válido aunque una notificación falle.
- Escalamiento de solicitudes vencidas.

## 5. Riesgo residual

Los controles propuestos reducen el riesgo, pero no eliminan:

- El desfase de hasta 45 días de la fuente mensual.
- La dependencia de la disponibilidad de Microsoft 365.
- El riesgo de error humano al validar excepciones.
- La ausencia de integración directa con el PBX.
- Los cambios de formato del archivo de nómina.

Por esta razón, el proceso debe conservar revisión humana, monitoreo de excepciones y un mecanismo de corrección.

## 6. Validación requerida

Antes de implantar:

- Seguridad Informática debe revisar la clasificación del dato y los controles de acceso.
- El administrador de Power Platform debe confirmar políticas DLP y conectores permitidos.
- Tecnología debe aceptar el mecanismo de respuesta y escalamiento.
- El cliente debe probar la matriz con datos ficticios y un conjunto controlado de escenarios.
- Cada control marcado como “Por validar” en el Excel debe recibir evidencia o mantenerse como brecha abierta.

## 7. Conclusión

El diseño es viable si la automatización se apoya en identidad institucional, mínimo privilegio, validaciones previas y trazabilidad. La seguridad no depende de ocultar el archivo, sino de limitar su circulación, controlar quién actúa y conservar evidencia verificable de cada cambio.
