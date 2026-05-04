# Uso de IA - Especialista de Casos de Uso

## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizó como base el archivo:
- `anexos/introduccion.md`
- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- `herramientas-agile/tarjetas-crc/`

# Prompt Utilizado

```
Actuá como un Senior Software Engineer especializado en diseño orientado a objetos, DDD, SOLID (enfocado en Liskov Substitution Principle), modelado de dominio y architecture review.

Usá como contexto estos artefactos:
- anexos/introduccion.md
- diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
- herramientas-agile/tarjetas-crc/ (todas las tarjetas CRC)

Objetivo:
Realizá una revisión de diseño enfocada exclusivamente en Liskov Substitution Principle (LSP) dentro del dominio del sistema de turnos.

Tarea:
1. Analizá todas las jerarquías de herencia existentes o propuestas en el modelo.
2. Verificá si cada subclase puede sustituir correctamente a su superclase sin alterar el comportamiento esperado del sistema.

Para cada jerarquía evaluá:
- Si la relación “es-un” (is-a) realmente representa una abstracción válida de dominio.
- Si las subclases respetan el contrato implícito o explícito de la superclase.
- Si hay precondiciones más fuertes en subclases.
- Si hay postcondiciones debilitadas.
- Si alguna subclase cambia semánticamente el comportamiento esperado.
- Si hay overrides que rompan invariantes.
- Si aparecen operaciones heredadas que ciertas subclases no pueden cumplir.
- Si existen “falsos subtipos” (subclases que conceptualmente no deberían heredar).

Detectá posibles violaciones de LSP como por ejemplo:
- Subclases que restringen comportamiento válido de la superclase.
- Métodos heredados que en subtipos lanzan excepciones o quedan vacíos.
- Variantes que cambian reglas de negocio en vez de especializarlas.
- Jerarquías que fuerzan herencia donde debería haber composición o roles.

Para cada caso:
1. Explicá si la jerarquía respeta o viola LSP.
2. Justificá por qué.
3. Mostrá qué comportamiento podría romperse al sustituir el subtipo.
4. Si hay problemas, proponé un rediseño:
   - ajustar la abstracción base
   - refactorizar la jerarquía
   - reemplazar herencia por composición si corresponde
   - separar conceptos mal modelados

Revisá críticamente tu propio análisis:
- Señalá falsos positivos donde parece haber un problema LSP pero no lo hay.
- Cuestioná si alguna jerarquía es demasiado abstracta o artificial.
- Verificá que los cambios propuestos sean coherentes con el dominio del sistema de turnos.
- Evitá críticas puramente teóricas; fundamentá desde comportamiento y negocio.

Formato de salida:

## Análisis de jerarquías

### Jerarquía: <Superclase / Subclases>
- Evaluación de sustitución:
- Contrato esperado:
- Riesgos o violaciones detectadas:
- Ejemplo de ruptura de comportamiento (si aplica):
- Rediseño recomendado:

## Falsos positivos / Jerarquías válidas
(identificá herencias que parecen sospechosas pero están bien modeladas)

## Revisión crítica final
- Riesgos en las jerarquías actuales:
- Ajustes recomendados:
- Observaciones de modelado de dominio:

Criterios:
- No hagas una checklist académica de LSP.
- Evaluá sustitución desde comportamiento real del dominio.
- Priorizá semántica del negocio sobre pureza teórica.
- Quiero una review como la haría un senior engineer en una design review rigurosa.

```

# Especialista en Principio de Sustitución de Liskov (LSP)

## Resultados obtenidos

Se utilizó GitHub Copilot en modo agente para analizar la estructura de clases del sistema de turnos médicos, particularmente las relacionadas con la gestión de turnos y profesionales.

Prompt utilizado:
"Analiza las clases del sistema y detecta posibles violaciones al principio de sustitución de Liskov (LSP). Indica si alguna subclase altera el comportamiento esperado de la clase base."

Respuesta obtenida (resumida):
Copilot detectó que ciertas clases que implementaban o extendían comportamientos relacionados a la gestión de turnos modificaban validaciones y resultados respecto a la clase base. Esto implicaba que una subclase no podía ser utilizada de forma transparente en lugar de su clase padre.

También sugirió revisar la consistencia en los métodos sobrescritos y evitar cambios en precondiciones o postcondiciones.

---

## Ajustes realizados

A partir del análisis, se realizaron los siguientes ajustes:

- Se revisaron los métodos sobrescritos en clases derivadas para asegurar que mantengan el mismo contrato que la clase base.
- Se evitaron cambios en validaciones que restringían más el comportamiento en las subclases.
- Se unificaron criterios de validación para mantener coherencia entre clases.
- Se reorganizó parte de la lógica para que las subclases no alteren el resultado esperado.

Justificación:
Estos cambios aseguran que cualquier instancia de una subclase pueda ser utilizada donde se espera la clase base sin generar errores o comportamientos inconsistentes, cumpliendo con el principio LSP.

---

## Conclusión

La aplicación del principio de sustitución de Liskov permitió mejorar la consistencia del diseño orientado a objetos del sistema.

Se logró una estructura más predecible y mantenible, evitando comportamientos inesperados al utilizar polimorfismo.

El uso de Copilot facilitó la detección de problemas en la jerarquía de clases y permitió realizar ajustes de forma más rápida y fundamentada.

---

## Iteraciones

Primera iteración:
Se solicitó a Copilot un análisis general del sistema para detectar posibles violaciones al principio LSP. Se identificaron problemas en métodos sobrescritos.

Segunda iteración:
Se pidió a Copilot sugerencias de mejora. Se recomendó mantener contratos consistentes entre clases y evitar modificar validaciones.

Tercera iteración:
Se aplicaron los cambios sugeridos y se volvió a consultar a Copilot para validar el diseño. Se ajustaron detalles menores.

Resultado final:
El sistema quedó alineado con el principio de sustitución de Liskov, mejorando la robustez y la calidad del diseño.


