| **Nombre del escenario:** | Flujo principal - Autorización de sobreturno | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Autorizar Sobreturno | | **ID Única:** | 04 |
| **Área** | Sistema de Turnos Médicos - Gestión de Agenda | | | |
| **Actor(es):** | Médico | | | |
| **Descripción:** | Permite al médico autorizar la asignación de un sobreturno en su agenda para atender a un paciente fuera de la disponibilidad regular. | | | |

| **Activar Evento:** | El médico decide evaluar la asignación de un sobreturno ante la solicitud de un paciente. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. El médico accede al sistema | Usuario autenticado |
| 2. El sistema muestra la agenda del día | Agenda del profesional |
| 3. El médico identifica un horario para asignar el sobreturno | Interfaz de agenda |
| 4. El médico selecciona la opción de asignar sobreturno | Funcionalidad del sistema |
| 5. El sistema solicita los datos del paciente | Formulario de selección |
| 6. El médico selecciona o ingresa el paciente | Base de datos de pacientes |
| 7. El sistema solicita confirmación de la asignación | Mensaje de confirmación |
| 8. El médico confirma la operación | Acción del usuario |
| 9. El sistema registra el sobreturno en la agenda | Base de datos / agenda |
| 10. El sistema muestra confirmación de la operación | Mensaje en pantalla |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El médico tiene acceso al sistema. La agenda del día está disponible. El paciente está registrado o puede ser seleccionado. |
| **Postcondiciones:** | El sobreturno queda registrado y asociado a un paciente en la agenda del médico. |
| **Suposiciones:** | El sistema permite la asignación de turnos fuera de la disponibilidad estándar. El médico tiene autorización para gestionar sobreturnos. |
| **Reunir requerimientos:** | Gestión de Sobreturnos  |
| **Aspectos sobresalientes:** | Control de cantidad máxima de sobreturnos. Impacto en la duración de la jornada. Validación de conflictos en agenda. |
| **Prioridad:** | Media |
| **Riesgo:** | Medio |