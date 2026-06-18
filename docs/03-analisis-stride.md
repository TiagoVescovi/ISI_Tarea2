# Análisis STRIDE

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`01-inventario-activos.md`](01-inventario-activos.md), [`02-arquitectura-trust-boundaries.md`](02-arquitectura-trust-boundaries.md).

El análisis STRIDE identifica amenazas sobre los componentes principales del flujo: desde el usuario hasta el backend que orquesta RAG, ingestión y agentes de IA. El foco está en datos legales confidenciales y vectores propios de sistemas con LLM y recuperación vectorial.

**Alcance:** amenazas priorizadas para evaluación DREAD. No incluye puntuación DREAD ni riesgo residual.

---

## Alcance del análisis

Se analiza el flujo que va del **usuario** (cliente o interno) al **frontend Next.js**, la **API y orquestador de IA**, y las integraciones con **almacén, base vectorial y LLMs**. Las fronteras TB-01 a TB-04 delimitan los cruces de confianza relevantes.

---

## Componente: Usuario (cliente o personal)

**Tipo:** Agente externo al perímetro de la firma.

| | | Riesgo | Amenaza | Descripción del ataque | Mitigaciones técnicas |
|---|---|---|---|---|---|
| S | Spoofing | Alto | Robo de credenciales / sesión | Phishing o robo de token OAuth2 para operar como otro usuario y acceder a contratos ajenos. | MFA; detección de login anómalo; cookies HttpOnly+Secure+SameSite; no almacenar tokens en localStorage |
| T | Tampering | Alto | Manipulación de requests (IDOR) | Modificación de parámetros en el navegador para acceder a documentos o consultas de otro cliente (multi-tenant). | Validación server-side; autorización por tenant en API |
| R | Repudiation | Medio | Negación de acciones | Usuario niega haber cargado un documento o ejecutado una consulta sin trazabilidad suficiente. | Logs de auditoría; timestamps en operaciones críticas |
| I | Information Disclosure | Alto | Exposición por sesión abierta | Sesión activa en dispositivo compartido expone contratos y respuestas legales. | Timeout automático; cierre de sesión en servidor |
| D | Denial of Service | Medio | Bloqueo de cuenta | Intentos repetidos de login bloquean acceso en momento crítico. | CAPTCHA; lockout progresivo; notificación al usuario |

---

## Componente: Frontend (Next.js)

**Tipo:** Interfaz web.

| | | Riesgo | Amenaza | Descripción del ataque | Mitigaciones técnicas |
|---|---|---|---|---|---|
| S | Spoofing | Alto | Secuestro de sesión (XSS) | Script malicioso roba token de sesión mal almacenado en el navegador. | Cookies HttpOnly; CSP estricta; sanitización con DOMPurify |
| T | Tampering | Alto | Dependencias comprometidas | Librería de terceros altera formularios o redirige datos legales. | SRI; auditoría de dependencias; versionado fijo |
| R | Repudiation | Medio | Acciones sin auditoría server-side | Operaciones críticas solo registradas en cliente. | Toda acción relevante genera evento en backend |
| I | Information Disclosure | Alto | Over-fetching de datos legales | API devuelve más campos de los necesarios; datos en source maps o bundle. | Minimización de datos; sin source maps en producción |
| D | Denial of Service | Medio | Clickjacking | Portal embebido en iframe malicioso. | X-Frame-Options DENY; CSP frame-ancestors 'none' |
| E | Elevation of Privilege | Alto | XSS almacenado con rol interno | Payload visible para abogados se ejecuta con permisos elevados. | Sanitización de salida; separación de sesiones por perfil |

---

## Componente: Backend (API + Orquestador + ETL)

**Tipo:** Servicio central.

| | | Riesgo | Amenaza | Descripción del ataque | Mitigaciones técnicas |
|---|---|---|---|---|---|
| S | Spoofing | Alto | Token o servicio suplantado | Invocación de API con JWT falsificado o publicación fraudulenta en cola ETL. | Validación criptográfica de tokens; autenticación entre servicios |
| T | Tampering | Crítico | Prompt injection en consulta RAG | Usuario inyecta instrucciones en la consulta que alteran el comportamiento del LLM o recuperan datos no autorizados. | Validación y sanitización de entradas; filtros en recuperación vectorial |
| T | Tampering | Alto | Data poisoning en ingesta | Documento malicioso cargado corrompe el índice vectorial y envenena respuestas futuras. | Validación de archivos; revisión de ingesta; aislamiento por tenant |
| R | Repudiation | Alto | Inferencias sin trazabilidad | Consulta RAG o borrador generado sin registro auditable de contexto y modelo usado. | Logging de prompts, fragmentos recuperados y modelo invocado |
| I | Information Disclosure | Crítico | Exfiltración de contexto hacia LLM cloud | Fragmentos de contratos enviados al LLM comercial quedan expuestos al proveedor. | Enrutar datos sensibles al LLM local; minimizar contexto enviado |
| I | Information Disclosure | Alto | Retrieval leakage | Recuperación vectorial devuelve fragmentos de otro cliente por fallo de filtrado. | Filtros estrictos por tenant en búsqueda; pruebas de aislamiento |
| D | Denial of Service | Alto | Agotamiento de recursos | Volumen masivo de consultas o cargas agota API, cola u orquestador. | Rate limiting; límites de concurrencia; circuit breaker |
| E | Elevation of Privilege | Crítico | Fallo de autorización multi-tenant | Bug en API permite a un cliente acceder al corpus de otro. | RBAC; pruebas de autorización; mínimo privilegio |

---

## Resumen por componente

| Componente | Total amenazas | Críticas | Altas | Medias | Riesgo global |
|------------|---------------:|---------:|------:|-------:|---------------|
| Usuario | 5 | 0 | 3 | 2 | Alto |
| Frontend Next.js | 6 | 0 | 4 | 2 | Alto |
| Backend plataforma | 8 | 3 | 4 | 1 | Crítico |
| **Total** | **19** | **3** | **11** | **5** | **Crítico** |

---

## Trazabilidad

| Documento | Relación |
|-----------|----------|
| [`04-priorizacion-dread.md`](04-priorizacion-dread.md) | 14 amenazas evaluadas con DREAD |
| [`05-mapa-attack.md`](05-mapa-attack.md) | Mapeo a técnicas MITRE ATT&CK |
