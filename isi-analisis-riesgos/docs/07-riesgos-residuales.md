# Riesgos residuales

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`01-inventario-activos.md`](01-inventario-activos.md), [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md), [`03-analisis-stride.md`](03-analisis-stride.md), [`04-priorizacion-dread.md`](04-priorizacion-dread.md), [`05-mapa-attack.md`](05-mapa-attack.md), [`06-plan-mitigacion.md`](06-plan-mitigacion.md), [`../references/STRIDE.md`](../references/STRIDE.md), [`../references/DREAD.md`](../references/DREAD.md), [`../references/analisis_amenazas.md`](../references/analisis_amenazas.md), [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK).

**Alcance:** evaluación cualitativa del riesgo que **permanece** tras aplicar el plan de mitigación (`06`) a las **16** amenazas priorizadas en DREAD. **No** incluye nuevas mitigaciones ni re-puntuación numérica DREAD.

---

## 1. Introducción

### 1.1 Propósito

Documentar los **riesgos residuales** — escenarios en los que el daño puede materializarse pese a los controles planificados — con trazabilidad desde STRIDE y DREAD hasta los controles y tratamientos de `06`, justificación de por qué el riesgo no puede eliminarse por completo con los materiales disponibles en `references/`, y clasificación **Bajo / Medio / Alto** coherente con las bandas de severidad de DREAD.

### 1.2 Relación con el plan de mitigación

El plan [`06-plan-mitigacion.md`](06-plan-mitigacion.md) define **mitigar**, **evitar**, **transferir** y **aceptar**. Este documento desarrolla lo que queda después de esas decisiones:

| Tratamiento en `06` | Efecto sobre el residual |
|---------------------|--------------------------|
| **Mitigar** | Reduce probabilidad o impacto; **no garantiza** eliminación total. |
| **Evitar** | Elimina el cruce de frontera para la amenaza concreta (p. ej. Chroma on-prem sin TB-10). |
| **Transferir** | Reasigna parte de la responsabilidad operativa al tercero; **no elimina** el riesgo para la firma ni para los datos afectados. |
| **Aceptar** | La organización asume conscientemente el residual documentado aquí, sujeto a monitoreo. |

> **Transferencia y riesgo residual:** según el propio plan (`06` §1, §5), transferir riesgo ante LLM comercial, Pinecone o IdP es **complementario**. La firma conserva exposición a confidencialidad, integridad y disponibilidad del servicio; la transferencia **no implica** que el riesgo desaparezca.

### 1.3 Cadena de trazabilidad

```text
03 STRIDE (amenaza) → 04 DREAD (urgencia inicial) → 05 ATT&CK (TTP) → 06 (controles + tratamiento) → 07 (riesgo residual cualitativo)
```

El score DREAD de `04` califica la amenaza **antes** de mitigación. En `07` la reducción se expresa como **evaluación cualitativa posterior** a la implementación prevista de controles; **no** como recálculo matemático de DREAD.

### 1.4 Criterios de clasificación del riesgo residual

Se reutilizan las **bandas de severidad** de [`../references/DREAD.md`](../references/DREAD.md) §3.2, aplicadas de forma **cualitativa** al escenario post-controles:

| Nivel residual | Criterio (después de `06`) |
|----------------|----------------------------|
| **Alto** | Persiste exposición relevante con daño potencial muy grave; controles atenúan vectores pero no el impacto estructural (p. ej. datos en cloud con contexto recuperado). |
| **Medio** | Controles reducen explotabilidad o frecuencia; el escenario residual sigue siendo plausible con impacto significativo. |
| **Bajo** | Controles cubren el vector principal; residual limitado a fallos marginales u operativos. |
| **Evitado** | La amenaza deja de aplicar por decisión arquitectónica documentada (rama Chroma on-prem para STR-TB10-I). |

**Coherencia con DREAD inicial:** si el **Damage** original en `04` es muy alto (≥ 9), el residual **no** se clasifica como **Bajo** salvo que la amenaza quede **evitada**.

### 1.5 Supuestos

| ID | Supuesto |
|----|----------|
| **S1** | Los controles de `06` se implementan según lo documentado. |
| **S2** | Escenario **mixto** TB-07 + TB-08: parte de la carga usa LLM comercial y parte LLM local para datos altamente sensibles (`02` TB-07, TB-08). |
| **S3** | TB-10 es **condicional**: se documentan **ambas** ramas — Chroma on-prem y Pinecone/SaaS (`02` TB-10). |
| **S4** | Revisión humana legal obligatoria para salidas de impacto legal (`06` §6.3, §6.14, §6.16; STRIDE §3.3 segregación de funciones / múltiples aprobaciones). |

