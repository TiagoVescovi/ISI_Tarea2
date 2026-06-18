# Registro de prompt — matriz STRIDE

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 10:30 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/03-analisis-stride.md` (y actualización de `docs/02-arquitectura-trust-boundaries.md`) |

## Prompt exacto

```text
Tomando:

* Inventario de activos (01-inventario-activos.md)
* Arquitectura y trust boundaries (02-arquitectura-trust-boundaries.md)

quiero construir la matriz STRIDE.

Analizar cada componente principal y cada trust boundary.

Para cada amenaza indicar:

* Componente afectado.
* Categoría STRIDE.
* Descripción.
* Activos afectados.
* Justificación.

IMPORTANTE:

* No realizar todavía DREAD.
* No proponer mitigaciones.
* No priorizar riesgos.

Quiero obtener únicamente el catálogo inicial de amenazas STRIDE.
```

## Nota de trazabilidad

La salida se materializó en **`docs/03-analisis-stride.md`**: 52 amenazas por componente (Parte A) y 28 por trust boundary (Parte B), total 80 entradas con IDs `STR-C*` y `STR-TB*`.
