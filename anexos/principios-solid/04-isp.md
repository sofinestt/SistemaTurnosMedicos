# Principio de Segregación de Interfaces (ISP)

---

## Propósito y Tipo del Principio SOLID

El **Principio de Segregación de Interfaces (ISP)** establece que los clientes no deben verse forzados a depender de interfaces que no utilizan. En otras palabras, es preferible tener muchas interfaces específicas antes que una interfaz general con demasiados métodos.

**Tipo:** Principio de diseño estructural

**Problema que resuelve:** Cuando una clase debe implementar métodos que no necesita, se genera acoplamiento innecesario y código muerto. ISP promueve interfaces cohesivas y especializadas, donde cada interfaz representa un comportamiento específico del dominio.

**Importancia de interfaces cohesivas y especializadas:**
- Cada interfaz tiene un propósito claro y limitado
- Las clases solo implementan lo que realmente necesitan
- Se reduce el acoplamiento entre componentes
- Facilita el mantenimiento y la evolución del sistema

---

## Motivación

### El problema en el sistema de turnos médicos

En el diseño inicial del sistema, las clases presentaban responsabilidades mezcladas que violaban el principio de responsabilidad única (SRP) y generaban interfaces "gordas":

| Clase | Problema |
|-------|----------|
| **Turno** | Mezclaba datos (fecha, hora, estado) con comportamiento de negocio (reservar, cancelar, reprogramar) |
| **Paciente** | Mantenía datos personales Y gestionaba turnos (pedirTurno, cancelarTurno) |
| **Medico** | Consultaba agenda Y definía disponibilidad Y autorizaba sobreturnos (tres dominios distintos) |
| **Secretaria** | Realizaba operaciones de agenda, notificaciones y gestión de pacientes |

### Cómo ISP ayuda a resolverlo

ISP propone segregar las interfaces grandes en otras más pequeñas y específicas:

```
ANTES (Interface "gorda"):
────────────────────────────
interface IPersona
├── getNombre()
├── getApellito()
├── getTelefono()
├── getMail()
├── pedirTurno()      ← No aplica a Secretaria
├── agendarTurno()    ← No aplica a Paciente
└── autorizarSobreturno()  ← No aplica a Paciente ni Secretaria

DESPUÉS (Interfaces segregadas):
─────────────────────────────────
interface IDatosPersona        → getNombre, getApellido, getTelefono, getMail
interface IClienteTurnos      → solicitarTurno, cancelarTurno, consultarMisTurnos
interface IProfesionalAgenda  → consultarAgenda, definirDisponibilidad, autorizarSobreturno
interface IGestorTurnos       → agendarTurno, reprogramarTurno, cancelarTurno
```

Cada clase ahora implementa solo las interfaces relevantes para su rol:

- **Paciente** → `IDatosPersona` + `IClienteTurnos`
- **Medico** → `IDatosPersona` + `IProfesionalAgenda`
- **Secretaria** → `IDatosPersona` + `IGestorTurnos`

---

## Explicación de Interfaces

### ¿Qué es una interfaz en diseño orientado a objetos?

Una **interfaz** es un contrato que define un conjunto de métodos que una clase se compromete a implementar. No contiene lógica de negocio, solo la firma de las operaciones disponibles.

### Aplicación de ISP en el sistema de turnos médicos

El sistema implementa las siguientes interfaces segregadas:

| Interfaz | Propósito | Métodos |
|---------|-----------|---------|
| `IDatosPersona` | Datos básicos de identificación | getNombre, getApellido, getTelefono, getMail |
| `IClienteTurnos` | Solicitud y gestión de turnos por parte del paciente | solicitarTurno, cancelarTurno, consultarMisTurnos |
| `IProfesionalAgenda` | Gestión de agenda profesional | consultarAgenda, definirDisponibilidad, autorizarSobreturno |
| `IGestorTurnos` | Operaciones de agenda (secretaria) | agendarTurno, reprogramarTurno, cancelarTurno |
| `INotificador` | Envío de notificaciones | enviarRecordatorio, enviarConfirmacion, enviarCancelacion |
| `ITurnoDatos` | Datos puros del turno | getId, getFecha, getHora, getEstado |
| `ICicloVidaTurno` | Cambios de estado del turno | confirmar, completar, cancelar |
| `IGestorDisponibilidad` | Control de horarios disponibles | estaDisponible, obtenerBloquesLibres |

