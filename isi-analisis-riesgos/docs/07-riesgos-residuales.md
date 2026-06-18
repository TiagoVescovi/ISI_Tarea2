# Riesgos residuales

**Escenario:** 11 — Plataforma de Asistencia Legal basada en IA Generativa (RAG).

**Fuentes:** [`06-plan-mitigacion.md`](06-plan-mitigacion.md), [`04-priorizacion-dread.md`](04-priorizacion-dread.md).

Riesgo que permanece después de aplicar las mitigaciones del plan. Permite decidir qué aceptar, transferir o seguir tratando.

---

## Criterios

| Nivel residual | Criterio |
|----------------|----------|
| **Bajo** | Mitigación reduce fuertemente probabilidad e impacto; monitoreo normal |
| **Medio** | Persiste exposición por configuración, error humano, variantes de ataque o dependencia operativa |
| **Alto** | Riesgo relevante pese a controles; requiere revisión prioritaria |
| **Crítico** | No aceptable sin acciones adicionales |

---

## Matriz de riesgos residuales

| ID | Amenaza (DREAD) | Residual | Impacto | Justificación |
|----|-----------------|----------|---------|---------------|
| R01 | V1 Robo credenciales | Medio | Medio | MFA reduce robo directo; persiste phishing e ingeniería social |
| R02 | V2 Sesión abierta | Bajo | Medio | Timeout y cierre server-side mitigan dispositivos compartidos |
| R03 | V3 IDOR | Medio | Alto | Depende de reglas de negocio y pruebas continuas de autorización |
| R04 | V4 Over-fetching | Medio | Alto | Riesgo en cambios futuros de API o builds |
| R05 | V5 XSS | Medio | Alto | XSS avanzado puede afectar sesión activa pese a CSP |
| R06 | V6 Dependencias | Medio | Alto | Vulnerabilidades nuevas en paquetes no detectadas a tiempo |
| R07 | V7 Prompt injection | Medio | Alto | Ataques indirectos y jailbreaks evolucionan; no hay defensa absoluta |
| R08 | V8 Exfil LLM cloud | Medio | Alto | Transferencia al proveedor no elimina responsabilidad de la firma |
| R09 | V9 Data poisoning | Medio | Alto | Envenenamiento sutil puede pasar validaciones automáticas |
| R10 | V10 Retrieval leakage | Medio | Alto | Similitud semántica puede mezclar contextos entre tenants |
| R11 | V11 Autorización multi-tenant | Medio | Alto | Bugs lógicos en autorización son recurrentes en SaaS |
| R12 | V12 Logs | Bajo | Medio | Política de no-log reduce exposición; riesgo por mala configuración |
| R13 | V13 Agotamiento recursos | Medio | Crítico | Picos de carga o umbrales mal calibrados pueden degradar servicio |
| R14 | V14 Sin trazabilidad | Bajo | Bajo | Logging estructurado cubre el caso; gaps por omisión operativa |

---

## Riesgos aceptados formalmente

| Riesgo | Motivo de aceptación |
|--------|----------------------|
| Alucinación del LLM | Inherente a modelos generativos; se mitiga con revisión legal obligatoria |
| Borradores erróneos sin revisión | Riesgo de proceso; la firma acepta si hay revisión humana antes de entrega |
| Pinecone SaaS (si aplica) | Exposición al proveedor; se acepta o se evita usando Chroma on-prem |

---

## Resumen

| Nivel residual | Cantidad | Observación |
|----------------|----------|-------------|
| **Bajo** | 3 | R02, R12, R14 |
| **Medio** | 11 | Patrón dominante tras controles |
| **Alto** | 0 | Ninguna amenaza queda en Alto con diseño mixto y controles planificados |
| **Crítico** | 0 | — |

La mayoría de amenazas bajan a **Medio** por dependencia de configuración, evolución de ataques contra IA y revisión humana en salidas legales.

---

## Trazabilidad

| Documento | Relación |
|-----------|----------|
| [`04-priorizacion-dread.md`](04-priorizacion-dread.md) | Puntajes DREAD iniciales |
| [`06-plan-mitigacion.md`](06-plan-mitigacion.md) | Controles aplicados |
