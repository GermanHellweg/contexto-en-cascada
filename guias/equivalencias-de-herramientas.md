# Equivalencias de herramientas

El método es agnóstico: cada capa define un artefacto y un criterio, no una herramienta. Esta tabla es para que puedas armarte tu propia combinación.

## Por capa

| Capa | Artefacto | Implementación de referencia | Alternativas | Mínimo viable |
|---|---|---|---|---|
| 1. Comportamientos | Documento por módulo | Markdown en el repo | Confluence, Notion, cualquier wiki | Un archivo de texto |
| 2. Plan de pruebas | Casos clasificados y priorizados | QASE | TestRail, Xray, Zephyr, TestLink | Markdown o planilla |
| 3. Automatización | Criterios + contexto por módulo | Markdown en el repo | Igual | Un archivo de texto |
| 4. Generación | Specs trazables | Playwright + TypeScript + Claude Code | Cypress, Selenium, Robot; OpenCode, Codex, Cursor, Gemini CLI | Escribirlos a mano |

La última columna importa: **las etapas 1, 2 y 3 se pueden hacer sin ninguna herramienta especial y sin IA.** Son texto. El agente acelera la etapa 4 y ayuda en la 2, pero el valor del método no depende de tener presupuesto.

## Agentes sin costo o de bajo costo

Para quien quiera probar el método sin pagar licencias:

- **OpenCode** es un agente de código abierto que corre en la terminal y permite conectar distintos proveedores de modelos, incluidos los gratuitos o de bajo costo. Soporta MCP, así que las dos conexiones que este método usa (gestor de casos y navegador) funcionan igual.
- **Modelos locales** vía Ollama o similares sirven razonablemente para tareas mecánicas de la capa 4, con más supervisión.
- **Repartir el trabajo por dificultad** es la estrategia más eficiente: un modelo económico para expandir casos y generar specs repetitivos, uno más capaz para consolidar el documento maestro, que es donde el razonamiento define la calidad del resultado.

El archivo `prompts/adaptadores/AGENTS.md` es el equivalente de `CLAUDE.md` para OpenCode y otros agentes compatibles con ese formato.

## Cómo elegir

Tres preguntas, en este orden:

1. **¿El gestor de casos expone una API o un MCP?** Si no, la trazabilidad va a ser manual y el método pierde su mejor propiedad.
2. **¿El framework permite montar precondiciones por API?** Si cada test tiene que llegar a su estado haciendo clics, el suite va a ser lento y frágil.
3. **¿El agente puede leer archivos del repositorio y navegar la aplicación?** Sin lo primero no hay contexto; sin lo segundo, los selectores son adivinados.

Cualquier combinación que responda que sí a las tres sirve.
