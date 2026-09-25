# Contexto en Cascada

Una metodología para generar pruebas con agentes de IA sin perder el control sobre qué se prueba y por qué.

> Creado por **German Hellweg**. Presentado en la charla **"El agente no adivina: automatización asistida por IA sin perder el control"**
> — PY Testing Fest 2026, Asunción, 26 de septiembre de 2026.

---

## El problema

Pedirle casos de prueba a un agente de IA sin contexto produce resultados que se ven bien y no sirven: comportamientos inventados, casos duplicados, selectores que no existen, cero relación con el riesgo real del producto. El agente no falla por falta de capacidad. Falla porque no sabe qué hace el sistema.

La respuesta habitual es escribir prompts más largos. La de este método es otra: **construir el contexto como un artefacto versionado, no como un mensaje de chat.**

## La idea en una frase

Primero se documenta qué hace el sistema, módulo por módulo y en el orden de la UI. De ahí se deriva qué probar. De ahí, qué vale la pena automatizar. Y recién ahí entra el agente, que ya no tiene que adivinar nada.

## El flujo

```
  1. DOCUMENTO MAESTRO
  ┌──────────────────────┐
  │ Documentos modulares │──► HALLAZGOS: bugs, ambigüedades, inconsistencias
  │ (orden de la UI)     │    → entregables desde el primer día
  └──────────┬───────────┘
             │ solo lo aclarado
  ┌──────────▼───────────┐
  │ Documento maestro    │   fuente de verdad de la aplicación
  └──────────┬───────────┘
             ▼
  2. PLAN DE PRUEBAS (MD) ──exportar──► TMT asigna IDs ──► vuelven al plan
             │
             ▼
  3. AUTOMATION PROGRESS  +  ESTÁNDARES DE AUTOMATIZACIÓN
     qué se automatiza,        reglas del suite
     en qué orden, estado      + contexto por módulo
             │
             ▼
  4. GENERACIÓN CON AGENTE ──► specs con ID ──► resultados al TMT
             │
             └──► el análisis de fallo retroalimenta el documento maestro
```

Cada documento es la fuente de verdad del siguiente. El agente nunca improvisa: lee el anterior.

### Qué entrega cada etapa

| Etapa | Produce | Ventaja principal |
|---|---|---|
| **1. Documento maestro** | Documentos modulares con hallazgos → documento maestro | **Valor desde el día uno**: bugs y ambigüedades encontrados antes de escribir un test. Una sola fuente de verdad. |
| **2. Plan de pruebas** | Casos por categoría, con prioridad, severidad, tipo y behavior; IDs del TMT | Derivación sistemática. Cobertura medible. Reportes automáticos en el TMT. |
| **3. Automation progress y estándares** | Casos por tier y fase, con estado; reglas del suite | Se automatiza lo que importa. "No automatizamos esto" es una decisión escrita. Progreso visible. |
| **4. Generación con agente** | Specs trazables | Velocidad sin perder criterio. Selectores reales. Mejora con cada lote. |

---

## Cómo empezar

1. **Elegí un módulo, no el sistema entero.** El que más duele.
2. **Relevalo en un documento modular**, en el orden de la UI. La vía recomendada es un agente de código con acceso al repositorio (`prompts/01-relevar-modulo.md`). También se puede redactar a mano, con más precaución, o a mano y refinado con IA, cuidando que no agregue reglas. Registrá cada hallazgo y aclaralo; los bugs se reportan en el momento.
3. **Consolidalo en el documento maestro** cuando no queden hallazgos abiertos (`prompts/02-consolidar-documento-maestro.md`).
4. **Derivá el plan de pruebas** del módulo (`prompts/03-derivar-plan-de-pruebas.md`) y revisalo a mano.
5. **Exportalo al TMT y traé los IDs**, por MCP o exportando y matcheando (`guias/tmt-e-ids.md`).
6. **Armá el automation progress** y definí los estándares de automatización (`prompts/04-generar-automation-progress.md`).
7. **Alimentá el repositorio de pruebas** con el agente, en lotes de 5 a 10 casos, hasta completar el progress (`prompts/05-generar-specs.md`).
8. **Pasá al siguiente módulo.**

Antes de arrancar, completá `guias/adaptacion.md` con tu stack y convenciones. Si el proyecto ya tiene tiempo y tests heredados, leé `guias/proyecto-existente.md`.

---

## Qué hay en cada carpeta

### `metodo/` — la metodología explicada

Leela en orden. Es el "por qué" de todo lo demás.

| Archivo | Qué explica |
|---|---|
| `00-premisas.md` | Las cinco afirmaciones sobre las que se apoya el método, y lo que el método **no** resuelve. |
| `01-documentacion-de-comportamientos.md` | Documentos modulares, orden de la UI, las tres vías de redacción y sus riesgos, hallazgos, y cómo se consolida el maestro. |
| `02-plan-de-pruebas.md` | Cómo cada sección del maestro alimenta una categoría de casos, los campos de cada caso (prioridad, severidad, tipo, behavior), revisión humana y dónde vive el plan. |
| `03-automation-progress-y-estandares.md` | El automation progress (prioridad ≠ tier, fases, estados, exclusiones) y los estándares de automatización, con el contexto por módulo. |
| `04-generacion-con-agente.md` | Qué fuentes lee el agente, el rol de los MCP, el ciclo de generación por lotes y qué revisa el humano. |
| `05-categorias-y-analisis-de-fallo.md` | Las **diez categorías de casos**, cada una ligada a una sección del documento, y las **categorías de fallo** (para que un test rojo diga dónde está el problema). |

