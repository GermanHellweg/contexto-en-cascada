# Etapa 1 — Documento maestro de comportamientos

Esta etapa tiene **dos pasos** y dos productos distintos:

```
Módulo A ──► Documento modular A ──┐
Módulo B ──► Documento modular B ──┼──► Documento maestro de comportamientos
Módulo C ──► Documento modular C ──┘        (fuente de verdad de la aplicación)
                  │
                  └──► Hallazgos: bugs, ambigüedades, inconsistencias
                       (entregables desde el primer día)
```

| | Documento modular | Documento maestro |
|---|---|---|
| **Alcance** | Un módulo | Toda la aplicación |
| **Naturaleza** | Documento de trabajo | Fuente de verdad |
| **Contiene** | Comportamientos relevados + hallazgos abiertos y resueltos | Solo comportamientos aclarados |
| **Admite dudas** | Sí, es donde se registran | No |
| **Lo consume** | El equipo, para resolver hallazgos | El plan de pruebas y los agentes |

---

## Paso 1 — Documentos modulares

### Qué son

El relevamiento de **un módulo a la vez**: qué hace el sistema hoy, observado y verificado, más todo lo que se encontró en el camino que no cierra.

### Por qué módulo por módulo

- **El alcance es manejable.** Un módulo se releva en días; un sistema entero, en meses que nadie va a autorizar.
- **El valor aparece enseguida.** El primer módulo documentado ya produce hallazgos.
- **El agente trabaja mejor con contexto acotado.** Pedirle que releve "la aplicación" produce generalidades; pedirle que releve "el módulo de reservas" produce reglas concretas.
- **Se puede paralelizar** entre personas del equipo.
- **Se puede priorizar.** Arrancar por el módulo que más duele.

### Qué contiene

Cada documento modular recorre el módulo por aspectos, siempre en el mismo orden. Esa estructura fija es la que después permite derivar el plan de pruebas de forma sistemática: **cada sección alimenta una categoría de casos**.

| Sección | Qué describe | Alimenta la categoría |
|---|---|---|
| Código de módulo | Identificador corto y estable (`VEN-PED`) | Los IDs de los casos (`TC-VEN-PED-001`) |
| Descripción funcional | Qué hace, para quién, ruta base, ubicación en el menú | Contexto del plan |
| Usuarios, permisos y planes | Quién puede hacer qué, en frontend y backend | Permisos y roles |
| Pantallas y vistas | Cada pantalla en orden del frontend: filtros, columnas, textos literales, modales | Listado y búsqueda · Formularios |
| Acciones disponibles | Qué hace cada botón o acción, por pantalla | CRUD · Acciones de fila · Errores |
| Validaciones de negocio | Reglas de entrada, en frontend y backend, con mensajes literales | Validación de datos |
| Estados y transiciones | Estados, cómo se muestran, qué transiciones son válidas | Estados y transiciones |
| Endpoints relacionados | Cada endpoint con su fuente | Casos de API y permisos |
| Integraciones | Servicios externos, colas, procesos programados | Integraciones |
| Reglas de negocio adicionales | Cálculos, significado de montos, comportamientos sin pantalla | Reglas de negocio |
| Notas o ambigüedades | Los hallazgos | Nada: vuelven al equipo |

Plantilla completa, con instrucciones y ejemplos por sección, en `plantillas/documento-modular.md`.

### En qué orden: el de la UI

Los módulos, y los comportamientos dentro de cada módulo, se documentan **en el mismo orden en que aparecen en la interfaz**: el orden del menú, y dentro de cada pantalla, de arriba hacia abajo y de izquierda a derecha.

- **Trazabilidad natural.** Cualquiera encuentra un comportamiento sabiendo dónde está en pantalla, y cualquiera ubica en pantalla un comportamiento leyendo el documento.
- **Nada queda afuera.** Recorrer la UI en orden es un checklist implícito.
- **Todo lo demás hereda el orden.** El plan de pruebas, el automation progress y las carpetas del suite siguen la misma secuencia.

Lo que no tiene pantalla —procesos automáticos, tareas programadas, integraciones, webhooks— va en una sección propia de **comportamientos sin interfaz**, al final del módulo al que pertenecen.

### Cómo se redacta, y el riesgo de cada vía

| Vía | Cómo | Riesgo | Cómo mitigarlo |
|---|---|---|---|
| **A mano** | Recorriendo la aplicación, leyendo tickets y especificaciones, preguntando al equipo. | Se documenta lo que uno *cree* que hace el sistema, no lo que hace. Se saltean las reglas negativas. Queda incompleto. | Más precaución: contrastar cada comportamiento con la aplicación y hacer revisar por otra persona. |
| **A mano, refinado con IA** | Se redacta un borrador y se le pide a un agente que lo ordene y complete el formato. | Al "refinar", el agente rellena huecos con supuestos plausibles: exactamente el problema que el método busca evitar. | Pedirle que mejore **la forma** y que marque lo que falta **como hallazgo**, sin agregar reglas propias. |
| **Analizando el código (recomendado)** | Un agente de código con acceso al repositorio releva el módulo. | El código describe lo que el sistema hace, que no siempre es lo que debería hacer. | Verificar contra la aplicación y registrar cada diferencia como hallazgo. |

