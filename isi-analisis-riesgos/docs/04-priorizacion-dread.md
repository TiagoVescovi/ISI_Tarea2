# Priorización DREAD

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes metodológicas:** [`../references/DREAD.md`](../references/DREAD.md), [`../references/STRIDE.md`](../references/STRIDE.md).

**Entrada:** catálogo de amenazas en [`03-analisis-stride.md`](03-analisis-stride.md) (63 IDs `STR-*`).

**Alcance:** priorización cualitativa-cuantitativa mediante DREAD. **No** incluye mitigaciones, plan de tratamiento ni riesgo residual.

---

## Introducción

### Qué es DREAD

DREAD es un modelo de evaluación de riesgos de seguridad que califica amenazas o vulnerabilidades en **cinco dimensiones** (Damage, Reproducibility, Exploitability, Affected Users, Discoverability), cada una en una escala de **0 a 10**, y obtiene un **puntaje medio** entre 0 y 10 para priorizar la atención. La metodología y las escalas utilizadas en este documento siguen estrictamente [`../references/DREAD.md`](../references/DREAD.md).

### Relación entre STRIDE y DREAD

Según [`../references/STRIDE.md`](../references/STRIDE.md) y [`../references/DREAD.md`](../references/DREAD.md), **STRIDE** responde *qué* amenazas existen (categorías S, T, R, I, D, E sobre componentes y fronteras), mientras que **DREAD** responde *con qué urgencia relativa* debe tratarse cada amenaza identificada. El flujo del trabajo es: inventario y arquitectura → matriz STRIDE → priorización DREAD → (etapas posteriores) mitigación y controles.

### Criterios de selección de amenazas evaluadas

Del catálogo revisado de **63** amenazas (`STR-*`), se evaluaron **16** según los criterios siguientes:

1. **Relevancia para el escenario legal y RAG:** exposición de contratos confidenciales, integridad del asesoramiento automatizado, vectores IA/RAG (prompt injection, data poisoning, retrieval leakage).
2. **Cobertura STRIDE:** al menos una amenaza por categoría principal (S, T, R, I, E); **D** representada donde el impacto operativo es material (no se forzó DoS genérico si el daño al negocio es secundario frente a confidencialidad e integridad).
3. **Diversidad de componentes:** API, proceso RAG, ETL, base vectorial, borradores, auditoría, IdP, fronteras TB-01, TB-03, TB-07, TB-09, TB-10.
4. **Evitar redundancia en DREAD:** cuando dos IDs STRIDE describen el mismo daño (p. ej. escalada en API vs frontera lógica cliente/interno), se priorizó la instancia con mayor trazabilidad al activo; la complementaria queda en la matriz STRIDE para mitigación futura.

### Fórmula y clasificación de severidad

```text
DREAD Score = (Damage + Reproducibility + Exploitability + Affected Users + Discoverability) / 5
```

| Rango del score | Nivel | Fuente |
|-----------------|-------|--------|
| 9,0 – 10,0 | Crítico | [`DREAD.md`](../references/DREAD.md) §3.2 |
| 7,0 – 8,9 | Alto | idem |
| 5,0 – 6,9 | Medio | idem |
| 3,0 – 4,9 | Bajo | idem |
| 0,0 – 2,9 | Informativo | idem |

En la tabla resumen, la primera columna **D** = Damage (daño); la segunda **D** = Discoverability (detectabilidad), conforme a la convención del material de la materia.

---

## Tabla resumen

