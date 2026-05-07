# Uso de IA - Documentador y Coordinador de repositorio(SRP)

## Herramienta utilizada

Se utilizó gitHub Copilot en modo Agent integrado en VS Code

## Contexto proporciando

- `Lee el archivo: anexos/introduccion.md`
- `Lee el diagrama: diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- `Lee todas las tarjetas CRC que existan en: herramientas-agile/tarjetas-crc/`

## Prompt utilizado

```
Contexto obligatorio:

Lee el archivo: anexos/introduccion.md
Lee el diagrama: diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
Lee todas las tarjetas CRC que existan en: herramientas-agile/tarjetas-crc/
Tarea:

Identifica 3 clases principales del sistema que violan el Principio de Responsabilidad Única (SRP).

Para cada clase debes entregar:

Responsabilidades actuales (múltiples)
Problemas de mantenibilidad que genera
Propuesta de refactorización aplicando SRP (clases nuevas o separadas)
Al final genera:

El contenido completo y profesional para el archivo anexos/principios-solid/01-srp.md usando esta estructura exacta:

- Principio de Responsabilidad Única (SRP)
- Propósito y Tipo del Principio SOLID
- Motivación
- Explicación de Herencia (o Aplicación del SRP)
- Estructura de Clases (con imagen)
- Justificación Técnica
También genera el código PlantUML para el diagrama 01-solid-01-srp.puml

```

## Resultados obtenidos

He completado la tarea solicitada. Identifiqué tres clases principales que violan el Principio de Responsabilidad Única (SRP) basándome en el análisis de la introducción, el diagrama inicial y las tarjetas CRC: Medico, Secretaria y Agenda. Para cada una, detallé las responsabilidades actuales, los problemas de mantenibilidad y una propuesta de refactorización aplicando SRP.

Generé el contenido completo y profesional para el archivo 01-srp.md, siguiendo exactamente la estructura especificada. También creé el archivo 01-solid-01-srp.puml con el código PlantUML que ilustra la refactorización propuesta.

Los archivos están listos para revisión. Si necesitas ajustes o más detalles, avísame.


---

## Ajustes realizados

Realiza correcciones generales y mejoras para que cumpla perfectamente con la consigna de la actividad SRP:

Correcciones requeridas:
- Ajustar exactamente a la estructura solicitada:

- Principio de Responsabilidad Única (SRP)
- Propósito y Tipo del Principio SOLID
- Motivación
- Explicación de Herencia (o Aplicación del SRP)
- Estructura de Clases (con imagen)
- Justificación Técnica
- Mejorar la redacción para que sea más formal y académica.

- Corregir la ruta de la imagen a: ../../diagramas/01-diagrama-clases/01-solid-01-srp.png

- Fortalecer la Motivación con un ejemplo concreto del Sistema de Turnos Médicos.

- Mejorar la claridad en la refactorización de las 3 clases (Medico, Secretaria y Agenda).

- Mejorar la Justificación Técnica explicando lo que muestra el diagrama.

## Conclusión

La aplicación del Principio de Responsabilidad Única permitió mejorar la coherencia del diseño del sistema de turnos médicos.

Se logró una documentación más clara y profesional sobre cómo Medico, Secretaria y Agenda deben separarse en responsabilidades específicas para evitar clases con múltiples razones para cambiar.

El uso de Copilot facilitó la generación del contenido, la corrección de la estructura solicitada y la adaptación final del archivo 01-srp.md con una ruta de imagen correcta y una redacción formal

## Iteraciones

Primera iteración:
Solicite identificar tres clases del sistema que violaban SRP y generar un documento completo con la estructura requerida. Se detectaron Medico, Secretaria y Agenda como casos principales.

Segunda iteración:
Pedi mejoras en la redacción, limpieza y una motivación más concreta, además de corregir la ruta de la imagen y clarificar la refactorización propuesta.

Tercera iteración:
Solicite el ajuste final del título de la sección a Explicación de Herencia / Aplicación del SRP y la confirmación exacta de la sección ## Estructura de Clases (con imagen).