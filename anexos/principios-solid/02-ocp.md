# Principios Abierto/Cerrado (OCP)

## Propósito y Tipo del principio SOLID
El principio de Abierto/Cerrado (OCP) implica que el comportamiento de un módulo puede ser extendido para satisfacer nuevos requerimientos sin necesidad de alterar su código fuente original.

- Abierto para la extensión: Significa que es posible ampliar el comportamiento del módulo a medida que cambian los requisitos de la aplicación
- Cerrado para la modificación: Significa que el código fuente de dicho módulo es inviolable y no debe ser retocado, ya que está probado y funciona correctamente. Modificarlo podría introducir errores en cascada o bugs inesperados.

## Motivación
El problema en el diseño actual, la lógica esta mezclada dentro de las clases. Por ejemplo si la clase `turno` tiene un if para calcular la duración según el tipo de consulta, cada vez que agregamos un nuevo tipo de consulta, debemos modificar el código existente de `turno`, lo que introduce riesgo de errores y viola el principio OCP.

el principio OCP lo resuelve cerrando el código fuente(La clase turno no se toca), y se abre para la extensión, creando nuevas clases que extiendan el comportamiento.

## Explicación de Herencia
La herencia es un mecanismo que permite que una subclase herede atributos y comportamientos de una superclase.

En el Sistema de Turnos se aplica en dos casos principales:
- 1.Turno
- `tipoConsulta`(superclase abstracta)
- `Control`,`PrimeraVez`,`Sobreturno`(subclases)

- 2.Disponibilidad
- `Agenda`(superclase abstracta)
- `medico`,`secretaria`(subclases)




## Estructura de Clases
```
@startuml diagrama-ocp-turno
class Turno {
    +estado : String
    +tipoConsulta : String
    +duracionReal: int
    +duracionPrevista : int
    +fecha : int
    +hora : int
    +nombre() : String
}
class TipoConsulta {
    +duracionPrevista : int
    +nombre : String
    +tipoConsulta : String
}
class Control {
    +duracionMinutos : int = 15
    +permiteExtension : boolean = false
}
class PrimeraVez {
    +duracionMinutos : int = 30
    +permiteExtension : boolean = true
}
class Sobreturno{
    +duracionMinutos : int
    +permiteExtension : boolean = true
}
Turno o-- TipoConsulta
TipoConsulta <-- Control
TipoConsulta <-- PrimeraVez
TipoConsulta <-- Sobreturno

@enduml
```
![Imagen](diagramas\01-diagrama-clases\01-solid-02-ocp.png)

## Justificación técnica

En el diagrama UML vemos, como a partir de la clase principal `Turno`, se genera una superclase abstracta `TipoConsulta` la cual a su vez tiene tres subclases `Control`,`PrimeraVez`,`Sobreturno`. Lo que hace el principio OCP es que se pueda agregar otro método a la clase principal `Turno` a través de subclases genereadas a partir de la superclase abstracta `TipoConsulta`, sin tener que modificar directamente la clase principal y evitando posibles errores o bugs.