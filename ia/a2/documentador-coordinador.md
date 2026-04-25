# Uso de IA - Diseñador Tarjetas CRC

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizó como base el archivo:
- anexos/introduccion.md

## Prompt utilizado

```
Actua como un Señor Supremo Engineer realizando code review profesional

Estas analizando los cambios de una Pull Request Activa

INSTRUCCIONES IMPORTANTES

Identifica problemas reales del código
Enumera tus hallazgos (1,2,3...)
Cada hallazgo debe ser independiente
Se claro, mecánica y concreto
No inventes problemas hipotéticos sin evidencia en el código
No incluyas sugerencias de teoría

Para cada hallazgo usa EXACTAMENTE esta estructura

======================
Hallazgo #numero

Author
Linea

Tipo de problema 
Severidad
Sugerencia de mejora

Ejemplo de  código corregido (si aplica)

*código 

==========================
DECISION DE REVISION HUMANO

[] Aceptar sugerencia
[] Rechazar sugerencia

Justificación del revisor humano

Al final agregar:

EVALUACION GENERAL DE LA PR
Evaluación general de calidad y riesgos técnicos

DECISION GENERAL EVALUADA POR IA:

APPROVE

REQUEST CHANGES

COMMENT ONLY: No completes la sección DECISION DEL REVISOR HUMANO 
Debe quedar vacio para edición manual.
Completa comentarios en la pull request en las líneas correspondientes.

```

## Resultados obtenidos

Se logró una revisión sistemática y profesional de las tarjetas CRC, los diagramas de casos de uso y los escenarios principales del sistema de turnos médicos. El prompt permitió identificar problemas concretos, mejorar la claridad de responsabilidades y asegurar la trazabilidad entre actores, casos de uso y entidades. Se obtuvieron tarjetas CRC más precisas y alineadas con los requerimientos funcionales, y se documentaron escenarios de uso completos y realistas.

## Ajustes realizados

- Se corrigieron y completaron las tarjetas CRC para reflejar mejor las responsabilidades, colaboradores y atributos de cada entidad (Persona, Paciente, Médico, Secretaria, Turno, Agenda, Disponibilidad, Notificación, Llegada Paciente).
- Se revisaron y ajustaron los diagramas de casos de uso para asegurar la coherencia con los procesos reales del sistema, agregando relaciones de inclusión y actores faltantes.
- Se actualizaron los escenarios de casos de uso, detallando pasos, precondiciones, postcondiciones y riesgos, para cubrir los flujos principales de registro, agendamiento, cancelación, reprogramación y sobreturno.

## Conclusión

El uso del prompt estructurado y la integración de IA facilitaron una revisión exhaustiva y objetiva de los artefactos clave del sistema. Las correcciones realizadas mejoraron la calidad de la documentación y la alineación entre los distintos modelos. El proceso permitió detectar inconsistencias, fortalecer la trazabilidad y dejar una base sólida para futuras iteraciones y validaciones del sistema.

