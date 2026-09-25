# Prompt — Generar el automation progress y el contexto de módulo

> Etapa 3. El automation progress se crea una vez y se actualiza en cada lote.
> El contexto de módulo se arma antes del primer spec de cada módulo.

## A. Armar el automation progress

```
A partir del plan de pruebas, armá el automation progress siguiendo
plantillas/automation-progress.md.

ENTRADAS:
- Plan de pruebas: [ruta, o TMT vía MCP]
- Estándares de automatización: [ruta]
- Contexto del proyecto: [qué es lo más crítico del negocio, qué módulos
  están en refactor, restricciones de datos, qué no se automatiza nunca]

REGLAS:
1. Proponé los criterios de tier adaptados a este proyecto. Recordá:
   prioridad ≠ tier. El tier es el orden de automatización.
2. Definí una meta de Tier 1 concreta y chica (por ejemplo: todos los smoke
   + los P0 del flujo principal), con su número.
3. Proponé las decisiones clave y las fases por bloque.
4. Armá el índice de módulos en orden de automatización y, para Tier 1,
   la tabla por módulo con TC-ID, ID del TMT, título, tipo, prioridad y
   estado 📋 pendiente. Si falta el ID del TMT, no lo inventes: [ID-PENDIENTE].
5. Listá lo que queda fuera de automatización, con motivo.
6. Completá pre-requisitos, bloqueantes y riesgos con lo que sepas;
   lo que no sepas, marcalo para completar con el equipo.
7. No escribas código todavía.
```

## B. Armar el contexto del módulo

```
Vas a construir el archivo de contexto del módulo [CÓDIGO] siguiendo
plantillas/contexto-de-modulo.md.

1. Recorré la aplicación en [URL] con el MCP de Playwright y relevá rutas,
   pantallas y los localizadores reales de los elementos principales.
2. Revisá los specs y helpers existentes en [ruta] y extraé lo reutilizable.
3. Listá los roles disponibles y cómo se autentica cada uno.

Dejá con TODO lo que no puedas completar con evidencia. No inventes
selectores ni helpers. La sección "Trampas conocidas" arrancá vacía.
```
