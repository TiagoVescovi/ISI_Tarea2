# Análisis STRIDE

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`01-inventario-activos.md`](01-inventario-activos.md), [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md).

**Alcance:** catálogo de amenazas STRIDE sobre componentes principales y trust boundaries (TB-01 … TB-10). Incluye amenazas propias de **IA generativa y RAG** cuando aplican al escenario. **No** incluye DREAD, mitigaciones, priorización ni evaluación de riesgo residual.

**Versión:** revisión crítica (consolidación de redundancias y refuerzo de amenazas LLM/RAG). Ver [Apéndice — Consolidaciones](#apéndice--consolidaciones).

**Convención de IDs:** `STR-Cxx-y` = amenaza sobre **componente**; `STR-TBxx-y` = amenaza en **trust boundary**. La letra final indica categoría STRIDE: **S**poofing, **T**ampering, **R**epudiation, **I**nformation disclosure, **D**enial of service, **E**levation of privilege. Sufijos `T2`, `T3` = variantes **T**ampering distintas en el mismo componente (p. ej. vectores de ataque RAG).

**Criterio Parte A vs Parte B:** la Parte A modela amenazas **intrínsecas al componente o proceso**. La Parte B modela solo amenazas **propias del cruce de la frontera** (mecanismos que dependen del salto de confianza). Si una amenaza es la misma en ambos niveles, se documenta **una vez** y se referencia desde la frontera.

---

## Leyenda STRIDE

| Categoría | Significado en este catálogo |
|-----------|------------------------------|
| **S** | Suplantación de identidad, origen o actor (usuario, servicio, mensaje, token). |
| **T** | Alteración no autorizada de datos, código, mensajes, índices, contexto recuperado o resultados generados. |
| **R** | Imposibilidad de atribuir o auditar una acción o inferencia de forma fiable. |
| **I** | Divulgación de información a quien no debería acceder a ella. |
| **D** | Interrupción o degradación del servicio o del procesamiento. |
| **E** | Obtención de privilegios o acceso fuera del mandato del sujeto. |

---

## Parte A — Amenazas por componente principal

### A.1 Aplicación frontend institucional (Next.js)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C01-S | S | Suplantación del usuario en el navegador (sesión activa comprometida o dispositivo compartido) que permite operar la UI como titular de la cuenta. | Identidades federadas (OAuth2); Consultas en lenguaje natural; Documentos contractuales (vía acciones en UI) | Punto de interacción humano; la sesión en el cliente condiciona quién actúa (TB-01). |
| STR-C01-T | T | Alteración en el cliente del comportamiento de la interfaz (DOM, scripts inyectados, extensiones) que modifica lo mostrado o lo enviado respecto de la intención del usuario. | Aplicación frontend (Next.js); Respuestas RAG; Borradores preliminares; Resultados de auditoría | Integridad percibida del asesoramiento legal expuesto en pantalla. |
| STR-C01-I | I | Exposición involuntaria en el cliente de datos sensibles (caché, almacenamiento local, historial) derivados de consultas, respuestas o documentos visualizados. | Consultas; Respuestas RAG; Documentos contractuales; Borradores preliminares | Activos de alta confidencialidad transitan por el canal usuario–frontend (TB-01). |

*Categorías no modeladas:* **D** (degradación local del navegador no altera la disponibilidad del servicio backend); **R** (la atribución recae en API/sesión, no en el frontend aislado); **E** (escalada de privilegios se modela en API y TB-09).

---

### A.2 Servicios de aplicación / API

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C02-S | S | Invocación de la API con token o identidad inválida, expirada o falsificada aceptada por fallo de validación. | Identidades federadas (OAuth2); Servicios de aplicación / API | Guardián lógico entre UI y subsistemas (TB-02, TB-03). |
| STR-C02-T | T | Alteración de payloads en la capa de aplicación (metadatos de documentos, parámetros de consulta RAG, referencias a trabajos en cola). | Documentos contractuales; Consultas; Representación indexada (metadatos); Proceso de ingestión | Acopla carga, consultas y publicación a RabbitMQ y orquestador. |
| STR-C02-R | R | Acciones vía API sin registro auditable suficiente (carga, consulta, borrador, auditoría). | Activos de información intercambiados por API; Identidades federadas | Trazabilidad exigida en ecosistema legal y de cumplimiento. |
| STR-C02-I | I | Respuestas de API con sobreexposición de datos (enumeración, exceso de campos, fuga entre contextos cliente/interno). | Documentos contractuales; Documentación legal de referencia; Representación indexada; Respuestas RAG | Debe acotar alcance por identidad (TB-09). |
| STR-C02-D | D | Agotamiento de la capa API por volumen de solicitudes que degrada ingestión y consultas. | Servicios de aplicación / API; Procesos RAG, borradores y auditoría | Entrada síncrona hacia orquestador, cola y almacenes. |
| STR-C02-E | E | Fallo de autorización que permite a un sujeto acceder a recursos o operaciones de otro rol o tenant. | Identidades federadas; Documentos contractuales; Documentación legal; Plantillas; Resultados de auditoría | Uso dual interno/cliente; complementa TB-09-E en el enforcement de API. |

---

### A.3 Orquestador de IA (LangChain / LlamaIndex en Python)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C03-T | T | Alteración no autorizada de cadenas de prompts, herramientas de agente o parámetros de recuperación que cambian el comportamiento del RAG o de agentes de borradores/auditoría. | Procesos RAG, borradores y auditoría; Respuestas RAG; Borradores; Resultados de auditoría | Núcleo de IA generativa y agentes autónomos del escenario. |
| STR-C03-R | R | Decisiones del orquestador (fragmentos recuperados, modelo invocado, borrador emitido) sin traza reproducible. | Procesos RAG, borradores y auditoría; Consultas; Representación indexada | Reconstrucción del tratamiento automatizado para responsabilidad profesional. |
| STR-C03-I | I | Filtración de contexto sensible en logs, trazas o salidas intermedias del orquestador (fragmentos de contratos en prompts ensamblados). | Representación indexada; Documentos contractuales; Consultas; Credenciales técnicas | Ensambla contexto RAG antes de TB-07/TB-08. |
| STR-C03-D | D | Bloqueo o lentitud del orquestador por volumen de consultas/agentes. | Orquestador de IA; Procesos RAG, borradores y auditoría | Componente compartido por todos los casos de uso de IA. |
| STR-C03-E | E | Agente o flujo que ejecuta herramientas (lectura de índice, LLM, plantillas) fuera del alcance de la sesión iniciadora. | Procesos RAG, borradores y auditoría; Base vectorial; Plantillas; Integración LLM comercial / LLM local | Agentes autónomos amplían superficie de acción (escenario). |

*Categorías no modeladas:* **S** (suplantación de origen de peticiones internas cubierta por STR-C02-S y credenciales en STR-C11-I).

---

### A.4 Broker y mensajería (RabbitMQ)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C04-S | S | Publicación en la cola haciéndose pasar por la API u otro publicador legítimo. | Broker RabbitMQ; Proceso de ingestión; Credenciales técnicas | TB-04; mensajes disparan pipeline ETL. |
| STR-C04-T | T | Modificación de mensajes (referencias a documentos, prioridad, reintentos) que corrompe el trabajo de ingestión. | Proceso de ingestión; Documentos contractuales (referencias); Representación indexada (indirecta) | Integridad del pipeline asíncrono. |
| STR-C04-I | I | Lectura no autorizada de colas con metadatos o referencias a documentos confidenciales. | Documentos contractuales; Documentación legal de referencia; Credenciales técnicas | Mensajes pueden identificar cargas sensibles. |
| STR-C04-D | D | Inundación de la cola o agotamiento del broker que retrasa o impide indexación. | Broker RabbitMQ; Proceso de ingestión | ETL asíncrono explícito en el escenario. |

---

### A.5 Proceso de ingestión y preparación de datos (pipeline ETL / workers)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C05-T | T | Corrupción en extracción, chunking o generación de embeddings (texto truncado, metadatos erróneos) que desalinea el índice respecto de los documentos fuente. | Representación indexada; Documentos contractuales; Documentación legal de referencia | RAG “fundamentado en documentación cargada” depende de ETL íntegro. |
| STR-C05-T2 | T | **Data poisoning:** documento cargado (PDF/Word) con contenido diseñado para envenenar el índice o influir en recuperaciones futuras (instrucciones embebidas, texto irrelevante masivo, metadatos engañosos). | Documentos contractuales; Representación indexada; Proceso de ingestión | Vector de ataque RAG en la **ingesta**; riesgo de corpus envenenado documentado en el escenario. |
| STR-C05-I | I | Indexación en namespace o metadatos de un contexto distinto al autorizado (mezcla lógica de corpus entre clientes o entre interno y cliente). | Representación indexada; Documentos contractuales; Documentación legal | Riesgo multi-usuario sin aislamiento estricto (TB-09). |
| STR-C05-E | E | Worker con permisos excesivos sobre almacén o colas respecto del mandato del pipeline. | Almacén de documentos y plantillas; Credenciales técnicas; Proceso de ingestión | TB-05; privilegios batch amplios. |

*Categorías no modeladas:* **S** (publicador falso cubierto por STR-C04-S); **D** (disponibilidad del broker en STR-C04-D); **R** (trazabilidad de jobs en STR-C02-R / proceso).

---

### A.6 Almacén de documentos y plantillas

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C06-T | T | Sustitución o borrado no autorizado de archivos fuente o plantillas. | Documentos contractuales; Documentación legal de referencia; Plantillas institucionales | Integridad documental impacta RAG y borradores. |
| STR-C06-I | I | Lectura no autorizada de contratos o plantillas por actor sin mandato. | Documentos contractuales; Plantillas institucionales | Confidencialidad alta en inventario. |
| STR-C06-D | D | Indisponibilidad del almacén que impide cargas, ingestión o uso de plantillas por agentes. | Almacén de documentos y plantillas; Proceso de ingestión; Proceso de borradores | Persistencia necesaria para carga y agentes. |

*Categorías no modeladas:* **S** (acceso con credencial suplantada cubierto por STR-C11-I y STR-C04-S); **E** (sobre-privilegio de workers en STR-C05-E).

---

### A.7 Base de datos vectorial (Chroma o Pinecone)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C07-T | T | Alteración de embeddings o metadatos en el índice que cambia qué fragmentos recupera el RAG (incluye escritura maliciosa desde componente comprometido). | Representación indexada; Respuestas RAG (indirectas) | Integridad del fundamento de las respuestas. |
| STR-C07-I | I | **Retrieval leakage:** recuperación por similitud que expone fragmentos de documentos fuera del alcance del consultante (cross-tenant, cross-corpus o por metadatos incorrectos). | Representación indexada; Documentos contractuales; Documentación legal; Respuestas RAG | Riesgo central del RAG en corpus legal mezclado; término reconocido en literatura LLM/RAG. |
| STR-C07-D | D | Caída o degradación del servicio vectorial que impide consultas y actualizaciones de índice. | Base de datos vectorial; Proceso consulta RAG; Proceso de ingestión | Alta criticidad de disponibilidad en inventario. |

*Categorías no modeladas:* **S** (cliente suplantado vía STR-C11-I / credenciales); **R** (atribución de consultas en STR-C12-R); **E** (autorización de lectura en STR-C02-E / STR-C07-I).

---

### A.8 Integración con LLM comerciales (API OpenAI / Anthropic)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C08-S | S | Uso de la API comercial con claves robadas o cliente suplantado hacia el proveedor. | Integración LLM comerciales; Credenciales técnicas | Integración explícita por API (TB-07). |
| STR-C08-D | D | Indisponibilidad o limitación de cuota del proveedor que bloquea flujos dependientes de la API comercial. | Integración LLM comerciales; Procesos RAG, borradores y auditoría | Dependencia de tercero para inferencia. |

*Categorías no modeladas:* **I** y **R** en cruce hacia tercero modeladas en **STR-TB07-I** y **STR-TB07-R** (frontera de salida de perímetro). **T** en tránsito hacia proveedor de baja plausibilidad con TLS estándar; no forzada.

---

### A.9 Servicio de inferencia con LLM open source local

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C09-S | S | Invocación no autorizada del endpoint de inferencia local haciéndose pasar por el orquestador. | Servicio LLM local OSS; Credenciales técnicas; Orquestador de IA | Canal para datos altamente sensibles (TB-08). |
| STR-C09-T | T | Sustitución de modelo, pesos o configuración de inferencia local que altera salidas sin detección del orquestador. | Servicio LLM local OSS; Borradores; Respuestas RAG; Resultados de auditoría | Ciclo de vida distinto orquestador vs runtime (TB-08). |
| STR-C09-D | D | Caída del servicio local que impide tratar casos reservados a LLM on-prem. | Servicio LLM local OSS; Procesos enrutados por local | Política de datos sensibles del escenario. |

*Categorías no modeladas:* **I** en canal interno en **STR-TB08-I**; **R** en STR-C03-R; **E** en STR-C03-E.

---

### A.10 Proveedor de identidad federado (IdP)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C10-S | S | Emisión o aceptación de tokens OAuth2 fraudulentos o compromiso de cuentas en el IdP. | Identidades federadas (OAuth2); Proveedor IdP | OAuth2 federado explícito (TB-03). |
| STR-C10-T | T | Manipulación de *claims* o atributos de rol (cliente vs interno) en tokens emitidos o aceptados. | Identidades federadas; Servicios de aplicación / API | TB-09 depende de atributos correctos. |
| STR-C10-I | I | Filtración de metadatos de identidad o tokens en canales de autenticación mal protegidos. | Identidades federadas; Credenciales técnicas (client secret, si aplica) | Flujos OAuth atraviesan TB-03. |
| STR-C10-D | D | Indisponibilidad del IdP que impide autenticaciones nuevas. | Proveedor IdP; Frontend; Servicios de aplicación / API | Dependencia de tercero para acceso. |

*Categorías no modeladas:* **R** (repudio de login federado de bajo impacto operativo frente a auditoría de API); **E** (escalada vía claims en STR-C10-T y STR-C02-E).

---

### A.11 Entorno de ejecución Python y dependencias del orquestador

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C11-T | T | Compromiso de dependencias (LangChain, LlamaIndex u otras) que altera el comportamiento del orquestador. | Orquestador de IA; Entorno Python; Procesos RAG, borradores y auditoría | Base de integridad del procesamiento de IA. |
| STR-C11-I | I | Lectura no autorizada de secretos o variables de entorno en el host del orquestador. | Credenciales técnicas; Integración LLM comercial; Base vectorial; Almacén | Concentración de credenciales de integración. |
| STR-C11-D | D | Fallo del host o agotamiento de recursos que detiene flujos de IA. | Entorno Python; Orquestador de IA | Alta disponibilidad del núcleo de IA. |

*Categorías no modeladas:* **S**, **R**, **E** (no aplican de forma distintiva a este activo de infraestructura en el escenario documentado).

---

### A.12 Proceso de consulta RAG (recuperación + generación)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C12-T | T | **Prompt injection (directa):** instrucciones maliciosas en la consulta del usuario que intentan anular políticas del sistema, exfiltrar contexto o manipular la salida del LLM. | Consultas en lenguaje natural; Respuestas RAG; Representación indexada | Vector clásico de prompt injection en sistemas con entrada en lenguaje natural (escenario). |
| STR-C12-T2 | T | **Indirect prompt injection:** instrucciones embebidas en documentos del corpus (cargados o de referencia) que el RAG recupera y el LLM interpreta como mandatos, alterando comportamiento o filtrando contenido. | Documentos contractuales; Documentación legal; Representación indexada; Respuestas RAG | Ataque vía **retrieval** sin modificar la consulta del usuario; distinto de STR-C12-T y de STR-C05-T2 (envenenamiento en ingesta). |
| STR-C12-T3 | T | **Manipulación de contexto recuperado / alucinación con impacto legal:** el LLM presenta como fundamentada una respuesta con citas, cláusulas o conclusiones jurídicas incorrectas, inventadas o no sustentadas en los fragmentos recuperados. | Respuestas RAG; Documentación legal de referencia; Consultas | Integridad del asesoramiento automatizado; riesgo reputacional y de responsabilidad profesional en dominio legal. |
| STR-C12-R | R | Imposibilidad de reconstruir qué fragmentos sustentaron una respuesta ante reclamación o revisión. | Respuestas RAG; Representación indexada; Consultas | Trazabilidad del fundamento documental del RAG. |

*Categorías no modeladas:* **I** (**Retrieval leakage** en STR-C07-I; exposición vía respuesta en STR-TB09-I); **S**, **D**, **E** (cubiertas en API, orquestador y vector DB).

---

### A.13 Proceso de generación de borradores contractuales por agentes de IA

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C13-T | T | Borrador con cláusulas alteradas, omitidas o jurídicamente incoherentes respecto de plantillas y datos del caso (incluye alucinación de términos contractuales). | Borradores preliminares; Plantillas institucionales; Documentos contractuales | Agentes autónomos sobre plantillas — capacidad explícita del escenario. |
| STR-C13-I | I | Incorporación en el borrador de información de otro asunto o cliente por recuperación errónea del agente. | Borradores; Documentos contractuales; Representación indexada | Combina plantillas, RAG y datos del caso. |
| STR-C13-E | E | Agente que accede a plantillas o documentos fuera del alcance del rol o asunto iniciador. | Proceso de borradores; Plantillas; Identidades federadas | TB-09 y naturaleza autónoma del agente. |

*Categorías no modeladas:* **S**, **D**, **R** (R parcialmente en STR-C03-R; D en STR-C03-D / STR-C08-D / STR-C09-D).

---

### A.14 Proceso de auditoría automatizada de cumplimiento normativo local

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-C14-T | T | Resultados de auditoría falsos o manipulados (hallazgos omitidos, inventados o contradichos por el documento evaluado). | Resultados de auditoría automatizada; Documentos contractuales | Módulo de auditoría automatizada explícito; alucinación con impacto en cumplimiento. |
| STR-C14-I | I | Divulgación de resultados de auditoría a usuarios o contextos no autorizados. | Resultados de auditoría; Identidades federadas | Alta confidencialidad en inventario. |
| STR-C14-R | R | Falta de trazabilidad de criterios o fuentes normativas usadas en una evaluación automatizada. | Resultados de auditoría; Proceso de auditoría | Fuentes normativas aún TODO en escenario; repudio de proceso sigue siendo relevante. |

*Categorías no modeladas:* **S**, **D**, **E** (autenticación en STR-C02-*; disponibilidad compartida; escalada en STR-C02-E).

---

## Parte B — Amenazas por trust boundary

> Solo se listan amenazas **no duplicadas** en Parte A. Cuando la frontera comparte el mismo problema de seguridad, se referencia el ID del componente.

### TB-01 — Usuario / red externa ↔ Frontend Next.js

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB01-S | S | **Phishing / interfaz falsa:** el usuario autentica o introduce datos en una UI que imita el frontend institucional fuera del dominio de confianza de la firma. | Identidades federadas; Aplicación frontend | Mecanismo propio del cruce humano–sistema; distinto de STR-C01-S (sesión ya dentro de UI legítima). |
| STR-TB01-I | I | Interceptación u observación en red o dispositivo del usuario (red pública, malware en endpoint) de cargas, consultas o respuestas. | Documentos contractuales; Consultas; Respuestas RAG; Borradores; Resultados de auditoría | Activos listados en TB-01 atraviesan esta frontera. |
| STR-TB01-T | T | Inyección en el navegador que altera tráfico o presentación **antes** de que la API aplique controles de servidor. | Consultas; Respuestas RAG; Frontend Next.js | Complementa STR-C01-T en el lado servidor/cliente de la frontera. |

---

### TB-02 — Frontend Next.js ↔ Servicios de aplicación / API

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB02-S | S | Interceptación o robo de tokens de sesión/API **en tránsito** entre navegador y backend (p. ej. red comprometida, TLS mal configurado). | Identidades federadas; Credenciales técnicas (tokens) | Mecanismo de frontera; STR-C02-S cubre fallo de **validación** en servidor, no el robo en tránsito. |
| STR-TB02-E | E | Invocación directa de la API eludiendo la UI (abuso de endpoints, herramientas automatizadas) para operaciones no previstas en el flujo de presentación. | Servicios de aplicación / API; Activos accesibles vía API | Cambio de modelo de ejecución cliente vs servidor (TB-02). |

*Referencia:* alteración de payloads y sobreexposición — **STR-C02-T**, **STR-C02-I**.

---

### TB-03 — Plataforma ↔ Proveedor de identidad federado (OAuth2)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB03-S | S | Ataque al **flujo OAuth** (redirect manipulado, intercepción de código de autorización, cliente OAuth mal registrado). | Identidades federadas; Proveedor IdP | Mecanismo de federación en el cruce de perímetro; complementa STR-C10-S (compromiso en IdP). |
| STR-TB03-T | T | Alteración de tokens o aserciones **en tránsito** entre IdP y plataforma si el canal o la validación es deficiente. | Identidades federadas; Servicios de aplicación / API | Distinto de STR-C10-T (manipulación de claims en emisión). |

*Referencia:* indisponibilidad del IdP — **STR-C10-D**.

---

### TB-04 — Servicios de aplicación ↔ RabbitMQ

*Amenazas en esta frontera:* **STR-C04-S**, **STR-C04-T**, **STR-C04-I**, **STR-C04-D** (modeladas en el componente broker; el cruce HTTP→mensajería no añade vectores distintos en el escenario documentado).

---

### TB-05 — Workers ETL ↔ Almacén de documentos y plantillas

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB05-T | T | Modificación no autorizada de archivos en el almacén **desde el contexto privilegiado del worker** durante o tras el procesamiento ETL. | Documentos contractuales; Plantillas institucionales | Vector en el cruce batch–repositorio; STR-C06-T cubre el almacén como activo, no el abuso del worker en la frontera. |

*Referencia:* lectura fuera de alcance del job — **STR-C05-E**; lectura no autorizada genérica — **STR-C06-I**.

---

### TB-06 — Indexación / consulta ↔ Base de datos vectorial

*Amenazas en esta frontera:* **STR-C07-T**, **STR-C07-I** (Retrieval leakage), **STR-C07-D**. La lectura/escritura en el índice no introduce categorías adicionales justificadas más allá del componente vectorial.

---

### TB-07 — Orquestador ↔ LLM comercial (OpenAI / Anthropic)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB07-I | I | **Exposición de información sensible vía RAG hacia tercero:** tránsito de consultas, contexto recuperado o fragmentos de contratos/jurisprudencia al proveedor cloud como parte del prompt o telemetría permitida. | Consultas; Representación indexada; Documentos contractuales; Documentación legal; Respuestas RAG | Salida de perímetro explícita en escenario (LLM comercial vs local para datos sensibles). |
| STR-TB07-R | R | Disputa sobre qué datos fueron procesados por el proveedor sin evidencia correlacionable en la plataforma. | Procesos RAG, borradores y auditoría; Consultas | Límite de auditoría con operador externo. |

*Referencia:* claves robadas hacia proveedor — **STR-C08-S**; indisponibilidad — **STR-C08-D**.

---

### TB-08 — Orquestador ↔ LLM open source local

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB08-I | I | Exposición del contexto sensible en el **canal interno** orquestador–inferencia (logs, volcados, API sin autenticación entre procesos). | Consultas; Representación indexada; Documentos contractuales | Datos altamente sensibles enrutados aquí; distinto de STR-C09-T (integridad del modelo). |

*Referencia:* suplantación y sustitución de modelo — **STR-C09-S**, **STR-C09-T**.

---

### TB-09 — Rol cliente ↔ Rol personal interno (autorización lógica)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB09-E | E | Cliente que accede a corpus interno, documentos de otro cliente o funciones reservadas al personal (auditoría, borradores avanzados). | Documentación legal; Documentos contractuales; Resultados de auditoría; Plantillas; Procesos RAG, borradores y auditoría | Frontera lógica de gobernanza “interno + clientes” (escenario). |
| STR-TB09-I | I | **Filtración horizontal en salidas:** respuesta RAG o borrador que mezcla información de otro asunto/cliente en la sesión activa. | Respuestas RAG; Borradores; Representación indexada | Manifestación en la frontera de roles de fallos de aislamiento (complementa STR-C07-I en recuperación). |

*Referencia:* enforcement en API — **STR-C02-E**; claims erróneos — **STR-C10-T**.

---

### TB-10 — (Condicional) Plataforma ↔ Base vectorial SaaS (p. ej. Pinecone)

| ID | Categoría | Descripción | Activos afectados | Justificación |
|----|-----------|-------------|-------------------|---------------|
| STR-TB10-I | I | Acceso del **proveedor SaaS** del índice a embeddings y metadatos derivados de documentos confidenciales en su infraestructura. | Representación indexada; Base de datos vectorial (Pinecone); Documentos contractuales (derivados) | TB-10 condicional; subprocesador adicional respecto de Chroma on-prem. |
| STR-TB10-D | D | Indisponibilidad del servicio gestionado externo con componentes on-prem aún activos. | Base de datos vectorial; Proceso consulta RAG; Proceso de ingestión | Dependencia de tercero adicional. |
| STR-TB10-R | R | Limitación para demostrar accesos al índice en el lado del proveedor ante un incidente. | Representación indexada; Base de datos vectorial | Cadena de subprocesadores (TB-10). |

---

## Resumen del catálogo

| Ámbito | Entradas activas | Notas |
|--------|------------------|--------|
| Parte A — Componentes | **47** | Incluye STR-C05-T2, STR-C12-T/T2/T3 (IA/RAG) |
| Parte B — Trust boundaries | **16** | TB-04 y TB-06 por referencia a Parte A |
| **Total IDs únicos** | **63** | Reducción desde 80 tras consolidación |

| Categoría STRIDE | Recuento (Parte A + B) |
|------------------|------------------------|
| S | 10 |
| T | 18 |
| R | 6 |
| I | 18 |
| D | 9 |
| E | 6 |

---

## Relación con otros entregables

| Documento | Relación |
|-----------|----------|
| [`04-priorizacion-dread.md`](04-priorizacion-dread.md) | Priorización DREAD de 16 amenazas STR-* (rangos según `references/DREAD.md`). |
| [`05-mapa-attack.md`](05-mapa-attack.md) | Mapear amenazas prioritarias a tácticas y técnicas MITRE ATT&CK. |
| [`06-plan-mitigacion.md`](06-plan-mitigacion.md) | Controles por ID de amenaza. |
| [`07-riesgos-residuales.md`](07-riesgos-residuales.md) | Riesgo tras mitigación. |

---

## Apéndice — Consolidaciones

Amenazas **eliminadas o fusionadas** respecto de la versión anterior (80 entradas). Motivo: mismo problema de seguridad modelado dos veces (componente + frontera) o categoría poco justificada en el escenario.

| ID anterior | Disposición | Motivo |
|-------------|-------------|--------|
| STR-C01-D | Eliminada | Degradación solo local del navegador; no amenaza distintiva del activo en el escenario. |
| STR-C03-S | Eliminada | Suplantación de llamadas internas cubierta por STR-C02-S y STR-C11-I. |
| STR-C05-S | Eliminada | Publicador falso en cola = STR-C04-S. |
| STR-C06-S | Eliminada | Acceso con credencial suplantada cubierto por credenciales técnicas (STR-C11-I). |
| STR-C08-I | Fusionada en STR-TB07-I | Divulgación hacia LLM cloud es propia del cruce de perímetro TB-07. |
| STR-C08-R | Fusionada en STR-TB07-R | Repudio con proveedor externo en frontera TB-07. |
| STR-C09-I | Fusionada en STR-TB08-I | Exposición en canal interno orquestador–inferencia. |
| STR-C12-I | Fusionada en STR-C07-I | Retrieval leakage modelado en base vectorial / recuperación. |
| STR-C12-T (texto genérico) | Reemplazada por STR-C12-T, STR-C12-T2, STR-C12-T3 | Desglose Prompt Injection directa/indirecta y alucinación legal. |
| STR-TB02-T | Eliminada | Duplicaba STR-C02-T en la frontera API. |
| STR-TB03-D | Eliminada | Duplicaba STR-C10-D. |
| STR-TB04-S/T/I | Por referencia | Duplicaban STR-C04-S/T/I. |
| STR-TB05-I | Por referencia | Duplicaba STR-C05-E / STR-C06-I. |
| STR-TB05-E | Eliminada | Duplicaba STR-C05-E. |
| STR-TB06-I/T/D | Por referencia | Duplicaban STR-C07-I/T/D. |
| STR-TB07-T | Eliminada | Tampering en tránsito TLS poco plausible como amenaza separada sin evidencia de diseño. |
| STR-TB08-S/T | Por referencia | Duplicaban STR-C09-S/T. |
| STR-TB09-S | Eliminada | Suplantación cliente→interno cubierta por STR-C10-T, STR-TB03-S y STR-TB09-E. |

**Nuevos IDs:** STR-C05-T2 (Data poisoning); STR-C12-T2 (Indirect prompt injection); STR-C12-T3 (alucinación / contexto con impacto legal). STR-C12-T renombrado conceptualmente a Prompt injection directa (mismo ID conservado).

---

## Trazabilidad

- Inventario de activos: [`01-inventario-activos.md`](01-inventario-activos.md)
- Trust boundaries: [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md)
- Registro de prompts (IA): [`../prompts/06-prompt-matriz-stride.md`](../prompts/06-prompt-matriz-stride.md), [`../prompts/07-prompt-revision-stride.md`](../prompts/07-prompt-revision-stride.md)
