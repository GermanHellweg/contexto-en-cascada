---
titulo: Automation Progress — [Aplicación]
version: 1.0
fecha: AAAA-MM-DD
framework: [Playwright + TypeScript]
estado: vigente
tipo: automation-progress
---

# Automation Progress — [Aplicación]

<!--
PLANTILLA — Automation progress (etapa 3 del método Contexto en Cascada).

Qué es: el documento que extrae del plan de pruebas los casos con más valor
de automatizar, los ordena por tier y fase, y lleva el avance de cada uno.
Vive en el repositorio de pruebas y ES EL DOCUMENTO CON EL QUE TRABAJA EL
AGENTE: toma de acá los próximos casos, los automatiza y actualiza su estado.

Se complementa con estandares-de-automatizacion.md, que define CÓMO se
escriben los tests (estructura, trazabilidad, comandos, convenciones).

Mantenelo vivo: estados, bloqueantes y próximas acciones se actualizan
después de cada lote. Un progress desactualizado es peor que no tenerlo,
porque el agente lo va a creer.
-->

## Introducción

<!-- Tres ideas:
     1. De dónde sale (enlace al plan de pruebas, total de casos y módulos) y
        para qué sirve (el agente lo consulta para programar; lleva el avance).
     2. Prioridad ≠ tier.
     3. Cómo se reportan los resultados (TMT, reporter). -->

Este documento organiza la **automatización E2E** de **[Aplicación]** a partir del [plan de pruebas](./plan-de-pruebas.md) ([n] casos en [n] módulos). Es el documento que el agente consulta para programar los tests con **[framework]** y para llevar el avance (📋 pendiente · 🔄 en progreso · ✅ automatizado).

**Prioridad ≠ tier.** La *prioridad* (P0–P3) mide el impacto del caso; el *tier* mide el **orden de automatización** (retorno, criticidad, estabilidad, reutilización). Un caso P0 muy inestable puede diferirse; un smoke P1 transversal puede ir primero. Este documento clasifica por tier y arranca por un **Tier 1** accionable.

**[TMT] es el reporteador.** Cada test referencia su caso (TC-ID en el título) y su ID del TMT, para que el TMT refleje el estado de automatización y los resultados de cada ejecución. Ver `estandares-de-automatizacion.md` › Trazabilidad.

## Resumen

<!-- Lo que un responsable quiere ver primero. Actualizalo en cada lote. -->

| Métrica | Valor |
|---------|-------|
| Total de casos (plan de pruebas) | [n] |
| Casos smoke (todos → Tier 1) | [n] |
| Casos P0 | [n] |
| **Casos en Tier 1 (meta inicial)** | **[n]** |
| Automatizados | [n] |
| % de avance | [n]% |
| Estado | [📋 No iniciado / 🔄 En progreso / ✅ Tier 1 completo] |

> **Meta concreta de Tier 1: [n] casos** = [ej.: todos los smoke + los P0 no-smoke del flujo principal del negocio].

## Criterios de tier

<!-- Adaptá los factores a tu negocio. La primera fila es la más importante:
     ¿qué es lo que, si falla, más le duele a este producto? -->

| Factor | Tier 1 | Tier 2 | Tier 3 |
|--------|--------|--------|--------|
| Criticidad para el negocio | Flujo principal (si falla, no se opera) | Soporte al flujo principal | Sin relación directa |
| Impacto | Bloquea la operación | Afecta el flujo, hay alternativa | Cosmético o aislado |
| Tipo | Smoke + CRUD central | Funcional + validaciones | Bordes, integraciones |
| Frecuencia de uso | Diaria | Regular | Esporádica |
| Alcance del fallo | Varios módulos o todos los usuarios | Un módulo | Aislado |
| Reutilización | Transversales (login, layout, tablas) | Funciones de un módulo | Validaciones puntuales |
| Estabilidad | Interfaz y selectores estables | Mayormente estable | En cambio activo |
| Relación con prioridad | Casi todos P0 | Mezcla P0/P1 | P1 de borde, P2/P3 |

## Decisiones clave

