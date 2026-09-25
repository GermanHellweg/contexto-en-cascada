---
titulo: Estándares de Automatización — [Aplicación]
version: 1.0
fecha: AAAA-MM-DD
framework: [Playwright + TypeScript]
tipo: estandares-de-automatizacion
---

# Estándares de Automatización — [Aplicación]

<!--
PLANTILLA — Estándares de automatización (etapa 3 del método Contexto en Cascada).

Qué es: las reglas de CÓMO se escriben los tests en este proyecto. Se definen
una vez, según el proyecto, y el agente las respeta en cada spec que genera.
El QUÉ y el CUÁNDO están en automation-progress.md.
Lo específico de cada módulo va en su archivo de contexto (contexto/[modulo].md).
-->

## 1. Estructura de archivos

<!-- Recomendado: espejo del menú de la aplicación, un spec por módulo,
     en el mismo orden que el documento maestro y el plan. Marcá con ✅ lo
     creado y con 📋 lo previsto: el árbol también muestra el avance. -->

Estructura **espejo del menú**, un `.spec.ts` por módulo. Los tests E2E viven en `tests/e2e/`, separados de los tests unitarios existentes.

```
tests/e2e/
├── transversal/
│   ├── auth.spec.ts              ✅
│   └── layout.spec.ts            📋
├── [grupo]/
│   └── [modulo].spec.ts          📋
├── administracion/               📋
└── helpers/
    ├── auth.ts                   ✅  login por rol
    ├── env.ts                    ✅  URLs y variables
    └── email.ts                  📋  lectura de buzón de prueba
```

## 2. Trazabilidad con el TMT

<!-- Cómo cada test lleva su caso. Es lo que permite que los resultados
     lleguen solos al TMT. Documentá el mecanismo exacto: un detalle mal
     puesto (un tipo de anotación equivocado) rompe el enlace sin avisar. -->

- Cada `test(...)` lleva su **TC-ID** en el título y su **ID del TMT** en una anotación.
- Con `playwright-qase-reporter`, la anotación que enlaza es `type: 'QaseID'`:

```ts
test('TC-[GRUPO-MOD]-001 — Crear un pedido con un ítem válido', async ({ page }) => {
  test.info().annotations.push({ type: 'QaseID', description: '[NNNN]' });
  // ...
});
```

- El reporter se activa solo cuando la variable `QASE_MODE=testops` está presente. Sin ella, la suite corre con el reporte nativo. Publicar en el TMT es opcional y nunca bloquea la ejecución local.
- Casos nuevos todavía sin ID en el TMT: se marcan `[ID-PENDIENTE]` en el progress hasta importarlos.

## 3. Comandos de ejecución

<!-- Los scripts del package.json. Nombres cortos y consistentes. -->

| Script | Reporte | Uso |
|--------|---------|-----|
| `npm run pw:test` | TMT + nativo | Publica resultados en el TMT y genera el HTML |
| `npm run pw:test:qa` | Solo nativo | Ejecución normal, sin publicar |
| `npm run pw:ui` | Nativo | Modo interactivo |
| `npm run pw:headed` | Nativo | Con navegador visible |
| `npm run pw:report` | — | Abre el último reporte HTML |

```json
"scripts": {
  "pw:test": "cross-env QASE_MODE=testops playwright test",
  "pw:test:qa": "playwright test",
  "pw:ui": "playwright test --ui",
  "pw:headed": "playwright test --headed",
  "pw:report": "playwright show-report"
}
```

> `cross-env` permite definir variables de entorno igual en Windows, macOS y Linux.

## 4. Ambiente y configuración

- **Ambiente de pruebas:** [URL], definido como valor por defecto en `helpers/env.ts`. [Si hay varios, cómo se elige.]
- **Variables:** en `tests/e2e/.env`, **fuera del repositorio** (en `.gitignore`), con un `.env.example` versionado que lista los nombres sin valores.
- **Nunca** credenciales, tokens ni datos personales en el código ni en los documentos.

## 5. Convenciones de código

- **Localizadores:** por rol y texto visible primero (`getByRole`, `getByLabel`); `data-testid` cuando no alcanza. Nunca selectores CSS frágiles por posición.
- **Esperas:** nunca fijas. Si hace falta una, falta un estado observable en la interfaz: reportarlo.
- **Aserciones:** sobre el resultado observable del comportamiento, no solo sobre la navegación.
- **Aislamiento:** cada test monta su precondición y puede correr solo y en paralelo.
- **Helpers:** reutilizar los existentes; proponer uno nuevo antes de crearlo.
- **Tests pendientes de un bug:** `test.fixme()` con el motivo, y el caso queda 🔄 en el progress.

## 6. Contextos por módulo

| Módulo | Archivo |
|--------|---------|
| [GRUPO-MOD] | `contexto/[modulo].md` |

## 7. Mantenimiento

- **Tests inestables:** [criterio para estabilizar o quitar del suite].
- **Clasificación de fallos:** [quién y cuándo] — ver `metodo/05-categorias-y-analisis-de-fallo.md`.
- **Revisión de estándares:** [frecuencia].
