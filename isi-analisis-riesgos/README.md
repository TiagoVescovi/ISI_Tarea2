# ISI — Análisis de riesgos

## Datos del proyecto

| Campo | Valor |
|--------|--------|
| **Alumno** | Tiago Vescovi |
| **Docente** | Andrés Pastorini |
| **Materia** | Introducción a la Seguridad Informática (ISI) |
| **Año** | 2026 |

## Escenario seleccionado

**Escenario 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG)**

Una firma de servicios profesionales desarrolla un ecosistema interno y orientado a clientes para consulta automatizada de documentos legales, contratos y jurisprudencia mediante LLMs. El sistema contempla, entre otras capacidades: carga de contratos confidenciales (PDF/Word); consultas en lenguaje natural con respuestas fundamentadas en documentación cargada (RAG); agentes de IA para borradores preliminares de contratos comerciales a partir de plantillas; y auditoría automatizada de cumplimiento normativo local.

**Alcance documentado para el análisis (descripción del escenario, no resultado del análisis):** orquestador de IA (LangChain / LlamaIndex) en Python; base de datos vectorial (Chroma o Pinecone); integración por API con LLM comercial externo (OpenAI / Anthropic); LLM open source alojado localmente para datos altamente sensibles; frontend en Next.js; autenticación federada OAuth2; pipeline ETL asíncrono con RabbitMQ.

## Objetivo general

Realizar el trabajo académico de ISI sobre **análisis de riesgos** del escenario 11, aplicando la metodología y entregables que exija la materia, con trazabilidad y cumplimiento de la política de uso de IA.

## Estado actual

| Entregable | Estado |
|------------|--------|
| `docs/01-inventario-activos.md` | Completado |
| `docs/02-arquitectura-trust-boundaries.md` | Completado |
| `docs/03-analisis-stride.md` | Completado (63 IDs `STR-*`) |
| `docs/04-priorizacion-dread.md` | Completado (16 amenazas evaluadas) |
| `docs/05-mapa-attack.md` | Completado (16 amenazas; 15 mapeadas a ATT&CK) |
| `docs/06-plan-mitigacion.md` | Completado (16 amenazas) |
| `docs/07-riesgos-residuales.md` | Completado (16 amenazas; evaluación cualitativa residual) |
| `diagrams/` | Pendiente |
| `tools/` | Pendiente |

## Herramientas de IA utilizadas

Registrar cada herramienta empleada. Los **prompts** (fecha y hora, herramienta, entregable vinculado si aplica, texto exacto) van en archivos bajo `prompts/`, según [`prompts/README.md`](prompts/README.md).

