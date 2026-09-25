# Módulo: [Grupo del menú] › [Nombre del módulo]

<!--
PLANTILLA — Documento modular (etapa 1, paso 1 del método Contexto en Cascada).

Qué es: el relevamiento completo de UN módulo. Es un documento de TRABAJO:
describe lo que el sistema hace y, en la última sección, registra todo lo que
no cierra (bugs, dudas, cosas a verificar). Esos hallazgos son entregables
desde el primer día.

Cómo se redacta: recomendado, con un agente de código con acceso al
repositorio (prompts/01-relevar-modulo.md). El frontend es la fuente de verdad
de estructura y flujo; el backend, de validaciones, permisos y reglas.

Reglas de escritura:
- Presente indicativo, comportamiento observable ("el listado muestra…").
- Nombrá los textos de la interfaz entre comillas, tal como aparecen.
- Citá la fuente de cada regla (archivo, DTO, componente) cuando exista.
- No resuelvas dudas por tu cuenta: van a "Notas o ambigüedades".
- Enlazá otros módulos con [[CÓDIGO]].
- Si una sección no aplica, escribí "No aplica" y por qué. No la borres.

Cuándo pasa al documento maestro: cuando no quedan notas [PENDIENTE],
[VERIFICAR] o [COMPLETAR] que cambien el comportamiento esperado.
-->

## Código de módulo
`[GRUPO-MOD]` [PROPUESTO]

<!-- Código corto y estable: se usa en los IDs de los casos (TC-[GRUPO-MOD]-NNN)
     y en los tags del TMT. Ej.: VEN-PED = Ventas › Pedidos.
     Marcá [PROPUESTO] hasta que el equipo lo confirme. -->

> **Relevado [AAAA-MM-DD]** contra la versión [back X.Y.Z / front X.Y.Z].
> <!-- Si es una regeneración, indicá qué reemplaza y qué cambió. -->

## Descripción funcional

<!-- Qué hace el módulo, para quién y dónde está. Un párrafo.
     Incluí la ruta base y la ubicación en el menú.
     Si se parece a otro módulo, aclarar la diferencia evita confusiones. -->

*Ejemplo:* Gestión de **pedidos de venta** de la empresa. Presenta el listado de pedidos con su estado y total, y el detalle de cada pedido con sus ítems, pagos y acciones. Es el segundo ítem del grupo "Ventas" del menú. Ruta base: `/ventas/pedidos`.

> A diferencia de [[VEN-FAC]] (facturas), un pedido no tiene valor fiscal: se convierte en factura desde la acción "Facturar".

## Usuarios, permisos y planes

<!-- Quién puede hacer qué. Una fila por acción o endpoint.
     Incluí cómo se verifica el permiso en frontend y en backend:
     muchas inconsistencias aparecen justo acá (una pantalla sin protección,
     un endpoint sin guard). Si hay planes o suscripciones que habilitan
     funciones, van en esta sección. -->

| Acción / endpoint | Permiso requerido | Rol / plan |
|-------------------|-------------------|------------|
| Ver listado y detalle | `PEDIDOS` lectura | Vendedor, Administrador |
| Crear y editar pedido | `PEDIDOS` escritura | Vendedor, Administrador |
| Anular pedido | `PEDIDOS` escritura | Administrador |

Notas de permisos:
- **Front — listado:** protegido con [guard / HOC]; sin acceso redirige a [ruta].
- **Front — detalle:** [protegido / sin protección propia → ver Notas].
- **Back:** [guard a nivel controller / por endpoint].

## Pantallas y vistas

Orden según el frontend.

<!-- Una entrada por pantalla, en el orden en que el usuario las encuentra.
     Para cada una: ruta, archivo del frontend, elementos principales
     (filtros, columnas, tarjetas, tabs, botones), estado vacío y modales.
     Los textos de la interfaz, entre comillas y literales. -->

1. **Listado de pedidos** (`/ventas/pedidos`, `[ruta/al/componente]`): buscador "Buscar pedido...", filtro de estado y botón "Nuevo pedido". Columnas: "N°", "Cliente", "Fecha", "Total", "Estado". Paginación. Estado vacío: "No hay pedidos".
2. **Detalle del pedido** (`/ventas/pedidos/[id]`):
   - **Encabezado:** número, cliente, chip de estado; acciones "Editar" y "Facturar".
   - **Ítems:** tabla con "Producto", "Cantidad", "Precio", "Subtotal".
   - **Modales:** confirmación de anulación, conversión a factura.

**Componentes:** [librería de UI y componentes relevantes: tablas, date pickers, diálogos…]

## Acciones disponibles

<!-- Agrupadas por pantalla. Para cada acción: qué la dispara, qué hace,
     qué endpoint llama, y qué pasa si falla (mensaje de error literal). -->

### Listado
- **Buscar** (debounce [n] ms) · **Filtrar** por estado · ordenar por columna · paginación.
- **"Nuevo pedido"** → formulario de alta.
- **Click en fila** → detalle.

