# Criterios de priorización STRIDE

**Matriz relacionada:** [tabla-stride-cliente.xlsx](tabla-stride-cliente.xlsx). **Fecha:** 2 de octubre de 2026. Puntajes iniciales del equipo, pendientes de validación institucional.

## 1. Interpretación de las escalas

La probabilidad se valora sobre un ciclo de operación del proceso, con las debilidades del escenario inicial. Es ordinal: **no expresa un porcentaje ni una frecuencia medida**. No se han observado ejecuciones del TO-BE ni se dispone de estadísticas de incidentes del tenant. Los controles actuales desconocidos no se presentan como inexistentes.

| Probabilidad | Criterio cualitativo |
|---|---|
| 1 | Excepcional: requiere condiciones poco plausibles y controles comprobados que dificultan el escenario |
| 2 | Posible: puede ocurrir, pero no hay señales de recurrencia ni una exposición cotidiana identificada |
| 3 | Plausible: existe una ruta concreta o una dependencia vulnerable; recurrencia no documentada |
| 4 | Probable en el escenario de exposición asumido: actividad frecuente y barrera insuficiente; exige confirmar esa exposición con el cliente |
| 5 | Casi segura: repetición documentada o fallo sistemático; no se asigna sin evidencia |

| Impacto | Criterio cualitativo |
|---|---|
| 1 | Efecto local menor, recuperable sin interrumpir el proceso ni divulgar datos |
| 2 | Corrección acotada; retraso breve o pocos registros operativos afectados |
| 3 | Retraso del ciclo o reproceso significativo, de alcance contenido |
| 4 | Directorio incorrecto, interrupción relevante o exposición de datos personales operativos |
| 5 | Exposición amplia de nómina o compromiso grave de integridad, acceso o control; requiere evaluación institucional |

Puntaje = P × I. Bajo 1–4; Medio 5–9; Alto 10–16; Crítico 17–25. No es una escala prescrita por STRIDE ni una valoración oficial de la Universidad. Un impacto 5 es una hipótesis sobre el alcance del incidente, no una afirmación de que todos los campos de nómina son datos sensibles en sentido legal.

## 2. Fundamento inicial por amenaza

La columna de fundamento distingue debilidad del proceso y supuestos técnicos que faltan por comprobar. Los valores 4 no prueban que haya ocurrido un incidente.

| ID | P | I | Fundamento / evidencia pendiente |
|---|---:|---:|---|
| S-01 | 4 | 4 | Respuestas son parte habitual del proceso. Se asume exposición a suplantación si identidad/confirmación no se validan; MFA y permisos reales por comprobar. Un cierre falso altera el directorio. |
| S-02 | 3 | 4 | Conexiones y propietarios aún no inventariados; existe ruta de uso de una identidad incorrecta, no incidentes demostrados. Puede comprometer integridad y continuidad. |
| T-01 | 3 | 5 | Insumo pasa por descarga y preparación; se debe probar integridad. Se asume impacto amplio porque una nómina alterada puede afectar muchas conciliaciones. |
| T-02 | 4 | 5 | La cliente explicó la ambigüedad de cargo/unidad y el problema del cruce. Llave errónea reutilizada puede asociar masivamente personas y extensiones; comprobar conteos en muestra. |
| T-03 | 4 | 4 | Edición directa es una actividad habitual del proceso actual. Se asumen barreras insuficientes hasta probar permisos; saltar validación deja información incorrecta. |
| T-04 | 3 | 5 | Flujo propuesto depende de configuración aún no gobernada. Un cambio no autorizado puede alterar destinatarios o toda la lógica; no hay evidencia de ataque ocurrido. |
| R-01 | 4 | 4 | Coordinación por Teams/reuniones declarada, sin registro estructurado de confirmación. Se valora probable falta de evidencia, no la frecuencia de negaciones. |
| R-02 | 3 | 4 | Seguimiento manual e historial por comprobar; corrección sin motivo es plausible. Impide reconstruir cambios y resolver disputas. |
| I-01 | 4 | 4 | Cada novedad produce comunicación. Se asume riesgo frecuente si una plantilla copia el archivo/detalle completo; plantilla final inexistente. Exposición de datos operativos. |
| I-02 | 4 | 5 | Nómina completa se recibe cada mes. Supuesto conservador: permisos heredados/compartición pueden exponerla ampliamente; acceso efectivo aún no inventariado. Priorizar comprobación, no presumir configuración insegura como hecho. |
| I-03 | 3 | 5 | Repositorio público y evidencias académicas son una ruta plausible de divulgación. No se observó nómina/directorio real en el árbol revisado; impacto 5 si se publicara el archivo completo. |
| I-04 | 3 | 4 | Descargas y exportaciones generan copias; retención no confirmada. Exposición persistente y dificultad de eliminación, sin incidente demostrado. |
| D-01 | 4 | 4 | Se asume fragilidad si los flujos dependen de un propietario/conexión individual; propiedad final y monitoreo por definir. Detiene solicitudes y retorno. |
| D-02 | 3 | 4 | Cambios de esquema pueden romper Power Query, pero no hay historial de cambios recurrentes. Se ajusta P de 4 a 3; un fallo detiene la conciliación y exige reproceso. |
| D-03 | 3 | 3 | Límites y volumen real por comprobar. Ruta plausible de retraso; 5–10 cambios declarados no prueban saturación. Impacto contenido en notificaciones/ciclo. |
| D-04 | 2 | 4 | No hay evidencia de ausencia o carga fallida recurrente del corte esperado. Se ajusta P de 3 a 2. La entrega a mes vencido es una limitación conocida, distinta de no recibir el archivo en el calendario acordado. |
| E-01 | 3 | 4 | Matriz de roles sin configuración verificada. Privilegio excesivo es plausible; permitiría editar/borrar datos y controles. |
| E-02 | 3 | 5 | Gobierno de privilegios del tenant por revisar. Un operador con administración excesiva podría afectar múltiples recursos; no se afirma que ese acceso exista. |
| E-03 | 3 | 5 | Si la solicitud enlaza la nómina, Tecnología recibe información que no necesita. Ruta plausible de exposición del archivo completo; separar fuente y caso mínimo. |

