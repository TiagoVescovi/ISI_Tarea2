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

> **Nota:** Los componentes anteriores provienen del enunciado del escenario. Cualquier detalle adicional de arquitectura o activos queda **TODO: Completar durante la fase de análisis.**

## Objetivo general

Realizar el trabajo académico de ISI sobre **análisis de riesgos** del escenario 11, aplicando la metodología y entregables que exija la materia, con trazabilidad y cumplimiento de la política de uso de IA.

## Estado actual

**Escenario definido (11). Fase de preparación del repositorio y plantillas documentales.**

El análisis (activos detallados, amenazas, STRIDE, DREAD, mapeos MITRE, controles, mitigaciones, riesgos residuales, conclusiones y diagramas definitivos) **no está iniciado** en esta etapa. Los documentos bajo `docs/` son esqueletos con marcadores TODO.

## Trazabilidad de documentos

El mapa maestro de entregables y rutas está en [`docs/00-indice-documentos.md`](docs/00-indice-documentos.md).

## Herramientas de IA utilizadas

Registrar cada herramienta empleada. Los **prompts** (fecha y hora, herramienta, entregable vinculado si aplica, texto exacto) van en archivos bajo `prompts/`, según [`prompts/README.md`](prompts/README.md).

| Fecha y hora (UTC-3) | Herramienta | Uso breve |
|----------------------|-------------|-----------|
| 2026-05-14 19:30 | Cursor (asistente IA) | Comprensión del sistema (arquitectura conceptual) — [`prompts/03-prompt-entender-proyecto.md`](prompts/03-prompt-entender-proyecto.md) |
| 2026-05-14 20:30 | Cursor (asistente IA) | Inventario de activos (`docs/01-inventario-activos.md`) — [`prompts/04-prompt-inventario-activos.md`](prompts/04-prompt-inventario-activos.md) |
| 2026-05-28 23:00 | Cursor (asistente IA) | Estructura inicial del repositorio — [`prompts/01-prompt-inicial.md`](prompts/01-prompt-inicial.md) |
| 2026-05-30 18:30 | Cursor (asistente IA) | Actualización al escenario 11 y plantillas — [`prompts/02-prompt-contexto.md`](prompts/02-prompt-contexto.md) |
| 2026-06-16 16:15 | Cursor (asistente IA) | Trust boundaries (`docs/02-arquitectura-trust-boundaries.md`) — [`prompts/05-prompt-trust-boundaries.md`](prompts/05-prompt-trust-boundaries.md) |
| <!-- TODO: próximos usos de IA --> | | |

## Estructura del repositorio

```text
isi-analisis-riesgos/
├── README.md                      # Este archivo
├── prompts/
│   ├── 00-memory-bank.md          # Contexto central para IA (escenario, reglas)
│   ├── 01-prompt-inicial.md       # Registro de prompt (estructura inicial)
│   ├── 02-prompt-contexto.md      # Registro de prompt (escenario 11)
│   ├── 03-prompt-entender-proyecto.md
│   ├── 04-prompt-inventario-activos.md
│   ├── 05-prompt-trust-boundaries.md
│   └── README.md                  # Convención de registro de prompts
├── docs/
│   ├── README.md                  # Guía de la carpeta docs
│   ├── 00-indice-documentos.md    # Trazabilidad entre entregables
│   ├── 01-inventario-activos.md
│   ├── 02-arquitectura-trust-boundaries.md
│   ├── 03-matriz-stride.md
│   ├── 04-priorizacion-dread.md
│   ├── 05-mapeo-attck-atlas.md
│   ├── 06-plan-mitigacion.md
│   ├── 07-riesgos-residuales.md
│   ├── 08-controles-nist-iso.md
│   ├── 09-conclusiones.md
│   └── 10-diagrama-flujo-datos.md # Narrativa / referencia al DFD en diagrams/
├── diagrams/                      # DFD y otros diagramas (cuando correspondan)
├── templates/                     # Plantillas reutilizables
│   └── plantilla-seccion.md       # Esqueleto genérico de sección
├── tools/                         # Utilidades opcionales
└── references/
    ├── README.md
    └── bibliografia.md            # Referencias normativas y técnicas del escenario
```

## Cómo registrar prompts

1. Crear un archivo en `prompts/` (p. ej. `04-prompt-inventario-activos.md`) con **fecha y hora**, **herramienta**, **entregable vinculado** (si aplica) y **prompt exacto**.  
2. Añadir una fila a la tabla **Herramientas de IA utilizadas** de este README.  
3. Actualizar `prompts/00-memory-bank.md` si cambia el estado del proyecto o los entregables.

## Referencias de apoyo (lista del escenario)

Para citas formales y enlaces, usar [`references/bibliografia.md`](references/bibliografia.md). Incluye las fuentes indicadas en el escenario (OWASP LLM Top 10, MITRE ATLAS, NIST AI RMF, NIST SP 800-30, NIST SP 800-53, ISO 27001, Ley 18.331, guías AGESIC).