La vía recomendada es la del código porque el agente lee las reglas donde realmente viven:

- validaciones de formularios y requests;
- políticas y permisos por rol;
- transiciones de estado en los modelos;
- migraciones (restricciones, valores por defecto, campos obligatorios);
- textos de la interfaz y mensajes de error.

El agente describe lo que el código hace. La persona contrasta eso con lo que el sistema *debería* hacer. **La diferencia entre ambas cosas es exactamente donde están los hallazgos.**

### Hallazgos: el entregable desde el momento uno

Documentar un módulo con rigor encuentra problemas. Siempre. Es el efecto más subestimado del método: **antes de escribir un solo test, ya hay entregables.**

Se registran en la última sección del documento modular, **Notas o ambigüedades**, cada uno con una etiqueta:

| Etiqueta | Significa | Ejemplo |
|---|---|---|
| `[BUG]` | El sistema hace algo que no debería. Se indica el objetivo. | Un endpoint de anulación no valida el rol: cualquiera puede anular por API. |
| `[PENDIENTE fecha]` | Ambigüedad sin definir, consultada con alguien. | ¿Un pedido facturado puede anularse si se anula la factura? |
| `[VERIFICAR]` | El código sugiere algo que hay que confirmar en la aplicación. | El formulario acepta decimales pero el backend exige enteros. |
| `[COMPLETAR]` | Información que falta y hay que pedir al equipo. | Política de reintentos del correo de confirmación. |
| `[RESUELTO]` | Aclarado, con la decisión tomada. | La pantalla de detalle ahora exige permiso de lectura. |
| `[RESUELTO — intencional]` | Parecía un bug y es el comportamiento esperado. | El listado muestra los totales sin impuestos a propósito. |

Las notas resueltas **no se borran**: explican por qué el sistema es como es y evitan rediscutirlo. Los `[BUG]` se reportan en el gestor de incidencias en el momento, sin esperar a tener tests.

### Criterio de "terminado"

El documento modular está listo para pasar al maestro cuando:

1. Todos los comportamientos del módulo están relevados.
2. No quedan notas `[PENDIENTE]`, `[VERIFICAR]` o `[COMPLETAR]` que cambien el comportamiento esperado (los `[BUG]` pueden seguir abiertos en el gestor de incidencias: lo que tiene que estar claro es el comportamiento **esperado**).
3. Alguien que no trabajó en el módulo puede responder, solo leyéndolo, qué pasa en los escenarios de borde más obvios.

---

## Paso 2 — Documento maestro de comportamientos

### Qué es

La consolidación de los documentos modulares en **un único documento de la aplicación**, que describe qué hace el sistema. Es la fuente de verdad del producto y la entrada de todas las capas siguientes.

### La regla que lo define

> **Al documento maestro solo entra lo aclarado.**

Si un módulo tiene notas abiertas que cambian su comportamiento, se queda en el documento modular hasta resolverse. Un documento maestro con dudas deja de ser fuente de verdad, y un agente que lo lee no distingue una regla firme de una suposición.

Cuando una nota es un `[BUG]`, el maestro documenta el **comportamiento esperado**, no el defectuoso. Los tests derivados de ese comportamiento van a fallar hasta que el bug se corrija — y eso es correcto: es el suite haciendo su trabajo.

### Estructura

1. **Encabezado** con título, versión, fecha y estado.
2. **Introducción:** qué es la aplicación, qué es este documento y qué no incluye (los hallazgos).
3. **Glosario** de términos del dominio y técnicos.
4. **Índice de módulos**, en el orden de navegación.
5. **Bloques y grupos**, siguiendo el menú: por ejemplo Bloque 0 (transversal / acceso), Bloque A (aplicación operativa), Bloque B (administración), y dentro de cada uno los grupos del menú.
6. **Un apartado por módulo**, con las mismas secciones que su documento modular **menos "Notas o ambigüedades"**.

Plantilla en `plantillas/documento-maestro.md`.

### Reglas de escritura

- **Presente indicativo, observable.** Qué pasa, no qué debería pasar ni cómo está implementado.
- **Sin lenguaje de testing.** Nada de "verificar que".
- **Códigos de módulo permanentes**, heredados del documento modular: son los que aparecen en los IDs de los casos.
- **Textos de la interfaz literales**, entre comillas: son los que después usan los tests.
- **Las reglas negativas son tan importantes como las positivas.**

---

## Ventajas de esta capa

- **Delivery desde el primer día.** Bugs, ambigüedades e inconsistencias reportados antes de automatizar nada. Es el argumento más fuerte para conseguir el tiempo que el método necesita.
- **Alinea a QA con producto y desarrollo.** Los hallazgos obligan a tomar decisiones que estaban implícitas.
- **Una sola fuente de verdad.** Se terminan las discusiones sobre qué debería hacer el sistema.
- **Onboarding.** Una persona nueva entiende el producto leyendo un documento.
- **Análisis de impacto.** Ante un cambio, se ve qué comportamientos toca y qué pruebas se derivan de ellos.
- **Contexto consumible por agentes.** Todas las capas siguientes se apoyan en este documento.