<!-- Las decisiones que condicionan todo el suite, escritas para que nadie
     (ni el agente) las rediscuta. Cada una con su motivo. Ejemplos de
     decisiones típicas: -->

- Los **smoke** son la mayor prioridad de automatización: todos van a **Tier 1**.
- **[Flujo principal]** tiene prioridad máxima aunque sus casos sean funcionales.
- Los módulos **en refactor** no se automatizan hasta estabilizarse.
- Las **integraciones** externas que requieran datos especiales o mocks se **difieren a Tier 3**, salvo el smoke del flujo principal.
- Los casos de **permisos** requieren **usuarios diferenciados** por rol (ver pre-requisitos).
- **Datos:** [ej.: no se crean scripts de limpieza; la suite usa datos existentes de solo lectura + creación con IDs únicos].
- **Exclusiones manuales:** [ej.: la carga de certificados no se automatiza; se valida a mano].
- **Correo:** [ej.: los flujos con código o enlace por email se automatizan leyendo un buzón de prueba].

## Fases de automatización

Dentro de cada fase: primero los smoke, después los funcionales, en orden de navegación.

| Fase | Alcance | Objetivo | Casos Tier 1 |
|------|---------|----------|--------------|
| **Fase 1 — Infraestructura + acceso** | Setup del framework, helpers de login, módulos transversales | Base reutilizable de toda la suite | [n] |
| **Fase 2 — Núcleo del negocio** | [módulos del flujo principal] | Cubrir lo que más importa | [n] |
| **Fase 3 — Resto operativo** | [módulos de consulta, reportes] | Completar el bloque operativo | [n] |
| **Fase 4 — Administración** | [módulos de back-office] | Requiere super usuario | [n] |
| **Fase 5 — Integraciones** | Servicios externos | Cuando haya estrategia de datos o mocks | (Tier 3) |

## Índice de módulos (orden de automatización)

| # | Código | Módulo | Bloque / Grupo | Casos | Tier 1 | Fase |
|---|--------|--------|----------------|-------|--------|------|
| 1 | `AUTH` | Autenticación y acceso | Bloque 0 · Transversal | [n] | [n] | 1 |
| 2 | `[GRUPO-MOD]` | [Módulo] | Bloque A · [Grupo] | [n] | [n] | 2 |
| | | **Total** | | **[n]** | **[n]** | |

## Plan Tier 1 — detalle por módulo

Estado: 📋 pendiente · 🔄 en progreso · ✅ automatizado. El detalle entre paréntesis indica en qué punto está (`draft`, `fixme`, en revisión).

<!-- Una sección por módulo. El encabezado resume fase, cantidad y archivo
     destino. La tabla es lo que el agente lee para saber qué hacer y lo que
     actualiza al terminar. -->

### AUTH — Autenticación y acceso  *(Fase 1 · [n] casos Tier 1 · `transversal/auth.spec.ts`)*

| TC-ID | ID TMT | Título | Tipo | Prioridad | Estado |
|-------|--------|--------|------|-----------|--------|
| TC-AUTH-001 | [Q-NNNN] | Verificar inicio de sesión con credenciales válidas | smoke | P0 | ✅ automatizado |
| TC-AUTH-002 | [Q-NNNN] | Verificar mensaje de error ante credenciales inválidas | functional | P0 | 🔄 draft en `transversal/auth.spec.ts` |
| TC-AUTH-003 | [Q-NNNN] | Verificar restablecimiento de contraseña por enlace | smoke | P0 | 📋 pendiente — requiere buzón de prueba |

### [GRUPO-MOD] — [Módulo]  *(Fase 2 · [n] casos Tier 1 · `[grupo]/[modulo].spec.ts`)*

| TC-ID | ID TMT | Título | Tipo | Prioridad | Estado |
|-------|--------|--------|------|-----------|--------|
| TC-[GRUPO-MOD]-001 | [Q-NNNN] | Crear un pedido con un ítem válido | smoke | P0 | 📋 pendiente |
| TC-[GRUPO-MOD]-006 | [Q-NNNN] | Restringir la anulación a Administrador | functional | P0 | 🔄 `fixme` — falla por bug conocido, se activa al corregirse |

