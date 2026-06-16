# Carpeta `prompts/`

Aquí se guardan los **registros de prompts** utilizados con herramientas de IA durante el proyecto, en cumplimiento de la política de la materia.

**Proyecto:** Escenario 11 — Plataforma de Asistencia Legal con IA generativa (RAG). El contexto vivo del escenario y las reglas para la IA están en [`00-memory-bank.md`](00-memory-bank.md).

## Uso

Por cada sesión o interacción que deba quedar documentada, crear un archivo (por ejemplo `04-prompt-inventario-activos.md`) que incluya como mínimo:

1. **Fecha y hora** (con zona horaria, p. ej. UTC-3)
2. **Herramienta utilizada** (nombre del producto o modelo, si aplica)
3. **Entregable vinculado** (ruta en el repo, si aplica)
4. **Prompt exacto** (copia textual)

Los archivos pueden ser breves; lo importante es la trazabilidad. Si un prompt influyó en un entregable concreto, enlazar en el registro al archivo bajo `docs/` correspondiente (ver [`../docs/00-indice-documentos.md`](../docs/00-indice-documentos.md)).

## Archivos en esta carpeta

Las **fechas y horas** coinciden con las registradas dentro de cada archivo (campo *Fecha y hora*). El número del archivo (`01` … `05`) no implica orden cronológico de las sesiones.

| Archivo | Contenido registrado (según cada prompt) |
|---------|------------------------------------------|
| `00-memory-bank.md` | Contexto vivo del proyecto: escenario 11, tecnologías, metodologías previstas, entregables, estado y reglas para la IA. |
| `01-prompt-inicial.md` | **2026-05-28 23:00 (UTC-3).** Pedido inicial: actuar como arquitecto/gestor académico ISI, **sin escenario definido**; crear estructura `isi-analisis-riesgos/`, carpeta `prompts/`, `00-memory-bank.md` solo con base y TODO, README con datos del proyecto y sección «Herramientas de IA utilizadas», README por carpeta; **no** inventario, STRIDE, DREAD, diagramas ni escenario ficticio. |
| `02-prompt-contexto.md` | **2026-05-30 18:30 (UTC-3).** **Actualización de contexto:** escenario 11 (plataforma legal RAG); adaptar README y memory bank, cubrir entregables con esqueletos en `docs/`, referencias en `references/`, trazabilidad; **no** análisis completo, STRIDE, DREAD, mitigaciones, amenazas inventadas ni conclusiones. |
| `03-prompt-entender-proyecto.md` | **2026-05-14 19:30 (UTC-3).** Antes del análisis de riesgos: **entender el sistema** a partir del escenario — componentes, flujo de datos punta a punta, actores, sistemas internos/externos, posibles trust boundaries, información faltante; **sin** STRIDE ni DREAD; validar arquitectura conceptual. Entregable en ese momento: solo orientación (sin archivo en repo). |
| `04-prompt-inventario-activos.md` | **2026-05-14 20:30 (UTC-3).** Construir **inventario de activos** (nombre, tipo, descripción, relevancia, criticidad preliminar, impacto CIA) con arquitectura ya validada; diferenciar información vs tecnología; **sin** STRIDE, DREAD, mitigaciones ni amenazas inventadas. → `docs/01-inventario-activos.md`. |
| `05-prompt-trust-boundaries.md` | **2026-06-16 16:15 (UTC-3).** Completar **trust boundaries** en `docs/02-arquitectura-trust-boundaries.md` (nombre, componentes, descripción, motivo del cambio de confianza, activos que cruzan, justificación); **sin** STRIDE, amenazas, DREAD, mitigaciones ni evaluación de riesgos. |
| `README.md` | Esta guía |
