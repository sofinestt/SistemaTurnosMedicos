# Análisis del Diseño: Principio de Inversión de Dependencias (DIP)

---

## 1. Dependencias Directas Identificadas

Basándome en las tarjetas CRC, estas son las dependencias concretas que violan DIP:

| Clase | Depende Directamente de |
|-------|------------------------|
| **Agenda** | Turno, Medico, Secretaria, Disponibilidad |
| **Turno** | Paciente, Medico, Agenda, Disponibilidad, Notificacion |
| **Notificacion** | Turno, Paciente, Secretaria |
| **Disponibilidad** | Agenda, Turno, Medico |
| **Paciente** | Turno, Agenda, Notificacion |
| **Medico** | Agenda, Turno, Disponibilidad, Secretaria |
| **Secretaria** | Agenda, Turno, Paciente, Medico, Notificacion |

---

## 2. Abstracciones Propuestas

### Interfaces para Actores del Sistema

```python
# Interfaz para cualquier persona en el sistema
class IPersona:
    + getNombre(): string
    + getApellido(): string
    + getTelefono(): string
    + getMail(): string

# Paciente es un rol que puede solicitar turnos
class IPaciente(IPersona):
    + solicitarTurno(fecha, hora, tipoConsulta): Turno
    + cancelarTurno(turnoId)
    + getTurnos(): list[Turno]

# Médico es un rol que consulta agenda y autoriza sobreturnos
class IMedico(IPersona):
    + getEspecialidad(): string
    + getMatricula(): string
    + consultarAgenda(fecha): Agenda
    + autorizarSobreturno(turnoId)
    + definirDisponibilidad(disponibilidad)

# Secretaria es un rol que gestiona turnos
class ISecretaria(IPersona):
    + crearTurno(paciente, medico, fecha, hora): Turno
    + reprogramarTurno(turnoId, nuevaFecha, nuevaHora)
    + cancelarTurno(turnoId)
    + enviarNotificacion(paciente, mensaje)
```

### Interfaces para Comportamientos del Dominio

```python
# La agenda gestiona turnos y disponibilidad
class IAgenda:
    + agregarTurno(turno): bool
    + eliminarTurno(turnoId)
    + verificarConflicto(fecha, hora, medico): bool
    + getTurnosDelDia(fecha): list[Turno]
    + getTurnosDeSemana(semana): list[Turno]

# Disponibilidad define cuándo se puede atender
class IDisponibilidad:
    + estaDisponible(fecha, hora): bool
    + getBloquesHorarios(): list[BloqueHorario]
    + agregarExcepcion(excepcion)
    + estaBloqueado(fecha): bool

# Notificaciones registran y envían avisos
class INotificacion:
    + enviar(recordatorio): bool
    + registrarEnvio(notificable): bool
    + getNotificacionesPendientes(): list[Notificacion]
    + getHistorial(pacienteId): list[Notificacion]
```

---

## 3. Inyección de Dependencias: Dónde y Cómo

| Clase | Tipo de Inyección | Justificación |
|-------|-------------------|----------------|
| **Turno** | Constructor | Necesita referencias a Paciente y Medico desde su creación |
| **Agenda** | Constructor | Inyectar IDisponibilidad para verificar horarios sin acoplar a Disponibilidad concreta |
| **Notificacion** | Setter + Constructor | Permite cambiar el canal de envío (Email, SMS, WhatsApp) en tiempo de ejecución |
| **Secretaria** | Constructor | Recibir implementaciones de IAgenda e INotificacion para poder mockear en tests |

### Ejemplo de Constructor con DI

```python
class Turno:
    def __init__(self, paciente: IPaciente, medico: IMedico, 
                 fecha: date, hora: time, tipoConsulta: ITipoConsulta):
        self._paciente = paciente      # Interfaz IPaciente
        self._medico = medico          # Interfaz IMedico
        self._fecha = fecha
        self._hora = hora
        self._tipoConsulta = tipoConsulta  # Interfaz ITipoConsulta

class Agenda:
    def __init__(self, medico: IMedico, disponibilidad: IDisponibilidad):
        self._medico = medico
        self._disponibilidad = disponibilidad  # ✅ Invertida
        self._turnos = []
```

