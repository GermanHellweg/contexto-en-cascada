# Premisas del método

Cinco afirmaciones sobre las que se apoya todo lo demás. Si no estás de acuerdo con estas, el método no te va a servir.

## 1. Un agente es tan bueno como el contexto que recibe

No es una limitación temporal de los modelos. Es estructural: un agente no puede inferir reglas de negocio que no están escritas en ninguna parte. Cuando no las encuentra, las inventa de forma plausible — que es el peor resultado posible, porque el output parece correcto.

**Consecuencia práctica:** la inversión no va en el prompt. Va en el documento que el prompt referencia.

## 2. Documentar comportamientos entrega valor antes de escribir un test

Relevar un módulo con rigor encuentra bugs, ambigüedades e inconsistencias. Siempre. Esos hallazgos son entregables desde el primer día, mucho antes de que exista automatización.

Y el mismo documento sigue rindiendo: sirve para derivar el plan de pruebas, para onboarding, para resolver discusiones con producto, para estimar el impacto de un cambio y para generar automatización. Un artefacto, muchos usos. La documentación que solo sirve para "estar documentado" muere; esta no, porque es insumo de un proceso.

## 3. "Qué probar" y "cómo automatizar" son decisiones distintas

Mezclarlas lleva al error más común de la automatización: automatizar lo que es fácil de automatizar en lugar de lo que importa. Separar las capas 2 y 3 obliga a declarar explícitamente por qué un caso queda fuera del suite automatizado, y eso es una decisión de riesgo, no de conveniencia técnica.

## 4. La trazabilidad no es burocracia: es lo que permite analizar

Un spec ligado al caso que lo origina, y ese caso ligado al comportamiento documentado, permite responder preguntas que de otro modo no tienen respuesta: qué cobertura real tiene este módulo, qué se rompe si cambia esta regla, cuántos fallos vienen del producto y cuántos del suite.

Sin trazabilidad tenés un log de tests rojos. Con trazabilidad tenés información.

## 5. El fallo de un test es un dato sobre el sistema entero, no solo sobre el código

Un test rojo puede significar un bug, un problema del test, un dato malo, un ambiente caído — o que el documento maestro está desactualizado. Ese último caso es el que cierra el ciclo: el análisis de fallo retroalimenta la capa 1 y mantiene la documentación viva. Ver `05-categorias-y-analisis-de-fallo.md`.

---

## Qué NO resuelve este método

Vale decirlo antes que alguien lo descubra por su cuenta:

- **No reemplaza el criterio de QA.** Acelera la producción, no la decisión de qué es riesgoso.
- **No sirve para exploratorio.** El testing exploratorio vive de lo no documentado; este método vive de lo documentado. Son complementarios, no sustitutos.
- **Tiene un costo de arranque real.** El primer módulo se siente lento. La ganancia aparece del segundo o tercero en adelante.
- **Requiere disciplina de mantenimiento.** Un documento maestro desactualizado es peor que no tenerlo, porque el agente lo va a creer.
- **No arregla un producto sin definición.** Si nadie en el equipo sabe qué debería hacer el sistema, el método expone ese problema pero no lo resuelve.
