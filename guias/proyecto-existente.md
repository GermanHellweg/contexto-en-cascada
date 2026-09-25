# Adaptar el método a un proyecto que ya está andando

El caso normal no es un proyecto nuevo. Es un producto con años encima, un suite de tests heredado de tres personas distintas y cero documentación de comportamientos. El método funciona igual, pero el orden de entrada cambia.

## No empieces por el documento maestro completo

Escribir la documentación de todo el producto antes de tocar nada es la forma más confiable de abandonar el método en la semana tres. Nadie va a autorizar ese tiempo y vos no vas a tener paciencia.

## Empezá por donde duele

Elegí **un solo módulo** con estas características:

- Se rompe seguido o genera bugs en producción.
- Va a seguir existiendo (no está por ser reescrito).
- Es lo bastante chico como para documentarlo en dos o tres días.

Ese módulo es la prueba piloto. Si el método funciona ahí, el argumento para extenderlo lo da el resultado, no la teoría.

## Secuencia sugerida

**Semana 1 — Documentar lo que hay.** El documento modular del módulo elegido, idealmente con un agente de código que lea el repositorio (`prompts/01-relevar-modulo.md`). Fuentes adicionales: tickets viejos, la aplicación corriendo, los tests existentes y la gente del equipo. Los hallazgos van a ser muchos: eso está bien, son el primer valor entregado, y en un proyecto viejo suelen incluir bugs que nadie había visto.

**Semana 2 — Plan de pruebas y auditoría de lo existente.** Derivar los casos y después mapear los tests que ya existen contra ellos. Vas a encontrar tres cosas, todas útiles:

| Hallazgo | Qué hacer |
|---|---|
| Tests que no corresponden a ningún caso | Evaluar: ¿cubren algo real que falta documentar, o son ruido histórico? |
| Casos sin ningún test | La lista de trabajo priorizada. |
| Tests duplicados que verifican lo mismo | Candidatos a eliminar. Reducir el suite también es progreso. |

**Semana 3 — Contexto del módulo y primer lote generado.** Armar el archivo de contexto extrayendo las convenciones que el suite ya usa, y generar el primer lote de 5 a 10 specs sobre los huecos detectados.

**Semana 4 — Medir y decidir.** Comparar el esfuerzo del módulo piloto contra la forma habitual de trabajo, y decidir si se extiende.

## Qué hacer con el suite heredado

No lo reescribas. El método no requiere que todo el código siga sus convenciones: requiere que **lo nuevo** las siga y que exista trazabilidad hacia adelante. Los tests viejos se van migrando cuando se tocan por otro motivo.

## El error más común

Documentar lo que el sistema *debería* hacer en vez de lo que *hace*. En un producto viejo la diferencia es grande y llena de decisiones deliberadas que nadie recuerda. Documentá el comportamiento real en el documento modular y registrá las rarezas como hallazgos. Si después resulta que era un bug de hace cuatro años, mejor todavía: lo encontraste documentando.
