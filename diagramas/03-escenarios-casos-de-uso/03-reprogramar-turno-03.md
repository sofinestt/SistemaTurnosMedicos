| **Nombre del escenario:** | Flujo principal - Reprogramación de turno  | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Reprogramar Turno | | **ID Única:** | 03 |
| **Área** | Sistema de Turnos Médicos - Gestión de Turnos | | | |
| **Actor(es):** | Secretaria | | | |
| **Descripción:** | Permite a la secretaria modificar la fecha y/o el horario de un turno previamente agendado, asegurando disponibilidad y registrando el cambio. | | | |

| **Activar Evento:** | La secretaria selecciona un turno existente y solicita su reprogramación. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. La secretaria accede al sistema | Usuario autenticado |
| 2. El sistema muestra la agenda de turnos | Agenda del sistema |
| 3. La secretaria selecciona el turno a reprogramar | Interfaz de agenda |
| 4. La secretaria ingresa la nueva fecha y horario | Datos de reprogramación |
| 5. El sistema valida la disponibilidad del nuevo horario | Reglas de agenda |
| 6. El sistema solicita confirmación del cambio | Mensaje de confirmación |
| 7. La secretaria confirma la reprogramación | Acción del usuario |
| 8. El sistema libera el horario original | Agenda del profesional |
| 9. El sistema asigna el nuevo horario al turno | Registro de turnos |
| 10. El sistema actualiza la información del turno | Base de datos |
| 11. El sistema registra el cambio en el historial | Historial de cambios |
| 12. El sistema notifica la reprogramación | Sistema de notificaciones |
| 13. El sistema muestra confirmación de la operación | Mensaje en pantalla |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Existe un turno previamente agendado. La secretaria tiene acceso válido al sistema. |
| **Postcondiciones:** | El turno queda actualizado con la nueva fecha y horario, el horario anterior se libera y el cambio queda registrado en el historial. |
| **Suposiciones:** | El sistema valida correctamente la disponibilidad de horarios. Existe un mecanismo de historial y notificaciones. |
| **Reunir requerimientos:** | Reprogramación de Turnos  |
| **Aspectos sobresalientes:** | Validación de disponibilidad del nuevo horario. Liberación automática del turno anterior. Trazabilidad del cambio. Notificación a las partes involucradas. |
| **Prioridad:** | Alta  |
| **Riesgo:** | Medio  |