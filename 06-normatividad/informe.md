# Informe — Cumplimiento Legal y Normativo

**Fase:** Evaluación normativa de la arquitectura · Corte 2  
**Checklist:** [checklist-cliente.xlsx](checklist-cliente.xlsx)

## 1. Alcance

La arquitectura trata datos de colaboradores incluidos en la nómina y en el directorio: nombres, identificadores, correo institucional, cargo, unidad, posición y extensión. El análisis se concentra en protección de datos personales, confidencialidad, seguridad y responsabilidad demostrada.

La evaluación es académica y preliminar. No sustituye la revisión de la política institucional, del oficial de protección de datos, de Seguridad Informática ni un concepto jurídico de la Universidad.

La cliente autorizó mencionar a la Universidad y al área en el documento académico, pero esa autorización no equivale a permiso para publicar datos personales. En la sesión de levantamiento pidió restringir el directorio al grupo de trabajo y señaló que la nómina contiene información de mayor sensibilidad. El repositorio conserva únicamente documentación, estructuras y datos ficticios.

## 2. Marco de referencia

- **Ley 1581 de 2012:** principios, derechos de los titulares y deberes de responsables y encargados.
- **Decreto 1074 de 2015, Capítulo 25:** reglas reglamentarias sobre autorización, políticas y responsabilidad demostrada.
- **Guías de la Superintendencia de Industria y Comercio:** medidas apropiadas, efectivas y verificables, gestión de riesgos y función de protección de datos.
- **Políticas internas de la Universidad:** deben solicitarse y prevalecen para clasificación, acceso, retención, incidentes y uso de Microsoft 365.

## 3. Roles de tratamiento por validar

| Actor | Rol preliminar | Decisión pendiente |
|---|---|---|
| Universidad de La Sabana | Responsable del tratamiento | Confirmar el área que determina finalidad y medios |
| Jefatura de Cultura de Innovación y Servicio | Usuaria y custodio operativo | Formalizar responsabilidades sobre el directorio |
| Desarrollo Humano | Fuente institucional | Confirmar reglas de suministro y uso de la nómina |
| Dirección de Tecnología | Operador técnico y receptor de solicitudes | Definir acceso mínimo y evidencia requerida |
| Microsoft | Proveedor de servicios tecnológicos | Revisar contratos y condiciones institucionales vigentes |
| Equipo académico | Acceso temporal para análisis, si fue autorizado | No conservar ni publicar datos reales; cerrar acceso al finalizar |

## 4. Principios aplicados al diseño

| Principio | Aplicación propuesta |
|---|---|
| Finalidad | Usar los datos solo para mantener el directorio y gestionar extensiones |
| Libertad | Verificar la base jurídica y las autorizaciones aplicables; no presumirlas |
| Veracidad y calidad | Usar fuente institucional, llave estable y procedimiento de corrección |
| Transparencia | Mantener mecanismo para consultas y correcciones conforme a la política institucional |
| Acceso y circulación restringida | Grupos de mínimo privilegio; detalle disponible solo para quien lo necesita |
| Seguridad | Controles técnicos, humanos y administrativos acordes con el riesgo |
| Confidencialidad | Prohibición de divulgar o reutilizar información fuera de la finalidad autorizada |
| Responsabilidad demostrada | Evidencia de decisiones, controles, pruebas, responsables y revisiones |

## 5. Ciclo de vida del dato

### 5.1. Recolección

- La nómina se obtiene de la fuente autorizada de Desarrollo Humano.
- Se registra periodo, fecha, origen y responsable de la carga.
- No se recolectan datos adicionales “por si acaso”.

### 5.2. Uso

- Power Query procesa únicamente los campos requeridos para identificar novedades.
- Las gestoras consultan el directorio operativo, no el archivo de nómina.
- Tecnología recibe la información mínima necesaria para ejecutar cada solicitud.

### 5.3. Circulación

- Los avisos enviados por Teams u Outlook contienen un identificador y un enlace protegido, no la nómina completa.
- GitHub conserva exclusivamente documentación, esquemas y datos ficticios.

### 5.4. Conservación

