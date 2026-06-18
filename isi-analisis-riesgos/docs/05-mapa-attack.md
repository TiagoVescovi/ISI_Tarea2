# Mapa MITRE ATT&CK

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`03-analisis-stride.md`](03-analisis-stride.md), [`04-priorizacion-dread.md`](04-priorizacion-dread.md).

Este documento contextualiza las amenazas priorizadas mediante un escenario hipotético de ataque y técnicas del framework MITRE ATT&CK.

Diagrama: [`../diagrams/attack-flow.png`](../diagrams/attack-flow.png).

---

## Escenario hipotético planteado

**Actor:** Atacante externo orientado a espionaje de datos legales.

**Objetivo:** Obtener contratos confidenciales y contexto de consultas RAG.

**Punto de entrada:** Portal web (Next.js) y API expuesta.

**Flujo del ataque:**

1. El atacante escanea el portal y la API para identificar endpoints de autenticación y carga documental (TB-01).
2. Obtiene acceso mediante phishing contra un usuario cliente o explotando credenciales débiles (T1566, T1078), cruzando TB-01.
3. Prueba manipulación de parámetros para acceder a documentos de otro tenant (IDOR) o inyecta instrucciones en una consulta RAG (prompt injection, T1059).
4. Carga un documento envenenado que altera el índice vectorial (data poisoning, T1565) vía pipeline ETL.
5. Fuerza consultas que recuperan fragmentos de otros clientes por fallo de filtrado (retrieval leakage, T1005).
6. Si el diseño enruta contexto sensible al LLM comercial, el atacante provoca exfiltración hacia el proveedor cloud (T1567).
7. Extrae datos mediante consultas automatizadas o abuso de permisos elevados en API (T1119).

---

## Técnicas identificadas

| Táctica | ID | Técnica | Activo afectado | Mitigación principal |
|---------|-----|---------|-----------------|----------------------|
| Reconnaissance | T1595 | Active Scanning | Frontend / API | WAF; rate limiting en TB-01 |
| Initial Access | T1566.002 | Phishing | Credenciales OAuth2 | MFA; detección de login anómalo |
| Initial Access | T1078 | Valid Accounts | Cuentas de usuario | MFA; políticas de contraseña en IdP |
| Execution | T1059.006 | Python / prompt injection | Orquestador RAG | Sanitización de entradas; validación de prompts |
| Persistence | T1556 | Modify Authentication Process | IdP / tokens | Validación estricta de tokens; rotación de secretos |
| Privilege Escalation | T1068 | Exploitation for Privilege Escalation | API multi-tenant | RBAC; pruebas de autorización por tenant |
| Defense Evasion | T1070 | Indicator Removal | Logs de auditoría | Logs centralizados append-only; SIEM |
| Collection | T1005 | Data from Local System | Base vectorial / almacén | Filtros por tenant; cifrado en reposo |
| Collection | T1119 | Automated Collection | API / documentos | Rate limiting; alertas por volumen anómalo |
| Exfiltration | T1567.002 | Exfiltration to Cloud Storage | LLM comercial | LLM local para datos sensibles; minimizar contexto |
| Impact | T1565 | Data Manipulation | Índice RAG / borradores | Validación de ingesta; revisión humana de salidas |

---

## Conclusión

El componente más crítico es el **backend (API + orquestador)**: concentra autorización multi-tenant, ensamblado de contexto RAG y decisión de enrutamiento hacia LLM local o cloud. La defensa requiere controles en TB-02, TB-03 y TB-04.

---

## Trazabilidad

| Documento | Relación |
|-----------|----------|
| [`06-plan-mitigacion.md`](06-plan-mitigacion.md) | Controles derivados de técnicas ATT&CK |
