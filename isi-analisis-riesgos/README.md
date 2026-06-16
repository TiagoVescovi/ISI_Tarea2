# ISI — Análisis de riesgos

## Datos del proyecto

| Campo | Valor |
|--------|--------|
| **Alumno** | Tiago Vescovi |
| **Docente** | Andrés Pastorini |
| **Materia** | Introducción a la Seguridad Informática (ISI) |
| **Año** | 2026 |

## Objetivo general

Preparar el trabajo académico de la materia ISI centrado en **análisis de riesgos**, siguiendo la metodología y requisitos del curso. El desarrollo del caso (escenario, activos, amenazas y controles) comenzará **solo cuando el escenario esté definido** por la cátedra o el enunciado.

## Estado actual

**Escenario pendiente de definición.**

No se documenta aún inventario de activos, modelado de amenazas (p. ej. STRIDE), puntuación (p. ej. DREAD), mapeos MITRE ATT&CK, controles NIST ni planes de mitigación hasta contar con el escenario oficial.

## Herramientas de IA utilizadas

Registrar aquí cada herramienta de IA empleada durante el proyecto (nombre, versión o enlace si aplica). Los **prompts** concretos se archivan en la carpeta `prompts/` según el procedimiento acordado.

| Fecha | Herramienta | Uso breve (sin detalle del escenario) |
|--------|-------------|----------------------------------------|
| <!-- TODO: completar cuando se utilice IA --> | | |

## Estructura del repositorio

```text
isi-analisis-riesgos/
├── README.md                 # Este archivo
├── prompts/                  # Registro de prompts y memory bank para IA
├── docs/                     # Documentación del trabajo (cuando exista escenario)
├── diagrams/                 # Diagramas (cuando correspondan al escenario)
├── templates/                # Plantillas reutilizables
├── tools/                    # Scripts o utilidades auxiliares
└── references/               # Referencias bibliográficas y enlaces
```

## Cómo registrar prompts

Por cada uso relevante de IA, añadir un archivo en `prompts/` (convención sugerida: `YYYY-MM-DD-herramienta-tema.md` o similar) con **fecha**, **herramienta** y **prompt exacto**. Actualizar también la tabla de esta sección y el archivo `prompts/00-memory-bank.md` cuando cambie el estado del proyecto.
