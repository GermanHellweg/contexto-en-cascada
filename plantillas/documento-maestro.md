---
titulo: Documento Maestro — [Aplicación]
version: 1.0
fecha: AAAA-MM-DD
estado: vigente
tipo: documentacion-funcional-y-tecnica
---

# Documento Maestro — [Aplicación]

<!--
PLANTILLA — Documento maestro (etapa 1, paso 2 del método Contexto en Cascada).

Qué es: la consolidación de todos los documentos modulares en UNA fuente de
verdad del comportamiento esperado de la aplicación. Es lo que leen el plan de
pruebas y los agentes.

La regla que lo define: SOLO ENTRA LO ACLARADO.
- Cada módulo mantiene las mismas secciones que su documento modular,
  MENOS "Notas o ambigüedades": los hallazgos viven en el documento modular.
- Si un hallazgo fue un [BUG], acá se documenta el comportamiento ESPERADO
  (el corregido), no el defectuoso.
- Si un módulo tiene notas abiertas que cambian el comportamiento, no se
  incorpora todavía.

Orden: el mismo que la navegación de la aplicación (menú / sidebar).
El documento crece módulo a módulo; nunca se escribe de una vez.
-->

## Introducción

<!-- Tres párrafos:
     1. Qué es la aplicación, para quién, qué problema resuelve.
     2. Qué es este documento: referencia única del comportamiento esperado,
        espejo del plan de pruebas, construido cruzando frontend y backend.
     3. Qué NO incluye (los hallazgos, que viven en los documentos modulares)
        y cómo está organizado (bloques y grupos). -->

[Aplicación] es [descripción].

Este documento consolida la documentación funcional y técnica de todos los módulos de la plataforma, en el mismo orden en que aparecen en la navegación. Su propósito es servir como referencia única del comportamiento esperado del sistema para el equipo y como espejo del plan de pruebas. Cada módulo se documenta cruzando el frontend (fuente de verdad de estructura y flujo) con el backend (validaciones, endpoints, permisos y reglas de negocio).

Describe el **comportamiento esperado** de forma limpia. Los hallazgos de QA (bugs, puntos a verificar y pendientes de definición) no se incluyen: viven en cada documento modular (`[ruta]/modulos/`). Está organizado en [n] bloques: **Bloque 0** (transversal / acceso), **Bloque A** (aplicación operativa) y **Bloque B** (administración).

## Glosario

<!-- Términos del dominio, de la aplicación y técnicos que aparecen en el
     documento. Sirve a personas nuevas y evita que un agente interprete
     un término a su manera. -->

| Término | Definición |
|---------|------------|
| **[Término de negocio]** | [Qué significa en esta aplicación.] |
| **[Entidad]** | [Qué representa y cómo se relaciona con otras.] |
| **[Término técnico]** | [Ej.: guard, DTO, cola de trabajos.] |

## Índice de módulos

Módulos incluidos, en orden de navegación:

| Código | Módulo | Sección |
|--------|--------|---------|
| `AUTH` | Autenticación y acceso | [AUTH](#mod-auth) |
| `[GRUPO-MOD]` | [Grupo] › [Módulo] | [[GRUPO-MOD]](#mod-grupo-mod) |

---

# Bloque 0 — Transversal / acceso

<!-- Lo que atraviesa toda la aplicación: autenticación, perfil, layout,
     permisos globales. -->

<a id="mod-auth"></a>

### Módulo: Autenticación y acceso

#### Código de módulo
`AUTH`

#### Descripción funcional
#### Usuarios, permisos y planes
#### Pantallas y vistas
#### Acciones disponibles
#### Validaciones de negocio
#### Estados y transiciones
#### Endpoints relacionados
#### Integraciones
#### Reglas de negocio adicionales

---

# Bloque A — Aplicación operativa

## Grupo: [Grupo del menú]

<a id="mod-grupo-mod"></a>

### Módulo: [Nombre del módulo]

#### Código de módulo
`[GRUPO-MOD]`

<!-- Mismas secciones que el documento modular, sin "Notas o ambigüedades".
     Si una sección no aplica, "No aplica" y por qué. -->

#### Descripción funcional
#### Usuarios, permisos y planes
#### Pantallas y vistas
#### Acciones disponibles
#### Validaciones de negocio
#### Estados y transiciones
#### Endpoints relacionados
#### Integraciones
#### Reglas de negocio adicionales

---

# Bloque B — Administración

## Grupo: [Grupo del menú]

<!-- Back-office, configuración, funciones de super usuario. -->
