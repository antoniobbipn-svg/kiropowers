---
name: canales-digitales-amx
displayName: Canales Digitales AMX · Estándar de Trabajo
description: Estándar de trabajo reutilizable para proyectos de Canales Digitales AMX. Empaqueta las reglas de trabajo (Git, modalidades, QA, acceso a BD, frontend), el steering semilla de arquitectura, el sistema de diseño Claro Atlas y un MCP MySQL semilla con placeholders. Al activarse por primera vez, guía la configuración de la conexión a base(s) de datos y propone el análisis inicial del workspace.
author: Canales Digitales AMX
keywords:
  - canales digitales
  - amx
  - claro atlas
  - estandar de trabajo
  - reglas de trabajo
  - steering
  - mcp mysql
  - base de datos mysql
  - onboarding workspace
  - convenciones de ramas
  - documentacion qa
  - frontend local sin cdn
---

# Canales Digitales AMX — Estándar de Trabajo

Este Power empaqueta el estándar de trabajo de **Canales Digitales AMX** para que cualquier proyecto o workspace nuevo arranque con las mismas reglas, convenciones, memoria de arquitectura y sistema de diseño, sin copiar archivos a mano.

## Qué incluye

| Componente | Propósito |
|---|---|
| `steering/00-reglas-de-trabajo.md` | Reglas de trabajo (Git, aprobación de planes, modalidades, QA, acceso a BD vía MCP, frontend/Jenkins/Claro Atlas, PowerShell). **Siempre activo.** |
| `steering/01-arquitectura-semilla.md` | Memoria viva de arquitectura **semilla** (esqueleto a poblar con el análisis del workspace). **Siempre activo.** |
| `claro-atlas-2025/` | Sistema de diseño UX/UI oficial (contrato `CLARO_ATLAS_AGENT_NATIVE_CSS_v4.md`, maqueta `claro-atlas.html`, assets locales). |
| `mcp.json` | MCP MySQL **semilla** con placeholders `${VAR}` (sin credenciales). Un servidor por entorno: dev, local, prod. |

## Onboarding

Ejecuta estos pasos la **primera vez** que el Power se active en un workspace. Si un paso ya está hecho, omítelo.

### Paso 0: Idioma e intención

Responde siempre en español. Antes de cualquier análisis o cambio, aplica la **Regla 10.1**: si el primer mensaje del usuario no deja clara la modalidad, pregunta en cuál trabajará (EVOLUCIÓN, MANTENIMIENTO, MIGRACIÓN, NUEVO, DUDAS E INVESTIGACIÓN).

### Paso 1: Materializar el estándar como steering SIEMPRE activo del workspace (CRÍTICO)

> **Por qué este paso existe:** un Power de Kiro se activa **on-demand por keywords** de la conversación; **no** rige de forma incondicional en cada sesión por el solo hecho de estar instalado. Lo que sí se carga en **todas** las conversaciones de un workspace son los **steering de workspace** con `inclusion: always`. Por eso, para que las reglas de Canales Digitales AMX rijan **siempre** (sin depender de que el usuario mencione un keyword), el estándar debe **desplegarse** desde el Power hacia el workspace destino.

La **primera vez** que este Power se active en un workspace, y si aún no existe el estándar desplegado (no hay `.kiro/steering/` apuntando a estas reglas ni un `.agents/` del estándar), el asistente debe **proponer al usuario desplegarlo** y, con su visto bueno, crear:

1. `.agents/AGENTS.md` ← copia del contenido de `steering/00-reglas-de-trabajo.md` de este Power (sin el frontmatter de steering).
2. `.agents/steering.md` ← copia de `steering/01-arquitectura-semilla.md` (semilla, para poblar en el Paso 3).
3. `claro-atlas-2025/` ← copia de la carpeta homónima de este Power (estándar de diseño con sus assets).
4. Puentes de Kiro con `inclusion: always` que referencian los `.agents/*.md`:
   - `.kiro/steering/00-reglas-de-trabajo.md` → `#[[file:../../.agents/AGENTS.md]]`
   - `.kiro/steering/01-arquitectura-y-contexto.md` → `#[[file:../../.agents/steering.md]]`
5. Un `.gitignore` en la raíz que proteja `.kiro/settings/mcp.json`, `.env` y credenciales.

A partir de ese despliegue, el workspace queda regido por el estándar en **cada** conversación (vía los steering `inclusion: always`), **aunque el Power no se vuelva a invocar**. El Power cumple así su rol de **distribución/instalación**; los steering de workspace garantizan la **vigencia permanente**.

