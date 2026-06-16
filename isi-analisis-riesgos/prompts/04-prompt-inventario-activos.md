# Registro de prompt — inventario de activos

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-16 14:45 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/01-inventario-activos.md` |

## Prompt exacto

```text
Perfecto.

Tomando la arquitectura validada, quiero construir el inventario de activos del sistema.

Para cada activo:

* Nombre del activo.
* Tipo (información, software, servicio, infraestructura, identidad, proceso).
* Descripción.
* Justificación de por qué es un activo relevante.
* Clasificación preliminar de criticidad.
* Impacto potencial sobre confidencialidad, integridad y disponibilidad.

IMPORTANTE:

* Basarse únicamente en el escenario y la arquitectura validada.
* Diferenciar activos de información y activos tecnológicos.
* No realizar todavía STRIDE.
* No realizar todavía DREAD.
* No proponer mitigaciones.
* No inventar amenazas.

Quiero obtener primero una lista sólida de activos para utilizar posteriormente en el modelado de amenazas.
```

## Nota de trazabilidad

La salida de la asistencia de IA se materializó en el repositorio en **`docs/01-inventario-activos.md`** (y actualización del estado de E01 en **`docs/00-indice-documentos.md`**). Este archivo cumple el requisito de registro en `prompts/` para ese intercambio.