| ID STRIDE | Descripción breve | D | R | E | A | D | Score | Nivel |
|-----------|-------------------|---|---|---|---|---|-------|-------|
| STR-C12-T | Prompt injection directa en consulta RAG | 7 | 9 | 8 | 7 | 9 | **8,0** | Alto |
| STR-TB07-I | Exposición de contexto legal sensible hacia LLM comercial en la nube | 9 | 8 | 7 | 9 | 6 | **7,8** | Alto |
| STR-C12-T3 | Alucinación o contexto manipulado con impacto en asesoramiento legal | 9 | 8 | 5 | 9 | 7 | **7,6** | Alto |
| STR-TB09-E | Cliente accede a corpus o funciones de otro cliente o del personal interno | 9 | 8 | 7 | 7 | 7 | **7,6** | Alto |
| STR-C07-I | Retrieval leakage (fragmentos fuera de alcance en base vectorial) | 9 | 7 | 7 | 7 | 7 | **7,4** | Alto |
| STR-C02-E | Fallo de autorización en API (acceso cross-tenant o cross-rol) | 8 | 8 | 7 | 7 | 7 | **7,4** | Alto |
| STR-C05-T2 | Data poisoning en documentos cargados (ingesta al índice RAG) | 8 | 8 | 7 | 8 | 5 | **7,2** | Alto |
| STR-TB10-I | Exposición de embeddings a proveedor SaaS de base vectorial (Pinecone) | 8 | 9 | 3 | 9 | 7 | **7,2** | Alto |
| STR-TB01-S | Phishing / interfaz falsa frente al usuario | 7 | 8 | 7 | 5 | 8 | **7,0** | Alto |
| STR-C12-T2 | Indirect prompt injection vía documentos del corpus | 8 | 7 | 6 | 8 | 5 | **6,8** | Medio |
| STR-C12-R | Imposibilidad de reconstruir fragmentos que sustentaron una respuesta RAG | 7 | 10 | 3 | 8 | 5 | **6,6** | Medio |
| STR-TB03-S | Ataque al flujo OAuth2 federado | 8 | 7 | 6 | 7 | 5 | **6,6** | Medio |
| STR-TB09-I | Filtración horizontal en respuestas RAG o borradores entre asuntos | 8 | 7 | 6 | 5 | 6 | **6,4** | Medio |
| STR-C13-T | Borrador contractual con cláusulas erróneas o incoherentes (agente IA) | 9 | 8 | 4 | 5 | 6 | **6,4** | Medio |
| STR-C10-T | Manipulación de claims de rol (cliente vs interno) en tokens OAuth2 | 8 | 7 | 5 | 7 | 4 | **6,2** | Medio |
| STR-C14-T | Resultados de auditoría de cumplimiento falsos o manipulados | 8 | 7 | 5 | 6 | 5 | **6,2** | Medio |

---

## Justificación detallada

### STR-C12-T

**Descripción**

**Prompt injection (directa):** instrucciones maliciosas en la consulta del usuario que intentan anular políticas del sistema, exfiltrar contexto recuperado o alterar la salida del LLM en el proceso de consulta RAG.

**Damage (7/10)**

Según escala DREAD §2.1 (7 = alto: pérdida significativa de datos / impacto grave), la integridad del asesoramiento automatizado y la posible exposición de fragmentos del corpus en respuestas indebidas afectan la confianza del cliente y la responsabilidad profesional; no implica destrucción total del sistema (10).

**Reproducibility (9/10)**

Escala §2.2 (9 = muy alta, condiciones comunes): vectores documentados en el catálogo STRIDE; reproducible de forma consistente en sistemas con entrada en lenguaje natural sin controles específicos.

**Exploitability (8/10)**

Escala §2.3 (7 = alta; 8 cercano a herramientas y ejemplos públicos): no requiere compromiso previo del servidor; basta acceso legítimo a la función de consulta.

**Affected Users (7/10)**

Escala §2.4 (7 = algún grupo grande): cualquier usuario con permiso de consulta RAG puede ser vector o víctima indirecta si el sistema devuelve información indebida.

**Discoverability (9/10)**

Escala §2.5 (9 = fácil de descubrir): ampliamente documentado en guías de seguridad LLM y pruebas funcionales básicas.

**Score DREAD:** (7 + 9 + 8 + 7 + 9) / 5 = **8,0**

**Nivel:** Alto

---

### STR-TB07-I

**Descripción**

