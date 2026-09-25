# MCP del navegador (Playwright)

## Para qué sirve en este método

Para que el agente **vea la aplicación real** antes de escribir un selector. Sin esta conexión, los localizadores salen de la imaginación del modelo y son la principal causa de specs generados que no corren.

Con la conexión, el ciclo cambia: el agente navega, inspecciona la estructura de la página, confirma que el elemento existe y recién entonces escribe el localizador.

## Playwright en cuatro comandos

| Comando | Para qué |
|---|---|
| `npm init playwright@latest` | Instala Playwright, los navegadores y un proyecto de ejemplo. |
| `npx playwright codegen <url>` | Abre la aplicación y graba las interacciones como código. Útil para relevar pantallas complejas. |
| `npx playwright test --ui` | Ejecuta los tests en modo interactivo: se ve cada paso, se puede pausar y repetir. |
| `npx playwright show-report` | Abre el reporte HTML, con el trace de cada fallo para diagnosticarlo. |

## Instalación del MCP

El servidor MCP de Playwright se instala como paquete de npm y se declara en la configuración de MCP del cliente que uses. En Claude Code se agrega como servidor local; en OpenCode va en la sección `mcp` de su configuración; en otros clientes, en el archivo equivalente.

Consultá la documentación oficial de Playwright para el comando exacto y las opciones disponibles: la configuración cambia entre versiones.

## Cómo usarlo bien

- **Apuntá siempre a un ambiente de pruebas.** El agente va a hacer clics, cargar formularios y crear datos.
- **Usuarios dedicados.** Una cuenta por rol, para el agente, separada de las que usan las personas.
- **Relevamiento antes de generar.** Conviene un paso explícito de recorrido de la pantalla antes de pedir specs; el resultado de ese relevamiento alimenta el archivo de contexto del módulo.
- **Si un selector del contexto ya no existe, eso es un hallazgo.** Puede ser un cambio de UI no comunicado. Que el agente avise en vez de improvisar un reemplazo.

## Por qué Playwright encaja bien con agentes

Estas son las razones para la elección, útiles también para la charla:

| Característica | Por qué importa en un flujo con agente |
|---|---|
| Espera automática | Menos inestabilidad por timing, y por lo tanto un test rojo es más probablemente un bug real. Eso hace confiable el análisis de fallo. |
| Locators semánticos (rol, texto, label) | El código generado se lee casi como el caso de prueba, y un humano lo revisa rápido. |
| Aislamiento por contexto de navegador | Tests independientes y paralelizables por diseño, que es justo lo que el método pide. |
| Trace viewer | Diagnóstico visual del fallo, clave para clasificarlo bien. |
| Codegen | Punto de partida para relevar interacciones complejas. |
| MCP oficial | El agente ve el DOM real en vez de suponerlo. |
| Un solo proyecto para UI y API | Las precondiciones se montan por API, que es la forma correcta. |

## Alternativas

**Cypress** tiene buena experiencia de desarrollo y comunidad grande; su modelo de ejecución hace más incómodo el manejo de múltiples pestañas y orígenes. **Selenium** es el estándar de mayor alcance en lenguajes y navegadores, con más código propio para llegar al mismo lugar. **Robot Framework** es fuerte donde el equipo no programa.

El método funciona con cualquiera de los cuatro. Lo que cambia es cuánto trabajo queda del lado humano.
