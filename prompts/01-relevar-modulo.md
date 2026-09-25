# Prompt — Relevar un módulo (documento modular)

> Etapa 1, paso 1. Para un agente de código con **acceso al repositorio de la aplicación** (frontend y backend).
> El resultado es un documento de trabajo: su valor principal está en "Notas o ambigüedades".

```
Vas a relevar el módulo [Grupo] › [Módulo] de la aplicación [APLICACIÓN]
y producir su documento modular siguiendo EXACTAMENTE la estructura de
plantillas/documento-modular.md.

FUENTES:
- Frontend: [ruta del repo]. Es la fuente de verdad de estructura y flujo:
  pantallas, orden, textos, formularios, protección de rutas.
- Backend: [ruta del repo]. Es la fuente de verdad de validaciones (DTOs,
  validadores), permisos (guards, políticas), endpoints, estados y reglas.
- Documentación adicional, si existe: [Swagger, colecciones de API, tickets].

REGLAS:

1. Describí lo que el sistema HACE, en presente y de forma observable.
   Los textos de la interfaz, literales y entre comillas.

2. Seguí el orden del frontend: las pantallas en el orden en que el usuario
   las encuentra, y dentro de cada pantalla, de arriba hacia abajo.

3. Citá la fuente de cada regla: archivo, componente, DTO o endpoint.
   En "Endpoints relacionados", indicá si la fuente es código, Swagger u otra.

4. Cruzá frontend y backend en cada sección. Las diferencias entre ambos
   (una validación que existe en uno y no en el otro, una pantalla sin
   protección con un endpoint protegido) son hallazgos.

5. Registrá en "Notas o ambigüedades", con su etiqueta, todo lo que no cierre:
   [BUG]        el sistema hace algo que no debería (indicá el objetivo)
   [PENDIENTE]  ambigüedad sin definir
   [VERIFICAR]  algo que el código sugiere y hay que confirmar en la app
   [COMPLETAR]  información que falta y hay que pedir al equipo
   Redactá cada nota como algo que una persona del equipo pueda responder,
   con la ruta del archivo involucrado.

6. NO RESUELVAS hallazgos por tu cuenta. No elijas la interpretación
   "más razonable": registrala como nota.

7. Si una sección no aplica al módulo, escribí "No aplica" y por qué.

Empezá mostrándome un índice: las pantallas que encontraste, en orden,
y la lista de notas detectadas con su etiqueta. Esperá mi confirmación
antes de desarrollar el documento completo.
```

## Variante: refinar un borrador escrito a mano

Redactar a mano tiene su riesgo: uno documenta lo que *cree* que hace el sistema, se saltean las reglas negativas y queda incompleto. Refinarlo con IA tiene el riesgo contrario: el agente rellena huecos con supuestos. Este prompt limita al agente a la forma:

```
Te paso un borrador del documento modular de [Módulo] escrito a mano: [ruta].
Ordenalo según plantillas/documento-modular.md y mejorá la redacción.

NO agregues reglas, pantallas, validaciones ni condiciones que no estén
en el borrador. Todo lo que te parezca incompleto, contradictorio o
ambiguo, NO lo completes: registralo en "Notas o ambigüedades"
con la etiqueta [COMPLETAR] o [VERIFICAR].
```

## Después de generar

1. **Verificar contra la aplicación.** Lo que dice el código y lo que pasa en pantalla pueden diferir: eso también es una nota.
2. **Completar lo que el código no dice.** Reglas que viven en la cabeza del equipo.
3. **Llevar las notas a quien corresponda.** Producto para `[PENDIENTE]` y `[COMPLETAR]`, desarrollo para `[BUG]`. Reportar los bugs en el gestor de incidencias en ese momento.
4. **Actualizar las etiquetas** a `[RESUELTO]` a medida que se aclaran, con la decisión tomada. No borrarlas.
