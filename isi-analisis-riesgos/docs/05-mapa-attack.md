# Mapa ATT&CK

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes metodológicas:** [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK), [`../references/analisis_amenazas.md`](../references/analisis_amenazas.md) (solo contexto de TTPs sectoriales; las técnicas citadas deben existir en `MITRE_ATTCK` §3), [`04-priorizacion-dread.md`](04-priorizacion-dread.md), [`03-analisis-stride.md`](03-analisis-stride.md).

**Alcance:** mapeo de las **16** amenazas evaluadas en DREAD hacia tácticas y técnicas MITRE ATT&CK documentadas en el material de clase. **No** incluye mitigaciones ni riesgo residual.

---

## 1. Introducción

### Propósito

Relacionar las amenazas identificadas en STRIDE y priorizadas en DREAD con el marco **MITRE ATT&CK**, para contextualizar tácticas y técnicas (TTPs) y apoyar el [`06-plan-mitigacion.md`](06-plan-mitigacion.md).

### Alcance

- **Entrada:** las 16 amenazas `STR-*` de la tabla resumen en [`04-priorizacion-dread.md`](04-priorizacion-dread.md).
- **Salida:** táctica y técnica(s) ATT&CK por amenaza, con cadena de trazabilidad a activos (`01-inventario-activos.md`) y trust boundaries (`02-arquitectura-trust-boundaries.md`).
- **Fuera de alcance:** MITRE ATLAS, OWASP LLM, frameworks no presentes en `references/`.

### Limitaciones metodológicas

| Limitación | Descripción |
|------------|-------------|
| **Solo técnicas tabuladas en clase** | Se utilizan únicamente IDs definidos en [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK) §3 (tablas por táctica). Si una técnica aparece solo en listados resumidos de otros apuntes sin definición en `MITRE_ATTCK`, **no se emplea**. |
| **ATT&CK centrado en adversario** | ATT&CK modela comportamiento ofensivo. Amenazas que describen **fallos del sistema** sin actor (p. ej. alucinación del LLM) se mapean al **efecto** adversarial posible (Impact) cuando la descripción STRIDE lo permite. |
| **STR-C12-R** | Amenaza de **no repudio** (falta de trazabilidad); sin técnica ofensiva ATT&CK directa. Ver §5. |
| **STR-C02-E y STR-TB09-E** | Las descripciones STRIDE apuntan a **abuso de identidades o tokens válidos** con autorización incorrecta (afín a *Valid Accounts*, citada en [`analisis_amenazas.md`](../references/analisis_amenazas.md) §6.2 como **T1078**). **T1078 no figura** en las tablas de [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK) §3. Se asigna **T1068** (*Exploitation for Privilege Escalation*) como aproximación disponible: explotación de una debilidad en controles de autorización que produce acceso efectivo no permitido (categoría **E** en STRIDE). |
| **TB-10 condicional** | **STR-TB10-I** aplica solo si el despliegue usa base vectorial SaaS (p. ej. Pinecone), según [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md) TB-10. |

### Cadena de trazabilidad

```text
Amenaza STRIDE → Score DREAD → Activo afectado → Trust Boundary → Táctica ATT&CK → Técnica ATT&CK → Justificación
```

---

## 2. Metodología de mapeo

1. Tomar ID, descripción y activos desde [`03-analisis-stride.md`](03-analisis-stride.md).
2. Tomar score y nivel desde [`04-priorizacion-dread.md`](04-priorizacion-dread.md).
3. Ubicar trust boundary en [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md).
4. Asignar **técnica primaria** cuya definición en `MITRE_ATTCK` §3 sea coherente con la descripción STRIDE.
5. Añadir **técnica secundaria** solo si un segundo vector está explícito en la descripción y la técnica existe en `MITRE_ATTCK` §3.

---

## 3. Resumen ejecutivo

### Cantidad mapeada

| Resultado | Cantidad |
|-----------|----------|
| Amenazas con al menos una técnica ATT&CK (primaria) | **15** |
| Amenazas sin técnica ofensiva ATT&CK | **1** (STR-C12-R) |
| **Total** | **16** |

### Tabla maestra

