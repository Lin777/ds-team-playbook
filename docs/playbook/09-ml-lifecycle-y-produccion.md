# 09 · ML lifecycle y producción

> No todos los modelos nacen para producción.  
> Y está bien que así sea.

Este capítulo describe cómo pensamos el **ciclo de vida de un modelo**
desde que es un experimento hasta (si corresponde) producción.

No busca enseñar a desplegar modelos.
Busca **poner criterios claros** para decidir
cuándo algo está listo para dar ese paso
y qué responsabilidades aparecen después.

---

## El problema que estamos tratando de evitar

Sin una visión clara de lifecycle, suelen pasar cosas como:

- modelos “en producción” sin dueño claro
- cambios experimentales impactando usuarios reales
- degradación silenciosa de performance
- dificultad para volver atrás
- confusión entre research y producto

El problema rara vez es técnico.
Es **falta de criterios y ownership explícitos**.

---

## El lifecycle como estados, no como pipeline rígido

Pensamos el lifecycle como **estados mentales distintos**,
no como una línea recta inevitable.

Un modelo puede avanzar, quedarse, o retirarse.

```

Exploración
↓
Validación
↓
Pre-producción
↓
Producción
↓
Monitoreo
↓
Retirada

```

No todos los modelos recorren todo el camino.
Forzar eso suele ser una mala señal.

---

## Etapas del lifecycle (visión práctica)

### Exploración

- notebooks
- hipótesis
- experimentos rápidos
- aprendizaje

Aquí prima la velocidad sobre la robustez.

---

### Validación

- comparación con baselines
- resultados más estables
- decisiones explícitas

No es producción.
Es **confirmar que vale la pena seguir**.

---

### Pre-producción

- código más estable
- contratos claros
- tests mínimos
- revisión más estricta

Esta etapa evita llevar experimentos frágiles a producción.

---

### Producción

- usuarios reales
- impacto real
- costo real del error

Acá cambia el juego.

---

### Monitoreo

- performance en el tiempo
- cambios en los datos
- señales de degradación

Un modelo sin monitoreo
no está realmente en producción.

---

### Retirada

- modelos también se apagan
- retirar es una decisión válida
- planear salida desde el inicio es sano

---

## 🔒 MUST · Ownership claro

Todo modelo en producción debe tener un **owner explícito**.

El owner es responsable de:

- entender el modelo
- responder a incidentes
- decidir cuándo iterar o retirar

No significa trabajar solo.
Significa que **alguien tiene la responsabilidad final**.

!!! danger
    “Todos lo cuidamos”  
    suele significar  
    “nadie lo cuida”.

---

## Qué cambia cuando algo entra a producción

Pasar a producción implica aceptar nuevos trade-offs:

- estabilidad > novelty
- simplicidad > complejidad
- interpretabilidad > mejoras marginales
- seguridad > rapidez

Muchas ideas buenas en research
no sobreviven estos trade-offs.
Eso es normal.

---

## 🔒 MUST · Monitoreo mínimo esperado

Un modelo en producción debe tener,
como mínimo, señales de monitoreo claras.

### Qué monitorear (nivel básico)

- métricas de performance (cuando sea posible)
- distribución de inputs
- volumen / frecuencia de uso
- errores y excepciones

No todo requiere dashboards complejos.
Requiere **visibilidad suficiente**.

---

??? example "Ejemplo · Señales de monitoreo simples"
    - caída sostenida de performance
    - cambios fuertes en la distribución de inputs
    - aumento de errores
    - outputs fuera de rango esperado

    Estas señales no explican todo,
    pero avisan **cuándo mirar más de cerca**.

---

## 🌱 DESEABLE · Tracking y herramientas

Por defecto, el playbook es **tool-agnostic**.

Sin embargo, si el equipo o proyecto lo permite,
herramientas como **MLflow (open-source)** pueden ser útiles:

- tracking de experimentos
- registro de modelos
- comparación de runs
- metadata centralizada

MLflow puede usarse:

- en modo local
- sin costo
- sin depender de plataformas comerciales

No es obligatorio.
Es una **mejora progresiva**, no un requisito.

---

## Data drift y performance (sin obsesión)

El drift existe.
Ignorarlo no lo elimina.

Pero medir todo desde el día uno
suele generar ruido.

Regla práctica:

- empieza simple
- mide lo que puedas explicar
- agrega complejidad solo si aporta señal

---

## Antipatrones (señales de alerta)

- “este notebook ya está en prod”
- modelos sin métricas post-deploy
- nadie sabe quién es el owner
- cambios silenciosos
- no hay forma clara de rollback

Estos patrones suelen indicar
que el lifecycle no está claro.

---

## Relación con el resto del playbook

- [`08 · Experimentación`](08-experimentacion-y-trazabilidad.md) → decide qué vale la pena
- [`07 · Calidad`](07-calidad-testing-y-ci.md) → asegura estabilidad técnica
- [`09 · Lifecycle`](09-ml-lifecycle-y-produccion.md) → gestiona impacto real
- capítulos posteriores → profundizan operación

Este capítulo es donde
la experimentación se convierte en responsabilidad.

---

## Cierre

Llevar un modelo a producción
no es un premio.
Es un **compromiso**.

Tener criterios claros,
ownership definido
y monitoreo básico
no frena la innovación.

La hace sostenible.
