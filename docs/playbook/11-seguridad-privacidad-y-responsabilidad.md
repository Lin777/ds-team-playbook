# 11 · Seguridad, privacidad y responsabilidad

> La mayoría de los incidentes de seguridad  
> no ocurren por mala intención.  
> Ocurren por descuido, presión o falta de acuerdos claros.

Este capítulo define cómo abordamos
**seguridad y privacidad en proyectos de Data Science**,
desde una perspectiva práctica y responsable.

No busca convertir al equipo en expertos legales.
Busca **reducir riesgos reales** y proteger a:

- usuarios
- empresa
- equipo

---

## El problema que estamos tratando de evitar

Sin criterios claros de seguridad y privacidad, suelen aparecer:

- datos sensibles en notebooks
- credenciales expuestas “temporalmente”
- modelos entrenados con datos mal entendidos
- outputs que revelan información no deseada
- incidentes que nadie sabe cómo manejar

El problema rara vez es falta de ética.
Es **falta de estructura y conciencia compartida**.

---

## Seguridad y privacidad en Data Science

En DS, el riesgo no está solo en el código.
También está en:

- los datos
- los modelos
- los outputs
- las decisiones automatizadas

Por eso, la seguridad no es un “add-on”.
Es parte del diseño.

---

## 🔒 MUST · Principios básicos

Estas reglas aplican **siempre**,
independientemente del proyecto.

---

### Protección de credenciales

- Las keys **no viven en el código**
- No se suben al repo
- No se comparten por chat
- No se hardcodean “solo para probar”

Usamos:

- variables de entorno
- `.env` local
- secrets gestionados por la plataforma

Un leak de credenciales no es un bug menor.
Es un **incidente de seguridad**.

---

### Manejo responsable de datos

Antes de usar un dataset,
debemos poder responder:

- ¿de dónde viene?
- ¿qué tipo de datos contiene?
- ¿hay datos sensibles o personales?
- ¿tenemos permiso para usarlos?

Si no podemos responder estas preguntas,
el riesgo es alto.

---

### Principio de mínimo acceso

Las personas y sistemas:

- solo acceden a lo que necesitan
- solo por el tiempo necesario

Más acceso ≠ más productividad.  
Más acceso = más superficie de riesgo.

---

## 🔒 MUST · Privacidad por diseño

La privacidad no se “agrega” al final.
Se diseña desde el inicio.

Esto implica:

- evitar usar datos sensibles si no es necesario
- anonimizar cuando sea posible
- minimizar almacenamiento de datos crudos
- pensar en los outputs, no solo en los inputs

---

??? example "Ejemplo · Riesgo en outputs"
    Un modelo puede no exponer datos directamente,
    pero sí permitir inferencias indebidas.

    Ejemplos:
    - revelar información personal indirectamente
    - permitir reidentificación
    - memorizar datos sensibles

    El riesgo no siempre es obvio.

---

## 🌱 DESEABLE · Prácticas que reducen riesgo

Estas prácticas no siempre son obligatorias,
pero **bajan mucho el riesgo operativo**.

---

### Revisión de datos y modelos

Antes de mover algo a producción:

- revisar datasets
- revisar features
- revisar outputs esperados

No es paranoia.
Es **responsabilidad profesional**.

---

### Logging consciente

Logs son poderosos,
pero también peligrosos.

Buenas prácticas:

- no loggear datos sensibles
- no loggear payloads completos sin necesidad
- revisar logs como parte del diseño

---

### Documentar decisiones sensibles

Si una decisión tiene impacto en:

- privacidad
- seguridad
- usuarios finales

Debe quedar documentada:

- qué se decidió
- por qué
- qué riesgos se aceptaron

Los ADRs son el lugar natural para esto.

---

## Responsabilidad en modelos y decisiones

Los modelos no son neutrales.
Reflejan:

- los datos
- los supuestos
- las decisiones humanas

Por eso, responsabilidad significa:

- entender limitaciones
- comunicar incertidumbre
- evitar sobreprometer resultados

---

## 🔒 MUST · Ownership y accountability

Todo sistema que impacta usuarios
debe tener:

- un owner técnico
- un punto claro de contacto

Cuando algo falla,
la pregunta no es “¿quién tuvo la culpa?”,
sino:
> “¿quién puede ayudar a resolverlo?”

---

## Antipatrones (señales de alerta)

- “esto es solo para testing”
- datasets sin origen claro
- logs con datos sensibles
- nadie sabe quién es el owner
- asumir que seguridad es problema de otro equipo

Estos patrones suelen preceder incidentes reales.

---

## Relación con el resto del playbook

- [`03 · Entornos`](03-entornos-y-dependencias.md) → evita leaks accidentales
- [`05 · Estándares`](05-estandares-de-codigo.md) → código responsable
- [`09 · Lifecycle`](09-ml-lifecycle-y-produccion.md) → impacto en producción
- [`10 · Rituales`](10-rituales-feedback-y-crecimiento.md) → conversaciones difíciles a tiempo

La seguridad no vive en un solo capítulo.
Pero este capítulo la **hace explícita**.

---

## Cierre

La seguridad perfecta no existe.
La irresponsabilidad sí.

Tomar decisiones conscientes,
documentarlas
y reducir riesgos innecesarios
es parte del trabajo profesional en Data Science.

No es un freno.
Es una forma de cuidar lo que construimos.