| ID STRIDE | Cat. | Score DREAD | Nivel | Activo principal | Trust boundary | Táctica (primaria) | Técnica primaria | Técnica secundaria |
|-----------|------|-------------|-------|------------------|----------------|--------------------|------------------|-------------------|
| STR-C12-T | T | 8,0 | Alto | Consultas en lenguaje natural | TB-02, TB-06, TB-07/08 | Execution | T1059.006 | T1565 |
| STR-TB07-I | I | 7,8 | Alto | Representación indexada | TB-07 | Exfiltration | T1567.002 | T1005 |
| STR-C12-T3 | T | 7,6 | Alto | Respuestas RAG | TB-06, TB-07/08 | Impact | T1565 | — |
| STR-TB09-E | E | 7,6 | Alto | Documentación legal de referencia | TB-09 | Privilege Escalation | T1068 * | — |
| STR-C07-I | I | 7,4 | Alto | Representación indexada | TB-06 | Collection | T1005 | — |
| STR-C02-E | E | 7,4 | Alto | Identidades federadas (OAuth2) | TB-02, TB-09 | Privilege Escalation | T1068 * | — |
| STR-C05-T2 | T | 7,2 | Alto | Documentos contractuales | TB-04, TB-05, TB-06 | Lateral Movement | T1080 | T1565 |
| STR-TB10-I | I | 7,2 | Alto | Representación indexada | TB-10 † | Collection | T1530 | — |
| STR-TB01-S | S | 7,0 | Alto | Identidades federadas | TB-01 | Initial Access | T1566.002 | T1036 |
| STR-C12-T2 | T | 6,8 | Medio | Documentos contractuales | TB-05, TB-06, TB-07/08 | Lateral Movement | T1080 | T1059.006 |
| STR-C12-R | R | 6,6 | Medio | Respuestas RAG | — | — | — | — |
| STR-TB03-S | S | 6,6 | Medio | Identidades federadas | TB-03 | Persistence | T1556 | T1566.002 |
| STR-TB09-I | I | 6,4 | Medio | Respuestas RAG | TB-09 | Collection | T1005 | — |
| STR-C13-T | T | 6,4 | Medio | Borradores preliminares | TB-07/08, TB-09 | Impact | T1565 | — |
| STR-C10-T | T | 6,2 | Medio | Identidades federadas | TB-03, TB-09 | Persistence | T1556 | — |
| STR-C14-T | T | 6,2 | Medio | Resultados de auditoría | TB-07/08, TB-09 | Impact | T1565 | — |

\* Ver limitación **T1078** en §1.  
† Solo si aplica despliegue SaaS vectorial (TB-10).

### Frecuencia de tácticas (técnica primaria)

| Táctica ATT&CK | Amenazas | Frecuencia |
|----------------|----------|------------|
| Impact | C12-T3, C13-T, C14-T | 3 |
| Collection | C07-I, TB10-I, TB09-I | 3 |
| Privilege Escalation | TB09-E, C02-E | 2 |
| Lateral Movement | C05-T2, C12-T2 | 2 |
| Persistence | TB03-S, C10-T | 2 |
| Execution | C12-T | 1 |
| Exfiltration | TB07-I | 1 |
| Initial Access | TB01-S | 1 |

### Técnicas compartidas (primaria o secundaria)

| Técnica | Nombre (`MITRE_ATTCK` §3) | Amenazas |
|---------|---------------------------|----------|
| **T1565** | Data Manipulation | C12-T (sec.), C12-T3, C05-T2 (sec.), C13-T, C14-T |
| **T1005** | Data from Local System | C07-I, TB09-I, TB07-I (sec.) |
| **T1080** | Taint Shared Content | C05-T2, C12-T2 |
| **T1068** | Exploitation for Privilege Escalation | TB09-E, C02-E |
| **T1556** | Modify Authentication Process | TB03-S, C10-T |
| **T1059.006** | Python | C12-T, C12-T2 (sec.) |
| **T1566.002** | Spearphishing Link | TB01-S, TB03-S (sec.) |

---

## 4. Mapeo detallado por amenaza

### 4.1 Nivel Alto

