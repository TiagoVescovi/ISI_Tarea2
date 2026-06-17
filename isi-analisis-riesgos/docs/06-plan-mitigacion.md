# Plan de mitigación

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`01-inventario-activos.md`](01-inventario-activos.md), [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md), [`03-analisis-stride.md`](03-analisis-stride.md), [`04-priorizacion-dread.md`](04-priorizacion-dread.md), [`05-mapa-attack.md`](05-mapa-attack.md), [`../references/STRIDE.md`](../references/STRIDE.md), [`../references/DREAD.md`](../references/DREAD.md), [`../references/analisis_amenazas.md`](../references/analisis_amenazas.md), [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK).

**Alcance:** tratamiento de las **16** amenazas evaluadas en DREAD. **No** incluye la cuantificación detallada del riesgo residual (ver [`07-riesgos-residuales.md`](07-riesgos-residuales.md)).

---

## 1. Introducción

### Propósito

Documentar medidas de **tratamiento del riesgo** — mitigar, transferir, evitar y aceptar — para las amenazas priorizadas, alineadas al análisis STRIDE, la urgencia DREAD y el mapeo MITRE ATT&CK.

### Tipos de tratamiento

| Tipo | Uso en este plan |
|------|------------------|
| **Mitigar** | Controles técnicos, operativos o de proceso documentados en `references/`. |
| **Evitar** | Decisiones de arquitectura del escenario (p. ej. LLM local, Chroma on-prem) que eliminan o reducen el cruce de frontera. |
| **Transferir** | Tratamiento complementario ante dependencia de terceros (LLM cloud, SaaS vectorial, IdP); **sin** detallar cláusulas contractuales no presentes en `references/`. |
| **Aceptar** | Riesgo residual identificado aquí; desarrollo en [`07-riesgos-residuales.md`](07-riesgos-residuales.md). |

### Urgencia según DREAD

Según [`../references/DREAD.md`](../references/DREAD.md) §3.2: amenazas **Altas** (7,0–8,9) → remediación **urgente**; **Medias** (5,0–6,9) → remediación en **siguiente sprint**.

### Limitaciones

- Controles citados **solo** si aparecen en `references/` (STRIDE, `analisis_amenazas.md`, `MITRE_ATTCK`).
- **STR-C02-E** y **STR-TB09-E:** limitación **T1078** documentada en [`05-mapa-attack.md`](05-mapa-attack.md) §1.
- **STR-TB10-I:** condicional según TB-10 en [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md).

### Cadena de trazabilidad

```text
Amenaza STRIDE → Score DREAD → Técnica ATT&CK → Controles → Tratamiento → Riesgo residual (07)
```

---

## 2. Marco de controles utilizado

| Control / contramedida | Tipo | Fuente en `references/` |
|------------------------|------|-------------------------|
| MFA | Preventivo | STRIDE §3.1 S; `analisis_amenazas.md` §5.3; caso ransomware §9 |
| RBAC / mínimo privilegio / gestión de privilegios (M1026) | Preventivo | STRIDE §3.4 I, §3.6 E; MITRE_ATTCK perfil FIN7 |
| Validación de entrada / sanitización | Preventivo | STRIDE §3.2 T; `analisis_amenazas.md` Kill Chain Weaponize |
| Hardening | Preventivo | STRIDE §3.6 E |
| TLS / cifrado | Preventivo | STRIDE §3.4 I |
| Minimización de datos | Preventivo | STRIDE §3.4 I |
| Tokens con expiración; logging de autenticación | Preventivo / Detectivo | STRIDE §3.1 S |
| Integridad (hashing, firmas); auditoría de cambios | Preventivo | STRIDE §3.2 T |
| Logging inmutable; timestamps (NTP); segregación de funciones / múltiples aprobaciones | Preventivo / Detectivo | STRIDE §3.3 R |
| Rate limiting / throttling | Preventivo | STRIDE §3.5 D; APIs STRIDE §4.2 |
| Segmentación de red (M0930) | Preventivo | MITRE_ATTCK §3.1; `analisis_amenazas.md` Kill Chain C&C |
| Anti-phishing (M1049); filtrado web (M1021); capacitación (M1017) | Preventivo | MITRE_ATTCK §3.1; `analisis_amenazas.md` §5.3 |
| Control de aplicaciones / whitelisting (M1038) | Preventivo | MITRE_ATTCK §3.1; `analisis_amenazas.md` Installation |
| WAF (complementario a validación de entrada) | Preventivo | `analisis_amenazas.md` Kill Chain Weaponize |
| DLP; monitoreo de egress | Detectivo / Preventivo | `analisis_amenazas.md` Actions on Objectives; caso ransomware §9 |
| SIEM; reglas de correlación | Detectivo | `analisis_amenazas.md` §1.3, §7.3; MITRE purple team |
| EDR / HIDS | Detectivo | `analisis_amenazas.md` Exploitation / Installation |
| IDS/IPS; monitoreo DNS | Detectivo | `analisis_amenazas.md` Command & Control |
| Backups inmutables (WORM) | Correctivo | `analisis_amenazas.md` caso ransomware §9 |
| Enrutamiento a LLM local; Chroma on-prem | Evitar | Escenario en `01`, `02` TB-07/TB-08, TB-10 |

