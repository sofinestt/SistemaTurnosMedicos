## Anexo – Principios SOLID

### Propósito general de los principios SOLID

Los principios SOLID son un conjunto de cinco reglas fundamentales de diseño con el propósito general de crear estructuras de software de "nivel medio" que sean más comprensibles, flexibles y mantenibles. Estos principios existen para combatir y mitigar síntomas de un diseño degradado o "podrido", tales como la rigidez, que hace al software difícil de cambiar; la fragilidad, que causa rupturas inesperadas en el sistema ante modificaciones simples; y la inmovilidad, que dificulta la reutilización de piezas de código en otros proyectos. Su aplicación mejora significativamente el diseño de software al facilitar la tolerancia al cambio, reducir el acoplamiento y aumentar la cohesión entre los componentes, lo que permite que el sistema evolucione con un impacto mínimo y proporciona una base técnica que asegura que el software esté asentado sobre "roca firme" ante los vientos del cambio.

### Los 5 principios:

SRP (Single Responsibility Principle): cada clase debe tener una única razón para cambiar. [Responsabilidad Única (SRP)](./01-srp.md)
OCP (Open/Closed Principle): las clases deben estar abiertas para extensión, pero cerradas para modificación. [Abierto/Cerrado (OCP)](./02-ocp.md)
LSP (Liskov Substitution Principle): las subclases deben poder sustituir a sus clases base sin alterar el comportamiento del sistema. [Sustitución de Liskov (LSP)](./03-lsp.md)
ISP (Interface Segregation Principle): es mejor tener varias interfaces específicas que una sola interfaz general con métodos innecesarios. [Segregación de Interfaces (ISP)](./04-isp.md)
DIP (Dependency Inversion Principle): los módulos de alto nivel no deben depender de módulos de bajo nivel, sino de abstracciones. [Inversión de Dependencias (DIP)](./05-dip.md)




