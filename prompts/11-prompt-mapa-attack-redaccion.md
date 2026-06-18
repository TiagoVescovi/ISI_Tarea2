# Registro de prompt — redacción mapa ATT&CK

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-17 22:30 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `docs/05-mapa-attack.md` |

## Prompt exacto

```text
El análisis me parece correcto en términos generales.

Antes de generar docs/05-mapa-attack.md, verificá que todas las técnicas ATT&CK utilizadas existan explícitamente en references/MITRE_ATTCK. Si alguna técnica no figura en el material de clase, no la utilices y documentá la limitación metodológica correspondiente.

Además, revisá si para STR-TB09-E y STR-C02-E resulta más apropiado utilizar T1078 (Valid Accounts) que T1068, ya que las amenazas parecen asociadas a fallos de autorización utilizando identidades válidas más que a una elevación de privilegios mediante explotación.

El resto del enfoque (16 amenazas, STR-C12-R sin equivalente ATT&CK ofensivo, TB-10 condicional y técnicas secundarias justificadas) puede mantenerse.
```

## Resultado de la asistencia

Redacción completa de **`docs/05-mapa-attack.md`**: mapeo de 16 amenazas DREAD (15 con técnica ATT&CK, STR-C12-R excluida). Verificación: **T1078** no está en `MITRE_ATTCK` §3; limitación documentada; STR-C02-E y STR-TB09-E mapeadas a **T1068** como aproximación disponible.
