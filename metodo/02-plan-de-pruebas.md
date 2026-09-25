# Etapa 2 — Plan de pruebas

## Qué es

Todos los casos de prueba de la aplicación, derivados del documento maestro, **módulo por módulo** y en el mismo orden de navegación. Al terminar esta etapa hay dos documentos en Markdown: el maestro y el plan.

## Por qué ahora es sistemático

Con el documento maestro completo, generar casos deja de ser un ejercicio de imaginación. Cada módulo tiene las mismas secciones, y **cada sección alimenta una categoría de casos** (ver `05-categorias-y-analisis-de-fallo.md`):

```
Documento maestro — módulo VEN-PED          Plan de pruebas — módulo VEN-PED
───────────────────────────────────         ──────────────────────────────────
Usuarios, permisos y planes      ────────►  Permisos y roles
Pantallas y vistas               ────────►  Listado y búsqueda · Formularios
Acciones disponibles             ────────►  Operaciones CRUD · Acciones de fila
Validaciones de negocio          ────────►  Validación de datos
Estados y transiciones           ────────►  Estados y transiciones
Integraciones                    ────────►  Integraciones
Reglas de negocio adicionales    ────────►  Reglas de negocio
```

Esa correspondencia es la trazabilidad: un caso de la categoría "Validación de datos" del módulo `VEN-PED` sale de la sección "Validaciones de negocio" de ese módulo. Si cambia esa sección, se sabe exactamente qué casos revisar.

El agente es muy bueno en esta expansión mecánica. La persona es la que decide qué casos importan y cuáles son ruido.

## Por qué módulo por módulo

- **Se revisa de verdad.** Un módulo de 40 casos se lee; un plan de 1.000 generado de una vez se aprueba sin leer.
- **Los errores del agente se detectan temprano**, antes de multiplicarse por todos los módulos.
- **Se sigue el mismo orden** que el documento maestro y la aplicación.

## Casos: comportamiento esperado, nunca el bug

Cada caso describe lo que el sistema **debe** hacer. Si el documento modular registró un `[BUG]`, el caso valida el comportamiento correcto: va a fallar hasta que se corrija, y eso es exactamente lo esperado. Un caso que "valida el bug" convierte al suite en cómplice del defecto.

## Qué lleva cada caso

Los campos están pensados para importarse directamente a un TMT como QASE:

```markdown
###### TC-VEN-PED-006: Restringir la anulación a Administrador
- **Description:** Valida que un usuario Vendedor no pueda anular un pedido
  ni desde la interfaz ni llamando al endpoint directamente.
- **Priority:** high
- **Severity:** critical
- **Type:** functional
- **Behavior:** negative
- **Automation:** is-not-automated
- **Tags:** module:ven-ped, category:permissions, type:functional, priority:P0, bloque:operativo
```

| Campo | Qué mide | Valores |
|---|---|---|
| **ID** | Identificador del caso | `TC-[MÓDULO]-NNN` |
| **Description** | Qué valida, en una o dos oraciones | "Valida que…" |
| **Priority** | Qué se prueba primero | `high` · `medium` · `low` (y `P0`–`P3` en los tags) |
| **Severity** | Cuánto daño hace si falla | `blocker` · `critical` · `major` · `normal` · `minor` · `trivial` |
| **Type** | Para qué ejecución sirve | `smoke` · `functional` · `regression` · `integration` |
| **Behavior** | Qué escenario plantea | `positive` · `negative` · `destructive` |
| **Automation** | Estado en el TMT | `is-not-automated` · `automated` · `to-be-automated` |
| **Tags** | Filtros para el TMT y los reportes | `module:` · `category:` · `type:` · `priority:` · `bloque:` |

## Cuatro clasificaciones, cuatro preguntas

Conviene no mezclarlas, porque cada una responde algo distinto:

| Clasificación | Pregunta | Ejemplo |
|---|---|---|
| **Categoría** | ¿De qué sección del documento sale? | Permisos y roles |
| **Prioridad** | ¿Qué se prueba primero? | P0: entra en cada ejecución |
| **Severidad** | ¿Cuánto daño hace si falla? | Crítica: seguridad, datos o dinero |
| **Tipo y behavior** | ¿Para qué ejecución sirve y qué escenario plantea? | functional · negative |

El **impacto** —a cuánta gente o a qué proceso afecta un fallo— no es un campo propio: se combina con la severidad y la frecuencia de uso para decidir la prioridad. Muchos equipos asocian "severidad" solo a defectos; acá se asigna también al caso, como estimación del daño si el comportamiento que verifica se rompe. QASE, por ejemplo, tiene ese campo en el caso.

## Resumen global

El plan abre con tablas de conteo por prioridad, tipo, behavior y módulo. No son decorativas: son la forma más rápida de detectar un plan desbalanceado. Un módulo crítico sin casos negativos, o un plan con 90% de prioridad alta, se ve ahí antes que en ningún otro lado.

## Revisión humana obligatoria

Si esta revisión se saltea, el error se propaga a la automatización y aparece semanas después como un suite que verifica cosas que a nadie le importan.

1. **¿Hay duplicados con distinta redacción?** El error más común de la generación asistida.
2. **¿Las prioridades reflejan riesgo real o vinieron por defecto?**
3. **¿Hay categorías vacías sin explicación en módulos críticos?**
4. **¿Algún caso valida un bug en lugar del comportamiento esperado?**
5. **¿Hay casos imposibles de ejecutar** porque dependen de datos que no existen?

## Dónde vive el plan: el repositorio o un TMT

El plan puede quedarse como Markdown en el repositorio, o exportarse a un test management tool (TMT) como QASE, TestRail o Xray. La recomendación es el TMT: da historial de ejecuciones, reportes y un lugar compartido con el resto del equipo.

El TMT le asigna a cada caso **su propio ID**, distinto del `TC-ID`. Ese ID tiene que volver al plan y, después, a cada spec: es lo que permite que los resultados de la automatización lleguen solos al TMT. Hay dos formas de traerlo —por MCP, o exportando y pidiéndole al agente que matchee— descritas en `guias/tmt-e-ids.md`.

## Ventajas de esta etapa

- **Derivación sistemática.** Las secciones del documento y las categorías convierten "pensar casos" en un recorrido.
- **Cobertura visible.** Las tablas de resumen muestran el balance del plan de un vistazo.
- **Trazabilidad.** Cada caso sabe de qué módulo y de qué sección viene.
- **Listo para el TMT.** Los campos se importan sin reescribir nada.
- **Detecta huecos en el documento maestro.** Derivar casos obliga a leerlo con lupa; lo que falta aparece y vuelve al documento modular como nota.
