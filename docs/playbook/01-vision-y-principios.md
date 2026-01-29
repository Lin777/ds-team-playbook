# 01 · Visión y principios

Antes de hablar de herramientas, estructuras o convenciones, vale la pena responder una pregunta clave:

> **¿Cómo queremos que funcione un equipo de Data Science en la práctica?**

Este capítulo define la **filosofía de trabajo** detrás de todo el playbook.  
Las “reglas” que aparecen más adelante no existen por amor al orden, sino para **evitar problemas muy concretos** que aparecen cuando los equipos crecen.

---

## La visión (en simple)

Un equipo de Data Science saludable debería poder:

- incorporar a alguien nuevo **en días, no semanas**
- reproducir cualquier experimento pasado **sin arqueología**
- cambiar un modelo **sin miedo**
- discutir decisiones técnicas **con contexto, no con opiniones sueltas**
- pasar de exploración a producción **sin reescribir todo**

Si alguna de estas cosas no es posible, el problema casi nunca es “falta de talento”, sino **falta de estructura**.

---

## Por qué hacen falta principios

Cuando no hay principios explícitos, aparecen patrones conocidos:

- cada proyecto se organiza distinto
- cada persona tiene su “forma preferida”
- las decisiones se repiten una y otra vez
- los errores vuelven… pero con nombres distintos

!!! warning "Patrón clásico"
    “Este proyecto es especial, acá no aplican las reglas.”

Casi nunca es tan especial.  
Y cuando lo es, **igual conviene documentar por qué**.

Los principios sirven como **filtro de decisiones**.  
No te dicen exactamente *qué* hacer, pero sí *cómo pensar* cuando aparece una duda.

---

## Principios del playbook

### 1️⃣ Reproducibilidad antes que heroísmo

Si un resultado solo puede ser reproducido por una persona, en una laptop específica, un martes con luna llena… **tenemos un problema**.

La reproducibilidad permite:

- confiar en los resultados
- iterar sin miedo
- depurar errores reales (no fantasmas)

Cuando no se prioriza:

- “funciona en mi máquina”
- métricas que cambian sin explicación
- imposibilidad de auditar decisiones pasadas

!!! tip "Regla práctica"
    Si mañana te vas de vacaciones, el equipo debería poder seguir trabajando.

---

### 2️⃣ Decisiones explícitas, no implícitas

Herramientas, librerías y convenciones **siempre son decisiones**, incluso cuando no se documentan.

No escribirlas lleva a:

- discusiones recurrentes
- cambios silenciosos
- falta de contexto para personas nuevas

Por eso usamos **Architecture Decision Records (ADR)**:

- para dejar claro *qué* se eligió
- *por qué* se eligió
- y *qué alternativas se descartaron*

!!! info "Dato honesto"
    Muchas ADRs nacen después de una discusión repetida por tercera vez.

---

### 3️⃣ Notebooks para explorar, código para producir

Los notebooks son increíbles para:

- explorar datos
- probar ideas
- visualizar resultados

Son muy malos para:

- versionado
- testing
- reutilización
- producción

Cuando no se separan responsabilidades:

- notebooks gigantes imposibles de mantener
- lógica crítica escondida en celdas
- dificultad para revisar cambios

!!! tip "Modelo mental"
    El notebook es el laboratorio.  
    El código es la fábrica.

---

### 4️⃣ Automatizar lo aburrido (antes de que se rompa)

Todo lo que se hace a mano:

- se olvida
- se hace distinto cada vez
- eventualmente falla

Automatizar no es “sobreingeniería”, es:

- reducir errores humanos
- ahorrar tiempo
- detectar problemas antes

Ejemplos claros:

- tests que corren solos
- linting automático
- CI que falla antes de mergear

!!! warning "War story"
    La mayoría de bugs graves no son complejos.  
    Son cosas simples que nadie automatizó.

---

### 5️⃣ Convenciones claras reducen fricción

Las convenciones no existen para limitar, sino para **evitar decisiones innecesarias**.

Cuando no hay convenciones:

- cada PR genera debates de estilo
- el código se vuelve inconsistente
- revisar se vuelve lento y frustrante

Con convenciones claras:

- los repos “se sienten iguales”
- el foco está en la lógica, no en el formato
- el feedback es más fácil y menos personal

!!! tip "Resultado esperado"
    Menos discusiones sobre *cómo*, más conversaciones sobre *qué* y *por qué*.

---

## Cómo usar estos principios

Estos principios no son dogmas. Son **guías prácticas**.

- Si una regla no aplica a tu contexto → documenta el porqué
- Si una convención genera más problemas que soluciones → propón un cambio
- Si algo “siempre fue así” → es buen candidato a revisión

La única regla realmente importante es esta:

!!! danger "Regla no negociable"
    Los problemas se hacen visibles.  
    Lo implícito es el enemigo del equipo.

---

## Relación con el resto del playbook

A partir de aquí:

- **Onboarding** muestra cómo estos principios se traducen en práctica
- **Entornos, Git y código** los operacionalizan
- **Experimentación y ML lifecycle** los llevan al mundo real
- **Templates y ADRs** ayudan a mantener consistencia sin esfuerzo extra

Si entiendes este capítulo, el resto del playbook **tiene sentido**.

---

## Cierre

Este playbook no busca crear equipos “perfectos”, sino equipos que:

- aprenden
- mejoran
- y no tropiezan con la misma piedra cada trimestre

O dicho de otra forma:

> No podemos evitar todos los problemas.  
> Pero sí podemos evitar los **problemas conocidos**.
