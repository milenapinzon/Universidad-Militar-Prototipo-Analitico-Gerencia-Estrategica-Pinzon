## Contexto del problema

| Elemento | Definición |
|---|---|
| Usuario | Diseñador de Tags |
| Insight principal | “Esa no era la última versión; ahora toca volver a corregir la etiqueta.” El diseñador pierde tiempo y se frustra cuando no puede identificar con claridad cuál es la versión vigente. |
| Problema central | El proceso de desarrollo y gestión de etiquetas presenta fallas de trazabilidad y coordinación en el manejo de cambios, versiones y validaciones entre las áreas involucradas. |
| Causa priorizada | **Gestión fragmentada de versiones y cambios de etiquetas mediante archivos y canales no integrados.** |

## Tabla de priorización

| Causa | Fase DT origen (E/D/I) | Insight de empatía | Supuesto central | Pregunta analítica | Variables (nombres exactos) | Tipo (Outcome / Explic / Control / Segmento) | Cálculo / Transformación | Métrica (nombre + fórmula) | Periodo / Segmento | Patrón esperado (si cierta) | Condición refutación | Valor esperado para usuario/ciudadano | Riesgo si falsa | Acción si confirma | Acción si refuta | Experimento analítico mínimo (query + visual 1 línea) | Estado (V/A/R) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Gestión fragmentada de versiones y cambios de etiquetas mediante archivos y canales no integrados.** | Definir | “Esa no era la última versión; ahora toca volver a corregir la etiqueta.” El diseñador pierde tiempo y se frustra cuando no puede identificar con claridad cuál es la versión vigente. | Si una solicitud de etiqueta presenta problemas de trazabilidad, por uso de un canal informal o de una versión no vigente, entonces tendrá un mayor porcentaje de reprocesos que una solicitud sin esos problemas. | ¿Las solicitudes de etiquetas con problemas de trazabilidad presentan un mayor porcentaje de reprocesos que las solicitudes sin problemas de trazabilidad durante junio de 2026? | `reproceso_flag`<br>`problema_trazabilidad_flag`<br>`canal_informal_flag`<br>`version_incorrecta_flag`<br>`id_solicitud`<br>`semana`<br>`prioridad`<br>`area_solicitante`<br>`tipo_etiqueta` | Outcome: `reproceso_flag`<br>Explicativa: `problema_trazabilidad_flag`<br>Explicativa: `canal_informal_flag`<br>Explicativa: `version_incorrecta_flag`<br>Control: `id_solicitud`<br>Control: `semana`<br>Control: `prioridad`<br>Segmento: `area_solicitante`<br>Segmento: `tipo_etiqueta` | 1. Crear `problema_trazabilidad_flag`:<br>• `1` si `canal_informal_flag = 1` O `version_incorrecta_flag = 1`.<br>• `0` si `canal_informal_flag = 0` Y `version_incorrecta_flag = 0`.<br>2. Contar cada `id_solicitud` una sola vez.<br>3. Calcular `TR_PT`: porcentaje de reprocesos de las solicitudes con problemas de trazabilidad.<br>4. Calcular `TR_SPT`: porcentaje de reprocesos de las solicitudes sin problemas de trazabilidad.<br>5. Restar ambas tasas para obtener la diferencia en puntos porcentuales.<br>6. Revisar el resultado por `semana`, `prioridad`, `area_solicitante` y `tipo_etiqueta`. | **Diferencia del Porcentaje de Reprocesos por Problemas de Trazabilidad (DPRPT)**<br><br>`DPRPT = TR_PT − TR_SPT`<br><br>`TR_PT` = porcentaje de reprocesos de las solicitudes con problemas de trazabilidad.<br>`TR_SPT` = porcentaje de reprocesos de las solicitudes sin problemas de trazabilidad. | Junio de 2026. Cálculo mensual, con revisión semanal y segmentación por `area_solicitante` y `tipo_etiqueta`. | **DPRPT ≥ 5 puntos porcentuales. El porcentaje de reprocesos del grupo con problemas de trazabilidad debe superar en al menos 5 p.p. al porcentaje del grupo sin problemas.** | **DPRPT < 5 puntos porcentuales. Si la diferencia es menor de 5 p.p., igual a cero o negativa, la hipótesis se refuta.** | Reducir correcciones evitables, pérdida de tiempo y frustración asociadas con el uso de versiones incorrectas o canales informales. | Invertir tiempo y recursos en una causa que no explica suficientemente los reprocesos y dejar de analizar otras causas relevantes del proceso. | Priorizar el diseño de una prueba piloto de control de versiones, canal oficial y reglas de validación para las solicitudes de etiquetas. | Descartar esta causa como prioridad y analizar la siguiente causa del árbol: aplicación variable de criterios, responsabilidades y secuencias de validación entre áreas. | **QUERY:** `Crear problema_trazabilidad_flag, calcular la DPRPT de junio de 2026 y revisar el resultado por semana, área y tipo de etiqueta.`<br>**VISUAL:** `Gráfico de barras con el porcentaje de reprocesos de los dos grupos y la diferencia en puntos porcentuales.` | V |

