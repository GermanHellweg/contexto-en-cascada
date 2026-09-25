# Etapa 4 — Generación asistida por agente

## Qué cambia respecto de pedirle tests a un chat

Todo. En esta capa el agente no recibe una descripción: recibe **fuentes de verdad ya construidas**, y trabaja sobre el automation progress hasta completarlo.

| Fuente | Qué le aporta |
|---|---|
| Automation progress | Qué casos tocan ahora, en qué orden, y dónde anotar el avance. |
| Casos (del TMT o del plan) | Qué hay que verificar, con su ID real. |
| Estándares y contexto del módulo | Reglas del suite, helpers, roles, trampas conocidas. |
| Documento maestro | Qué hace el sistema y bajo qué reglas. |
| La aplicación en ejecución | El DOM real: selectores que existen de verdad. |

Con eso, la tarea del agente deja de ser creativa y pasa a ser de traducción. Traducir es algo que los modelos hacen muy bien; inventar reglas de negocio no.

## El rol de los MCP

Dos conexiones cambian cualitativamente el resultado, porque eliminan los dos puntos donde el agente suele alucinar.

### TMT (MCP de QASE u equivalente)

El agente lee los casos directamente del gestor y recupera sus ID. El spec generado queda ligado al caso desde que nace, sin que nadie copie identificadores a mano.

Esto resuelve el problema clásico de la trazabilidad: no es que a los equipos no les importe, es que mantenerla manualmente es tedioso y por eso se abandona. Cuando la traza se escribe sola, sobrevive.

Si no se puede instalar el MCP, se exportan los casos y el agente matchea los IDs. Las dos opciones y el reporter que devuelve los resultados al TMT están en `guias/tmt-e-ids.md`.

### Navegador (MCP de Playwright u equivalente)

El agente abre la aplicación, navega, inspecciona el DOM y verifica que los selectores existan antes de escribirlos. Sin esto, los localizadores son la principal fuente de specs generados que no corren.

Configuración en `guias/mcp-playwright.md`.

## El ciclo de generación

```
1. Tomar los próximos casos 📋 pendiente del automation progress
2. El agente lee: casos + estándares + contexto de módulo + documento maestro
3. El agente recorre la aplicación y confirma selectores
4. Genera los specs, con la referencia al ID de caso en cada uno
5. Ejecuta los specs generados
6. Corrige lo que falla — y clasifica por qué falló
7. Revisión humana y PR
8. Se actualiza el automation progress y lo aprendido vuelve al contexto del módulo
9. Siguiente lote, hasta completar el progress
```

El paso 8 es el que hace que el segundo lote salga mejor que el primero, y el quinto mejor que el segundo. Sin él, el agente repite los mismos errores indefinidamente.

## Tamaño del lote

Generar de a pocos casos por vez, no el módulo entero. Razones:

- Un lote chico se revisa de verdad; uno grande se aprueba sin leer.
- Los errores sistemáticos se detectan temprano, antes de multiplicarse por cuarenta.
- El contexto del agente se mantiene manejable y la calidad no se degrada.

Entre cinco y diez casos por lote es un punto razonable para empezar.

## Qué revisa el humano

No la sintaxis — para eso están el linter y la ejecución. Lo que se revisa es:

1. **¿El spec verifica lo que el caso dice?** El error más peligroso es un test verde que verifica otra cosa.
2. **¿Las aserciones son significativas?** Un `expect(page).toHaveURL()` no prueba que la reserva se canceló.
3. **¿Reutiliza lo que existe?** O generó su propio helper paralelo a uno que ya estaba.
4. **¿Es aislado?** ¿Corre solo, en paralelo, dos veces seguidas?
5. **¿La traza al caso está puesta y es correcta?**

## Límite honesto

El agente acelera la producción de specs. No decide qué probar, no evalúa riesgo y no reemplaza la revisión. Un equipo que usa esta capa sin haber construido las tres anteriores va a producir más tests malos, más rápido.

## Ventajas de esta capa

- **Velocidad sin perder criterio.** El agente traduce casos a código; el criterio ya quedó tomado en las capas anteriores.
- **Trazabilidad automática.** Con el MCP del gestor, el ID del caso entra al spec sin que nadie lo copie.
- **Selectores reales.** Con el MCP del navegador, el agente verifica el DOM antes de escribir.
- **Mejora con el uso.** Cada lote actualiza el contexto del módulo y el siguiente sale mejor.
- **Los fallos informan.** Clasificados según el análisis de fallo, un test rojo dice si el problema está en el producto, en el suite o en la documentación.
