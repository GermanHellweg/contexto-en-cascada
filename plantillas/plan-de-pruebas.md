---
titulo: Plan de Pruebas — [Aplicación]
version: 1.0
fecha: AAAA-MM-DD
estado: vigente
tipo: plan-de-pruebas-funcional
---

# Plan de Pruebas — [Aplicación]

<!--
PLANTILLA — Plan de pruebas (etapa 2 del método Contexto en Cascada).

Qué es: todos los casos de prueba de la aplicación, generados MÓDULO POR
MÓDULO a partir del documento maestro, en el mismo orden de navegación.

Reglas:
- Cada caso describe el COMPORTAMIENTO ESPERADO, nunca un bug. Si hay un bug
  conocido, el caso valida el comportamiento correcto: va a fallar hasta que
  se corrija, y eso es lo esperado.
- Cada módulo se divide en las diez categorías (ver metodo/05). Cada categoría
  sale de una sección del documento maestro: esa es la trazabilidad.
- Los campos de cada caso están pensados para importarse a un TMT (QASE u otro).
- Se revisa a mano antes de exportar: duplicados, prioridades por defecto,
  categorías vacías en módulos críticos.
-->

## Introducción

<!-- Qué contiene el plan, de dónde sale (enlace al documento maestro),
     cómo está organizado y en qué formato están los casos. -->

Este documento consolida en un único plan de pruebas funcional todos los casos de **[Aplicación]**, generados módulo por módulo a partir del [documento maestro](./documento-maestro.md). Cada caso describe el **comportamiento esperado** del sistema: los bugs conocidos se expresan como el caso correcto que debe cumplirse una vez corregidos.

**Organización.** Los módulos siguen el orden de navegación de la aplicación. Dentro de cada módulo, los casos se agrupan por categoría: Operaciones CRUD, Validación de datos, Listado y búsqueda, Acciones de fila, Comportamiento de formularios, Estados y transiciones, Permisos y roles, Reglas de negocio, Integraciones y Manejo de errores.

**Formato e importación.** Cada caso tiene sus campos completos (Description, Priority, Severity, Type, Behavior, Automation, Tags), listos para convertirse a JSON o CSV e importarse al TMT.

## Índice de módulos

