# Uso de IA - Especialista de Casos de Uso

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizó como base el archivo:
- anexos/introduccion.md

# Prompt Utilizado

```
Actuá como un Software Engineer senior especializado en análisis de requisitos y modelado de casos de uso.

Tu tarea es leer cuidadosamente el archivo anexos/introduccion.md junto con la plantilla de escenarios disponible en el proyecto, y completar todos los campos de cada escenario de forma clara, consistente y profesional.

Objetivo

A partir del contenido de anexos/introduccion.md, identificar los casos de uso (explícitos o implícitos) y desarrollar los escenarios correspondientes completando los siguientes campos:

ID
Área
Actores
Descripción
Evento activador
Tipo de señal (interna / externa / temporal)
Flujo principal (pasos numerados)
Precondiciones
Postcondiciones
Suposiciones
Requerimientos relacionados (referenciar IDs si existen)
Aspectos sobresalientes (reglas de negocio, validaciones, restricciones)
Prioridad (alta / media / baja + breve justificación)
Riesgo (alto / medio / bajo + breve justificación)

Instrucciones
Leé completamente anexos/introduccion.md para comprender el dominio, contexto y objetivos del sistema.
Identificá todos los casos de uso relevantes, incluso si no están explícitamente definidos.
Generá uno o más escenarios por cada caso de uso detectado.
Usá lenguaje técnico claro, preciso y consistente con prácticas de ingeniería de software.
Ante información faltante, realizá suposiciones razonables y documentalas en el campo "Suposiciones".
Describí el flujo principal como una secuencia ordenada de interacciones entre actores y sistema.
Mantené consistencia terminológica en todos los escenarios.
Evitá contradicciones con el contenido del documento base.
Priorizá claridad, completitud y calidad técnica.
Formato de salida

Completá los escenarios usando la plantilla existente en el proyecto. Si no está definida explícitamente, utilizá el siguiente formato en Markdown:

| **Nombre del escenario:** |Flujo principal - | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** |  | | **ID Única:** |  |
| **Área** |  | | | |
| **Actor(es):** | | | | |
| **Descripción:** |  | | | |

| **Activar Evento** |  | **Identificadores e iniciadores de casos de uso** |  |
|---|---|---|---|
| **Tipo de señal** | ☐ Interna | ☐ Externa | ☐ Temporal | 

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1.  |  |
| 2.  |  |
| 3.  |  |
| 4.  |  |
| 5.  |  |
| 6.  |  |
| 7.  |  |
| 8.  |  |
| 9.  |  |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** |  |
| **Poscondiciones:** |  |
| **Suposiciones:** |  |
| **Reunir requerimentos:** |  |
| **Aspectos sobresalientes:** |  |
| **Prioridad:** | (Alta - Media -Baja) |
| **Riesgo:** | (Alto - Medio -Bajo) |

```

## Resultados obtenidos

A partir del prompt definido y de los casos de uso, se generaron 5 escenarios de casos de us del Sistema Turnos Médicos.

La generación incluyó:
- Validaciones de negocio implícitas no detalladas originalmente (por ejemplo conflictos de agenda, duplicidad de registros, trazabilidad de cambios).
- Identificación de riesgos operativos(Concurrencia, Sobrecarga de agenda, Inconsistencias).
- Justificación de prioridad y riesgo para cada escenario.

## Ajustes Realizados

Se revisó críticamente y ajustó el resultado incial generado por IA en los siguientes puntos:

- Se eliminaron pasos redundantes.
- Se agregaron pasos faltantes
- Se mejoró la secuencia actor-sistema para que refleje correctamente interacciones de casos de uso.
- Se completaron precondiciones incompletas
- Se ajustaron postcondiciones para reflejar cambios de estado. liberación de agenda y trazabilidad.

## Conclusión

El uso de IA agilizó y facilitó el proceso para la creación de los escenarios de casos de uso.
útil como punto de partida, para luego de la revisión, realizar los cambios necesarios. 