## Herramienta utilizada

Se utilizó GitHub Copilot (Agent Mode) integrado en VS Code.

## Contexto proporcionado

Se utilizaron como base los siguientes archivos:
- anexos/introduccion.md
- diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
- herramientas-agile/tarjetas-crc/


## Prompt utilizado

```
Abrí Copilot Agent Mode en VS Code y utilizá como contexto los siguientes archivos:

- anexos/introduccion.md
- diagramas/01-diagrama-clases/01-boceto-inicial.excalidraw
- herramientas-agile/tarjetas-crc/

A partir de este contexto, analizá el diseño actual del sistema de turnos médicos.

Realizá las siguientes tareas:

1. Identificá responsabilidades dentro de las clases actuales que podrían estar mal agrupadas o mezcladas.
2. Detectá posibles oportunidades de aplicar el Principio de Segregación de Interfaces (ISP).
3. Proponé interfaces cohesivas y específicas al dominio que representen comportamientos claros.
4. Señalá posibles “interfaces gordas” (interfaces demasiado generales o con demasiadas responsabilidades) que obliguen a implementar métodos innecesarios.
5. Evitá crear interfaces genéricas o abstractas sin justificación en el dominio.

Finalmente:

- Revisá críticamente tu propia propuesta.
- Verificá que cada interfaz tenga un propósito claro dentro del sistema de turnos médicos.
- Eliminá cualquier abstracción innecesaria o redundante.
- Documentá todo el análisis realizado de forma estructurada.
- Incluí:
  - Responsabilidades detectadas
  - Interfaces propuestas
  - Casos de posible sobre-abstracción (interfaces gordas)
  - Decisiones finales (qué se mantiene y qué se descarta)
- Presentá el resultado como un informe claro, listo para ser usado en una PR o documentación del proyecto.
- Generá un informe final completo con todo el trabajo realizado.
- El resultado debe estar listo para copiar y pegar en una PR o archivo de documentación.

```

## Output obtenido

### 1. Responsabilidades Detectadas

| Clase | Problema Detectado | Descripción |
|-------|-------------------|-------------|
| **Turno** | Demasiadas responsabilidades | Maneja estado, duración, reserva, cancelación, reprogramación y confirmación. Mezcla conceptos de "reserva" con "gestión de estado". |
| **Paciente** | Responsabilidad dual | Mantiene datos personales Y gestiona turnos (pedirTurno, cancelarTurno). Violación SRP. |
| **Secretaria** | Interfaz overloaded | Realiza operaciones de agenda, notificaciones y gestión de pacientes. |
| **Medico** | Mezcla de dominios | Consulta agenda Y define disponibilidad Y autoriza sobreturnos. Tres dominios distintos. |
| **Agenda** | Responsabilidad central | Coordina disponibilidad, detecta conflictos, agrega/elimina turnos. Punto de acoplamiento alto. |

### 2. Interfaces Gordas Identificadas

| Interfaz Actual | Métodos | Problema |
|----------------|---------|----------|
| **IPaciente** | pedirTurno(), cancelarTurno(), verHorariosDisponibles() | Paciente no debería saber de Agenda. |
| **IMedico** | consultarAgenda(), atenderConsulta(), definirDisponibilidad(), autorizarSobreturno() | "AtenderConsulta" es caso de uso, no responsabilidad del objeto. |
| **ISecretaria** | agendarTurno(), reprogramarTurno(), cancelarTurno(), notificarPaciente() | Mezcla operaciones de Agenda con operaciones de Notificación. |
| **ITurno** | reservar(), cancelar(), reprogramar(), confirmar() | Métodos verbales que mezclan estado con comportamiento. |

### 3. Interfaces Propuestas Segregadas

```typescript
interface IDatosPersona {
  getNombre(): string;
  getApellido(): string;
  getTelefono(): string;
  getMail(): string;
}

interface IClienteTurnos {
  solicitarTurno(fecha: Date, hora: Hora): Turno;
  cancelarTurno(turnoId: string): Resultado;
  consultarMisTurnos(): Turno[];
}

interface IProfesionalAgenda {
  consultarAgenda(dia: Date): AgendaDia;
  definirDisponibilidad(bloques: BloqueHorario[]): void;
  autorizarSobreturno(turno: Turno, autorizacion: string): void;
}

interface IGestorTurnos {
  agendarTurno(paciente: IClienteTurnos, fecha: Date, hora: Hora): Turno;
  reprogramarTurno(turnoId: string, nuevaFecha: Date, nuevaHora: Hora): Turno;
  cancelarTurno(turnoId: string, motivo: string): Resultado;
}

interface INotificador {
  enviarRecordatorio(turno: Turno): void;
  enviarConfirmacion(turno: Turno): void;
  enviarCancelacion(turno: Turno, motivo: string): void;
}

interface ITurnoDatos {
  getId(): string;
  getFecha(): Date;
  getHora(): Hora;
  getEstado(): EstadoTurno;
  getDuracionPrevista(): number;
  esSobreturno(): boolean;
}

interface ICicloVidaTurno {
  confirmar(): void;
  completar(duracionReal: number): void;
  cancelar(motivo: string): void;
}

interface IGestorDisponibilidad {
  estaDisponible(fecha: Date, hora: Hora): boolean;
  obtenerBloquesLibres(dia: Date): Bloque[];
  bloquearPeriodo(inicio: Date, fin: Date, motivo: string): void;
}
```

