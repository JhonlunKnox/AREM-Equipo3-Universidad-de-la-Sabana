# Architecture Vision

> Fase Architecture Vision — TOGAF ADM · Corte 1

---

## 1. Declaración de visión

Que el directorio de extensiones telefónicas deje de ser un archivo que alguien mantiene a mano y pase a ser información institucional que se actualiza sola, con un circuito de confirmación cerrado entre la unidad de servicio y la Dirección de Tecnología.

El objetivo no es construir un sistema. Es rediseñar un proceso apoyándose en herramientas que la Universidad ya tiene licenciadas, de modo que la unidad pueda operarlo y sostenerlo sin dependencia externa.

---

## 2. Alcance

### Dentro del alcance

- Proceso de conciliación entre la nómina institucional y el directorio de extensiones.
- Proceso de solicitud y confirmación de aprovisionamiento con la Dirección de Tecnología.
- Estructura y gobierno del dato del directorio de extensiones.
- Proceso de atención y transferencia de llamadas, en lo que resulta afectado por la calidad del directorio.

### Fuera del alcance

- Buzón PQRSF, encuestas de satisfacción y modelo de servicio.
- Canales de WhatsApp, correo electrónico y redes sociales.
- Plataforma de telefonía (PBX) y ejecución técnica del aprovisionamiento.
- Sistemas de información de Desarrollo Humano que originan la nómina.

---

## 3. Vista de motivación (ArchiMate)

### 3.1. Stakeholders

| Stakeholder | Interés |
|---|---|
| Usuario final | Ser atendido y transferido correctamente en el primer contacto. |
| Gestora de servicio (×4) | Disponer de información vigente para transferir sin error. |
| Profesional de Experiencia y Servicio | Reducir el esfuerzo manual y eliminar las reuniones de revisión línea a línea. |
| Jefe de Cultura de Innovación y Servicio | Calidad y consistencia del servicio de atención. |
| Analista de Aprovisionamiento (Tecnología) | Recibir solicitudes estructuradas sin depender de reuniones. |
| Desarrollo Humano | Consistencia entre la planta de personal y los recursos asignados. |

### 3.2. Drivers

- **Experiencia de servicio** — calidad percibida del contacto institucional.
- **Eficiencia operativa** — uso del tiempo del personal de la unidad.
- **Calidad del dato institucional** — vigencia y confiabilidad de la información operativa.
- **Cumplimiento normativo** — protección de datos personales y políticas de seguridad informática.

### 3.3. Assessments

| ID | Diagnóstico |
|---|---|
| A1 | La conciliación manual sobre 6.400 registros consume 2–3 horas por ciclo para detectar 5–10 novedades (relación señal/ruido ≈ 0,12 %). |
| A2 | El archivo de nómina no admite cruce automatizado por funciones de búsqueda. |
| A3 | No existe canal de notificación bidireccional entre la unidad y Tecnología. |
| A4 | El ciclo de actualización depende del agendamiento de reuniones (esperas de hasta 3 semanas). |
| A5 | Cargo + Unidad no constituyen una llave unívoca de identificación de personas. |
| A6 | La nómina se entrega mes vencido: desfase estructural de hasta 45 días. |
| A7 | El directorio reside en OneDrive, sin esquema tipado ni trazabilidad del proceso. La titularidad técnica del espacio debe confirmarse. |

### 3.4. Goals

| ID | Objetivo |
|---|---|
| G1 | Directorio de extensiones vigente y confiable. |
| G2 | Ciclo de actualización mensual sostenido. |
| G3 | Eliminar las reuniones de revisión línea a línea. |
| G4 | Trazabilidad completa del ciclo de aprovisionamiento. |

### 3.5. Outcomes esperados

| ID | Resultado |
|---|---|
| O1 | Reducción del tiempo de conciliación por ciclo. |
| O2 | Ningún registro permanece indefinidamente en estado «pendiente por aprovisionar». |
| O3 | Disminución de transferencias erróneas de llamadas. |

### 3.6. Principles

