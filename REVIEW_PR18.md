# CODE REVIEW - PULL REQUEST #18
## Sistema de Turnos Médicos - Diseño Orientado a Objetos

**Revisado por:** Superior Engineer  
**Fecha:** 2026-04-09  
**Decisión:** REQUEST CHANGES

---

## HALLAZGOS

======================
### Hallazgo #1

**Tipo de problema:** Clase faltante identificada en documentación pero no modelada  
**Severidad:** ALTA  
**Archivo:** boceto_inicial_clases.puml

**Problema:**
La clase `TipoConsulta` aparece descrita como abstracta en la documentación (sección Polimorfismo) pero NO está presente en el diagrama PUML.

**Sugerencia de mejora:** 
Agregar la jerarquía de polimorfismo:
```puml
abstract class TipoConsulta {
  nombre
  duracionEstimada
  calcularDuracion()
}

class Control extends TipoConsulta {
  calcularDuracion()
}

class PrimeraVez extends TipoConsulta {
  calcularDuracion()
}

Turno --> TipoConsulta
```

**Justificación:** La documentación describe explícitamente esta jerarquía como fundamental para la extensibilidad del sistema. Su ausencia genera inconsistencia entre especificación y diseño.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

======================
### Hallazgo #2

**Tipo de problema:** Clase faltante - Requisito funcional RF7 no modelado  
**Severidad:** ALTA  
**Archivo:** boceto_inicial_clases.puml

**Problema:**
RF7 establece: "El sistema debe mantener un historial completo de todas las modificaciones realizadas sobre los turnos". No existe clase que modele esto.

**Sugerencia de mejora:**
```puml
class RegistroHistorico {
  idCambio
  turno
  usuarioResponsable
  tipoModificacion
  fechaHora
  detalleAnterior
  detalleNuevo
  registrarCambio()
}

Turno --> RegistroHistorico
Secretaria --> RegistroHistorico
Medico --> RegistroHistorico
```

**Justificación:** Sin esta clase no hay forma de implementar auditoría de cambios, que es un requisito crítico para resolver disputas.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

======================
### Hallazgo #3

**Tipo de problema:** Clase faltante - Requisito funcional RF5 no modelado  
**Severidad:** ALTA  
**Archivo:** boceto_inicial_clases.puml

**Problema:**
RF5 requiere: "El sistema debe enviar notificaciones automáticas al paciente... El canal preferido es WhatsApp; el mail es alternativa". No hay clase modelo.

**Sugerencia de mejora:**
```puml
class Notificacion {
  idNotificacion
  paciente
  tipoNotificacion
  canal
  contenido
  fechaEnvio
  estado
  enviar()
  registrar()
}

enum CanalNotificacion {
  WHATSAPP
  EMAIL
}

Secretaria --> Notificacion
Paciente --> Notificacion
```

**Justificación:** RF5 es requisito explícito. Su ausencia en diagrama genera vacío en especificación técnica.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

======================
### Hallazgo #4

**Tipo de problema:** Clase faltante - Requisito funcional RF6 no modelado  
**Severidad:** MEDIA  
**Archivo:** boceto_inicial_clases.puml

**Problema:**
RF6: "El sistema debe permitir registrar la llegada física del paciente al consultorio". Sin clase modelo.

**Sugerencia de mejora:**
```puml
class RegistroLlegada {
  idRegistro
  turno
  horaReal
  estado
  minutosTarde
  registrar()
  calcularTolerancia()
}

Secretaria --> RegistroLlegada
Turno --> RegistroLlegada
```

**Justificación:** RF6 requiere representación en el modelo de datos.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

======================
### Hallazgo #5

**Tipo de problema:** Jerarquía de herencia incompleta  
**Severidad:** MEDIA  
**Archivo:** boceto_inicial_clases.puml

**Problema:**
Documentación propone clase base `Persona` (introduccion.md, sección Herencia) pero NO aparece en PUML. Paciente, Medico y Secretaria deberían heredar de ella.