| # | Código | Módulo | Bloque / Grupo | Casos |
|---|--------|--------|----------------|-------|
| 1 | [`AUTH`](#mod-auth) | Autenticación y acceso | Bloque 0 — Transversal | [n] |
| 2 | [`[GRUPO-MOD]`](#mod-grupo-mod) | [Módulo] | Bloque A — Operativa · [Grupo] | [n] |
| | | **Total** | | **[n]** |

## Resumen global

<!-- Estas tablas permiten revisar el balance del plan de un vistazo:
     un módulo crítico sin casos negativos, o un plan con 90% de prioridad
     alta, se ven acá antes que en ningún otro lado. -->

**Por prioridad**

| P0 | P1 | P2 | P3 | Total |
|----|----|----|----|-------|
| [n] | [n] | [n] | [n] | [n] |

**Por tipo**

| smoke | functional | regression | integration | Total |
|-------|-----------|-----------|-------------|-------|
| [n] | [n] | [n] | [n] | [n] |

**Por behavior**

| positive | negative | destructive | Total |
|----------|----------|-------------|-------|
| [n] | [n] | [n] | [n] |

**Total por módulo**

| Código | P0 | P1 | P2 | P3 | smoke | funct. | regr. | integ. | positive | negative | destr. | Total |
|--------|----|----|----|----|-------|--------|-------|--------|----------|----------|--------|-------|
| `[GRUPO-MOD]` | [n] | [n] | [n] | [n] | [n] | [n] | [n] | [n] | [n] | [n] | [n] | [n] |

## Definición de los campos

<!-- Incluí esta tabla para que cualquiera clasifique igual.
     Ajustá los valores a los que use tu TMT. -->

| Campo | Qué mide | Valores |
|-------|----------|---------|
| **Priority** | Qué se prueba primero. | `high` · `medium` · `low` (tag `P0`–`P3` para más granularidad) |
| **Severity** | Cuánto daño hace si el comportamiento falla. | `blocker` · `critical` · `major` · `normal` · `minor` · `trivial` |
| **Type** | Para qué ejecución sirve el caso. | `smoke` (camino crítico, se corre siempre) · `functional` · `regression` · `integration` |
| **Behavior** | Qué tipo de escenario plantea. | `positive` (camino esperado) · `negative` (el sistema rechaza lo inválido) · `destructive` (acciones irreversibles o de borrado) |
| **Automation** | Estado de automatización en el TMT. | `is-not-automated` · `automated` · `to-be-automated` |
| **Tags** | Filtros para el TMT y los reportes. | `module:` · `category:` · `type:` · `priority:` · `bloque:` |

> **Prioridad no es impacto.** La prioridad ordena la ejecución; la severidad estima el daño; el impacto (a cuánta gente o proceso afecta) suele combinarse con ambos para decidir la prioridad.

---

# Bloque 0 — Transversal / acceso

<a id="mod-auth"></a>

### Casos de prueba — Autenticación y acceso (AUTH)

<!-- ... -->

---

# Bloque A — Aplicación operativa

## Grupo: [Grupo del menú]

<a id="mod-grupo-mod"></a>

### Casos de prueba — [GRUPO-MOD] ([Grupo] › [Módulo])

<!-- Encabezado del módulo: un resumen de dos líneas copiado o resumido del
     documento maestro, con ruta base y permisos. Le da contexto a quien lee
     un caso suelto y al agente que genera specs. -->

> Gestión de pedidos de venta: listado con estado y total, detalle con ítems y acciones de facturar y anular.
> Ruta base: `/ventas/pedidos`. Permisos: `PEDIDOS` lectura / escritura. Los casos describen el comportamiento esperado; para bugs conocidos validan el comportamiento objetivo.

---

#### Operaciones CRUD

###### TC-[GRUPO-MOD]-001: Crear un pedido con un ítem válido
- **Description:** Valida que al completar cliente e ítem y guardar, el pedido se cree en estado Borrador y aparezca en el listado.
- **Priority:** high
- **Severity:** critical
- **Type:** smoke
- **Behavior:** positive
- **Automation:** is-not-automated
- **Tags:** module:[grupo-mod], category:crud, type:smoke, priority:P0, bloque:operativo

#### Validación de datos

###### TC-[GRUPO-MOD]-002: Rechazar un pedido sin ítems
- **Description:** Valida que el backend rechace la creación de un pedido con la lista de ítems vacía, según el DTO de alta, y que el formulario muestre el mensaje correspondiente.
- **Priority:** high
- **Severity:** major
- **Type:** functional
- **Behavior:** negative
- **Automation:** is-not-automated
- **Tags:** module:[grupo-mod], category:validation, type:functional, priority:P1, bloque:operativo

###### TC-[GRUPO-MOD]-003: Rechazar limit fuera del rango 1–100 en el listado
- **Description:** Valida que el endpoint de listado rechace un `limit` menor a 1 o mayor a 100.
- **Priority:** low
- **Severity:** minor
- **Type:** functional
- **Behavior:** negative
- **Automation:** is-not-automated
- **Tags:** module:[grupo-mod], category:validation, type:functional, priority:P3, bloque:operativo

#### Listado y búsqueda

###### TC-[GRUPO-MOD]-004: Buscar pedido por número con debounce
- **Description:** Valida que el buscador ("Buscar pedido...") filtre el listado por número o cliente aplicando el debounce configurado.
- **Priority:** medium
- **Severity:** normal
- **Type:** functional
- **Behavior:** positive
- **Automation:** is-not-automated
- **Tags:** module:[grupo-mod], category:listing, type:functional, priority:P2, bloque:operativo

#### Acciones de fila

#### Comportamiento de formularios

#### Estados y transiciones

###### TC-[GRUPO-MOD]-005: Impedir anular un pedido facturado
- **Description:** Valida que un pedido en estado Facturado no muestre la acción "Anular" y que el endpoint de anulación lo rechace.
- **Priority:** high
- **Severity:** major
- **Type:** functional
- **Behavior:** negative
- **Automation:** is-not-automated
- **Tags:** module:[grupo-mod], category:states, type:functional, priority:P1, bloque:operativo

#### Permisos y roles

###### TC-[GRUPO-MOD]-006: Restringir la anulación a Administrador
- **Description:** Valida que un usuario Vendedor no pueda anular un pedido ni desde la interfaz ni llamando al endpoint directamente. *(Comportamiento objetivo: hoy es un bug conocido.)*
- **Priority:** high
- **Severity:** critical
- **Type:** functional
- **Behavior:** negative
- **Automation:** is-not-automated
- **Tags:** module:[grupo-mod], category:permissions, type:functional, priority:P0, bloque:operativo

#### Reglas de negocio

#### Integraciones

#### Manejo de errores

<!-- Si una categoría no tiene casos, dejá una línea explicando por qué
     ("No aplica: el módulo no tiene formularios"). Una categoría vacía sin
     explicación es un hueco sin revisar. -->

---

# Bloque B — Administración
