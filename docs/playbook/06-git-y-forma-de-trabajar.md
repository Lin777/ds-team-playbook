# 06 · Git y forma de trabajar

> Git no es solo una herramienta de versionado.  
> Es una forma de comunicarnos con otras personas  
> y con nuestro yo del futuro.

Este capítulo define **convenciones simples de Git**
pensadas para equipos de Data Science pequeños,
donde la claridad y la velocidad importan más que la ceremonia.

---

## El problema que queremos evitar

Sin acuerdos claros, Git suele volverse:

- commits difíciles de entender
- ramas que viven demasiado tiempo
- PRs grandes que nadie quiere revisar
- secretos que llegan al repo por accidente

Nada de esto es falta de profesionalismo.
Es **falta de estructura mínima compartida**.

---

## 🔒 Convenciones de commits (normativo)

Cada commit debería representar **una intención clara**.

El mensaje importa porque:

- ayuda a entender *qué pasó*
- facilita debugging y reverts
- construye una historia útil del proyecto

### Formato recomendado

```

<tipo>: <mensaje corto y claro>

```

Tipos más comunes en proyectos de Data Science:

- `feat` – nueva funcionalidad
- `fix` – corrección de bugs
- `docs` – cambios de documentación
- `refactor` – cambios internos sin cambiar comportamiento
- `chore` – tareas de mantenimiento

---

??? example "Ejemplos · Mensajes de commit (bueno vs malo)"
    ❌ **Malos mensajes**

    - `update`
    - `fix stuff`
    - `changes`
    - `wip`

    No dicen qué cambió ni por qué.

    ---

    ✅ **Buenos mensajes**

    - `feat: add prompt router for multi-model fallback`
    - `fix: prevent leaking PII in request logs`
    - `docs: explain uv workflow for new contributors`
    - `refactor: extract feature engineering pipeline`

    Son específicos, accionables y fáciles de entender.

---

## 🔒 Ramas (branches)

Usamos un enfoque **trunk-based light**.

La idea es simple:

- `main` siempre está en estado deployable
- las ramas son cortas
- se eliminan después del merge

### Naming de ramas

Formato recomendado:

```

<tipo>/<descripcion-corta>

```

Ejemplos:

- `feat/prompt-cache`
- `fix/gcp-auth-timeout`
- `docs/onboarding`

Este naming ayuda a:

- entender el propósito de la rama
- navegar el repo
- mantener consistencia en el equipo

---

## 🔒 Pull Requests (PRs) en equipos pequeños

En equipos de hasta 3 personas,
los PRs deben ser **rápidos de entender y revisar**.

Un buen PR:

- hace una cosa
- tiene un tamaño razonable
- se puede revisar en minutos, no horas

---

### Qué debe explicar un PR

Un PR debería responder claramente:

- **Qué cambió**
- **Por qué cambió**
- **Cómo probarlo** (si aplica)

!!! tip
    Usamos un template estándar para los PRs, que está en [`../docs-templates/pull_request_template.md`](../docs-templates/pull_request_template.md).

---

??? example "Ejemplo · PR claro vs PR confuso"
    ❌ **PR confuso**

    > “Varios cambios y mejoras”

    - cambios mezclados
    - sin contexto
    - difícil de revisar

    ---

    ✅ **PR claro**

    > “Add prompt cache to reduce latency on repeated queries”

    Incluye:
    - breve contexto del problema
    - qué se hizo
    - impacto esperado
    - notas para el reviewer

---

### Tamaño del PR

No hay un número mágico,
pero una buena regla empírica es:

> Si el PR no se puede explicar en pocas frases,  
> probablemente se puede dividir.

PRs pequeños:

- reciben mejor feedback
- se mergean más rápido
- generan menos conflictos

---

## 🔒 `.gitignore` como barrera de seguridad

Hay archivos que **nunca** deberían llegar al repositorio.

Entre ellos:

- `.env`
- credenciales
- data local
- outputs temporales
- checkpoints

Estos archivos:

- viven fuera de Git
- se gestionan localmente o por infraestructura

Si alguno de ellos aparece en un diff,
el PR **no se mergea**.

Esto no es burocracia.
Es **protección del equipo y del proyecto**.

---

??? danger "Ejemplo · Señales de alerta en un PR"
    - aparece un archivo `.env`
    - se ve una API key en texto plano
    - se sube data local “solo para probar”

    Cualquiera de estas señales
    bloquea el merge hasta corregirlo.

---

## 🌱 Avanzado · Merge vs Rebase

No es obligatorio dominar esto desde el día uno,
pero es importante entender la diferencia.

### Merge

- conserva toda la historia
- más explícito
- menos riesgoso

Es la opción **recomendada por defecto**.

---

### Rebase

- produce una historia más lineal
- reescribe commits
- requiere cuidado

Regla simple:

> Nunca rebasear una rama que otras personas usan.

El rebase es útil:

- en ramas personales
- antes de abrir un PR
- para limpiar commits locales

---

## Antipatrones (señales de alerta)

- commits genéricos
- ramas que duran semanas
- PRs gigantes
- force-push sin contexto
- secretos en el repo (aunque sea por accidente)

Estos patrones suelen indicar
que el proceso necesita ajuste.

---

## Relación con el resto del playbook

- [`05 · Estándares de código`](05-estandares-de-codigo.md) define qué esperamos del código
- [`06 · Git`](06-git-y-forma-de-trabajar.md) define cómo colaboramos sobre él
- [`07 · Calidad y CI`](07-calidad-testing-y-ci.md) automatiza estos acuerdos

Git es el punto donde
todas las prácticas se encuentran.

---

## Cierre

Git no debería ser una fuente de fricción.
Debería ser una herramienta que **facilite el trabajo en equipo**.

Con reglas simples y compartidas:

- los cambios se entienden mejor
- los errores se detectan antes
- el equipo avanza con menos desgaste
