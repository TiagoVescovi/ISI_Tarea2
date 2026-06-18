# ISI — Análisis de riesgos

## Datos del proyecto

| Campo | Valor |
|--------|--------|
| **Alumno** | Tiago Vescovi |
| **Docente** | Andrés Pastorini |
| **Materia** | Introducción a la Seguridad Informática (ISI) |
| **Año** | 2026 |

## Escenario

**Escenario 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG)**

Firma de servicios profesionales con plataforma para clientes e internos: carga de contratos confidenciales, consultas RAG, borradores con agentes de IA y auditoría de cumplimiento normativo.

**Alcance técnico:** Next.js, OAuth2, LangChain/LlamaIndex, RabbitMQ, ETL, Chroma o Pinecone, LLM comercial y LLM local.

## Entregables

| Documento / artefacto | Ubicación |
|------------------------|-----------|
| Inventario de activos | `docs/01-inventario-activos.md` |
| Arquitectura y trust boundaries | `docs/02-arquitectura-trust-boundaries.md` |
| Análisis STRIDE | `docs/03-analisis-stride.md` |
| Priorización DREAD | `docs/04-priorizacion-dread.md` |
| Mapa ATT&CK | `docs/05-mapa-attack.md` |
| Plan de mitigación | `docs/06-plan-mitigacion.md` |
| Riesgos residuales | `docs/07-riesgos-residuales.md` |
| Diagramas | `diagrams/arquitectura.png`, `diagrams/attack-flow.png` |

## Herramientas de IA utilizadas

Registro de prompts en [`prompts/`](prompts/).

| Fecha (UTC-3) | Herramienta | Uso |
|---------------|-------------|-----|
| 2026-05-28 | Cursor | Estructura del repositorio |
| 2026-05-30 | Cursor | Contexto escenario 11 |
| 2026-06-14 | Cursor | Inventario de activos |
| 2026-06-16 | Cursor | Trust boundaries |
| 2026-06-17 | Cursor | STRIDE, DREAD, ATT&CK, mitigación |
| 2026-06-18 | Cursor | Riesgos residuales |
| 2026-06-18 | Cursor | Activos humanos en inventario (`prompts/16`) |
| 2026-06-18 | Gamma | Presentación oral (`prompts/17`) |

## Estructura

```text
isi-analisis-riesgos/
├── docs/                  # Análisis (01–07)
├── diagrams/              # PNG exportados desde draw.io
├── references/            # Material de clase
├── prompts/               # Registro de prompts (política IA)
└── templates/             # Plantilla de referencia del curso
```

Materiales de apoyo en [`references/`](references/).
