# Anexo - Introducción al Diseño Orientado a Objetos

# Los Cuatro Fundamentos de POO

# Requisitos iniciales del sistema

## **Requerimientos del usuario**

## RF1 – Gestión de turnos médicos

#### El sistema debe permitir gestionar los turnos del consultorio.

### Funcionalidades

- El sistema debe permitir agendar turnos, evitando superposición de horarios.

- El sistema debe permitir reprogramar turnos existentes.

- El sistema debe permitir cancelar turnos.

 -El sistema del paciente debe tener los botones de "Visualizar turnos disponibles", "Agendar" y "Cancelar" y en cada paso le debe llegar notificación al mail y whatsapp.

-La secretaria debe tener las opciones de "Agendar", "Reprogramar", "Cancelar" y "Notificar al paciente" para avisar en el sistema que ya el paciente se ha anunciado al turno, y esta debe aparecer en el sistema del doctor.

### Tipos de turnos

El sistema debe diferenciar entre:

- Control (15 minutos)

- Primera consulta (30 minutos)

### Gestión de horarios

- El sistema debe respetar el horario habitual:

    - Lunes a viernes de 9:00 a 13:00 y de 15:00 a 19:00.

- El sistema debe permitir:

   - Bloquear horarios por vacaciones o feriados y por actividades fijas


### Reglas del consultorio 

No se deben programar:

- Primeras consultas los viernes a última hora.

- Procedimientos los lunes.

- Los sobreturnos:

    - No deben generarse automáticamente.

     - Deben ser definidos manualmente por el médico, debe ser el único en tener acceso y toma de desición

- Tolerancia:

   - 10 minutos de llegada tarde.

  - La cancelación depende del médico.

## RF2 – Notificaciones
#### El sistema debe gestionar la comunicación con los pacientes.

- El sistema debe enviar recordatorios automáticos el día anterior al turno.

- El sistema debe notificar al paciente en caso de:

   - Reprogramación

    - Cancelación

## RF3 – Registro e historial

#### El sistema debe permitir registrar y auditar la información de los turnos.

- El sistema debe guardar un historial de cambios:

  - Usuario que realizó la acción

   - Tipo de modificación

- El sistema debe permitir:

  - Registrar la llegada del paciente

   - Registrar la hora real de llegada (esto lo marca la secretaria una vez llegado el paciente y haberse anunciado)

 #Requisitos no funcionales

  ## RNF1 -  Usabilidad
   - El sistema debe ser fácil de entender y ser práctico para la vista tanto del paciente como de la secretaria y el médico 
   - Debe ser una interfaz distinta tanto como para el paciente como para el médico y la secretaria


   ## RNF2 - Información y seguridad
   - El sistema debe contener datos de los pacientes, asi como su nombre, fecha de nacimiento, dni,  número de teléfono y mail, para poder enviar las notificaciones
   - El sistema debe detectar cuando es un usuario nuevo para que se reserve como "Primera consulta"
   - La información del paciente debe estar segura

# Casos de Uso
## CU 1 : Registro de Paciente
   - Actor:  Victoria / Dr.Molina
   - Objetivo: Permitir un nuevo paciente en el sistema.
   - Flujo principal: 
      - **1:** El/La recepcionista accede al sistema.
      - **2:** Selecciona la opción "Registrar paciente".
      - **3:** Ingresa los datos del paciente.
      - **4:** Confirma la información.
      - **5:** El sistema guarda los datos.
   - Precondición: El sistema está en funcionamiento.
   - Postcondición: El paciente queda registrado en el sistema.
## CU 2 : Agendar Turno
   - Actor: Victoria / Dr. Molina
   - Descripción: Permite reservar un espacio de tiempo en la agenda del profesional.
   - Flujo Principal:
      - **1:** La secretaria busca disponibilidad en la agenda del profesional.
      - **2:** Selecciona un horario libre y define el tipo de consulta para que el sistema estime la duración. 
      - **3:** El sistema valida que el horario este disponible y no haya conflictos con la agenda.
      - **4:** La secretaria asocia los datos del paciente al turno.
      - **5:** El sistema confirma la reserva y bloquea el horario.
   - Precondición: El profesional debe tener su disponibilidad registrada en el sistema.
   - Postcondición: El turno queda registrado en estado activo y el horario se visualiza como ocupado en la agenda.
## CU 3 : Reprogramar Turno
   - Actor: Victoria
   - Descripción: Modifica la fecha o el horario de un turno ya establecido previamente.
   - Flujo Principal:
      - **1:** La secretaria selecciona el turno existente desde la visualización de la agenda.
      - **2:** Selecciona la nueva fecha y hora solicitada por el paciente.
      - **3:** El sistema confirma que el nuevo horario solicitado esté disponible.
      - **4:** Se confirma el cambio, el sistema libera el horario anterior y ocupa el nuevo.
      - **5:** El sistema registra automáticamente el movimiento en el historial de cambios.
   - Precondición: Debe existir un turno agendado previamente en el sistema.
   - Postcondición: El turno se actualiza con la nueva información y el cambio queda registrado para auditoría.
## CU 4 : Cancelar Turno
   - Actor: Victoria
   - Descripción: Permite la cancelación de un turno existente.
   - Flujo Principal:
      - **1:** La secretaria busca el turno en la agenda.
      - **2:** Selecciona la opción de cancelar el turno.
      - **3:** El sistema solicita el registro del motivo de cancelación y lo guarda en el historial de cambios.
      - **4:** El sistema cambia el estado del turno a "cancelado" y libera el horario bloqueado.
      - **5:** El sistema notifica la cancelación del turno.
   - Precondición: Debe existir un turno previamente agendado.
   - Postcondición: El horario queda disponible nuevamente y se registra en el historial de cambios.
## CU 5 : Registro de llegada del paciente (Check-in)
   - Actor: Victoria
   - Descripción: Registrar la llegada del paciente al consultorio.
   - Flujo Principal:
      - **1:** El paciente llega a la recepción.
      - **2:** La secretaria busca el turno del paciente en la agenda.
      - **3:** La secretaria marca al paciente como "presente".
      - **4:** El sistema registra la hora real de llegada del paciente
      - **5:** El sistema envia una notificación automática al profesional anunciando la llegada del paciente.
   - Precondición: El paciente debe tener un turno agendada para la fecha.
   - Postcondición: El estado de presencia es visible para el profesional en su agenda, permitiéndole organizar el flujo de atención.
## CU 6 : Autorización de Sobreturno
   - Actores : Dr. Molina / Victoria
   - Descripción: Agregar turno en un horario ya ocupado o fuera del esquema habitual.
   - Flujo Principal:
      - **1:** La secretaria identifica un caso de urgencia o insistencia del paciente donde no hay disponibildad.
      - **2:** La secretaria consulta al profesional sobre la posibilidad de agendar un sobreturno.
      - **3:** El profesional autoriza la excepción(Limite de 2 sobreturnos)
      - **4:** La secretaria confirma e ingresa el nuevo turno en el sistema.
      - **5:** El sistema permite la superposición de un horario específico y registra la marca de Sobreturno.
   - Precondición: El horario solicitado debe estar ocupado o bloqueado por el sistema.
   - Postcondición: Se crea un turno adicional superpuesto que el profesional visualiza en su agenda diaria.
# Boceto inicial del diseño de clases
