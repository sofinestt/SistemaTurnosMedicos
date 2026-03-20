# Requisitos iniciales del sistema

## **Requerimientos del usuario**

## RF1 – Gestión de turnos médicos

#### El sistema debe permitir gestionar los turnos del consultorio.

### Funcionalidades

- El sistema debe permitir agendar turnos, evitando superposición de horarios.

- El sistema debe permitir reprogramar turnos existentes.

- El sistema debe permitir cancelar turnos.

### Tipos de turnos

El sistema debe diferenciar entre:

- Control (15 minutos)

- Primera consulta (30 minutos)

### Gestión de horarios

- El sistema debe respetar el horario habitual:

    - Lunes a viernes de 9:00 a 13:00 y de 15:00 a 19:00.

- El sistema debe permitir:

   - Bloquear horarios por vacaciones o feriados.

  - Bloquear horarios por actividades fijas.

### Reglas de negocio

No se deben programar:

- Primeras consultas los viernes a última hora.

- Procedimientos los lunes.

- Los sobreturnos:

    - No deben generarse automáticamente.

     - Deben ser definidos manualmente.

   - Máximo sugerido: 2 por día.

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

   - Registrar la hora real de llegada