#### STR-C12-T — Prompt injection directa en consulta RAG

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 8,0 — Alto |
| **Descripción (STRIDE)** | Instrucciones maliciosas en la consulta del usuario que intentan anular políticas del sistema, exfiltrar contexto o manipular la salida del LLM (`03` A.12). |
| **Activos afectados** | Consultas en lenguaje natural; Respuestas del sistema basadas en RAG; Representación indexada para recuperación |
| **Trust boundaries** | TB-02 (frontend–API), TB-06 (consulta–vector DB), TB-07 o TB-08 (invocación LLM) |
| **Táctica primaria** | **Execution** |
| **Técnica primaria** | **T1059.006** — Python (`MITRE_ATTCK` §3.2): el orquestador LangChain/LlamaIndex en Python procesa la consulta maliciosa como parte de la cadena de ejecución. |
| **Táctica secundaria** | **Impact** |
| **Técnica secundaria** | **T1565** — Data Manipulation (§3.11): objetivo documentado de alterar la salida o el contexto recuperado. |
| **Justificación** | La amenaza se materializa cuando entrada en lenguaje natural es interpretada por el intérprete Python del orquestador; el daño buscado es manipulación de datos de salida. |

---

#### STR-TB07-I — Exposición de contexto legal sensible hacia LLM comercial

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,8 — Alto |
| **Descripción (STRIDE)** | Tránsito de consultas, contexto recuperado o fragmentos de contratos/jurisprudencia al proveedor cloud como parte del prompt (`03` TB-07). |
| **Activos afectados** | Consultas en lenguaje natural; Representación indexada; Documentos contractuales; Documentación legal de referencia; Respuestas RAG |
| **Trust boundaries** | **TB-07** — Orquestador ↔ LLM comercial (OpenAI / Anthropic) |
| **Táctica primaria** | **Exfiltration** |
| **Técnica primaria** | **T1567.002** — Exfiltration to Cloud Storage (§3.10): envío de datos hacia infraestructura cloud del proveedor vía servicio web. |
| **Táctica secundaria** | **Collection** |
| **Técnica secundaria** | **T1005** — Data from Local System (§3.8): el actor provoca recuperación de fragmentos del índice/corpus que luego transitan por el canal. |
| **Justificación** | TB-07 documenta salida de perímetro hacia tercero; la técnica de exfiltración a servicio cloud describe el destino del dato sensible. |

---

#### STR-C12-T3 — Alucinación o contexto manipulado con impacto legal

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,6 — Alto |
| **Descripción (STRIDE)** | El LLM presenta como fundamentada una respuesta con citas o conclusiones jurídicas incorrectas, inventadas o no sustentadas en fragmentos recuperados (`03` A.12). |
| **Activos afectados** | Respuestas RAG; Documentación legal de referencia; Consultas en lenguaje natural |
| **Trust boundaries** | TB-06, TB-07 o TB-08 (flujo RAG completo) |
| **Táctica primaria** | **Impact** |
| **Técnica primaria** | **T1565** — Data Manipulation (§3.11): manipulación de la información presentada al usuario (integridad del asesoramiento). |
| **Técnica secundaria** | — |
| **Justificación** | La descripción STRIDE es de integridad de salida (categoría T). ATT&CK modela el resultado como manipulación de datos visibles para la víctima. *Nota:* puede manifestarse sin adversario; el mapeo describe el efecto explotable o inducible. |

---

#### STR-TB09-E — Cliente accede a corpus o funciones de otro rol

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,6 — Alto |
| **Descripción (STRIDE)** | Cliente que accede a corpus interno, documentos de otro cliente o funciones reservadas al personal (`03` TB-09). |
| **Activos afectados** | Documentación legal de referencia; Documentos contractuales; Resultados de auditoría; Plantillas institucionales; Procesos RAG, borradores y auditoría |
| **Trust boundaries** | **TB-09** — Separación lógica cliente / personal interno |
| **Táctica primaria** | **Privilege Escalation** |
| **Técnica primaria** | **T1068** — Exploitation for Privilege Escalation (§3.4): explotación de debilidad en controles de autorización que permite capacidades no previstas para el rol. |
| **Técnica secundaria** | — |
| **Justificación** | Categoría STRIDE **E** (elevation of privilege). Semánticamente el abuso encaja con *cuentas válidas* y autorización incorrecta (**T1078**, listada en `analisis_amenazas.md` §6.2), pero **T1078 no está definida** en `MITRE_ATTCK` §3; se usa **T1068** como técnica disponible en el material de clase. |

