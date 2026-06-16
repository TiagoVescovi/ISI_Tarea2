# Inventario de activos

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Base:** enunciado del escenario y arquitectura lógica validada (Next.js, OAuth2 federado, API/servicios de aplicación, RabbitMQ y pipeline ETL, orquestador LangChain/LlamaIndex, base vectorial Chroma o Pinecone, integración API con LLM comercial OpenAI/Anthropic, LLM open source local para datos altamente sensibles, almacén de documentos y plantillas).

**Alcance de esta versión:** inventario para modelado posterior de amenazas. **No** incluye STRIDE, DREAD, mitigaciones ni catalogación de amenazas.

**Criticidad preliminar:** valoración cualitativa (*Alta* / *Media* / *Baja*) según importancia para el negocio jurídico y la protección de datos descrita en el escenario; **no** constituye análisis de riesgo ni puntuación DREAD.

**Impacto CIA:** grado *potencial* de daño a confidencialidad (C), integridad (I) y disponibilidad (A) si el activo se viera afectado, a nivel descriptivo (*Alta* / *Media* / *Baja*).

---

## Leyenda de tipos

| Tipo | Uso en este inventario |
|------|-------------------------|
| Información | Datos, contenidos o productos del tratamiento (incluye derivados relevantes como respuestas o borradores). |
| Software | Componentes implementados bajo control de la firma (código, aplicaciones). |
| Servicio | Capacidad expuesta o consumida (interna o de terceros) con interfaz clara. |
| Infraestructura | Plataforma de ejecución o almacenamiento que soporta otros activos. |
| Identidad | Sujetos, cuentas, atributos de identidad o credenciales técnicas de acceso. |
| Proceso | Secuencia automatizada de tratamiento o decisión con valor para el negocio. |

---

## 1. Activos de información

| Nombre del activo | Tipo | Descripción | Por qué es relevante | Criticidad preliminar | Impacto potencial C | I | A |
|-------------------|------|-------------|------------------------|------------------------|---------------------|---|---|
| Documentos contractuales y otros documentos legales cargados por clientes (PDF/Word) | Información | Contenido confidencial aportado por clientes para consulta y tratamiento en la plataforma. | Es el insumo sensible explícito del escenario; sustenta RAG y otros flujos. | Alta | Alta | Alta | Media |
| Documentación legal de referencia (contratos, jurisprudencia y corpus interno para consulta) | Información | Conjunto de documentos legales y jurisprudencia sobre los que opera la consulta automatizada. | Define la calidad y el alcance de las respuestas fundamentadas; activo intelectual y contractual de la firma. | Alta | Alta | Alta | Media |
| Consultas en lenguaje natural de usuarios | Información | Preguntas formuladas por usuarios internos o clientes al sistema. | Pueden revelar estrategia, asuntos sensibles o datos personales; alimentan el RAG y los LLM. | Alta | Alta | Media | Baja |
| Respuestas del sistema basadas en RAG | Información | Salidas textuales atadas a la documentación recuperada. | Actúan como asesoramiento automatizado; errores o filtraciones impactan directamente a clientes y responsabilidad profesional. | Alta | Alta | Alta | Media |
| Borradores preliminares de contratos comerciales generados por agentes de IA | Información | Versiones iniciales de contratos producidas a partir de plantillas institucionales. | Combina datos del caso y plantillas; alto impacto legal y comercial si se alteran o exponen indebidamente. | Alta | Alta | Alta | Media |
| Resultados de la auditoría automatizada de cumplimiento normativo local | Información | Salidas del módulo de auditoría (dictámenes, hallazgos o marcadores según implementación futura). | Condicionan decisiones de cumplimiento; deben ser íntegros y acotados en divulgación. | Alta | Alta | Alta | Media |
| Plantillas institucionales de contratos comerciales | Información | Modelos corporativos usados por los agentes para redactar borradores. | Propiedad intelectual y estándar de negocio; su alteración afecta todos los borradores generados. | Alta | Media | Alta | Media |
| Representación indexada para recuperación (fragmentos, metadatos y contenido asociado a embeddings) | Información | Unidad lógica de texto y metadatos vinculados al índice vectorial derivados de documentos fuente. | Lo que realmente se expone al modelo en RAG; acumula confidencialidad del corpus y de cargas de clientes. | Alta | Alta | Alta | Media |

---

## 2. Activos tecnológicos (software, servicios e infraestructura)