- Debe definirse el periodo de conservación de nóminas de entrada, novedades, solicitudes e historial.
- La solución no fija un periodo sin conocer la tabla de retención o política institucional.
- La eliminación debe seguir un procedimiento autorizado y dejar evidencia cuando corresponda.

### 5.5. Corrección y atención de derechos

- La arquitectura debe permitir ubicar registros por la llave institucional.
- Toda corrección debe registrar autor, fecha, motivo y valor anterior.
- La ruta para consultas y reclamos debe enlazarse con el procedimiento institucional existente.

## 6. Brechas preliminares

| ID | Brecha | Riesgo | Acción requerida |
|---|---|---|---|
| N1 | No se dispone de la política institucional aplicable al proyecto | Controles o periodos incompatibles con la Universidad | Solicitar política y validarla con el responsable interno |
| N2 | No se ha documentado la base jurídica o autorización aplicable | Tratamiento sin evidencia suficiente | Obtener concepto del responsable de protección de datos |
| N3 | El directorio reside en OneDrive y no se ha confirmado la titularidad técnica ni la continuidad del espacio | Pérdida de control y continuidad si depende de un espacio individual | Migrar a sitio institucional con propietarios definidos |
| N4 | No existe matriz formal de acceso | Circulación excesiva | Aprobar perfiles y grupos de mínimo privilegio |
| N5 | No existe periodo de conservación documentado | Conservación indefinida | Definir retención y eliminación por tipo de registro |
| N6 | No hay evidencia estructurada de cambios | No se puede demostrar control | Activar historial y conservar ID de solicitud y responsable |
| N7 | No existe procedimiento de incidente específico | Respuesta tardía o incompleta | Integrar la solución al proceso institucional de incidentes |
| N8 | El equipo académico puede tener acceso temporal a muestras | Divulgación o retención no autorizada | Usar datos ficticios; retirar accesos y eliminar copias al cierre |

## 7. Evidencias requeridas

- Política de tratamiento de datos personales de la Universidad.
- Inventario o registro institucional de la base de datos, cuando aplique.
- Matriz de roles y permisos aprobada.
- Evidencia de configuración del sitio, listas, versiones y grupos.
- Documento de finalidad y campos mínimos.
- Regla de conservación y eliminación.
- Prueba de flujo con datos ficticios.
- Registro de riesgos y controles.
- Procedimiento de atención de consultas, correcciones e incidentes.
- Acta o aprobación del dueño del proceso y de Tecnología.

## 8. Estado de cumplimiento

El checklist marca la mayoría de los criterios como `Pendiente` o `Parcial` porque la documentación interna y la configuración del tenant no se encuentran en el repositorio. Esto es deliberado: la ausencia de evidencia no se presenta como cumplimiento.

Contiene 18 criterios: uno Cumple (REPO-01, limitado a los archivos de esta entrega), siete Parcial y diez Pendiente/Por validar. REPO-01 se sustenta en la regla del README, el .gitignore y la revisión del árbol de archivos al 2 de octubre de 2026; no certifica todo el historial ni cumplimiento institucional. El control recurrente de publicación I-03 permanece Parcial en STRIDE. La revisión excluye datos operativos del cliente, no los créditos académicos del equipo.

La columna **Hito objetivo propuesto** completa los 18 criterios con momentos de revisión (antes del piloto, antes de datos reales, en cada publicación o al cierre). Son propuestas que cada responsable debe confirmar, no fechas o SLA acordados. Las aprobaciones siguen pendientes. El [esquema de datos TO-BE](../03-arquitectura-c4/esquema-datos-to-be.md) aporta el diccionario inicial para DAT-01, pero no reemplaza la aprobación de campos ni la prueba de permisos.

La arquitectura incorpora controles compatibles con los principios de la Ley 1581, pero solo podrá declararse conforme después de que la Universidad confirme la base jurídica, la política, los roles, la retención, la configuración y las evidencias.

## 9. Conclusión

La principal recomendación normativa es separar la nómina del directorio operativo y reducir la circulación de datos. SharePoint, Lists y Power Automate pueden aportar trazabilidad y control, pero la plataforma por sí sola no demuestra cumplimiento. La Universidad debe definir finalidad, autorizaciones o base jurídica, responsabilidades, permisos, conservación y atención de derechos, y conservar evidencia verificable de esas decisiones.