---

#### STR-C07-I — Retrieval leakage

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,4 — Alto |
| **Descripción (STRIDE)** | Recuperación por similitud que expone fragmentos fuera del alcance del consultante (cross-tenant, cross-corpus o metadatos incorrectos) (`03` A.7). |
| **Activos afectados** | Representación indexada; Documentos contractuales; Documentación legal; Respuestas RAG |
| **Trust boundaries** | **TB-06** — Indexación/consulta ↔ base vectorial |
| **Táctica primaria** | **Collection** |
| **Técnica primaria** | **T1005** — Data from Local System (§3.8): recolección de archivos/datos del sistema (índice) no autorizados para el contexto del consultante. |
| **Técnica secundaria** | — |
| **Justificación** | La fuga ocurre en la fase de recuperación de datos almacenados en el índice vectorial. |

---

#### STR-C02-E — Fallo de autorización en API

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,4 — Alto |
| **Descripción (STRIDE)** | Fallo de autorización que permite a un sujeto acceder a recursos u operaciones de otro rol o tenant (`03` A.2). |
| **Activos afectados** | Identidades federadas (OAuth2); Documentos contractuales; Documentación legal; Plantillas; Resultados de auditoría |
| **Trust boundaries** | TB-02, **TB-09** |
| **Táctica primaria** | **Privilege Escalation** |
| **Técnica primaria** | **T1068** — Exploitation for Privilege Escalation (§3.4): explotación de fallo en la capa de autorización de la API. |
| **Técnica secundaria** | — |
| **Justificación** | Misma limitación que STR-TB09-E: el escenario describe uso de **identidad o token válido** con alcance indebido (afín a **T1078**, no tabulada en `MITRE_ATTCK` §3). **T1068** aproxima la explotación del fallo de control de acceso que eleva efectivamente los privilegios del sujeto. |

---

#### STR-C05-T2 — Data poisoning en documentos cargados

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,2 — Alto |
| **Descripción (STRIDE)** | Documento cargado con contenido diseñado para envenenar el índice o influir en recuperaciones futuras (`03` A.5). |
| **Activos afectados** | Documentos contractuales; Representación indexada; Proceso de ingestión |
| **Trust boundaries** | TB-04, TB-05, TB-06 |
| **Táctica primaria** | **Lateral Movement** |
| **Técnica primaria** | **T1080** — Taint Shared Content (§3.7): contaminación de contenido compartido (corpus/indexación compartida). |
| **Táctica secundaria** | **Impact** |
| **Técnica secundaria** | **T1565** — Data Manipulation (§3.11): alteración de la representación indexada y de recuperaciones futuras. |
| **Justificación** | El vector es la ingesta de documento malicioso al corpus compartido; el efecto es manipulación del índice RAG. |

---

#### STR-TB10-I — Exposición de embeddings a proveedor SaaS vectorial

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,2 — Alto |
| **Descripción (STRIDE)** | Acceso del proveedor SaaS del índice a embeddings y metadatos derivados de documentos confidenciales (`03` TB-10). |
| **Activos afectados** | Representación indexada; Base de datos vectorial (Pinecone); Documentos contractuales (derivados) |
| **Trust boundaries** | **TB-10** *(condicional: solo Pinecone u otro SaaS)* |
| **Táctica primaria** | **Collection** |
| **Técnica primaria** | **T1530** — Data from Cloud Storage (§3.11): acceso a datos alojados en infraestructura cloud del proveedor. |
| **Técnica secundaria** | — |
| **Justificación** | TB-10 documenta residencia de embeddings en tercero; la técnica describe obtención de datos desde almacenamiento cloud. |

---

#### STR-TB01-S — Phishing / interfaz falsa

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 7,0 — Alto |
| **Descripción (STRIDE)** | El usuario autentica o introduce datos en una UI que imita el frontend institucional fuera del dominio de confianza (`03` TB-01). |
| **Activos afectados** | Identidades federadas; Aplicación frontend (Next.js) |
| **Trust boundaries** | **TB-01** |
| **Táctica primaria** | **Initial Access** |
| **Técnica primaria** | **T1566.002** — Spearphishing Link (§3.1): enlace a sitio falso que suplanta la aplicación legítima. |
| **Táctica secundaria** | **Defense Evasion** |
| **Técnica secundaria** | **T1036** — Masquerading (§3.5): interfaz que oculta la identidad real del servicio. |
| **Justificación** | La descripción STRIDE es suplantación en el cruce usuario–frontend; las técnicas documentan engaño por enlace y apariencia falsa. |

