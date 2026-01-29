# 07 · Calidad, testing y CI

> La calidad no aparece al final.  
> Se construye todos los días, en pequeño.

Este capítulo describe cómo aseguramos
**calidad mínima continua**
en proyectos de Data Science,
sin frenar el trabajo ni imponer prácticas irreales.

No buscamos cobertura perfecta.
Buscamos **confianza razonable**.

---

## Qué entendemos por calidad en Data Science

En Data Science, calidad no es solo accuracy.

También es:

- estabilidad
- reproducibilidad
- capacidad de detectar errores temprano
- confianza para cambiar cosas sin romper todo

Los tests y la CI no están para controlar,
sino para **reducir riesgos silenciosos**.

---

## 🔒 MUST · Testing mínimo esperado

No todo es testeable en Data Science,
y fingir que lo es solo genera frustración.

Pero hay cosas que **sí o sí** deben validarse.

---

### Qué sí testear

- funciones críticas
- contratos de entrada y salida
- tipos y shapes
- invariantes importantes del dominio
- casos borde conocidos

Estos tests suelen ser:

- rápidos
- deterministas
- baratos de mantener

---

### Qué no es prioridad (al inicio)

- métricas del modelo
- resultados exactos de entrenamiento
- notebooks exploratorios
- benchmarks complejos

Esto no significa “nunca”,
significa **no bloquear el avance temprano**.

---

??? example "Ejemplo · Test pequeño pero valioso"
    ```python
    def test_preprocess_output_shape():
        X = preprocess(raw_data)
        assert X.shape[1] == EXPECTED_NUM_FEATURES
    ```

    Este test:
    - no depende del modelo
    - detecta errores comunes
    - falla rápido y de forma clara

---

## 🌱 DESEABLE · Tests más expresivos

A medida que el proyecto madura,
algunos tests adicionales agregan mucho valor.

---

### Tests de invariantes del dominio

Reglas del negocio suelen ser más estables
que los modelos.

---

??? example "Ejemplo · Test de invariante"
    ```python
    def test_predictions_are_non_negative():
        preds = model.predict(sample_input)
        assert (preds >= 0).all()
    ```

    No valida performance.
    Valida una **regla del dominio**.

---

### Tests de errores esperados

Asegurarse de que el código falle
de forma controlada también es calidad.

---

??? example "Ejemplo · Error esperado"
    ```python
    def test_empty_input_raises_error():
        with pytest.raises(ValueError):
            preprocess([])
    ```

---

## Notebooks y testing

Los notebooks **no se testean directamente**.

La lógica que merece tests:

- se mueve a `src/`
- se importa desde notebooks
- se valida en tests aislados

Esto mantiene:

- notebooks livianos
- tests simples
- menos duplicación

---

## 🔒 MUST · CI como acuerdo automático

Usamos **GitHub Actions** como sistema de CI.

El objetivo no es sofisticación,
es **automatizar acuerdos básicos del equipo**.

La CI corre:

- en cada Pull Request
- en cada merge a `main`

??? info "Cómo implementar CI con GitHub Actions usando `uv` (guía rápida)"
    En este playbook usamos **`uv`** como estándar para
    entornos y dependencias.
    La CI sigue exactamente el mismo flujo que local.

    ### Documentación recomendada
    - [Guía oficial · Build and test Python (GitHub Actions)](https://docs.github.com/actions/guides/building-and-testing-python)
    - [Documentación oficial de `uv`](https://docs.astral.sh/uv/)

    ### Idea general
    1. El repo define dependencias en `pyproject.toml`
       y versiones exactas en `uv.lock`
    2. La CI recrea el entorno con `uv sync`
    3. Los tests se ejecutan dentro de ese entorno

    Esto garantiza que:
    - CI y local usan las mismas dependencias
    - los resultados son reproducibles
    - no hay installs “especiales” solo para CI

    ### Ejemplo mínimo de workflow (referencia)

    ```yaml
    name: CI

    on:
      pull_request:
      push:
        branches: [ "main" ]

    jobs:
      test:
        runs-on: ubuntu-latest

        steps:
          - uses: actions/checkout@v4

          - name: Set up Python
            uses: actions/setup-python@v5
            with:
              python-version: "3.11"

          - name: Install uv
            run: |
              curl -Ls https://astral.sh/uv/install.sh | sh

          - name: Sync environment
            run: |
              uv sync

          - name: Run tests
            run: |
              uv run pytest
    ```

    Este workflow:
    - instala `uv`
    - recrea el entorno desde `uv.lock`
    - ejecuta los tests dentro del entorno del proyecto
    - falla antes de mergear si algo está roto

---

### Qué debería correr en CI (mínimo viable)

- instalación del entorno
- ejecución de tests rápidos
- checks de calidad (lint / formato)

Si algo falla en CI,
el problema se detecta **antes de mergear**,
no después.

---

## CI vs testing local

Es importante entender la diferencia.

- **Testing local**  
  Feedback rápido para quien desarrolla.
  Permite iterar sin fricción.

- **CI**  
  Garantía compartida del equipo.
  Evita olvidos y errores humanos.

Uno no reemplaza al otro.
Se complementan.

---

## 🌱 DESEABLE · Expandir CI con el tiempo

Cuando el proyecto crece,
la CI puede evolucionar para incluir:

- tests más costosos
- checks adicionales
- validaciones específicas del proyecto

La clave es que:
> la CI crezca con el proyecto,
> no que lo bloquee desde el día uno.

---

## Antipatrones (señales de alerta)

- no hay tests “porque es research”
- tests lentos que nadie corre
- CI que falla por razones irrelevantes
- ignorar CI para “avanzar más rápido”

Estos patrones suelen
pagar intereses altos después.

---

## Relación con el resto del playbook

- [`05 · Estándares`](05-estandares-de-codigo.md) define qué esperamos del código
- [`06 · Git`](06-git-y-forma-de-trabajar.md) define cuándo se valida
- [`07 · CI`](07-calidad-testing-y-ci.md) lo hace automático
- [`09 · Lifecycle`](09-ml-lifecycle-y-produccion.md) lleva estos conceptos a producción

La CI es el punto donde
las buenas intenciones se vuelven verificables.

---

## Cierre

Los tests no garantizan calidad.
Pero **la ausencia de tests garantiza incertidumbre**.

La CI no elimina errores.
Pero evita que lleguen tarde.

Ese es el objetivo.