### 1.6 Responsable de la aceptación del riesgo

El escenario y los entregables previos **no definen** el rol o comité que formaliza la aceptación. Queda **pendiente de definición por la organización** (socio responsable, CISO, comité de riesgos u otro mecanismo interno).

### 1.7 Limitaciones metodológicas

| Limitación | Descripción |
|------------|-------------|
| **L1** | `references/` no define fórmula de riesgo residual; la clasificación es cualitativa. |
| **L2** | ATT&CK no modela fallos inherentes del LLM; alucinación se aproximó a T1565 en `05`. |
| **L3** | STR-C12-R no tiene técnica ATT&CK ofensiva (`05` §5). |
| **L4** | T1078 ausente en `MITRE_ATTCK` §3 → T1068 para STR-C02-E y STR-TB09-E. |
| **L5** | Controles **detectivos** (SIEM, DLP) mejoran detección, no eliminan la ocurrencia. |
| **L6** | `references/` no incluye guías LLM/RAG específicas (grounding, aislamiento semántico vectorial). |

---

## 2. Resumen ejecutivo

### 2.1 Tabla maestra — 16 amenazas y riesgo residual

| # | ID STRIDE | DREAD ini. | Nivel ini. | ATT&CK (prim.) | Tratamiento `06` | Residual principal | Nivel residual | Aceptado |
|---|-----------|------------|------------|----------------|------------------|-------------------|----------------|----------|
| 1 | STR-C12-T | 8,0 | Alto | T1059.006 | Mitigar | Bypass de validación / inyección avanzada | **Medio** | No |
| 2 | STR-TB07-I | 7,8 | Alto | T1567.002 | Evitar+Mitigar+Transferir | Exposición residual en cloud; visibilidad del proveedor | **Medio** * | Parcial |
| 3 | STR-C12-T3 | 7,6 | Alto | T1565 | Mitigar+Aceptar | Alucinación; error en revisión legal | **Medio** | **Sí** |
| 4 | STR-TB09-E | 7,6 | Alto | T1068 | Mitigar | Fallos lógicos de autorización TB-09 | **Medio** | No |
| 5 | STR-C07-I | 7,4 | Alto | T1005 | Mitigar | Fuga por similitud semántica en retrieval | **Medio** | No |
| 6 | STR-C02-E | 7,4 | Alto | T1068 | Mitigar | Bugs de autorización API cross-tenant | **Medio** | No |
| 7 | STR-C05-T2 | 7,2 | Alto | T1080 | Mitigar | Data poisoning sutil en ingesta | **Medio** | No |
| 8 | STR-TB10-I | 7,2 | Alto | T1530 | Evitar **o** Mit+Trans+Aceptar | A: **Evitado** / B: subprocesador SaaS | **Evitado** / **Medio** | B: **Sí** |
| 9 | STR-TB01-S | 7,0 | Alto | T1566.002 | Mitigar | Phishing exitoso pese a controles | **Medio** | No |
| 10 | STR-C12-T2 | 6,8 | Medio | T1080 | Mitigar | Inyección indirecta vía corpus | **Medio** | No |
| 11 | STR-C12-R | 6,6 | Medio | — | Mitigar | Gaps de logging o correlación | **Bajo** | No |
| 12 | STR-TB03-S | 6,6 | Medio | T1556 | Mitigar+Transferir | Dependencia o compromiso del IdP | **Medio** | Parcial |
| 13 | STR-TB09-I | 6,4 | Medio | T1005 | Mitigar | Mezcla de datos en salida generada | **Medio** | No |
| 14 | STR-C13-T | 6,4 | Medio | T1565 | Mitigar+Aceptar | Borrador erróneo; fallo de revisión | **Medio** | **Sí** |
| 15 | STR-C10-T | 6,2 | Medio | T1556 | Mitigar | Claims o tokens incorrectos | **Bajo** | No |
| 16 | STR-C14-T | 6,2 | Medio | T1565 | Mitigar+Aceptar | Dictamen erróneo; fallo de revisión experta | **Medio** | **Sí** |

\* En escenario mixto TB-07/TB-08 con enrutamiento activo hacia LLM local para datos altamente sensibles; ver §3.1.

### 2.2 Distribución