---

## 3. Resumen de amenazas priorizadas

| ID STRIDE | Cat. | DREAD | Nivel | TB principal | ATT&CK (prim.) | Tratamiento principal |
|-----------|------|-------|-------|--------------|----------------|----------------------|
| STR-C12-T | T | 8,0 | Alto | TB-02, TB-06, TB-07/08 | T1059.006 | Mitigar |
| STR-TB07-I | I | 7,8 | Alto | TB-07 | T1567.002 | Evitar + Mitigar |
| STR-C12-T3 | T | 7,6 | Alto | TB-06, TB-07/08 | T1565 | Mitigar + Aceptar* |
| STR-TB09-E | E | 7,6 | Alto | TB-09 | T1068 | Mitigar |
| STR-C07-I | I | 7,4 | Alto | TB-06 | T1005 | Mitigar |
| STR-C02-E | E | 7,4 | Alto | TB-02, TB-09 | T1068 | Mitigar |
| STR-C05-T2 | T | 7,2 | Alto | TB-04, TB-05, TB-06 | T1080 | Mitigar |
| STR-TB10-I | I | 7,2 | Alto | TB-10 † | T1530 | Evitar o Mitigar + Transferir |
| STR-TB01-S | S | 7,0 | Alto | TB-01 | T1566.002 | Mitigar |
| STR-C12-T2 | T | 6,8 | Medio | TB-05, TB-06, TB-07/08 | T1080 | Mitigar |
| STR-C12-R | R | 6,6 | Medio | — | — | Mitigar |
| STR-TB03-S | S | 6,6 | Medio | TB-03 | T1556 | Mitigar + Transferir |
| STR-TB09-I | I | 6,4 | Medio | TB-09 | T1005 | Mitigar |
| STR-C13-T | T | 6,4 | Medio | TB-07/08, TB-09 | T1565 | Mitigar + Aceptar* |
| STR-C10-T | T | 6,2 | Medio | TB-03, TB-09 | T1556 | Mitigar |
| STR-C14-T | T | 6,2 | Medio | TB-07/08, TB-09 | T1565 | Mitigar + Aceptar* |

\* Riesgo residual → [`07-riesgos-residuales.md`](07-riesgos-residuales.md).  
† Condicional: ver §4.8 y §5.8.

---

## 4. Controles transversales

