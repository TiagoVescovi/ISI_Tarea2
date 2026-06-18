# Inventario de activos

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

La plataforma consolida documentación legal confidencial, consultas en lenguaje natural con respuestas RAG, borradores generados por agentes de IA y auditoría de cumplimiento. Los activos listados son los de **mayor criticidad** para el análisis de riesgos.

**Alcance:** inventario para threat modeling posterior. No incluye STRIDE, DREAD ni mitigaciones.

---

## Activos de información

Datos generados y procesados por la firma; alta criticidad por impacto en confidencialidad, responsabilidad legal y secreto comercial.

| Activo | Tipo | Descripción | Criticidad | Justificación |
|--------|------|-------------|------------|---------------|
| Documentos contractuales y legales cargados | Información | PDF/Word aportados por clientes para consulta y tratamiento | Alta | Insumo sensible del escenario; sustenta RAG y borradores |
| Documentación legal de referencia | Información | Corpus interno, jurisprudencia y plantillas de consulta | Alta | Activo intelectual de la firma; define calidad de respuestas |
| Consultas de usuarios | Información | Preguntas en lenguaje natural de clientes e internos | Alta | Pueden revelar estrategia o datos personales |
| Respuestas RAG | Información | Salidas textuales atadas a documentación recuperada | Alta | Actúan como asesoramiento automatizado |
| Borradores de contratos generados por IA | Información | Versiones preliminares a partir de plantillas | Alta | Alto impacto legal y comercial |
| Resultados de auditoría de cumplimiento | Información | Hallazgos y dictámenes del módulo de auditoría | Alta | Condicionan decisiones de cumplimiento |
| Índice vectorial (embeddings y metadatos) | Información | Fragmentos indexados para recuperación RAG | Alta | Es lo que realmente expone el modelo en inferencia |

---

## Activos tecnológicos

Componentes que almacenan, procesan o transmiten datos legales sensibles.

| Activo | Tipo | Descripción | Criticidad | Justificación |
|--------|------|-------------|------------|---------------|
| Aplicación frontend (Next.js) | Software | Interfaz para clientes y personal interno | Alta | Punto de acceso a cargas, consultas y resultados |
| Servicios de aplicación / API | Software | Lógica de negocio, autorización y acoplamiento a subsistemas | Alta | Centraliza flujos entre UI, cola, almacenes y orquestador |
| Orquestador de IA (LangChain / LlamaIndex) | Software | Encadena recuperación, invocación de LLM y agentes | Alta | Núcleo del tratamiento de IA del escenario |
| Base de datos vectorial (Chroma o Pinecone) | Infraestructura | Almacenamiento de embeddings para búsqueda semántica | Alta | Persistencia del índice RAG; Pinecone implica tercero |
| Almacén de documentos y plantillas | Infraestructura | Repositorio de PDF/Word y plantillas institucionales | Alta | Residencia de contratos confidenciales |
| Integración LLM comercial (OpenAI / Anthropic) | Servicio | Inferencia en la nube | Alta | Canal de posible exfiltración de contexto sensible |
| LLM local (open source on-prem) | Servicio | Inferencia local para datos altamente sensibles | Alta | Mitiga exposición a terceros en subconjunto de casos |
| RabbitMQ y pipeline ETL | Infraestructura | Cola y procesamiento asíncrono de ingestión | Media | Desacopla carga documental hacia el índice |

---

## Activos de identidad

| Activo | Tipo | Descripción | Criticidad | Justificación |
|--------|------|-------------|------------|---------------|
| Identidades federadas (OAuth2) | Identidad | Cuentas de clientes y personal vía federación | Alta | Control de acceso y trazabilidad |
| Proveedor de identidad (IdP) | Servicio | Emisor de tokens OAuth2 | Alta | Tercero de confianza para autenticación |
| Credenciales técnicas | Identidad | Secretos hacia APIs, vector DB, colas y LLMs | Alta | Compromiso habilita acceso amplio a datos |

---

## Activos de proceso

| Activo | Tipo | Descripción | Criticidad | Justificación |
|--------|------|-------------|------------|---------------|
| Proceso de consulta RAG | Proceso | Pregunta → recuperación → contexto → LLM | Alta | Núcleo del valor y del riesgo de divulgación |
| Proceso de generación de borradores y auditoría | Proceso | Agentes de IA sobre plantillas y normativa | Alta | Automatiza salidas legalmente relevantes |

---

## Activos humanos

Personas y organizaciones que operan el sistema, acceden a datos legales sensibles o participan como terceros de confianza. En un entorno legal, el factor humano condiciona confidencialidad, integridad del asesoramiento y responsabilidad profesional.

| Activo | Tipo | Descripción | Criticidad | Justificación |
|--------|------|-------------|------------|---------------|
| Abogados y personal legal | Personas | Profesionales que consultan RAG, revisan borradores, validan auditorías y cargan documentación de casos | Alta | Sus acciones impactan directamente la calidad y confidencialidad del asesoramiento; error o acceso indebido afecta responsabilidad legal |
| Personal de TI y administradores | Personas | Equipo que administra servidores, API, orquestador, credenciales, colas y configuración de LLM/vector DB | Alta | Privilegios elevados sobre infraestructura y datos; compromiso o abuso puede afectar confidencialidad y disponibilidad de toda la plataforma |
| Clientes externos | Personas | Usuarios de empresas cliente que cargan contratos y realizan consultas en su tenant | Alta | Manejan documentación confidencial propia; son vector de riesgo por error, filtración involuntaria o cuenta comprometida |
| Personal administrativo de la firma | Personas | Staff de soporte, onboarding o gestión documental sin rol legal pleno | Media | Acceso acotado pero puede tratar metadatos o datos identificatorios; menor exposición que abogados o clientes |
| Proveedor cloud / LLM (cadena de suministro) | Personas | Organización y personal detrás de servicios SaaS (OpenAI, Anthropic, Pinecone u otros) que procesan o almacenan datos según contrato | Alta | Actor externo; el contexto legal enviado a inferencia o índice SaaS queda fuera del control directo de la firma — riesgo de cadena de suministro y cumplimiento |

---

## Trazabilidad

| Documento | Relación |
|-----------|----------|
| [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md) | Componentes y fronteras que materializan estos activos |
| [`03-analisis-stride.md`](03-analisis-stride.md) | Amenazas sobre activos, componentes y actores humanos |
| [`../prompts/16-prompt-activos-humanos.md`](../prompts/16-prompt-activos-humanos.md) | Ampliación del inventario — activos humanos |
| [`../diagrams/arquitectura.png`](../diagrams/arquitectura.png) | Vista de arquitectura |