| Nivel residual | Cantidad | Observación |
|----------------|----------|-------------|
| **Medio** | 13 | Patrón dominante tras controles de `06`. |
| **Bajo** | 2 | STR-C12-R, STR-C10-T. |
| **Evitado** | 1 (rama A) | STR-TB10-I con Chroma on-prem. |
| **Alto** | 0 | Ninguna amenaza queda en Alto residual con política mixta y controles planificados; STR-TB07-I podría acercarse a Alto si predominara el uso cloud con contexto recuperado (S2). |

### 2.3 Riesgos formalmente aceptados

Cuatro amenazas con tratamiento **Aceptar** en `06`: STR-C12-T3, STR-C13-T, STR-C14-T y STR-TB10-I (solo **rama B** Pinecone/SaaS). Detalle en §5.

---

## 3. Riesgos residuales transversales

Esta sección agrupa factores que atraviesan múltiples amenazas. Cada ficha de §4 referencia la sección aplicable.

### 3.1 LLM comercial (TB-07) — escenario mixto TB-07 + TB-08

| Campo | Contenido |
|-------|-----------|
| **Contexto** | El escenario contempla LLM comercial (OpenAI/Anthropic) y LLM local para datos altamente sensibles (`02` TB-07, TB-08). |
| **Amenazas vinculadas** | STR-TB07-I (primaria); STR-C12-T, C12-T2, C12-T3, C13-T, C14-T cuando el proceso usa API comercial. |
| **Controles en `06`** | Evitar vía TB-08; TLS; minimización de datos; DLP; segmentación M0930; transferencia al proveedor (`06` §5). |
| **Residual** | Consultas y contexto no clasificados como “altamente sensibles” siguen cruzando TB-07; el proveedor puede procesar prompts con fragmentos recuperados; dependencia operativa del tercero. |
| **Por qué persiste** | Minimización y DLP reducen volumen pero no eliminan el cruce si el RAG operativo usa cloud; **transferir no elimina** la exposición de confidencialidad. |
| **Nivel agregado** | **Medio** con política mixta activa; **Alto** si la mayoría del tráfico RAG usara cloud con contexto recuperado. |
| **Límite en `references/`** | Sin matriz de clasificación de sensibilidad ni controles DLP específicos RAG. |

### 3.2 Base vectorial SaaS — Pinecone (TB-10, rama B)

| Campo | Contenido |
|-------|-----------|
| **Condición** | Aplica solo si el despliegue usa Pinecone u otro SaaS (`02` TB-10). |
| **Amenaza** | STR-TB10-I. |
| **Controles en `06`** | TLS; cifrado en reposo donde aplique; minimización de metadatos; transferencia al proveedor SaaS. |
| **Residual** | Acceso del proveedor a embeddings y metadatos; incidente en infraestructura ajena; dependencia de disponibilidad. |
| **Por qué persiste** | Los embeddings derivan de documentos confidenciales; minimización no elimina el índice; transferencia **no suprime** el riesgo para la firma. |
| **Nivel** | **Medio** — aceptado explícitamente en `06` §6.8 rama B. |
| **Rama A (Chroma on-prem)** | STR-TB10-I **evitada** — sin residual asociado a TB-10. |
| **Límite en `references/`** | Sin cláusulas contractuales ni certificaciones del proveedor. |

### 3.3 IdP externo federado (TB-03)

| Campo | Contenido |
|-------|-----------|
| **Amenazas** | STR-TB03-S, STR-C10-T; dependencia indirecta en STR-C02-E, STR-TB09-E (cadena identidad → RBAC). |
| **Controles en `06`** | MFA; hardening OAuth; tokens con expiración; logging; M1021; transferencia de autenticación primaria al IdP. |
| **Residual** | Compromiso o indisponibilidad del IdP; mala configuración de federación; phishing OAuth; claims incorrectos propagados. |
| **Por qué persiste** | La autenticación primaria reside fuera del perímetro (`02` TB-03); transferir **no elimina** la dependencia ni el impacto en acceso a la plataforma. |
| **Nivel agregado** | **Medio**. |
| **Límite en `references/`** | Sin hardening OAuth detallado ni SLA del IdP. |

### 3.4 Alucinaciones e integridad generativa del LLM

