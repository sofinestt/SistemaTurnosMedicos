# Introducción al Diseño Orientado a Objetos

---

## Descripción del paradigma orientado a objetos

El paradigma orientado a objetos (POO) es un modelo de programación que organiza el software en torno a **objetos**: entidades que combinan datos (atributos) y comportamiento (métodos) en una única unidad. En lugar de pensar el programa como una secuencia de instrucciones, la POO nos invita a modelar el mundo real como un conjunto de objetos que interactúan entre sí.

Es importante porque permite construir sistemas más fáciles de entender, mantener y extender. Cuando el modelo refleja fielmente cómo funciona el dominio real —en nuestro caso, un consultorio médico— los cambios futuros se pueden introducir sin romper lo que ya funciona. La POO también favorece la reutilización del código y la separación clara de responsabilidades entre las partes del sistema.

---

## Los cuatro fundamentos de POO

### 1. Encapsulamiento

El encapsulamiento consiste en ocultar el estado interno de un objeto y exponer solo las operaciones que otros objetos pueden usar para interactuar con él.

**Ejemplo en el proyecto:** La clase `Agenda` es la única responsable de agregar, quitar y verificar turnos. Ninguna otra parte del sistema puede manipular directamente la lista interna de turnos. Si la secretaria quiere agregar un turno, debe hacerlo a través de la operación `agregarTurno()` de la agenda, que internamente verifica que no haya conflictos de horario. Esto garantiza que la regla de no superposición nunca pueda ser violada accidentalmente.

```
Agenda
─────────────────────────
- turnos: lista (privada)
─────────────────────────
+ agregarTurno()
+ eliminarTurno()
+ verificarConflicto()
+ mostrarAgendaDiaria()
```

### 2. Abstracción

La abstracción consiste en representar solo los aspectos relevantes de una entidad para el sistema, dejando de lado los detalles que no importan en este contexto.

**Ejemplo en el proyecto:** Un `Paciente` en este sistema no es una persona completa — es la representación mínima necesaria para gestionar turnos: nombre, DNI, teléfono y mail. No modelamos su historial médico, su obra social ni sus antecedentes clínicos, porque eso está fuera del alcance del sistema de turnos. La abstracción nos permite enfocarnos en lo que el sistema realmente necesita saber.

```
Paciente
─────────────────────────
- nombre
- dni
- telefono
- mail
─────────────────────────
+ pedirTurno()
+ cancelarTurno()
```

### 3. Herencia

La herencia permite que una clase (subclase) comparta atributos y comportamiento de otra clase (superclase), evitando duplicación y estableciendo una jerarquía natural entre conceptos del dominio.

**Ejemplo en el proyecto:** Tanto `Paciente` como `Medico` y `Secretaria` comparten características básicas de una persona: nombre, apellido, teléfono y mail. Podría existir una clase base `Persona` que contenga esos atributos comunes, y cada rol especializado herede de ella agregando solo lo que le es propio. El `Medico` agrega matrícula y especialidad; la `Secretaria` agrega sus operaciones de gestión de agenda.

```
        Persona
       /       \
  Paciente    Medico    Secretaria
```

> Decisión de diseño: en el MVP esta jerarquía está identificada pero no implementada todavía. El modelo está preparado para incorporarla sin romper la estructura existente.

### 4. Polimorfismo

El polimorfismo permite que objetos de distintos tipos respondan al mismo mensaje de formas diferentes, según su naturaleza específica.

**Ejemplo en el proyecto:** El sistema maneja distintos tipos de consulta (`Control` y `Primera vez`), y cada uno tiene una duración estimada diferente (15 y 30 minutos respectivamente). El método `calcularDuracion()` puede comportarse de forma distinta según el tipo de consulta, sin que el resto del sistema necesite saber cuál es. En el futuro, si se agregan nuevos tipos de consulta (por ejemplo, `Procedimiento`), solo se necesita definir su propio comportamiento sin modificar el código existente.

```
TipoConsulta (abstracto)
+ calcularDuracion()

  Control            PrimeraVez
  + calcularDuracion()  + calcularDuracion()
    → 15 min              → 30 min
```

---

## Requisitos iniciales del sistema

> Los siguientes requisitos fueron extraídos de los materiales provistos por el cliente: mail de la secretaria, transcripción de la reunión de kickoff, notas del cuaderno del Dr. Molina, mensajes de WhatsApp y conversaciones de Slack del equipo de desarrollo.

