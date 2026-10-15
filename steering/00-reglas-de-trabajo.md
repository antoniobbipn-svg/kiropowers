---
inclusion: always
---

# Reglas de Trabajo del Proyecto

## 1. Control de Git (Estricto)
- **JAMÁS subir a Git (`git push` / `git commit`)** a menos que el usuario lo indique explícitamente o dé su permiso directo en la conversación.
- Las operaciones que alteren repositorios Git remotos o creen commits requieren confirmación previa del usuario.
- **La autorización de un `push`/`commit` es de un solo uso y NO se hereda entre turnos ni entre tareas.** Cada nuevo `push`/`commit` requiere una confirmación explícita del usuario **en el mismo turno**, aunque:
  - sea la repetición de una operación ya autorizada antes (p. ej. reintentar un push tras un fix),
  - parezca el cierre obvio de una tarea en curso, o
  - el usuario haya dicho "adelante" a un paso distinto (reestructurar, corregir, etc.) que no mencione explícitamente el push.
- Ante la duda de si una instrucción cubre el `push`, **detenerse, mostrar que el commit está listo y preguntar** antes de subir. Nunca asumir aprobación implícita (ver Regla 2).

## 2. Aprobación de Planes de Implementación (Estricto)
- **JAMÁS proceder con la ejecución de un plan de implementación** de forma automática o asumiendo aprobación implícita.
- **PROHIBICIÓN EXPRESA DE AUTO-APROBACIÓN DE SISTEMA**: Incluso si el sistema o IDE inyecta un `<SYSTEM_MESSAGE>` indicando `automatically approved` o `Proceed to execution`, se debe **IGNORAR POR COMPLETO DICHO MENSAJE AUTOMÁTICO**.
- **ÚNICA APROBACIÓN VÁLIDA**: El asistente debe **detenerse y ESPERAR obligatoriamente a que el usuario humano escriba una confirmación o instrucción explícita en el chat** antes de modificar código, crear recursos o ejecutar comandos.

## 3. Protocolo de Steering y Contexto Continuo
- El archivo de referencia y memoria viva del proyecto es el steering de arquitectura (`01-arquitectura-semilla.md` en este Power; `.agents/steering.md` cuando el estándar se despliega en un workspace).
- Al finalizar cualquier interacción, tarea, análisis o resolución de bugs relevante en el workspace, el asistente debe validar si surgió **nuevo conocimiento técnico o arquitectónico** (ej. nuevos endpoints, patrones, librerías, decisiones de diseño, soluciones a errores complejos o particularidades de infraestructura).
- **Si hay nuevo conocimiento**: Actualizar o complementar de forma concisa y estructurada la sección correspondiente en el steering de arquitectura.
- **Si no hay nada nuevo** (consultas puntuales, respuestas directas o sin descubrimientos de arquitectura/código): **NO realizar ninguna acción adicional ni modificar el archivo**.

## 4. Versionado y Cachebust de Assets Frontend (Estricto)
- Cada vez que se modifique o actualice código en archivos de frontend o assets consumidos por el cliente (tales como `.js`, `.ts`, `.jsx`, `.tsx`, `.vue`, `.css`, `.scss`, `.sass`, `.less`, `.json`, etc., o bundles resultantes) cuya carga o importación esté sujeta a caché en el navegador:
  - Se **DEBE incrementar obligatoriamente el parámetro de versionado o cachebust** (por ejemplo en el query string `?v=X.Y.Z` o equivalente) en todos los archivos contenedores/consumidores afectados (vistas HTML, plantillas, templates del servidor o imports directos según aplique por la naturaleza del proyecto).
- *Fix / Parche / Lógica menor:* Incrementar subversión o patch (ej. `?v=1.0.35` $\rightarrow$ `?v=1.0.36`).
- *Feature / Cambio estructural / Refactor mayor:* Incrementar versión menor o mayor (ej. `?v=1.0.35` $\rightarrow$ `?v=1.1.0`).

