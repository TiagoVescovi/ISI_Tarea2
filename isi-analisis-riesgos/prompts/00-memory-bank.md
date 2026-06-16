# Memory bank — contexto para IA

> Contexto central para futuras interacciones. Usar solo información del escenario provista por la materia y del análisis elaborado por el equipo; marcar **TODO: Completar durante la fase de análisis.** donde aún no exista trabajo validado.

## Contexto general del proyecto

- **Materia:** Introducción a la Seguridad Informática (ISI), 2026.
- **Alumno:** Tiago Vescovi.
- **Docente:** Andrés Pastorini.
- **Trabajo:** Análisis de riesgos sobre el escenario seleccionado (preparación y luego fase de análisis).

## Escenario seleccionado

**Escenario 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG)**

- Firma de servicios profesionales; ecosistema interno y para clientes.
- Consulta automatizada de documentos legales, contratos y jurisprudencia con LLMs.
- Funcionalidades descritas en el enunciado: carga de contratos confidenciales (PDF/Word); consultas en lenguaje natural con respuestas basadas en documentación cargada (RAG); agentes de IA para borradores preliminares de contratos comerciales desde plantillas; auditoría automatizada de cumplimiento normativo local.

## Tecnologías identificadas

*(Lista proveniente del alcance del escenario; ampliación solo con análisis documentado.)*

- Frontend Next.js
- OAuth2 (autenticación federada)
- LangChain
- LlamaIndex
- Python
- RabbitMQ
- Chroma o Pinecone (base de datos vectorial)
- OpenAI / Anthropic (LLM comercial externo vía API)
- LLM open source alojado localmente (datos altamente sensibles)
- Arquitectura RAG

## Metodologías previstas

*(Previstas para el trabajo de la materia; sin completar matrices ni hallazgos en esta fase.)*

- STRIDE
- DREAD
- MITRE ATT&CK
- MITRE ATLAS
- OWASP Top 10 for LLM Applications
- NIST SP 800-30
- NIST SP 800-53
- NIST AI RMF
- ISO 27001

## Entregables previstos

| Orden | Entregable | Documento / ubicación |
|--------|------------|------------------------|
| — | Índice y trazabilidad | `docs/00-indice-documentos.md` |
| 1 | Inventario de activos | `docs/01-inventario-activos.md` |
| 2 | Arquitectura y trust boundaries | `docs/02-arquitectura-trust-boundaries.md` |
| 3 | Matriz STRIDE | `docs/03-matriz-stride.md` |
| 4 | Priorización DREAD | `docs/04-priorizacion-dread.md` |
| 5 | Mapeo ATT&CK / ATLAS | `docs/05-mapeo-attck-atlas.md` |
| 6 | Plan de mitigación | `docs/06-plan-mitigacion.md` |
| 7 | Riesgos residuales | `docs/07-riesgos-residuales.md` |
| 8 | Controles NIST / ISO | `docs/08-controles-nist-iso.md` |
| 9 | Conclusiones | `docs/09-conclusiones.md` |
| 10 | Diagrama de flujo de datos (DFD) | `docs/10-diagrama-flujo-datos.md` + artefactos en `diagrams/` |

**TODO: Completar durante la fase de análisis** — verificar contra el enunciado oficial de la cátedra si hay entregables o formatos adicionales.

## Estado actual

- **Escenario:** definido (11 — plataforma legal RAG/IA generativa).
- **Fase:** preparación de repositorio, referencias base y esqueletos de documentos; **sin** análisis de riesgos completado.

## Archivos importantes

| Ruta | Propósito |
|------|-----------|
| `README.md` | Datos del proyecto, escenario, estado, IA, estructura |
| `docs/00-indice-documentos.md` | Trazabilidad entre entregables |
| `references/bibliografia.md` | Referencias normativas y técnicas |
| `prompts/README.md` | Registro de prompts |
| `prompts/00-memory-bank.md` | Este memory bank |

## Reglas para futuras interacciones con IA

1. No inventar activos, límites de confianza, amenazas, controles ni cifras de riesgo: documentar solo lo fundado en escenario, consigna o fuentes citadas.
2. Usar **TODO: Completar durante la fase de análisis.** donde falte trabajo analítico no elaborado aún.
3. Registrar cada uso relevante: **fecha**, **herramienta**, **prompt exacto** en `prompts/`; actualizar la tabla del `README.md` principal.
4. No adelantar STRIDE, DREAD, ATT&CK/ATLAS completos, OWASP LLM Top 10 como matriz cerrada, matrices de riesgo cuantitativas, mitigaciones definitivas ni conclusiones finales salvo que la fase de análisis y la consigna lo autoricen.
5. Mantener coherencia entre README, memory bank, índice en `docs/` y registros de prompts.

## Requisitos obligatorios de IA (cumplimiento permanente)

- Registro de prompts en `prompts/`.
- Archivo `prompts/00-memory-bank.md` actualizado cuando cambie el contexto o el estado.
- Sección **Herramientas de IA utilizadas** en `README.md`.
- Cada registro con **fecha**, **herramienta** y **prompt exacto**.
- Trazabilidad de la asistencia generada por IA (vínculo con índice y documentos afectados cuando aplique).

## TODO

- [ ] **TODO: Completar durante la fase de análisis** — inventario de activos y límites de confianza validados.
- [ ] **TODO: Completar durante la fase de análisis** — matrices y mapeos (STRIDE, DREAD, ATT&CK/ATLAS, OWASP LLM).
- [ ] **TODO: Completar durante la fase de análisis** — controles, mitigación, riesgos residuales, conclusiones y DFD definitivo.
- [ ] Verificar lista de entregables contra consigna oficial de ISI 2026.
