| **Nombre del escenario:** | Flujo principal - Agendamiento de turno | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Agendar Turno | | **ID Única:** | 05 |
| **Área** | Sistema de Turnos Médicos - Gestión de Agenda | | | |
| **Actor(es):** | Secretaria, Médico | | | |
| **Descripción:** | Permite registrar un turno en la agenda de un profesional, asignando un paciente a un horario disponible. | | | |

| **Activar Evento:** | La secretaria o el médico selecciona la opción "Agendar turno" en el sistema. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | *Información para los pasos* |
|---|---|
| 1. El actor accede al sistema y consulta la agenda del profesional | Usuario autenticado |
| 2. El sistema muestra la disponibilidad del profesional | Agenda del profesional |
| 3. El actor selecciona un horario libre | Interfaz de agenda |
| 4. El actor define el tipo de consulta | Tipo de consulta (duración estimada) |
| 5. El sistema valida disponibilidad y ausencia de conflictos | Reglas de agenda |
| 6. El actor ingresa o selecciona los datos del paciente | Base de datos de pacientes |
| 7. El sistema asocia el paciente al turno | Registro de turno |
| 8. El sistema guarda el turno y bloquea el horario | Base de datos / agenda |
| 9. El sistema muestra confirmación de turno agendado | Mensaje en pantalla |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El profesional tiene disponibilidad cargada. El actor tiene acceso al sistema. El paciente está registrado o puede ser seleccionado. |
| **Poscondiciones:** | El turno queda registrado en estado activo y el horario aparece como ocupado en la agenda. |
| **Suposiciones:** | El sistema calcula correctamente la duración según el tipo de consulta. La agenda está actualizada en tiempo real. |
| **Reunir requerimentos:** | Agendamiento de Turnos (05) |
| **Aspectos sobresalientes:** | Validación de conflictos de agenda. Manejo de duración de turnos según tipo de consulta. Posible concurrencia (dos usuarios intentando reservar el mismo horario). |
| **Prioridad:** | Alta |
| **Riesgo:** | Alto  |