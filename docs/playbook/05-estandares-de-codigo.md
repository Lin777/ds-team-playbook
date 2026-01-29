# 05 · Estándares de código

> El código se escribe una vez.  
> Se lee muchas más veces.  
> Y casi nunca por la misma persona.

Este capítulo define **estándares normativos** sobre cómo escribimos código
en proyectos de Data Science.

No existen para imponer un “estilo personal”,
sino para **reducir fricción, errores y riesgos reales**
cuando el proyecto crece y más personas participan.

---

## Por qué hacen falta estándares

Sin estándares claros, aparecen patrones conocidos:

- cada persona escribe “a su manera”
- los PRs se llenan de comentarios de formato
- la lógica importante se pierde entre detalles
- errores simples llegan a producción

El problema no es el talento.
Es la **falta de acuerdos explícitos**.

Los estándares permiten que:

- el feedback se enfoque en lógica y decisiones
- el código sea fácil de leer y mantener
- los errores caros se detecten antes

---

## 🔒 MUST (no negociables)

Estas reglas se esperan **en todos los proyectos**.
Si no se cumplen, el código **no debería mergearse**.

---

### Secrets y configuración

Las keys **NO van en el código**.  
Las keys **NO se suben al repositorio**.  
Nunca.

Esto incluye, entre otros:

- API keys de LLMs
- credenciales de GCP / AWS
- tokens de acceso
- secretos de servicios externos

!!! danger "Mensaje claro"
    Exponer una key no es un descuido menor.  
    Es un **incidente de seguridad**.

Las consecuencias reales incluyen:

- bloqueos automáticos de cuentas
- rotación forzada de llaves
- interrupciones en producción
- exposición de infraestructura
- problemas de compliance

La configuración sensible se maneja **vía variables de entorno**.
El código asume que esas variables existen,
pero **no conoce sus valores**.

Usamos archivos `.env` como estándar para configuración local.

El código **nunca** depende de valores hardcodeados.
Depende de variables de entorno cuyo contrato está definido
en un archivo `.env.example` versionado.

Los archivos `.env` reales **no se versionan** (se agregan al `.gitignore`).

??? example "Ejemplo · Uso correcto de `.env` como estándar del equipo"
    📄 **Archivo versionado (`.env.example`)**

    ```env
    # LLMs
    OPENAI_API_KEY=
    ANTHROPIC_API_KEY=

    # Cloud
    GCP_PROJECT_ID=
    GCP_REGION=
    ```

    Este archivo:
    - vive en el repo
    - **no contiene valores reales**
    - define el contrato de configuración del proyecto

    ---

    📄 **Archivo local (NO versionado) (`.env`)**

    ```env
    OPENAI_API_KEY=sk-xxxx
    GCP_PROJECT_ID=my-project
    ```

    Este archivo:
    - existe solo en tu máquina
    - se carga localmente
    - **nunca se commitea**

    ---

    🧠 **Reglas del equipo**

    - `.env` está en `.gitignore`
    - `.env.example` siempre existe
    - si una variable nueva es necesaria:
        1. se agrega a `.env.example`
        2. se documenta brevemente
        3. se comunica al equipo

    > Si un proyecto no tiene `.env.example`,
    > el onboarding está incompleto.

---

### Código importable y reutilizable

La lógica crítica:

- vive en `src/`
- se puede importar
- se puede testear

Los notebooks:

- sirven para explorar
- sirven para comunicar
- **no** contienen lógica core del sistema

Cuando el código no es importable:

- testear se vuelve difícil
- refactorizar da miedo
- la duplicación aparece rápido

---

### Funciones pequeñas y explícitas

Las funciones deberían:

- hacer una cosa
- tener un propósito claro
- ser fáciles de leer de arriba hacia abajo

Funciones largas suelen indicar:

- demasiadas responsabilidades
- lógica difícil de testear
- código que nadie quiere tocar

!!! tip
    Si explicar una función requiere más de dos frases,
    probablemente hace demasiado.

---

### Nombres claros

Los nombres son parte del diseño.

Preferimos:

- nombres explícitos
- términos del dominio
- evitar abreviaciones crípticas

Un buen nombre:

- reduce la necesidad de comentarios
- hace el código más autoexplicativo
- facilita el onboarding

---

### Comportamiento explícito

Evitar:

- efectos secundarios escondidos
- dependencias implícitas
- estados globales mágicos

El código debería dejar claro:

- qué entra
- qué sale
- qué cambia

Esto hace que:

- los bugs sean más predecibles
- el debugging sea más simple
- el testing tenga sentido

---

## 🌱 DESEABLE (muy recomendado)

Estas prácticas no siempre son obligatorias,
pero **elevan mucho la calidad del código**
y deberían adoptarse cuando sea posible.

---

### Typing como herramienta de comunicación

El typing no está pensado solo para herramientas.
Está pensado para personas.

