# Uso de IA - Especialista de OCP

## Herramienta utilizada

Se utilizó gitHub Copilot en modo Agent integrado en VS Code

## Contexto proporciando

Se utilizó como base el archivo:
- `diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`
- `anexos/introduccion.md`
- `herramientas-agile/tarjetas-crc/`

## Prompt utilizado

```
Actuá como un Senior Software Engineer especializado en diseño orientado a objetos, DDD, SOLID (enfocado en Open/Closed Principle), refactoring y architecture review.

Usá como contexto estos artefactos:
- anexos/introduccion.md
- diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
- herramientas-agile/tarjetas-crc/ (todas las tarjetas CRC)

Objetivo:
Realizá una revisión de diseño enfocada exclusivamente en Open/Closed Principle (OCP) dentro del dominio del sistema de turnos.

Tarea:
1. Analizá el diseño actual e identificá clases que tengan lógica condicional que podría modelarse mejor mediante abstracciones, polimorfismo o composición. Prestá atención a:
- if/else o switch por tipos
- comportamiento gobernado por flags
- reglas de negocio variantes embebidas en una misma clase
- type checks o lógica dependiente de roles
- código que para agregar nuevos casos obliga a modificar la clase

2. Para cada caso detectado:
- Indicá la clase involucrada.
- Mostrá la lógica condicional o punto de variación problemático.
- Explicá por qué compromete OCP.
- Describí qué cambios futuros requerirían modificar código existente.

3. Proponé cómo rediseñarlo para permitir extensión sin modificación:
- uso de herencia cuando tenga sentido
- polimorfismo
- interfaces o estrategias
- composition over inheritance cuando sea mejor opción
- patrones relevantes (Strategy, State, Template Method, Specification, etc.)

4. Para cada propuesta indicá:
- Qué abstracción nueva aparece.
- Qué clases o variantes concretas surgirían.
- Cómo se agregaría una nueva variante sin tocar código existente.
- Cómo mejora extensibilidad y mantenibilidad.

5. Revisá críticamente tu propuesta:
- Verificá que no haya sobreingeniería.
- Cuestioná si alguna abstracción propuesta es artificial.
- Señalá casos donde NO conviene reemplazar condicionales por polimorfismo.
- Confirmá que las extensiones respetan el dominio del sistema de turnos.

Formato de salida:

## Hallazgos OCP

### Clase: <nombre>
- Lógica condicional detectada:
- Problema de diseño:
- Riesgo ante cambios:
- Refactor propuesto:
- Extensión vía polimorfismo/abstracción:
- Cómo agregar nuevos casos sin modificar código:
- Tradeoffs:

## Falsos positivos / Condicionales válidos
(identificá condicionales que deberían mantenerse y explicá por qué)

## Revisión crítica final
- Posibles sobre-abstracciones:
- Riesgos de diseño:
- Ajustes recomendados:

Criterios:
- No apliques OCP de forma mecánica.
- Priorizá coherencia con el dominio antes que “usar herencia porque sí”.
- Justificá cada propuesta desde reglas de negocio y evolución probable del sistema.
- Quiero una review como la haría un senior engineer en una design review, no una checklist académica.
```

## Resultados obtenidos

### Hallazgos OCP

---

### Clase: Turno (Estado)

- **Lógica condicional detectada:** El atributo `estado` (pendiente, confirmado, cancelado, completado) gobierna qué operaciones son válidas. Probablemente existe código como: `if (turno.getEstado().equals("CANCELADO")) { throw new Exception("No se puede reprogramar"); }` o `if (estado == "PENDIENTE") { permitirConfirmacion(); }`.

- **Problema de diseño:** La lógica de transiciones de estado está embebida en la clase Turno (o en servicios que la usan). Agregar un nuevo estado (ej: "EN_ESPERA", "SUSPENDIDO", "NO_ASISTIO") requiere modificar múltiples lugares donde se validan las operaciones.

- **Riesgo ante cambios:** Si RF1 evoluciona para incluir estados como "en espera por confirmación del paciente" o "suspendido por fuerza mayor", cada nueva regla de transición obliga a tocar código existente.

