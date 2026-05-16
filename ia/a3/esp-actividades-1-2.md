# Uso de IA - Especialista en Diagramas de Actividades-Casos 1 y 2

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizó como base los archivos:

- diagramas/02-casos-de-uso/01-caso-uso-cancelar-turno.puml
- diagramas/02-casos-de-uso/02-caso-de-uso-agendar-turno.puml
- diagramas/03-escenarios-casos-de-uso/03-registro-paciente-registro-de-paciente-exitoso-01.md
- diagramas/03-escenarios-casos-de-uso/03-cancelar-turno-cancelacion-de-turno-por-secretaria-exitoso-02.md

## Proms utilizados en casos 1 y 2
### Interacción caso 1   
```
 -  Genera un diagrama de actividades en PlantUML para el caso de uso "Cancelar turno".

Usa como contexto:
- diagramas/02-casos-de-uso/01-caso-uso-cancelar-turno.puml
- diagramas/03-escenarios-casos-de-uso/03-cancelar-turno-cancelacion-de-turno-por-secretaria-exitoso-02.md

Requisitos:
- mínimo 10 actividades
- mínimo 3 swimlanes
- incluir decisiones y bifurcaciones
- representar claramente el flujo principal
- usar sintaxis PlantUML profesional

Genera únicamente el código PlantUML completo.
```
### Interacción caso 2
```
Genera un diagrama de actividades en PlantUML para el caso de uso "Agendar turno".

Usa como contexto:
- diagramas/02-casos-de-uso/02-caso-de-uso-agendar-turno.puml
- diagramas/03-escenarios-casos-de-uso/03-registro-paciente-registro-de-paciente-exitoso-01.md

Requisitos:
- mínimo 10 actividades
- mínimo 3 swimlanes
- incluir decisiones y bifurcaciones
- representar claramente el flujo principal
- usar sintaxis PlantUML profesional

Genera únicamente el código PlantUML completo.
```
## Ajustes realizados

### caso 1
- No hubo ajustes realizados con ia

### caso 2
```
El archivo PlantUML tiene errores de sintaxis y no renderiza correctamente en PlantUML Preview.

Corrige el código manteniendo:
- mínimo 10 actividades
- mínimo 3 swimlanes
- decisiones y bifurcaciones
- flujo principal del caso de uso "Agendar Turno"

Asegúrate de:
- balancear correctamente los if/endif
- mantener sintaxis válida de PlantUML
- generar un diagrama renderizable

Devuelve únicamente el código PlantUML corregido.
```

```
Corrige el diagrama de actividades en PlantUML para que renderice correctamente en PlantUML Preview.

Mantén los requisitos de la consigna:
- mínimo 10 actividades
- mínimo 3 swimlanes
- decisiones y bifurcaciones
- flujo principal claro
- sintaxis profesional PlantUML

Verifica especialmente:
- balance correcto de if/endif
- estructura válida de swimlanes
- sintaxis compatible con PlantUML

Devuelve únicamente el código PlantUML corregido completo.
```

## Conclusión

El uso de Copilot permitió acelerar la generación inicial de los diagramas de caso 1 y 2 , pero fue necesario realizar mayormente ajustes manuales para garantizar una  coherencia en  el sistema.