| ID | Principio |
|---|---|
| P1 | Usar exclusivamente la tecnología corporativa existente (Microsoft 365). |
| P2 | La solución debe ser operable y mantenible por la propia unidad. |
| P3 | Fuente única de verdad para el dato del directorio. |

### 3.7. Requirements

| ID | Requerimiento | Realiza |
|---|---|---|
| RQ1 | Conciliación automatizada entre nómina y directorio con reporte de novedades clasificadas. | G2, G3 |
| RQ2 | Canal de solicitud de aprovisionamiento hacia Tecnología. | G1, G4 |
| RQ3 | Canal de confirmación de retorno desde Tecnología. | G1, G4 |
| RQ4 | Llave de identificación estable entre las dos fuentes de datos. | G1 |

### 3.8. Constraints

| ID | Restricción | Afecta |
|---|---|---|
| C1 | Implementación restringida a la suite Microsoft 365. | RQ1, RQ2, RQ3 |
| C2 | Licenciamiento estándar de Power Automate. | RQ1, RQ2, RQ3 |
| C3 | La nómina es el único insumo y llega mes vencido. | RQ1 |
| C4 | Tratamiento de datos personales conforme a la Ley 1581 de 2012. | Todos |

---

## 4. Beneficios esperados de la transformación

| Situación actual | Situación objetivo |
|---|---|
| Revisión manual de 6.400 registros por ciclo | Reporte automático de 5–10 excepciones |
| 2–3 horas de reunión de conciliación | Validación breve del reporte |
| Solicitudes verbales sin registro | Solicitudes estructuradas y trazables |
| Sin confirmación de retorno | Confirmación que actualiza el directorio automáticamente |
| Estado «pendiente» indefinido | Vencimiento y escalamiento automático |
| Actualización bimensual o trimestral | Actualización mensual sostenida |
| Archivo en OneDrive, sin estados ni auditoría del proceso | Fuente institucional con esquema y auditoría |

---

## 5. Criterios de éxito

- El ciclo de actualización del directorio se ejecuta con periodicidad mensual de forma sostenida.
- La identificación de novedades deja de requerir la revisión de la totalidad de los registros.
- Ningún registro permanece en estado «pendiente por aprovisionar» sin confirmación ni seguimiento.
- La solución opera íntegramente sobre las herramientas corporativas existentes.
- La documentación entregada permite al cliente configurar, operar y mantener la solución sin asistencia del equipo.

---

## 6. Riesgos y supuestos

| Tipo | Descripción | Tratamiento |
|---|---|---|
| Supuesto | El correo institucional permite la correspondencia inicial entre nómina y directorio, dado que el Id Empleado no está hoy en el directorio. | Validar en Arquitectura de Datos; propagar el Id Empleado al directorio como llave estable. |
| Supuesto | El cliente dispone de permisos para crear listas en Microsoft 365. | Confirmar antes del Corte 2. |
| Riesgo | Taxonomías organizacionales divergentes (Departamento y Establecimiento en nómina frente a Unidad en directorio). | Construir tabla de correspondencia; documentar en el modelo de datos. |
| Riesgo | La regla de qué cargos tienen derecho a extensión es conocimiento tácito. | Externalizar a un catálogo mantenible. |
| Riesgo | El licenciamiento podría no cubrir determinados conectores. | Validar cada conector con el cliente antes de proponerlo. |

---

## 7. Límite explícito de la solución

La nómina llega mes vencido y es el único insumo disponible. Esto implica un desfase de hasta 45 días entre el evento real y su detección que **ninguna automatización puede eliminar**.

El canal bidireccional con Tecnología lo compensa parcialmente: los ingresos nuevos que requieren extensión se capturan en el momento en que ocurren, sin esperar a la nómina. Los cambios de cargo internos siguen dependiendo del ciclo mensual.

La recomendación de fondo —que Desarrollo Humano reporte novedades directamente— es un cambio de coordinación entre áreas, no técnico, y queda planteado como oportunidad para la fase de Architecture Change Management.
