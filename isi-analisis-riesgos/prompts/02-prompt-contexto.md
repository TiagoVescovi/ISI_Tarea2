# Registro de prompt — actualización de contexto (escenario 11)

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-15 16:30 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | `README.md`, `prompts/00-memory-bank.md`, `docs/*`, `references/bibliografia.md`, `templates/plantilla-seccion.md`, índice `docs/00-indice-documentos.md` |

## Prompt exacto

```text
# ACTUALIZACIÓN DE CONTEXTO DEL PROYECTO

Ya existe una estructura inicial del repositorio creada previamente.

A partir de este momento el escenario seleccionado para el trabajo es:

# Escenario 11: Plataforma de Asistencia Legal basada en IA Generativa (RAG)

[PEGAR AQUÍ EL ESCENARIO COMPLETO]

# OBJETIVO DE ESTA TAREA

NO desarrollar todavía el análisis completo.

NO completar STRIDE.

NO completar DREAD.

NO generar mitigaciones.

NO inventar amenazas.

NO generar conclusiones.

NO completar entregables.

La tarea actual es únicamente adaptar la estructura y la documentación base al escenario seleccionado.

# ACCIONES PERMITIDAS

1. Actualizar README.md para reflejar el escenario elegido.
2. Actualizar prompts/00-memory-bank.md incorporando el contexto del escenario.
3. Verificar que la estructura del repositorio cubra todos los entregables exigidos por la materia.
4. Crear plantillas vacías o esqueletos documentales para los futuros entregables.
5. Agregar referencias bibliográficas y normativas relevantes al escenario.
6. Crear trazabilidad entre documentos.

# ACCIONES NO PERMITIDAS

* Inventar activos.
* Inventar trust boundaries.
* Inventar amenazas.
* Completar STRIDE.
* Completar DREAD.
* Completar ATT&CK.
* Completar OWASP LLM Top 10.
* Completar matrices de riesgo.
* Realizar estimaciones de impacto o probabilidad.
* Generar controles o mitigaciones definitivas.

Si alguna sección requiere información que todavía no fue analizada, dejar:

TODO: Completar durante la fase de análisis.

# MEMORY BANK

Actualizar prompts/00-memory-bank.md para incluir:

## Escenario seleccionado

Plataforma de Asistencia Legal basada en IA Generativa (RAG)

## Tecnologías identificadas

* Frontend Next.js
* OAuth2
* LangChain o LlamaIndex
* Python
* RabbitMQ
* Chroma o Pinecone
* OpenAI o Anthropic
* LLM local
* Arquitectura RAG

## Metodologías previstas

* STRIDE
* DREAD
* MITRE ATT&CK
* MITRE ATLAS
* OWASP Top 10 for LLM
* NIST SP 800-30
* NIST SP 800-53
* NIST AI RMF
* ISO 27001

## Entregables previstos

* Inventario de activos
* Arquitectura y trust boundaries
* Matriz STRIDE
* Priorización DREAD
* ATT&CK / ATLAS Mapping
* Plan de mitigación
* Riesgos residuales
* Controles NIST / ISO
* Conclusiones
* DFD de arquitectura RAG

# REQUISITO IMPORTANTE

Antes de modificar archivos:

1. Mostrar qué archivos serán modificados.
2. Explicar brevemente por qué.
3. Esperar aprobación para cambios destructivos.
4. Mantener compatibilidad con los requisitos académicos y la política obligatoria de uso de IA.

# RESULTADO ESPERADO

Dejar el repositorio preparado para comenzar posteriormente el análisis de riesgos del escenario 11, sin adelantar contenido analítico que todavía no haya sido elaborado.
```

## Nota de trazabilidad

En la ejecución real, el texto completo del escenario 11 reemplazó el marcador `[PEGAR AQUÍ EL ESCENARIO COMPLETO]` en el mensaje al asistente; aquí se conserva la plantilla tal como figura en el borrador del archivo.
