# Registro de prompt — redacción riesgos residuales

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-18 09:45 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/07-riesgos-residuales.md` |

## Prompt exacto

```text
Usá las opciones recomendadas:

D1: A — Documentar ambas ramas de TB-10 (Chroma on-prem y Pinecone/SaaS).
D2: A — Mantener escenario mixto TB-07 + TB-08.
D3: A — Clasificación residual solo en Bajo / Medio / Alto, sin puntajes DREAD residuales.
D4: No inventar responsables. Si no están definidos en el escenario, dejarlo como pendiente de definición por la organización.
D5: A — Incluir sección transversal dedicada (LLM comercial, Pinecone, IdP, alucinaciones y factor humano) y referenciarla desde cada amenaza.

Adicionalmente, no presentar la reducción del riesgo como un recálculo matemático de DREAD, sino como una evaluación cualitativa posterior a la implementación de controles. También dejar explícito que transferir riesgo a terceros no implica eliminarlo.

Con estas decisiones podés redactar docs/07-riesgos-residuales.md y registrar el prompt correspondiente. También no olvides tomar de referencia los documentos ya creados y los documentos que hay en /references
```

## Resultado de la asistencia

Redacción completa de **`docs/07-riesgos-residuales.md`**: 16 amenazas con trazabilidad 03→06, evaluación cualitativa residual (Bajo/Medio/Alto/Evitado), sección transversal §3, riesgos aceptados, matrices, monitoreo y límites de `references/`. Actualización de README, memory bank y registros de prompts.
