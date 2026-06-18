# Plan de mitigación

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`03-analisis-stride.md`](03-analisis-stride.md), [`04-priorizacion-dread.md`](04-priorizacion-dread.md), [`05-mapa-attack.md`](05-mapa-attack.md).

Plan de tratamiento del riesgo para las **14** amenazas priorizadas con DREAD. La priorización sigue el puntaje DREAD: críticas/altas en corto plazo; medias en sprint siguiente.

**Alcance:** no incluye cuantificación del riesgo residual (ver [`07-riesgos-residuales.md`](07-riesgos-residuales.md)).

---

## Criterio de priorización

| Puntaje DREAD | Severidad | Tratamiento |
|---------------|-----------|-------------|
| 9,0 – 10,0 | Crítico | Mitigación inmediata |
| 7,0 – 8,9 | Alto | Mitigación prioritaria en corto plazo |
| 5,0 – 6,9 | Medio | Mitigación planificada |

---

## Matriz de prioridad de amenazas

| Prioridad | ID | Amenaza | Severidad | Acción de mitigación | Plazo |
|:---------:|:---|---------|-----------|----------------------|-------|
| 1 | V7 | Prompt injection en consulta RAG | Alto | Validación y sanitización de entradas; filtros en recuperación; separación instrucción/contexto | Inmediato |
| 2 | V13 | Agotamiento de recursos | Alto | Rate limiting; límites de concurrencia; circuit breaker | Inmediato |
| 3 | V8 | Exfiltración hacia LLM cloud | Alto | Enrutar datos sensibles al LLM local; minimizar contexto enviado; acuerdos con proveedor | Corto plazo |
| 4 | V11 | Fallo autorización multi-tenant | Alto | RBAC; pruebas IDOR/autorización; aislamiento estricto por cliente | Corto plazo |
| 5 | V3 | Manipulación requests (IDOR) | Alto | Validación server-side; ownership por recurso y tenant | Corto plazo |
| 6 | V10 | Retrieval leakage | Alto | Filtros por tenant en vector DB; pruebas de aislamiento semántico | Corto plazo |
| 7 | V4 | Over-fetching frontend | Alto | Minimización de campos en API; sin datos sensibles en build | Corto plazo |
| 8 | V12 | Exfiltración en logs | Alto | Política de no-log; enmascaramiento; cifrado de logs | Corto plazo |
| 9 | V9 | Data poisoning en ingesta | Alto | Validación de archivos; revisión de pipeline ETL; cuarentena de cargas | Corto plazo |
| 10 | V6 | Dependencias comprometidas | Alto | npm audit; SRI; versionado fijo de paquetes | Mediano plazo |
| 11 | V5 | Secuestro sesión (XSS) | Medio | Cookies HttpOnly; CSP; DOMPurify | Mediano plazo |
| 12 | V1 | Robo credenciales | Medio | MFA; notificación de logins; detección anómala | Mediano plazo |
| 13 | V2 | Sesión abierta | Medio | Timeout 15 min; cierre server-side; panel de sesiones | Mediano plazo |
| 14 | V14 | Inferencias sin trazabilidad | Medio | Logging de consulta, fragmentos, modelo y usuario | Mediano plazo |

---

## Controles por categoría

### Preventivos

- MFA y OAuth2 federado robusto
- RBAC y mínimo privilegio por tenant
- Validación y sanitización de entradas (API y prompts RAG)
- Enrutamiento por sensibilidad: LLM local vs cloud
- Cifrado en tránsito (TLS) y reposo
- CSP, cookies seguras, sin JWT en localStorage
- Rate limiting y WAF en perímetro

### Detectivos

- Logging centralizado y correlación en SIEM
- Alertas por consultas masivas o patrones anómalos
- Auditoría de dependencias (npm audit)
- Monitoreo de accesos cross-tenant

### Correctivos

- Circuit breaker hacia LLM y servicios dependientes
- Procedimiento de revocación de sesiones y tokens
- Restauración desde backup del índice vectorial

---

## Trazabilidad

| Documento | Relación |
|-----------|----------|
| [`07-riesgos-residuales.md`](07-riesgos-residuales.md) | Riesgo remanente tras estos controles |
