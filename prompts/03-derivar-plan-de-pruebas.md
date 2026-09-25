# Prompt — Derivar el plan de pruebas de un módulo

> Etapa 2. El agente hace la expansión mecánica; la persona decide prioridad, severidad y qué es ruido.
> Un módulo por vez, en el orden del documento maestro.

```
A partir del documento maestro, derivá los casos de prueba del módulo
[CÓDIGO] y agregalos al plan de pruebas siguiendo plantillas/plan-de-pruebas.md.

ENTRADA: [ruta del documento maestro], sección del módulo [CÓDIGO]
CATEGORÍAS: las diez de metodo/05-categorias-y-analisis-de-fallo.md

REGLAS:

1. Recorré las secciones del módulo en orden y generá los casos de la
   categoría que cada sección alimenta (tabla de correspondencia en
   metodo/05). Por ejemplo: "Validaciones de negocio" → Validación de datos;
   "Usuarios, permisos y planes" → Permisos y roles.

2. Cada caso describe el COMPORTAMIENTO ESPERADO. Nunca valides un bug.

3. IDs correlativos: TC-[CÓDIGO]-001, 002…

4. Completá todos los campos: Description, Priority, Severity, Type,
   Behavior, Automation (is-not-automated) y Tags
   (module:, category:, type:, priority:, bloque:).
   Definiciones de cada valor en metodo/02-plan-de-pruebas.md.
   Priority y Severity son propuestas: los valores finales los decido yo.

5. En permisos, generá casos tanto por interfaz como por endpoint directo.

6. Si una categoría no tiene casos, dejá una línea explicando por qué.

7. Si al derivar detectás algo que el documento maestro no cubre, NO lo
   inventes como caso: listalo aparte bajo "NOTAS PARA EL DOCUMENTO MODULAR".

8. Actualizá el índice de módulos y las tablas de resumen global.

Antes de escribir los casos completos, mostrame solo los títulos agrupados
por categoría, para revisar cobertura y duplicados.
```

## Revisión humana

Los cinco puntos de control están en `metodo/02-plan-de-pruebas.md`. El más importante en la práctica: **buscar duplicados con distinta redacción**, el error más frecuente de la generación asistida.
