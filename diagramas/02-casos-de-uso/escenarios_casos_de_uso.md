| **Nombre del escenario:** |Flujo principal - Paciente registra un turno médico | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Agendar turno | | **ID Única:** | 1 |
| **Área** | Sistema de Turnos Medicos | | | |
| **Actor(es):** | Paciente | | | |
| **Descripción:** | Permite al paciente reservar un turno médico, según la disponibilidad de la Agenda| | | |

| **Activar Evento:** | El paciente abre la aplicación del Sistema de Turnos Medicos, donde ingresa con sus datos y selecciona el turno  | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |


| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. El paciente abre la aplicación | Sistema de Turnos médicos |
| 2. El paciente ingresa sus credenciales | Usuario y clave del paciente |
| 3. El sistema muestra el calendario con turnos disponibles | Calendario web |
| 4. El paciente selecciona fecha y horario deseado | Calendario web |
| 5. El sistema valida los datos del paciente | Datos del paciente |
| 6. El sistema verifica la disponibilidad del turno | Agenda de turnos |
| 7. El sistema registra el turno y envía una notificación al paciente | Sistema de notificaciones |
| 8. El sistema muestra la confirmación del turno asignado | Página de confirmación |
| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El paciente tiene usuario y clave válidos para acceder al sistema|
| **Poscondiciones:** | El turno queda registrado, la agenda se actualiza y el paciente recibe una notificación|
| **Suposiciones:** | EL paciente tiene sus datos registrados. El sistema se encuentra disponible y accesible desde un navegador |
| **Reunir requerimentos:** | Paciente tiene sus datos personales guardados y el Sistema cuenta con turnos disponibles |
| **Aspectos sobresalientes:** | ¿Se debe evitar que no se superpongan los turnos?
¿se debe controlar el numero de intentos de registro?¿El usuario puede cambiar la contraseña?¿El paciente tiene usuario y contraseña en el sistema? |
| **Prioridad:** | Alta |
| **Riesgo:** | Tiempo - Costo (Medio) |


| **Nombre del escenario:** |Flujo principal - Paciente cancela un turno médico | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Cancelar turno | | **ID Única:** | 2 |
| **Área** | Sistema de Turnos Medicos | | | |
| **Actor(es):** | Paciente | | | |
| **Descripción:** | Permite al paciente cancelar un turno médico| | | |

| **Activar Evento:** | El paciente accede al sistema, visualiza sus turnos y selecciona uno para cancelarlo.

| **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. El paciente abre la aplicación | Sistema de Turnos médicos |
| 2. El paciente ingresa sus credenciales | Usuario y clave del paciente |
| 3. El sistema muestra sus turnos próximos | Sistema web |
| 4. El paciente selecciona el turno asignado que desea cancelar| Sistema web |
| 5. El sistema valida los datos del paciente | Datos del paciente |
| 6. El sistema solicita la confirmación de cancelación | Agenda de turnos |
| 7. El paciente confirma la accion | Agenda de turnos |
| 8. El sistema muestra la confirmación del turno cancelado | Página de confirmación |
| 9. El sistema envia una notificacion al paciente |  Sistema de notificaciones |
| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El paciente tiene usuario y clave válidos para acceder al sistema|
| **Poscondiciones:** | El turno queda cancelado, la agenda se actualiza y el paciente recibe una notificación|
| **Suposiciones:** | El paciente tiene sus datos registrados. El sistema se encuentra disponible y accesible desde un navegador |
| **Reunir requerimentos:** | Paciente tiene sus datos personales guardados y el Sistema cuenta con un registro de turnos reservados del paciente |
| **Aspectos sobresalientes:** | ¿Se debe limitar el tiempo previo para cancelar un turno?¿Se debe permitir cancelar turnos ya vencidos? ¿Se debe notificar al médico ante la cancelación? ¿Se deben aplicar penalizaciones por cancelaciones frecuentes? |
| **Prioridad:** | Tiempo Media  |
| **Riesgo:** | Tiempo - Costo (Bajo) |


| **Nombre del escenario:** |Flujo principal - Secretaria necesita reprogramar un turno | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Reprogramar turno | | **ID Única:** | 3 |
| **Área** | Sistema de Turnos Medicos | | | |
| **Actor(es):** | Secretaria | | | |
| **Descripción:** | La secretaria reprograma un turno debido a un cambio en la disponibilidad o solicitud del paciente| | | |

