# Documentador Coordinador

## Objetivo

Documentar los prompts utilizados en GitHub Copilot (modo agente) durante la corrección del especialista en el Principio de Sustitución de Liskov (LSP).

---

## Prompts utilizados

### Prompt 1: Análisis de LSP

```Analiza las clases del sistema de turnos médicos y detecta posibles violaciones al principio de sustitución de Liskov (LSP). Indica si alguna subclase altera el comportamiento esperado de la clase base.```

Resultado:
Copilot identificó posibles inconsistencias en métodos sobrescritos donde las subclases modificaban validaciones y resultados esperados.

---

### Prompt 2: Mejora de diseño

```Sugiere cambios para asegurar que las subclases puedan reemplazar correctamente a sus clases base sin generar comportamientos inesperados.```

Resultado:
Se recomendó mantener contratos consistentes entre clases, evitando modificar precondiciones y postcondiciones.

---

### Prompt 3: Validación de sobrescrituras

```Revisa los métodos sobrescritos y verifica si respetan el comportamiento definido en la clase base.```

Resultado:
Se detectaron métodos que alteraban la lógica original, recomendando unificar criterios de validación.

---

### Prompt 4: Refactorización

```Propone una refactorización para mejorar la aplicación del principio de sustitución de Liskov en el sistema.```

Resultado:
Copilot sugirió reorganizar la jerarquía de clases y evitar dependencias innecesarias entre subclases.

---

## Conclusión

El uso de GitHub Copilot permitió identificar problemas en la estructura de clases y guiar la aplicación correcta del principio LSP.

Los prompts utilizados facilitaron un proceso iterativo de análisis, corrección y validación del diseño.