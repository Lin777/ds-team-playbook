# 08 · Experimentación y trazabilidad

> Un experimento que no se puede explicar  
> es solo una anécdota.

Este capítulo define cómo experimentamos en Data Science
de forma **ordenada, trazable y razonablemente reproducible**.

No buscamos rigor académico perfecto.
Buscamos poder responder, con honestidad:
**qué hicimos, por qué lo hicimos y qué aprendimos**.

---

## El problema que estamos tratando de evitar

Sin una forma clara de experimentar, aparecen patrones conocidos:

- notebooks con resultados “buenos” imposibles de repetir
- parámetros cambiados sin registro
- datasets que evolucionan sin dejar rastro
- decisiones tomadas por intuición sin evidencia clara

El resultado no es solo desorden,
es **pérdida de confianza en el trabajo hecho**.

---

## Qué significa “reproducible” en la práctica

En Data Science, reproducible no siempre significa
obtener exactamente el mismo número.

Significa:

- entender qué variables importaron
- poder repetir el proceso con resultados comparables
- explicar por qué algo funcionó (o no)

La reproducibilidad es un **continuo**, no un switch.

---

## Experimentar es un proceso, no un evento

Un buen experimento tiene, como mínimo:

- una hipótesis clara
- un cambio controlado
- una forma de evaluar resultados
- una conclusión explícita

No todos los experimentos son exitosos.
Pero **todos deberían dejar aprendizaje**.

---

## 🔒 MUST · Registrar los experimentos

Todo experimento relevante debe quedar registrado.

No importa la herramienta.
Importa que exista un rastro claro.

En este equipo usamos **Markdown** como formato base,
porque es:

- simple
- versionable
- fácil de revisar
- independiente de herramientas

---

### Template oficial de experimentos

!!! tip
    Usamos un template estándar para reportar experimentos, lo puedes encontrar en [`../docs-templates/experiment-report-template.md`](../docs-templates/experiment-report-template.md).

Ese archivo define:

- qué información se espera
- cómo estructurar el reporte
- qué preguntas responder

El capítulo explica *por qué*.
El template muestra *cómo se ve en la práctica*.

---

??? info "Cómo usar el template en la práctica"
    - Cada experimento relevante genera un archivo Markdown
    - El archivo vive en el repo (por ejemplo en `experiments/`)
    - No todos los experimentos tienen que ser “buenos”
    - Todos deberían ser **entendibles**

---

## Notebooks como laboratorio

Los notebooks son excelentes para:

- explorar ideas
- analizar resultados
- comunicar hallazgos

Un notebook bien usado:

- narra el proceso
- muestra decisiones
- no es la única fuente de verdad

Cuando un experimento importa:

- sus conclusiones se registran
- sus decisiones se documentan
- su resultado se resume fuera del notebook

---

??? example "Ejemplo · Buen experimento vs mal experimento"
    ❌ **Mal experimento**

    > “Probé varias cosas y este modelo funcionó mejor.”

    - sin hipótesis
    - sin contexto
    - imposible de reproducir

    ---

    ✅ **Buen experimento**

    > “Reducir la dimensionalidad mejoró estabilidad,
    > pero no impacto significativamente la métrica principal.”

    - hipótesis clara
    - cambio explícito
    - aprendizaje documentado

---

## Seeds, aleatoriedad y expectativas realistas

Fijar seeds ayuda a:

- debuggear
- comparar cambios
- reducir ruido innecesario

Pero obsesionarse con determinismo total
suele ser una falsa sensación de control.

Regla práctica:

- fija seeds cuando ayuden a entender
- no fuerces reproducibilidad artificial

---

## 🌱 DESEABLE · Herramientas de tracking

Por defecto, el playbook es **tool-agnostic**.

Sin embargo, si el equipo o proyecto lo permite,
herramientas como **MLflow** pueden agregar valor:

- tracking automático de parámetros
- comparación de runs
- registro de artefactos

Esto es especialmente común en plataformas como Databricks.

Si no existe esa infraestructura,
Markdown sigue siendo una excelente opción.

---

## 🌱 DESEABLE · Datasets y versionado (nivel básico)

Idealmente, un experimento debería dejar claro:

- qué datos usó
- de dónde salieron
- en qué versión estaban

No es necesario implementar sistemas complejos desde el inicio.

Un primer paso suficiente puede ser:

- hash del dataset
- fecha de extracción
- referencia a la fuente

!!! tip
    Si quieres ir más allá,
    vale la pena explorar herramientas como DVC
    o enfoques de data versioning más formales.

---

## Antipatrones (señales de alerta)

- “este fue el mejor de muchos”
- resultados sin contexto
- experimentos sin registro
- notebooks como única fuente de verdad

Estos patrones hacen difícil aprender,
incluso cuando el resultado es bueno.

---

## Relación con el resto del playbook

- [`03 · Entornos`](03-entornos-y-dependencias.md) → reproducibilidad técnica
- [`07 · CI`](07-calidad-testing-y-ci.md) → reproducibilidad del código
- [`08 · Experimentos`](08-experimentacion-y-trazabilidad.md) → reproducibilidad del proceso
- [`09 · Lifecycle`](09-ml-lifecycle-y-produccion.md) → reproducibilidad en producción

Este capítulo es el puente
entre explorar y decidir.

---

## Cierre

No todos los experimentos llevan a producción.
Pero todos deberían dejar aprendizaje.

Registrar experimentos no es burocracia.
Es **respetar el trabajo ya hecho**.
