# Principio de Sustitución de Liskov (LSP)

## Propósito y Tipo del principio SOLID

El principio de sustitución de Liskov (LSP) establece que los objetos de una superclase deben poder ser reemplazados por objetos de sus subclases sin alterar el funcionamiento correcto del programa.

En otras palabras: si S es una subclase de T, entonces los objetos de tipo T deberían poder ser reemplazados por objetos de tipo S sin modificar las propiedades correctas del programa.

## Motivación
En el diseño orientado a objetos, las jerarquías de herencia pueden parecer correctas a simple vista, pero pueden ocultar violaciones sutiles del contrato entre clases. LSP nos ayuda a detectar.

El problema en el sistema de turnos:

Imaginemos que tenemos una jerarquía `Persona` → `Paciente, Medico, Secretaria`. Ésta herencia desde el punto de vista de LSP, tiene un problema, la clase `Secretaria` no agrega ninguna propiedad nueva. Hereda exactamente las mismas que `Persona`.

Veamos las propiedades de cada clase:

| Clase | Propiedades |
|-------|-------------|
| Persona | nombre, apellido, telefono, mail |
| Paciente | dni, fechaNacimiento, telefono, mail |
| Medico | especialidad, matricula, telefono, mail |
| Secretaria | (sin propiedades adicionales) |

**Problema detectado:**  ¿Es esto una violación de LSP?

---

## Análisis de jerarquías

### Jerarquía 1: Persona → Paciente, Medico, Secretaria

- **Evaluación de sustitución:**
  La relación "es-un" entre Persona y sus subclases es conceptualmente válida: un Paciente "es una" Persona, un Médico "es una" Persona, una Secretaria "es una" Persona. Sin embargo, hay matices que evaluar.

- **Contrato esperado:**
  Una Persona debe tener: nombre, apellido, teléfono y mail. Las subclases heredan estos datos y pueden agregar los propios.

- **Riesgos o violaciones detectadas:**
  1. **Secretaria como "falso subtipo"**: La clase Secretaria no agrega propiedades adicionales. Solo hereda los datos de Persona. Si el sistema solo necesita los datos básicos de persona, la herencia funciona. Pero si hay código que espera que cada subclase tenga comportamiento distintivo, podría haber una sorpresa.

  2. **Propiedades duplicadas en Paciente y Medico**: Ambos tienen telefono y mail en sus propiedades, además de los específicos (dni, fechaNacimiento para Paciente; especialidad, matricula para Medico). Esto indica que probablemente heredan de Persona, pero las tarjetas CRC muestran estos datos como propios, lo que podría causar confusión.

  3. **Relación semántica cuestionable**: En el dominio del sistema de turnos, una Secretaria no es simplemente "una persona con datos". Es un **rol operativo** que realiza acciones específicas (gestionar agenda, notificar pacientes). La herencia por datos no captura esta semántica de rol.

- **Ejemplo de ruptura de comportamiento (si aplica):**
  ```java
  // Código cliente que espera una Persona
  void saludarPersona(Persona p) {
      System.out.println("Hola, " + p.getNombre());
  }
  
  // Llamada con diferentes subtipos
  saludarPersona(new Paciente()); // ✅ Funciona
  saludarPersona(new Medico());   // ✅ Funciona
  saludarPersona(new Secretaria()); // ✅ Funciona
  
  // Pero si el código hace:
  void procesarRol(Persona p) {
      if (p instanceof Medico) {
          ((Medico)p).autorizarSobreturno();
      }
      // ...
  }
  ```
  Este código viola LSP porque depende del tipo concreto en lugar de usar polimorfismo.

- **Rediseño recomendado:**
  1. **Opción A - Mantener la jerarquía**: Si la relación semántica "es-un" tiene sentido en el dominio, mantenerla. Pero documentar que Secretaria es un rol, no una entidad con estado propio.

  2. **Opción B - Usar composición en lugar de herencia**: Si la Secretaria es solo un rol que opera sobre la Agenda, podría modelarse como:
     ```java
     class Agenda {
         private Secretaria secretaria; // Composición
         // ...
     }
     ```

  3. **Opción C - Separar conceptos**: Si los datos son los mismos pero el comportamiento es diferente, considerar si la herencia es la mejor opción.

---

### Jerarquía 2: TipoConsulta → Control, PrimeraVez (propuesta OCP)

- **Evaluación de sustitución:**
  Esta es una jerarquía propuesta en la introducción para resolver el problema OCP de duración de consultas. Analicemos si cumple con LSP.

- **Contrato esperado:**
  - `getDuracionPrevista()`: retorna la duración en minutos
  - `getDescripcion()`: retorna el nombre del tipo de consulta

- **Riesgos o violaciones detectadas:**
  ✅ **Sin violaciones detectadas**. La jerarquía cumple con LSP:
  - Cada subclase implementa completamente el contrato de la superclase
  - No hay precondiciones más fuertes
  - No hay postcondiciones debilitadas
  - No se cambia el comportamiento esperado
  - La relación "es-un" es válida: un Control "es una" consulta, una PrimeraVez "es una" consulta

