# Trazabilidad de la entrega

## Cobertura del Taller 9

| Requisito | Evidencia | Verificacion |
|---|---|---|
| Narrativa en 4 actos | Presentacion y guion | Problema, analisis, solucion y plan aparecen en ese orden |
| Presentacion de maximo 10 minutos | Guion cronometrado | Duracion objetivo total de 9 min 40 s |
| Riesgos de 7 dominios | Matriz de riesgos | Hay riesgos de Negocio, Procesos, Datos, Aplicaciones, Infraestructura, Seguridad y Gobierno TI |
| Nivel calculado | Matriz de riesgos | Formula Impacto x Probabilidad y clasificacion automatica |
| Estrategias de riesgo | Matriz de riesgos | Eliminar, Reducir, Transferir o Aceptar; incluye riesgos aceptados y justificacion |
| Controles preventivos y reactivos | Matriz de riesgos | Cada riesgo identifica tipo de control |
| Roadmap por fases | Plan de implementacion | Habilitacion, corto, mediano y largo plazo con trazabilidad a riesgos y brechas |
| Gestion del cambio | Plan de implementacion | Grupos reales, perdida o temor, resistencia, comunicacion, capacitacion y resultado visible |
| Principios y estandares | Plan de gobernanza | Principios confirman o reescriben la Vision; cada uno permite rechazar una opcion |
| Tres metricas | Plan de gobernanza | Formula, fuente existente, linea base y umbral; M3 mide al comite |
| Procedimiento de cambios | Procedimiento de cambios | Disparadores, flujo, tiempos, roles, revision completa y formato futuro de ADR |
| Dos ADR reales o mas | Procedimiento de cambios | ADR-001, ADR-002 y ADR-003 con alternativas y consecuencias negativas |
| Vista de Motivacion | Drawio y PNG | Drivers y goals se conectan con principles, requirements y work packages |
| Vista de Migracion | Drawio y PNG | Cada fase aparece como Work Package asociado a gaps y plateaus |
| Resumen ejecutivo | Resumen ejecutivo | Decision, situacion, propuesta, beneficios, riesgos, roadmap y proximos pasos |
| Retroalimentacion del cliente | Formulario | Formato listo y explicitamente sin firma hasta diligenciamiento real |
| Reflexion individual | Borrador | Plantilla editable con campos que requieren experiencia personal |
| Referencias | Referencias | Enlaces al taller y a la evidencia de los Talleres 0 a 8 |

## Correspondencia entre fases, brechas, riesgos y ADR

| Fase | Work Package | Brechas principales | Riesgos principales | ADR |
|---|---|---|---|---|
| Habilitacion y control | WP0 | G08, controles no validados de seguridad y gobierno | R-01, R-07, R-11, R-13, R-14 | ADR-002 y politica de excepciones |
| Directorio gobernado | WP1 / S1 | G02, G06, G07, G09, G10, G12 | R-03, R-09, R-11, R-13 | ADR-001 |
| Conciliacion por excepciones | WP2 / S2 | G01, G05, G08, G11, G13, G14 | R-05, R-06, R-07, R-08 | ADR-002 |
| Aprovisionamiento trazable | WP3 / S3 | G03 y G04 | R-02, R-04, R-08, R-12 | ADR-003 |

## Puertas de calidad

1. **Antes del piloto:** permisos, licencias, DLP, base juridica, retencion y responsables documentados.
2. **Antes de datos reales:** prueba de acceso por rol, recuperacion, minimizacion y ausencia de datos reales en repositorios o notificaciones.
3. **Antes de automatizar cambios:** llaves, duplicados, taxonomias, reintentos y respuestas tardias probados con datos ficticios.
4. **Antes de operacion estable:** responsables formados, contingencia manual ensayada, metricas calculables y aceptacion del dueño del proceso.

## Elementos que requieren accion humana

- La cliente debe diligenciar y firmar su retroalimentacion si decide hacerlo.
- Cada integrante debe personalizar y enviar su reflexion individual segun el medio definido por el curso.
- La coevaluacion se envia de forma privada por el Aula Virtual y no se incluye en este repositorio.
- Las aprobaciones institucionales no pueden sustituirse por una declaracion del equipo.