### `plantillas/` — los artefactos vacíos

Markdown plano, sin dependencia de ninguna herramienta. Se copian al proyecto y se completan. Cada una tiene instrucciones en comentarios.

| Plantilla | Etapa | Para qué |
|---|---|---|
| `documento-modular.md` | 1 | Relevar un módulo por secciones, en el orden de la UI, con instrucciones y ejemplo en cada una, y registrar los hallazgos con etiquetas. |
| `documento-maestro.md` | 1 | Consolidar la aplicación en bloques y grupos del menú, con glosario e índice. Solo entra lo aclarado. |
| `plan-de-pruebas.md` | 2 | Índice, resúmenes por prioridad, tipo y behavior, definición de campos, y casos por categoría listos para importar al TMT. |
| `automation-progress.md` | 3 | Tiers, decisiones, fases, casos Tier 1 con estado, pre-requisitos, datos, bloqueantes y próximas acciones. El documento con el que trabaja el agente. |
| `estandares-de-automatizacion.md` | 3 | Estructura de archivos, trazabilidad con el TMT, comandos, ambiente y convenciones de código. Uno por proyecto. |
| `contexto-de-modulo.md` | 3 | Complemento de los estándares para un módulo: rutas, roles, selectores, helpers y **trampas conocidas**. |

### `prompts/` — instrucciones para el agente, una por paso

| Prompt | Qué hace el agente |
|---|---|
| `01-relevar-modulo.md` | Lee el código del módulo y produce el documento modular con sus hallazgos. Incluye variante para refinar un borrador escrito a mano. |
| `02-consolidar-documento-maestro.md` | Incorpora un módulo aclarado al documento maestro. |
| `03-derivar-plan-de-pruebas.md` | Expande los comportamientos en casos por tipo, y propone prioridad, impacto y severidad. |
| `04-generar-automation-progress.md` | Selecciona y ordena los casos a automatizar, y arma el contexto del módulo relevando la aplicación. |
| `05-generar-specs.md` | Toma los casos pendientes del progress, genera los specs verificando selectores en el navegador y actualiza los estados. |

Cada prompt incluye qué revisar a mano después de ejecutarlo.

`prompts/adaptadores/` tiene dos archivos de instrucciones permanentes para el agente, con las reglas del método y la jerarquía de fuentes de verdad:

- `CLAUDE.md` para Claude Code.
- `AGENTS.md` para OpenCode y otros agentes compatibles, con ejemplo de configuración de MCP.

### `guias/` — cómo aplicarlo en la práctica

| Guía | Para qué |
|---|---|
| `adaptacion.md` | **Completalo primero.** Declara tu stack, dónde viven los artefactos y tus convenciones. |
| `proyecto-existente.md` | Cómo entrar al método en un producto con años encima y tests heredados, en cuatro semanas. |
| `tmt-e-ids.md` | Exportar el plan a un TMT, traer los IDs (por MCP o exportando y matcheando) y enviar los resultados de ejecución al TMT. |
| `instalacion-playwright.md` | Paso a paso para instalar Playwright en un proyecto existente y crear la rama de trabajo (Windows, macOS y Linux). |
| `mcp-playwright.md` | Playwright en cuatro comandos, su MCP para que el agente verifique selectores, por qué encaja con agentes, y alternativas. |
| `equivalencias-de-herramientas.md` | Qué herramienta puede cumplir cada etapa, incluidas opciones sin costo. |

---

## Convenciones

| Elemento | Formato | Ejemplo | Dónde vive |
|---|---|---|---|
| Código de módulo | `[GRUPO-MOD]` | `VEN-PED` = Ventas › Pedidos | Documento modular, maestro, plan, progress |
| Caso de prueba | `TC-[GRUPO-MOD]-NNN` | `TC-VEN-PED-006` | Plan de pruebas, progress, título del test |
| ID del TMT | El que asigne tu TMT | `Q-1234` en QASE | Plan, progress, anotación del test |
| Notas del documento modular | Etiqueta entre corchetes | `[BUG]` · `[PENDIENTE]` · `[VERIFICAR]` · `[COMPLETAR]` · `[RESUELTO]` | Documento modular |
| Estado de automatización | Emoji + detalle | 📋 pendiente · 🔄 en progreso (`draft`, `fixme`) · ✅ automatizado | Automation progress |

Los códigos de módulo y los TC-ID son permanentes: no se renumeran aunque el contenido cambie.

---

## Agnóstico por diseño

La implementación de referencia usa Playwright + TypeScript, QASE como TMT y Claude Code como agente, porque son las herramientas que prefiero. Cada pieza es reemplazable —Codex, Cursor, OpenCode; TestRail, Xray; Cypress, Selenium— (ver `guias/equivalencias-de-herramientas.md`). Y un detalle importante: **las etapas 1, 2 y 3 son texto.** Se pueden hacer sin IA y sin presupuesto. El agente acelera, pero el valor del método no depende de él.

## Autor, licencia y aportes

Creado por **German Hellweg**, Senior QA Engineer.

Publicado bajo licencia MIT: podés copiar, adaptar y usar las plantillas y prompts en tus proyectos, manteniendo la atribución.

Si aplicaste el método y algo no encajó, abrí un issue contando qué fue: ese feedback es más valioso que un PR.