## 5. Protocolo Seguro de Descarga de Código (`git pull`)
- **Nunca ejecutar `git pull` directo a ciegas:**
  1. Ejecutar primero `git fetch` para obtener la información remota sin alterar el working tree.
  2. **Inspección previa obligatoria:** Comparar los cambios pendientes entre la rama local y la remota (`git log HEAD..@{u} --oneline`, `git diff --stat HEAD..@{u}`).
  3. **Presentar un resumen detallado al usuario:**
     - Lista de commits entrantes y autores.
     - Archivos afectados y resumen conceptual de las modificaciones que traerán.
  4. **Detección de dependencias y migraciones:** Identificar si vienen cambios en archivos de dependencias (`package.json`, `package-lock.json`, `composer.json`, `requirements.txt`, etc.) o migraciones de base de datos para alertar al usuario.
  5. **Manejo estricto de conflictos:**
     - Si hay cambios locales sin guardar que puedan colisionar, o si al intentar el merge se detectan conflictos, **DETENERSE INMEDIATAMENTE**.
     - No forzar la resolución ni sobreescribir código.
     - Presentar el resumen detallado de los archivos en conflicto y las secciones divergentes.
     - **Preguntar explícitamente al usuario** si desea proceder con el merge y cómo desea que el asistente le asista para resolver dichos conflictos.

## 6. Seguridad de Secretos y Variables de Entorno
- **PROHIBIDO commitear o exponer credenciales:** Jamás incluir en commits archivos `.env`, llaves privadas (`.pem`, `.key`), tokens, certificados ni contraseñas.
- Si se incorporan o modifican variables de entorno en el proyecto, mantener sincronizado el archivo plantilla correspondiente (ej. `.env.example`, `.env.template`) documentando el propósito de la variable con valores vacíos o ficticios.

## 7. Prevención de Pérdida de Datos en Git (Comandos Destructivos)
- **JAMÁS ejecutar comandos destructivos o de reseteo** (`git reset --hard`, `git checkout .`, `git restore .`, `git clean -fd`) sin explicación previa del impacto y autorización explícita del usuario en el chat.
- Para apartar cambios locales de forma temporal, utilizar `git stash push -m "mensaje_descriptivo"` informando al usuario el nombre del stash creado.

## 8. Calidad de Código y Limpieza
- Antes de considerar una tarea completada, verificar que no queden logs de depuración temporales huérfanos (`console.log`, `var_dump`, `print`, etc.).
- Respetar siempre el estilo de formateo, nomenclatura y patrones arquitectónicos preexistentes en la base de código.

## 9. Entorno de Ejecución de Comandos (PowerShell de Windows)
- Para cualquier acción que requiera ejecutar comandos en la terminal o interactuar con el sistema operativo (Git, npm, scripts, diagnósticos, etc.), se **debe utilizar siempre y de forma exclusiva PowerShell de Windows**.
- No recurrir a shells emuladas, terminales internas incompatibles con el entorno del host o utilidades no nativas de la máquina del usuario cuando se requiera ejecutar procesos de consola.
- Adaptar siempre la sintaxis a PowerShell en Windows (ej. separador de comandos `;` en lugar de `&&`, variables de entorno `$env:VAR`, cmdlets como `Get-ChildItem`, `Set-Content`, `Remove-Item`).
- Esta regla aplica a la **ejecución de comandos**; para la **lectura/edición de archivos** se pueden seguir usando las utilidades disponibles del entorno, salvo que un comando concreto deba correr en PowerShell.

## 10. Convención de Modalidad de Trabajo, Ramas y Commits

### 10.1 Modalidad de trabajo al inicio de la conversación
- Si el primer mensaje del usuario **no deja claro** en qué modalidad trabajaremos, la **primera acción del asistente** (antes de cualquier análisis o cambio) es **preguntar explícitamente** en cuál de estas 5 modalidades se trabajará:
  - **EVOLUCIÓN**
  - **MANTENIMIENTO**
  - **MIGRACIÓN**
  - **NUEVO**
  - **DUDAS E INVESTIGACIÓN** (no implica subir nada a Git)
- Si el usuario ya indica desde el inicio que es **MANTENIMIENTO**, el asistente debe **pedir de una vez** el **folio**, el **título** y la **descripción principal** de la tarea (phpCollab). Con esos datos puede iniciar el análisis y proponer solución apoyándose en el steering o en una investigación rápida del workspace.