---

### 4.2 Nivel Medio

#### STR-C12-T2 — Indirect prompt injection vía documentos del corpus

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 6,8 — Medio |
| **Descripción (STRIDE)** | Instrucciones embebidas en documentos del corpus que el RAG recupera y el LLM interpreta como mandatos (`03` A.12). |
| **Activos afectados** | Documentos contractuales; Documentación legal; Representación indexada; Respuestas RAG |
| **Trust boundaries** | TB-05, TB-06, TB-07/08 |
| **Táctica primaria** | **Lateral Movement** |
| **Técnica primaria** | **T1080** — Taint Shared Content (§3.7): contenido compartido en el corpus contaminado con instrucciones ocultas. |
| **Táctica secundaria** | **Execution** |
| **Técnica secundaria** | **T1059.006** — Python (§3.2): el orquestador ejecuta la cadena que incorpora el fragmento recuperado al prompt. |
| **Justificación** | Diferente de STR-C05-T2 (ingesta): aquí el documento ya está en el corpus y el vector de ataque es la **recuperación** en consulta. |

---

#### STR-TB03-S — Ataque al flujo OAuth2 federado

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 6,6 — Medio |
| **Descripción (STRIDE)** | Redirect manipulado, intercepción de código de autorización o cliente OAuth mal registrado (`03` TB-03). |
| **Activos afectados** | Identidades federadas; Proveedor IdP |
| **Trust boundaries** | **TB-03** |
| **Táctica primaria** | **Persistence** |
| **Técnica primaria** | **T1556** — Modify Authentication Process (§3.3): modificación o abuso del proceso de autenticación federada OAuth2. |
| **Táctica secundaria** | **Initial Access** |
| **Técnica secundaria** | **T1566.002** — Spearphishing Link (§3.1): si el vector incluye engañar al usuario con redirect malicioso. |
| **Justificación** | La amenaza ataca el mecanismo de federación en el cruce de perímetro TB-03. |

---

#### STR-TB09-I — Filtración horizontal en respuestas RAG o borradores

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 6,4 — Medio |
| **Descripción (STRIDE)** | Respuesta RAG o borrador que mezcla información de otro asunto o cliente (`03` TB-09). |
| **Activos afectados** | Respuestas RAG; Borradores preliminares; Representación indexada |
| **Trust boundaries** | **TB-09** |
| **Táctica primaria** | **Collection** |
| **Técnica primaria** | **T1005** — Data from Local System (§3.8): recolección indebida de datos de otro contexto que aparecen en la salida. |
| **Técnica secundaria** | — |
| **Justificación** | Complementa STR-C07-I en la manifestación en la frontera de roles; el daño es datos ajenos expuestos en la respuesta visible. |

---

#### STR-C13-T — Borrador contractual con cláusulas erróneas

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 6,4 — Medio |
| **Descripción (STRIDE)** | Borrador con cláusulas alteradas, omitidas o jurídicamente incoherentes (`03` A.13). |
| **Activos afectados** | Borradores preliminares; Plantillas institucionales; Documentos contractuales |
| **Trust boundaries** | TB-07/08, TB-09 |
| **Táctica primaria** | **Impact** |
| **Técnica primaria** | **T1565** — Data Manipulation (§3.11): manipulación del contenido del borrador generado. |
| **Técnica secundaria** | — |
| **Justificación** | Integridad del entregable legal automatizado; categoría T en STRIDE. |

---

#### STR-C10-T — Manipulación de claims de rol en tokens OAuth2

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 6,2 — Medio |
| **Descripción (STRIDE)** | Manipulación de claims de rol (cliente vs interno) en tokens emitidos o aceptados (`03` A.10). |
| **Activos afectados** | Identidades federadas; Servicios de aplicación / API |
| **Trust boundaries** | TB-03, TB-09 |
| **Táctica primaria** | **Persistence** |
| **Técnica primaria** | **T1556** — Modify Authentication Process (§3.3): alteración del proceso de autenticación/autorización vía tokens. |
| **Técnica secundaria** | — |
| **Justificación** | La descripción apunta a modificación de atributos de identidad en el flujo OAuth2. |

