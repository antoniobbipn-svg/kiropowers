# Canales Digitales AMX — Kiro Power

Power de Kiro que empaqueta el **estándar de trabajo de Canales Digitales AMX** para que cualquier workspace o proyecto nuevo arranque con las mismas reglas, memoria de arquitectura y sistema de diseño, sin copiar archivos a mano.

Sigue el formato de [Kiro Powers](https://kiro.dev/docs/powers/) (entry point `POWER.md`).

## Qué incluye

| Archivo / Carpeta | Propósito |
|---|---|
| `POWER.md` | Punto de entrada. Frontmatter (nombre, keywords), onboarding (configuración de BD, análisis inicial), mapa de steering y referencia de herramientas MCP. |
| `mcp.json` | MCP MySQL **semilla** con placeholders `${VAR}` (sin credenciales). Servidores `mysql-dev`, `mysql-local`, `mysql-prod`. |
| `steering/00-reglas-de-trabajo.md` | Las 15 reglas de trabajo (Git, modalidades, QA, acceso a BD, frontend/Jenkins/Claro Atlas, PowerShell). `inclusion: always`. |
| `steering/01-arquitectura-semilla.md` | Memoria viva de arquitectura **semilla** (esqueleto a poblar con el análisis del workspace). `inclusion: always`. |
| `claro-atlas-2025/` | Sistema de diseño UX/UI Claro Atlas (contrato, maqueta viva y assets locales: fuentes e íconos). |

## Modelo de uso: distribución vs. vigencia permanente (importante)

Un Power de Kiro se activa **on-demand por keywords** de la conversación; **no** rige de forma incondicional en cada sesión solo por estar instalado. Lo que se carga en **todas** las conversaciones son los **steering de workspace** con `inclusion: always`.

Por eso este Power separa dos roles:
- **Distribución (lo que hace el Power):** empaqueta y transporta el estándar completo, listo para instalar en cualquier workspace.
- **Vigencia permanente (lo que garantiza el "siempre"):** en su **primer uso**, el onboarding del Power **despliega** las reglas y el steering semilla al workspace destino como steering `inclusion: always` (`.kiro/steering/` + `.agents/`). A partir de ahí el estándar rige en cada conversación **aunque el Power no se vuelva a invocar**.

## Instalación

### Desde repositorio GitHub
1. Abre Kiro → panel **Powers**.
2. **Add power from GitHub** y pega la URL del repositorio.
3. Kiro registra el MCP con nombre namespaced y carga los steering por activación.
4. Sigue el onboarding de `POWER.md`: **Paso 1** despliega el estándar como steering siempre activo del workspace; luego configuración de BD y análisis inicial.

### Desde ruta local (para personalizar)
1. Clona o copia esta carpeta.
2. Kiro → panel **Powers** → **Add power from Local Path** → selecciona el directorio.

## Configuración del MCP MySQL (sin credenciales en el Power)

El `mcp.json` del Power **solo tiene placeholders**. Durante el onboarding, el asistente te pedirá los datos de cada BD (host, puerto, usuario, contraseña, base y clasificación de entorno) y los escribirá en el `mcp.json` **del workspace** (`.kiro/settings/mcp.json`), que debe estar en `.gitignore`.

> **Seguridad (Regla 6):** nunca se escriben credenciales reales en este Power ni se suben a Git. Preferir expansión `${VAR}` desde variables de entorno cuando se quiera evitar valores en claro.

## Acceso a base de datos (Regla 14)

- Antes de la primera consulta, el asistente pregunta: VPN conectada y a qué BD conectar.
- **QA / UAT / STAGING / PRODUCCIÓN:** solo lectura (`SELECT`).
- **DEV / LOCAL:** escritura permitida con confirmación explícita.
- Ante duda de clasificación: tratar como entorno sensible (solo lectura).

## Personalización

- `steering/01-arquitectura-semilla.md`: se puebla con el análisis del workspace destino.
- `claro-atlas-2025/`: estándar de diseño; actualízalo si evoluciona la versión del sistema de diseño.
- `POWER.md` (keywords): agrega términos propios del área para afinar la activación del Power.