- **Refactor propuesto:** Introducir el patrón State con una jerarquía `EstadoTurno`:
  ```java
  abstract class EstadoTurno {
      abstract boolean puedeReprogramar();
      abstract boolean puedeCancelar();
      abstract boolean puedeConfirmar();
  }
  ```

- **Extensión vía polimorfismo/abstracción:** Crear subclases `Pendiente`, `Confirmado`, `Cancelado`, `Completado`. Cada una implementa los métodos según las reglas de su estado.

- **Cómo agregar nuevos casos sin modificar código:** Crear nueva clase `Suspendido` que extienda `EstadoTurno`. El código de Turno no se toca.

- **Tradeoffs:** Overhead inicial para estados simples. Justificado solo si los estados evolucionan más allá de los 4 actuales. En el MVP actual puede ser prematuro.

---

### Clase: Turno (Tipo de Consulta)

- **Lógica condicional detectada:** Según la introducción, existen `Control` (15 min) y `Primera vez` (30 min). La lógica probablemente tiene: `if (tipoConsulta.equals("PRIMERA_VEZ")) { duracion = 30; } else { duracion = 15; }`.

- **Problema de diseño:** La duración está calculada con condicionales. Agregar un nuevo tipo (ej: "Procedimiento", "Urgencia") requiere encontrar y modificar todos los lugares donde se calcula duración.

- **Riesgo ante cambios:** Si el dominio evoluciona hacia nuevos tipos de consulta con duraciones diferentes, hay que tocar múltiples lugares.

- **Refactor propuesto:** Patrón Strategy o jerarquía polimórfica para `TipoConsulta`:
  ```java
  abstract class TipoConsulta {
      abstract int getDuracionPrevista();
  }
  ```

- **Extensión vía polimorfismo/abstracción:** Crear `Control` (15 min), `PrimeraVez` (30 min), `Procedimiento` (45 min), etc.

- **Cómo agregar nuevos casos sin modificar código:** Crear nueva clase `Urgencia` con su duración. Sin tocar Turno.

- **Tradeoffs:** La introducción ya muestra este patrón como ejemplo de polimorfismo. Es coherente con el dominio y tiene bajo riesgo.

---

### Clase: Notificación (Tipo y Canal)

- **Lógica condicional detectada:** La clase tiene `tipo` (recordatorio, reprogramación, cancelación) y `canal` (mail, SMS, WhatsApp). Probablemente existe: `if (tipo.equals("CANCELACION")) { mensaje = generarMensajeCancelacion(); }`.

- **Problema de diseño:** La generación de mensajes y el envío por diferentes canales está mezclada. Agregar nuevos tipos o canales requiere modificar la lógica existente.

- **Riesgo ante cambios:** Si RF5 evoluciona hacia múltiples canales de notificación, la complejidad crece.

- **Refactor propuesto:** Dos abstracciones independientes:
  - `GeneradorMensaje` (Strategy) para el contenido
  - `CanalNotificacion` (Strategy) para el medio de envío

- **Extensión vía polimorfismo/abstracción:** `GeneradorRecordatorio`, `GeneradorReprogramacion`, `CanalEmail`, `CanalSMS`, etc.

- **Cómo agregar nuevos casos sin modificar código:** Crear nuevas clases para cada variante. Sin tocar código existente.

- **Tradeoffs:** Mayor complejidad estructural. RF5 menciona notificaciones pero no múltiples canales aún. Puede ser prematuro.

---

### Clase: Disponibilidad (Excepciones)

- **Lógica condicional detectada:** Maneja "horario habitual" vs "excepciones" (vacaciones, feriados). Probablemente: `if (esDiaBloqueado(fecha)) { return false; } else if (tieneExcepcion(fecha)) { usarHorarioEspecial(); }`.

- **Problema de diseño:** La lógica de validación de disponibilidad mezcla reglas diferentes. Agregar nuevos tipos de excepción requiere modificar esta lógica.

