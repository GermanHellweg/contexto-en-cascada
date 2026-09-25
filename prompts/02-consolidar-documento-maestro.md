# Prompt — Consolidar en el documento maestro

> Etapa 1, paso 2. Se ejecuta cada vez que un documento modular queda listo.
> El documento maestro crece módulo a módulo; nunca se escribe de una vez.

```
Vas a incorporar el módulo [CÓDIGO] al documento maestro.

ENTRADAS:
- Documento modular: [ruta]
- Documento maestro actual: [ruta] (si es el primer módulo, partí de
  plantillas/documento-maestro.md)

REGLAS:

1. Antes de incorporar, revisá "Notas o ambigüedades" del documento modular.
   Si hay notas [PENDIENTE], [VERIFICAR] o [COMPLETAR] que cambian el
   comportamiento esperado, NO incorpores el módulo: listá esas notas y
   detenete.

2. Incorporá el módulo en su lugar según el orden de navegación: bloque,
   grupo y posición dentro del grupo. Actualizá el índice de módulos.

3. Copiá todas las secciones del documento modular EXCEPTO "Notas o
   ambigüedades". Los hallazgos no van al maestro.

4. Para las notas [BUG], documentá el comportamiento ESPERADO (el objetivo
   de la nota), no el comportamiento defectuoso actual.

5. Para las notas [RESUELTO], asegurate de que la sección correspondiente
   refleje la decisión tomada.

6. Si el módulo usa términos que no están en el glosario, proponé agregarlos.

7. Si algo del módulo contradice lo ya consolidado de otro módulo, no lo
   resuelvas: reportalo como una nota nueva en el documento modular.
```

## Revisión

Leer el resultado buscando dos cosas: dudas que se colaron redactadas como reglas firmes, y comportamientos defectuosos que quedaron documentados como si fueran los esperados.