| Campo | Contenido |
|-------|-----------|
| **Amenazas** | STR-C12-T3 (núcleo); impacto en STR-C13-T, STR-C14-T; agravante en STR-C12-T2. |
| **Controles en `06`** | Revisión humana obligatoria; segregación de funciones; logging de fragmentos; validación de coherencia (STRIDE T integridad — genérico). |
| **Residual** | Respuestas o borradores con contenido jurídico incorrecto, inventado o desalineado del contexto recuperado; comportamiento **inherente** a LLMs según `04` (STR-C12-T3). |
| **Por qué persiste** | `references/` no documenta controles de grounding verificable, citación forzada ni métricas de fidelidad RAG; ATT&CK modela Impact (T1565) pero no el fallo estocástico del modelo. |
| **Nivel agregado** | **Medio** (Damage inicial muy alto permanece como impacto potencial). |

### 3.5 Factor humano en revisión legal

| Campo | Contenido |
|-------|-----------|
| **Amenazas** | STR-C12-T3, STR-C13-T, STR-C14-T. |
| **Controles en `06`** | Segregación de funciones; múltiples aprobaciones (STRIDE §3.3); revisión humana obligatoria antes de adoptar respuesta, enviar borrador o dictamen. |
| **Residual** | Omisión de verificación frente al documento fuente; fatiga; plazos que debilitan la segregación; aprobación formal sin lectura exhaustiva. |
| **Por qué persiste** | STRIDE §3.3 mitiga repudio y refuerza proceso, **no** garantiza corrección jurídica ni elimina error humano. |
| **Nivel agregado** | **Medio**. |
| **Límite en `references/`** | Sin KPIs de calidad de revisión ni estándar de doble firma para todos los casos. |

---

## 4. Detalle por amenaza

Para cada amenaza: trazabilidad, controles aplicados, escenario residual, justificación, nivel, temas transversales y aceptación.

---

### 4.1 STR-C12-T — Prompt injection directa (DREAD inicial 8,0 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | A.12 — **T** — instrucciones maliciosas en consulta RAG (`03`) |
| **04** | 8,0 — Alto |
| **05** | T1059.006 (prim.); T1565 (sec.) |
| **06 — Controles** | Validación de entrada; M1038; hardening Python; logging; SIEM/EDR |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Técnicas de inyección que evaden validación sintáctica; encadenamiento con manipulación de salida o exfiltración de contexto |
| **Por qué no se elimina** | `references/` aporta validación de entrada genérica (STRIDE T/E) y whitelisting M1038; **no** hay contramedidas específicas anti–prompt injection LLM |
| **Nivel residual** | **Medio** |
| **Transversal** | — |
| **Aceptado** | No |

---

