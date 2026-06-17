# Registro de prompt — redacción plan de mitigación

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 23:45 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/06-plan-mitigacion.md` |

## Prompt exacto

```text
D1 — TB-10:
Documentar ambas ramas. Si se utiliza Chroma on-prem, tratar STR-TB10-I principalmente mediante evitación del riesgo. Si se utiliza Pinecone/SaaS, aplicar mitigación y transferencia. Mantener explícita la condición arquitectónica ya documentada en 02-arquitectura-trust-boundaries.md.

D2 — Transferir:
Sí. Incluir una subsección breve para riesgos asociados a terceros (LLM cloud, Pinecone y proveedor IdP), sin inventar cláusulas contractuales ni controles no presentes en references/. Limitarse a describir la estrategia de transferencia como tratamiento complementario.

D3 — Revisión humana legal:
Confirmado. Utilizar segregación de funciones, múltiples aprobaciones y revisión humana obligatoria como controles de proceso para STR-C12-T3, STR-C13-T y STR-C14-T, apoyándose en STRIDE categoría R y en los controles documentados en references/.

D4 — Aceptar:
Los riesgos aceptados deben quedar identificados en 06, pero desarrollarse en detalle en 07-riesgos-residuales.md. En 06 solamente indicar que existe riesgo residual y referenciar el entregable correspondiente.

D5 — WAF:
Incluirlo únicamente cuando se encuentre respaldado por references/analisis_amenazas.md y asociado a protección de APIs o carga documental. No utilizarlo como control principal; priorizar validación de entrada, hardening y controles STRIDE T.

D6 — Cobertura:
Documentar las 16 amenazas. Mantener el mismo nivel de detalle para todas para conservar la trazabilidad completa entre STRIDE, DREAD, ATT&CK y el plan de mitigación.

Instrucciones para la redacción:

Redactar directamente docs/06-plan-mitigacion.md.
Mantener coherencia estricta con 01, 02, 03, 04 y 05.
No inventar amenazas, activos, trust boundaries, técnicas ATT&CK ni controles fuera de references/.
Incluir todas las tablas propuestas en el análisis previo.
Mantener referencias cruzadas hacia los demás entregables.
Incluir la trazabilidad completa amenaza → DREAD → ATT&CK → controles → tratamiento.
Registrar el prompt utilizado según la estructura del proyecto.
Actualizar únicamente los archivos de seguimiento que correspondan al nuevo entregable.
```

## Resultado de la asistencia

Redacción completa de **`docs/06-plan-mitigacion.md`**: 16 amenazas con plan de mitigación, controles desde `references/`, transferencia ante terceros, ramas TB-10, tablas resumen, priorización, dependencias y agrupaciones.