**Sugerencia de mejora:**
```puml
abstract class Persona {
  nombre
  apellido
  telefono
  mail
}

class Paciente extends Persona {
  dni
  fechaNacimiento
  pedirTurno()
  cancelarTurno()
  verHorariosDisponibles()
}

class Medico extends Persona {
  especialidad
  matricula
  consultarAgenda()
  autorizarSobreturno()
}

class Secretaria extends Persona {
  agendarTurno()
  reprogramarTurno()
  cancelarTurno()
}
```

**Justificación:** Contradice propia documentación sobre fundamentos de POO.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

======================
### Hallazgo #6

**Tipo de problema:** Violación del principio SRP - Responsabilidades confusas  
**Severidad:** MEDIA  
**Archivo:** boceto_inicial_clases.puml

**Problema:**
Tanto `Secretaria` como `Agenda` tienen métodos para crear turnos:
- `Secretaria.agendarTurno()`
- `Agenda.agregarTurno()`

No queda claro cuál es el flujo real ni quién es responsable de qué.

**Sugerencia de mejora:**
Patrón cliente-servidor claramente definido:

```puml
class Secretaria {
  agenda: Agenda
  agendarTurno(paciente, fecha, hora, tipoConsulta) {
    # Secretaria es la interfaz de usuario
    # Delega responsabilidad a Agenda
    return this.agenda.agregarTurno(...)
  }
}

class Agenda {
  turnos: List<Turno>
  agregarTurno(paciente, fecha, hora, tipoConsulta) {
    # Agenda encapsula:
    # - La lista de turnos (privada)
    # - La lógica de validación
    # - La persistencia
    if (verificarConflicto(fecha, hora)) 
      throw ConflictoException
    return new Turno(...)
  }
}
```

**Justificación:** Encapsulamiento debe ser claro. Agenda encapsula datos y lógica; Secretaria es cliente.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

======================
### Hallazgo #7

**Tipo de problema:** Especificación ambigua en RF4  
**Severidad:** MEDIA  
**Archivo:** anexos/introduccion.md (línea 212-215)

**Problema:**
RF4 dice: "Los sábados son opcionales y se definen mes a mes"

No especifica:
- ¿Por mes-año?
- ¿Aplica a TODOS los sábados del mes o se configura cada sábado individualmente?
- ¿Se heredan configuraciones de otros meses?
- ¿Cómo se almacena esta información?

**Especificación mejorada:**
```
RF4 (revisado): 
"El sistema debe permitir habilitar/deshabilitar la disponibilidad 
de sábados para cada mes del año. Para cada sábado habilitado, 
se aplica el horario estándar (9-13, 15-19) a menos que se 
especifique excepción. La configuración se realiza mes por mes 
y no se hereda automáticamente."
```

**Modelo propuesto:**
```puml
class ConfiguracionMensual {
  mes
  año
  sabadosHabilitados: boolean
  definirSabadosDisponibles()
  obtenerSabadosDisponibles()
}

Medico --> ConfiguracionMensual
```

**Justificación:** Especificación vaga genera ambigüedad en implementación.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

======================
### Hallazgo #8

**Tipo de problema:** Falta caso de uso crítico  
**Severidad:** MEDIA  
**Archivo:** anexos/introduccion.md

**Problema:**
RF7 (Historial de cambios) NO tiene caso de uso asociado. Solo existen CU1-CU5.

**Sugerencia de mejora:**
Agregar CU6:

```
CU6 — Consultar Historial de Cambios

Actor principal: Secretaria, Medico (administrador)
Actores secundarios: Sistema de auditoría

Descripción: 
Consultar el historial completo de modificaciones de un turno 
específico para resolver disputas o realizar auditorías.

Precondiciones:
- El turno existe en el sistema
- El usuario tiene rol de Secretaria o Medico

Flujo principal:
1. El usuario busca el turno en la agenda
2. El usuario selecciona "Ver historial"
3. El sistema muestra lista de cambios ordenada por fecha
4. Para cada entrada: muestra quién cambió, qué cambió, cuándo
5. El usuario puede filtrar por tipo de cambio

Postcondiciones:
- El usuario visualizó completo el historial del turno
- No se realizó modificación alguna
- Se registró la consulta en logs de auditoría
```

