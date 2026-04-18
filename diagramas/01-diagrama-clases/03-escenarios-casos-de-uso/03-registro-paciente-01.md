| **Nombre del escenario:** | Flujo principal - Registro de paciente exitoso | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Registro de Paciente | | **ID Única:** | 01 |
| **Área** | Sistema de Turnos Médicos - Gestión de Pacientes | | | |
| **Actor(es):** | Secretaria, Médico | | | |
| **Descripción:** | Permite registrar un nuevo paciente en el sistema ingresando sus datos personales y de contacto. | | | |

| **Activar Evento:** | La secretaria o el médico selecciona la opción "Registrar paciente" en el sistema. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | *Información para los pasos* |
|---|---|
| 1. El actor accede al sistema | Usuario autenticado en el sistema |
| 2. Selecciona la opción "Registrar paciente" | Menú principal del sistema |
| 3. El sistema muestra el formulario de registro de paciente | Formulario de alta de paciente |
| 4. El actor ingresa los datos del paciente | Datos personales y de contacto |
| 5. El actor confirma la información ingresada | Botón "Confirmar" |
| 6. El sistema valida los datos obligatorios | Validaciones del formulario |
| 7. El sistema verifica que el paciente no exista previamente | Base de datos de pacientes |
| 8. El sistema guarda los datos del paciente | Persistencia en base de datos |
| 9. El sistema muestra confirmación de registro exitoso | Mensaje en pantalla |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El sistema está operativo. El actor tiene acceso válido al sistema. |
| **Poscondiciones:** | El paciente queda registrado correctamente en el sistema. |
| **Suposiciones:** | El paciente proporciona datos válidos y completos. Existe conexión al sistema y base de datos disponible. |
| **Reunir requerimentos:** | Registro de Pacientes  |
| **Aspectos sobresalientes:** | Validación de datos obligatorios. Control de duplicidad de pacientes. Posible integración con historial clínico. |
| **Prioridad:** | Alta |
| **Riesgo:** | Medio |