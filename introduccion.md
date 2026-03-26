# Anexo - Introducción al Diseño Orientado a Objetos

El paradigma orientado a objetos (POO) es un modelo de programación que organiza el desarrollo de software utilizando objetos, los cuales representan elementos del mundo real y contienen datos (atributos) y funciones (métodos) que permiten trabajar con esos datos.

En la programación orientada a objetos, los programas se estructuran a partir de clases, que funcionan como plantillas para crear objetos.

---

## Los Cuatros Fundamentos del POO ##  
### Encapsulamiento
Es donde se van proteger los datos y no cualquiera tiene el acceso a estos datos.

En el sistema planteado, por ejemplo, la clase Paciente no debería permitir que cualquiera cambie sus datos directamente.

### Herencia
Va permitir crear clases a partir de otras.

Por ejemplo: hay clases: doctor, paciente, secretaria, dichas clases pueden compartir __(nombre, apellido, edad,email, telefono)__. 
Cada uno hereda los datos básicos pero tiene funciones distintas.
### Polimorfismo
Significa que varios objetos pueden usar el mismo método pero hacer cosas diferentes.

Ejemplo con el método: __(Notificar)__
- Paciente: le llega la notificación de su turno a través de un mensaje ya sea a su correo a bien a su teléfono.
- Médico: le llega la notificación que tiene un turno agendado en el sistema.
- Secretaria: Le aparece en la agenda dicho turno.

En si todos comparten el mismo método pero tienen diferente funcionalidad.

### Abstracción

Va a mostrar solo lo más importante y oculta cómo funciona por dentro el sistema.

Ejemplo: El usuario no necesita saber cómo es el sistema por dentro:

- Controla horarios
- Evita superposición
- Guarda en base de datos
- Envía WhatsApp o mail

El usuario solo va a ver:

- Turnos disponibles
- Reservar turno
- Cancelar turno

En pocas palabras al usuario no se le permite ver el sistema por dentro, por la misma razón del resguardo de los diferentes pacientes.

---

## Requisitos iniciales del Sistema

### Funcionales

- El sistema debe permitir agendar turnos, evitando la superposición de horarios.

- El sistema debe permitir reprogramar turnos existentes.

- El sistema debe permitir cancelar turnos.

- El sistema del paciente debe tener los botones de "Reservar" y "Cancelar" y en cada paso le debe llegar notificación al mail y whatsapp.

- La secretaría debe tener las opciones de "Reservar" y "Reprogramar" para avisar en el sistema que ya el paciente se ha anunciado al turno, y esta debe aparecer en el sistema del médico.

### No Funcionales 

__Usabilidad__

- El sistema debe ser fácil de entender y ser práctico para la vista tanto del paciente como de la secretaria y el médico. 
- Debe ser una interfaz distinta tanto como para el paciente como para el médico y la secretaria.

__Información y seguridad__ 
 - El sistema debe contener datos de los pacientes, asi como su nombre, fecha de nacimiento, dni,  número de teléfono y mail, para poder enviar las notificaciones.
- El sistema debe detectar cuando es un usuario nuevo para que se reserve como "Primera consulta"
- La información del paciente debe estar segura.

---

## Casos de usos ##

__Precondiciones:__ El profesional de salud debe tener disponibilidad horaria para poder atender en el horario solicitado.

__Flujo básico:__

- 1.La secretaria selecciona al profesional y el día en la agenda.

- 2.El sistema muestra los bloques libres según el horario base y excepciones.

- 3.La secretaría ingresa los datos del paciente (nombre, teléfono, DNI, etc).

- 4.La secretaría elige el tipo de consulta: "Control" (15 min) o "Primera vez" (30 min).

- 5.El sistema válido que no haya otro turno en ese horario.

- 6.El sistema confirma y guarda el turno con estado "Activo".

__Flujos alternativos:__

__En caso de horarios ocupados:__

- 1.Se busca otra fecha accesible para el paciente y el profesional.
- 2.Un sobreturno autorizado por el profesional de la salud.
__poscondiciones:__turno queda registrado y el tiempo bloqueado en la agenda.

