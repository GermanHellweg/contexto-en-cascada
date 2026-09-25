# Categorías y análisis de fallo

Este es el documento que separa este método de "pedirle casos a la IA". Las dos taxonomías cumplen funciones distintas: una organiza la **generación** de casos, la otra organiza el **diagnóstico** cuando un test falla.

---

## Parte A — Categorías de casos

Sirven para expandir un módulo en casos de forma sistemática, en vez de depender de la inspiración. **Cada categoría sale de una sección del documento maestro**: esa es la trazabilidad entre la documentación y el plan.

| Categoría | Tag | Sale de la sección | Qué verifica |
|---|---|---|---|
| **Operaciones CRUD** | `category:crud` | Acciones disponibles | Crear, ver, editar y eliminar las entidades del módulo. |
| **Validación de datos** | `category:validation` | Validaciones de negocio | Obligatorios, formatos, límites, listas blancas, mensajes de error; en el frontend y en el backend. |
| **Listado y búsqueda** | `category:listing` | Pantallas y vistas | Columnas, búsqueda, filtros, orden, paginación, estado vacío. |
| **Acciones de fila** | `category:row-actions` | Acciones disponibles | Lo que se hace sobre un registro puntual del listado (ver, editar, descargar, anular). |
| **Comportamiento de formularios** | `category:forms` | Pantallas y vistas · Validaciones | Habilitado de botones, valores por defecto, dependencias entre campos, cancelar sin guardar. |
| **Estados y transiciones** | `category:states` | Estados y transiciones | Transiciones válidas, transiciones rechazadas, cómo se muestra cada estado. |
| **Permisos y roles** | `category:permissions` | Usuarios, permisos y planes | Cada rol ve y hace solo lo suyo; en la interfaz **y** en el endpoint. |
| **Reglas de negocio** | `category:business` | Reglas de negocio adicionales | Cálculos, montos, redondeos, efectos secundarios de una acción. |
| **Integraciones** | `category:integration` | Integraciones | Servicios externos, colas, correo, archivos; y qué pasa cuando fallan. |
| **Manejo de errores** | `category:errors` | Acciones · Integraciones | Mensajes ante fallos del servidor, de red o de un servicio externo. |

**Cómo se usa.** Al derivar el plan, se recorre cada sección del documento del módulo y se generan los casos de su categoría. Así, si una sección tiene contenido y su categoría no tiene casos, hay un hueco a la vista.

**Regla de revisión.** Una categoría puede quedar vacía, pero con una línea que explique por qué ("No aplica: el módulo no tiene formularios"). Una categoría vacía sin explicación es un hueco sin revisar. La pregunta de la revisión es: *¿por qué este módulo crítico no tiene casos de permisos?*

> Estas categorías clasifican **de dónde sale** el caso. La **importancia** (prioridad y severidad) y el **tipo** (smoke, functional, regression, integration) y el **behavior** (positive, negative, destructive) son otras clasificaciones, descritas en `02-plan-de-pruebas.md`.

---

## Parte B — Análisis de fallo

Cuando un test falla, la pregunta no es "¿lo arreglo?" sino "¿qué me está diciendo?". Clasificar el fallo antes de tocar código convierte un suite rojo en información sobre el proyecto.

| Categoría de fallo | Qué significa | Acción | A dónde vuelve |
|---|---|---|---|
| **Bug del producto** | El sistema no cumple el comportamiento documentado. | Reportar. | El test queda como está: está haciendo su trabajo. |
| **Defecto del test** | El spec está mal: selector incorrecto, aserción equivocada, precondición mal montada. | Corregir el spec. | Al contexto de módulo, si el error es repetible. |
| **Dato de prueba** | El dato que el test necesita no existe, cambió o fue consumido. | Corregir la estrategia de datos. | A los estándares de automatización. |
| **Ambiente / infraestructura** | Servicio caído, deploy a medias, red. | No es un fallo de QA. | Al equipo de plataforma. |
| **Inestabilidad (flaky)** | Pasa a veces. Timing, carrera, dependencia de orden. | Estabilizar o quitar del suite. | A los estándares y al automation progress. |
| **Cambio de especificación** | El producto cambió y el test refleja el comportamiento viejo. | Actualizar el documento modular y el maestro, después el caso y el spec, en ese orden. | **Al documento maestro.** |
| **Brecha de documentación** | El sistema hace algo razonable que el documento maestro no contempla. | Documentarlo en el documento modular (como nota) y consolidarlo. | **Al documento maestro.** |

### Por qué esto cierra el ciclo

Las dos últimas categorías son la razón de ser de la tabla. En un flujo sin trazabilidad, un cambio de especificación se resuelve arreglando el test y la documentación queda vieja en silencio. Con el ciclo cerrado, cada fallo de ese tipo es una señal que mantiene vivo el documento maestro — que es justamente el activo del que depende todo el método.

Un suite que solo produce bugs y flakies indica documentación estancada. Un suite que produce un flujo constante de brechas y cambios de spec indica documentación viva.

### Qué medir

- **Distribución de fallos por categoría, en el tiempo.** Es el indicador de salud del suite. Si "defecto del test" e "inestabilidad" dominan, el problema es la automatización, no el producto.
- **Tiempo entre el fallo y su clasificación.** Si nadie clasifica, el suite rojo se normaliza y el método se cae.
- **Fallos que retroalimentaron el documento maestro.** La métrica de que la documentación sigue viva.