**Exposición de información sensible vía RAG hacia tercero:** tránsito de consultas, contexto recuperado o fragmentos de contratos y jurisprudencia al proveedor de LLM comercial (OpenAI / Anthropic) al cruzar TB-07.

**Damage (9/10)**

Escala §2.1 (9 = muy alto, base de datos / corpus comprometido): divulgación de contratos confidenciales y datos de clientes a subprocesador externo; impacto en secreto profesional y normativa de datos personales (Ley 18.331 en el marco del escenario).

**Reproducibility (8/10)**

Escala §2.2 (7–9): ocurre de forma previsible cuando las consultas RAG enrutan al LLM cloud con contexto recuperado (configuración prevista en el escenario para parte de la carga).

**Exploitability (7/10)**

Escala §2.3 (7 = alta con adaptación): puede ser consecuencia de configuración por defecto o de un atacante que dispare consultas que maximicen contexto enviado; no requiere exploit de bajo nivel.

**Affected Users (9/10)**

Escala §2.4 (9 = muchos, >50 %): afecta potencialmente a todos los clientes cuyos documentos entren en el contexto enviado al proveedor.

**Discoverability (6/10)**

Escala §2.5 (5–7): requiere revisión de arquitectura, trazas de red o política de enrutamiento; no siempre visible para el usuario final.

**Score DREAD:** (9 + 8 + 7 + 9 + 6) / 5 = **7,8**

**Nivel:** Alto

---

### STR-C12-T3

**Descripción**

**Manipulación de contexto recuperado / alucinación con impacto legal:** el LLM presenta respuestas como fundamentadas con citas, cláusulas o conclusiones jurídicas incorrectas o no sustentadas en los fragmentos recuperados.

**Damage (9/10)**

Escala §2.1 (9): decisiones o actuaciones basadas en asesoramiento automatizado erróneo en dominio legal; riesgo de daño reputacional, contractual y de cumplimiento.

**Reproducibility (8/10)**

Escala §2.2 (7–9): comportamiento inherente a LLMs en RAG; se manifiesta con regularidad sin controles de grounding y revisión humana.

**Exploitability (5/10)**

Escala §2.3 (5 = media): puede surgir sin atacante (fallo del modelo); un atacante puede inducirlo vía prompt injection, pero el daño principal es sistémico.

**Affected Users (9/10)**

Escala §2.4 (9): todos los usuarios que confían en respuestas “fundamentadas” del sistema.

**Discoverability (7/10)**

Escala §2.5 (7): usuarios o abogados revisores pueden detectar incoherencias; no siempre evidente sin expertise.

**Score DREAD:** (9 + 8 + 5 + 9 + 7) / 5 = **7,6**

**Nivel:** Alto

---

### STR-TB09-E

**Descripción**

Escalada de privilegios en la frontera lógica **cliente ↔ personal interno:** un cliente accede a corpus interno, documentos de otro cliente o funciones reservadas (auditoría, borradores avanzados).

**Damage (9/10)**

Escala §2.1 (9): acceso a contratos y dictámenes ajenos; violación de confidencialidad profesional.

**Reproducibility (8/10)**

Escala §2.2 (7–9): fallos de autorización (IDOR, roles mal mapeados) suelen ser reproducibles en pruebas sistemáticas.

**Exploitability (7/10)**

Escala §2.3 (7): manipulación de identificadores o endpoints con conocimiento moderado de API.

**Affected Users (7/10)**

Escala §2.4 (7): subconjunto grande de clientes en escenario multi-tenant.

**Discoverability (7/10)**

Escala §2.5 (7): detectable con pruebas de autorización estándar.

**Score DREAD:** (9 + 8 + 7 + 7 + 7) / 5 = **7,6**

**Nivel:** Alto

---

### STR-C07-I

**Descripción**

**Retrieval leakage:** la base vectorial devuelve fragmentos de documentos fuera del alcance del consultante (cross-tenant, cross-corpus o metadatos incorrectos).

**Damage (9/10)**

Escala §2.1 (9): filtración directa de contenido contractual y jurisprudencial indexado.

**Reproducibility (7/10)**

