# 03 · Entornos y dependencias

> Si el entorno no es confiable,  
> **todo lo demás se vuelve sospechoso**.

Este capítulo define cómo trabajamos con entornos y dependencias
para evitar fricción innecesaria en el día a día.

No es un tutorial exhaustivo de herramientas.
Es una guía práctica para **trabajar y crear proyectos sin dolores de cabeza**.

---

## Prerrequisito: `uv`

En este playbook usamos **`uv`** para gestionar entornos y dependencias.

Antes de seguir, asegúrate de tenerlo instalado en tu máquina.

👉 Documentación oficial e instalación:  
<https://docs.astral.sh/uv/>

No es necesario entender todo `uv` ahora.
Con poder ejecutar los comandos que aparecen en este capítulo,
es suficiente para empezar.

!!! tip
    Si `uv` no está instalado, nada de lo que sigue va a funcionar.
    Mejor resolver esto primero y volver.

---

## (A) Trabajar sobre un proyecto existente

La mayoría de las veces no vas a crear un proyecto desde cero,
sino que vas a **sumarte a uno que ya existe**.

Esta sección cubre ese escenario.

---

### Fuente de verdad del entorno

En este equipo hay dos archivos que importan de verdad:

- **`pyproject.toml`**  
  Define *qué dependencias necesita el proyecto*.

- **`lockfile`**  
  Define *exactamente qué versiones se usan*.

Ambos están versionados en Git.

!!! danger "Regla simple"
    Si el lockfile no está en el repo,
    el entorno **no es reproducible**, aunque hoy funcione.

Esto es lo que permite que:

- todas las personas partan del mismo entorno
- los resultados sean comparables
- el onboarding no dependa de setups personalizados

---

### Workflow esperado (el camino feliz)

Desde cero, una vez clonado el repositorio,el proyecto debería poder correrse así:

```bash
uv sync        # crea el entorno e instala exactamente las dependencias definidas
```

A partir de ahí, **todo** se ejecuta dentro del entorno:

```bash
uv run pytest          # corre tests usando el entorno del proyecto
uv run python src/train.py  # ejecuta scripts con dependencias correctas
uv run mkdocs serve   # levanta la documentación localmente
```

!!! tip
    Si ejecutas comandos sin `uv run`,
    probablemente estés usando otro entorno sin darte cuenta.

---

### Errores comunes (y por qué duelen)

❌ **“Solo agregué una librería rápida”**

Si no está declarada y versionada,
nadie más tiene tu entorno.

❌ **“Después actualizo el lockfile”**

Ese “después” suele llegar
cuando algo ya se rompió.

❌ **“Ignorar warnings del entorno”**

Los warnings suelen ser el primer aviso.
El incidente viene después.

---

## (B) Crear un proyecto desde cero

Crear un proyecto nuevo es una oportunidad única:
todo está limpio, nadie depende de nada
y todavía no hay deuda técnica.

También es el momento donde más fácil es
sentar malas bases sin darse cuenta.

Esta sección cubre **cómo empezar bien desde el día 0**.

---

### 1️⃣ Inicializar un proyecto

Desde una carpeta vacía:

```bash
uv init        # crea un pyproject.toml y estructura base del proyecto
```

Esto crea el archivo `pyproject.toml`,
que será la **fuente de verdad del proyecto**.

No hace falta que el proyecto sea un paquete complejo todavía.
Lo importante es que:

- exista un lugar único para dependencias
- las decisiones queden explícitas desde el inicio

!!! tip
    Cuanto antes exista el `pyproject.toml`,
    menos “decisiones invisibles” aparecen después.

---

### 2️⃣ Agregar dependencias

Las dependencias se agregan explícitamente,
no “porque hacen falta ahora”,
sino porque **forman parte del contrato del proyecto**.

Ejemplo:

```bash
uv add pandas scikit-learn
# agrega estas librerías al pyproject.toml
# y actualiza el lockfile automáticamente
```

Esto garantiza que:

- cualquiera pueda recrear el entorno
- el proyecto sea reproducible
- CI y producción usen lo mismo que local

!!! warning
    Instalar dependencias sin declararlas
    es crear deuda técnica sin escribirla.

---

### 3️⃣ Grupos de dependencias

No todas las dependencias cumplen el mismo rol.

Algunas son necesarias para:

- correr el proyecto
- desarrollar
- testear
- generar documentación

Si mezclamos todo en un solo lugar,
los entornos se vuelven pesados y confusos.

#### Qué son los grupos de dependencias

Un **grupo** es una forma de decir:

> “Estas dependencias solo se necesitan en ciertos contextos”.

Por ejemplo:

```bash
uv add --dev pytest ruff
# dependencias solo para desarrollo (tests, linting)

uv add --group docs mkdocs mkdocs-material
# dependencias solo para documentación
```

Esto deja claro:

- qué es esencial para producción
- qué es solo para desarrollo o docs
- qué se puede instalar opcionalmente

!!! tip
    Si una dependencia no se usa en producción,
    probablemente no debería estar en el grupo principal.

---

### 4️⃣ Buenas prácticas desde el día 0

Algunas decisiones tempranas evitan muchos problemas después:

- crear el entorno antes de escribir código
- versionar el lockfile desde el primer commit
- usar grupos de dependencias desde el inicio
- evitar installs “rápidos” fuera del estándar

!!! info "Regla empírica"
    Lo que se hace “solo por ahora”
    suele quedarse mucho más tiempo del esperado.

---

## Cómo saber si vamos bien

Algunas señales claras:

- cualquier persona puede correr el proyecto desde cero
- reproducir resultados antiguos es posible
- CI usa el mismo entorno que local
- los bugs son consistentes (y eso es bueno)

Cuando esto pasa,
el entorno deja de ser un problema.
Y ese es el objetivo.

---

## Cierre

Invertir tiempo en entornos rara vez se siente productivo.
Hasta que un día no lo haces
y todo se rompe al mismo tiempo.

Un entorno estable no acelera el trabajo.
Pero **evita que todo lo demás se vuelva lento**.