### Requisitos Funcionales

**RF1 — Gestión de turnos**
El sistema debe permitir crear, reprogramar y cancelar turnos. Al crear un turno, el sistema debe verificar que no exista otro turno en el mismo horario para el mismo profesional. La operación de reprogramación debe registrar el cambio en el historial. La cancelación puede ser iniciada por la secretaria o por el propio paciente, pero siempre debe quedar registrada.

**RF2 — Control de superposición y sobreturnos**
El sistema debe impedir que se asignen dos turnos en el mismo horario, salvo cuando el médico autorice explícitamente un sobreturno. Los sobreturnos no se generan automáticamente: deben ser habilitados manualmente por el profesional. El sistema debe registrar que un turno es sobreturno y quién lo autorizó.

**RF3 — Visualización de agenda**
El sistema debe permitir visualizar la agenda del profesional por día y por semana. La vista debe mostrar los turnos existentes, los horarios bloqueados y los sobreturnos diferenciados. La secretaria y el médico deben poder acceder a esta vista.

**RF4 — Gestión de disponibilidad del profesional**
El sistema debe permitir definir el horario habitual del profesional (lunes a viernes de 9:00 a 13:00 y de 15:00 a 19:00) y registrar excepciones: días bloqueados por vacaciones, feriados o actividades especiales. Los sábados son opcionales y se definen mes a mes. El sistema no debe permitir agendar turnos en horarios bloqueados.

**RF5 — Notificaciones al paciente**
El sistema debe enviar notificaciones automáticas al paciente en los siguientes casos: recordatorio el día anterior al turno (y opcionalmente el mismo día a la mañana), aviso de reprogramación y aviso de cancelación. El canal preferido es WhatsApp; el mail es alternativa. Las notificaciones deben quedar registradas en el sistema.

**RF6 — Registro de llegada del paciente**
El sistema debe permitir registrar la llegada física del paciente al consultorio. Al registrar la llegada, se debe guardar la hora real de arribo. Este registro es creado por la secretaria cuando el paciente se anuncia en recepción. Si el paciente nunca llega, el registro no existe. La tolerancia de llegada es de 10 minutos, pero la decisión de atender o reprogramar al paciente tardío la toma el médico caso por caso.

**RF7 — Historial de cambios**
El sistema debe mantener un historial completo de todas las modificaciones realizadas sobre los turnos: quién realizó el cambio, qué tipo de modificación fue y en qué momento ocurrió. Este historial debe ser consultable para resolver disputas entre pacientes y personal del consultorio.

### Requisitos No Funcionales

**RNF1 — Usabilidad**
El sistema debe tener interfaces diferenciadas para cada actor: secretaria, médico y paciente. La interfaz debe ser intuitiva y no requerir capacitación extensa. El médico en particular necesita una vista simple que no le demande aprender funcionalidades complejas.

**RNF2 — Seguridad y privacidad**
Los datos personales de los pacientes (nombre, DNI, fecha de nacimiento, teléfono, mail) deben estar protegidos. El acceso al sistema debe estar restringido por rol: la secretaria gestiona la agenda, el médico autoriza sobreturnos y consulta su agenda, el paciente solo ve y gestiona sus propios turnos.

**RNF3 — Extensibilidad**
El sistema debe estar diseñado para soportar el crecimiento a múltiples profesionales sin necesidad de rediseñar la arquitectura. La agenda debe pertenecer al profesional como concepto, de forma que agregar un segundo médico implique simplemente crear una nueva agenda, no modificar la lógica existente.

**RNF4 — Disponibilidad y confiabilidad**
El sistema debe estar disponible en el horario de atención del consultorio. No debe perder datos ante errores. Las operaciones críticas (crear, cancelar, reprogramar turno) deben confirmarse antes de ejecutarse.

**RNF5 — Trazabilidad**
Toda acción realizada en el sistema debe quedar registrada con fecha, hora y usuario responsable. Esto aplica especialmente a modificaciones de turnos, autorizaciones de sobreturnos y registros de llegada, ya que el consultorio necesita poder auditar quién hizo qué ante cualquier disputa.

---

## Casos de uso

### CU1 — Crear turno

**Actor principal:** Secretaria  
**Actores secundarios:** Paciente (origen de la solicitud), Médico (dueño de la agenda)