> Si el estándar ya está desplegado en el workspace (los archivos de arriba existen), **omite este paso** y continúa.

### Paso 2: Configurar el acceso a base(s) de datos (MCP MySQL semilla)

El `mcp.json` de este Power trae **placeholders**, no credenciales. Para habilitar el acceso a BD:

1. Pregunta al usuario **cuántas y cuáles bases de datos** necesita conectar y, por cada una, su **clasificación de entorno** (DEV / LOCAL / QA / UAT / STAGING / PRODUCCIÓN).
2. Por cada BD, solicita: **host**, **puerto** (default `3306`), **usuario**, **contraseña** y **nombre de la base**.
3. Escribe esos valores en el `mcp.json` del workspace (`.kiro/settings/mcp.json`), **un servidor por BD**, con un nombre descriptivo que refleje el entorno (ej. `mysql-dev`, `mysql-prod`). Prefiere expansión de variables de entorno `${VAR}` cuando el usuario quiera evitar credenciales en claro.
4. **Nunca** escribas credenciales reales en el `mcp.json` del Power ni las subas a Git (Regla 6). El `.kiro/settings/mcp.json` debe estar en `.gitignore`.
5. Recuerda al usuario que `.kiro/settings/mcp.json` en Windows suele requerir edición vía PowerShell (`Set-Content`) o la UI de MCP de Kiro.

A partir de ese momento, **toda** interacción con BD se rige por la **Regla 14**:
- Antes de la primera consulta, pregunta (a) si está conectado a la VPN y (b) a qué BD conectar.
- En entornos **QA/UAT/STAGING/PRODUCCIÓN**: solo lectura (`SELECT`). Nada de INSERT/UPDATE/DELETE/DDL.
- En **DEV/LOCAL**: se permite escritura, pero siempre con confirmación explícita (Regla 2).
- Ante duda de clasificación, tratar como entorno sensible (solo lectura).

### Paso 3: Análisis inicial del workspace (Regla 11.2)

Si el steering de arquitectura sigue como **semilla** (secciones vacías, sin análisis previo), tu **primera sugerencia** debe ser proponer al usuario un **análisis completo del workspace** (librerías, microservicios, frontends). Tras su visto bueno:
- Documenta el resultado en el steering de arquitectura (estructura, stacks, bases de datos, flujos, convenciones, gotchas).
- Genera diagramas en **Mermaid**, priorizando el **E-R de la base de datos**, y luego secuencia, flujo, arquitectura, funcionalidad y catálogos según aplique.

### Paso 4: Frontend nuevo (si aplica)

Si el trabajo implica crear un front nuevo, aplica la **Regla 15**: recursos 100% locales (sin CDN), `package.json` para los filtros de seguridad de Jenkins, y el sistema de diseño **Claro Atlas** (`claro-atlas-2025/`), copiando sus assets al proyecto y consumiéndolos localmente.

# Cuándo cargar cada steering

| Intención del usuario | Carga |
|---|---|
| Cualquier interacción (reglas de trabajo siempre vigentes) | `steering/00-reglas-de-trabajo.md` |
| Contexto de arquitectura, servicios, BDs, gotchas del workspace | `steering/01-arquitectura-semilla.md` |
| Crear o rediseñar interfaces/pantallas | `claro-atlas-2025/CLARO_ATLAS_AGENT_NATIVE_CSS_v4.md` |

# Referencia de herramientas MCP (MySQL)

El servidor MySQL (`mysql_mcp_server`) expone:

| Herramienta | Descripción | Cuándo usarla |
|---|---|---|
| `execute_sql` | Ejecuta una sentencia SQL. | Consultas (SELECT) siempre; escritura solo en DEV/LOCAL con confirmación. |
| `get_schema_info` | Devuelve el esquema (tablas, columnas). | Explorar la estructura de la BD. |
| `get_table_sample` | Muestra filas de ejemplo de una tabla. | Entender datos reales antes de consultar. |

Todas quedan sujetas a la Regla 14 (clasificación de entorno y restricción de escritura).

---

## Metadatos del Power

- **Nombre:** canales-digitales-amx
- **Ámbito:** interno Canales Digitales AMX
- **Fuente de verdad del estándar:** `steering/00-reglas-de-trabajo.md` (reglas) y `steering/01-arquitectura-semilla.md` (memoria viva)

### Atribución del servidor MCP

Este Power configura un servidor MCP de MySQL basado en el paquete Python `mysql_mcp_server` (ejecutado con `py -m mysql_mcp_server`). Alternativa vía Node: `npx` del paquete equivalente si el entorno dispone de Node.js.