Las demás puntuaciones se conservan como hipótesis iniciales; deben subir o bajar con evidencia de permisos, historial y pruebas. La revisión deja **17 de 19** escenarios inherentes altos/críticos, frente a 18 antes del ajuste. La concentración sigue siendo elevada: no demuestra incidentes ni justifica eliminar la validación. Debe comprobarse especialmente la exposición asumida en los escenarios con P=4.

## 3. Riesgo residual proyectado y decisiones

Los valores residuales de la matriz solo aplican **si** se implantan los controles y sus pruebas son satisfactorias. Mientras no exista esa evidencia, no se afirma una reducción efectiva respecto del puntaje inicial. Estado del control:

- Existe: evidencia de control actual en el alcance revisado.
- Parcial: parte del control está presente; queda una actividad o prueba por completar.
- Propuesto: diseño no implantado.
- Por validar: capacidad/configuración no comprobada.

**I-02** permanece en **2 × 5 = 10 (Alto)** aun en el escenario controlado, por el impacto de exponer el archivo completo. Tratamiento propuesto: Mitigar. No está aceptado ni se dispone de un acta de aceptación.

| Decisión | Responsable propuesto | Hito | Evidencia necesaria |
|---|---|---|---|
| Revisar I-02 y exposición real de nómina | TI SharePoint / Seguridad | Antes de usar datos reales | Inventario de grupos, herencia/enlaces, prueba por rol y segregación de biblioteca |
| Reevaluar residual I-02 y decidir tratamiento restante | Seguridad y dueño del proceso | Antes del piloto con datos reales | Resultado de pruebas y decisión documentada; si permanece Alto, aprobación expresa o controles adicionales |
| Mantener revisión I-03/REPO-01 | Equipo del proyecto | Antes de cada publicación | Archivos, capturas y metadatos revisados; .gitignore y comprobación de archivos ya versionados |

No se avanza a un piloto con datos reales suponiendo que esas decisiones están resueltas. El cierre académico del Corte 2 entrega el análisis y sus brechas, no una aceptación de riesgos por parte de la Universidad.

## 4. Orden de trabajo propuesto

1. Biblioteca de nómina y mínimo privilegio (I-02/E-03): reducir exposición y resolver el residual Alto.
2. Llaves, completitud del corte y validación del cruce (T-02/T-01/D-02): evitar cambios sobre personas equivocadas.
3. Identidad, permisos y confirmación trazable (S-01/T-03/R-01): probar transiciones y respuestas tardías.
4. Continuidad, calendario y monitoreo (D-01/D-04): probar falla de conexión, ausencia de archivo y contingencia manual.

Este orden combina puntaje, alcance y dependencia entre controles; no se interpreta que todos los riesgos Altos tengan la misma urgencia. Método STRIDE y documentación técnica: [referencias](referencias.md).