## Ficha de indicador

| Campo | Definición |
|---|---|
| Supuesto central | Si una solicitud de etiqueta presenta problemas de trazabilidad, por uso de un canal informal o de una versión no vigente, entonces tendrá un mayor porcentaje de reprocesos que una solicitud sin esos problemas. |
| ¿Qué hago? (Acción) | Medir si los problemas de trazabilidad están asociados con un mayor porcentaje de reprocesos. |
| ¿Cómo lo hago? (Método) | Clasificando las solicitudes en dos grupos con y sin problemas de trazabilidad y calculando la diferencia entre sus porcentajes de reproceso. |
| ¿Para qué lo hago? (Propósito) | Determinar con datos si esta causa debe priorizarse para una prueba piloto o si debe analizarse otra causa del árbol. |
| Aspecto específico a medir | Diferencia del porcentaje de reprocesos entre solicitudes con problemas de trazabilidad y solicitudes sin problemas de trazabilidad. |
| Público objetivo (Para quién) | Áreas involucradas en la creación, modificación, validación y aprobación de etiquetas. |
| Dimensión | Eficacia: ¿logra el resultado? |
| Nombre del indicador | **Diferencia del Porcentaje de Reprocesos por Problemas de Trazabilidad (DPRPT)** |
| Numerador (Variable Y) | Solicitudes con `reproceso_flag = 1` dentro de cada grupo de trazabilidad. |
| Denominador (Población) | Total de solicitudes únicas de cada grupo, identificadas mediante `id_solicitud`. |
| Fórmula matemática | Ver bloque de fórmula. |
| Prueba de estrés | • Evitar división por cero si uno de los grupos no tiene solicitudes.<br>• Contar cada `id_solicitud` una sola vez.<br>• Verificar que `problema_trazabilidad_flag` se derive correctamente de `canal_informal_flag` y `version_incorrecta_flag`.<br>• Revisar posibles subregistros de reprocesos.<br>• Comprobar que la diferencia no dependa únicamente de una `semana`, `prioridad`, `area_solicitante` o `tipo_etiqueta`. |
| Tipo | Porcentaje (%). La diferencia entre los porcentajes se expresa en puntos porcentuales (p.p.). |
| Frecuencia de medición | Mensual: una vez al mes. |
| Fuente de datos (Verificación) | Matriz de seguimiento de solicitudes de etiquetas correspondiente a junio de 2026. |
| Línea base (Patrón actual) | Porcentaje de reprocesos observado en las solicitudes sin problemas de trazabilidad durante junio de 2026. Este valor se calculará al procesar la base y funcionará como grupo de referencia. |
| Patrón esperado (Meta) | **DPRPT ≥ 5 p.p. El porcentaje de reprocesos del grupo con problemas de trazabilidad debe ser al menos 5 puntos porcentuales mayor que el porcentaje del grupo de referencia.** |
| Condición de refutación (Fallo) | **DPRPT < 5 p.p. Si la diferencia es menor de 5 puntos porcentuales, igual a cero o negativa, la hipótesis se considera refutada.** |

```text
DPRPT = TR_PT − TR_SPT

TR_PT =
(Solicitudes con problema_trazabilidad_flag = 1 y reproceso_flag = 1
/ Total de solicitudes con problema_trazabilidad_flag = 1) × 100

TR_SPT =
(Solicitudes con problema_trazabilidad_flag = 0 y reproceso_flag = 1
/ Total de solicitudes con problema_trazabilidad_flag = 0) × 100
```
