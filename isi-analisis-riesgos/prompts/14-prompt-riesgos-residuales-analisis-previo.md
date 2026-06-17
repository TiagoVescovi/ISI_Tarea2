# Registro de prompt — análisis previo riesgos residuales

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-18 09:00 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/07-riesgos-residuales.md` (análisis previo; documento aún no redactado) |

## Prompt exacto

```text
Antes de redactar docs/07-riesgos-residuales.md realizá un análisis previo completo siguiendo la misma metodología utilizada en 05 y 06.

Necesito:

Inventario de todos los riesgos residuales que surgen de las 16 amenazas tratadas en 06-plan-mitigacion.md.
Trazabilidad obligatoria:
03-analisis-stride.md → 04-priorizacion-dread.md → 05-mapa-attack.md → 06-plan-mitigacion.md.
Identificación de controles aplicados y justificación de por qué el riesgo no puede eliminarse completamente.
Clasificación del residual (Bajo, Medio o Alto) con criterio consistente respecto a DREAD.
Riesgos derivados de:
uso de LLM comercial (TB-07),
Pinecone/SaaS si aplica (TB-10),
IdP externo,
alucinaciones del modelo,
errores humanos durante revisión legal.
Inconsistencias o limitaciones metodológicas detectadas.
Propuesta completa de estructura para docs/07-riesgos-residuales.md.
Tabla resumen con las 16 amenazas y su riesgo residual esperado.
No modificar archivos. Mostrar primero el análisis completo para revisión.

Indicá explícitamente cualquier supuesto utilizado y cualquier punto donde la documentación de references/ no permita justificar una reducción adicional del riesgo.
```

## Resultado de la asistencia

Análisis previo completo de `docs/07-riesgos-residuales.md`: inventario de 16 residuales, trazabilidad 03→06, clasificación cualitativa Bajo/Medio/Alto, sección transversal (TB-07, TB-10, IdP, alucinaciones, factor humano), limitaciones metodológicas, tabla resumen y propuesta de estructura. Preguntas D1–D5 para confirmación. **Sin** modificación de archivos en el repositorio en esa sesión.