Escala §2.2 (7): depende de fallos de filtrado o de embeddings; reproducible una vez identificado el patrón de consulta.

**Exploitability (7/10)**

Escala §2.3 (7): consultas crafted o explotación de metadatos débiles.

**Affected Users (7/10)**

Escala §2.4 (7): usuarios cuyo aislamiento de corpus falla o todos si el índice es compartido.

**Discoverability (7/10)**

Escala §2.5 (7): pruebas de recuperación y comparación de resultados entre tenants.

**Score DREAD:** (9 + 7 + 7 + 7 + 7) / 5 = **7,4**

**Nivel:** Alto

---

### STR-C02-E

**Descripción**

Fallo de **autorización en la API** que permite acceder a recursos u operaciones de otro rol o tenant (complemento técnico de TB-09 en la capa de enforcement).

**Damage (8/10)**

Escala §2.1 (7–8): acceso no autorizado a documentos, plantillas o resultados de auditoría vía API.

**Reproducibility (8/10)**

Escala §2.2 (7–9): típico de debilidades BOLA/IDOR reproducibles.

**Exploitability (7/10)**

Escala §2.3 (7): requiere conocimiento de API y tokens válidos del atacante.

**Affected Users (7/10)**

Escala §2.4 (7): grupo amplio en despliegue multi-cliente.

**Discoverability (7/10)**

Escala §2.5 (7): pruebas de penetración en endpoints.

**Score DREAD:** (8 + 8 + 7 + 7 + 7) / 5 = **7,4**

**Nivel:** Alto

---

### STR-C05-T2

**Descripción**

**Data poisoning:** documento cargado (PDF/Word) con contenido diseñado para envenenar el índice o influir en recuperaciones futuras (instrucciones embebidas, metadatos engañosos).

**Damage (8/10)**

Escala §2.1 (7–8): corrupción del fundamento documental del RAG para múltiples consultas posteriores.

**Reproducibility (8/10)**

Escala §2.2 (7–9): tras la ingesta, el efecto persiste en el índice.

**Exploitability (7/10)**

Escala §2.3 (7): explota el canal legítimo de carga de contratos previsto en el escenario.

**Affected Users (8/10)**

Escala §2.4 (7–9): usuarios que consulten sobre el corpus envenenado.

**Discoverability (5/10)**

Escala §2.5 (5): el payload puede estar oculto en documentos binarios o texto no revisado.

**Score DREAD:** (8 + 8 + 7 + 8 + 5) / 5 = **7,2**

**Nivel:** Alto

---

### STR-TB10-I

**Descripción**

*(Condicional: despliegue con Pinecone u otro SaaS vectorial.)* El proveedor aloja embeddings y metadatos derivados de documentos confidenciales fuera del control operativo directo de la firma (TB-10).

**Damage (8/10)**

Escala §2.1 (7–8): exposición persistente de representaciones indexadas a tercero.

**Reproducibility (9/10)**

Escala §2.2 (9): inherente a la decisión de arquitectura si se elige SaaS.

**Exploitability (3/10)**

Escala §2.3 (3 = baja): no es un exploit puntual sino exposición estructural; score bajo en esta dimensión según §2.3.

**Affected Users (9/10)**

Escala §2.4 (9): todos los documentos indexados en el servicio gestionado.

**Discoverability (7/10)**

Escala §2.5 (7): visible en diseño y contratos con proveedor.

**Score DREAD:** (8 + 9 + 3 + 9 + 7) / 5 = **7,2**

**Nivel:** Alto

---

### STR-TB01-S

**Descripción**

**Phishing / interfaz falsa** que induce al usuario a autenticarse o introducir datos fuera del frontend institucional legítimo (TB-01).

**Damage (7/10)**

Escala §2.1 (7): robo de credenciales y de documentos cargados en sitio falso.

**Reproducibility (8/10)**

Escala §2.2 (7–9): técnicas de phishing ampliamente replicadas.

**Exploitability (7/10)**

