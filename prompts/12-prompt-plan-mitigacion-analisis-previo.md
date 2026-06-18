# Registro de prompt — análisis previo plan de mitigación

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 23:00 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/06-plan-mitigacion.md` (análisis previo; documento aún no redactado) |

## Prompt exacto

```text
Necesito que prepares un análisis previo completo para redactar `docs/06-plan-mitigacion.md`, pero NO modifiques ningún archivo todavía.

Trabajá como si fueras un integrante más del grupo y seguí estas reglas:

* Primero analizá toda la documentación existente.
* Detectá inconsistencias, faltantes o contradicciones entre los entregables anteriores.
* No inventes amenazas, activos, trust boundaries, técnicas ATT&CK ni controles que no estén documentados en el proyecto o en `references/`.
* Antes de redactar el documento, explicame detalladamente qué vas a hacer y qué información vas a utilizar.
* Si encontrás problemas metodológicos, ambigüedades o decisiones que deban tomarse, detenete y preguntame antes de continuar.
* No generes todavía `docs/06-plan-mitigacion.md`.

Analizá especialmente:

* `docs/01-inventario-activos.md`
* `docs/02-arquitectura-trust-boundaries.md`
* `docs/03-analisis-stride.md`
* `docs/04-priorizacion-dread.md`
* `docs/05-mapa-attack.md`
* `prompts/00-memory-bank.md`
* Todo el contenido de `references/`

Necesito que me indiques:

1. Qué amenazas fueron priorizadas en DREAD y deben ser tratadas en el plan.
2. Qué activos y trust boundaries están involucrados en cada amenaza.
3. Qué tácticas y técnicas ATT&CK quedaron asociadas en `05-mapa-attack.md`.
4. Qué controles y mitigaciones aparecen realmente en los materiales de referencia disponibles.
5. Qué controles podrían reutilizarse de forma transversal para cubrir múltiples amenazas.
6. Qué amenazas deberían tratarse mediante:

   * Mitigar
   * Evitar
   * Transferir
   * Aceptar
7. Qué dependencias existen entre controles.
8. Qué amenazas deberían implementarse primero según prioridad DREAD.
9. Qué inconsistencias o limitaciones metodológicas existen entre STRIDE, DREAD y ATT&CK.
10. Una propuesta detallada de estructura para `docs/06-plan-mitigacion.md`.

Además, quiero que propongas:

* Tabla resumen de mitigaciones.
* Agrupación por prioridad de implementación.
* Agrupación por componente afectado.
* Agrupación por tipo de control (preventivo, detectivo, correctivo).
* Relación entre cada amenaza STRIDE, su score DREAD y sus controles asociados.

No redactes todavía el entregable.

Tu respuesta debe ser únicamente el análisis previo completo y las decisiones que necesitás que confirme antes de generar `docs/06-plan-mitigacion.md`.
```

## Resultado de la asistencia

Análisis previo completo de `docs/06-plan-mitigacion.md`: 16 amenazas DREAD, mapeo ATT&CK desde `05`, catálogo de controles en `references/`, controles transversales, tratamientos mitigar/evitar/transferir/aceptar, dependencias, priorización por fases, limitaciones metodológicas y propuesta de estructura. Preguntas D1–D6 para confirmación del usuario. **Sin** modificación de archivos en el repositorio en esa sesión.
