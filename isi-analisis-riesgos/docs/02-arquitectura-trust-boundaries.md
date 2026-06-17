# Arquitectura y trust boundaries

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** enunciado del escenario, arquitectura lógica validada en el trabajo (Next.js; OAuth2 federado; servicios de aplicación/API; RabbitMQ y pipeline ETL; orquestador LangChain/LlamaIndex; base vectorial Chroma o Pinecone; almacén de documentos y plantillas; APIs LLM OpenAI/Anthropic; LLM open source local) e [`01-inventario-activos.md`](01-inventario-activos.md).

**Alcance de este documento:** descripción de la arquitectura lógica de referencia y catálogo de **trust boundaries** para uso posterior en modelado de amenazas. **No** incluye STRIDE, DREAD, identificación de amenazas, mitigaciones ni evaluación de riesgos.

---

## Propósito

Describir la arquitectura lógica del sistema y delimitar **trust boundaries** (fronteras donde cambia el conjunto de controles, responsables o nivel de confianza) de forma explícita y trazable respecto del inventario de activos.

---

## Arquitectura lógica de referencia (resumen)

| Bloque | Componentes (nombres alineados al inventario) |
|--------|-----------------------------------------------|
| Presentación | Aplicación frontend institucional (Next.js) |
| Aplicación y orquestación | Servicios de aplicación / API; Orquestador de IA (LangChain / LlamaIndex en Python); Entorno de ejecución Python |
| Identidad externa | Proveedor de identidad federado (IdP) vinculado a OAuth2 |
| Mensajería y ETL | Broker y mensajería (RabbitMQ); proceso de ingestión y preparación de datos (pipeline ETL asíncrono) |
| Persistencia | Almacén de documentos y plantillas; Base de datos vectorial (Chroma o Pinecone) |
| Modelos | Integración con LLM comerciales (API OpenAI / Anthropic); Servicio de inferencia con LLM open source en instalación local |
| Procesos de negocio (lógicos) | Proceso de consulta RAG; Proceso de generación de borradores contractuales por agentes de IA; Proceso de auditoría automatizada de cumplimiento normativo local |

Los flujos de datos para diagramación se documentarán en [`../diagrams/arquitectura.drawio`](../diagrams/arquitectura.drawio) y [`../diagrams/arquitectura.png`](../diagrams/arquitectura.png).

---

## Trust boundaries

Cada frontera resume **dónde** cambia la confianza entre partes o zonas lógicas, **qué** componentes la delimitan, **qué** activos la atraviesan (según el inventario) y **por qué** es relevante documentarla para el análisis posterior (sin listar amenazas ni controles).

---

### TB-01 — Estación del usuario y red externa ↔ Aplicación web (Next.js)

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-01 — Perímetro usuario / Internet ↔ Frontend Next.js |
| **Componentes involucrados** | Usuario (cliente de la firma o personal interno); red y equipos fuera del control operativo directo de la plataforma; **Aplicación frontend institucional (Next.js)** (tal como se expone vía HTTPS según despliegue típico, no detallado en el escenario). |
| **Descripción** | Primera frontera entre el entorno del usuario y el software de interfaz bajo responsabilidad del proyecto de la firma. |
| **Motivo del cambio de confianza** | El dispositivo y la red del usuario no están bajo las mismas políticas ni supervisión que el código y el despliegue del frontend; comienza la zona atribuible al ecosistema de la firma. |
| **Activos que atraviesan la frontera** | **Información:** Documentos contractuales y otros documentos legales cargados por clientes (PDF/Word); Consultas en lenguaje natural de usuarios; Respuestas del sistema basadas en RAG; Borradores preliminares de contratos comerciales generados por agentes de IA; Resultados de la auditoría automatizada de cumplimiento normativo local (cuando se muestran en UI). **Identidad:** Identidades federadas de usuarios (OAuth2) — en forma de flujos de autenticación y sesión hacia el navegador. **Software:** Aplicación frontend institucional (Next.js) — entrega de código y activos estáticos al cliente. |
| **Justificación** | El escenario contempla uso **interno y orientado a clientes**; toda información legal y confidencial entra o sale por este canal respecto del usuario humano. Documentar la frontera permite anclar después el modelado al perímetro humano-interfaz sin confundir controles del endpoint con controles del backend. |

---