| Bloque | Controles | Amenazas cubiertas |
|--------|-----------|-------------------|
| **T1 — Identidad y acceso** | MFA; M1026; RBAC; tokens con expiración; logging auth | TB-01-S, TB-03-S, C10-T, C02-E, TB-09-E |
| **T2 — Autorización multi-tenant** | RBAC; validación permisos (STRIDE E); aislamiento lógico en índice | C02-E, TB-09-E, C07-I, TB-09-I |
| **T3 — Perímetro usuario** | M1049, M1021, M1017; TLS | TB-01-S, TB-03-S |
| **T4 — RAG / orquestador** | Validación entrada; M1038; hardening; logging | C12-T, C12-T2, C05-T2 |
| **T5 — Salida a terceros cloud** | Evitar LLM local / Chroma on-prem; TLS; minimización; DLP; M0930 | TB-07-I, TB-10-I |
| **T6 — Ingesta documental** | Validación entrada; integridad; auditoría ETL; WAF‡ en carga | C05-T2, C12-T2 |
| **T7 — Salidas legales automatizadas** | Segregación de funciones; revisión humana; logging fundamento | C12-T3, C13-T, C14-T, C12-R |
| **T8 — Detección** | SIEM; EDR; IDS/IPS; monitoreo DNS | Transversal |
| **T9 — Continuidad** | Backups WORM | Complementario |

‡ WAF según `analisis_amenazas.md` Weaponize, **complemento** a validación de entrada en API/carga.

---

## 5. Transferencia de riesgo ante terceros

Tratamiento **complementario** cuando el escenario delega funciones en proveedores externos (`01`, `02`):

| Tercero | Trust boundary | Amenazas relacionadas | Estrategia de transferencia |
|---------|----------------|----------------------|----------------------------|
| **Proveedor LLM comercial** (OpenAI / Anthropic) | TB-07 | STR-TB07-I | Transferir parte del riesgo operativo y de confidencialidad al proveedor mediante relación de subprocesamiento (marco del escenario). Complementar con **evitar** (LLM local) y **mitigar** (minimización, DLP, TLS). Sin detallar cláusulas no presentes en `references/`. |
| **Proveedor SaaS vectorial** (p. ej. Pinecone) | TB-10 | STR-TB10-I | Transferir riesgo de almacenamiento en infraestructura ajena. Alternativa preferente: **evitar** con Chroma on-prem (`02` TB-10). |
| **Proveedor IdP federado** (OAuth2) | TB-03 | STR-TB03-S, STR-C10-T | Transferir riesgo de autenticación primaria al IdP. Complementar con **mitigar** (MFA, hardening OAuth, validación tokens). |

---

## 6. Plan de mitigación por amenaza

