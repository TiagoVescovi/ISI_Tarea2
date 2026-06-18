# Registro de prompt — priorización DREAD

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 18:00 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/04-priorizacion-dread.md` |

## Prompt exacto

```text
Continuar con el siguiente entregable del proyecto:

**04-priorizacion-dread.md**

### Referencias obligatorias

Antes de comenzar, leer y utilizar como fuentes principales:

* `/references/STRIDE.md`
* `/references/DREAD.md`

Las definiciones, escalas, criterios de puntuación y metodología utilizadas deben respetar estrictamente el contenido de esos documentos.

No utilizar interpretaciones alternativas de STRIDE o DREAD salvo que estén explícitamente justificadas por las referencias proporcionadas.

### Contexto

Ya existe una matriz STRIDE completa para el escenario:

**Plataforma de Asistencia Legal basada en IA Generativa (RAG)**

La matriz contiene amenazas identificadas mediante IDs `STR-*`.

El documento actual debe tomar dichas amenazas como entrada y realizar su priorización mediante DREAD.

### Objetivo

Generar el documento:

`04-priorizacion-dread.md`

aplicando la metodología DREAD explicada en `/references/DREAD.md`.

### Metodología

Para cada amenaza seleccionada evaluar:

* Damage
* Reproducibility
* Exploitability
* Affected Users
* Discoverability

Calcular:

DREAD Score =
(Damage + Reproducibility + Exploitability +
Affected Users + Discoverability) / 5

Clasificar utilizando los rangos definidos en `/references/DREAD.md`.

### Selección de amenazas

NO evaluar las 80 amenazas.

Seleccionar entre 12 y 20 amenazas que representen:

* Los riesgos más relevantes del sistema.
* Diferentes categorías STRIDE.
* Diferentes componentes.
* Diferentes trust boundaries.

La selección debe estar justificada brevemente.

### Restricciones

* Utilizar únicamente amenazas existentes de la matriz STRIDE.
* No crear amenazas nuevas.
* No proponer mitigaciones.
* No realizar plan de tratamiento.
* No realizar análisis de riesgo residual.
* Mantener redacción académica y formal.
* Justificar todas las puntuaciones asignadas.
* Toda valoración debe ser coherente con las escalas definidas en `/references/DREAD.md`.
```

## Nota de trazabilidad

Evaluación DREAD de **16** amenazas `STR-*` seleccionadas del catálogo de 63 en `03-analisis-stride.md`. Escalas y rangos de severidad según `references/DREAD.md` §2 y §3.2.