- **Riesgo ante cambios:** RF4 menciona explícitamente excepciones (vacaciones, feriados, actividades especiales). Si evoluciona, hay que tocar código.

- **Refactor propuesto:** Patrón Specification para reglas de disponibilidad:
  ```java
  abstract class ReglaDisponibilidad {
      abstract boolean aplica(Fecha fecha, Horario hora);
  }
  ```

- **Extensión vía polimorfismo/abstracción:** `ReglaHorarioHabitual`, `ReglaVacaciones`, `ReglaFeriado`, `ReglaCurso`, etc.

- **Cómo agregar nuevos casos sin modificar código:** Crear nueva clase `ReglaJornadaExtensa`. Sin tocar Disponibilidad.

- **Tradeoffs:** RF4 justifica esta variabilidad. La complejidad puede no justificarse si las excepciones son pocas.

---

### Clase: Agenda (Conflictos)

- **Lógica condicional detectada:** Verifica conflictos de horario. Probablemente: `if (turno.esSobreturno() && medico.autorizaSobreturno()) { permitir; } else { verificarConflicto(); }`.

- **Problema de diseño:** La lógica de validación de conflictos mezcla la regla de no-superposición con la autorización de sobreturnos.

- **Riesgo ante cambios:** RF2 menciona sobreturnos como caso especial. Agregar nuevas políticas requiere modificar esta lógica.

- **Refactor propuesto:** Separar la política de conflictos en una estrategia inyectable:
  ```java
  interface PoliticaConflictos {
      boolean verificar(Agenda agenda, Turno turno);
  }
  ```

- **Extensión vía polimorfismo/abstracción:** `PoliticaNoSuperposicion`, `PoliticaSobreturnoAutorizado`, `PoliticaBufferEntreTurnos`.

- **Cómo agregar nuevos casos sin modificar código:** Crear nueva clase `PoliticaBufferEntreTurnos`. Sin tocar Agenda.

- **Tradeoffs:** RF2 lo justifica. Coherente con el dominio.

---

## Falsos positivos / Condicionales válidos

### Herencia Persona → Paciente/Medico/Secretaria

- **Detección:** La introducción propone una jerarquía `Persona` → `Paciente, Medico, Secretaria`.

- **Por qué NO es un problema OCP:** No hay lógica condicional que varíe por tipo de persona. Cada subclase simplemente agrega atributos (dni, fechaNacimiento para Paciente; especialidad, matricula para Medico). No hay comportamiento que varíe polimórficamente.

- **Veredicto:** Este es un **falso positivo de OCP**. La herencia aquí es por relación "es-un" natural del dominio, no por evitar condicionales. Debe mantenerse solo por coherencia semántica, no por OCP.

---

### Validación de Conflictos en Agenda

- **Detección:** La verificación de si un turno tiene conflicto con otro existente.

- **Por qué puede ser válido:** Si la lógica es simple ("no permitir superposición excepto sobreturnos autorizados") y no hay variantes previstas, el condicional puede ser aceptable.

- **Veredicto:** Depende de la evolución de RF2. Si los sobreturnos son el único caso especial, el condicional puede mantenerse. Solo refactorizar si aparecen nuevas políticas.

---

## Revisión crítica final

### Posibles sobre-abstracciones:

| Propuesta | Riesgo | Recomendación |
|-----------|--------|---------------|
| EstadoTurno | Si los estados son estables (3-4), puede ser overkill | Implementar solo si RF1 evoluciona |
| Notificación (canales) | Medio-alto, introduce complejidad | Postergar hasta que RF5 requiera múltiples canales |
| TipoConsulta | Bajo riesgo, ya está en la visión | Mantener |
| Disponibilidad (reglas) | Medio, pero RF4 lo justifica | Evaluar evolución |
| Agenda (política conflictos) | Bajo, RF2 lo justifica | Mantener |

### Riesgos de diseño:

1. **Herencia automática**: No toda variación debe modelarse con herencia. Evaluar composición vs herencia en cada caso.

2. **Sobreingeniería prematura**: Aplicar OCP cuando los requisitos no evolucionan genera complejidad innecesaria.

