# Principio de Sustitución de Liskov (LSP)

## Propósito y Tipo del Principio SOLID

El Principio de Sustitución de Liskov (LSP) es uno de los cinco principios SOLID en el diseño orientado a objetos. Establece que los objetos de una superclase deben poder ser reemplazados por objetos de sus subclases sin alterar el funcionamiento correcto del programa. En otras palabras, si S es una subclase de T, entonces los objetos de tipo T deberían poder ser reemplazados por objetos de tipo S sin modificar las propiedades correctas del programa. Este principio ayuda a prevenir violaciones sutiles en las jerarquías de herencia que pueden llevar a comportamientos inesperados y errores difíciles de detectar.

## Motivación

En el diseño orientado a objetos, las jerarquías de herencia pueden parecer correctas a simple vista, pero pueden ocultar violaciones del contrato entre clases. LSP nos ayuda a detectar estas violaciones asegurando que las subclases mantengan el comportamiento esperado de sus superclases.

En el sistema de turnos médicos, un ejemplo claro de la necesidad de aplicar LSP se encuentra en la jerarquía de clases Persona. Consideremos la jerarquía Persona → Paciente, Medico, Secretaria. La clase Secretaria hereda de Persona pero no agrega propiedades nuevas, solo hereda los datos básicos (nombre, apellido, telefono, mail). Esto plantea un problema: si el código cliente espera que las subclases de Persona tengan comportamientos distintivos o propiedades adicionales, la sustitución de Persona por Secretaria podría no funcionar como esperado, violando LSP.

Por ejemplo, si tenemos un método que procesa roles basándose en el tipo de subclase, como autorizar sobreturnos para Médicos, el uso de polimorfismo se rompe si dependemos de instanceof, lo que indica una violación de LSP.

## Explicación de Herencia

La herencia en programación orientada a objetos es un mecanismo que permite a una clase (subclase) heredar propiedades y métodos de otra clase (superclase), promoviendo la reutilización de código y la creación de jerarquías. Para cumplir con LSP, la herencia debe usarse de manera que las subclases puedan sustituir a sus superclases sin alterar el comportamiento esperado. Esto significa que las subclases deben respetar el contrato de la superclase, incluyendo precondiciones, postcondiciones y invariantes.

En el diseño de jerarquías del sistema de turnos, la herencia se aplica para modelar relaciones "es-un" válidas. Por ejemplo, un Paciente "es una" Persona, por lo que hereda los datos personales y agrega comportamientos específicos como pedirTurno(). Sin embargo, si una subclase como Secretaria no extiende el comportamiento de manera significativa, podría indicar que la herencia no es apropiada y que se debería considerar composición en su lugar.

## Estructura de Clases

A continuación, se presenta un diagrama UML que ilustra la jerarquía de clases Persona y sus subclases, destacando cómo las subclases pueden (o no) sustituir a la superclase sin alterar el comportamiento esperado.

![Diagrama LSP](diagramas/01-diagrama-clases/01-solid-03-lsp.png)

El diagrama PlantUML correspondiente se encuentra en [diagramas/01-diagrama-clases/01-solid-03-lsp.puml](diagramas/01-diagrama-clases/01-solid-03-lsp.puml).

## Justificación Técnica

El diagrama UML muestra la jerarquía Persona como superclase abstracta con atributos comunes (nombre, apellido, telefono, mail) y métodos getters. Las subclases Paciente, Medico y Secretaria heredan de Persona.

- **Paciente**: Agrega atributos específicos (dni, fechaNacimiento) y métodos como pedirTurno() y cancelarTurno(). Cumple con LSP porque extiende el contrato de Persona con comportamiento adicional sin alterar el esperado.

- **Medico**: Agrega especialidad, matricula y métodos como consultarAgenda() y autorizarSobreturno(). También cumple LSP al mantener la sustituibilidad.

- **Secretaria**: No agrega atributos nuevos, solo métodos operativos como gestionarTurnos(). Esto representa un problema LSP porque es un "falso subtipo": hereda datos pero no extiende significativamente el contrato. En el dominio, Secretaria es un rol operativo, no una entidad con estado propio, lo que sugiere usar composición en lugar de herencia.

La solución propuesta es correcta técnicamente porque identifica la violación y recomienda composición para Secretaria, preservando LSP al evitar herencias inválidas. Las clases Paciente y Medico demuestran sustitución válida, mientras que Secretaria ilustra el riesgo de herencia inapropiada. Esto asegura que el código cliente pueda usar objetos de subclases indistintamente sin sorpresas, manteniendo la robustez del sistema.