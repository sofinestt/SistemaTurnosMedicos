| **Nombre del escenario:** | Flujo principal - Cancelación de turno por secretaria exitoso | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Cancelar Turno | | **ID Única:** | CU-02 |
| **Área** | Sistema de Turnos Médicos - Gestión de Turnos | | | |
| **Actor(es):** | Secretaria | | | |
| **Descripción:** | Permite a la secretaria cancelar un turno existente, registrando el motivo de la cancelación y liberando el horario en la agenda. | | | |

| **Activar Evento:** | La secretaria accede al sistema y selecciona un turno para cancelarlo. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. La secretaria accede al sistema | Usuario autenticado |
| 2. El sistema muestra la agenda de turnos | Agenda del sistema |
| 3. La secretaria busca y selecciona el turno a cancelar | Interfaz de agenda |
| 4. La secretaria selecciona la opción "Cancelar turno" | Funcionalidad del sistema |
| 5. El sistema solicita el motivo de cancelación | Formulario de ingreso |
| 6. La secretaria ingresa el motivo de cancelación | Datos ingresados |
| 7. El sistema registra el motivo en el historial de cambios | Base de datos |
| 8. El sistema actualiza el estado del turno a "cancelado" | Registro de turnos |
| 9. El sistema libera el horario en la agenda | Agenda del profesional |
| 10. El sistema notifica la cancelación del turno | Sistema de notificaciones |
| 11. El sistema muestra confirmación de la operación | Mensaje en pantalla |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Existe al menos un turno previamente agendado. La secretaria tiene acceso válido al sistema. |
| **Postcondiciones:** | El turno queda cancelado, el horario se libera y se registra el motivo en el historial de cambios. |
| **Suposiciones:** | El sistema dispone de un mecanismo de registro de historial. El sistema cuenta con un módulo de notificaciones. |
| **Reunir requerimientos:** | Cancelación de Turnos  |
| **Aspectos sobresalientes:** | Registro obligatorio del motivo de cancelación. Trazabilidad de cambios en el historial. Notificación a las partes involucradas (paciente/profesional). |
| **Prioridad:** | Media  |
| **Riesgo:** | Medio  |