- **Ejemplo de ruptura de comportamiento (si aplica):**
  No hay ruptura. El código cliente puede usar cualquier subclase indistintamente:
  ```java
  // Código cliente
  int duracion = turno.getTipoConsulta().getDuracionPrevista();
  
  // Funciona con cualquier subtipo
  turno.setTipoConsulta(new Control());      // → 15 min
  turno.setTipoConsulta(new PrimeraVez());   // → 30 min
  ```

- **Rediseño recomendado:**
  ✅ **No es necesario redesignar**. La jerarquía está bien modelada.

---

### Jerarquía 3: EstadoTurno (propuesta OCP - análisis teórico)

- **Evaluación de sustitución:**
  Si implementamos una jerarquía EstadoTurno para resolver el problema OCP de estados de turno, debemos verificar que cumpla con LSP.

- **Contrato esperado:**
  - `puedeReprogramar()`: retorna boolean
  - `puedeCancelar()`: retorna boolean
  - `puedeConfirmar()`: retorna boolean

- **Riesgos o violaciones detectadas:**
  ⚠️ **Riesgo potencial**: Si una subclase lanza excepción en lugar de retornar boolean, rompe el contrato:
  ```java
  // ❌ Violación LSP
  class Cancelado extends EstadoTurno {
      boolean puedeReprogramar() {
          throw new OperacionNoPermitidaException(); // ⚠️ Rompe el contrato
      }
  }
  ```

- **Ejemplo de ruptura de comportamiento (si aplica):**
  ```java
  // Código cliente
  if (estado.puedeReprogramar()) { // Espera boolean
      turno.reprogramar();
  }
  
  // Si Cancelado lanza excepción, el flujo se interrumpe
  ```

- **Rediseño recomendado:**
  Usar retornos booleanos, no excepciones, para consultas de capacidad:
  ```java
  // ✅ Cumple LSP
  class Cancelado extends EstadoTurno {
      boolean puedeReprogramar() {
          return false; // Retorna boolean, no lanza excepción
      }
  }
  ```

---

## Falsos positivos / Jerarquías válidas

### TipoConsulta → Control, PrimeraVez

- **Detección:** La introducción propone esta jerarquía para resolver OCP.

- **Por qué es válida:**
  - La relación "es-un" es semánticamente correcta
  - Cada subclase implementa completamente el contrato
  - No hay precondiciones más fuertes
  - No hay postcondiciones debilitadas
  - El comportamiento es predecible y consistente

- **Veredicto:** ✅ **Jerarquía válida**. No hay violación LSP.

---

### Herencia de datos compartidos (Persona)

- **Detección:** La jerarquía Persona → Paciente/Medico/Secretaria comparte datos.

- **Por qué puede ser válida:**
  - Si el dominio justifica que todos son "personas" en el contexto del consultorio
  - Si hay operaciones que trabajan con la abstracción Persona
  - Si no hay comportamiento que varíe polimórficamente

- **Veredicto:** ⚠️ **Depende del contexto**. Puede ser válida si la semántica del dominio lo justifica. La Secretaria podría ser un caso borderline.

---

## Revisión crítica final

### Riesgos en las jerarquías actuales:

| Jerarquía | Riesgo | Nivel |
|-----------|--------|-------|
| Persona → Secretaria | Falso subtipo | Medio |
| Persona → Paciente/Medico | Propiedades duplicadas en tarjetas | Bajo |
| TipoConsulta → Control/PrimeraVez | Sin riesgos | ✅ |
| EstadoTurno (propuesta) | Excepciones en lugar de booleanos | Medio |


### Observaciones de modelado de dominio:

1. **La herencia no siempre es la mejor opción**: En el caso de Secretaria, podría ser más apropiado modelarla como un rol o composición, no como una subclase de Persona.

2. **El comportamiento importa más que los datos**: La herencia basada solo en datos compartidos puede ser una señal de alerta. La herencia debería usarse cuando hay comportamiento que compartir o especializar.

3. **LSP complementa OCP**: Las jerarquías propuestas para resolver OCP (como TipoConsulta) deben verificarse también contra LSP. Una solución OCP que viola LSP no es una buena solución.

---

## Conclusión

El análisis LSP del sistema de turnos médicos revela:

1. **1 jerarquía potencialmente problemática**: Persona → Secretaria (posible falso subtipo)

2. **1 jerarquía válida**: TipoConsulta → Control, PrimeraVez (cumple LSP)

3. **1 propuesta de riesgo**: EstadoTurno (si se implementa con excepciones)

### Recomendación general:

- Mantener la jerarquía TipoConsulta (está bien diseñada)
- Evaluar la jerarquía Persona → Secretaria (puede ser composición en lugar de herencia)
- Documentar el comportamiento de EstadoTurno si se implementa (usar booleanos, no excepciones)

La clave es priorizar la semántica del dominio sobre la pureza teórica. Si la herencia tiene sentido en el contexto del negocio, usarla. Si no, preferir composición o roles.