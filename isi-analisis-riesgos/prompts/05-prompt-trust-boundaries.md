# Registro de prompt — trust boundaries (arquitectura)

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-16 16:15 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/02-arquitectura-trust-boundaries.md` (y actualización de `docs/00-indice-documentos.md`, `docs/01-inventario-activos.md` en trazabilidad) |

## Prompt exacto

```text
Tomando la arquitectura validada del escenario 11 y el inventario de activos ya definido:

quiero completar la sección de Trust Boundaries del documento `02-arquitectura-trust-boundaries.md`.

Para cada trust boundary indicar:

* Nombre.
* Componentes involucrados.
* Descripción.
* Motivo por el cual existe un cambio de confianza.
* Activos que atraviesan la frontera.
* Justificación.

IMPORTANTE:

* No realizar STRIDE.
* No identificar amenazas.
* No realizar DREAD.
* No proponer mitigaciones.
* No evaluar riesgos.

El objetivo es únicamente documentar las fronteras de confianza que servirán posteriormente para el modelado de amenazas.
```

## Nota de trazabilidad

La salida se materializó en **`docs/02-arquitectura-trust-boundaries.md`** (TB-01 a TB-10). Mensaje posterior del usuario: solicitud de registrar este prompt en `prompts/` para cumplir la política de uso de IA de la materia.
