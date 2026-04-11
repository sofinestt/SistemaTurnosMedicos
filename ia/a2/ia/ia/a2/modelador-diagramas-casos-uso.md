# Uso de IA - Modelador de Diagramas de Casos de Uso

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizó como base el archivo:
- anexos/introduccion.md

## Prompt utilizado

Se solicitó a Copilot la generación de casos de uso y diagramas en PlantUML a partir del contexto del sistema de turnos médicos, incluyendo actores, casos de uso y relaciones <<include>> y <<extend>>.


Leé el archivo anexos/introduccion.md y utilizalo como contexto para el sistema de gestión de turnos médicos.

Generá una lista de casos de uso relevantes del sistema, identificando:

- Actores principales
- Casos de uso (en infinitivo)
- Relaciones entre casos de uso (asociación, <<include>>, <<extend>>)

Luego generá el código en PlantUML para al menos 5 diagramas de casos de uso, organizados por funcionalidad del sistema.

Asegurate de que:
- Los actores sean correctos y coherentes con el sistema
- Los casos de uso representen funcionalidades reales
- Se utilicen correctamente las relaciones <<include>> y <<extend>>

No inventes funcionalidades que no estén justificadas en el contexto.

## Resultados obtenidos

Copilot generó una primera versión de múltiples diagramas de casos de uso, incluyendo:
- Identificación de actores (Paciente, Secretaria, Médico)
- Casos de uso principales del sistema
- Relaciones entre casos de uso

## Ajustes realizados

Los resultados fueron revisados y modificados manualmente para mejorar la calidad del modelado:

- Corrección de nombres de casos de uso (uso de infinitivo)
- Revisión y ajuste de relaciones <<include>> y <<extend>>
- Eliminación de funcionalidades no coherentes con el sistema
- Separación de diagramas por responsabilidad
- Mejora en la claridad y consistencia de los diagramas

## Conclusión

El uso de Copilot permitió acelerar la generación inicial de los diagramas, pero fue necesario un análisis y ajuste manual para garantizar su coherencia con el sistema y cumplir con las buenas prácticas de modelado UML.