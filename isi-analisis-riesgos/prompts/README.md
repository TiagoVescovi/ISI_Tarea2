# Carpeta `prompts/`

Aquí se guardan los **registros de prompts** utilizados con herramientas de IA durante el proyecto, en cumplimiento de la política de la materia.

**Proyecto:** Escenario 11 — Plataforma de Asistencia Legal con IA generativa (RAG). El contexto vivo del escenario y las reglas para la IA están en [`00-memory-bank.md`](00-memory-bank.md).

## Uso

Por cada sesión o interacción que deba quedar documentada, crear un archivo (por ejemplo `04-prompt-inventario-activos.md`) que incluya como mínimo:

1. **Fecha y hora** (con zona horaria, p. ej. UTC-3)
2. **Herramienta utilizada** (nombre del producto o modelo, si aplica)
3. **Entregable vinculado** (ruta en el repo, si aplica)
4. **Prompt exacto** (copia textual)

Los archivos pueden ser breves; lo importante es la trazabilidad. Si un prompt influyó en un entregable concreto, enlazar en el registro al archivo bajo `docs/` correspondiente (ver [`../docs/README.md`](../docs/README.md)).

## Archivos en esta carpeta

Las **fechas y horas** coinciden con las registradas dentro de cada archivo (campo *Fecha y hora*). El número del archivo (`01` … `07`) no implica orden cronológico de las sesiones.

| Archivo | Contenido registrado (según cada prompt) |
|---------|------------------------------------------|
| `00-memory-bank.md` | Contexto vivo del proyecto: escenario 11, tecnologías, metodologías previstas, entregables, estado y reglas para la IA. |
| `01-prompt-inicial.md` | **2026-05-28 23:00 (UTC-3).** Pedido inicial: actuar como arquitecto/gestor académico ISI, **sin escenario definido**; crear estructura `isi-analisis-riesgos/`, carpeta `prompts/`, `00-memory-bank.md` solo con base y TODO, README con datos del proyecto y sección «Herramientas de IA utilizadas», README por carpeta; **no** inventario, STRIDE, DREAD, diagramas ni escenario ficticio. |
| `02-prompt-contexto.md` | **2026-05-30 18:30 (UTC-3).** **Actualización de contexto:** escenario 11 (plataforma legal RAG); adaptar README y memory bank, cubrir entregables con esqueletos en `docs/`, referencias en `references/`, trazabilidad; **no** análisis completo, STRIDE, DREAD, mitigaciones, amenazas inventadas ni conclusiones. |
| `03-prompt-entender-proyecto.md` | **2026-06-14 19:30 (UTC-3).** Antes del análisis de riesgos: **entender el sistema** a partir del escenario — componentes, flujo de datos punta a punta, actores, sistemas internos/externos, posibles trust boundaries, información faltante; **sin** STRIDE ni DREAD; validar arquitectura conceptual. Entregable en ese momento: solo orientación (sin archivo en repo). |
| `04-prompt-inventario-activos.md` | **2026-06-14 20:30 (UTC-3).** Construir **inventario de activos** (nombre, tipo, descripción, relevancia, criticidad preliminar, impacto CIA) con arquitectura ya validada; diferenciar información vs tecnología; **sin** STRIDE, DREAD, mitigaciones ni amenazas inventadas. → `docs/01-inventario-activos.md`. |
| `05-prompt-trust-boundaries.md` | **2026-06-16 16:15 (UTC-3).** Completar **trust boundaries** en `docs/02-arquitectura-trust-boundaries.md` (nombre, componentes, descripción, motivo del cambio de confianza, activos que cruzan, justificación); **sin** STRIDE, amenazas, DREAD, mitigaciones ni evaluación de riesgos. |
| `06-prompt-matriz-stride.md` | **2026-06-17 10:30 (UTC-3).** Construir **catálogo inicial STRIDE** desde inventario y trust boundaries (componente, categoría, descripción, activos, justificación); analizar cada componente principal y cada TB; **sin** DREAD, mitigaciones ni priorización. → `docs/03-analisis-stride.md`. |
| `07-prompt-revision-stride.md` | **2026-06-17 15:00 (UTC-3).** **Revisión crítica** del análisis STRIDE: consolidar redundancias C↔TB, incorporar amenazas IA/RAG; priorizar calidad sobre cantidad. → `docs/03-analisis-stride.md` (v. revisada, 63 IDs). |
| `08-prompt-priorizacion-dread.md` | **2026-06-17 18:00 (UTC-3).** **Priorización DREAD** de 16 amenazas STR-* según `references/DREAD.md` y `references/STRIDE.md`; sin mitigaciones. → `docs/04-priorizacion-dread.md`. |
| `09-prompt-mapa-attack.md` | **2026-06-17 21:00 (UTC-3).** **Propuesta** de mapeo ATT&CK de amenazas DREAD hacia `docs/05-mapa-attack.md`; análisis previo sin redactar el entregable. |
| `10-prompt-plan-mitigacion.md` | **2026-06-17 20:00 (UTC-3).** **Propuesta** de plan de mitigación para `docs/06-plan-mitigacion.md`; análisis previo sin redactar el entregable. |
| `11-prompt-mapa-attack-redaccion.md` | **2026-06-17 22:30 (UTC-3).** Redacción de `docs/05-mapa-attack.md` (mapeo ATT&CK de 16 amenazas DREAD; verificación de técnicas en `MITRE_ATTCK` §3). |
| `12-prompt-plan-mitigacion-analisis-previo.md` | **2026-06-17 23:00 (UTC-3).** **Análisis previo** de `docs/06-plan-mitigacion.md` (16 amenazas, controles, estructura); sin redactar el entregable. |
| `13-prompt-plan-mitigacion-redaccion.md` | **2026-06-17 23:45 (UTC-3).** Redacción de `docs/06-plan-mitigacion.md` tras confirmación D1–D6. |
| `14-prompt-riesgos-residuales-analisis-previo.md` | **2026-06-18 09:00 (UTC-3).** **Análisis previo** de `docs/07-riesgos-residuales.md` (16 amenazas, residual cualitativo); sin redactar el entregable. |
| `15-prompt-riesgos-residuales-redaccion.md` | **2026-06-18 09:45 (UTC-3).** Redacción de `docs/07-riesgos-residuales.md` tras confirmación D1–D5. |
| `README.md` | Esta guía |