### 4.2 STR-TB07-I — Exposición hacia LLM comercial (DREAD inicial 7,8 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | TB-07 — **I** |
| **04** | 7,8 — Alto (Damage 9) |
| **05** | T1567.002 (prim.); T1005 (sec.) |
| **06 — Controles** | Evitar (LLM local TB-08); TLS; minimización; DLP; M0930; transferencia proveedor |
| **06 — Tratamiento** | Evitar + Mitigar + Transferir |
| **Escenario residual** | Tráfico residual hacia cloud; visibilidad del proveedor sobre prompts/contexto; incidente o uso indebido en infraestructura del tercero |
| **Por qué no se elimina** | Escenario mixto mantiene TB-07 activo para parte de la carga; transferencia **no elimina** confidencialidad (`06` §5) |
| **Nivel residual** | **Medio** (política mixta S2); podría elevarse a **Alto** si predominara cloud con contexto recuperado |
| **Transversal** | [§3.1](#31-llm-comercial-tb-07--escenario-mixto-tb-07--tb-08) |
| **Aceptado** | Parcial — residual de subprocesamiento asumido en la relación con el proveedor |

---

### 4.3 STR-C12-T3 — Alucinación con impacto legal (DREAD inicial 7,6 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | A.12 — **T** |
| **04** | 7,6 — Alto (Damage 9) |
| **05** | T1565 |
| **06 — Controles** | Segregación de funciones; revisión humana obligatoria; logging fragmentos |
| **06 — Tratamiento** | Mitigar + **Aceptar** |
| **Escenario residual** | Respuesta presentada como fundamentada con error jurídico; revisión humana insuficiente o omitida bajo presión operativa |
| **Por qué no se elimina** | Comportamiento inherente a LLMs (`04`); controles de proceso no garantizan corrección (`references/` sin grounding verificable) |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.4](#34-alucinaciones-e-integridad-generativa-del-llm), [§3.5](#35-factor-humano-en-revisión-legal) |
| **Aceptado** | **Sí** — pendiente formalización por la organización (§1.6) |

---

### 4.4 STR-TB09-E — Cliente accede a corpus o funciones ajenas (DREAD inicial 7,6 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | TB-09 — **E** |
| **04** | 7,6 — Alto |
| **05** | T1068 * |
| **06 — Controles** | RBAC; M1026; validación permisos; MFA en privilegios |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Fallos lógicos de autorización (IDOR, mapeo incorrecto de roles) no detectados en pruebas |
| **Por qué no se elimina** | Controles preventivos STRIDE E no cubren toda la complejidad multi-tenant + TB-09 |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.3](#33-idp-externo-federado-tb-03) (cadena identidad) |
| **Aceptado** | No |

---

### 4.5 STR-C07-I — Retrieval leakage (DREAD inicial 7,4 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | A.7 — **I** |
| **04** | 7,4 — Alto |
| **05** | T1005 |
| **06 — Controles** | RBAC/filtros tenant; segmentación lógica namespaces; logging |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Recuperación de fragmentos de otro tenant/asunto por proximidad semántica en el espacio vectorial |
| **Por qué no se elimina** | RBAC y segmentación (M0930) no abordan aislamiento semántico en embeddings |
| **Nivel residual** | **Medio** |
| **Transversal** | — |
| **Aceptado** | No |

---

### 4.6 STR-C02-E — Fallo de autorización en API (DREAD inicial 7,4 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | A.2 — **E** |
| **04** | 7,4 — Alto |
| **05** | T1068 * |
| **06 — Controles** | Authz en servidor; RBAC; M1026; rate limiting |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Bugs en endpoints; validación incompleta de ownership de recursos |
| **Por qué no se elimina** | Misma familia que STR-TB09-E; rate limiting (STRIDE D) no corrige autorización rota |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.3](#33-idp-externo-federado-tb-03) |
| **Aceptado** | No |

---

### 4.7 STR-C05-T2 — Data poisoning en ingesta (DREAD inicial 7,2 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | A.5 — **T** |
| **04** | 7,2 — Alto |
| **05** | T1080; T1565 (sec.) |
| **06 — Controles** | Validación entrada; WAF‡; integridad hash; auditoría ETL |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Documento malicioso que supera validación superficial y envenena el índice de forma sutil |
| **Por qué no se elimina** | T1080 modela contenido compartido envenenado; sin análisis semántico de ingesta en `references/` |
| **Nivel residual** | **Medio** |
| **Transversal** | — |
| **Aceptado** | No |

‡ WAF complementario según `analisis_amenazas.md` Weaponize.

---

### 4.8 STR-TB10-I — Exposición embeddings en SaaS vectorial (DREAD inicial 7,2 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | TB-10 — **I** (condicional) |
| **04** | 7,2 — Alto |
| **05** | T1530 |
| **06 — Tratamiento** | Depende del despliegue (`02` TB-10) |

**Rama A — Chroma on-prem (TB-10 no aplica):**

| Campo | Contenido |
|-------|-----------|
| **Controles** | Evitar — índice bajo control de la firma |
| **Residual** | **Evitado** — sin cruce hacia subprocesador vectorial |
| **Nivel residual** | **Evitado** |
| **Aceptado** | No aplica |

**Rama B — Pinecone / SaaS vectorial:**

| Campo | Contenido |
|-------|-----------|
| **Controles** | TLS; cifrado reposo; minimización metadatos; transferencia SaaS |
| **Residual** | Exposición de embeddings/metadatos al proveedor; incidente en infraestructura ajena |
| **Por qué no se elimina** | Subprocesador fuera del perímetro; transferencia **no elimina** el riesgo |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.2](#32-base-vectorial-saas--pinecone-tb-10-rama-b) |
| **Aceptado** | **Sí** (rama B) — pendiente formalización por la organización (§1.6) |

---

### 4.9 STR-TB01-S — Phishing / interfaz falsa (DREAD inicial 7,0 — Alto)

| Campo | Contenido |
|-------|-----------|
| **03** | TB-01 — **S** |
| **04** | 7,0 — Alto |
| **05** | T1566.002; T1036 (sec.) |
| **06 — Controles** | M1049; M1021; M1017; MFA; TLS |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Usuario autentica en sitio falso; spearphishing dirigido al personal legal |
| **Por qué no se elimina** | Vector humano; capacitación M1017 reduce pero no elimina (`analisis_amenazas.md` §5.3) |
| **Nivel residual** | **Medio** |
| **Transversal** | — |
| **Aceptado** | No |

---

### 4.10 STR-C12-T2 — Indirect prompt injection (DREAD inicial 6,8 — Medio)

| Campo | Contenido |
|-------|-----------|
| **03** | A.12 — **T** |
| **04** | 6,8 — Medio |
| **05** | T1080; T1059.006 (sec.) |
| **06 — Controles** | Controles ingesta (C05-T2); M1038; validación consulta; logging |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Instrucciones ocultas en PDF/Word recuperadas en contexto y ejecutadas indirectamente |
| **Por qué no se elimina** | Depende de calidad de ingesta; encadenamiento T1080 → ejecución en orquestador |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.4](#34-alucinaciones-e-integridad-generativa-del-llm) (efecto en salida) |
| **Aceptado** | No |

---

### 4.11 STR-C12-R — Falta de trazabilidad del fundamento RAG (DREAD inicial 6,6 — Medio)

| Campo | Contenido |
|-------|-----------|
| **03** | A.12 — **R** |
| **04** | 6,6 — Medio |
| **05** | Sin técnica ATT&CK (`05` §5) |
| **06 — Controles** | Logging inmutable; NTP; SIEM |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Pérdida parcial de logs; correlación incompleta en SIEM; retención insuficiente |
| **Por qué no se elimina** | STRIDE §3.3 exige logs robustos pero no garantiza reconstrucción total; TSA/firmas no incluidas en `06` |
| **Nivel residual** | **Bajo** |
| **Transversal** | — |
| **Aceptado** | No |

---

### 4.12 STR-TB03-S — Ataque al flujo OAuth2 (DREAD inicial 6,6 — Medio)

| Campo | Contenido |
|-------|-----------|
| **03** | TB-03 — **S** |
| **04** | 6,6 — Medio |
| **05** | T1556; T1566.002 (sec.) |
| **06 — Controles** | MFA; hardening OAuth; tokens expiración; logging; M1021; transferencia IdP |
| **06 — Tratamiento** | Mitigar + Transferir |
| **Escenario residual** | Redirect manipulado; compromiso IdP; phishing OAuth |
| **Por qué no se elimina** | Autenticación primaria delegada; transferencia **no elimina** dependencia |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.3](#33-idp-externo-federado-tb-03) |
| **Aceptado** | Parcial — riesgo de autenticación transferido al IdP |

---

### 4.13 STR-TB09-I — Filtración horizontal en salidas (DREAD inicial 6,4 — Medio)

| Campo | Contenido |
|-------|-----------|
| **03** | TB-09 — **I** |
| **04** | 6,4 — Medio |
| **05** | T1005 |
| **06 — Controles** | Aislamiento recuperación (C07-I); RBAC sesión; logging salidas |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Mezcla de contexto entre asuntos en fase de **generación** (no solo retrieval) |
| **Por qué no se elimina** | Controles centrados en recuperación; sin filtrado de salida por tenant en `references/` |
| **Nivel residual** | **Medio** |
| **Transversal** | — |
| **Aceptado** | No |

---

### 4.14 STR-C13-T — Borrador contractual erróneo (DREAD inicial 6,4 — Medio)

| Campo | Contenido |
|-------|-----------|
| **03** | A.13 — **T** |
| **04** | 6,4 — Medio (Damage 9) |
| **05** | T1565 |
| **06 — Controles** | Segregación; revisión humana; integridad plantillas; logging |
| **06 — Tratamiento** | Mitigar + **Aceptar** |
| **Escenario residual** | Cláusulas alteradas no detectadas; envío prematuro al cliente |
| **Por qué no se elimina** | Generación IA + proceso humano fallible; sin diff contractual automático en `references/` |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.4](#34-alucinaciones-e-integridad-generativa-del-llm), [§3.5](#35-factor-humano-en-revisión-legal), [§3.1](#31-llm-comercial-tb-07--escenario-mixto-tb-07--tb-08) si usa LLM comercial |
| **Aceptado** | **Sí** — pendiente formalización por la organización (§1.6) |

---

### 4.15 STR-C10-T — Manipulación de claims en tokens (DREAD inicial 6,2 — Medio)

| Campo | Contenido |
|-------|-----------|
| **03** | A.10 — **T** |
| **04** | 6,2 — Medio |
| **05** | T1556 |
| **06 — Controles** | Validación firma/claims; MFA; M1026; logging |
| **06 — Tratamiento** | Mitigar |
| **Escenario residual** | Desincronización rol–claim; tokens válidos con claims incorrectos |
| **Por qué no se elimina** | Persistencia T1556 tras controles estándar de validación |
| **Nivel residual** | **Bajo** |
| **Transversal** | [§3.3](#33-idp-externo-federado-tb-03) |
| **Aceptado** | No |

---

### 4.16 STR-C14-T — Auditoría de cumplimiento manipulada o errónea (DREAD inicial 6,2 — Medio)

| Campo | Contenido |
|-------|-----------|
| **03** | A.14 — **T** |
| **04** | 6,2 — Medio (Damage 8) |
| **05** | T1565 |
| **06 — Controles** | Revisión experta; logging criterios; integridad salidas |
| **06 — Tratamiento** | Mitigar + **Aceptar** |
| **Escenario residual** | Dictamen falso u omitido aceptado por experto; sesgo en muestra |
| **Por qué no se elimina** | Automatización + juicio humano sin verificación formal en `references/` |
| **Nivel residual** | **Medio** |
| **Transversal** | [§3.4](#34-alucinaciones-e-integridad-generativa-del-llm), [§3.5](#35-factor-humano-en-revisión-legal) |
| **Aceptado** | **Sí** — pendiente formalización por la organización (§1.6) |

\* Limitación T1078 → T1068: ver [`05-mapa-attack.md`](05-mapa-attack.md) §1.

---

## 5. Riesgos aceptados formalmente

Las siguientes amenazas fueron marcadas con tratamiento **Aceptar** en `06`. La aceptación implica asumir el residual documentado, mantener controles de mitigación complementarios y monitorear indicadores apoyados en `references/` (logging STRIDE R; SIEM según `analisis_amenazas.md` §7.3).

| ID | Residual aceptado | Nivel | Condiciones de monitoreo |
|----|-------------------|-------|--------------------------|
| STR-C12-T3 | Alucinación con impacto legal pese a revisión humana | Medio | Auditoría de muestras de respuestas RAG; correlación logs fragmento–respuesta |
| STR-C13-T | Borrador contractual erróneo pese a revisión | Medio | Registro de borradores rechazados; trazabilidad plantilla–salida |
| STR-C14-T | Dictamen de auditoría erróneo pese a revisión experta | Medio | Logging de criterios y fragmentos usados |
| STR-TB10-I (rama B) | Exposición de embeddings en SaaS vectorial | Medio | Revisión periódica de modalidad de despliegue TB-10; minimización de metadatos |

**Responsable de la aceptación:** pendiente de definición por la organización (§1.6).

---

## 6. Matrices complementarias

### 6.1 Por componente afectado

| Componente | Amenazas | Nivel residual predominante |
|------------|----------|----------------------------|
| Proceso consulta RAG | C12-T, C12-T2, C12-T3, C12-R | Medio |
| Orquestador Python | C12-T, C12-T2 | Medio |
| Servicios API | C02-E, C10-T | Medio / Bajo |
| IdP / OAuth | TB-03-S, C10-T | Medio / Bajo |
| Base vectorial | C07-I, TB-10-I | Medio / Evitado (rama A) |
| Pipeline ETL | C05-T2, C12-T2 | Medio |
| Frontend / usuario | TB-01-S | Medio |
| TB lógica cliente–interno | TB-09-E, TB-09-I | Medio |
| LLM comercial (TB-07) | TB-07-I | Medio |
| Agentes borradores | C13-T | Medio |
| Auditoría automatizada | C14-T | Medio |

### 6.2 Por tipo de residual

| Tipo | Amenazas | Descripción |
|------|----------|-------------|
| **Técnico** | C12-T, C12-T2, C05-T2, C07-I, C02-E, TB-09-E, C10-T | Bypass de controles, bugs, poisoning, fuga semántica |
| **Tercero / transferido** | TB-07-I, TB-10-I (B), TB-03-S | LLM cloud, Pinecone, IdP — transferencia no elimina riesgo |
| **Proceso / humano** | C12-T3, C13-T, C14-T | Revisión legal fallible |
| **Inherente al modelo** | C12-T3, C13-T, C14-T, C12-T2 | Alucinación e integridad generativa |
| **Operativo / trazabilidad** | C12-R | Gaps de logging |

### 6.3 Comparación DREAD inicial vs residual cualitativo

| Nivel DREAD inicial (`04`) | Cantidad | Nivel residual (`07`) | Observación |
|----------------------------|----------|----------------------|-------------|
| Alto (9 amenazas) | 9 | Medio (8); Evitado (1 rama A TB-10) | Reducción cualitativa; Damage alto persiste como impacto potencial |
| Medio (7 amenazas) | 7 | Medio (5); Bajo (2) | C12-R y C10-T con mayor atenuación |

---

## 7. Monitoreo y revisión del residual

| Acción | Fuente | Amenazas relacionadas |
|--------|--------|----------------------|
| Reglas SIEM (login anómalo, exfil volumen, intérpretes) | `analisis_amenazas.md` §7.3 | Transversal; C12-T |
| Logging autenticación y consultas RAG | STRIDE §3.1 S, §3.3 R | TB-03-S, C12-R, C12-T3 |
| Monitoreo egress hacia proveedores LLM | `06` §11 | TB-07-I |
| Documentar rama TB-10 y política TB-07/TB-08 activas | `06` §11 | TB-10-I, TB-07-I |
| Revisión tras cambio arquitectónico | Buena práctica derivada del escenario | TB-10, TB-07 |

---

## 8. Límites de `references/` para reducción adicional

| Dominio | Amenazas | Qué no permite justificar en `references/` |
|---------|----------|---------------------------------------------|
| Prompt injection / RAG adversarial | C12-T, C12-T2 | Controles LLM-específicos más allá de validación genérica y M1038 |
| Aislamiento semántico vectorial | C07-I, TB-09-I | Filtros por similitud inter-tenant |
| Alucinación / grounding | C12-T3, C13-T, C14-T | Verificación automática de citas; métricas de fidelidad |
| Calidad revisión humana | C12-T3, C13-T, C14-T | Estándares medibles de doble revisión |
| Subprocesadores | TB-07-I, TB-10-I, TB-03-S | Cláusulas DPA, certificaciones, residencia de datos |
| Clasificación de sensibilidad | TB-07-I | Matriz datos → ruta TB-07 vs TB-08 |

En estos casos el residual documentado refleja el **tope** de reducción alcanzable con el marco metodológico del curso.

---

## 9. Conclusiones

Tras aplicar el plan de mitigación (`06`), **ninguna de las 16 amenazas desaparece por completo** salvo **STR-TB10-I en rama Chroma on-prem**, donde el riesgo queda **evitado**. El patrón dominante es riesgo residual **Medio**: los controles de `references/` atenúan vectores clásicos (autenticación, autorización, phishing, ingesta) pero **no eliminan** riesgos estructurales de IA generativa, dependencia de subprocesadores ni factor humano en la revisión legal.

Cuatro amenazas incorporan **aceptación explícita** del residual (C12-T3, C13-T, C14-T, TB-10-I rama B), sujeta a formalización por la organización. La **transferencia** a terceros (LLM, Pinecone, IdP) permanece como riesgo residual de la firma aunque sea tratamiento complementario válido.

La evaluación de este documento es **cualitativa** y posterior a la implementación prevista de controles; **no sustituye** ni recalcula los scores DREAD de [`04-priorizacion-dread.md`](04-priorizacion-dread.md).

---

## Relación con otros entregables

| Documento | Relación |
|-----------|----------|
| [`03-analisis-stride.md`](03-analisis-stride.md) | Origen de amenazas y categorías. |
| [`04-priorizacion-dread.md`](04-priorizacion-dread.md) | Urgencia inicial (pre-mitigación). |
| [`05-mapa-attack.md`](05-mapa-attack.md) | Técnicas ATT&CK por amenaza. |
| [`06-plan-mitigacion.md`](06-plan-mitigacion.md) | Controles y tratamientos aplicados. |

---

## Trazabilidad

- Inventario: [`01-inventario-activos.md`](01-inventario-activos.md)
- Trust boundaries: [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md)
- Referencias: [`../references/STRIDE.md`](../references/STRIDE.md), [`../references/DREAD.md`](../references/DREAD.md), [`../references/analisis_amenazas.md`](../references/analisis_amenazas.md), [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK)
- Registro de prompts (IA): [`../prompts/14-prompt-riesgos-residuales-analisis-previo.md`](../prompts/14-prompt-riesgos-residuales-analisis-previo.md), [`../prompts/15-prompt-riesgos-residuales-redaccion.md`](../prompts/15-prompt-riesgos-residuales-redaccion.md)