### TB-02 — Frontend Next.js ↔ Servicios de aplicación / API

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-02 — Presentación ↔ Lógica de aplicación en servidor |
| **Componentes involucrados** | **Aplicación frontend institucional (Next.js)**; **Servicios de aplicación / API** (hipótesis mínima de la arquitectura validada). |
| **Descripción** | Separación entre código que corre principalmente en el contexto del navegador y la lógica de negocio, autorización de servidor y acoplamiento a colas, almacenes y orquestador que residen en el lado servidor. |
| **Motivo del cambio de confianza** | Distinto nivel de control sobre integridad y confidencialidad: el servidor aplica reglas de negocio y acceso que el cliente no debe poder eludir; cambia el modelo de ejecución (cliente vs servidor de confianza). |
| **Activos que atraviesan la frontera** | **Información:** Igual que en TB-01 en cuanto a cargas, consultas y resultados intercambiados por API; además **Documentación legal de referencia** y **Plantillas institucionales** cuando el backend las sirve o procesa por solicitud del frontend. **Identidad:** Tokens u objetos de sesión entre frontend y API (derivados de **Identidades federadas de usuarios (OAuth2)**). **Software:** Ambos componentes como piezas que intercambian datos. |
| **Justificación** | Es la frontera clásica donde se centraliza la autorización y la preparación de trabajos asíncronos (p. ej. publicación a cola); es esencial para enlazar después controles de API sin mezclarlos con la superficie del navegador. |

---

### TB-03 — Plataforma ↔ Proveedor de identidad federado (OAuth2)

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-03 — Servicios de aplicación / API (y/o frontend) ↔ Proveedor de identidad federado (IdP) |
| **Componentes involucrados** | **Servicios de aplicación / API**; **Aplicación frontend institucional (Next.js)** (según el flujo OAuth elegido en implementación); **Proveedor de identidad federado (IdP)**. |
| **Descripción** | Intercambio de protocolo OAuth2 con un tercero de confianza para autenticación y federación, explícito en el escenario. |
| **Motivo del cambio de confianza** | La validez de identidades y tokens depende de controles y disponibilidad ajenos al núcleo de aplicación de la firma; cambia el responsable primario de la autenticación. |
| **Activos que atraviesan la frontera** | **Identidad:** Identidades federadas de usuarios (OAuth2); aserciones, códigos de autorización o tokens según el flujo (detalle de implementación: **TODO: Completar durante la fase de análisis**). **Identidad:** **Credenciales técnicas hacia APIs de LLM, base vectorial, colas y almacenamiento** solo en la medida en que el IdP emplee secretos de cliente hacia la plataforma (si aplica al diseño). |
| **Justificación** | El escenario exige **OAuth2 federado**; sin esta frontera no queda explícita la dependencia de un actor externo para el control de acceso al ecosistema. |

---

### TB-04 — Servicios de aplicación ↔ Infraestructura de mensajería (RabbitMQ)

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-04 — API / publicadores ↔ Broker y mensajería (RabbitMQ) |
| **Componentes involucrados** | **Servicios de aplicación / API** (publicación de trabajos de ingestión u otros eventos); **Broker y mensajería (RabbitMQ)**. |
| **Descripción** | Paso de solicitudes de trabajo desde la capa síncrona de aplicación hacia la cola que alimenta el pipeline ETL asíncrono descrito en el escenario. |
| **Motivo del cambio de confianza** | Cambia el modelo de acoplamiento temporal y la superficie de persistencia intermedia: la cola conserva o referencia trabajos con posible contenido sensible o metadatos; sus políticas de acceso no son idénticas a las de la API HTTP. |
| **Activos que atraviesan la frontera** | **Proceso:** Proceso de ingestión y preparación de datos (pipeline ETL asíncrono) — en forma de mensajes que disparan o describen el trabajo. **Información:** Referencias o cargas útiles de mensajes que identifiquen documentos (**Documentos contractuales…**, **Documentación legal de referencia**) y metadatos asociados a la indexación. **Identidad:** **Credenciales técnicas hacia APIs de LLM, base vectorial, colas y almacenamiento** usadas por componentes que publican o consumen. |
| **Justificación** | RabbitMQ es un componente explícito del escenario; delimitar esta frontera separa la confianza en la lógica de solicitud HTTP de la confianza en la mensajería y en los consumidores posteriores. |

---

### TB-05 — Workers del pipeline ETL ↔ Almacén de documentos y plantillas

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-05 — Procesamiento de ingestión ↔ Almacén de documentos y plantillas |
| **Componentes involucrados** | Procesadores del **Proceso de ingestión y preparación de datos (pipeline ETL asíncrono)** (consumidores de RabbitMQ); **Almacén de documentos y plantillas (ficheros u objeto)**; **Broker y mensajería (RabbitMQ)** como canal de activación. |
| **Descripción** | Lectura (y posible escritura de estados) de archivos fuente PDF/Word y plantillas institucionales durante la preparación de datos para embeddings. |
| **Motivo del cambio de confianza** | El almacén concentra confidencialidad e integridad de documentos a largo plazo; los workers tienen privilegios de lectura amplia sobre ese repositorio lógico distinto del resto de la aplicación interactiva. |
| **Activos que atraviesan la frontera** | **Información:** Documentos contractuales y otros documentos legales cargados por clientes (PDF/Word); Documentación legal de referencia; Plantillas institucionales de contratos comerciales. **Infraestructura:** Almacén de documentos y plantillas. **Proceso:** Proceso de ingestión y preparación de datos (pipeline ETL asíncrono). **Identidad:** Credenciales técnicas hacia… almacenamiento. |
| **Justificación** | El escenario combina **carga de contratos** y **plantillas** con un **pipeline ETL**; esta frontera documenta dónde el procesamiento batch toca el repositorio de archivos, distinto del canal interactivo usuario–API. |

