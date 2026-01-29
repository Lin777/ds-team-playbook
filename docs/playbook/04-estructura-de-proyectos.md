# 04 · Estructura de proyectos

> Una buena estructura no hace que el código sea mejor.  
> Hace que **los problemas aparezcan antes**.

Este capítulo propone una estructura base para proyectos de Data Science
y algunas variantes comunes según el tipo de trabajo.

No es una receta rígida.
Es una forma de **guiar el crecimiento del proyecto**
sin que se vuelva frágil o difícil de mantener.

---

## El problema que estamos tratando de evitar

Muchos proyectos de Data Science empiezan de forma ordenada
y, sin que nadie lo decida explícitamente, terminan así:

- notebooks con lógica crítica
- scripts con nombres ambiguos
- imports que dependen del directorio desde donde se corre algo
- dificultad para testear
- miedo a mover archivos “porque todo se rompe”

Nada de esto pasa de golpe.
Pasa cuando el proyecto crece **sin una estructura que acompañe**.

---

## La idea central

La estructura del proyecto debería:

- separar exploración de código reutilizable
- facilitar imports claros y estables
- hacer evidente qué es “core” y qué es auxiliar
- reducir la carga cognitiva para quien entra nuevo

O dicho de otra forma:

> la estructura debería hacer que **lo correcto sea lo fácil**.

---

## La estructura base

La mayoría de los proyectos pueden empezar con algo así:

```

project/
├── src/
├── notebooks/
├── tests/
├── data/
├── docs/
└── pyproject.toml

```

Esta estructura no dice *cómo* escribir el código,
pero sí **dónde vive cada tipo de cosa**.

---

## Responsabilidad de cada carpeta

### `notebooks/`

Exploración, análisis, visualización y comunicación.

Es el lugar para:

- entender los datos
- probar ideas
- contar historias

No es el lugar para:

- lógica crítica
- código que otros módulos dependan

---

### `src/`

Código reutilizable, importable y testeable.

Todo lo que:

- se usa más de una vez
- necesita tests
- podría terminar en producción

…tiene más sentido acá que en un notebook.

---

### `tests/`

Validación automática del comportamiento esperado.

Tener tests separados:

- reduce el miedo a refactorizar
- hace visibles los contratos del código
- mejora la calidad sin depender de memoria humana

---

### `data/`

Datos locales o temporales.

Normalmente:

- no se versionan en Git
- se regeneran desde una fuente conocida

La idea es clara: **datos y código no son lo mismo**.

---

### `docs/`

Documentación del proyecto:

- decisiones
- contexto
- guías de uso

Cuando la documentación vive cerca del código,
tiende a mantenerse más actualizada.

---

## Variantes comunes

No todos los proyectos son iguales.
La estructura base se puede adaptar sin romper la idea central.

---

### Variante A · Proyecto exploratorio (research-heavy)

Cuando el foco está en investigación temprana:

```

project/
├── notebooks/
├── src/
├── data/
└── pyproject.toml

```

Características:

- muchos notebooks
- código aún en evolución
- menor énfasis en tests

Señal para evolucionar:

- lógica que se copia entre notebooks
- funciones que “ya no son tan temporales”

---

### Variante B · Proyecto orientado a producción

Cuando el objetivo es robustez y estabilidad:

```

project/
├── src/
├── tests/
├── notebooks/
├── docs/
└── pyproject.toml

```

Características:

- código en `src/` como centro
- tests desde temprano
- notebooks como soporte, no núcleo

---

### Variante C · Proyecto híbrido (el más común)

La mayoría de los proyectos terminan acá:

```

project/
├── src/
├── notebooks/
├── tests/
├── data/
├── docs/
└── pyproject.toml

```

Exploración y producción conviven,
pero **con límites claros**.

---

## Antipatrones (señales de alerta)

Los antipatrones no son errores morales.
Son **señales tempranas** de que el proyecto está pidiendo estructura.

- *Toda la lógica vive en notebooks*  
  Funciona rápido. Escala mal.

- *Scripts con nombres tipo `final.py`*  
  Cuando todo es final, nada lo es.

- *Imports que dependen del directorio actual*  
  Si mover un archivo rompe todo, la estructura no está ayudando.

- *Copiar y pegar funciones entre archivos*  
  Suele ser el primer síntoma de deuda estructural.

Ver estas señales no implica “fallamos”.
Implica: **es momento de evolucionar**.

---

## Cuándo evolucionar la estructura

Algunas señales claras:

- más de una persona tocando el mismo código
- necesidad de tests
- PRs más grandes y difíciles de revisar
- miedo a refactorizar

La estructura no se diseña una vez.
Se **ajusta cuando el proyecto lo pide**.

---

## Relación con el resto del playbook

- [`03 · Entornos`](03-entornos-y-dependencias.md) hace posible que esta estructura sea reproducible
- [`05 · Estándares de código`](05-estandares-de-codigo.md) define cómo se escribe dentro de ella
- testing y CI se vuelven naturales cuando el código vive en `src/`

Este capítulo conecta
la exploración inicial
con la construcción de algo que pueda durar.

---

## Cierre

La estructura no garantiza éxito.
Pero **la falta de estructura garantiza fricción**.

Un proyecto bien estructurado
no se siente más rígido,
se siente más fácil de entender,
de cambiar
y de escalar.
