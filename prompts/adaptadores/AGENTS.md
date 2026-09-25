# AGENTS.md — ejemplo para OpenCode y agentes compatibles

<!--
`AGENTS.md` es el formato de instrucciones que leen OpenCode y varios otros
agentes de código. El contenido es equivalente al de CLAUDE.md: lo que cambia
es el nombre del archivo y la configuración de MCP y de modelo.

Si querés soportar ambos agentes sin duplicar contenido, dejá uno de los dos
como enlace simbólico al otro.
-->

# Proyecto de automatización — [PROYECTO]

Este proyecto sigue el método **Contexto en Cascada**. Antes de generar o modificar tests, leé la documentación de la capa correspondiente.

## Jerarquía de fuentes de verdad

1. `docs/comportamientos/maestro.md` — qué hace el sistema (fuente de verdad)
2. Los casos en el TMT o en el plan de pruebas — qué se verifica
3. `docs/automatizacion/estandares.md` y `docs/automatizacion/contexto/[modulo].md` — reglas del suite y del módulo
4. El código existente del suite

El trabajo se organiza en `docs/automatizacion/automation-progress.md`: tomá los casos 📋 pendiente de ahí y actualizá su estado (🔄 / ✅), el resumen y las próximas acciones al terminar cada lote.

Ante una contradicción entre fuentes: avisar, no resolver por cuenta propia.

Los documentos modulares (`docs/comportamientos/modulos/`) tienen notas abiertas: no son fuente de verdad para generar tests. Solo el documento maestro lo es.

## Reglas no negociables

- No inventar selectores: verificarlos contra la aplicación.
- No inventar reglas de negocio: si no está documentado, preguntar.
- No transcribir ID de casos de memoria: leerlos del TMT o del plan.
- Trazabilidad: TC-ID en el título de cada test; ID del TMT en la anotación `QaseID`.
- Reutilizar helpers existentes.
- Tests aislados y paralelizables.
- Aserciones sobre resultado observable.

## Configuración de MCP

OpenCode define los servidores MCP en su archivo de configuración
(`opencode.json` en el proyecto, o la configuración global del usuario).
Los dos servidores que este método usa:

```jsonc
{
  "mcp": {
    "playwright": {
      "type": "local",
      "command": ["npx", "@playwright/mcp@latest"],
      "enabled": true
    },
    "qase": {
      // Ver guias/tmt-e-ids.md — requiere token de API
      "type": "local",
      "command": ["[comando del servidor MCP del gestor]"],
      "enabled": true
    }
  }
}
```

> Verificá los nombres exactos de los campos contra la documentación vigente
> de OpenCode: la configuración cambia entre versiones.

## Modelos sin costo

OpenCode permite conectar varios proveedores. Para quien quiera probar el método sin presupuesto, la combinación razonable es un modelo gratuito o de bajo costo para las tareas mecánicas (expansión de casos, generación de specs repetitivos) y reservar un modelo más capaz para la consolidación del documento maestro, que es donde la calidad del razonamiento importa de verdad.

Es la ventaja práctica de que el método sea agnóstico: **las capas 1 y 2 son texto**, y el texto se puede producir con cualquier herramienta, incluso a mano.

## Comandos

```bash
[comando de ejecución del suite]
[comando de linter]
```