| Nombre del activo | Tipo | Descripción | Por qué es relevante | Criticidad preliminar | Impacto potencial C | I | A |
|-------------------|------|-------------|------------------------|------------------------|---------------------|---|---|
| Aplicación frontend institucional (Next.js) | Software | Interfaz para clientes y personal frente al ecosistema. | Punto de interacción con cargas, consultas y resultados; su integridad y disponibilidad condicionan todo el uso. | Alta | Media | Alta | Alta |
| Servicios de aplicación / API (capa entre UI y subsistemas) | Software | Lógica de negocio, autorización de servidor y acoplamiento a cola, almacenes y orquestador (*hipótesis mínima* de arquitectura validada). | Centraliza control de flujos y datos entre componentes nombrados en el escenario. | Alta | Media | Alta | Alta |
| Orquestador de IA (LangChain / LlamaIndex en Python) | Software | Componente que encadena recuperación, invocación de modelos y flujos de agentes. | Corazón del tratamiento de IA descrito; un fallo afecta a todos los casos de uso de IA. | Alta | Media | Alta | Alta |
| Broker y mensajería (RabbitMQ) | Infraestructura | Sistema de colas para el pipeline ETL asíncrono. | Desacopla ingestión; pérdida o lectura indebida de mensajes afecta confidencialidad de metadatos y orden de procesamiento. | Media | Media | Media | Alta |
| Base de datos vectorial (Chroma o Pinecone) | Servicio / Infraestructura | Almacenamiento de embeddings y datos asociados para búsqueda por similitud. | Persistencia del índice RAG; vector DB externa implica tercero de confianza adicional (según despliegue). | Alta | Alta | Alta | Alta |
| Almacén de documentos y plantillas (ficheros u objeto) | Infraestructura | Repositorio lógico de PDF/Word cargados y plantillas (*hipótesis mínima* para soportar carga y agentes). | Residencia de contratos confidenciales y plantillas; pérdida o corrupción impacta continuidad y prueba documental. | Alta | Alta | Alta | Media |
| Integración con LLM comerciales (API OpenAI / Anthropic) | Servicio | Acceso remoto a modelos de terceros para inferencia. | Canal por el que puede salir contexto sensible hacia proveedor externo según configuración de uso. | Alta | Alta | Media | Media |
| Servicio de inferencia con LLM open source en instalación local | Servicio | Modelo y runtime bajo control local para datos altamente sensibles, según el escenario. | Mitiga exposición a terceros para un subconjunto de casos; su disponibilidad es crítica para esa política. | Alta | Alta | Media | Media |
| Entorno de ejecución Python y dependencias del orquestador | Infraestructura | Plataforma donde corre el código Python del orquestador. | Base de confianza para integridad del procesamiento de IA y llamadas a backend. | Media | Media | Alta | Alta |

---

## 3. Activos de identidad y credenciales

| Nombre del activo | Tipo | Descripción | Por qué es relevante | Criticidad preliminar | Impacto potencial C | I | A |
|-------------------|------|-------------|------------------------|------------------------|---------------------|---|---|
| Identidades federadas de usuarios (OAuth2) | Identidad | Cuentas y atributos de autenticación/autorización de clientes y personal, mediados por federación. | Control de acceso al ecosistema y trazabilidad de acciones; pieza explícita del escenario. | Alta | Media | Alta | Media |
| Proveedor de identidad federado (IdP) | Servicio | Servicio externo o corporativo que emite tokens OAuth2 (*hipótesis mínima* ligada a “federada”). | Tercero de confianza para validez de identidades; indisponibilidad bloquea accesos. | Alta | Media | Media | Alta |
| Credenciales técnicas hacia APIs de LLM, base vectorial, colas y almacenamiento | Identidad | Secretos y llaves usados por los servicios para integrarse entre sí y con terceros. | Compromiso implica acceso amplio a datos y costos; son superficie clave aunque no estén nombradas literalmente en el escenario, son inherentes a las integraciones descritas. | Alta | Alta | Media | Baja |

---

## 4. Activos de tipo proceso

| Nombre del activo | Tipo | Descripción | Por qué es relevante | Criticidad preliminar | Impacto potencial C | I | A |
|-------------------|------|-------------|------------------------|------------------------|---------------------|---|---|
| Proceso de ingestión y preparación de datos (pipeline ETL asíncrono) | Proceso | Cadena desde recepción de documentos hasta generación y carga de embeddings en la base vectorial. | Garantiza que el RAG refleje documentos autorizados; errores introducen desalineación o filtraciones lógicas. | Alta | Media | Alta | Media |
| Proceso de consulta RAG (recuperación + generación) | Proceso | Flujo de pregunta → búsqueda en vector DB → construcción de contexto → invocación de LLM. | Núcleo del valor y del riesgo de divulgación indebida de fragmentos recuperados. | Alta | Alta | Alta | Media |
| Proceso de generación de borradores contractuales por agentes de IA | Proceso | Uso de plantillas y modelos para producir borradores preliminares. | Automatiza output legalmente relevante; requiere coherencia con políticas internas. | Alta | Alta | Alta | Media |
| Proceso de auditoría automatizada de cumplimiento normativo local | Proceso | Evaluación automatizada respecto de normativa local (detalle de fuentes no especificado en el escenario). | Impacta decisiones de cumplimiento; integridad y confidencialidad del resultado son centrales. | Alta | Alta | Alta | Media |

---

## 5. Supuestos de arquitectura reflejados aquí

Los siguientes elementos **no** están nombrados literalmente en el escenario pero forman parte de la **arquitectura validada** en el trabajo y se incluyen solo como activos mínimos necesarios:

- Servicios de aplicación / API entre el frontend y los demás subsistemas.
- Almacén de ficheros para documentos y plantillas.
- Workers del ETL como manifestación del “pipeline ETL asíncrono”.
- Proveedor IdP asociado a OAuth2 federado.
- Credenciales técnicas de integración entre componentes y con terceros.

---

## 6. Pendientes para refinar el inventario (sin inventar en esta etapa)

**TODO: Completar durante la fase de análisis** — según consigna y decisiones de modelado:

- Separar activos por **tenant** (cliente vs firma) si el enunciado o el diseño lo exigen.
- Detallar formatos de salida de auditoría y de borradores cuando exista diseño.
- Acotar si Chroma y Pinecone se modelan como alternativas excluyentes o coexistencia.
- Añadir activos adicionales solo si surgen de nueva información oficial (p. ej. bases transaccionales explícitas, KMS, etc.).

## Trazabilidad

- Índice: [`00-indice-documentos.md`](00-indice-documentos.md) (E01)
- Arquitectura y trust boundaries: [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md)
- Memory bank: [`../prompts/00-memory-bank.md`](../prompts/00-memory-bank.md)