| Fecha y hora (UTC-3) | Herramienta | Uso breve |
|----------------------|-------------|-----------|
| 2026-05-14 19:30 | Cursor (asistente IA) | Comprensión del sistema (arquitectura conceptual) — [`prompts/03-prompt-entender-proyecto.md`](prompts/03-prompt-entender-proyecto.md) |
| 2026-05-14 20:30 | Cursor (asistente IA) | Inventario de activos (`docs/01-inventario-activos.md`) — [`prompts/04-prompt-inventario-activos.md`](prompts/04-prompt-inventario-activos.md) |
| 2026-05-28 23:00 | Cursor (asistente IA) | Estructura inicial del repositorio — [`prompts/01-prompt-inicial.md`](prompts/01-prompt-inicial.md) |
| 2026-05-30 18:30 | Cursor (asistente IA) | Actualización al escenario 11 y plantillas — [`prompts/02-prompt-contexto.md`](prompts/02-prompt-contexto.md) |
| 2026-06-16 16:15 | Cursor (asistente IA) | Trust boundaries (`docs/02-arquitectura-trust-boundaries.md`) — [`prompts/05-prompt-trust-boundaries.md`](prompts/05-prompt-trust-boundaries.md) |
| 2026-06-17 10:30 | Cursor (asistente IA) | Análisis STRIDE (`docs/03-analisis-stride.md`) — [`prompts/06-prompt-matriz-stride.md`](prompts/06-prompt-matriz-stride.md) |
| 2026-06-17 15:00 | Cursor (asistente IA) | Revisión y consolidación STRIDE (`docs/03-analisis-stride.md`) — [`prompts/07-prompt-revision-stride.md`](prompts/07-prompt-revision-stride.md) |
| 2026-06-17 18:00 | Cursor (asistente IA) | Priorización DREAD (`docs/04-priorizacion-dread.md`) — [`prompts/08-prompt-priorizacion-dread.md`](prompts/08-prompt-priorizacion-dread.md) |
| 2026-06-17 20:00 | Cursor (asistente IA) | Propuesta plan de mitigación (`docs/06-plan-mitigacion.md`) — [`prompts/10-prompt-plan-mitigacion.md`](prompts/10-prompt-plan-mitigacion.md) |
| 2026-06-17 21:00 | Cursor (asistente IA) | Propuesta mapa ATT&CK (`docs/05-mapa-attack.md`) — [`prompts/09-prompt-mapa-attack.md`](prompts/09-prompt-mapa-attack.md) |
| 2026-06-17 22:30 | Cursor (asistente IA) | Mapa ATT&CK (`docs/05-mapa-attack.md`) — [`prompts/11-prompt-mapa-attack-redaccion.md`](prompts/11-prompt-mapa-attack-redaccion.md) |
| 2026-06-17 23:00 | Cursor (asistente IA) | Análisis previo plan de mitigación (`docs/06-plan-mitigacion.md`) — [`prompts/12-prompt-plan-mitigacion-analisis-previo.md`](prompts/12-prompt-plan-mitigacion-analisis-previo.md) |
| 2026-06-17 23:45 | Cursor (asistente IA) | Plan de mitigación (`docs/06-plan-mitigacion.md`) — [`prompts/13-prompt-plan-mitigacion-redaccion.md`](prompts/13-prompt-plan-mitigacion-redaccion.md) |
| 2026-06-18 09:00 | Cursor (asistente IA) | Análisis previo riesgos residuales (`docs/07-riesgos-residuales.md`) — [`prompts/14-prompt-riesgos-residuales-analisis-previo.md`](prompts/14-prompt-riesgos-residuales-analisis-previo.md) |
| 2026-06-18 09:45 | Cursor (asistente IA) | Riesgos residuales (`docs/07-riesgos-residuales.md`) — [`prompts/15-prompt-riesgos-residuales-redaccion.md`](prompts/15-prompt-riesgos-residuales-redaccion.md) |

## Estructura del repositorio

Todo el material generado se almacena en este repositorio Git **público**. Estructura sugerida por la materia:

```text
isi-analisis-riesgos/
├── README.md                    # Descripción del grupo y escenario elegido
├── prompts/                     # (Obligatorio si usan IA) Prompts utilizados
│   ├── 00-memory-bank.md        # Archivo de contexto para la IA
│   ├── 01-prompt-inicial.md
│   ├── 02-prompt-contexto.md
│   └── …                        # Un registro por interacción relevante
├── docs/
│   ├── 01-inventario-activos.md
│   ├── 02-arquitectura-trust-boundaries.md
│   ├── 03-analisis-stride.md
│   ├── 04-priorizacion-dread.md
│   ├── 05-mapa-attack.md
│   ├── 06-plan-mitigacion.md
│   └── 07-riesgos-residuales.md
├── diagrams/
│   ├── arquitectura.drawio
│   ├── arquitectura.png
│   └── attack-flow.png
├── templates/
│   └── Plantilla_Analisis_Riesgos.md
├── tools/
│   ├── threat-model.xml         # Exportado desde OWASP Threat Dragon / MS TMT
│   └── risk-matrix.xlsx         # Matriz de riesgos (si usa Excel)
└── references/
    ├── STRIDE.md
    ├── DREAD.md
    ├── MITRE_ATTCK
    ├── analisis_amenazas.md
    └── NIST-SP-800-30.pdf       # Normativas descargadas (TODO)
```

> **Nota:** Revisar el archivo `Recursos/Plantilla_Analisis_Riesgos.md` para el formato de documentación.

## Cómo registrar prompts

1. Crear un archivo en `prompts/` con **fecha y hora**, **herramienta**, **entregable vinculado** (si aplica) y **prompt exacto**.
2. Añadir una fila a la tabla **Herramientas de IA utilizadas** de este README.
3. Actualizar `prompts/00-memory-bank.md` si cambia el estado del proyecto o los entregables.

## Referencias de apoyo

Materiales de clase y normativas en [`references/`](references/). Ver [`references/bibliografia.md`](references/bibliografia.md).
