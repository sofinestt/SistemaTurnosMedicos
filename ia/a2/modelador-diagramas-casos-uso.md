# Uso de IA - Modelador de Diagramas de Casos de Uso

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizó como base el archivo:
- anexos/introduccion.md

## Prompt utilizado


Se solicitó a Copilot la generación de casos de uso y diagramas en PlantUML a partir del contexto del sistema de turnos médicos, incluyendo actores, casos de uso y relaciones <<include>> y <<extend>>.

```
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

```

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

## Iteraciones
### Iteración 1: Estructura Base
Se generaron los diagramas iniciales para los 5 procesos principales (Registro, Agenda, Cancelación, Reprogramación y Disponibilidad). La IA propuso una estructura estándar de actor-sistema.

### Iteración 2: Refinamiento de Lógica de Sobreturnos
Se detectó que el proceso de **Autorizar Sobreturno** era más crítico para el negocio que la reprogramación básica. Se solicitó a la IA reestructurar el archivo `03-caso-uso-reprogramar-turno.puml` para transformarlo en el diagrama de **Autorización de Sobreturno**, integrando al Médico como actor principal de la decisión.

### Iteración 3: Incorporación de Relaciones de Inclusión
Para cumplir con el **RF2 (Control de superposición)**, se agregaron relaciones `<<include>>` hacia los casos de uso "Validar reglas del consultorio" y "Registrar marca de sobreturno", asegurando que el diagrama refleje no solo la acción, sino también las validaciones y la trazabilidad requeridas.