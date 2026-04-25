# Uso de IA - ESPECIALISTA EN INVERSIÓN DE DEPENDENCIAS

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizaron como base los siguientes archivos:
- anexos/introduccion.md
- diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
- herramientas-agile/tarjetas-crc/


## Prompt utilizado

```
Quiero que analices el diseño actual del sistema de turnos médicos tomando como contexto los siguientes archivos:

anexos/introduccion.md
diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
herramientas-agile/tarjetas-crc/

A partir de eso, realizá lo siguiente:

Identificá dependencias directas hacia clases concretas en el diseño actual (violaciones al principio de inversión de dependencias).
Proponé abstracciones adecuadas (interfaces o clases abstractas) para invertir esas dependencias.
Indicá claramente en qué partes del sistema conviene aplicar inyección de dependencias y cómo hacerlo (por constructor, setter, etc.).
Explicá brevemente el motivo de cada cambio propuesto.

Tené en cuenta:

Evitá crear abstracciones innecesarias o que no representen conceptos del dominio.
Usá nombres claros y coherentes con el sistema de turnos médicos.
Mantené consistencia con las responsabilidades de cada clase según las tarjetas CRC.

Finalmente:

Mostrá una versión mejorada del diseño (puede ser en forma de lista o pseudocódigo).
Señalá qué decisiones podrían discutirse o ajustarse.
```

## Output obtenido

Copilot identificó múltiples dependencias entre clases del sistema como:

- Turno → Paciente, Medico, Agenda, Notificacion
- Agenda → Disponibilidad, Turno
- Secretaria → Agenda, Notificacion

Además, propuso interfaces como:

- IPersona, IPaciente, IMedico, ISecretaria
- IAgenda, IDisponibilidad, INotificacion

También sugirió aplicar inyección de dependencias en clases como Turno, Agenda y Notificacion.

## Análisis

La propuesta incluye una gran cantidad de abstracciones.

Se observa que:
- Algunas interfaces corresponden al principio ISP (roles como IPaciente, IMedico)
- Otras sí aportan a DIP, como IDisponibilidad e INotificacion

## Ajustes realizados

Se descartaron interfaces que no reducían acoplamiento real (como IPaciente o IMedico), ya que representan entidades del dominio y no dependencias externas.
Se mantuvo INotificacion porque representa un punto de variación del sistema (envío de mensajes), lo cual sí justifica inversión de dependencias.

## Conclusiones
El uso de IA permitió identificar rápidamente dependencias acopladas en el diseño inicial. Sin embargo, fue necesario aplicar criterio de diseño para evitar sobre-abstracción, priorizando únicamente aquellas interfaces que representan variabilidad real del sistema según el principio DIP.