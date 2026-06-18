# Registro de prompt — activos humanos en inventario

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-18 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/01-inventario-activos.md` |

## Contexto

Revisando la documentación del proyecto (inventario, arquitectura y actores en `02`), noté que el inventario cubría información, tecnología, identidad y procesos, pero **no** los activos humanos. En un escenario legal con RAG las personas son centrales: abogados que confían en salidas de IA, administradores con privilegios elevados, clientes que cargan contratos confidenciales y proveedores cloud que procesan contexto. Quiero completar el inventario antes de seguir con el modelado de amenazas.

## Prompt exacto

```text
Revisé todo el inventario y la arquitectura del Escenario 11. Falta una sección de activos humanos.

Actualizá `docs/01-inventario-activos.md` agregando **Activos humanos**, sin reescribir el resto salvo ajustes mínimos de coherencia.

**Escenario:** 11 — Plataforma legal con IA generativa (RAG). Firma de servicios profesionales; clientes externos e internos; OAuth2; LLM comercial y local; Chroma o Pinecone.

**Objetivo:** documentar personas y organizaciones relevantes para threat modeling, con la misma tabla que el resto del inventario: Activo | Tipo | Descripción | Criticidad | Justificación.

**Incluir como mínimo:**

1. **Abogados y personal legal** — Consultas RAG, revisión de borradores, auditoría, carga de documentación de casos.

2. **Personal de TI / administradores** — Infraestructura, API, orquestador, credenciales, configuración de LLM y vector DB.

3. **Clientes externos** — Usuarios de empresas cliente que cargan contratos y consultan en su tenant.

4. **Proveedor cloud / LLM (cadena de suministro)** — Organización detrás de OpenAI, Anthropic, Pinecone u otros SaaS que procesan o almacenan datos según contrato.

5. **Personal administrativo de la firma** (si aplica) — Soporte u onboarding, con criticidad acorde al acceso real.

**Reglas:**

- Tipo: **Personas**.
- Criticidad Alta / Media / Baja, coherente con el escenario (no todo en Alta).
- Basarse en `references/escenario_11.md` y `02-arquitectura-trust-boundaries.md`.
- No agregar STRIDE, DREAD, amenazas ni mitigaciones.
- No duplicar filas de identidad o servicios tecnológicos; acá van actores humanos u organizacionales.
- Mantener el formato del inventario actual.

**Salida:** sección nueva con 4–5 filas, lista para el análisis STRIDE.
```

## Nota de trazabilidad

La salida se materializó en **`docs/01-inventario-activos.md`** (sección «Activos humanos»), detectada como omisión al revisar la documentación completa del Escenario 11.