### 6.1 STR-C12-T — Prompt injection directa (DREAD 8,0 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Instrucciones maliciosas en la consulta que alteran políticas, exfiltran contexto o manipulan salida (`03` A.12). |
| **Activos** | Consultas en lenguaje natural; Respuestas RAG; Representación indexada |
| **Trust boundaries** | TB-02, TB-06, TB-07/08 |
| **ATT&CK** | T1059.006 (prim.); T1565 (sec.) |
| **Tratamiento** | **Mitigar** |
| **Controles** | **Validación de entrada** en API y orquestador (STRIDE T); **M1038** whitelisting de herramientas del orquestador (MITRE); **hardening** del entorno Python (STRIDE E); **logging** de consultas y respuestas (STRIDE R/S); **SIEM/EDR** sobre ejecución Python anómala (`analisis_amenazas.md`; MITRE purple team) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | Bypass de validaciones → ver [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.2 STR-TB07-I — Exposición de contexto hacia LLM comercial (DREAD 7,8 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Tránsito de consultas y contexto recuperado al proveedor cloud (`03` TB-07). |
| **Activos** | Representación indexada; Consultas; Documentos contractuales; Documentación legal; Respuestas RAG |
| **Trust boundaries** | **TB-07** |
| **ATT&CK** | T1567.002 (prim.); T1005 (sec.) |
| **Tratamiento** | **Evitar** + **Mitigar** + **Transferir** |
| **Controles** | **Evitar:** enrutar datos altamente sensibles al **LLM local** (escenario; TB-08). **Mitigar:** **TLS** (STRIDE I); **minimización de datos** en prompts (STRIDE I); **DLP** / monitoreo egress (`analisis_amenazas.md`); **segmentación** del canal cloud (M0930; `analisis_amenazas.md`). **Transferir:** riesgo al proveedor LLM (§5). |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | Tráfico no sensible residual hacia cloud → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.3 STR-C12-T3 — Alucinación con impacto legal (DREAD 7,6 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Respuesta presentada como fundamentada con contenido jurídico incorrecto o inventado (`03` A.12). |
| **Activos** | Respuestas RAG; Documentación legal; Consultas |
| **Trust boundaries** | TB-06, TB-07/08 |
| **ATT&CK** | T1565 |
| **Tratamiento** | **Mitigar** + **Aceptar** (residual) |
| **Controles** | **Segregación de funciones** y **revisión humana obligatoria** por personal legal antes de adoptar la respuesta (STRIDE R §3.3); **logging** de fragmentos recuperados y respuesta (STRIDE R); validación de coherencia con fuentes (STRIDE T integridad) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | Error del modelo tras controles → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.4 STR-TB09-E — Cliente accede a corpus o funciones ajenas (DREAD 7,6 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Cliente accede a corpus interno, otro tenant o funciones de personal (`03` TB-09). |
| **Activos** | Documentación legal; Documentos contractuales; Resultados auditoría; Plantillas; Procesos IA |
| **Trust boundaries** | **TB-09** |
| **ATT&CK** | T1068 |
| **Controles** | **RBAC** y **mínimo privilegio** (STRIDE E); **M1026** gestión de cuentas privilegiadas (MITRE); **validación de permisos** en cada operación API (STRIDE E); **MFA** en cuentas con privilegios ampliados (STRIDE S; `analisis_amenazas.md`) |
| **Tratamiento** | **Mitigar** |
| **Tipo** | Preventivo |
| **Riesgo residual** | Fallos lógicos no detectados → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.5 STR-C07-I — Retrieval leakage (DREAD 7,4 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Recuperación de fragmentos fuera del alcance del consultante (`03` A.7). |
| **Activos** | Representación indexada; Documentos contractuales; Documentación legal; Respuestas RAG |
| **Trust boundaries** | **TB-06** |
| **ATT&CK** | T1005 |
| **Tratamiento** | **Mitigar** |
| **Controles** | **RBAC** y filtros por tenant/rol en recuperación (STRIDE I/E); **segmentación lógica** de namespaces en base vectorial (M0930 / `analisis_amenazas.md` segmentación); **logging** de consultas y fragmentos devueltos (STRIDE R) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | Fugas por similitud semántica → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.6 STR-C02-E — Fallo de autorización en API (DREAD 7,4 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Acceso cross-tenant o cross-rol por fallo de autorización (`03` A.2). |
| **Activos** | Identidades federadas; Documentos; Documentación legal; Plantillas; Resultados auditoría |
| **Trust boundaries** | TB-02, **TB-09** |
| **ATT&CK** | T1068 |
| **Tratamiento** | **Mitigar** |
| **Controles** | **Autorización en servidor** en cada endpoint (STRIDE API §4.2 E); **RBAC**; **M1026**; validación de ownership de recursos (STRIDE E); **rate limiting** en API (STRIDE D §4.2) |
| **Tipo** | Preventivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.7 STR-C05-T2 — Data poisoning en ingesta (DREAD 7,2 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Documento cargado diseñado para envenenar el índice (`03` A.5). |
| **Activos** | Documentos contractuales; Representación indexada; Proceso ingestión |
| **Trust boundaries** | TB-04, TB-05, TB-06 |
| **ATT&CK** | T1080 (prim.); T1565 (sec.) |
| **Tratamiento** | **Mitigar** |
| **Controles** | **Validación de entrada** en carga e ingesta (STRIDE T; `analisis_amenazas.md`); **WAF** complementario en API de carga (`analisis_amenazas.md` Weaponize); **integridad** (hash) de documentos (STRIDE T); **auditoría de cambios** en pipeline ETL (STRIDE T) |
| **Tipo** | Preventivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.8 STR-TB10-I — Exposición de embeddings en SaaS vectorial (DREAD 7,2 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Proveedor SaaS accede a embeddings y metadatos (`03` TB-10). |
| **Activos** | Representación indexada; Base vectorial (Pinecone); Documentos (derivados) |
| **Trust boundaries** | **TB-10** (condicional) |
| **ATT&CK** | T1530 |
| **Tratamiento** | Depende del despliegue (`02` TB-10) |

**Rama A — Chroma on-prem (TB-10 no aplica):**

| Tratamiento | Controles |
|-------------|-----------|
| **Evitar** | Mantener índice bajo control de la firma; no cruzar TB-10 hacia SaaS externo. |

**Rama B — Pinecone / SaaS vectorial:**

| Tratamiento | Controles |
|-------------|-----------|
| **Mitigar** | **TLS**; **cifrado** en reposo donde aplique (STRIDE I); **minimización** de metadatos sensibles en índice (STRIDE I) |
| **Transferir** | Riesgo de almacenamiento al proveedor SaaS (§5) |
| **Aceptar** | Residual de subprocesador → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

**Tipo:** Preventivo (+ Transferir en rama B).

---

### 6.9 STR-TB01-S — Phishing / interfaz falsa (DREAD 7,0 — Alto)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Usuario autentica en UI falsa fuera del dominio de confianza (`03` TB-01). |
| **Activos** | Identidades federadas; Frontend Next.js |
| **Trust boundaries** | **TB-01** |
| **ATT&CK** | T1566.002 (prim.); T1036 (sec.) |
| **Tratamiento** | **Mitigar** |
| **Controles** | **M1049** anti-phishing; **M1021** filtrado web; **M1017** capacitación (`MITRE_ATTCK` §3.1; `analisis_amenazas.md` §5.3); **MFA** (STRIDE S); **TLS** / certificados (STRIDE S PKI) |
| **Tipo** | Preventivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.10 STR-C12-T2 — Indirect prompt injection (DREAD 6,8 — Medio)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Instrucciones embebidas en documentos del corpus recuperados por RAG (`03` A.12). |
| **Activos** | Documentos contractuales; Documentación legal; Representación indexada; Respuestas RAG |
| **Trust boundaries** | TB-05, TB-06, TB-07/08 |
| **ATT&CK** | T1080 (prim.); T1059.006 (sec.) |
| **Tratamiento** | **Mitigar** |
| **Controles** | Controles de **ingesta** (validación entrada, integridad) compartidos con STR-C05-T2; **M1038** en orquestador; **validación de entrada** en consulta (STRIDE T); **logging** (STRIDE R) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.11 STR-C12-R — Falta de trazabilidad del fundamento RAG (DREAD 6,6 — Medio)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Imposibilidad de reconstruir fragmentos que sustentaron una respuesta (`03` A.12). |
| **Activos** | Respuestas RAG; Representación indexada; Consultas |
| **ATT&CK** | Sin técnica ofensiva (`05` §5) |
| **Tratamiento** | **Mitigar** |
| **Controles** | **Logging inmutable** con identificador de fragmentos recuperados y versión de índice (STRIDE R); **timestamps** NTP (STRIDE R); correlación en **SIEM** (`analisis_amenazas.md` §7.3) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.12 STR-TB03-S — Ataque al flujo OAuth2 (DREAD 6,6 — Medio)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Redirect manipulado, intercepción de código o cliente mal registrado (`03` TB-03). |
| **Activos** | Identidades federadas; Proveedor IdP |
| **Trust boundaries** | **TB-03** |
| **ATT&CK** | T1556 (prim.); T1566.002 (sec.) |
| **Tratamiento** | **Mitigar** + **Transferir** |
| **Controles** | **MFA**; **hardening** OAuth (STRIDE E); **tokens con expiración** (STRIDE S); **logging** de autenticación (STRIDE S); **M1021** si el vector es enlace malicioso; **Transferir** riesgo de autenticación al IdP (§5) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.13 STR-TB09-I — Filtración horizontal en salidas (DREAD 6,4 — Medio)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Respuesta o borrador mezcla datos de otro asunto/cliente (`03` TB-09). |
| **Activos** | Respuestas RAG; Borradores; Representación indexada |
| **Trust boundaries** | **TB-09** |
| **ATT&CK** | T1005 |
| **Tratamiento** | **Mitigar** |
| **Controles** | Mismos que STR-C07-I (aislamiento recuperación); **RBAC** por sesión/asunto; **logging** de salidas (STRIDE R) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.14 STR-C13-T — Borrador contractual erróneo (DREAD 6,4 — Medio)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Borrador con cláusulas alteradas o incoherentes (`03` A.13). |
| **Activos** | Borradores preliminares; Plantillas; Documentos contractuales |
| **Trust boundaries** | TB-07/08, TB-09 |
| **ATT&CK** | T1565 |
| **Tratamiento** | **Mitigar** + **Aceptar** (residual) |
| **Controles** | **Segregación de funciones**; **revisión humana obligatoria** antes de uso o envío al cliente (STRIDE R §3.3); **integridad** respecto de plantillas (STRIDE T); **logging** del borrador generado (STRIDE R) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | Error residual del agente → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.15 STR-C10-T — Manipulación de claims en tokens (DREAD 6,2 — Medio)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Manipulación de claims de rol en tokens OAuth2 (`03` A.10). |
| **Activos** | Identidades federadas; Servicios API |
| **Trust boundaries** | TB-03, TB-09 |
| **ATT&CK** | T1556 |
| **Tratamiento** | **Mitigar** |
| **Controles** | Validación de firma y claims en API (STRIDE S/T); **MFA**; **M1026**; **logging** de autenticación (STRIDE S) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

### 6.16 STR-C14-T — Auditoría de cumplimiento manipulada (DREAD 6,2 — Medio)

| Campo | Contenido |
|-------|-----------|
| **Descripción** | Hallazgos falsos u omitidos en auditoría automatizada (`03` A.14). |
| **Activos** | Resultados auditoría; Documentos contractuales |
| **Trust boundaries** | TB-07/08, TB-09 |
| **ATT&CK** | T1565 |
| **Tratamiento** | **Mitigar** + **Aceptar** (residual) |
| **Controles** | **Revisión humana experta** del dictamen frente al documento fuente (STRIDE R segregación/múltiples aprobaciones); **logging** de criterios y fragmentos usados (STRIDE R); **integridad** de salidas (STRIDE T) |
| **Tipo** | Preventivo + Detectivo |
| **Riesgo residual** | → [`07-riesgos-residuales.md`](07-riesgos-residuales.md) |

---

## 7. Tabla resumen de mitigaciones

| ID | DREAD | Tratamiento | Controles principales | Tipo | ATT&CK | Componente |
|----|-------|-------------|----------------------|------|--------|------------|
| STR-C12-T | 8,0 Alto | Mitigar | Validación entrada; M1038; hardening; logging; SIEM/EDR | Prev+Det | T1059.006 | Orquestador / RAG |
| STR-TB07-I | 7,8 Alto | Evitar+Mitigar+Transferir | LLM local; TLS; minimización; DLP; segmentación | Prev+Det | T1567.002 | TB-07 |
| STR-C12-T3 | 7,6 Alto | Mitigar+Aceptar* | Revisión humana; segregación funciones; logging | Prev+Det | T1565 | Proceso RAG |
| STR-TB09-E | 7,6 Alto | Mitigar | RBAC; M1026; MFA privilegios | Prev | T1068 | API / TB-09 |
| STR-C07-I | 7,4 Alto | Mitigar | RBAC tenant; segmentación lógica índice; logging | Prev+Det | T1005 | Base vectorial |
| STR-C02-E | 7,4 Alto | Mitigar | Authz API; RBAC; M1026; rate limiting | Prev | T1068 | API |
| STR-C05-T2 | 7,2 Alto | Mitigar | Validación ingesta; WAF‡; integridad; auditoría ETL | Prev | T1080 | Pipeline ETL |
| STR-TB10-I | 7,2 Alto | Evitar o Mit+Trans | Chroma on-prem **o** cifrado+minimización+transferir SaaS | Prev | T1530 | TB-10 |
| STR-TB01-S | 7,0 Alto | Mitigar | M1049; M1021; M1017; MFA; TLS | Prev | T1566.002 | Frontend / TB-01 |
| STR-C12-T2 | 6,8 Medio | Mitigar | Ingesta+validación; M1038; logging | Prev+Det | T1080 | RAG / ingesta |
| STR-C12-R | 6,6 Medio | Mitigar | Logging inmutable; NTP; SIEM | Prev+Det | — | Proceso RAG |
| STR-TB03-S | 6,6 Medio | Mitigar+Transferir | MFA; hardening OAuth; M1021; logging | Prev+Det | T1556 | TB-03 / IdP |
| STR-TB09-I | 6,4 Medio | Mitigar | Aislamiento recuperación; RBAC sesión; logging | Prev+Det | T1005 | TB-09 |
| STR-C13-T | 6,4 Medio | Mitigar+Aceptar* | Revisión humana; segregación; logging | Prev+Det | T1565 | Agentes borradores |
| STR-C10-T | 6,2 Medio | Mitigar | Validación tokens; MFA; M1026 | Prev+Det | T1556 | OAuth / API |
| STR-C14-T | 6,2 Medio | Mitigar+Aceptar* | Revisión experta; logging criterios | Prev+Det | T1565 | Auditoría IA |

\* Residual detallado en [`07-riesgos-residuales.md`](07-riesgos-residuales.md).

---

## 8. Priorización de implementación

Según DREAD §3.2 y dependencias (§9).

| Fase | Plazo sugerido | Foco | Amenazas / bloques |
|------|----------------|------|-------------------|
| **1 — Urgente** | 0–30 días | Identidad; RAG crítico; política datos sensibles | STR-C12-T (T4); STR-TB07-I (T5 evitar+mitigar); T1 |
| **2 — Urgente** | 15–45 días | Autorización multi-tenant; ingesta | STR-TB09-E, C02-E, C07-I (T2); STR-C05-T2 (T6) |
| **3 — Urgente** | 30–60 días | Perímetro usuario; vector DB; salidas legales | STR-TB01-S (T3); STR-TB10-I (rama arquitectónica); STR-C12-T3 (T7) |
| **4 — Siguiente sprint** | 45–90 días | Medias; detección; trazabilidad | C12-T2, C12-R, TB-03-S, TB-09-I, C13-T, C10-T, C14-T; T8 |

*Plazos inspirados en la tabla de acciones del caso ransomware (`analisis_amenazas.md` §9.4), adaptados al escenario legal/RAG.*

---

## 9. Dependencias entre controles

```text
MFA + hardening OAuth (TB-03) ──► validación tokens (C10-T) ──► RBAC API (C02-E, TB-09-E)
        │
Logging autenticación ──────────┼──► SIEM
        │
Registro fragmentos RAG (C12-R) ┼──► trazabilidad salidas (C12-T3, C13-T, C14-T)
        │
Validación ingesta (C05-T2) ────┼──► reduce T1080 (C12-T2)
        │
RBAC + namespaces vectoriales ──┼──► C07-I, TB-09-I
        │
Política LLM local ─────────────┼──► reduce TB-07-I (antes de DLP fino)
        │
Segmentación (M0930) ───────────┼──► canal cloud / DLP egress
```

| Control previo | Control dependiente |
|----------------|---------------------|
| MFA / OAuth sólido | RBAC efectivo en API |
| Logging consultas/fragmentos | SIEM; STR-C12-R; revisión legal |
| Validación ingesta | Mitigación C05-T2 y C12-T2 |
| Política enrutamiento LLM | Mitigación TB-07-I |
| RBAC en índice | C07-I, TB-09-I |

---

## 10. Agrupaciones

### 10.1 Por componente afectado

| Componente | Amenazas |
|------------|----------|
| Proceso consulta RAG | C12-T, C12-T2, C12-T3, C12-R |
| Orquestador Python | C12-T, C12-T2 |
| Servicios API | C02-E, C10-T |
| IdP / OAuth | TB-03-S, C10-T |
| Base vectorial / TB-06 | C07-I, TB-10-I |
| Pipeline ETL / ingesta | C05-T2, C12-T2 |
| Frontend / usuario | TB-01-S |
| TB lógica cliente–interno | TB-09-E, TB-09-I |
| LLM comercial (TB-07) | TB-07-I |
| Agentes borradores | C13-T |
| Auditoría automatizada | C14-T |

### 10.2 Por tipo de control

| Tipo | Controles | Amenazas principales |
|------|-----------|---------------------|
| **Preventivo** | MFA, RBAC, validación entrada, M1038, M1021, M1049, TLS, minimización, rate limiting, revisión humana, evitar cloud | Mayoría S, E, T, I |
| **Detectivo** | SIEM, EDR, IDS/IPS, DLP, monitoreo DNS, logging | TB-01-S, TB-03-S, TB-07-I, C12-T, C12-R |
| **Correctivo** | Backups WORM | Continuidad (complementario) |

---

## 11. Recomendaciones operativas

1. **SIEM:** implementar reglas de correlación según `analisis_amenazas.md` §7.3 (login anómalo, exfil volumen, actividad sospechosa en intérpretes).
2. **Capacitación:** programa periódico anti-phishing (M1017; vectores §5.3).
3. **Monitoreo:** integraciones OAuth (TB-03), APIs de carga documental y egress hacia proveedores LLM (TB-07).
4. **Revisión humana:** flujo obligatorio para respuestas RAG de impacto legal, borradores y dictámenes de auditoría (STRIDE R).
5. **Arquitectura:** documentar en operaciones qué rama TB-10 (Chroma vs Pinecone) y política TB-07/TB-08 están activas.

---

## 12. Conclusiones

El plan prioriza **vectores RAG e IA** (STR-C12-T, exposición cloud, integridad de salidas legales) y **aislamiento multi-tenant** (autorización API y recuperación vectorial), coherente con la priorización DREAD. Los controles provienen exclusivamente de `references/`. La **transferencia** ante terceros es complementaria y no sustituye mitigación ni evitación arquitectónica. Los **riesgos residuales** identificados se desarrollan en [`07-riesgos-residuales.md`](07-riesgos-residuales.md).

---

## Relación con otros entregables

| Documento | Relación |
|-----------|----------|
| [`03-analisis-stride.md`](03-analisis-stride.md) | Origen de amenazas y categorías. |
| [`04-priorizacion-dread.md`](04-priorizacion-dread.md) | Urgencia y selección de 16 amenazas. |
| [`05-mapa-attack.md`](05-mapa-attack.md) | Técnicas ATT&CK por amenaza. |
| [`07-riesgos-residuales.md`](07-riesgos-residuales.md) | Detalle de riesgos aceptados y residuales. |

---

## Trazabilidad

- Inventario: [`01-inventario-activos.md`](01-inventario-activos.md)
- Trust boundaries: [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md)
- Referencias: [`../references/STRIDE.md`](../references/STRIDE.md), [`../references/DREAD.md`](../references/DREAD.md), [`../references/analisis_amenazas.md`](../references/analisis_amenazas.md), [`../references/MITRE_ATTCK`](../references/MITRE_ATTCK)
- Registro de prompts (IA): [`../prompts/10-prompt-plan-mitigacion.md`](../prompts/10-prompt-plan-mitigacion.md), [`../prompts/12-prompt-plan-mitigacion-analisis-previo.md`](../prompts/12-prompt-plan-mitigacion-analisis-previo.md), [`../prompts/13-prompt-plan-mitigacion-redaccion.md`](../prompts/13-prompt-plan-mitigacion-redaccion.md)