3. **Inconsistencia entre propuestas**: Si se usa herencia para algunos casos y composición para otros, puede generar confusión.

4. **Abstracciones artificiales**: Crear jerarquías solo "por si acaso" sin justificación del dominio.

### Ajustes recomendados:

1. **Prioridad alta** (justificado por requisitos actuales):
   - `TipoConsulta` con polimorfismo (ya contemplado en la visión del proyecto)
   - `PoliticaConflictos` para sobreturnos (RF2)

2. **Prioridad media** (evaluar evolución):
   - `EstadoTurno` solo si los estados crecen
   - `ReglaDisponibilidad` si las excepciones aumentan

3. **Postergar** (sobreingeniería prematura):
   - Múltiples canales de notificación
   - Jerarquía de `GeneradorMensaje` compleja

4. **Mantener como está**:
   - Jerarquía `Persona` (falso positivo OCP)
   - Validación simple de conflictos (si no evoluciona)

---

## Ajustes realizados

### 1. Identificación de puntos de variación

Se analizaron las clases del modelo actual (`Turno`, `Notificacion`, `Disponibilidad`, `Agenda`) identificando dónde existe lógica condicional que podría violar OCP.

### 2. Clasificación de hallazgos

Se categorizaron los casos en:
- **Problemas OCP reales**: donde la extensión futura es probable y el refactor tiene sentido
- **Falsos positivos**: donde el condicional es aceptable y no requiere refactor

### 3. Priorización de propuestas

Se estableció una jerarquía de prioridad basada en los requisitos funcionales:
- **Alta prioridad**: `TipoConsulta` (ya contemplado en la visión), `PoliticaConflictos` (RF2)
- **Media prioridad**: `EstadoTurno`, `ReglaDisponibilidad`
- **Baja prioridad**: Canales de notificación múltiples

### 4. Documentación de tradeoffs

Para cada propuesta se documentaron los riesgos y beneficios, evitando aplicar OCP de forma mecánica.

---

## Conclusión

El análisis OCP del sistema de turnos médicos revela un modelo sólido para un MVP, con oportunidades de mejora identificadas pero no críticas.

### Hallazgos principales:

1. **5 casos de potencial violación OCP** identificados en las clases `Turno`, `Notificacion`, `Disponibilidad` y `Agenda`.

2. **1 falso positivo** detectado: la jerarquía `Persona` → `Paciente/Medico/Secretaria` no es un problema OCP sino una relación de dominio natural.

3. **2 propuestas de alto valor**: `TipoConsulta` y `PoliticaConflictos` están justificadas por los requisitos actuales (RF1, RF2).

4. **3 propuestas de baja prioridad**: pueden postergarse hasta que los requisitos evolucionen.

### Recomendación general:

**No aplicar refactorizaciones preventivas**. El modelo actual es extensible y las abstracciones propuestas deben implementarse solo cuando los requisitos evolucionen y justifiquen la complejidad.

La clave es priorizar diseño guiado por el dominio, no aplicación mecánica de SOLID. Las propuestas deben justificarse desde las reglas de negocio y la evolución probable del sistema.

---

## Iteraciones

### Iteración 1: Análisis inicial
- **Fecha**: 2026-04-25
- **Acción**: Revisión de artefactos (introduccion.md, tarjetas CRC, diagrama de clases)
- **Resultado**: Identificación de 5 casos potenciales de violación OCP

### Iteración 2: Clasificación y priorización
- **Fecha**: 2026-04-25
- **Acción**: Clasificación de hallazgos en problemas reales vs falsos positivos
- **Resultado**: 4 problemas reales + 1 falso positivo

### Iteración 3: Propuesta de refactor
- **Fecha**: 2026-04-25
- **Acción**: Diseño de soluciones usando patrones (State, Strategy, Specification)
- **Resultado**: Propuestas documentadas con tradeoffs

### Iteración 4: Revisión crítica
- **Fecha**: 2026-04-25
- **Acción**: Auditoría de propias propuestas para evitar sobreingeniería
- **Resultado**: Priorización final y ajustes recomendados