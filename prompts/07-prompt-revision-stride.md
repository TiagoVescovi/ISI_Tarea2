# Registro de prompt — revisión matriz STRIDE

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 15:00 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/03-analisis-stride.md` (revisión crítica y consolidación) |

## Prompt exacto

```text
Revisá el documento 03-matriz-stride.md teniendo en cuenta el contenido ya definido en:

01-inventario-activos.md
02-arquitectura-trust-boundaries.md

Necesito que hagas una revisión crítica y mejora de la matriz STRIDE actual.

Objetivos:

Detectar amenazas duplicadas o excesivamente similares entre:

amenazas modeladas por componente (STR-Cxx-*)
amenazas modeladas por trust boundary (STR-TBxx-*)

Cuando dos amenazas representen esencialmente el mismo problema de seguridad, consolidarlas o eliminar redundancias para evitar inflar artificialmente el catálogo.

Incorporar amenazas específicas de sistemas de IA generativa y RAG cuando corresponda, utilizando terminología reconocida en la industria. Evaluar especialmente:
Prompt Injection
Indirect Prompt Injection
Data Poisoning
Retrieval Leakage
Hallucination con impacto sobre procesos legales
Manipulación de contexto recuperado
Exposición de información sensible mediante RAG
Revisar la cobertura de categorías STRIDE para cada componente y trust boundary.
No es obligatorio que todos tengan las seis categorías.
Si una categoría no aplica, evitar agregar amenazas artificiales.
Verificar que las categorías presentes estén justificadas y sean coherentes con el escenario.
Mantener la estructura documental actual:
Introducción
Leyenda STRIDE
Parte A (componentes)
Parte B (trust boundaries)
Resumen del catálogo
Relación con otros entregables
Trazabilidad
Mantener los identificadores existentes siempre que sea posible.
Si una amenaza se elimina por redundancia, explicar brevemente el motivo.
Si se crean nuevas amenazas, asignar IDs consistentes con el esquema actual.
Priorizar calidad y trazabilidad por encima de cantidad.
No busco maximizar el número de amenazas.
Prefiero una matriz más pequeña pero mejor justificada y más fácil de utilizar posteriormente en:
04-priorizacion-dread.md
05-mapeo-attck-atlas.md
06-plan-mitigacion.md
07-riesgos-residuales.md
08-controles-nist-iso.md
Evitar inventar componentes, tecnologías, flujos o activos que no estén presentes en:
el escenario original,
el inventario de activos,
la arquitectura y trust boundaries ya documentados.

Resultado esperado:

Generar una nueva versión completa de 03-matriz-stride.md, coherente con los documentos existentes, eliminando redundancias, incorporando amenazas relevantes de IA generativa/RAG y dejando un catálogo más equilibrado y mantenible para las siguientes etapas del trabajo.
```

## Nota de trazabilidad

Revisión: 80 → **63 IDs únicos**; apéndice de consolidaciones; nuevas amenazas STR-C05-T2, STR-C12-T2, STR-C12-T3; Parte B reducida a amenazas propias del cruce de frontera.