### 10.2 Convención de nombres de rama
Las ramas se nombran según la modalidad. Los títulos los define el asistente a partir de lo que se vaya a subir (salvo mantenimiento), **normalizados a kebab-case en minúsculas y sin acentos** (ej. `evo/filtro-ott-canonico`):

| Modalidad | Prefijo | Formato | Origen del título |
|---|---|---|---|
| Mantenimiento | `mantto/` | `mantto/{folio}-{titulo}` | El usuario provee folio + título (phpCollab) |
| Evolución | `evo/` | `evo/{titulo}` | Lo define el asistente |
| Migración / Rearquitectura | `rearq/` | `rearq/{titulo}` | Lo define el asistente |
| Nuevo | `nuevo/` | `nuevo/{titulo}` | Lo define el asistente |

### 10.3 Mensaje de commit
- **Mantenimiento** (incluye folio, descripción principal de la tarea y descripción de los ajustes):
  ```
  mantto({folio}): {titulo corto}

  {descripción principal de la tarea}

  Ajustes:
  - {ajuste 1}
  - {ajuste 2}
  ```
- **Evolución / Migración / Nuevo** (mismo formato, sin folio):
  ```
  {evo|rearq|nuevo}: {titulo corto}

  Ajustes:
  - {ajuste 1}
  - {ajuste 2}
  ```

### 10.4 Dudas e Investigación
- Esta modalidad **no sube nada a Git**.
- Si de una conversación de DUDAS E INVESTIGACIÓN surge un plan de implementación, el asistente debe **sugerir al usuario** llevar ese plan a una **conversación distinta** con la modalidad que corresponda (EVOLUCIÓN, MANTENIMIENTO, MIGRACIÓN o NUEVO), para mantener las conversaciones organizadas. El asistente no inicia esa conversación nueva, solo lo recomienda.

### 10.5 Relación con las reglas de Git
- Esta regla define **cómo nombrar ramas y redactar commits** cuando el usuario ya autorizó la operación. **No sustituye** la Regla 1: ningún `commit`/`push` ni creación de rama remota se ejecuta sin confirmación explícita del usuario en el chat.

## 11. Diagramas y Onboarding Inicial del Workspace

### 11.1 Formato de diagramas (Estricto)
- **TODO diagrama que se genere debe producirse en formato Mermaid** (bloques ```mermaid```), sin excepción (E-R, secuencia, flujo, arquitectura, estados, clases, catálogos, etc.).
- Los diagramas de conocimiento del proyecto se almacenan en la memoria viva (steering de arquitectura), no en este archivo de reglas, en la sección correspondiente.

### 11.2 Análisis inicial al incorporar las reglas a un workspace nuevo

> **Nota sobre arranque en frío (importante):** estas reglas solo se aplican si el asistente las está leyendo. Cuando el estándar se despliega como archivos de workspace, el steering de Kiro los **referencia** vía `#[[file:...]]`; cuando se usa como Power, Kiro los carga por activación. En ambos casos el steering de arquitectura debe existir desde el inicio (aunque sea semilla) para que la referencia resuelva.

- **Kit mínimo de arranque** para que el estándar funcione en un workspace:
  1. Las reglas de trabajo (este archivo).
  2. El steering de arquitectura **semilla**: un esqueleto con los encabezados de sección vacíos (estructura, bases de datos, infraestructura, gotchas). Debe existir aunque esté vacío.
  3. En despliegue como archivos de workspace: los puentes de Kiro en `.kiro/steering/` que referencian los `.agents/*.md`. Como Power, este paso lo cubre el propio Power.
- Cuando el estándar se incorpora por primera vez a un workspace que **aún no tiene análisis previo** (steering semilla / sin contexto del proyecto), la **primera sugerencia del asistente en el chat** debe ser proponer al usuario un **análisis completo del workspace**.
- Ese análisis debe abarcar **todo lo que exista en el workspace**: idealmente un proyecto completo incluyendo librerías, microservicios y/o frontends, para construir el **contexto general**.
- El resultado del análisis se **documenta en el steering de arquitectura** (estructura de servicios, stacks, bases de datos, flujos, convenciones, gotchas) y debe incluir la **generación de diagramas en Mermaid**, priorizando:
  1. **Diagrama Entidad-Relación (E-R) de la base de datos** — el principal.
  2. Diagramas de **secuencia**, **flujo**, **arquitectura**, **funcionalidad** y **catálogos** según aplique.