Escala §2.3 (7): requiere campaña social pero con baja barrera técnica.

**Affected Users (5/10)**

Escala §2.4 (5 = pocos, >10 %): subconjunto de usuarios que caen en la estafa, no el 100 % simultáneo.

**Discoverability (8/10)**

Escala §2.5 (7–9): vectores conocidos y probados en concienciación de seguridad.

**Score DREAD:** (7 + 8 + 7 + 5 + 8) / 5 = **7,0**

**Nivel:** Alto

---

### STR-C12-T2

**Descripción**

**Indirect prompt injection:** instrucciones embebidas en documentos del corpus que el RAG recupera y el LLM interpreta como mandatos, alterando comportamiento sin modificar la consulta del usuario.

**Damage (8/10)**

Escala §2.1 (7–8): manipulación de respuestas y posible exfiltración vía contenido recuperado.

**Reproducibility (7/10)**

Escala §2.2 (7): requiere documento malicioso indexado y consulta que lo recupere.

**Exploitability (6/10)**

Escala §2.3 (5–7): depende de capacidad de cargar o contaminar corpus y de recuperación exitosa.

**Affected Users (8/10)**

Escala §2.4 (7–9): usuarios que consulten sobre documentos envenenados.

**Discoverability (5/10)**

Escala §2.5 (5): difícil de distinguir de contenido legítimo sin análisis forense del corpus.

**Score DREAD:** (8 + 7 + 6 + 8 + 5) / 5 = **6,8**

**Nivel:** Medio

---

### STR-C12-R

**Descripción**

Imposibilidad de **reconstruir qué fragmentos sustentaron una respuesta RAG** ante reclamación, auditoría o revisión profesional.

**Damage (7/10)**

Escala §2.1 (7): no siempre pérdida de datos inmediata, pero grave impacto en responsabilidad y defensa ante disputas.

**Reproducibility (10/10)**

Escala §2.2 (10): si el logging es insuficiente, el vacío es permanente en cada consulta afectada.

**Exploitability (3/10)**

Escala §2.3 (3): gap de diseño más que exploit activo; baja explotabilidad ofensiva directa.

**Affected Users (8/10)**

Escala §2.4 (7–9): todos los consumidores de respuestas RAG sin trazabilidad.

**Discoverability (5/10)**

Escala §2.5 (5): suele detectarse en auditorías internas, no por atacantes externos.

**Score DREAD:** (7 + 10 + 3 + 8 + 5) / 5 = **6,6**

**Nivel:** Medio

---

### STR-TB03-S

**Descripción**

Ataque al **flujo OAuth2 federado** (redirect manipulado, intercepción de código de autorización) en el cruce plataforma–IdP (TB-03).

**Damage (8/10)**

Escala §2.1 (7–8): toma de sesión o identidad federada.

**Reproducibility (7/10)**

Escala §2.2 (7): depende de configuración OAuth y de canal del usuario.

**Exploitability (6/10)**

Escala §2.3 (5–7): conocimiento de OAuth y condiciones de red del usuario.

**Affected Users (7/10)**

Escala §2.4 (7): usuarios que completen flujo comprometido.

**Discoverability (5/10)**

Escala §2.5 (5): requiere pruebas dirigidas del flujo de login.

**Score DREAD:** (8 + 7 + 6 + 7 + 5) / 5 = **6,6**

**Nivel:** Medio

---

### STR-TB09-I

**Descripción**

**Filtración horizontal en salidas:** respuesta RAG o borrador que mezcla información de otro asunto o cliente en la sesión activa (manifestación en frontera de roles).

**Damage (8/10)**

Escala §2.1 (7–8): divulgación en la respuesta visible al usuario.

**Reproducibility (7/10)**

Escala §2.2 (7): ligada a fallos de aislamiento de recuperación o generación.

**Exploitability (6/10)**

Escala §2.3 (5–7): puede ser espontánea o inducida.

**Affected Users (5/10)**

Escala §2.4 (5): típicamente pares de clientes/asuntos en condiciones de fallo.

