# Registro de prompt — estructura inicial del repositorio

| Campo | Valor |
|--------|--------|
| **Fecha y hora** | 2026-06-15 10:00 (UTC-3) |
| **Herramienta** | Cursor (asistente IA en el IDE) |
| **Entregable vinculado** | Estructura `isi-analisis-riesgos/` (`README.md`, `prompts/`, `docs/`, `diagrams/`, `templates/`, `tools/`, `references/`, `prompts/00-memory-bank.md`) |

## Prompt exacto

````text
# OBJETIVO

Actúa como un arquitecto de software y gestor de proyectos académicos.

Estoy comenzando un trabajo de la materia **Introducción a la Seguridad Informática (ISI)**.

Datos del proyecto:

* Estudiante: Tiago Vescovi
* Docente: Andrés Pastorini
* Año: 2026

IMPORTANTE: AÚN NO SE HA DEFINIDO EL ESCENARIO DEL TRABAJO.

Por lo tanto, NO debes desarrollar contenido específico, análisis de riesgos, amenazas, matrices STRIDE, DREAD, MITRE ATT&CK, controles NIST ni documentación técnica del caso.

Tu única tarea es crear la estructura inicial del repositorio y dejar preparado el entorno de trabajo.

# FORMA DE TRABAJO OBLIGATORIA

Antes de realizar cualquier modificación:

1. Explicar qué vas a crear.
2. Mostrar un plan breve de trabajo.
3. Esperar confirmación cuando una acción implique modificar o eliminar archivos existentes.
4. Nunca inventar información del escenario.
5. Utilizar marcadores TODO cuando falte información.
6. Mantener consistencia entre todos los archivos creados.
7. No generar documentación de análisis hasta que se proporcione el escenario.

# REQUISITOS OBLIGATORIOS DE IA

Este proyecto debe cumplir estrictamente con la política de uso de IA de la materia.

## Registro de Prompts

Crear la carpeta:

```text
prompts/
```

y dejar preparada una estructura para almacenar todos los prompts utilizados durante el proyecto.

Cada prompt futuro deberá incluir:

* Fecha
* Herramienta utilizada
* Prompt exacto utilizado

## Memory Bank

Crear obligatoriamente:

```text
prompts/00-memory-bank.md
```

Este archivo debe funcionar como contexto central para futuras interacciones con IA.

Debe contener únicamente una estructura base y marcadores TODO.

No debe asumir ningún escenario.

## Declaración de Herramientas de IA

Preparar en el README una sección obligatoria:

```markdown
## Herramientas de IA utilizadas
```

para registrar las herramientas utilizadas durante el proyecto.

# ESTRUCTURA A CREAR

Crear únicamente la estructura de carpetas y archivos vacíos o con contenido mínimo descriptivo:

```text
isi-analisis-riesgos/
├── README.md
├── prompts/
│   ├── 00-memory-bank.md
│   └── README.md
├── docs/
│   └── README.md
├── diagrams/
│   └── README.md
├── templates/
│   └── README.md
├── tools/
│   └── README.md
└── references/
    └── README.md
```

# CONTENIDO MÍNIMO

README.md:

* Nombre del proyecto
* Alumno
* Docente
* Materia
* Objetivo general
* Estado actual: "Escenario pendiente de definición"
* Sección "Herramientas de IA utilizadas"
* Estructura del repositorio

prompts/00-memory-bank.md:

* Contexto general del proyecto
* Estado actual
* Archivos importantes
* Reglas para futuras interacciones con IA
* TODO: definir escenario

README.md de cada carpeta:

* Explicar brevemente para qué se utilizará esa carpeta durante el proyecto.

# IMPORTANTE

NO crear:

* Inventario de activos
* STRIDE
* DREAD
* MITRE ATT&CK
* Planes de mitigación
* Riesgos residuales
* Diagramas
* Arquitecturas
* Escenarios ficticios

Solo preparar la estructura base del repositorio y la metodología de trabajo para el proyecto.
````

## Nota de trazabilidad

Primera interacción documentada: creación de la estructura base del repositorio antes de fijar el escenario 11.