__Registrador Llegada del Paciente:__

__Actor principal:__ Secretaria (Victoria)

__Descripción:__ Permite marcar a un paciente si ha llegado o no básicamente al consultorio para que el médico sepa que puede llamarlo.

__Precondiciones:__    El paciente debe tener un turno agendado para el día y hora actual.

__Flujo Básico:__

- 1 El paciente se presenta en el consultorio.
- 2 La Secretaria busca el turno en la agenda del día y si los datos coinciden con el paciente.
- 3 La Secretaria marca al paciente como "Presente".

- 4 El sistema registra automáticamente la hora real de llegada.

__Flujo Alternativo:__
 __Llegada tarde:__

- Si el paciente llega después de los 10 minutos de tolerancia, el sistema registra la llegada pero muestra una alerta de retraso.(queda en decisión del profesional si atender o cancelar la consulta).

- Si el paciente no asiste se lo tacha como “Ausente”.

- Si el paciente cancela la consulta,se borra y se sigue con la consulta que le prosigue.

__Postcondiciones:__  El estado del turno cambia a "Presente" o ”Ausente” y es visible en la agenda del profesional.

### Autorizar Sobreturno

__Actor principal:__  Profesional (Dr. Molina)

__Descripción:__  Permitir la asignación de un turno en un horario que ya se encontraba ocupado y usarlo para atender a otro paciente, deben ser definidos manualmente por el médico, debe ser el único en tener acceso y toma de decisión.

__Precondiciones:__  El horario deseado ya debe estar reservado para otro paciente.
__Flujo Básico:__

- El profesional intenta agregar una consulta en un horario ocupado.

- El profesional accede a la agenda de horarios y emite un sobreturno en el sistema.

- Tras la aprobación, el sistema guarda el segundo turno en el mismo bloque de horario.

__Flujo Alternativo:__
-Límite Alcanzado: Si ya hay dos sobreturnos en el día, el sistema muestra una advertencia registrando la preferencia del médico de no exceder ese número.

__Postcondiciones:__ Coexisten dos turnos en el mismo horario en la agenda.

### Cancelar un turno

__Actor principal:__ Secretaria o paciente.

__Descripción:__ Permitir la cancelación de un turno o consulta médica por sucesos ocurridos que impidan al paciente o profesional asistir.

__Postcondiciones:__ Debe existir un turno previamente registrado en el sistema.

__Flujo Básico:__

- La Secretaria localiza el turno en la agenda y cancela el turno.
- El sistema solicita (opcionalmente) el motivo de la cancelación para que quede asentado si fue por pedido del paciente o decisión del médico.

- El sistema actualiza el estado del turno de "Activo" a "Cancelado".

- El sistema registra automáticamente quién canceló la consulta y cuándo sucedió el cambio.

- El sistema dispara una notificación obligatoria al paciente (preferentemente por WhatsApp) para confirmar la cancelación.

__Flujos Alternativos:__ Se solicita una reprogramación en caso de que cancele el profesional.
 __Postcondiciones:__ El horario queda libre en la agenda (en caso de que cancele el paciente) o se realiza una reprogramación (en caso de que cancele el profesional).

### Bloquear horarios

__Actor principal:__ Secretaria (Valeria).

__Descripción:__  Permite definir periodos de tiempo en los que el profesional no estará disponible para atender pacientes, asegurando que el sistema impida automáticamente la reserva de turnos en esos bloques.

__Precondiciones:__ Notificar días y horarios donde el profesional no se encuentra activo o trabajando.

__Flujo Básico:__ La secretaria accede a la configuración de disponibilidad del profesional. Selecciona el rango de fechas u horas a bloquear (ej. vacaciones, feriados o las clases del doctor los jueves).

__Flujos Alternativos:__ Cualquier paciente que necesite atención deberá programar su consulta a fechas donde se encuentre disponible el profesional.

__Postcondiciones:__ Los horarios bloqueados no aparecen como opción al intentar agendar nuevos turnos.

---

## Boceto Inicial del diseño de clases ##

 [[Boceto Inicial de clases](01-boceto_inicial_clases.png)]