### ¿Por qué estas interfaces están justificadas?

Cada interfaz representa un comportamiento real del dominio:

- `IClienteTurnos` → Solo el paciente solicita y cancela sus propios turnos
- `IProfesionalAgenda` → Solo el médico define disponibilidad y autoriza sobreturnos
- `IGestorTurnos` → Solo la secretaria opera sobre la agenda
- `ITurnoDatos` + `ICicloVidaTurno` → Turno separa datos puros de comportamiento de estado

---

## Estructura de Clases

### Diagrama UML de Interfaces Segregadas


![Diagrama UML de ISP](../../diagramas/01-diagrama-clases/01-solid-04-isp.puml)

---

## Justificación Técnica

### Análisis del diagrama UML

El diagrama muestra la aplicación del principio ISP en el sistema de turnos médicos:

#### 1. Interfaces Segregadas por Dominio

Las **8 interfaces** propuestas representan comportamientos específicos del dominio:

- **IDatosPersona**: Compartido por Paciente, Medico y Secretaria (datos de identificación)
- **IClienteTurnos**: Solo para Paciente (solicita/cancela turnos propios)
- **IProfesionalAgenda**: Solo para Medico (consulta agenda, define disponibilidad)
- **IGestorTurnos**: Solo para Secretaria (opera sobre la agenda)
- **INotificador**: Para Notificacion (envío de mensajes)
- **ITurnoDatos** + **ICicloVidaTurno**: Para Turno (separación datos/comportamiento)
- **IGestorDisponibilidad**: Para Disponibilidad (control de horarios)

#### 2. Implementación selectiva

Cada clase implementa **solo las interfaces que necesita**:

| Clase | Interfaces implementadas |
|-------|---------------------------|
| Paciente | IDatosPersona, IClienteTurnos |
| Medico | IDatosPersona, IProfesionalAgenda |
| Secretaria | IDatosPersona, IGestorTurnos |
| Turno | ITurnoDatos, ICicloVidaTurno |
| Agenda | IGestorTurnos, IGestorDisponibilidad |
| Disponibilidad | IGestorDisponibilidad |
| Notificacion | INotificador |

#### 3. Relaciones de composición

- **Agenda** *- Turno*: La agenda contiene turnos (relación de composición)
- **Turno** -- Paciente/Medico: El turno referencia a paciente y médico (asociación)

### Por qué la solución es correcta técnicamente

1. **Sin métodos innecesarios**: Ninguna clase implementa métodos que no utiliza. Por ejemplo, Secretaria no tiene "solicitarTurno()" porque esa responsabilidad es del paciente.

2. **Cohesión por rol**: Cada interfaz representa un rol claro en el sistema. IClienteTurnos representa al paciente como cliente del sistema de turnos.

3. **Bajo acoplamiento**: Las clases dependen solo de las interfaces que necesitan, no de implementaciones concretas.

4. **Facilidad de cambio**: Si se necesita modificar el comportamiento de "solicitarTurno", solo afecta a Paciente (implementador de IClienteTurnos), no a otras clases.

5. **Separación de responsabilidades en Turno**: La división de Turno en ITurnoDatos (datos puros) e ICicloVidaTurno (comportamiento de estado) evita que una clase maneje dos conceptos distintos.

---

## Conclusión

El principio ISP se aplica correctamente en el sistema de turnos médicos a través de:

- **8 interfaces segregadas** que representan comportamientos específicos del dominio
- **Implementación selectiva** donde cada clase solo implementa lo que necesita
- **Eliminación de interfaces "gordas"** que obligaban a implementar métodos innecesarios
- **Separación de datos y comportamiento** en la clase Turno

Esta diseño cumple con ISP: los clientes (clases) no dependen de interfaces que no utilizan, lo que resulta en un sistema más mantenible y extensible.