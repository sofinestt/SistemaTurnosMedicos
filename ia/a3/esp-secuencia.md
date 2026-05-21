# Uso de IA - Especialista en Diagramas de Secuencia

## Herramienta utilizada

Se utilizó ChatGPT integrado como herramienta de apoyo durante el desarrollo en VS Code.

## Contexto proporcionado

Se utilizaron como base los archivos de casos de uso y escenarios del proyecto SistemaTurnosMedicos para representar interacciones entre actores y objetos mediante diagramas de secuencia en PlantUML.

## Prompts utilizados

### Interacción 1
```
Necesito el código PlantUML para un diagrama de secuencia del caso de uso "Agendar turno".  

Requisitos: 

- Representar actores y participantes 
- Incluir mensajes entre objetos 
- Representar flujo principal
- Usar sintaxis PlantUML profesional  

Genera únicamente el código PlantUML completo.
```

### Interacción 2
```
Genera un diagrama de secuencia en PlantUML para el caso de uso "Cancelar turno".  Usa como contexto los escenarios y casos de uso del sistema.  

Requisitos: 

- Representar interacción entre secretaria, sistema y agenda 
- Incluir mensajes y respuestas 
- Mantener flujo claro 
- Usar sintaxis PlantUML válida  

Devuelve únicamente el código PlantUML completo.
```

### Interacción 3
```
Corrige el código PlantUML del diagrama de secuencia porque no renderiza correctamente en PlantUML Preview.  

Verifica: 

- Participantes correctamente definidos 
- Mensajes válidos 
- Sintaxis compatible con PlantUML 
- Flujo coherente del caso de uso  

Devuelve únicamente el código corregido completo.
```

## Ajustes realizados

### Caso 1
- Se realizaron correcciones menores de sintaxis en PlantUML.
- Se ajustaron nombres de participantes para mantener coherencia con el sistema.

### Caso 2
- Se corrigieron errores de renderizado en PlantUML Preview.
- Se reorganizaron mensajes entre participantes para mejorar claridad del flujo.

## Conclusión

El uso de ChatGPT permitió agilizar la generación inicial de los diagramas de secuencia en PlantUML y facilitar la comprensión de la sintaxis UML. Posteriormente se realizaron ajustes manuales para asegurar coherencia con los casos de uso y correcto renderizado de los diagramas.