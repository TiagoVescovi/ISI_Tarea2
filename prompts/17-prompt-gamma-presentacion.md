# Prompt Gamma — presentación completa (10 diapositivas)

**Fecha:** 2026-06-18  
**Uso:** copiar el bloque «Prompt para Gamma» en [gamma.app](https://gamma.app) para generar la presentación de una vez.

**Después de generar:** subir `diagrams/arquitectura.png` en la slide 3 y, si aplica, `diagrams/attack-flow.png` en la slide 7.

---

## Prompt para Gamma

```
Generá una presentación en español de exactamente **10 diapositivas** para una exposición oral universitaria de **15 minutos**.

**Presentador:** Tiago Vescovi  
**Materia:** Introducción a la Seguridad Informática (ISI) — 2026  
**Docente:** Andrés Pastorini  
**Escenario 11:** Plataforma de Asistencia Legal basada en IA Generativa (RAG)

---

## OBJETIVO DE LA PRESENTACIÓN

Mostrar una **visión general** del análisis de riesgos realizado: qué se analizó, metodología, hallazgos principales y conclusiones. **No** es un informe técnico exhaustivo. Lenguaje claro, orientado a negocio legal (confidencialidad, responsabilidad profesional, datos de clientes).

**Metodología del trabajo:** Inventario → Arquitectura y trust boundaries → STRIDE → DREAD → MITRE ATT&CK → Mitigación → Riesgos residuales.

**Cifras clave del análisis (usar estas, no inventar otras):**
- 4 trust boundaries (TB-01 a TB-04)
- 19 amenazas STRIDE en 3 componentes
- 14 amenazas priorizadas con DREAD
- 11 técnicas MITRE ATT&CK
- Residual tras mitigación: 0 Alto · 11 Medio · 3 Bajo

---

## REGLAS DE FORMATO

- **NO** generar Notas del orador / Speaker Notes.
- Todo el contenido debe ir **visible en las diapositivas**.
- Máximo **5 bullets** por slide (excepto slides con tablas).
- Diseño sobrio y profesional: azul/verde oscuro, acentos naranja para riesgos altos.
- **Slide 3 (Arquitectura):** reservar **≥70% del slide** como recuadro vacío centrado con el texto exacto: 「 SUBIR IMAGEN: arquitectura.png 」. NO generar diagramas con IA.
- **Slide 7 (ATT&CK):** si hay espacio, recuadro secundario o pie con: 「 SUBIR IMAGEN: attack-flow.png 」. NO generar diagramas con IA.
- En slides 5 y 6 usar las **tablas y datos** provistos abajo (pueden resumirse visualmente pero sin cambiar cifras ni nombres de amenazas).

---

## DIAPOSITIVA 1 — Carátula

Título: Análisis de Riesgos — Escenario 11  
Subtítulo: Plataforma legal con IA generativa (RAG)

Tiago Vescovi  
ISI 2026 — Introducción a la Seguridad Informática  
Andrés Pastorini

Sin bullets. Diseño limpio.

---

## DIAPOSITIVA 2 — Contexto y alcance del análisis

• Firma de servicios profesionales: plataforma para clientes externos y personal interno  
• Funciones: carga de contratos, consultas RAG, borradores con IA, auditoría de cumplimiento  
• Datos altamente sensibles: secreto comercial, datos personales, responsabilidad legal  
• Objetivo: identificar y priorizar riesgos con STRIDE, DREAD y MITRE ATT&CK  
• Entregables: inventario, arquitectura, STRIDE, DREAD, ATT&CK, mitigación y riesgo residual

---

## DIAPOSITIVA 3 — Arquitectura

Título: Arquitectura

「 SUBIR IMAGEN: arquitectura.png 」 (recuadro grande, ≥70% del slide)

Pie breve (bullets mínimos debajo o al costado):
• Frontend Next.js · API · Orquestador IA · ETL/RabbitMQ  
• Chroma on-prem o Pinecone · LLM comercial o LLM local  
• OAuth2 federado · 4 trust boundaries (TB-01 a TB-04)

---

## DIAPOSITIVA 4 — Inventario de activos

Título: Inventario de activos

Organizar en **4 bloques compactos** (no tabla gigante):

**Información (7):** documentos legales, corpus de referencia, consultas, respuestas RAG, borradores IA, auditoría, índice vectorial

**Tecnología (8):** Next.js, API, orquestador LangChain/LlamaIndex, vector DB, almacén documentos, LLM cloud, LLM local, RabbitMQ/ETL

**Identidad y procesos:** OAuth2, IdP, credenciales técnicas; procesos RAG y borradores/auditoría

**Humanos (5):** abogados, personal TI, clientes externos, personal administrativo, proveedor cloud/LLM (cadena de suministro)

Cierre: todos los activos críticos alineados al escenario legal con IA.

---

## DIAPOSITIVA 5 — Análisis STRIDE

Título: Análisis STRIDE

Intro (1 línea): STRIDE = Suplantación · Manipulación · Repudio · Divulgación · Denegación · Elevación de privilegios.

**Tabla resumen por componente** (generar tabla legible en slide):

| Componente | Amenazas | Críticas | Altas | Medias | Riesgo global |
|------------|----------|----------|-------|--------|---------------|
| Usuario | 5 | 0 | 3 | 2 | Alto |
| Frontend Next.js | 6 | 0 | 4 | 2 | Alto |
| Backend (API + Orquestador + ETL) | 8 | 3 | 4 | 1 | Crítico |
| **Total** | **19** | **3** | **11** | **5** | **Crítico** |

**Amenazas críticas del backend** (bullets o mini-tabla):
• Prompt injection en consulta RAG  
• Exfiltración de contexto hacia LLM cloud  
• Fallo de autorización multi-tenant

**Vectores propios de IA/RAG:** prompt injection, data poisoning, retrieval leakage, exfiltración a LLM comercial.

---

## DIAPOSITIVA 6 — Matriz DREAD con priorización

Título: Matriz DREAD con priorización

Subtítulo: 14 amenazas evaluadas · Score = promedio de Daño, Reproducibilidad, Explotabilidad, Usuarios afectados, Detectabilidad

**Generar tabla completa en la slide** (14 filas):

| Id | Componente | Amenaza | Score | Severidad |
|----|------------|---------|-------|-----------|
| V1 | Usuario | Robo de credenciales / sesión | 6,8 | Medio |
| V2 | Usuario | Exposición por sesión abierta | 6,4 | Medio |
| V3 | Usuario | Manipulación de requests (IDOR) | 7,8 | Alto |
| V4 | Frontend | Over-fetching de datos legales | 7,8 | Alto |
| V5 | Frontend | Secuestro de sesión (XSS) | 6,8 | Medio |
| V6 | Frontend | Dependencias comprometidas | 7,0 | Alto |
| V7 | Backend | Prompt injection en consulta RAG | 8,4 | Alto |
| V8 | Backend | Exfiltración de contexto hacia LLM cloud | 7,8 | Alto |
| V9 | Backend | Data poisoning en ingesta | 7,0 | Alto |
| V10 | Backend | Retrieval leakage (multi-tenant) | 7,4 | Alto |
| V11 | Backend | Fallo de autorización multi-tenant | 7,8 | Alto |
| V12 | Backend | Exfiltración en logs del orquestador | 7,6 | Alto |
| V13 | Backend | Agotamiento de recursos | 8,2 | Alto |
| V14 | Backend | Inferencias sin trazabilidad | 6,4 | Medio |

**Top 3 prioridad:** V7 (8,4) · V13 (8,2) · V3/V4/V8/V11 (7,8)

---

## DIAPOSITIVA 7 — ATT&CK

Título: ATT&CK — Escenario hipotético de ataque

**Actor:** atacante externo · **Objetivo:** contratos confidenciales y contexto RAG · **Entrada:** portal Next.js y API

Flujo resumido (máx. 5 bullets):
• Escaneo y phishing / cuentas válidas (T1595, T1566, T1078)  
• IDOR multi-tenant o prompt injection (T1059)  
• Data poisoning en ingesta (T1565)  
• Retrieval leakage y exfiltración a LLM cloud (T1005, T1567)  
• Recolección automatizada vía API (T1119)

**Tabla compacta de técnicas** (ID + nombre, 2 columnas o chips):
T1595 Active Scanning · T1566.002 Phishing · T1078 Valid Accounts · T1059.006 Prompt injection · T1068 Privilege Escalation · T1005 Data from Local System · T1119 Automated Collection · T1567.002 Exfiltration to Cloud · T1565 Data Manipulation · T1556 Modify Auth · T1070 Indicator Removal

Pie: componente más crítico = backend (API + orquestador).

「 SUBIR IMAGEN: attack-flow.png 」 si el layout lo permite.

---

## DIAPOSITIVA 8 — Riesgos residuales

Título: Riesgos residuales

Tras el plan de mitigación:

| Nivel | Cantidad |
|-------|----------|
| Bajo | 3 |
| Medio | 11 |
| Alto | 0 |
| Crítico | 0 |

Bullets:
• El patrón dominante es **Medio**: configuración, evolución de ataques IA, dependencia operativa  
• **Aceptados formalmente:** alucinación del LLM, borradores sin revisión humana, Pinecone SaaS (si aplica)  
• Transferir riesgo al proveedor cloud **no** elimina la responsabilidad de la firma

---

## DIAPOSITIVA 9 — Recomendaciones principales

Título: Recomendaciones principales

Cuatro bloques (1 bullet cada bloque con sub-bullets cortos):

**1. Acceso:** MFA · RBAC · OAuth2 · aislamiento multi-tenant

**2. RAG / IA:** validación de entradas · sanitización de prompts · filtros en recuperación vectorial

**3. Datos sensibles:** enrutar lo crítico al LLM local · preferir Chroma on-prem · cifrado

**4. Proceso:** revisión legal obligatoria de salidas · logging y trazabilidad de inferencias

Prioridad inmediata (mencionar): V7 prompt injection · V13 agotamiento de recursos · V8 exfil LLM cloud

---

## DIAPOSITIVA 10 — Conclusiones

Título: Conclusiones

• Los riesgos se concentran en el **backend**: autorización multi-tenant, RAG y salida hacia LLMs externos  
• STRIDE + DREAD + ATT&CK permiten priorizar con base técnica y comunicar a negocio  
• La defensa es en **capas**: controles técnicos + enrutamiento por sensibilidad + revisión humana  
• Con IA generativa no hay cero riesgo: mitigar, documentar y aceptar conscientemente lo residual

Cierre opcional una línea: Gracias · ¿Preguntas?

---

## RECORDATORIO FINAL PARA GAMMA

- Exactamente 10 slides, en este orden.
- Presentación ejecutiva (~1,5 min por slide), no informe.
- Sin notas del orador.
- Datos de tablas STRIDE y DREAD tal como se indicaron.
```
