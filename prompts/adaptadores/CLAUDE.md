# CLAUDE.md — ejemplo para Claude Code

<!--
Copiar a la raíz del proyecto de automatización y adaptar.
Este archivo es lo que el agente lee en cada sesión: mantenelo corto.
Lo específico de cada módulo va en su archivo de contexto, no acá.
-->

# Proyecto de automatización — [PROYECTO]

Este proyecto sigue el método **Contexto en Cascada**. Antes de generar o modificar tests, leé la documentación de la capa correspondiente.

## Jerarquía de fuentes de verdad

Cuando haya contradicción entre fuentes, este es el orden de autoridad:

1. `docs/comportamientos/maestro.md` — qué hace el sistema (fuente de verdad)
2. Los casos en el TMT (vía MCP) o en el plan de pruebas — qué se verifica
3. `docs/automatizacion/estandares.md` y `docs/automatizacion/contexto/[modulo].md` — reglas del suite y del módulo
4. El código existente del suite

El trabajo se organiza en `docs/automatizacion/automation-progress.md`: tomá los casos 📋 pendiente de ahí y actualizá su estado (🔄 / ✅), el resumen y las próximas acciones al terminar cada lote.

Si encontrás una contradicción, **avisá; no la resuelvas por tu cuenta.**

Los documentos modulares (`docs/comportamientos/modulos/`) son documentos de trabajo con notas abiertas: **no son fuente de verdad para generar tests.** Solo el documento maestro lo es.

## Stack

- Framework: [Playwright + TypeScript]
- Gestor de casos: [QASE] (MCP configurado)
- Navegador: MCP de Playwright, apuntando a [URL del ambiente de pruebas]

## Reglas no negociables

- **No inventes selectores.** Verificalos contra la aplicación antes de escribirlos.
- **No inventes reglas de negocio.** Si no está en el documento maestro, preguntá.
- **No transcribas ID de casos de memoria.** Leelos del TMT o del plan.
- **Trazabilidad:** TC-ID en el título de cada test; ID del TMT en la anotación `QaseID`.
- **Reutilizá helpers existentes.** Antes de crear uno nuevo, proponelo.
- **Tests aislados y paralelizables.** Nada que dependa del orden.
- **Aserciones sobre resultado observable**, no sobre navegación.

## Comandos

```bash
[comando de ejecución del suite]
[comando de ejecución de un archivo]
[comando de linter]
```

## Convenciones

- Estructura de carpetas: [describir]
- Nombres de archivo: [patrón]
- Trazabilidad al caso: [cómo se anota el ID]
- Estrategia de localización: [role / test-id]

## Flujo de trabajo esperado

Generación en lotes de 5 a 10 casos. Después de cada lote: ejecutar, clasificar fallos según el análisis de fallo del método, actualizar el automation progress y el contexto del módulo con lo aprendido.
