# Informe de Análisis ISP - Sistema de Turnos Médicos

> **Fecha de análisis:** 25 de abril de 2026
> **Principio aplicado:** Interface Segregation Principle (ISP)
> **Contexto:** Análisis basado en tarjetas CRC, diagrama de clases y documentación del sistema

---

## 1. Responsabilidades Detectadas

### 1.1 Responsabilidades Mezcladas o Mal Agrupadas

| Clase | Problema Detectado | Descripción |
|-------|-------------------|-------------|
| **Turno** | Demasiadas responsabilidades | Maneja estado, duración, reserva, cancelación, reprogramación y confirmación. Mezcla conceptos de "reserva" con "gestión de estado". |
| **Paciente** | Responsabilidad dual | Mantiene datos personales Y gestiona turnos (pedirTurno, cancelarTurno). Violación SRP. |
| **Secretaria** | Interfaz overloaded | Realiza operaciones de agenda, notificaciones y gestión de pacientes. |
| **Medico** | Mezcla de dominios | Consulta agenda Y define disponibilidad Y autoriza sobreturnos. Tres dominios distintos. |
| **Agenda** | Responsabilidad central | Coordina disponibilidad, detecta conflictos, agrega/elimina turnos. Punto de acoplamiento alto. |

### 1.2 Responsabilidades Específicas por Clase

```
Turno (6 responsabilidades):
├── Controlar fecha/hora
├── Gestionar estado (reservado, cancelado, completado)
├── Calcular duración prevista
├── Calcular duración real
├── Manejar sobreturno
└── Registrar reserva/cancelación/reprogramación

→ PROBLEMA: "Reservar" y "Cancelar" son operaciones de negocio que 
   deberían estar en Agenda, no en Turno. Turno solo debería ser 
   un objeto pasivo con datos.
```

---

## 2. Oportunidades de Aplicar ISP

### 2.1 Interfaces Gordas Identificadas

| Interfaz Actual | Métodos | Problema |
|----------------|---------|----------|
| **IPaciente** | pedirTurno(), cancelarTurno(), verHorariosDisponibles() | Paciente no debería saber de Agenda. |
| **IMedico** | consultarAgenda(), atenderConsulta(), definirDisponibilidad(), autorizarSobreturno() | "AtenderConsulta" es caso de uso, no responsabilidad del objeto. |
| **ISecretaria** | agendarTurno(), reprogramarTurno(), cancelarTurno(), notificarPaciente() | Mezcla operaciones de Agenda con operaciones de Notificación. |
| **ITurno** | reservar(), cancelar(), reprogramar(), confirmar() | Métodos verbales que mezclan estado con comportamiento. |

### 2.2 Análisis de Acoplamiento

```
Interfaz "Gorda" Actual:
────────────────────────────
IPersona (propuesta teórica)
├── getNombre()
├── getApellido()
├── getTelefono()
├── getMail()
└── [todos los actores la implementan]

→ PROBLEMA: Si Persona es interface única, cada subclase 
   hereda métodos que no usa. Ej: Secretaria no necesita 
   "pedirTurno()" pero Paciente sí.
```

---

## 3. Interfaces Cohesivas Propuestas

### 3.1 Interfaces Segregadas por Dominio

