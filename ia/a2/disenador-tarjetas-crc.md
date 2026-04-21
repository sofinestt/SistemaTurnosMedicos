# Uso de IA - Diseñador Tarjetas CRC

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizó como base el archivo:
- anexos/introduccion.md

## Prompt utilizado

```
Leé los siguientes archivos como contexto del sistema:
- anexos/introduccion.md
- diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw

A partir de ese contexto:

1. Identificá las clases principales del sistema de turnos médicos.
2. Para cada clase, generá una tarjeta CRC en formato Markdown usando esta plantilla:

|  |  |  |  |
|---|---|---|---|
| Nombre de la Clase: | ... | | |
| Superclase: | ... | | |
| Subclase: | ... | | |
| Responsabilidades | Colaboradores | Pensamiento del objeto | Propiedad |

3. Requisitos:
- Incluir al menos 5 clases relevantes
- Las responsabilidades deben ser coherentes con el sistema
- Los colaboradores deben ser otras clases del sistema
- El pensamiento del objeto debe estar en primera persona
- Las propiedades deben ser atributos reales de la clase
- Mantener consistencia con el diagrama de clases proporcionado

4. Luego, hacé una revisión crítica del diseño:
- Detectá posibles errores o malas decisiones de modelado
- Señalá responsabilidades mal asignadas
- Identificá clases innecesarias o faltantes
- Proponé mejoras justificadas

5. Finalmente:
- Entregar la versión corregida y mejorada de las tarjetas CRC en Markdown
- Lista para copiar en el repositorio

No des explicaciones generales, solo mostrar:
1) tarjetas CRC iniciales
2) observaciones críticas
3) versión final corregida

```
## Resultados obtenidos

Copilot generó un conjunto inicial de tarjetas CRC que incluían las clases principales del sistema, como Paciente, Médico, Turno, Recepción y Sistema de Turnos. Las tarjetas contenían responsabilidades, colaboradores, propiedades y el pensamiento del objeto.

Sin embargo, en la primera versión se detectaron algunas inconsistencias, como:
- Responsabilidades repetidas entre clases
- Colaboradores poco claros o genéricos (por ejemplo, “Sistema” en lugar de clases concretas)
- Algunas propiedades no estaban bien definidas o eran redundantes


## Ajustes realizados
Se realizaron las siguientes mejoras sobre la salida generada por la IA:
- Se corrigieron responsabilidades mal asignadas (por ejemplo, evitar que el Paciente gestione lógica interna del Turno)
- Se ajustaron los colaboradores para que siempre sean clases concretas del sistema
- Se eliminaron responsabilidades duplicadas entre clases
- Se mejoró la redacción del pensamiento del objeto para que sea clara y en primera persona
- Se definieron propiedades más precisas y coherentes con cada clase
- Se verificó la consistencia con el diagrama de clases inicial

## Conclusión

El uso de GitHub Copilot permitió acelerar la identificación de clases y la generación inicial de las tarjetas CRC. Sin embargo, fue necesaria una revisión crítica para corregir errores de modelado y asegurar la coherencia del diseño.

La IA resultó útil como punto de partida, pero el resultado final dependió del análisis y ajustes realizados manualmente.
