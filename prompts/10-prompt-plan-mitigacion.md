# Registro de prompt — plan de mitigación (propuesta)

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 20:00 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/06-plan-mitigacion.md` (análisis y propuesta previa; documento aún no redactado) |

## Prompt exacto

```text
Necesito que continúes con el siguiente entregable del proyecto:

docs/06-plan-mitigacion.md

Antes de hacer cualquier cambio:

Revisá completamente:
docs/01-inventario-activos.md
docs/02-arquitectura-trust-boundaries.md
docs/03-analisis-stride.md
docs/04-priorizacion-dread.md
docs/05-mapa-attack.md
prompts/00-memory-bank.md
references/
Explicame primero:
qué información vas a utilizar de cada documento
cómo pensás construir el plan de mitigación
qué amenazas vas a tomar como prioritarias
qué estructura va a tener el documento
No modifiques ningún archivo hasta que yo apruebe tu propuesta.

Para este entregable quiero que trabajes únicamente con la información existente en el proyecto y con los materiales de clase que están en references/.

No agregues metodologías, frameworks, controles o conceptos externos que no hayan sido vistos en clase.

No quiero referencias a:

MITRE ATLAS
OWASP LLM
ISO 27001
CIS Controls
CVSS
FAIR
OCTAVE
Frameworks externos no incluidos en los materiales del curso

El documento debe estar alineado con:

amenazas identificadas en STRIDE
priorización realizada en DREAD
técnicas ATT&CK documentadas en 05-mapa-attack.md

No inventes amenazas nuevas.

No inventes activos nuevos.

No modifiques análisis previos para que encajen con el plan.

El plan debe construirse a partir de lo que ya existe.

Las mitigaciones propuestas deben justificarse utilizando únicamente conceptos vistos en clase.

Por ejemplo, si corresponde según el escenario, podés utilizar controles como:

MFA
Segmentación de red
EDR
IDS/IPS
SIEM
Hardening
Gestión de privilegios
Capacitación contra phishing
Monitoreo DNS
Filtrado de contenido web
Control de aplicaciones
DLP
Backups
Whitelisting

pero solamente cuando tengan sentido respecto al riesgo analizado.

Quiero un documento formal en Markdown con:

Introducción
Resumen de amenazas priorizadas
Plan de mitigación por amenaza
Priorización de implementación
Recomendaciones operativas
Conclusiones

Utilizá tablas cuando aporten claridad.

Importante:

No hagas cambios todavía.
Primero mostrame el análisis de cómo pensás hacerlo.
Esperá mi aprobación antes de crear o modificar archivos.
Si encontrás inconsistencias entre documentos, avisame antes de continuar.
```

## Resultado de la asistencia (en ese intercambio)

Propuesta de plan de mitigación para las **16** amenazas DREAD, estructura del documento, controles alineados a `references/`, e identificación de **`05-mapa-attack.md` vacío** como bloqueante para el vínculo ATT&CK. **Sin** modificación de archivos en el repositorio en esa sesión.
