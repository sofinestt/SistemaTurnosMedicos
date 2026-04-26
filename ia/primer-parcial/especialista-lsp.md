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

## Resultados obtenidos

## Ajustes Realizados

## Conclusión

## Iteraciones