**Justificación:** RF7 es requisito pero sin caso de uso definido.

==========================
**DECISION DE REVISION HUMANO**

[ ] Aceptar sugerencia
[ ] Rechazar sugerencia

Justificación:

---

## EVALUACIÓN GENERAL DE LA PR

### Calificación: ⚠️ REQUIERE CAMBIOS CRÍTICOS

### Análisis de riesgos técnicos:

| Riesgo | Impacto | Evidencia |
|--------|---------|-----------|
| Clases no modeladas pero requeridas | CRÍTICO | RF5, RF6, RF7 sin representación en diagrama |
| Inconsistencia teoría vs. práctica | ALTO | Doc describe clases que no aparecen en PUML |
| Responsabilidades difusas | ALTO | Duplicación de métodos entre clases |
| Especificaciones ambiguas | MEDIO | RF4 poco clara |
| Casos de uso incompletos | MEDIO | RF7 sin CU asociado |

### Fortalezas ✅

- Documentación clara de requisitos funcionales y no funcionales
- Análisis detallado de casos de uso principales (CU1-CU5)
- Conceptos de POO bien explicados con ejemplos del dominio
- Precondiciones y postcondiciones bien definidas
- Enfoque centrado en el problema (consultorio médico)

### Debilidades ❌

- **Brecha documentación vs. diagrama:** Teoría propone estructuras que no aparecen en PUML
- **Falta de modelado crítico:** 3 funcionalidades esenciales no representadas
- **Jerarquía incompleta:** Persona propuesta pero no implementada
- **Responsabilidades poco claras:** SRP violado en algunas clases
- **Ambigüedades en especificación:** RF4 necesita clarificación

---

## DECISION GENERAL EVALUADA POR IA:

### 🛑 REQUEST CHANGES

**Razones principales:**

1. **Incompletitud crítica:** El diagrama de clases no modela funcionalidades que son requisitos OBLIGATORIOS (RF5, RF6, RF7)

2. **Inconsistencia doctrina:** La documentación describe jerarquía Persona que NO aparece en PUML. Contradice propio texto sobre herencia.

3. **Ambigüedades en especificación:** RF4 necesita reescritura para clarificar implementación.

4. **Vacío en casos de uso:** Falta CU6 para que RF7 sea implementable.

5. **Violaciones de principios:** SRP incumplido en responsabilidades duplicadas.

### Acciones requeridas antes de aprobación:

- [ ] Agregar clase `TipoConsulta` con subclases `Control` y `PrimeraVez` (polimorfismo)
- [ ] Agregar clase `RegistroHistorico` para implementar RF7
- [ ] Agregar clase `Notificacion` para implementar RF5
- [ ] Agregar clase `RegistroLlegada` para implementar RF6
- [ ] Implementar jerarquía `Persona` como base de `Paciente`, `Medico`, `Secretaria`
- [ ] Aclarar y separar responsabilidades entre `Secretaria` y `Agenda`
- [ ] Reescribir RF4 con especificación clara sobre configuración de sábados
- [ ] Agregar caso de uso CU6 para consulta de historial
- [ ] Agregar clase `ConfiguracionMensual` si corresponde

---

**Nota final:** 

La especificación tiene fundamentos sólidos y estructura clara, pero requiere ajustes críticos en COMPLETITUD antes de proceder a implementación. 

La brecha entre la documentación teórica (que describe clases, herencias y patrones) y el diagrama técnico (PUML) debe resolverse para evitar confusiones críticas durante el desarrollo del código. No es prudente comenzar implementación con estas ambigüedades presentes.

**Rating:** 5/10 (incompleto, pero base sólida para correcciones)