> **Casos extra:** si al construir un spec se automatizan casos fuera de la meta del tier, listalos igual en la tabla con la nota "(extra)". El progress refleja lo que existe, no solo lo planeado.

## Fuera de automatización

<!-- Lo que se decidió NO automatizar, con motivo. Evita rediscutir lo mismo
     en cada release y responde "¿por qué esto no está automatizado?". -->

| TC-ID | Título | Motivo |
|-------|--------|--------|
| TC-[GRUPO-MOD]-NNN | [título] | [ej.: validación visual; se prueba a mano] |

## Pre-requisitos de infraestructura

<!-- Todo lo que tiene que existir antes de automatizar cada fase.
     Cuando algo falta, es un bloqueante: aparece también abajo. -->

| Pre-requisito | Fase | Estado |
|---------------|------|--------|
| Dependencias del framework y del reporter instaladas | 1 | [✅ / 📋] |
| Configuración del framework creada y validada | 1 | [✅ / 📋] |
| Helpers de login por rol | 1 | [✅ / 📋] |
| Variables de ambiente en `.env` (fuera del repositorio) | 1 | [✅ / 📋] |
| Ambiente de pruebas disponible y definido | 1 | [✅ / 📋] |
| Usuarios de prueba diferenciados por rol | 1 / 4 | [✅ / 📋] |
| Plan importado al TMT e IDs volcados | 1 | [✅ / 📋] |
| Datos de prueba para el flujo principal | 2 | [✅ / 📋] |
| Buzón de prueba para flujos con correo | [n] | [✅ / 📋] |

## Estrategia de datos de prueba

<!-- La sección que más problemas evita. Definila con el equipo de desarrollo
     y fechala. Preguntas a responder:
     - ¿Los tests crean sus propios datos o usan datos existentes?
     - ¿Hay limpieza? ¿Por script, por API o por la interfaz?
     - ¿Qué datos no se pueden tocar?
     - ¿Cómo se evitan colisiones entre ejecuciones? -->

Definiciones acordadas con el equipo ([AAAA-MM-DD]):

1. **Enfoque de provisión:** [ej.: datos de solo lectura existentes para consultas; creación dentro del test con identificadores únicos por ejecución (sufijo con timestamp) para los CRUD; borrado solo por la acción nativa de la interfaz cuando el borrado es el caso bajo prueba].
2. **Datos base:** [cuentas, empresas o registros de prueba disponibles y qué se puede hacer con cada uno].
3. **Exclusiones:** [datos o acciones sensibles que no se automatizan].
4. **Usuarios:** [un usuario por rol necesario; qué puede y qué no puede hacer cada uno].
5. **Integraciones (Tier 3):** [sandboxes o credenciales de prueba necesarias más adelante].

> Nunca pongas credenciales, tokens ni datos personales en este documento: van en `.env`, fuera del repositorio.

## Bloqueantes y riesgos

<!-- Separá resueltos de abiertos y fechá cada tanda: el historial muestra
     cuánto trabajo de desbloqueo hubo, que también es trabajo de QA. -->

**Bloqueantes resueltos ([AAAA-MM-DD]):**
- ✅ [bloqueante] — [cómo se resolvió].

**Bloqueantes abiertos:**
- 📋 [bloqueante] — [qué se necesita y de quién].

**Riesgos:**
- [ej.: módulos en refactor fuera de alcance hasta estabilizarse].
- [ej.: integraciones externas: inestabilidad y necesidad de mocks → Tier 3].
- [ej.: selectores en cambio: priorizar roles, labels y test-ids].
- [ej.: sin CI/CD, la suite depende de ejecución manual disciplinada].

## Próximas acciones

<!-- Lo siguiente, en orden. Es lo primero que lee el agente al retomar. -->

**Ya hecho ([AAAA-MM-DD]):** [resumen de lo completado].

**Próximos pasos:**
1. [acción concreta]
2. [acción concreta]
3. [acción concreta]