**Descripción:** La secretaria registra un nuevo turno en la agenda del profesional a partir de una solicitud del paciente.

**Precondiciones:**
- El paciente existe en el sistema o sus datos están disponibles para registrarlo.
- El profesional tiene horario disponible en la franja solicitada.
- La secretaria tiene sesión iniciada en el sistema.

**Flujo principal:**
1. La secretaria selecciona la opción "Nuevo turno" en el sistema.
2. El sistema solicita los datos del paciente (nombre, DNI o identificador).
3. La secretaria ingresa o selecciona el paciente existente.
4. El sistema solicita fecha, hora y tipo de consulta (Control o Primera vez).
5. La secretaria ingresa los datos del turno.
6. El sistema verifica que el horario esté disponible y no exista superposición.
7. El sistema registra el turno y lo muestra en la agenda del profesional.
8. El sistema envía una notificación de confirmación al paciente.

**Flujos alternativos:**
- *Horario ocupado:* En el paso 6, si el horario no está disponible, el sistema informa el conflicto y sugiere horarios alternativos. La secretaria puede seleccionar uno o consultar con el médico para autorizar un sobreturno.
- *Paciente nuevo:* En el paso 3, si el paciente no existe, el sistema permite registrarlo con sus datos básicos antes de continuar.

**Postcondiciones:**
- El turno queda registrado en la agenda del profesional.
- El paciente recibe notificación de confirmación.
- El evento queda registrado en el historial de cambios.

---

### CU2 — Reprogramar turno

**Actor principal:** Secretaria  
**Actores secundarios:** Paciente (afectado por el cambio)

**Descripción:** La secretaria modifica la fecha u hora de un turno existente, ya sea por solicitud del paciente o por necesidad del consultorio.

**Precondiciones:**
- El turno existe en el sistema y está en estado activo.
- La secretaria tiene sesión iniciada.

**Flujo principal:**
1. La secretaria busca el turno a modificar por nombre del paciente o fecha.
2. El sistema muestra los datos del turno actual.
3. La secretaria selecciona la opción "Reprogramar".
4. El sistema solicita la nueva fecha y hora.
5. La secretaria ingresa el nuevo horario deseado.
6. El sistema verifica disponibilidad en el nuevo horario.
7. El sistema actualiza el turno con la nueva fecha y hora.
8. El sistema registra el cambio en el historial (quién reprogramó, cuándo, desde qué horario hacia cuál).
9. El sistema envía notificación al paciente informando el nuevo horario.

**Flujos alternativos:**
- *Nuevo horario ocupado:* En el paso 6, el sistema informa el conflicto y la secretaria debe elegir otro horario.

**Postcondiciones:**
- El turno refleja el nuevo horario.
- El paciente fue notificado del cambio.
- El historial registra la reprogramación con todos sus datos.

---

### CU3 — Cancelar turno

**Actor principal:** Secretaria o Paciente  
**Actores secundarios:** Médico (puede ser afectado)

**Descripción:** Un turno existente es cancelado, ya sea por solicitud del paciente o por decisión del consultorio.

**Precondiciones:**
- El turno existe y está en estado activo.
- El actor que cancela tiene los permisos correspondientes.

**Flujo principal:**
1. El actor (secretaria o paciente) selecciona el turno a cancelar.
2. El sistema muestra los datos del turno.
3. El actor confirma la cancelación.
4. El sistema cambia el estado del turno a "Cancelado".
5. El sistema registra la cancelación en el historial: quién canceló, fecha y hora de la cancelación.
6. El sistema envía notificación al paciente confirmando la cancelación.
7. Si corresponde, el sistema notifica al médico del cambio en su agenda.

**Flujos alternativos:**
- *Cancelación por paciente:* El paciente solo puede cancelar sus propios turnos. El sistema registra que la cancelación fue iniciada por el paciente.

**Postcondiciones:**
- El turno queda en estado "Cancelado" y el horario queda libre en la agenda.
- El historial registra la cancelación con todos sus datos.
- El paciente fue notificado.

---

### CU4 — Autorizar sobreturno

**Actor principal:** Médico  
**Actores secundarios:** Secretaria (ejecuta la carga una vez autorizado)

**Descripción:** El médico habilita manualmente un sobreturno en un horario que ya tiene otro turno asignado, cuando considera necesario atender a un paciente adicional.

