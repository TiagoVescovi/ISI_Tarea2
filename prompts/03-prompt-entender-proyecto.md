# Registro de prompt — comprensión del sistema (arquitectura conceptual)

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-16 09:30 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | Sin archivo en repo en esa interacción (respuesta orientativa para validar arquitectura; base para `docs/02-arquitectura-trust-boundaries.md` y DFD) |

## Prompt exacto

```text
Antes de comenzar el análisis de riesgos, quiero entender completamente el sistema.

A partir del escenario, identificá:

Componentes principales.
Flujo de datos de punta a punta.
Actores involucrados.
Sistemas internos.
Sistemas externos.
Posibles trust boundaries.
Información que falta definir.

No completes todavía STRIDE ni DREAD.

Quiero primero validar la arquitectura conceptual para asegurarme de que entendemos el escenario correctamente.
```

## Nota de trazabilidad

En el borrador original del archivo aparecía duplicada la línea «Flujo de datos de punta a punta.»; en el **prompt exacto** de este registro se dejó una sola vez, alineada con la intención del pedido.
