# Priorización DREAD

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`03-analisis-stride.md`](03-analisis-stride.md), [`../references/DREAD.md`](../references/DREAD.md).

DREAD prioriza las amenazas identificadas en STRIDE asignando un puntaje de 0 a 10 en cinco dimensiones: daño, reproducibilidad, explotabilidad, usuarios afectados y detectabilidad.

**Alcance:** 14 amenazas evaluadas del catálogo STRIDE. No incluye mitigaciones ni riesgo residual.

---

## Criterios DREAD

| Criterio | Descripción |
|----------|-------------|
| (D) Damage | Gravedad del impacto si el ataque tiene éxito |
| (R) Reproducibility | Facilidad para replicar el ataque |
| (E) Exploitability | Esfuerzo o habilidad requerida del atacante |
| (A) Affected Users | Cantidad de personas o sistemas afectados |
| (D) Discoverability | Facilidad para descubrir la vulnerabilidad |

```text
DREAD Score = (Damage + Reproducibility + Exploitability + Affected Users + Discoverability) / 5
```

| Rango | Severidad |
|-------|-----------|
| 9,0 – 10,0 | Crítico |
| 7,0 – 8,9 | Alto |
| 5,0 – 6,9 | Medio |
| 3,0 – 4,9 | Bajo |

---

## Matriz de riesgos

| Id | Componente | Amenaza | D | R | E | A | D | Score | Severidad |
|:--:|------------|---------|--:|--:|--:|--:|--:|------:|-----------|
| V1 | Usuario | Robo de credenciales / sesión | 8 | 8 | 7 | 5 | 6 | 6,8 | Medio |
| V2 | Usuario | Exposición por sesión abierta | 8 | 8 | 9 | 5 | 2 | 6,4 | Medio |
| V3 | Usuario | Manipulación de requests (IDOR) | 9 | 9 | 7 | 7 | 7 | 7,8 | Alto |
| V4 | Frontend | Over-fetching de datos legales | 8 | 8 | 7 | 8 | 8 | 7,8 | Alto |
| V5 | Frontend | Secuestro de sesión (XSS) | 9 | 7 | 6 | 6 | 6 | 6,8 | Medio |
| V6 | Frontend | Dependencias comprometidas | 8 | 6 | 7 | 9 | 5 | 7,0 | Alto |
| V7 | Backend | Prompt injection en consulta RAG | 9 | 9 | 8 | 8 | 8 | 8,4 | Alto |
| V8 | Backend | Exfiltración de contexto hacia LLM cloud | 9 | 8 | 7 | 9 | 6 | 7,8 | Alto |
| V9 | Backend | Data poisoning en ingesta | 8 | 7 | 7 | 8 | 5 | 7,0 | Alto |
| V10 | Backend | Retrieval leakage (multi-tenant) | 9 | 7 | 7 | 7 | 7 | 7,4 | Alto |
| V11 | Backend | Fallo de autorización multi-tenant | 9 | 8 | 7 | 8 | 7 | 7,8 | Alto |
| V12 | Backend | Exfiltración en logs del orquestador | 8 | 8 | 8 | 7 | 7 | 7,6 | Alto |
| V13 | Backend | Agotamiento de recursos | 7 | 9 | 9 | 8 | 8 | 8,2 | Alto |
| V14 | Backend | Inferencias sin trazabilidad | 6 | 8 | 8 | 6 | 4 | 6,4 | Medio |

---

## Priorización

Las amenazas con mayor puntaje combinan alto impacto sobre datos legales con facilidad de explotación en vectores propios de RAG:

1. **V7 — Prompt injection** (8,4)
2. **V13 — Agotamiento de recursos** (8,2)
3. **V3, V4, V8, V11 — IDOR, over-fetching, exfil LLM, multi-tenant** (7,4 – 7,8)
4. **V10 — Retrieval leakage** (7,4)

El resto queda en severidad **Media** y se planifica en sprints siguientes.

---

## Trazabilidad

| Documento | Relación |
|-----------|----------|
| [`06-plan-mitigacion.md`](06-plan-mitigacion.md) | Tratamiento de las 14 amenazas |