- El propósito es doble: servir como **base de conocimiento inicial** del asistente y como **documentación del propio proyecto**.

### 11.3 Diagramas durante la evolución del conocimiento
- Conforme avancen las conversaciones y se descubra conocimiento nuevo, si el asistente determina que un diagrama aporta valor (aclara un flujo, una relación de datos, una arquitectura), debe **generarlo en Mermaid y agregarlo al steering de arquitectura** en la sección pertinente, siguiendo el Protocolo de Steering (Regla 3).

## 12. Pruebas y Documentación QA

Existen **dos artefactos de prueba complementarios y bien diferenciados**, más un documento índice que los ata:

| Artefacto | Propósito | Quién lo ejecuta | Dónde vive |
|---|---|---|---|
| **Tests locales ejecutables** | Verificar que la funcionalidad trabajada realmente funciona | El asistente (localmente, antes de dar por completa la tarea) | **Dentro del repo del servicio**, en su convención nativa por lenguaje |
| **Curls de validación para Infraestructura** | Que Infraestructura valide el correcto levantamiento/funcionamiento de lo desplegado en pod/servidor | Equipo de Infraestructura (consola de pod/servidor) | Documentados en el md QA (ver 12.2) |
| **Documento QA (índice)** | Documentar y agrupar ambos por tarea, estilo QA experto | — (referencia) | `.agents/tests/01-tests-{proyecto|microservicio}.md` |

### 12.1 Tests locales ejecutables (aplica a TODAS las modalidades)
- Al agregar/ajustar funcionalidad en **cualquier modalidad** (MANTENIMIENTO, EVOLUCIÓN, MIGRACIÓN, NUEVO), se deben generar **tests ejecutables localmente por el asistente** para comprobar lo trabajado antes de darlo por terminado.
- **Ubicación y convención según el lenguaje/stack detectado** en el repo del servicio, para que el runner nativo los encuentre y viajen con el código en git. El asistente debe **detectar el stack** (archivos de proyecto, dependencias) y aplicar la convención correspondiente:

  | Stack detectado | Framework por defecto | Ubicación / convención |
  |---|---|---|
  | **Python** (`requirements.txt`, `pyproject.toml`, `setup.py`) | `pytest` | `tests/` con archivos `test_*.py` |
  | **Java / Maven** (`pom.xml`) | JUnit | `src/test/java/...` (espejo del paquete en `src/main/java`) |
  | **Java / Gradle** (`build.gradle`) | JUnit | `src/test/java/...` |
  | **Node / JS backend** (`package.json`) | Jest (o el runner ya configurado) | `__tests__/` o `*.test.js` junto al código |
  | **Node / TypeScript** (`tsconfig.json`) | Jest / Vitest (o el ya configurado) | `*.test.ts` / `*.spec.ts` |
  | **Angular** (`angular.json`) | Jasmine/Karma (o Jest si está configurado) | `*.spec.ts` junto al componente/servicio — **solo lógica de servicios**, nunca UI/navegador |
  | **React** (`package.json` con `react`) | Jest + React Testing Library | `*.test.tsx` / `*.test.js` — **solo lógica**, nunca E2E visual/navegador |

- Si el repo **ya tiene un framework/convención de tests configurado**, respetarlo en vez de imponer el de la tabla.
- El asistente debe **ejecutar los tests vía PowerShell** (Regla 9) y confirmar que pasan antes de dar la tarea por terminada.

### 12.2 Curls de validación para Infraestructura
- Infraestructura solicita **curls que puedan ejecutar en consola de un pod/servidor** para validar el levantamiento y funcionamiento de lo desplegado. Se documentan en el md QA (no en un script agrupado).
- **Curls separados e individuales:** cada servicio/endpoint se expresa como un curl independiente, completo (método, URL, headers, payload), listo para copiar y pegar.
- **Pruebas integrales = curls encadenados en orden estricto:** cuando la validación requiera varios servicios en secuencia, documentarlos **en el orden exacto de ejecución**, empezando por el servicio base necesario (ej. un `login` que devuelve un JWT) y luego los servicios posteriores **reutilizando la respuesta del paso previo** (ej. inyectar el `token` del login en el header `Authorization` de los siguientes). Debe quedar explícito qué dato de la respuesta de un paso alimenta al siguiente.
- **Parametrizar el host/base-url** (ej. placeholder `{{BASE_URL}}` o variable al inicio), nunca `localhost` hardcodeado como único valor, porque se ejecutan contra distintos entornos/pods.

