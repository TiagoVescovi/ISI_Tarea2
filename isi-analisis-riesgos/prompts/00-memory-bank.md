# Memory bank — contexto para IA

> Contexto central para futuras interacciones. Usar solo información del escenario provista por la materia y del análisis elaborado por el equipo; marcar **TODO: Completar durante la fase de análisis.** donde aún no exista trabajo validado.

## Contexto general del proyecto

- **Materia:** Introducción a la Seguridad Informática (ISI), 2026.
- **Alumno:** Tiago Vescovi.
- **Docente:** Andrés Pastorini.
- **Trabajo:** Análisis de riesgos sobre el escenario seleccionado.

## Escenario seleccionado

**Escenario 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG)**

- Firma de servicios profesionales; ecosistema interno y para clientes.
- Consulta automatizada de documentos legales, contratos y jurisprudencia con LLMs.
- Funcionalidades descritas en el enunciado: carga de contratos confidenciales (PDF/Word); consultas en lenguaje natural con respuestas basadas en documentación cargada (RAG); agentes de IA para borradores preliminares de contratos comerciales desde plantillas; auditoría automatizada de cumplimiento normativo local.

## Tecnologías identificadas

*(Lista proveniente del alcance del escenario; ampliación solo con análisis documentado.)*

- Frontend Next.js
- OAuth2 (autenticación federada)
- LangChain / LlamaIndex
- Python
- RabbitMQ
- Chroma o Pinecone (base de datos vectorial)
- OpenAI / Anthropic (LLM comercial externo vía API)
- LLM open source alojado localmente (datos altamente sensibles)
- Arquitectura RAG

## Metodologías previstas

*(Según materiales de clase en `references/`.)*

- STRIDE
- DREAD
- MITRE ATT&CK

## Entregables previstos

| Orden | Entregable | Documento / ubicación |
|--------|------------|------------------------|
| 1 | Inventario de activos | `docs/01-inventario-activos.md` |
| 2 | Arquitectura y trust boundaries | `docs/02-arquitectura-trust-boundaries.md` |
| 3 | Análisis STRIDE | `docs/03-analisis-stride.md` |
| 4 | Priorización DREAD | `docs/04-priorizacion-dread.md` |
| 5 | Mapa ATT&CK | `docs/05-mapa-attack.md` |
| 6 | Plan de mitigación | `docs/06-plan-mitigacion.md` |
| 7 | Riesgos residuales | `docs/07-riesgos-residuales.md` |

**Artefactos complementarios:** diagramas en `diagrams/`; modelos en `tools/`; plantilla en `templates/Plantilla_Analisis_Riesgos.md`.

## Estado actual

- **Escenario:** definido (11 — plataforma legal RAG/IA generativa).
- **Completado:** inventario de activos, trust boundaries, análisis STRIDE (63 IDs), priorización DREAD (16 amenazas), mapa ATT&CK (`05-mapa-attack.md`).
- **Pendiente:** plan de mitigación, riesgos residuales, diagramas y herramientas.

## Archivos importantes

| Ruta | Propósito |
|------|-----------|
| `README.md` | Datos del proyecto, escenario, estado, IA, estructura |
| `references/` | Apuntes de clase (STRIDE, DREAD, MITRE ATT&CK, análisis de amenazas) |
| `prompts/README.md` | Registro de prompts |
| `prompts/00-memory-bank.md` | Este memory bank |

## Reglas para futuras interacciones con IA

1. No inventar activos, límites de confianza, amenazas, controles ni cifras de riesgo: documentar solo lo fundado en escenario, consigna o fuentes citadas.
2. Usar **TODO: Completar durante la fase de análisis.** donde falte trabajo analítico no elaborado aún.
3. Registrar cada uso relevante: **fecha**, **herramienta**, **prompt exacto** en `prompts/`; actualizar la tabla del `README.md` principal.
4. No adelantar entregables posteriores a la fase actual (p. ej. mitigaciones definitivas antes de completar el mapa ATT&CK) salvo que la consigna lo autorice.
5. Mantener coherencia entre README, memory bank y registros de prompts.

## Requisitos obligatorios de IA (cumplimiento permanente)

- Registro de prompts en `prompts/`.
- Archivo `prompts/00-memory-bank.md` actualizado cuando cambie el contexto o el estado.
- Sección **Herramientas de IA utilizadas** en `README.md`.
- Cada registro con **fecha**, **herramienta** y **prompt exacto**.

## TODO

- [ ] Completar `docs/06-plan-mitigacion.md` y `docs/07-riesgos-residuales.md`.
- [ ] Generar diagramas en `diagrams/` y artefactos en `tools/` según consigna.