---

### TB-06 — Pipeline ETL y orquestación ↔ Base de datos vectorial

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-06 — Componentes de indexación y consulta ↔ Base de datos vectorial (Chroma o Pinecone) |
| **Componentes involucrados** | **Proceso de ingestión y preparación de datos (pipeline ETL asíncrono)** (escritura de embeddings); **Orquestador de IA (LangChain / LlamaIndex en Python)** y/o **Servicios de aplicación / API** en fase de consulta (lectura para RAG); **Base de datos vectorial (Chroma o Pinecone)**. |
| **Descripción** | Lectura y escritura del índice de similitud que soporta el RAG, incluyendo vectores y metadatos asociados. |
| **Motivo del cambio de confianza** | El índice agrega representaciones derivadas de documentos confidenciales; además, si la implementación usa **Pinecone** u otro SaaS, existe un cambio respecto del control físico/lógico respecto de instalación totalmente bajo la firma (p. ej. Chroma on-prem). |
| **Activos que atraviesan la frontera** | **Información:** Representación indexada para recuperación (fragmentos, metadatos y contenido asociado a embeddings); enlace lógico con **Documentos contractuales…** y **Documentación legal de referencia** como fuentes. **Servicio / Infraestructura:** Base de datos vectorial (Chroma o Pinecone). **Proceso:** Proceso de ingestión…; **Proceso de consulta RAG (recuperación + generación)** en la parte de recuperación. **Identidad:** Credenciales técnicas hacia… base vectorial. |
| **Justificación** | El escenario define explícitamente la **base vectorial** y el **RAG**; esta frontera separa el tratamiento en motores de IA del repositorio de similitud, punto central para recuperación de contexto. |

---

### TB-07 — Orquestador de IA ↔ Integración con LLM comerciales (API)

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-07 — Perímetro de la firma ↔ Proveedor LLM en la nube (OpenAI / Anthropic) |
| **Componentes involucrados** | **Orquestador de IA (LangChain / LlamaIndex en Python)**; **Entorno de ejecución Python**; **Integración con LLM comerciales (API OpenAI / Anthropic)** (servicio externo). |
| **Descripción** | Invocación remota de inferencia: se envían prompts y, en la práctica del RAG, **contexto recuperado** desde la base vectorial hacia un tercero, según políticas de uso aún no detalladas en el escenario. |
| **Motivo del cambio de confianza** | El proveedor comercial opera fuera del perímetro de control de la firma; cambian obligaciones contractuales, visibilidad del dato y superficie de dependencia de terceros respecto del procesamiento local. |
| **Activos que atraviesan la frontera** | **Información:** Consultas en lenguaje natural de usuarios; Representación indexada para recuperación (como contexto en el prompt); Respuestas del sistema basadas en RAG (vuelta); Borradores preliminares… (si el agente usa API comercial); Resultados de la auditoría automatizada… (si aplica el mismo canal); fragmentos de **Documentos contractuales…** / **Documentación legal de referencia** presentes en el contexto enviado. **Servicio:** Integración con LLM comerciales. **Identidad:** Credenciales técnicas hacia APIs de LLM…. **Proceso:** Proceso de consulta RAG…; Proceso de generación de borradores…; Proceso de auditoría automatizada… (cuando utilicen el LLM comercial). |
| **Justificación** | El escenario contempla explícitamente **API con LLM comercial externo**; es la frontera más sensible entre datos legales confidenciales y un operador ajeno, independientemente de amenazas concretas. |

---

### TB-08 — Orquestador de IA ↔ Servicio de inferencia LLM open source local

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-08 — Orquestador ↔ Subsistema de inferencia local (LLM OSS) |
| **Componentes involucrados** | **Orquestador de IA (LangChain / LlamaIndex en Python)**; **Servicio de inferencia con LLM open source en instalación local**. |
| **Descripción** | Canal de inferencia bajo control de la firma para **datos altamente sensibles**, según el escenario, alternativo o complementario al LLM comercial. |
| **Motivo del cambio de confianza** | Aunque ambos lados suelen ser “internos”, cambia el límite de confianza entre el **código de orquestación** (cadena de herramientas, prompts dinámicos) y el **runtime del modelo** (pesos, GPU/servidor de inferencia, versiones de modelo). Son equipos y ciclos de vida distintos. |
| **Activos que atraviesan la frontera** | **Información:** Mismas categorías que en TB-07 cuando el diseño enrute por el LLM local (consultas, contexto RAG, borradores, auditoría). **Servicio:** Servicio de inferencia con LLM open source en instalación local. **Software:** Orquestador de IA. **Identidad:** Credenciales técnicas entre orquestador y servicio de inferencia (si aplica). |
| **Justificación** | El escenario distingue explícitamente **LLM local** para sensibilidad alta; documentar la frontera evita fusionar indebidamente “todo lo interno” en un solo dominio de confianza. |

