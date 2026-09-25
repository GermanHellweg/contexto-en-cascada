# TMT, IDs y reportes

## Por qué importa

Cuando el plan de pruebas se exporta a un test management tool (TMT), el TMT le asigna a cada caso **su propio ID**. Si ese ID llega a los specs, al ejecutar la automatización los resultados vuelven solos al TMT, caso por caso, y el reporte de ejecución se arma sin trabajo manual.

```
Plan de pruebas (MD) ──exportar──► TMT asigna IDs
        ▲                              │
        └──────── los IDs vuelven ─────┘
                       │
                       ▼
     Automation progress y specs llevan el ID
     (TC-ID en el título, ID del TMT en una anotación)
                       │
                  ejecución
                       ▼
     El reporter envía los resultados al TMT
```

La trazabilidad manual no se abandona por desinterés: se abandona porque es tediosa. Cuando el ID viaja solo, sobrevive.

> Uso QASE porque es la herramienta que prefiero. TestRail, Xray, Zephyr o cualquier TMT con API sirven igual.

## Dos formas de traer los IDs

### Opción A — MCP del TMT

El agente se conecta al TMT, lee los casos y sus IDs directamente de la fuente, y los escribe en el plan, en el automation progress y en cada spec.

- **Ventaja:** siempre al día, sin pasos intermedios. El agente también puede consultar ejecuciones previas.
- **Requiere:** poder instalar y configurar el servidor MCP del TMT.

**QASE** publica un servidor MCP oficial. Hay dos variantes: una alojada por Qase, que se conecta con la cuenta vía OAuth sin token (disponible en el plan Enterprise, en su infraestructura cloud estándar), y una autogestionada con token de API propio, que funciona en cualquier plan. La documentación del repositorio oficial cubre la configuración para Claude Code, Cursor, Codex y OpenCode.

> Verificá contra la documentación vigente: planes, endpoints y nombres de herramientas cambian.

### Opción B — Exportar y matchear

Se exportan los casos del TMT (CSV, JSON o lo que el TMT permita) y se le pide al agente que matchee cada caso con su equivalente en el plan de pruebas en Markdown y anote el ID.

```
Te paso la exportación de casos del TMT: [ruta].
Matcheá cada caso con su equivalente en el plan de pruebas [ruta],
por título y pasos, y anotá el ID del TMT en el campo "ID en el TMT".

Si un caso no tiene equivalente claro, o matchea con más de uno,
NO lo asignes: listalo aparte para que lo resuelva yo.
```

- **Ventaja:** no requiere instalar nada. Sirve con cualquier TMT.
- **Requiere:** repetir la exportación cuando se agregan casos, y revisar los matcheos dudosos.

## El reporte de ejecución

Con los IDs en los specs, el último eslabón es el reporter: un plugin del framework que, al terminar la ejecución, envía el resultado de cada test al TMT usando su ID.

Para Playwright y QASE existe `playwright-qase-reporter`. Cada test lleva su TC-ID en el título y su ID de QASE en una anotación de tipo `QaseID`, que es la que el reporter usa para enlazar el resultado con el caso:

```ts
test('TC-VEN-PED-001 — Crear un pedido con un ítem válido', async ({ page }) => {
  test.info().annotations.push({ type: 'QaseID', description: '1234' });
  // ...
});
```

> Ojo con el tipo de la anotación: con otro valor el test corre igual, pero el resultado no se enlaza al caso y nadie se entera.

Conviene que publicar en el TMT sea **opcional**: el reporter se activa solo cuando está definida la variable `QASE_MODE=testops`, y sin ella la suite corre con el reporte nativo. Así una ejecución local nunca depende del TMT. Scripts sugeridos en `plantillas/estandares-de-automatizacion.md`.

Otros TMT tienen reporters equivalentes. Si no hay, la API del TMT y un paso en el CI cumplen la misma función.

## Higiene mínima

- **Tokens con el menor alcance posible**, fuera del repositorio, en variables de entorno.
- **Ambiente de pruebas antes que producción**, sobre todo si el agente tiene permiso de escritura.
- **La creación masiva de casos se revisa.** Un agente con permiso de escritura y un prompt ambiguo puede llenar un proyecto de duplicados en un minuto.

## Sin TMT

El plan de pruebas en Markdown dentro del repositorio funciona: el agente lo lee como cualquier otro archivo y el ID local del caso cumple la función del ID del TMT. Se pierde el historial y el reporte centralizado, no el método.