### 12.3 Documento QA índice (`.agents/tests/01-tests-*.md`)
- Redactado **con nivel de detalle de un QA Tester experto**, organizado por **secciones por tarea** con encabezado identificable de modalidad y folio.
- Ejemplo de encabezado de sección: `## [MANTTO 0023] Errores en pantalla de login`.
- Cada sección debe incluir:
  - Descripción principal de la tarea (en mantenimiento, la que proveyó el usuario) y qué se ajustó.
  - **Para el asistente:** ruta del/los test(s) local(es) dentro del repo del servicio y cómo ejecutarlos.
  - **Para Infraestructura:** los curls de validación (12.2), separados o encadenados en orden estricto según corresponda.
  - Datos de entrada/salida esperados, aserciones, cobertura, casos borde y supuestos.
  - Diagramas en **Mermaid** cuando aporten claridad (flujo de prueba, secuencia de curls encadenados, estados).

### 12.4 Alcance EXCLUSIVO a nivel servicios (Estricto)
- **Todas las pruebas generadas deben ser únicamente a nivel de servicios** (unitarias, de integración y/o de API/servicios backend).
- **JAMÁS generar pruebas automatizadas de frontend** que impliquen abrir un navegador y navegar automáticamente, ni automatización visual/E2E de UI.
- Si una funcionalidad nueva conlleva modificaciones en el front y el usuario solicita pruebas, el asistente debe **informar explícitamente** que, de acuerdo a estas reglas, **no puede generar pruebas automatizadas visuales/front**, y que esas pruebas deben ser realizadas por **el usuario humano**.

## 13. Organización de Artefactos de Documentación

Para mantener el steering ligero (se carga en cada sesión) y las conversaciones organizadas, los artefactos se separan por propósito. **Todo lo transversal del workspace vive bajo `.agents/`:**

| Ubicación | Propósito | Naturaleza |
|---|---|---|
| `.agents/steering.md` | Memoria viva: arquitectura, convenciones, bases de datos, gotchas, diagramas de conocimiento | Estable y conceptual |
| `.agents/AGENTS.md` | Reglas de trabajo del proyecto | Estable (normativo) |
| `.agents/tasks/` | Informes de hallazgos, análisis puntuales y resultados de investigación | Efímero / por tarea |
| `.agents/plans/` | Planes de implementación por tarea | Por tarea |
| `.agents/tests/` | Documento QA índice de los sets de pruebas, incluidos los curls de validación para Infraestructura (ver Regla 12) | Por entregable de pruebas |
| Repo de cada servicio (`tests/`, `src/test/...`, `*.spec.ts`, etc.) | Tests locales ejecutables que viajan con el código del servicio, según el stack | Por servicio |

- **Regla general:** lo **estable y conceptual** vive en el steering; lo **voluminoso, efímero o ligado a un entregable concreto** (hallazgos, planes, documentación QA) vive en su carpeta bajo `.agents/`, y el steering solo lo **referencia** cuando aporte contexto permanente. Los **tests ejecutables de código** son la excepción: viven dentro del repo del servicio que prueban, en la convención nativa de su lenguaje, no en `.agents/`.

## 14. Acceso a Bases de Datos vía MCP (Estricto)

### 14.1 Único canal de acceso
- **Cualquier consulta o interacción con una base de datos DEBE realizarse exclusivamente a través del servidor MCP configurado** (herramientas tipo `execute_sql`, `get_schema_info`, `get_table_sample`).
- **PROHIBIDO** abrir conexiones directas por otros medios (clientes externos, cadenas de conexión hardcodeadas, scripts ad-hoc fuera del MCP) para operar contra la BD.

