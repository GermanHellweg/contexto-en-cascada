# Contexto de módulo — [MÓDULO]

> Complementa los estándares de automatización con lo específico de este módulo.

<!--
PLANTILLA — El artefacto que lee el agente antes de generar specs.
Todo lo específico de este módulo que no está en la documentación general.
Se actualiza DESPUÉS de cada lote de generación con lo que se aprendió.
-->

## Referencias

- **Documento maestro:** [ruta del archivo]
- **Casos en el gestor:** [proyecto / suite / filtro]
- **Specs existentes:** [ruta de la carpeta]

## Navegación

| Pantalla | Ruta | Cómo se llega |
|---|---|---|
| [nombre] | `/ruta` | [desde dónde] |

## Roles y autenticación

| Rol | Usuario de prueba | Cómo autenticar |
|---|---|---|
| [rol] | [usuario] | [storage state / helper / login] |

## Selectores y convenciones

- **Estrategia de localización:** [role / test-id / label]
- **Atributo de test-id:** [`data-testid`]
- **Selectores establecidos del módulo:**

| Elemento | Localizador |
|---|---|
| [elemento] | [localizador] |

## Datos de prueba

- **Cómo se crean:** [seed / factory / API]
- **Datos fijos disponibles:** [lista]
- **Datos que NO se deben modificar:** [lista]

## Helpers y page objects existentes

<!-- Para que el agente reutilice en vez de escribir paralelos. -->

| Helper | Ruta | Qué hace |
|---|---|---|
| [nombre] | [ruta] | [función] |

## Trampas conocidas de este módulo

<!-- LA SECCIÓN MÁS VALIOSA DEL ARCHIVO.
     Acá se acumula lo aprendido a los golpes. Cada vez que un spec generado
     falla por algo específico del módulo, se agrega una línea acá. -->

- [Ej.: la grilla se renderiza en dos pasos; esperar al estado final, no al primer render.]
- [Ej.: el endpoint de búsqueda tarda; no reducir el timeout por defecto en esta pantalla.]
- [Ej.: no usar el usuario admin@demo para tests de escritura: lo comparten otros suites.]

## Qué NO hacer en este módulo

- [restricción]