**Discoverability (6/10)**

Escala §2.5 (5–7): el usuario o revisor legal puede detectar contenido ajeno.

**Score DREAD:** (8 + 7 + 6 + 5 + 6) / 5 = **6,4**

**Nivel:** Medio

---

### STR-C13-T

**Descripción**

Borrador contractual generado por **agente de IA** con cláusulas alteradas, omitidas o jurídicamente incoherentes respecto de plantillas y datos del caso.

**Damage (9/10)**

Escala §2.1 (9): impacto directo en instrumentos comerciales y responsabilidad de la firma.

**Reproducibility (8/10)**

Escala §2.2 (7–9): los LLM y agentes pueden producir errores de forma recurrente.

**Exploitability (4/10)**

Escala §2.3 (3–5): a menudo fallo del sistema sin atacante; explotación deliberada es menos trivial.

**Affected Users (5/10)**

Escala §2.4 (5): afecta al cliente/asunto del borrador en curso.

**Discoverability (6/10)**

Escala §2.5 (5–7): revisión legal puede detectar errores antes de firma.

**Score DREAD:** (9 + 8 + 4 + 5 + 6) / 5 = **6,4**

**Nivel:** Medio

---

### STR-C10-T

**Descripción**

Manipulación de **claims de rol** (cliente vs personal interno) en tokens OAuth2 que altera el mandato de acceso.

**Damage (8/10)**

Escala §2.1 (7–8): escalada de privilegios vía identidad.

**Reproducibility (7/10)**

Escala §2.2 (7): requiere fallo en validación de tokens o compromiso del IdP.

**Exploitability (5/10)**

Escala §2.3 (5): conocimiento de JWT/OAuth y condiciones específicas.

**Affected Users (7/10)**

Escala §2.4 (7): potencialmente todos los usuarios si el fallo es sistémico.

**Discoverability (4/10)**

Escala §2.5 (3–5): no visible sin inspección de tokens o código.

**Score DREAD:** (8 + 7 + 5 + 7 + 4) / 5 = **6,2**

**Nivel:** Medio

---

### STR-C14-T

**Descripción**

Resultados de **auditoría automatizada de cumplimiento** falsos o manipulados (hallazgos omitidos o inventados).

**Damage (8/10)**

Escala §2.1 (7–8): decisiones de cumplimiento basadas en dictamen erróneo.

**Reproducibility (7/10)**

Escala §2.2 (7): errores o manipulación del módulo de auditoría pueden repetirse.

**Exploitability (5/10)**

Escala §2.3 (5): puede ser fallo del modelo o ataque a entradas del proceso.

**Affected Users (6/10)**

Escala §2.4 (5–7): clientes o asuntos sometidos a auditoría.

**Discoverability (5/10)**

Escala §2.5 (5): requiere revisión experta del dictamen frente al documento fuente.

**Score DREAD:** (8 + 7 + 5 + 6 + 5) / 5 = **6,2**

**Nivel:** Medio

---

## Resumen ejecutivo

### Cantidad evaluada

Se evaluaron **16** amenazas de **63** identificadas en [`03-analisis-stride.md`](03-analisis-stride.md).

### Distribución por severidad (según rangos DREAD §3.2)

| Nivel | Cantidad | % |
|-------|----------|---|
| Crítico (9,0 – 10,0) | 0 | 0 % |
| Alto (7,0 – 8,9) | 9 | 56 % |
| Medio (5,0 – 6,9) | 7 | 44 % |
| Bajo (3,0 – 4,9) | 0 | 0 % |
| Informativo (0,0 – 2,9) | 0 | 0 % |

### Top 10 amenazas por puntuación DREAD

