# Actores involucrados en el sistema:
Secretaria (Victoria).



Profesional/Médico (Dr.Molina).

Paciente (Cualquier tipo de paciente involucrado de manera indirecta con el sistema).

# Casos de uso:
__Agendar Turno:__ registrar una cita nueva distinguiendo si es "Control" o "Primera vez".

__Horarios del consultorio:__ Contiene horarios habituales: lunes a viernes de 09:00 a 13:00; Y de 15:00 a 19:00.

__Duración de turnos distinguiendo si es "Control" o "Primera vez":__ Utilizado para estimar la duración (15 o 30 minutos).

__Reprogramar turno:__ mover los turnos si hay que reprogramar.

__Cancelar turno:__ dar de baja un turno a pedido del paciente o del doctor.

__Visualizar agenda:__ poder ver la agenda del día o de la semana sin tener que revisar todo a mano.

__Lista de espera:__ permiten consultar la posición o el tiempo estimado de espera.

__Bloquear horarios:__ cuando hay vacaciones, reuniones o feriados.

__Autorizar sobreturno:__ agregar manualmente un paciente en un horario ya ocupado.

__Enviar recordatorios automáticos el día anterior:__ enviarlos mediante WhatsApp o email también si no hay otra opción.

__Registrar llegada del paciente (Check-in):__ marcar cuando el paciente ya está físicamente en la sala de espera.

# Casos de uso completos:

## Agendar turno
__actor principal:__ secretaria (Victoria).

__Descripción:__ buscamos reservar un espacio de tiempo para un paciente en la agenda del profesional de la salud, para asi evitar superposiciones entre pacientes.

__Precondiciones:__ el profesional de salud debe tener disponibilidad horaria para poder atender en el horario solicitado.
__Flujo básico:__   
 1. La secretaria selecciona al profesional y el día en la agenda.

 2. El sistema muestra los bloques libres según el horario base y excepciones.

 3. La secretaria ingresa los datos del paciente (nombre, teléfono, DNI, etc).

 4. La secretaria elige el tipo de consulta: "Control" (15 min) o "Primera vez" (30 min).

 5. El sistema válida que no haya otro turno en ese horario.

 6. El sistema confirma y guarda el turno con estado "Activo".

#### __Flujos alternativos:__
__en caso de horarios ocupados:__
1.Se busca otra fecha accesible para el paciente y el profesional.

2. Un sobreturno autorizado por el profesional de la salud.

__poscondiciones:__ turno queda registrado y el tiempo bloqueado en la agenda.


 ## Registrar Llegada del Paciente
__Actor Principal:__ Secretaria (Victoria)

__Descripcion:__   Permite marcar a un paciente si ha llegado o no  físicamente al consultorio para que el médico sepa que puede llamarlo.

__Precondiciones:__    El paciente debe tener un turno agendado para el día y hora actual.

__Flujo Básico:__ 
  1. El paciente se presenta en el consultorio.

 2. La Secretaria busca el turno en la agenda del día y si los datos coinciden con el paciente.
3. La Secretaria marca al paciente como "Presente".
 4. El sistema registra automáticamente la hora real de llegada.

__Flujo Alternativo:__ 

 - Llegada tarde:  Si el paciente llega después de los 10 minutos de tolerancia, el sistema registra la llegada pero muestra una alerta de retraso.(queda en decisión del profesional  si atender o cancelar la consulta)

- si el paciente no asiste se lo tacha como”Ausente”

- si el paciente cancela la consulta,se borra y se sigue con la consulta que le prosigue.

__postcondiciones:__  El estado del turno cambia a "Presente" o ”Ausente” y  es visible en la agenda del profesional.

## Autorizar Sobreturno

__Actor Principal:__  Profesional(Dr.Molina) 

__Descripción:__  Permitir la asignación de un turno en un horario que ya se encontraba ocupado y usarlo para atender a otro paciente, deben ser definidos manualmente por el médico, debe ser el único en tener acceso y toma de decisión.

__Precondiciones:__  El horario deseado ya debe estar reservado por otro paciente.

__Flujo Básico:__ 

- el profesional intenta agregar una consulta en un horario ocupado.

- el profesional accede a la agenda de horarios y emite un sobreturno en el sistema.

- Tras la aprobación, el sistema guarda el segundo turno en el mismo bloque de horario.


__Flujo Alternativo:__ 

- Límite Alcanzado: Si ya hay dos sobreturnos en el día, el sistema muestra una advertencia recordando la preferencia del médico de no exceder ese número.

__Postcondiciones:__ Coexisten dos turnos en el mismo horario en la agenda.


## Cancelar un turno
__Actor Principal:__ Secretaria o paciente  

__Descripción:__ permitir la cancelación de un turno o consulta médica por sucesos ocurridos que impidan al paciente o profesional asistir.

__Postcondiciones:__ debe existir un turno previamente registrado en el sistema.

__Flujo Básico:__ 

-  La Secretaria localiza el turno en la agenda y cancela el turno.

- El sistema solicita (opcionalmente) el motivo de la cancelación para que quede asentado si fue por pedido del paciente o decisión del médico.

- El sistema actualiza el estado del turno de "Activo" a *"Cancelado".

-  El sistema registra automáticamente quién  canceló la consulta  y cuándo sucedió el cambio .

- El sistema dispara una notificación obligatoria al paciente (preferentemente por WhatsApp) para confirmar la cancelación.
 
 __Flujos Alternativos:__ se solicita una reprogramacion en caso de que cancele el profesional.

 __Postcondiciones:__ El horario queda libre en la agenda( en caso de que cancele el paciente) o se realiza una reprogramación( en caso de que cancele el profesional).


## bloquear horarios:
__Actor Principal:__ Secretaria (Valeria)

__Descripción:__  Permite definir periodos de tiempo en los que el profesional no estará disponible para atender pacientes, asegurando que el sistema impida automáticamente la reserva de turnos en esos bloques.

__Precondciones:__ notificar días y horarios donde el profesional no se encuentra activo o trabajando.

__Flujo Básico:__ La secretaria accede a la configuración de disponibilidad del profesional
Selecciona el rango de fechas u horas a bloquear (ej. vacaciones, feriados o las clases del doctor los jueves)


__Flujos Alternativos:__ cualquier paciente que necesite atencion debera programar su consulta a fechas donde se encuentre disponible el profesional.

__Postcondiciones:__ Los horarios bloqueados no aparecen como opción al intentar agendar nuevos turnos