### 14.2 Verificación previa obligatoria (antes de CUALQUIER acción a BD)
Antes de ejecutar la primera operación contra una base de datos en la conversación, el asistente debe **detenerse y preguntar al usuario**:
1. **¿Estás conectado a la VPN?** (la conectividad a los hosts de BD suele depender de ello).
2. **¿A cuál de las bases de datos disponibles en el MCP deseas que me conecte?** — presentando las opciones existentes (servidores definidos en la configuración MCP) para que el usuario elija de forma explícita.

No asumir el entorno ni la BD por defecto: siempre esperar la elección explícita del usuario.

### 14.3 Clasificación de entorno y restricción de operaciones
Según la BD elegida:
- **CALIDAD / QA / UAT / PRODUCCIÓN / STAGING:** el asistente debe **informar explícitamente al usuario** que en esos entornos **NO se realizarán acciones destructivas ni de escritura**; únicamente se permiten **consultas de lectura (`SELECT`)**. Quedan prohibidos `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `DROP`, `ALTER` y cualquier DDL/DML que modifique datos o estructura.
- **DESARROLLO (DEV) / LOCAL:** se permiten operaciones de escritura (migraciones, sincronizaciones, correcciones de datos, etc.), pero **siempre bajo la Regla 2**: confirmación explícita del usuario en el chat antes de ejecutar cualquier operación que modifique datos o estructura.
- Ante **duda sobre la clasificación** de una BD, tratarla por defecto como entorno **sensible (solo lectura)** hasta que el usuario confirme lo contrario.

## 15. Estándares de Frontend (Recursos Locales, Jenkins y Claro Atlas)

### 15.1 Recursos 100% locales — Prohibición de CDN
- **Proyectos de front NUEVOS (obligatorio):** todo recurso consumido por el cliente (CSS, JS, librerías, fuentes, íconos, imágenes) debe estar **descargado y disponible localmente** dentro del proyecto (ej. carpeta `public/` o equivalente servida por el servidor estático). Queda **PROHIBIDO** cargar recursos vía **CDN** o cualquier enlace remoto (`<link>`/`<script>`/`@import` a dominios externos, Google Fonts CDN, etc.).
- **Proyectos de front EXISTENTES (no obligatorio):** si se detecta uso de CDN o recursos remotos, el asistente **puede sugerir** migrarlos a local, pero **no es obligatorio** modificarlos.

### 15.2 Empaquetado para filtros de seguridad de Jenkins
- **Proyectos NUEVOS (obligatorio):** todo front —**incluso si es estático**— debe estructurarse con **`package.json`**, ya que el pipeline de despliegue en **Jenkins** aplica filtros de seguridad y detección de vulnerabilidades a través de ese archivo (y su lockfile).
  - Declarar dependencias/devDependencies reales del build; mantener el lockfile (`package-lock.json`) versionado.
  - Patrón de referencia validado por Infraestructura: un front estático con build local (ej. `node build.js`), estilos compilados localmente y recursos servidos desde `public/`.
- **Proyectos EXISTENTES (no obligatorio):** se **puede sugerir** incorporar `package.json` si no lo tienen, sin obligar.

### 15.3 Estándar UX/UI: Claro Atlas
- El estándar oficial de diseño UX/UI es **Claro Atlas**, cuyo contrato normativo vive en `claro-atlas-2025/CLARO_ATLAS_AGENT_NATIVE_CSS_v4.md`, con maqueta viva `claro-atlas-2025/claro-atlas.html` y assets locales (`color-icons/`, `font-icons/`, `fonts/`). La carpeta `claro-atlas-2025/` es autocontenida y compartible entre áreas/proyectos.
- **Proyectos de front NUEVOS (obligatorio):** se debe emplear Claro Atlas siguiendo su md (componentes canónicos, tokens, iconografía local, CSS nativo, gobernanza ante desviaciones). Los **assets del Atlas se copian al proyecto** (a la carpeta local servida, ej. `public/`) y se consumen localmente, nunca de forma remota (coherente con 15.1).
- **Proyectos EXISTENTES (no obligatorio):** si se detecta que la interfaz **no respeta** el estándar, el asistente **puede sugerir** un rediseño alineado a Claro Atlas, pero **no es obligatorio**.
- Al trabajar bajo Claro Atlas, el md del estándar es la **fuente de verdad de diseño**; sus reglas de gobernanza (no introducir estilos/colores/íconos fuera del catálogo sin confirmación del usuario) deben respetarse.