**Precondiciones:**
- El médico tiene sesión iniciada en el sistema.
- Existe al menos un turno en el horario donde se quiere agregar el sobreturno.

**Flujo principal:**
1. El médico accede a su agenda y visualiza el horario con conflicto.
2. El médico selecciona la opción "Autorizar sobreturno" para ese horario.
3. El sistema solicita confirmación explícita del médico.
4. El médico confirma la autorización.
5. El sistema registra el sobreturno como autorizado, indicando el médico que autorizó y el momento de la autorización.
6. La secretaria puede ahora crear el turno adicional en ese horario.
7. El sistema muestra el sobreturno diferenciado visualmente en la agenda.

**Flujos alternativos:**
- *Médico no disponible:* Si el médico no está presente, el sobreturno no puede ser autorizado, salvo urgencia real evaluada caso por caso.

**Postcondiciones:**
- El sobreturno queda registrado como autorizado por el médico.
- La secretaria puede proceder a cargar el turno adicional.
- El evento queda en el historial.

---

### CU5 — Registrar llegada del paciente

**Actor principal:** Secretaria  
**Actores secundarios:** Paciente (se presenta físicamente), Médico (ve el estado en su agenda)

**Descripción:** La secretaria registra que el paciente llegó físicamente al consultorio, indicando la hora real de arribo.

**Precondiciones:**
- Existe un turno activo para el paciente en el día actual.
- La secretaria tiene sesión iniciada.
- El paciente se presentó en recepción.

**Flujo principal:**
1. La secretaria busca el turno del paciente por nombre o por la agenda del día.
2. El sistema muestra el turno con su horario previsto.
3. La secretaria selecciona la opción "Registrar llegada".
4. El sistema registra la hora actual como hora real de llegada del paciente.
5. El sistema crea un registro de presencia asociado al turno.
6. El sistema actualiza el estado del turno a "Paciente presente".
7. El médico puede ver en su agenda que el paciente ya está en sala de espera.

**Flujos alternativos:**
- *Paciente tardío:* Si el paciente llega después de los 10 minutos de tolerancia, el sistema registra igualmente la llegada pero la secretaria debe consultar al médico si lo atiende o reprograma. El médico decide caso por caso.
- *Paciente no llega:* Si el paciente no se presenta, no se crea ningún registro de presencia. El turno puede marcarse como "Ausente" al finalizar el horario.

**Postcondiciones:**
- El registro de llegada queda guardado con la hora real de arribo.
- El médico puede ver el estado actualizado en su agenda.
- Si el paciente llegó tarde, queda registrado para referencia futura.

---

## Boceto inicial del diseño de clases

El diagrama de clases inicial se encuentra en:

📁 [`diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw`](../diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw)

![Boceto inicial de clases](../diagramas/01-diagrama-clases/01-boceto-inicial.png)

### Clases identificadas

| Clase | Responsabilidad principal |
|---|---|
| `Paciente` | Representa a la persona que solicita turnos |
| `Medico` | Profesional dueño de la agenda; autoriza sobreturnos |
| `Secretaria` | Opera la agenda en nombre del médico |
| `Turno` | Unidad central del sistema; conecta paciente, médico y tiempo |
| `Agenda` | Controla la disponibilidad y evita conflictos de horario |
| `Disponibilidad` | Define los bloques de tiempo habilitados o bloqueados |
| `RegistroPresencia` | Registra la llegada física del paciente al consultorio |

### Decisiones de diseño justificadas

- **`Agenda` como controlador central:** La agenda es la única responsable de verificar conflictos. Ninguna otra clase puede agregar turnos directamente — esto garantiza la regla de no superposición (encapsulamiento).
- **`Disponibilidad` como clase separada:** Los horarios habituales y las excepciones (sábados variables, feriados, vacaciones) se modelan como objetos propios, no como simples atributos de fecha, porque tienen comportamiento propio (`esDisponible()`, `bloquearHorario()`).
- **`RegistroPresencia` como entidad independiente:** La llegada del paciente no es un estado del turno — es un evento físico que puede ocurrir antes del horario previsto o no ocurrir nunca. Modelarlo como clase separada preserva esta distinción (abstracción).
- **Herencia pendiente:** `Paciente`, `Medico` y `Secretaria` comparten atributos de persona. La jerarquía con clase base `Persona` está identificada pero no implementada en esta etapa — decisión consciente para mantener el modelo simple en el MVP.