```typescript
// ============================================================================
// INTERFAZ: Datos de Persona (solo datos, sin comportamiento)
// ============================================================================
interface IDatosPersona {
  getNombre(): string;
  getApellido(): string;
  getTelefono(): string;
  getMail(): string;
}

// ============================================================================
// INTERFAZ: Comportamiento de Paciente en el sistema de turnos
// ============================================================================
interface IClienteTurnos {
  solicitarTurno(fecha: Date, hora: Hora): Turno;
  cancelarTurno(turnoId: string): Resultado;
  consultarMisTurnos(): Turno[];
}

// ============================================================================
// INTERFAZ: Comportamiento de Médico relacionado con agenda
// ============================================================================
interface IProfesionalAgenda {
  consultarAgenda(dia: Date): AgendaDia;
  definirDisponibilidad(bloques: BloqueHorario[]): void;
  autorizarSobreturno(turno: Turno, autorizacion: string): void;
}

// ============================================================================
// INTERFAZ: Comportamiento de Secretaria en gestión de turnos
// ============================================================================
interface IGestorTurnos {
  agendarTurno(paciente: IClienteTurnos, fecha: Date, hora: Hora): Turno;
  reprogramarTurno(turnoId: string, nuevaFecha: Date, nuevaHora: Hora): Turno;
  cancelarTurno(turnoId: string, motivo: string): Resultado;
}

// ============================================================================
// INTERFAZ: Comportamiento de Notificaciones
// ============================================================================
interface INotificador {
  enviarRecordatorio(turno: Turno): void;
  enviarConfirmacion(turno: Turno): void;
  enviarCancelacion(turno: Turno, motivo: string): void;
}

// ============================================================================
// INTERFAZ: Datos puros de Turno (objeto valor)
// ============================================================================
interface ITurnoDatos {
  getId(): string;
  getFecha(): Date;
  getHora(): Hora;
  getEstado(): EstadoTurno;
  getDuracionPrevista(): number;
  esSobreturno(): boolean;
}

// ============================================================================
// INTERFAZ: Ciclo de vida del Turno
// ============================================================================
interface ICicloVidaTurno {
  confirmar(): void;
  completar(duracionReal: number): void;
  cancelar(motivo: string): void;
}

// ============================================================================
// INTERFAZ: Gestión de Disponibilidad
// ============================================================================
interface IGestorDisponibilidad {
  estaDisponible(fecha: Date, hora: Hora): boolean;
  obtenerBloquesLibres(dia: Date): Bloque[];
  bloquearPeriodo(inicio: Date, fin: Date, motivo: string): void;
}
```

### 3.2 Matriz de Responsabilidades Segregadas

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

---

## 4. Casos de Sobre-abstracción Detectados

### 4.1 Interfaces que NO se justifican en el dominio

| Interfaz Propuesta | Descripción | Decisión | Justificación |
|-------------------|-------------|----------|----------------|
| `IActoresSistema` | Interface genérica para cualquier actor | **DESCARTAR** | No hay comportamiento común suficiente entre Paciente, Medico y Secretaria que justifique una interfaz única. |
| `IEntidadBase` | Clase base abstracta para todas las entidades | **DESCARTAR** | El sistema no necesita polimorfismo entre entidades. Cada una tiene responsabilidades distintas. |
| `IGestionable` | Interface genérica para objetos gestionables | **DESCARTAR** | Abstracción sin sentido en el dominio. Turno y Disponibilidad no son "gestionables" de la misma forma. |
| `IEstado` | Interface para objetos con estado | **DESCARTAR** | Solo Turno tiene estados relevantes. No hay necesidad de generalizar. |

### 4.2 Abstracciones Válidas (que SÍ se justifican)

| Interfaz | Justificación |
|----------|----------------|
| `IDatosPersona` | Los tres actores (Paciente, Medico, Secretaria) comparten datos comunes de identificación. |
| `IClienteTurnos` | Paciente tiene comportamiento específico relacionado con turnos que no comparten otros actores. |
| `IProfesionalAgenda` | Solo el médico tiene la responsabilidad de definir disponibilidad y autorizar sobreturnos. |
| `IGestorTurnos` | La secretaria opera sobre la agenda de forma diferenciada al paciente. |

---

## 5. Decisiones Finales

### 5.1 Qué SE MANTIENE