Un ejemplo simple:

```python
def predict(input_text: str) -> Prediction:
    ...
```

Sin leer la implementación ya sabemos:

- qué espera la función
- qué devuelve
- cómo usarla

El typing ayuda a:

- documentar contratos
- detectar errores temprano
- refactorizar con más seguridad

No buscamos perfección.
Buscamos **claridad suficiente**.

---

### Automatizar formato y checks

El formato manual genera:

- discusiones innecesarias
- PRs ruidosos
- inconsistencias evitables

Automatizar:

- reduce fricción
- ahorra tiempo
- hace el feedback más valioso

Las herramientas concretas (formatter, linter, type checker)
se definen en ADRs,
pero el principio es claro:
**lo repetible se automatiza**.

---

### Notebooks como puente, no destino

Los notebooks son excelentes para:

- explorar
- visualizar
- iterar rápido

Señales de que un notebook ya no es exploratorio:

- funciones copiadas en varios lugares
- lógica usada por otros módulos
- dependencia de resultados previos

En ese punto, mover código a `src/`
no es burocracia,
es **cuidar el proyecto**.

---

### Comentarios que expliquen el “por qué”

Los comentarios más valiosos explican:

- decisiones
- trade-offs
- contexto

Evitar comentarios que solo describen *qué* hace el código.
Eso debería ser evidente por cómo está escrito.

??? example "Ejemplo 1 · Comentarios inline: lo que NO vs lo que SÍ"
    ❌ **Comentario poco útil**

    ```python
    # Cargamos el modelo
    model = load_model(path)
    ```

    No agrega información: el código ya lo dice.

    ✅ **Comentario que explica el “por qué”**

    ```python
    # Usamos este modelo base porque es el único que
    # mantiene latencia aceptable en producción.
    # El modelo más grande tiene mejor accuracy,
    # pero rompe el SLA bajo carga.
    model = load_model(BASE_MODEL_PATH)
    ```

    Este comentario aporta:
    - contexto
    - decisión
    - trade-off explícito

??? example "Ejemplo 2 · Docstrings: contrato + supuestos (no relleno)"
    Los docstrings no deberían repetir el código, sino explicar
    **qué problema resuelve** y **bajo qué supuestos**.

    ❌ **Docstring poco útil**

    ```python
    def predict(text):
        """Hace una predicción."""
        ...
    ```

    ✅ **Docstring que explica intención y contexto**

    ```python
    def predict(text: str) -> Prediction:
        """
        Genera una predicción usando el modelo actualmente
        desplegado en producción.

        Se asume que el texto ya fue preprocesado
        y que la latencia es un factor crítico.

        Cambiar el modelo puede impactar SLAs.
        """
        ...
    ```

    Este docstring deja claro:
    - qué hace la función
    - qué asume
    - qué no se debe cambiar a la ligera

??? example "Ejemplo 3 · LLMs: decisiones que NO deben quedar implícitas"
    En sistemas con LLMs, muchas decisiones no son obvias:
    modelo, temperatura, prompts, límites, etc.
    Esas decisiones **deben explicarse**.

    ❌ **Sin contexto**

    ```python
    response = client.chat(
        model="gpt-4o-mini",
        temperature=0.2
    )
    ```

    ✅ **Con contexto y trade-offs**

    ```python
    # Usamos gpt-4o-mini por balance costo/latencia.
    # temperature=0.2 reduce variabilidad para respuestas
    # determinísticas en flujos críticos.
    # No subir temperatura sin revisar impacto en outputs.
    response = client.chat(
        model="gpt-4o-mini",
        temperature=0.2
    )
    ```

    Esto evita:
    - cambios accidentales
    - discusiones repetidas
    - bugs “inexplicables”

---

## Antipatrones (señales de alerta)

Los antipatrones no son fallos morales.
Son **síntomas tempranos**.

- funciones gigantes
- lógica crítica sin tests
- código “mágico”
- secrets hardcodeados
- comentarios defensivos (“esto es horrible pero funciona”)

Ver estas señales es una invitación a mejorar,
no a señalar culpables.

---

## Relación con el resto del playbook

- [`04 · Estructura`](04-estructura-de-proyectos.md) define dónde vive el código
- [`06 · Git`](06-git-y-forma-de-trabajar.md) define cómo colaboramos sobre él
- [`07 · Calidad y CI`](07-calidad-testing-y-ci.md) automatiza estos estándares
- [`11 · Seguridad`](11-seguridad-privacidad-y-responsabilidad.md) profundiza riesgos y mitigaciones

Este capítulo convierte principios abstractos
en **reglas concretas del día a día**.

---

## Cierre

Los estándares no existen para limitar.
Existen para **proteger al equipo**:

- del caos
- de errores repetidos
- de riesgos innecesarios

El objetivo no es escribir código perfecto.
Es escribir código que **otras personas puedan entender,
usar y mejorar sin miedo**.
