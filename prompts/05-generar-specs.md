# Prompt — Generar los specs desde el automation progress

> Etapa 4. Lotes de 5 a 10 casos. Nunca el módulo entero de una vez.

```
Vas a generar los specs de los próximos [N] casos en estado 📋 pendiente
del módulo [CÓDIGO] en el automation progress.

CONTEXTO OBLIGATORIO — leelo antes de escribir una línea de código:
1. Automation progress: [ruta]
2. Estándares de automatización: [ruta]
3. Contexto del módulo: [ruta]
4. Documento maestro (sección del módulo): [ruta]
5. Los casos en el TMT (vía MCP) o en el plan de pruebas: [ruta / filtro]
6. La aplicación corriendo en [URL], accesible con el MCP de Playwright

PROCEDIMIENTO:

1. Tomá los casos del automation progress y leé sus pasos y resultado esperado
   desde el TMT o el plan. Los ID salen de la fuente: no los transcribas.

2. Antes de escribir cada spec, navegá la pantalla involucrada y confirmá que
   los localizadores existen. Si un localizador del contexto de módulo ya no
   funciona, avisame: es un hallazgo, no algo para arreglar por tu cuenta.

3. Escribí los specs siguiendo los estándares y el contexto de módulo.
   Reutilizá los helpers y page objects existentes; si necesitás uno nuevo,
   proponelo antes de crearlo.

4. Cada test lleva su TC-ID en el título y el ID del TMT en la anotación
   definida en los estándares (con playwright-qase-reporter: type 'QaseID').

5. Las aserciones deben verificar el resultado observable que dice el
   comportamiento, no solo que la navegación ocurrió.

6. Cada test debe ser aislado: monta su propia precondición y no depende
   del orden de ejecución.

7. Ejecutá los specs generados y mostrame el resultado.

8. Si algo falla, NO lo arregles todavía: clasificá el fallo según
   metodo/05-categorias-y-analisis-de-fallo.md y mostrame la clasificación.

9. Actualizá el automation progress: estado de cada caso (🔄 draft,
   🔄 fixme con motivo, o ✅ si pasa y fue revisado), archivo del spec,
   resumen de avance y próximas acciones.
```

## Después del lote

1. Revisar con los cinco puntos de `metodo/04-generacion-con-agente.md`.
2. Clasificar los fallos y actuar según la tabla de análisis.
3. **Actualizar el archivo de contexto del módulo** con lo aprendido: nuevas trampas, nuevos helpers, selectores corregidos.
4. Recién entonces, el siguiente lote.

El paso 3 es el que hace que el método mejore con el uso. Si se saltea, cada lote cuesta lo mismo que el primero.