| Posición | ID STRIDE | Score | Nivel |
|----------|-----------|-------|-------|
| 1 | STR-C12-T | 8,0 | Alto |
| 2 | STR-TB07-I | 7,8 | Alto |
| 3 | STR-C12-T3 | 7,6 | Alto |
| 4 | STR-TB09-E | 7,6 | Alto |
| 5 | STR-C07-I | 7,4 | Alto |
| 6 | STR-C02-E | 7,4 | Alto |
| 7 | STR-C05-T2 | 7,2 | Alto |
| 8 | STR-TB10-I | 7,2 | Alto |
| 9 | STR-TB01-S | 7,0 | Alto |
| 10 | STR-C12-T2 | 6,8 | Medio |

### Análisis de los riesgos más importantes para la plataforma

1. **Vectores de IA generativa y RAG (STR-C12-T, STR-C12-T2, STR-C12-T3, STR-C05-T2, STR-C07-I):** concentran el mayor daño potencial en un escenario cuyo valor es el asesoramiento legal “fundamentado”. La **prompt injection directa** obtiene la puntuación más alta (8,0), coherente con explotabilidad y detectabilidad altas en el catálogo STRIDE. La **alucinación con impacto legal** y el **retrieval leakage** comparten daño muy alto (9) por afectar confidencialidad e integridad del consejo automatizado.

2. **Salida de perímetro y subprocesadores (STR-TB07-I, STR-TB10-I):** el escenario contrasta LLM local vs comercial y Chroma vs Pinecone; el tránsito de contexto confidencial hacia **LLM en la nube** (7,8) y el alojamiento de **embeddings en SaaS** (7,2) son riesgos estructurales de confidencialidad, con reproducibilidad alta cuando esas opciones de despliegue se adopten.

3. **Aislamiento cliente / interno y multi-tenant (STR-TB09-E, STR-C02-E, STR-C07-I):** en un ecosistema orientado a clientes y personal interno, los fallos de **autorización** y la **fuga entre tenants** tienen daño muy alto (8–9) y puntajes en rango **Alto**, alineados con el inventario de activos de contratos confidenciales.

4. **Integridad de entregables legales automatizados (STR-C13-T, STR-C14-T):** borradores y auditorías automatizadas puntúan en rango **Medio** (6,2–6,4) por menor explotabilidad ofensiva directa, pero **daño alto** (8–9) si se adoptan sin revisión humana; relevantes para el plan de mitigación (entregable 06).

5. **Identidad y usuario (STR-TB01-S, STR-TB03-S, STR-C10-T):** permanecen en **Alto** o **Medio**; complementan pero no superan en score a los riesgos RAG y de confidencialidad del corpus, coherente con la naturaleza del escenario.

**Conclusión:** la priorización DREAD sitúa en la franja **Alta** principalmente amenazas de **confidencialidad del corpus legal** (fugas, nube, SaaS vectorial) e **integridad del RAG** (inyección de prompts, envenenamiento de datos, respuestas no fundamentadas). Ninguna amenaza evaluada alcanza el rango **Crítico** (≥ 9,0) con la calibración de [`DREAD.md`](../references/DREAD.md), lo que refleja ausencia de destrucción total del sistema pero sí impacto grave en activos de información jurídica.

---

## Relación con otros entregables

| Documento | Relación |
|-----------|----------|
| [`03-analisis-stride.md`](03-analisis-stride.md) | Origen de los IDs `STR-*` evaluados. |
| [`05-mapa-attack.md`](05-mapa-attack.md) | Mapeo ATT&CK de las 16 amenazas evaluadas (15 con técnica primaria; STR-C12-R sin equivalente ofensivo). |
| [`06-plan-mitigacion.md`](06-plan-mitigacion.md) | **TODO:** tratamiento según nivel DREAD. |
| [`07-riesgos-residuales.md`](07-riesgos-residuales.md) | **TODO:** riesgo tras mitigación. |

---

## Trazabilidad

- Referencias metodológicas: [`../references/DREAD.md`](../references/DREAD.md), [`../references/STRIDE.md`](../references/STRIDE.md)
- Análisis STRIDE: [`03-analisis-stride.md`](03-analisis-stride.md)
- Registro de prompt (IA): [`../prompts/08-prompt-priorizacion-dread.md`](../prompts/08-prompt-priorizacion-dread.md)
