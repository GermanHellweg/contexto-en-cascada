# Guía de instalación de Playwright y creación de la rama de trabajo

> Paso a paso para instalar Playwright en un proyecto existente desde cero y crear la rama de trabajo. Los comandos son iguales en Windows, macOS y Linux salvo donde se indica. Al terminar vas a tener Playwright funcionando y una rama lista para empezar la Fase 1 del automation progress.

## Punto de partida

- El repositorio del proyecto clonado en tu máquina.
- Node.js instalado (se verifica en el paso 1).
- Git y acceso al repositorio remoto configurados.

---

## Paso 1 — Verificar Node.js y npm

Abrí una terminal (PowerShell en Windows, la terminal del sistema en macOS o Linux, o la terminal integrada del editor):

    node --version
    npm --version

Deberías ver números de versión. Si aparece "no se reconoce el comando" o "command not found", instalá Node.js desde https://nodejs.org (versión LTS) y volvé a abrir la terminal.

---

## Paso 2 — Posicionarte en el repositorio y actualizar la rama base

    cd ruta/al/repositorio

> En Windows la ruta se escribe con barras invertidas: `cd C:\ruta\al\repositorio`.

Pasate a la rama base del equipo (por ejemplo `develop` o `main`) y traé lo último del remoto:

    git checkout develop
    git pull origin develop

Así la rama nueva sale de la versión más reciente.

---

## Paso 3 — Crear la rama de trabajo

    git checkout -b feature/playwright

Verificá que quedaste en ella:

    git branch

La rama activa aparece con un asterisco.

---

## Paso 4 — Instalar las dependencias del proyecto

    npm install

Lee el `package.json` y descarga todo a `node_modules`. La primera vez puede tardar.

---

## Paso 5 — Instalar Playwright y sus dependencias

Playwright, el reporter del TMT y dos utilidades:

    npm install --save-dev @playwright/test playwright-qase-reporter dotenv cross-env

| Paquete | Para qué |
|---------|----------|
| `@playwright/test` | El framework. |
| `playwright-qase-reporter` | Envía los resultados a QASE. Reemplazalo por el reporter de tu TMT, o quitalo si no usás uno. |
| `dotenv` | Lee las variables del archivo `.env`. |
| `cross-env` | Define variables de entorno igual en todos los sistemas operativos. |

Después, descargá los navegadores que usa Playwright:

    npx playwright install

Sin este paso los tests no corren. Si tu configuración solo usa Chromium, alcanza con `npx playwright install chromium`.

> **Linux:** si faltan librerías del sistema, `npx playwright install --with-deps` las instala junto con los navegadores.

---

## Paso 6 — Verificar o crear `playwright.config.ts`

Fijate si ya existe `playwright.config.ts` en la raíz del proyecto (puede haberlo creado el agente como parte de la Fase 1).

- **Si existe**, no lo toques por ahora: se valida en el paso 7.
- **Si no existe**, generá la estructura base:

      npm init playwright@latest

  Respuestas recomendadas:
  - ¿TypeScript o JavaScript? → **TypeScript**
  - ¿Dónde poner los tests? → la carpeta definida en los estándares (por ejemplo `tests/e2e`)
  - ¿Agregar workflow de GitHub Actions? → según tu proyecto; **No** si todavía no hay CI
  - ¿Instalar navegadores? → **Sí**, si no lo hiciste en el paso 5

> Si los estándares de automatización ya definen la estructura de carpetas, alineá esta configuración con ellos para no duplicar carpetas.

---

## Paso 7 — Agregar los scripts y verificar que Playwright corre

Agregá los scripts de ejecución al `package.json` (definidos en los estándares de automatización):

```json
"scripts": {
  "pw:test": "cross-env QASE_MODE=testops playwright test",
  "pw:test:qa": "playwright test",
  "pw:ui": "playwright test --ui",
  "pw:headed": "playwright test --headed",
  "pw:report": "playwright show-report"
}
```

Abrí el modo interactivo:

    npm run pw:ui

Si se abre la interfaz de Playwright, aunque todavía no haya tests, la instalación está bien.

Si falla por una variable de entorno faltante, es esperable en esta etapa: Playwright está instalado y falta la configuración de ambiente (`helpers/env.ts` y `.env`), que se crea en la Fase 1. Lo importante acá es que Playwright se ejecute.

---

## Paso 8 — Primer commit

    git status

Vas a ver cambios en `package.json`, `package-lock.json` y posiblemente `playwright.config.ts` y una carpeta de tests.

> **Antes de commitear**, revisá el `.gitignore`. Tienen que estar ignorados: `node_modules/`, `test-results/`, `playwright-report/`, `playwright/.cache/` y **el archivo `.env`**. Las credenciales nunca se commitean.

    git add .
    git commit -m "chore: instalar y configurar Playwright para automatización E2E"

---

## Paso 9 — Publicar la rama

    git push -u origin feature/playwright

El `-u` vincula la rama local con la remota: a partir de ahí alcanza con `git push` y `git pull`.

---

## Resumen del flujo

    1. node --version / npm --version            → verificar Node
    2. cd ruta/al/repositorio                     → entrar al repo
       git checkout develop
       git pull origin develop
    3. git checkout -b feature/playwright         → crear la rama
    4. npm install                                → dependencias del proyecto
    5. npm install --save-dev @playwright/test playwright-qase-reporter dotenv cross-env
       npx playwright install                     → Playwright + navegadores
    6. (verificar o crear playwright.config.ts)
    7. agregar scripts pw:* y npm run pw:ui       → verificar que corre
    8. git add . / git commit                     → primer commit
    9. git push -u origin feature/playwright      → publicar la rama

---

## Qué sigue

Con Playwright instalado y la rama creada, el siguiente paso es la **Fase 1 del automation progress**: la infraestructura base (`helpers/auth.ts` con el login por rol, `helpers/env.ts` con URLs y variables, `.env` y `.env.example`). A partir de ahí se automatizan los módulos en orden de fase.

## Problemas comunes

- **"npx no se reconoce":** Node no está bien instalado o la terminal no se reinició después de instalarlo. Cerrala y abrila de nuevo.
- **Los navegadores no descargan:** reintentá `npx playwright install`. Detrás de un proxy corporativo puede hacer falta configuración de red.
- **`pw:ui` no abre:** verificá que `playwright.config.ts` exista y sea válido. Si el error menciona una variable de entorno, es la configuración de ambiente pendiente de la Fase 1.
- **El push es rechazado:** revisá que tu acceso (SSH o token) esté vigente y que tengas permiso de escritura en el repositorio.
