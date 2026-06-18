### Escenario 11: Plataforma de Asistencia Legal basada en IA Generativa (RAG)



Presentación: jueves 18 de Junio

**Contexto:**
Una firma de servicios profesionales está desarrollando un ecosistema interno y orientado a clientes para la consulta automatizada de documentos legales, contratos y jurisprudencia utilizando Modelos de Lenguaje Grande (LLMs). El sistema permite:

* Carga de contratos confidenciales en formatos PDF/Word por parte de clientes.
* Consultas en lenguaje natural con respuestas fundamentadas en la documentación cargada (arquitectura RAG).
* Agentes autónomos de IA que redactan borradores preliminares de contratos comerciales basados en plantillas institucionales.
* Auditoría automatizada de cumplimiento normativo local.

**Alcance del análisis:**

* Orquestador de IA (LangChain / LlamaIndex) sobre Python.
* Base de datos vectorial (Vector DB como Chroma o Pinecone) para el almacenamiento de *embeddings*.
* Integración vía API con LLM comercial externo (OpenAI/Anthropic) y LLM *open-source* hospedado localmente para datos altamente sensibles.
* Frontend institucional en Next.js con autenticación federada (OAuth2).
* Pipeline de ingesta de datos (ETL) asincrónico basado en colas de mensajería (RabbitMQ).

**Plantillas y Enlaces de Ayuda:**

* **OWASP Top 10 para LLM:** [OWASP Top 10 for LLM Applications v1.1](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
* **Vectores de Ataque en IA:** [MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems)](https://atlas.mitre.org/)
* **Protección de Datos:** [Agesic - Guía de Evaluación de Impacto en Protección de Datos Personales](https://www.google.com/search?q=https://www.gub.uy/agencia-gobierno-electronico-sociedad-informacion-conocimiento/comunicacion/publicaciones/guia-para-evaluacion-de-impacto-en-la-proteccion-de-datos-personales)
* **Controles de IA:** [NIST AI Risk Management Framework (AI RMF 1.0)](https://www.nist.gov/itl/ai-risk-management-framework)

**Actividades sugeridas:**

1. Identificar activos críticos y clasificar la sensibilidad de los datos de entrenamiento/contexto (secreto comercial, datos personales según Ley 18.331).
2. Trazar los límites de confianza (*trust boundaries*) específicamente en la frontera entre la red interna, la base de datos vectorial y las llamadas a APIs de LLM externas.
3. Aplicar STRIDE adaptando las categorías a sistemas inteligentes (ej. *Spoofing* de identidad en prompts, *Tampering* de la base de datos vectorial mediante inyección de datos falsos).
4. Analizar el riesgo específico de la técnica **Prompt Injection** (directa e indirecta) y cómo puede derivar en la exfiltración de documentos de otros clientes (*Data Leakage*).
5. Evaluar los controles de acceso basados en roles (RBAC) aplicados a los resultados del recuperador vectorial (*vector retrieval stage*) antes de enviar el contexto al LLM.

**Entregables extra sugeridos**

* Diagrama de flujo de datos (DFD) identificando el pipeline de RAG y las llamadas a modelos externos.
* Threat Modeling de OWASP LLM Top 10 cruzado con la matriz STRIDE.
* Matriz de mitigación priorizada basada en el AI RMF de NIST o controles NIST SP 800-53 (Familia MP - Media Protection y AC - Access Control).
* Plan de contingencia ante fallos de alineación del modelo o respuestas alucinadas con impacto legal.