---

#### STR-C14-T — Resultados de auditoría falsos o manipulados

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 6,2 — Medio |
| **Descripción (STRIDE)** | Hallazgos omitidos, inventados o contradichos por el documento evaluado (`03` A.14). |
| **Activos afectados** | Resultados de auditoría automatizada; Documentos contractuales |
| **Trust boundaries** | TB-07/08, TB-09 |
| **Táctica primaria** | **Impact** |
| **Técnica primaria** | **T1565** — Data Manipulation (§3.11): manipulación de los resultados del proceso de auditoría. |
| **Técnica secundaria** | — |
| **Justificación** | Integridad del dictamen de cumplimiento; categoría T en STRIDE. |

---

## 5. Amenazas sin equivalente ATT&CK ofensivo

### STR-C12-R — Imposibilidad de reconstruir fragmentos sustentadores

| Campo | Contenido |
|-------|-----------|
| **Score DREAD / Nivel** | 6,6 — Medio |
| **Categoría STRIDE** | **R** — Repudiation |
| **Descripción (STRIDE)** | Imposibilidad de reconstruir qué fragmentos sustentaron una respuesta ante reclamación o revisión (`03` A.12). |
| **Activos afectados** | Respuestas RAG; Representación indexada; Consultas en lenguaje natural |
| **Trust boundaries** | Proceso lógico de consulta RAG (sin TB exclusiva) |
| **Táctica / Técnica ATT&CK** | **No aplica** |
| **Justificación de exclusión** | ATT&CK documenta TTPs de **adversario**. Esta amenaza describe **ausencia de trazabilidad** del sistema (gap de logging/no repudio), no una acción ofensiva mapeable a §3 de `MITRE_ATTCK`. No corresponde asignar **T1070** (Indicator Removal): la descripción no indica borrado de evidencia por atacante, sino falta de registro. Relacionada con STR-C03-R en orquestador (`03` A.3), no evaluada en DREAD. |

---

## 6. Análisis agregado para etapas posteriores

### Observaciones para mitigación (`06-plan-mitigacion.md`)

- **T1565** y **T1005** concentran varias amenazas RAG y multi-tenant; los controles deberán priorizar integridad de salidas, aislamiento de recuperación y segmentación lógica.
- **T1068** en TB09-E y C02-E comparte la limitación **T1078**; las mitigaciones de autorización (gestión de privilegios, validación de tokens) aplican a ambas.
- **TB-07** y **TB-10** vinculan técnicas de exfiltración/colección cloud; coherente con política del escenario de LLM local para datos sensibles.

### Diagrama de flujo de ataque

Artefacto previsto: [`../diagrams/attack-flow.png`](../diagrams/attack-flow.png) — **TODO:** generar a partir de las cadenas TB → técnica de este documento.

---

## 7. Relación con otros entregables

| Documento | Relación |
|-----------|----------|
| [`01-inventario-activos.md`](01-inventario-activos.md) | Activos citados en cada fila del mapeo. |
| [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md) | TB-01 … TB-10 (TB-10 condicional). |
| [`03-analisis-stride.md`](03-analisis-stride.md) | Origen de IDs y descripciones `STR-*`. |
| [`04-priorizacion-dread.md`](04-priorizacion-dread.md) | Scores DREAD y selección de 16 amenazas. |
| [`06-plan-mitigacion.md`](06-plan-mitigacion.md) | Controles alineados a técnicas ATT&CK. |

---

## Trazabilidad

- Referencia MITRE: [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK)
- Análisis de amenazas (contexto TTPs): [`../references/analisis_amenazas.md`](../references/analisis_amenazas.md)
- Registro de prompts (IA): [`../prompts/09-prompt-mapa-attack.md`](../prompts/09-prompt-mapa-attack.md), [`../prompts/11-prompt-mapa-attack-redaccion.md`](../prompts/11-prompt-mapa-attack-redaccion.md)