| **Activar Evento:** | La secretaria reprograma un turno médico viendo la disponibilidad en el sistema | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. La secretaria abre el sistema de turnos médicos | Sistema de turnos |
| 2. La secretaria ingresa con sus credenciales | Usuario y contraseña del operador del sistema |
| 3. El sistema despliega el calendario con los turnos asignados a cada paciente | Sistema web |
| 4. La secretaria selecciona el turno a reprogramar | Sistema web de turnos |
| 5. El sistema muestra la disponibilidad de otras fechas | Calendario web de turnos |
| 6. La secretaria consulta al paciente sobre su disponibilidad | Paciente |
| 7. El sistema valida que el nuevo horario esté libre| Validación de disponbilidad|
| 8.  El sistema valida correctamente la reprogramación del turno | Página de confirmación |
| 9. El sistema actualiza el nuevo turno y horario | Sistema web de turnos |
| 10.  El sistema le envía al paciente una notificación de su nuevo turno | Sistema de notificaciones|

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | La secretaria tiene un usuario y clave válidos. El paciente ya tiene un turno asignado |
| **Poscondiciones:** | El turno queda reprogramado exitosamente y el paciente es notificado|
| **Suposiciones:** | EL paciente esta registrado y tiene un turno asignado. Hay un navegador para ingresar al sitio. Hay horarios disponibles para reprogramar |
| **Reunir requerimentos:** | Reprogramación de turno |
| **Aspectos sobresalientes:** | ¿Hay disponibilidad en el sistema? ¿El paciente accede a la reprogramacion del turno?  |
| **Prioridad:** | Tiempo Alta |
| **Riesgo:** | Tiempo - Costo Medio |




| **Nombre del escenario:** |Flujo principal - Secretaria define la disponibilidad del médico | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Gestionar disponibilidad del médico | | **ID Única:** | 4 |
| **Área** | Sistema de Turnos Medicos | | | |
| **Actor(es):** | Secretaria | | | |
| **Descripción:** | Secretaria define la disponibilidad del médico en el calendario de turnos| | | |

| **Activar Evento:** | La secretaria necesita gestionar la disponibilidad del médico| **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. La secretaria abre el sistema de turnos médicos | Sistema de turnos |
| 2. La secretaria ingresa con sus credenciales | Usuario y contraseña del operador del sistema |
| 3. El sistema despliega el calendario | Calendario web de turnos |
| 4. La secretaria selecciona fechas y define horarios disponibles o no disponibles| Calendario web de turnos |
| 5. La secretaria confirma el bloqueo de fechas y horarios seleccionados | Calendario web de turnos |
| 6. El sistemar registra los cambios en la disponibilidad| Página de confirmación |
| 7. El sistema valida y actualiza la disponibilidad del médico| Página de confirmación |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | La secretaria tiene un usuario y clave válidos. Existe un calendario modificable. Existe un medico registrado |
| **Poscondiciones:** | La disponibilidad del médico queda actualizada según los cambios realizados|
| **Suposiciones:** | El calendario muestra todas lo dias y horarios del mes |
| **Reunir requerimentos:** | Disponibilidad |
| **Aspectos sobresalientes:** | ¿Se valida correctamente la nueva disponibilidad? ¿Los cambios en la disponibilidad generan conflictos con turnos existentes?|
| **Prioridad:** | Tiempo Alta |
| **Riesgo:** | Tiempo - Costo Medio |


| **Nombre del escenario:** |Flujo principal - Médico autoriza un sobreturno | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Gestionar sobreturno | | **ID Única:** | 5 |
| **Área** | Sistema de Turnos Medicos | | | |
| **Actor(es):** | Medico | | | |
| **Descripción:** | El médico evalúa y autoriza la asignación de un sobreturno en su agenda| | | |

| **Activar Evento:** | El médico evalúa la posibilidad de otorgar un sobreturno ante la solicitud de un paciente| **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. El médico ingresa al sistema de turnos médicos | Sistema de turnos |
| 2. El médico ingresa con sus credenciales | Usuario y contraseña del operador del sistema |
| 3. El sistema muestra los turnos del dia | Calendario web de turnos |
| 4. El médico selecciona un horario ocupado para asignar un sobreturno| Calendario web de turnos |
| 5. El sistema solicita confirmación para registrar el sobreturno | Calendario web de turnos |
| 6. El médico confirma la asignación del sobreturno| Confirmación del médico|
| 7. El sistema registra el sobreturno en la agenda del médico| Página de confirmación |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El médico posee credenciales en el sistema. Están todos los turnos de ese día ocupados |
| **Poscondiciones:** | Se registra un sobreturno en la agenda y el paciente queda asignado|
| **Suposiciones:** | El apciente está registrado en el sistema. El paciente solicita atención ese mismo día. El médico tiene la capacidad de aceptar sobreturnos. El sistema permite registrar turnos fuera de la disponibilidad normal |
| **Reunir requerimentos:** | Permitir la asignación de turnos fuera de la disponibilidad normal |
| **Aspectos sobresalientes:** | ¿El paciente acepta el sobreturno? ¿La agenda esta llena? ¿El médico decide autorizar el sobreturno?
| **Prioridad:** | Tiempo Medio |
| **Riesgo:** | Tiempo - Costo Medio |