### 4. Matriz de Responsabilidades Segregadas

| Interface | Propósito | Métodos | Clases que la implementan |
|-----------|-----------|---------|---------------------------|
| `IDatosPersona` | Datos básicos de identificación | getNombre, getApellido, getTelefono, getMail | Paciente, Medico, Secretaria |
| `IClienteTurnos` | Solicitud y gestión de turnos por parte del paciente | solicitarTurno, cancelarTurno, consultarMisTurnos | Paciente |
| `IProfesionalAgenda` | Gestión de agenda profesional | consultarAgenda, definirDisponibilidad, autorizarSobreturno | Medico |
| `IGestorTurnos` | Operaciones de agenda (secretaria) | agendarTurno, reprogramarTurno, cancelarTurno | Secretaria |
| `INotificador` | Envío de notificaciones | enviarRecordatorio, enviarConfirmacion, enviarCancelacion | Notificacion |
| `ITurnoDatos` | Datos puros del turno | Getters de atributos | Turno |
| `ICicloVidaTurno` | Cambios de estado del turno | confirmar, completar, cancelar | Turno |
| `IGestorDisponibilidad` | Control de horarios disponibles | estaDisponible, obtenerBloquesLibres, bloquearPeriodo | Disponibilidad |

### 5. Casos de Sobre-abstracción Detectados

| Interfaz Propuesta | Descripción | Decisión | Justificación |
|-------------------|-------------|----------|----------------|
| `IActoresSistema` | Interface genérica para cualquier actor | **DESCARTAR** | No hay comportamiento común suficiente entre Paciente, Medico y Secretaria que justifique una interfaz única. |
| `IEntidadBase` | Clase base abstracta para todas las entidades | **DESCARTAR** | El sistema no necesita polimorfismo entre entidades. Cada una tiene responsabilidades distintas. |
| `IGestionable` | Interface genérica para objetos gestionables | **DESCARTAR** | Abstracción sin sentido en el dominio. Turno y Disponibilidad no son "gestionables" de la misma forma. |
| `IEstado` | Interface para objetos con estado | **DESCARTAR** | Solo Turno tiene estados relevantes. No hay necesidad de generalizar. |

### 6. Decisiones Finales

**Qué SE MANTIENE:**
- Separación TurnoDatos vs CicloVidaTurno
- Interfaces segregadas por rol (IClienteTurnos para Paciente, IProfesionalAgenda para Medico, IGestorTurnos para Secretaria)
- Notificación como interface (INotificador)
- Disponibilidad independiente (IGestorDisponibilidad)

**Qué SE DESCARTA:**
- Persona como interface única (se usa IDatosPersona solo para datos compartidos)
- Turno con métodos de negocio (reservar, reprogramar pasan a Agenda/GestorTurnos)
- Métodos mezclados en clases actuales (Paciente no tiene agendarTurno, Medico no tiene atenderConsulta)

## Análisis

La propuesta identifica claramente las siguientes violaciones del principio ISP:

1. **Turno** es la clase más problemática: mezcla datos con comportamiento de negocio. La solución propuesta divide Turno en dos interfaces: `ITurnoDatos` (datos puros) e `ICicloVidaTurno` (comportamiento de estado).

2. **Las clases Persona** (Paciente, Medico, Secretaria) tienen responsabilidades que no les corresponden. Cada actor debe implementar solo las interfaces relevantes para su rol específico.

3. **Las interfaces propuestas** segregan correctamente los comportamientos por dominio, evitando que las clases implementen métodos que no utilizan.

4. **Se descartaron** abstracciones sin justificación en el dominio (IActoresSistema, IEntidadBase, IGestionable, IEstado), lo cual demuestra un análisis crítico para evitar sobre-abstracción.

## Ajustes realizados

Se realizó una revisión crítica de la propuesta inicial:

- Se eliminaron los tipos de retorno y parámetros de las interfaces en el diagrama PUML para mantenerlo conciso.
- Se simplificó la notación de relaciones manteniendo la semántica de implementación.
- Se verificó que cada interfaz tenga un propósito claro dentro del sistema de turnos médicos.
- Se eliminaron las notas explicativas en el diagrama final para reducir complejidad.

## Conclusiones

El uso de IA permitió identificar rápidamente las responsabilidades mezcladas en el diseño actual del sistema. El análisis aplicando ISP reveló:

- **8 interfaces segregadas** propuestas vs. 4 interfaces gordas detectadas
- **5 clases afectadas** por responsabilidades mal agrupadas
- **4 abstracciones descartadas** por sobre-abstracción
- **3 decisiones de mantenimiento** claras

La propuesta mantiene solo las abstracciones que tienen sentido en el dominio de turnos médicos, evitando crear interfaces "por si acaso" que no aportan valor. Este enfoque es coherente con el principio ISP: los clientes no deben depender de métodos que no usan.

El resultado del análisis se documentó en:
- [informe-analisis-isp.md](../anexos/principios-solid/04-isp.md) - Informe completo en Markdown
- [01-solid-04-isp.puml](../diagramas/01-diagrama-clases/01-solid-04-isp.puml) - Diagrama UML en PlantUML