| Decisión | Descripción |
|----------|-------------|
| **Separación TurnoDatos vs CicloVidaTurno** | Turno se divide en: datos puros (fecha, hora, estado) + comportamiento de ciclo de vida (confirmar, completar, cancelar). |
| **Interfaces segregadas por rol** | IClienteTurnos para Paciente, IProfesionalAgenda para Medico, IGestorTurnos para Secretaria. |
| **Notificación como interface** | INotificador se mantiene separada de IGestorTurnos. |
| **Disponibilidad independiente** | IGestorDisponibilidad se mantiene como interface propia. |

### 5.2 Qué SE DESCARTA

| Decisión | Descripción |
|----------|-------------|
| **Persona como interface única** | Descartamos la idea de una interface IPersona que hereden todos. En su lugar, usamos IDatosPersona solo para datos compartidos. |
| **Turno con métodos de negocio** | Descartamos reservar(), reprogramar() en la clase Turno. Pasan a ser operaciones de la Agenda/GestorTurnos. |
| **Métodos mezclados en clases actuales** | Paciente no debe tener "agendarTurno()" (es de Secretaria). Medico no debe tener "atenderConsulta()" (es un caso de uso). |

### 5.3 Estructura Final Propuesta

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    ARQUITECTURA ISP - SISTEMA DE TURNOS                  │
└─────────────────────────────────────────────────────────────────────────┘

PERSONAS (Datos compartidos)
├── IDatosPersona
│   ├── getNombre(): string
│   ├── getApellido(): string
│   ├── getTelefono(): string
│   └── getMail(): string
│
├── Paciente implements IDatosPersona, IClienteTurnos
│   └── Responsabilidades: datos + solicitar/cancelar turnos propios
│
├── Medico implements IDatosPersona, IProfesionalAgenda
│   └── Responsabilidades: datos + agenda + disponibilidad + sobreturnos
│
└── Secretaria implements IDatosPersona, IGestorTurnos
    └── Responsabilidades: datos + gestión completa de agenda

TURNOS
├── ITurnoDatos (datos puros)
│   ├── getId(), getFecha(), getHora(), getEstado(), etc.
│
├── ICicloVidaTurno (comportamiento de estado)
│   ├── confirmar(), completar(), cancelar()
│
└── Turno implements ITurnoDatos, ICicloVidaTurno

AGENDA Y DISPONIBILIDAD
├── IGestorTurnos (operaciones de agenda)
│   ├── agendarTurno(), reprogramarTurno(), cancelarTurno()
│
├── IGestorDisponibilidad (horarios)
│   ├── estaDisponible(), obtenerBloquesLibres(), bloquearPeriodo()
│
└── Agenda implements IGestorTurnos, IGestorDisponibilidad

NOTIFICACIONES
├── INotificador
│   ├── enviarRecordatorio(), enviarConfirmacion(), enviarCancelacion()
│
└── Notificacion implements INotificador
```

---

## 6. Resumen Ejecutivo

| Aspecto | Cantidad |
|---------|----------|
| Interfaces gordas detectadas | 4 |
| Interfaces propuestas segregadas | 8 |
| Interfaces descartadas (sobre-abstracción) | 4 |
| Responsabilidades mal agrupadas | 5 clases afectadas |
| Decisiones de mantenimiento | 4 |
| Decisiones de descarte | 3 |

### Conclusión

El diseño actual del sistema presenta varias violaciones del **Principio de Segregación de Interfaces (ISP)**:

1. **Turno** es la clase más problemática: mezcla datos con comportamiento de negocio.
2. **Las clases Persona** (Paciente, Medico, Secretaria) tienen responsabilidades que no les corresponden.
3. **Las interfaces propuestas** segregan correctamente los comportamientos por dominio.
4. **Se descartaron** abstracciones sin justificación en el dominio (IActoresSistema, IEntidadBase, IGestionable, IEstado).

La propuesta mantiene solo las abstracciones que tienen sentido en el dominio de turnos médicos, evitando crear interfaces "por si acaso" que no aportan valor.

---

> **Nota:** Este informe fue generado como parte del análisis de principios SOLID para el proyecto Sistema de Turnos Médicos.