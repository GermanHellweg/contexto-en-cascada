# Etapa 3 — Automation progress y estándares de automatización

Con el plan de pruebas completo, esta etapa decide **qué se automatiza, en qué orden y con qué reglas**. Produce dos artefactos que viven en el repositorio de pruebas y que el agente lee en cada sesión:

| Artefacto | Qué es | Cambia |
|---|---|---|
| **Automation progress** | Los casos del plan que vale la pena automatizar, priorizados, cada uno con su estado. | Todo el tiempo: es el tablero de trabajo. |
| **Estándares de automatización** | Las reglas del suite: arquitectura, convenciones, datos, y el contexto de cada módulo. | Poco: se definen una vez por proyecto y se ajustan. |

---

## Automation progress

### Qué es

El documento que **extrae del plan de pruebas principal** los casos con más valor de automatizar según las necesidades del proyecto. No es una copia del plan: es una selección ordenada, con una decisión explícita por cada caso.

Es el documento con el que interactúa el agente. Cuando se le pide generar specs, no se le dice "automatizá el módulo de usuarios": se le dice "tomá los próximos casos pendientes del progress".

### Por qué es un documento aparte

Porque si no lo es, se automatiza lo fácil. El login se automatiza en todos los proyectos del mundo; el flujo con tres integraciones, donde está el riesgo, queda "para más adelante" siempre.

Separarlo obliga a que **"no automatizamos esto" sea una decisión escrita**, con motivo, revisable.

### Prioridad no es tier

La **prioridad** (P0–P3, del plan de pruebas) mide el impacto del caso. El **tier** mide el **orden de automatización**. Son dimensiones distintas: un caso P0 muy inestable puede diferirse, y un smoke P1 transversal —como el login, que usan todos los demás tests— puede ir primero.

El tier se decide combinando varios factores:

| Factor | Tier 1 | Tier 2 | Tier 3 |
|--------|--------|--------|--------|
| Criticidad para el negocio | Flujo principal | Soporte al flujo | Sin relación directa |
| Impacto | Bloquea la operación | Hay alternativa | Cosmético o aislado |
| Tipo | Smoke + CRUD central | Funcional + validaciones | Bordes, integraciones |
| Frecuencia de uso | Diaria | Regular | Esporádica |
| Alcance del fallo | Varios módulos | Un módulo | Aislado |
| Reutilización | Transversales | De un módulo | Puntuales |
| Estabilidad | Estable | Mayormente estable | En cambio activo |

El progress arranca por un **Tier 1 accionable**: una meta concreta y chica (por ejemplo, todos los smoke más los P0 del flujo principal) que se puede terminar y mostrar.

### Decisiones clave y fases

El progress deja escritas las decisiones que condicionan todo el suite —qué módulos quedan fuera por estar en refactor, cómo se manejan los datos, qué no se automatiza nunca— y organiza el trabajo en **fases** por bloque: primero infraestructura y acceso transversal, después el núcleo del negocio, después el resto operativo, la administración y, al final, las integraciones. Dentro de cada fase, primero los smoke y después los funcionales, en orden de navegación.

Si el proyecto se frena a la mitad —que es lo que suele pasar— lo que quedó hecho es lo que valía la pena.

### Criterios de exclusión

Un caso queda fuera de automatización cuando depende de percepción visual, el comportamiento cambia todas las semanas, la precondición es imposible de reproducir, involucra una acción sensible que se prefiere validar a mano, o automatizarlo cuesta más que ejecutarlo a mano en su frecuencia real. Queda listado con su motivo en "Fuera de automatización".

### Estados

| Estado | Significa |
|---|---|
| 📋 pendiente | Seleccionado, sin spec todavía. |
| 🔄 en progreso | Con spec en curso. El detalle indica el punto: `draft`, `fixme` (espera la corrección de un bug), en revisión. |
| ✅ automatizado | En el suite, pasando, con su ID del TMT. |

### Más que una lista de casos

El progress también lleva lo que rodea al trabajo: **pre-requisitos** de infraestructura, **estrategia de datos** acordada con desarrollo, **bloqueantes** resueltos y abiertos con fecha, **riesgos**, y **próximas acciones**. Es lo primero que lee el agente al retomar, y lo primero que debería leer una persona que se suma al proyecto.

### Lo que responde

Con los estados al día, el progress responde lo que cualquier responsable pregunta: qué está automatizado, qué falta, qué no se va a automatizar y por qué. Es la respuesta, con evidencia, al "automatizá todo".

---

## Estándares de automatización

### Qué son

Las decisiones del suite que se toman **una vez** y que el agente respeta en **cada** spec que genera. Se definen según el proyecto: no hay un estándar universal, hay un estándar escrito.

### Qué definen, como mínimo

1. **Estructura de archivos:** espejo del menú, un spec por módulo.
2. **Estrategia de datos:** seeds, fixtures, factories o creación por API.
3. **Autenticación por rol:** cómo se obtiene una sesión sin repetir el login en cada test.
4. **Localizadores:** estrategia (rol, label, test-id) y cuándo usar cada una.
5. **Trazabilidad:** el `TC-ID` en el título de cada test y el ID del TMT en una anotación que el reporter entiende.
6. **Comandos de ejecución:** scripts cortos y consistentes (con y sin publicación al TMT, modo interactivo, reporte).
7. **Ambiente y variables:** dónde corre la suite y cómo se configuran las credenciales, siempre fuera del repositorio.
8. **Convenciones de código:** localizadores, esperas, aserciones, aislamiento, uso de `fixme`.

### El contexto por módulo

Los estándares generales se completan con un **archivo de contexto por módulo**: todo lo específico de ese módulo que el agente necesita y que no está en ningún otro lado. Rutas, roles, selectores establecidos, helpers existentes para reutilizar, datos de prueba y, sobre todo, **trampas conocidas**.

Esa última sección es la que más rinde: acumula lo aprendido a los golpes y evita que el agente repita el mismo error en cada lote. Se actualiza después de cada generación.

---

## Plantillas

- `plantillas/automation-progress.md`
- `plantillas/estandares-de-automatizacion.md`
- `plantillas/contexto-de-modulo.md`

## Ventajas de esta capa

- **Se automatiza lo que importa, no lo fácil.** Los criterios ponen el riesgo por delante de la comodidad técnica.
- **"No automatizamos esto" es una decisión, no un olvido.**
- **Progreso visible.** El estado del suite se lee en un documento, no se reconstruye preguntando.
- **Consistencia.** Los estándares hacen que el spec número 200 siga las mismas reglas que el primero.
- **Menos errores del agente.** El contexto por módulo le da lo que la documentación general no tiene.