---

## 4. Versión Mejorada del Diseño

### Estructura de Abstracciones

```
IPersona (abstracta)
    │
    ├── IPaciente
    │   └── Paciente (implementa)
    ├── IMedico
    │   └── Medico (implementa)
    └── ISecretaria
        └── Secretaria (implementa)

ITipoConsulta (abstracta)
    ├── Control (+ calcularDuracion() → 15 min)
    └── PrimeraVez (+ calcularDuracion() → 30 min)

IAgenda (abstracta)
    └── Agenda (implementa)

IDisponibilidad (abstracta)
    └── Disponibilidad (implementa)

INotificacion (abstracta)
    ├── NotificacionEmail
    ├── NotificacionSMS
    └── NotificacionWhatsApp
```

### Pseudocódigo Mejorado

```python
# Clases concretas dependen de abstracciones, no de otras concretas

class Agenda:
    def __init__(self, medico: IMedico, disponibilidad: IDisponibilidad):
        self._medico = medico
        self._disponibilidad = disponibilidad  # ✅ Dependencia invertida
    
    def agregarTurno(self, turno: Turno):
        if self._disponibilidad.estaDisponible(turno.fecha, turno.hora):
            # lógica existente...
            pass

class Notificacion:
    def __init__(self, canal: ICanalNotificacion):  # ✅ Setter o Constructor
        self._canal = canal
    
    def enviar(self, mensaje, destinatario):
        self._canal.enviar(mensaje, destinatario)

# canal puede ser: EmailSender, SmsSender, WhatsAppSender
```

---

## 5. Decisiones Discutibles

| Decisión | Argumento a Favor | Argumento en Contra |
|----------|-------------------|---------------------|
| **Interfaz ITipoConsulta** | Ya existe en el diseño (Control/PrimeraVez) con comportamiento polimórfico | Puede ser overkill si solo hay 2 tipos y no se expandirán |
| **Separación INotificacion por canal** | Permite cambiar fácilmente de proveedor (SendGrid → Twilio) | Añade complejidad si solo se usa un canal |
| **Inyectar IDisponibilidad en Agenda** | Facilita testing con disponibilidad mockeada | Agenda ya tiene la responsabilidad de verificar conflictos |
| **Interfaces para actores (IPaciente, IMedico)** | Útil si hay diferentes tipos de pacientes (obra social, particular) | En MVP puede ser prematuro si solo existe un tipo |

---

## 6. Recomendación

Para el **MVP**, sugiero comenzar con las abstracciones de mayor impacto:

1. **ITipoConsulta** → Ya está identificada en la documentación
2. **IDisponibilidad en Agenda** → Facilita testing y evolución
3. **ICanalNotificacion** → Permite cambiar canal sin modificar lógica de negocio

Las interfaces de actores (IPaciente, IMedico) pueden postergarse hasta que se confirme que habrá variaciones de estos roles.

---

## 7. Resumen de Cambios Propuestos

| Cambio | Tipo | Motivación |
|--------|------|-------------|
| Crear `IPersona` como clase abstracta | Abstracción | Unifica datos comunes de Paciente, Medico, Secretaria |
| Crear `IPaciente`, `IMedico`, `ISecretaria` | Interfaz | Permite variaciones de roles sin acoplar |
| Crear `IAgenda` | Interfaz | Agenda puede ser implementada de diferentes formas |
| Crear `IDisponibilidad` | Interfaz | Facilita testing y evolución de reglas de disponibilidad |
| Crear `ICanalNotificacion` | Interfaz | Permite cambiar proveedor de notificaciones |
| Inyectar `IDisponibilidad` en `Agenda` | DI Constructor | Desacopla verificación de horarios |
| Inyectar `ICanalNotificacion` en `Notificacion` | DI Setter | Permite cambio dinámico de canal |