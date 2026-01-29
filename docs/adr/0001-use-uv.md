# ADR-0001 · Usar `uv` para entornos y dependencias

- Estado: Aceptado
- Fecha: 2026-01-29
- Autor(es): Evelyn Cusi

---

## Contexto

En proyectos de Data Science, los entornos suelen romperse de formas
dolorosamente repetibles:

- instalaciones manuales que no quedan registradas
- entornos distintos entre personas y CI
- resultados que cambian “sin tocar nada”
  (spoiler: sí se tocó algo, solo que fue una dependencia)

El problema no es solo productividad,
es **reproducibilidad y confianza**.

Para este playbook necesitamos una solución que:

1. haga trivial recrear el entorno (local y CI),
2. deje una **fuente de verdad** para dependencias,
3. genere un **lockfile reproducible**,
4. sea lo suficientemente simple como para que el equipo la use siempre.

La industria se está moviendo hacia flujos
`pyproject.toml`-céntricos y uso de lockfiles
para reproducibilidad. :contentReference[oaicite:0]{index=0}

---

## Decisión

Adoptamos **`uv`** como herramienta estándar del equipo para:

- gestión de proyecto basada en `pyproject.toml`
- resolución de dependencias y generación de lockfile (`uv.lock`)
- creación y sincronización del entorno (`uv sync`)
- ejecución de comandos dentro del entorno (`uv run ...`)

`uv` se adopta como herramienta “todo en uno”
para el flujo diario de dependencias, entornos y ejecución. :contentReference[oaicite:1]{index=1}

---

## Motivación

### Reproducibilidad por defecto

El uso de un lockfile fija versiones exactas
(directas y transitivas),
reduciendo cambios inesperados
y facilitando reproducibilidad entre personas y CI. :contentReference[oaicite:2]{index=2}

Los lockfiles también son relevantes
para seguridad de supply chain,
evitando cambios aguas arriba no controlados. :contentReference[oaicite:3]{index=3}

---

### Simplicidad operativa

El flujo `uv sync` + `uv run` reduce fricción diaria:

- menos pasos manuales
- menos ambigüedad sobre el entorno activo
- separación clara entre resolver (`lock`) e instalar (`sync`)

Esto encaja bien con un enfoque reproducible y automatizable. :contentReference[oaicite:4]{index=4}

---

### Ergonomía y adopción

Una herramienta rápida y simple
tiene mayor probabilidad de ser usada consistentemente.

`uv` se posiciona como significativamente más rápido que `pip`
y como reemplazo de múltiples piezas del toolchain tradicional. :contentReference[oaicite:5]{index=5}

> La velocidad no es el objetivo en sí;
> el objetivo es que el estándar se use incluso en días de prisa.

---

## Alternativas consideradas

### `pip` + `venv` + `requirements.txt`

**Pros**

- mínimo común denominador
- ampliamente conocido

**Contras**

- reproducibilidad depende de disciplina manual
- lockfiles no son parte natural del flujo
- fácil desviarse del contrato

`uv` soporta este flujo,
pero recomienda `pyproject.toml` como estándar moderno. :contentReference[oaicite:6]{index=6}

---

### `pip-tools` + `venv`

**Pros**

- lockfile robusto
- conocido en entornos SWE

**Contras**

- más piezas y más fricción
- mayor riesgo de uso inconsistente

---

### Poetry

**Pros**

- enfoque `pyproject.toml`
- lockfile integrado
- herramienta madura

**Contras**

- más convenciones propias
- menos alineado con un flujo “un comando para ejecutar todo”

Poetry documenta su alineación con PEP-517. :contentReference[oaicite:7]{index=7}

---

### Conda (y variantes)

**Pros**

- fuerte en stacks científicos
- útil para binarios complejos (CUDA)

**Contras**

- reproducibilidad más compleja
- sistema paralelo al packaging Python
- más fricción en repos Python-first

---

## Consecuencias

### Positivas

- entornos reproducibles (`uv.lock` + `uv sync`) :contentReference[oaicite:8]{index=8}
- menos “funciona en mi máquina”
- onboarding más rápido
- CI más confiable
- contrato claro sobre dependencias

---

### Negativas / trade-offs

- curva de aprendizaje inicial (acotada)
- herramienta relativamente nueva frente a `pip`/Poetry :contentReference[oaicite:9]{index=9}
- algunos casos edge pueden requerir excepciones documentadas

---

## Guía de adopción

El estándar operativo está documentado en:

👉 `docs/03-entornos-y-dependencias.md`

Reglas prácticas:

- `pyproject.toml` es la fuente de verdad
- `uv.lock` se versiona
- instalar entorno: `uv sync`
- ejecutar comandos: `uv run ...`

La instalación de `uv` se referencia
desde la documentación oficial. :contentReference[oaicite:10]{index=10}

---

## Métricas de éxito

- una persona nueva puede ejecutar el proyecto con `uv sync`
- CI reproduce exactamente las dependencias locales
- disminuyen incidentes por cambios de dependencias
- experimentos antiguos se pueden reproducir sin adivinar versiones

---

## Cuándo revisitar esta decisión

Revisitar si:

- aparecen limitaciones repetidas que impactan al equipo
- el ecosistema converge hacia un estándar distinto
- el costo de excepciones supera el beneficio del estándar

---

## Referencias

- [Contexto del ecosistema `pyproject.toml` y PEP-517](https://peps.python.org/pep-0517/). :contentReference[oaicite:0]{index=0}
- [Documentación oficial de `uv`](https://docs.astral.sh/uv/). :contentReference[oaicite:1]{index=1}
- [Conceptos de proyectos: `uv sync` (entornos reproducibles)](https://docs.astral.sh/uv/concepts/projects/sync/). :contentReference[oaicite:2]{index=2}
- [Por qué los lockfiles ayudan a la seguridad del supply chain](https://www.aikido.dev/blog/why-we-need-lockfiles-to-secure-our-supply-chain). :contentReference[oaicite:3]{index=3}
- [`uv sync` explicado (lock vs sync)](https://docs.astral.sh/uv/concepts/projects/sync/). :contentReference[oaicite:4]{index=4}
- [Overview de `uv` (posicionamiento y velocidad)](https://docs.astral.sh/uv/). :contentReference[oaicite:5]{index=5}
- [`uv` y flujos tipo `requirements.txt` (pip/compile)](https://docs.astral.sh/uv/pip/compile/). :contentReference[oaicite:6]{index=6}
- [Poetry y `pyproject.toml` (alineación con PEP-517)](https://python-poetry.org/docs/pyproject/). :contentReference[oaicite:7]{index=7}
- [`uv sync` (recrear entorno desde lockfile)](https://docs.astral.sh/uv/concepts/projects/sync/). :contentReference[oaicite:8]{index=8}
- [Artículo práctico: `uv` en Python (Real Python)](https://realpython.com/python-uv/). :contentReference[oaicite:9]{index=9}
- [Instalación oficial de `uv`](https://docs.astral.sh/uv/). :contentReference[oaicite:10]{index=10}
