# Registro de prompt — mapa ATT&CK

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 21:00 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/05-mapa-attack.md` (análisis y propuesta previa; documento aún no redactado) |

## Prompt exacto

```text
Recordá registar los prompts, necesito que trabajes sobre `docs/05-mapa-attack.md`.

IMPORTANTE:

* No modifiques ningún archivo todavía.
* Primero revisá toda la documentación existente y explicame exactamente cómo pensás construir el documento antes de hacer cambios.
* No inventes amenazas, activos, técnicas, tácticas ni controles.
* Utilizá únicamente información ya documentada en el proyecto y en los materiales de referencia.
* Si encontrás inconsistencias, amenazas sin trazabilidad o información faltante, detenete y avisame antes de continuar.
* Todo lo que agregues debe estar vinculado a documentación existente.

Objetivo:

Completar `docs/05-mapa-attack.md` realizando el mapeo de las amenazas priorizadas en DREAD hacia tácticas y técnicas MITRE ATT&CK.

Fuentes obligatorias:

* `docs/01-inventario-activos.md`
* `docs/02-arquitectura-trust-boundaries.md`
* `docs/03-analisis-stride.md`
* `docs/04-priorizacion-dread.md`
* `prompts/00-memory-bank.md`
* `references/MITRE_ATTCK`
* `references/analisis_amenazas.md`

Reglas:

* Utilizar únicamente las amenazas ya seleccionadas y evaluadas en `04-priorizacion-dread.md`.
* No incorporar amenazas nuevas.
* No utilizar MITRE ATLAS.
* No utilizar OWASP LLM.
* No utilizar frameworks externos que no estén en `references/`.
* El mapeo debe basarse exclusivamente en MITRE ATT&CK y en el material visto en clase.

Quiero que primero identifiques:

1. Qué amenazas fueron priorizadas en DREAD.
2. Qué activos y trust boundaries están involucrados.
3. Qué tácticas ATT&CK aplican a cada amenaza.
4. Qué técnicas ATT&CK corresponden según la descripción de cada amenaza.
5. Si existe alguna amenaza que no pueda mapearse razonablemente a ATT&CK.

Luego proponeme la estructura completa de `docs/05-mapa-attack.md` antes de escribirlo.

La idea es que el documento mantenga trazabilidad clara:

Amenaza STRIDE → Score DREAD → Activo afectado → Trust Boundary → Táctica ATT&CK → Técnica ATT&CK → Justificación

Además quiero que me indiques:

* Cuántas amenazas van a quedar mapeadas.
* Qué tácticas ATT&CK aparecen con mayor frecuencia.
* Si existen amenazas que comparten la misma técnica.
* Si encontrás inconsistencias entre STRIDE, DREAD y el mapeo ATT&CK.

No escribas todavía el documento final. Primero mostrame el análisis y el plan detallado de cómo lo vas a construir.
```

## Resultado de la asistencia (en ese intercambio)

Análisis y propuesta de mapeo ATT&CK para las **16** amenazas de `04-priorizacion-dread.md` (**15** con técnica primaria, **STR-C12-R** sin equivalente ofensivo directo), estructura del documento y detección de inconsistencias. **Sin** modificación de archivos en el repositorio en esa sesión.