### Detalle
- **"Facturar"** → convierte el pedido en factura ([[VEN-FAC]]). Solo en estado `CONFIRMADO`.
- **"Anular"** → modal de confirmación; error → "No se pudo anular el pedido".

## Validaciones de negocio

<!-- Reglas de entrada de datos, con su fuente. Frontend (esquema de
     formulario) y backend (DTO/validadores) por separado: cuando no coinciden,
     es un hallazgo. Incluí mensajes de error literales, límites, formatos,
     obligatorios y valores por defecto. -->

- **Formulario de pedido (front, [esquema]):** cliente obligatorio ("Seleccioná un cliente"); al menos un ítem; cantidad > 0.
- **Alta de pedido (back, `[CrearPedidoDto]`):** `clienteId` requerido; `items` 1–100; `cantidad ≥ 1`.
- **Listado (back):** `page ≥ 1`; `limit` 1–100 (def 10); `orderBy` en lista blanca.

## Estados y transiciones

<!-- Estados de las entidades del módulo, cómo se muestran (texto y color del
     chip), qué transiciones son válidas, qué las dispara (usuario, proceso
     automático, integración) y qué efectos tienen. -->

**Estado del pedido:** `BORRADOR` "Borrador" (gris), `CONFIRMADO` "Confirmado" (azul), `FACTURADO` "Facturado" (verde), `ANULADO` "Anulado" (rojo).

**Transiciones:**
- `BORRADOR → CONFIRMADO`: al guardar con ítems válidos.
- `CONFIRMADO → FACTURADO`: acción "Facturar".
- `CONFIRMADO → ANULADO`: acción "Anular" (solo Administrador).
- Desde `FACTURADO` o `ANULADO` no hay transiciones.

## Endpoints relacionados

<!-- Todos los endpoints que usa el módulo, con su fuente (código, Swagger,
     colección Postman). Si un endpoint aparece en el código pero no en la
     documentación de la API, anotalo. -->

| Método | Ruta | Propósito | Fuente |
|--------|------|-----------|--------|
| GET | `/pedidos?...` | Listar pedidos | código |
| GET | `/pedidos/:id` | Detalle | código |
| POST | `/pedidos` | Crear | código |
| POST | `/pedidos/:id/anular` | Anular | código |

## Integraciones

<!-- Servicios externos, colas, almacenamiento, correo, procesos programados.
     Qué aporta cada uno y qué pasa cuando falla. -->

- **Servicio de correo:** envía la confirmación al cliente. Si falla, el pedido se guarda igual.
- **Proceso programado:** [si aplica: qué hace, cuándo corre, cómo se dispara manualmente].

## Reglas de negocio adicionales

<!-- Lo que no entra en las secciones anteriores: significado de montos o
     indicadores, cálculos, redondeos, reglas de moneda, comportamientos
     sin pantalla (procesos automáticos). -->

- **Total del pedido** = suma de subtotales − descuentos; se recalcula al editar ítems.
- **Numeración:** correlativa por empresa, no reutilizable tras anular.

## Notas o ambigüedades

<!-- LA SECCIÓN QUE ENTREGA VALOR DESDE EL DÍA UNO.
     Todo lo que no cierra, con una etiqueta y, si aplica, la fuente:

     [BUG]        El sistema hace algo que no debería. Indicar el objetivo.
     [PENDIENTE]  Ambigüedad sin definir. Con fecha y con quién se consulta.
     [VERIFICAR]  Algo que el código sugiere y hay que confirmar en la app.
     [COMPLETAR]  Información que falta y hay que pedir al equipo.
     [RESUELTO]   Aclarado. Indicar la decisión. "[RESUELTO — intencional]"
                  cuando lo que parecía un bug es el comportamiento esperado.

     No borres las notas resueltas: explican por qué el sistema es como es.
     Los [BUG] se reportan en el gestor de incidencias en el momento. -->

**Resueltos:**
- **[RESUELTO]** El detalle ahora exige el permiso `PEDIDOS` lectura (antes era accesible con cualquier token).

**Revisión [AAAA-MM-DD]:**
1. **[BUG]** El endpoint `POST /pedidos/:id/anular` no valida el rol: un Vendedor puede anular por API aunque la interfaz no le muestre el botón. **Objetivo:** restringir a Administrador. Back: `[ruta/al/controller]`.
2. **[PENDIENTE AAAA-MM-DD]** No está definido si un pedido `FACTURADO` puede anularse cuando se anula su factura. Consultado con Producto.
3. **[VERIFICAR]** El formulario permite cantidad decimal, pero el DTO exige entero. Confirmar en la app qué pasa al guardar.
4. **[COMPLETAR]** Política de reintentos del correo de confirmación.
5. **[RESUELTO — intencional]** El listado muestra los totales sin impuestos. Es el comportamiento esperado.