---

### TB-09 — Rol “cliente de la firma” ↔ Rol “personal interno” (dominio de autorización lógico)

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-09 — Separación lógica de confianza entre cliente externo y personal interno |
| **Componentes involucrados** | Mismos **Servicios de aplicación / API** y **Aplicación frontend institucional (Next.js)**; políticas de autorización (detalle: **TODO: Completar durante la fase de análisis**); **Identidades federadas de usuarios (OAuth2)** con distinción de roles o *claims*. |
| **Descripción** | El escenario describe un ecosistema **interno y orientado a clientes**; en el modelo lógico conviene explicitar la frontera de **mandato y acceso** entre dos clases de sujetos que usan la misma plataforma. |
| **Motivo del cambio de confianza** | Cambia el nivel de privilegio esperado y el conjunto de documentos y funciones accesibles (p. ej. corpus interno frente a cargas de un solo cliente), aunque el software sea compartido. |
| **Activos que atraviesan la frontera** | **Información:** Prácticamente todos los activos de información del inventario, según alcance por rol (documentos de clientes vs corpus interno; resultados de auditoría visibles solo a internos, etc. — matriz de acceso: **TODO: Completar durante la fase de análisis**). **Identidad:** Identidades federadas de usuarios (OAuth2). **Proceso:** Proceso de consulta RAG…; Proceso de generación de borradores…; Proceso de auditoría automatizada… (según qué rol puede iniciarlos). |
| **Justificación** | No introduce un componente nuevo; formaliza una frontera de **negocio y gobernanza** exigida por el alcance “cliente + interno” del escenario y necesaria antes de modelar abusos de privilegio en fases posteriores. |

---

### TB-10 — (Condicional) Infraestructura bajo control de la firma ↔ Base vectorial gestionada por tercero

| Campo | Contenido |
|--------|-----------|
| **Nombre** | TB-10 — Operador de la plataforma ↔ Proveedor SaaS de base vectorial (*solo si aplica Pinecone u otro servicio gestionado*) |
| **Componentes involucrados** | **Servicios de aplicación / API**, **Orquestador de IA**, **Proceso de ingestión…** (según quién escriba en el índice); **Base de datos vectorial** en modalidad **servicio gestionado externo** (p. ej. Pinecone según el escenario). Si el despliegue usa **solo Chroma** bajo control de la firma, esta frontera **no aplica** y debe marcarse como fuera de modelo o fusionarse con TB-06. |
| **Descripción** | Cuando el índice vectorial reside en infraestructura de un tercero, los embeddings y metadatos salen del perímetro operativo directo de la firma hacia el proveedor del servicio. |
| **Motivo del cambio de confianza** | Cambian responsabilidad operativa, ubicación lógica del dato y cadena de subprocesadores respecto de una instalación enteramente on-prem. |
| **Activos que atraviesan la frontera** | **Información:** Representación indexada para recuperación… **Servicio / Infraestructura:** Base de datos vectorial (Chroma o Pinecone) — en la variante Pinecone. **Identidad:** Credenciales técnicas hacia… base vectorial. |
| **Justificación** | El escenario permite **Chroma o Pinecone**; Pinecone implica explícitamente un salto de confianza hacia proveedor externo adicional respecto de muchos despliegues de Chroma. Documentarlo como **condicional** evita inventar un despliegue único. |

---

## Relación con otros entregables

| Documento | Relación |
|-----------|----------|
| [`01-inventario-activos.md`](01-inventario-activos.md) | Nombres y tipos de activos citados en cada TB. |
| [`../diagrams/`](../diagrams/) | Los flujos entre procesos y almacenes deberán reflejar estas fronteras en el diagrama de arquitectura. |
| [`03-analisis-stride.md`](03-analisis-stride.md) | Catálogo STRIDE revisado (63 IDs); ver apéndice de consolidaciones. |

---

## Trazabilidad

- Inventario de activos: [`01-inventario-activos.md`](01-inventario-activos.md)
- Diagramas: [`../diagrams/`](../diagrams/)
- Memory bank: [`../prompts/00-memory-bank.md`](../prompts/00-memory-bank.md)
- Registro de prompt (IA): [`../prompts/05-prompt-trust-boundaries.md`](../prompts/05-prompt-trust-boundaries.md)
