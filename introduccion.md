# Requisitos iniciales del sistema

## **Requerimientos del usuario**

## RF1 – Gestión de turnos médicos

#### El sistema debe permitir gestionar los turnos del consultorio.

### Funcionalidades

- El sistema debe permitir agendar turnos, evitando superposición de horarios.

- El sistema debe permitir reprogramar turnos existentes.

- El sistema debe permitir cancelar turnos.

 -El sistema del paciente debe tener los botones de "Reservar" y "Cancelar" y en cada paso le debe llegar notificación al mail y whatsapp.

-La secretaria debe tener las opciones de "Reservar" y "Reprogramar" para avisar en el sistema que ya el paciente se ha anunciado al turno, y esta debe aparecer en el sistema del doctor.

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

   - Registrar la hora real de llegada (esto lo marca la secretaria una vez llegado el paciente y haberse anunciadp)

 #Requisitos no funcionales
  ## RNF1 -  Usabilidad
   - El sistema debe ser fácil de entender y ser práctico para la vista tanto del paciente como de la secretaria y el médico 
   - Debe ser una interfaz distinta tanto como para el paciente como para el médico y la secretaria


   ## RNF2 - Información y seguridad
   - El sistema debe contener datos de los pacientes, asi como su nombre, edad, número de teléfono y mail, para poder enviar las notificaciones
   - El sistema debe detectar cuando es un usuario nuevo para que se reserve como "Primera consulta"
   - La información del paciente debe estar segura
