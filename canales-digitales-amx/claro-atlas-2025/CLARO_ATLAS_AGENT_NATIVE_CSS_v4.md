# 🧭 STEERING DE DISEÑO - ATLAS (Claro 2026) - VERSIÓN AGENTE UNIVERSAL 4.0
## Estándar Oficial para Creación de Pantallas y Rediseño de Interfaces — Claro Atlas Native CSS

> **Documento Core de Estilos Visuales, Gobernanza y UI**
> Este documento es el **contrato operativo y normativo único** para cualquier agente IA que deba **crear nuevas pantallas/componentes** o **rediseñar interfaces existentes** bajo el sistema de diseño Claro Atlas. Establece el catálogo universal de componentes referenciado a la maqueta viva `claro-atlas.html`, las directrices de CSS nativo, la preservación estricta de lógica funcional y el **protocolo ineludible de gobernanza ante solicitudes fuera del estándar**.

---

## 🗺️ ÍNDICE OPERATIVO

1. [Contrato Operativo del Agente IA (Creación y Rediseño)](#1-contrato-operativo-del-agente-ia)
2. [Protocolo Estricto de Gobernanza y Desviaciones de Estándar](#2-protocolo-estricto-de-gobernanza-y-desviaciones-de-estándar)
3. [Regla de Oro: Cero Impacto en la Lógica Funcional](#3-regla-de-oro-cero-impacto-en-la-lógica-funcional)
4. [Descubrimiento Obligatorio de Assets Locales](#4-descubrimiento-obligatorio-de-assets-locales)
5. [Catálogo Maestro de Componentes Atlas (Referencia Visual: claro-atlas.html)](#5-catálogo-maestro-de-componentes-atlas)
6. [Tokens de Diseño: Colores, Tipografías y Gradientes](#6-tokens-de-diseño)
7. [Iconografía Oficial (Font-Icons y Color-Icons)](#7-iconografía-oficial)
8. [Patrones Responsivos Universales (Desktop vs Móvil)](#8-patrones-responsivos-universales)
9. [Protocolo Universal de Trabajo para Agentes](#9-protocolo-universal-de-trabajo-para-agentes)
10. [Matriz de Conflictos y Decisiones](#10-matriz-de-conflictos-y-decisiones)
11. [Checklist Obligatorio antes de Entregar](#11-checklist-obligatorio-antes-de-entregar)
12. [Definition of Done](#12-definition-of-done)

---

## 1. 🧭 CONTRATO OPERATIVO DEL AGENTE IA

### 1.1 Alcance Universal (Nuevas Creaciones y Rediseño)
Estas directrices aplican a **cualquier portal, aplicación, vista o componente** que el usuario solicite crear o rediseñar con Claro Atlas. El agente no debe asumir una estructura de carpetas rígida ni un único framework; debe inspeccionar y descubrir la estructura del proyecto objetivo antes de implementar.

- **Para Rediseño:** Modernizar la presentación visual preservando intacta toda la funcionalidad, datos, enlaces, estados y eventos preexistentes.
- **Para Nuevas Pantallas / Elementos:** Construir la maquetación y estructura visual utilizando estricta y exclusivamente los componentes, tokens, espaciados y tipografías del estándar Claro Atlas.

### 1.2 Regla de Precedencia de Instrucciones
Cuando existan instrucciones simultáneas o en aparente tensión, el agente debe resolverlas en este orden jerárquico estricto:

1. **Integridad Funcional y Seguridad:** Nunca romper lógica, llamadas a APIs, estados, eventos, parámetros de formularios o navegación.
2. **Gobernanza Atlas:** No introducir elementos visuales, colores o fuentes fuera del estándar sin previo aviso y confirmación del usuario.
3. **Alcance Explícito del Usuario:** Respetar la lista de archivos que el usuario autorizó modificar.
4. **Reglas No Negociables del Steering:** Componentes canónicos, iconografía local, CSS nativo y responsive.
5. **Objetivos UX/UI:** Jerarquía de información, claridad y reducción de carga cognitiva.

### 1.3 Estándar CSS Nativo — Prohibición de Dependencias Externas
Claro Atlas es un sistema de diseño **autosuficiente**. Queda terminantemente prohibido:
- Agregar `<link>` hacia CDN de frameworks o iconos externos (Tailwind, Bootstrap, FontAwesome, Google Fonts CDN, etc.).
- Agregar `<script>` para cargar librerías CSS o frameworks de estilos.
- Introducir utility classes externas (`p-4`, `flex`, `hidden`, etc. provenientes de Tailwind u homólogos) si no están definidas dentro del CSS local de Atlas.
- Toda regla visual nueva debe implementarse en **CSS nativo** aprovechando las variables y componentes oficiales.

---

## 2. 🛑 PROTOCOLO ESTRICTO DE GOBERNANZA Y DESVIACIONES DE ESTÁNDAR

> [!CAUTION]
> **REGLA DE ORO DE GOBERNANZA:**
> Si el usuario solicita un color, botón, tipografía, icono o estilo visual que **NO esté definido en el catálogo Claro Atlas**, el agente IA **NO DEBE APLICARLO DIRECTAMENTE**.

### Procedimiento Obligatorio ante Solicitudes Fuera del Estándar:

1. **Frenar la Aplicación Inmediata:** No inyectar el código no estándar en el archivo.
2. **Notificar con Claridad:** Indicar al usuario que el elemento solicitado (ej. color morado, tipografía Arial/Montserrat, botón cuadrado con borde azul neón, etc.) **no forma parte del estándar oficial Claro Atlas**.
3. **Proponer la Alternativa Oficial Equivalente:** Ofrecer la alternativa semántica y visualmente válida más cercana dentro de `claro-atlas.html`:
   - *Si solicita un color personalizado para alerta/peligro:* Proponer `--red-600` (`#DA291C`) o `--yellow-600` (`#FFC722`) según la criticidad.
   - *Si solicita un botón personalizado:* Mostrar las opciones del catálogo (`.claro-btn-p-normal`, `.claro-btn-s-normal`, `.claro-btn-t-gray-normal`).
   - *Si solicita una fuente externa:* Recordar que el estándar utiliza exclusivamente `AMX` (títulos y marcas) y `Roboto` (cuerpo, datos y formularios).
4. **Solicitar Confirmación:** Esperar la instrucción explícita del usuario para aplicar la alternativa oficial propuesta antes de editar el archivo.

---

## 3. 🚨 REGLA DE ORO: CERO IMPACTO EN LA LÓGICA FUNCIONAL

El rediseño o maquetación aplica exclusivamente a la **capa de presentación visual**.

- **PERMITIDO:** Reorganizar contenedores de presentación, añadir wrappers visuales, aplicar clases Atlas, cambiar estilos tipográficos, mejorar la jerarquía visual de la información.
- **CONDICIONAL:** Modificar atributos `id` o `class` solo si se demuestra fehacientemente que son puramente estéticos y no tienen ningún selector, enlace, escucha de eventos JS o enlace funcional asociado.
- **ESTRICTAMENTE PROHIBIDO:**
  - Alterar o eliminar scripts JavaScript/TypeScript.
  - Modificar nombres de funciones, variables, rutas de endpoints o llamadas a APIs.
  - Alterar atributos funcionales como `name`, `type`, `value`, `data-*` que alimentan la lógica de negocio.
  - Eliminar controles de formulario, filtros o botones funcionales para "simplificar" visualmente la pantalla.
  - Modificar llamadas dinámicas como `innerHTML` sin adaptar el nuevo HTML generado a las clases Atlas respetando los datos inyectados.

---

## 4. 📦 DESCUBRIMIENTO OBLIGATORIO DE ASSETS LOCALES

Todo proyecto bajo el estándar Claro Atlas debe contener de forma local los siguientes recursos. El agente debe **descubrir sus rutas físicas en el proyecto** antes de invocarlos:

1. **Fuentes AMX:** Títulos, marcas y acentos principales (`AMX-Regular`, `AMX-Medium`, `AMX-Bold`).
2. **Fuentes Roboto:** Cuerpo de texto, tablas, formularios y subtítulos (`Roboto-Regular`, `Roboto-Medium`, `Roboto-Bold`).
3. **Font-Icons Monocromáticos:** Hoja de estilos (ej. `font-claro.css`) con prefijo de clase `ico-*`.
4. **Color-Icons Ilustrativos:** Hoja de estilos (ej. `icons.local.css`) o directorio vectorial con clases `evIco-*`, `ceEvIco-*`, `cIco-*`.

*Regla:* Si algún recurso local no está disponible en el proyecto, el agente debe reportarlo al usuario y buscar un equivalente del catálogo local existente, **nunca descargarlo de un CDN externo**.

---

## 5. 📚 CATÁLOGO MAESTRO DE COMPONENTES ATLAS

El archivo **`claro-atlas.html`** constituye la **fuente canónica y viva** de la maqueta y código HTML/CSS del estándar Claro Atlas. 

Al crear una nueva pantalla o rediseñar un componente, el agente debe remitirse a la sección correspondiente de `claro-atlas.html` para replicar con exactitud el marcado, las dimensiones y las clases CSS oficiales:

| # | Componente / Categoría | Sección en `claro-atlas.html` | Clases Principales de Referencia | Directriz y Cotas Oficiales |
|---|---|---|---|---|
| **1** | **Tipografías y Títulos** | `#sec-1` | `.text-h1`, `.text-h2`, `.text-subtitle`, `.text-body`, `.kicker` | Títulos en **AMX Bold/Medium**. Cuerpo, datos y formularios en **Roboto**. Kickers en mayúsculas 11px rojo (`#DA291C`). |
| **2** | **Paleta de Colores & Tokens** | `#sec-2` | `--red-600`, `--gray-800`, `--blue-600`, etc. | Escala oficial de tokens semánticos (Rojo institucional, Grises neutros, Azul conectividad, Amarillo advertencia, Verde éxito). |
| **3** | **Gradientes y Transparencias** | `#sec-3` | `.grad-surface`, `.frosted-glass-pill` | Transparencias con `backdrop-filter: blur(8px)` y gradientes oficiales registrados. |
| **4** | **Iconografía** | `#sec-4` | `<i class="ico-{nombre}"></i>`, `<i class="evIco-{nombre}"></i>` | 460+ iconos monocromáticos y catálogo a color. Totalmente prohibidas librerías externas (Lucide, FontAwesome). |
| **5** | **Formularios & Inputs** | `#sec-5` | `.claro-native-input`, `.claro-native-select`, `.claro-floating-input-box`, `.pdf-check-box`, `.pdf-radio-box`, `.claro-search-input-box` | Inputs estándar: **40px altura**, radio 8px. Floating inputs: **46px**. Búsqueda: 35px o 45px. Reset obligatorio de flecha en selects (`appearance: none`). Checkboxes y radios en verde éxito (`#04B58E`). |
| **6** | **Botones y Acciones** | `#sec-6` | `.claro-btn-spec`, `.claro-btn-p-normal`, `.claro-btn-s-normal`, `.claro-btn-t-gray-normal`, `.claro-btn-disabled` | **32px altura estándar**, curvatura en píldora (`border-radius: 16px`), `min-width: 160px`. Tipografía **AMX Bold**. Relieve 3D overlay en primarios. En modales/móvil full-width se permite **48px**. |
| **7** | **Enlaces (Links)** | `#sec-7` | `.text-link`, `.claro-link` | Azul institucional (`var(--blue-600)` / `#0097A9`), subrayado y peso medium. |
| **8** | **Progreso y Steppers** | `#sec-8` | `.pdf-step-track`, `.pdf-step-labels`, `.node-outline-teal`, `.node-solid-teal` | Nodos circulares de 28px con padding de contención de 14px en los extremos del track para evitar desbordes. Pasos activos/completados en Teal (`#0097A9`). |
| **9** | **Navegación & Tabs** | `#sec-9` | `.claro-tab-group`, `.claro-tab-item`, `.claro-native-pagination` | Toggles segmentados en formato píldora de **48px de altura**. Paginadores con flechas y círculos activos en rojo institucional. |
| **10**| **Header Institucional** | `#sec-10` | `.sheet-header`, `.claro-brand`, `.brand-atlas` | Logotipo vectorial oficial Claro en rojo institucional + palabra descriptiva en tipografía AMX. |
| **11**| **Menús Desplegables** | `#sec-11` | `.claro-dropdown-menu`, `.dropdown-item` | Elevación suave, bordes redondeados y fondos de alta legibilidad (`#FFFFFF` con sombras neutras). |
| **12**| **Footers Institucionales** | `#sec-12` | `.claro-native-footer`, `.claro-checkout-footer` | Pie corporativo oscuro en `#2D2D2D` o pie de checkout claro en `#FAFAFA` con sellos de seguridad. |
| **13**| **Notificaciones & Toasts** | `#sec-13` | `.claro-native-toast`, `.success-toast`, `.error-toast`, `.warning-toast` | **Altura mínima obligatoria: 56px**. Fondos claros en escala 100, borde sólido de 1px en escala 500/600, icono representativo y texto oscuro de alto contraste. |
| **14**| **Espaciados y Grillas** | `#sec-14` | Multiplos base: 4px, 8px, 16px, 24px, 32px, 40px, 60px | Separación entre módulos: **60px**. Padding interior de tarjetas: **40px**. Márgenes móviles: 10px a 16px. |

---

## 6. 🎨 TOKENS DE DISEÑO

### 6.1 Tabla Maestra de Colores Oficiales

| Token | Hex | Rol Semántico en Claro Atlas | Mapeo / Conversión Permitida |
|---|---|---|---|
| `var(--red-600)` | `#DA291C` | **Rojo Claro Principal:** Botones primarios, CTAs, kickers, acentos de marca | Reemplaza `#ef4444`, `#ff0000`, `bg-red-600` |
| `var(--red-700)` | `#C7251A` | **Rojo Hover:** Estados hover de botones primarios y bordes activos | Reemplaza `#dc2626` |
| `var(--red-100)` | `#FCF3F3` | **Rojo Fondo:** Fondos de error, badges de atención, active tabs suaves | Reemplaza `#fee2e2` |
| `var(--blue-600)` | `#0097A9` | **Azul Principal (Teal):** Links, focus de inputs, steppers activos, badges de estado | Reemplaza `#3b82f6`, `#0284c7`, azul primario genérico |
| `var(--blue-500)` | `#2FBCC4` | **Azul Secundario:** Acentos de conectividad y badges secundarios | Reemplaza `#06b6d4` |
| `var(--bluegray-100)` | `#E0E7F6` | **Azul Grisáceo Fondo:** Fondos de tarjetas de planes y selección | Reemplaza `#e0f2fe`, `#dbeafe` |
| `var(--gray-900)` | `#222222` | **Gris Muy Oscuro:** Overlays y fondos de tooltips | Reemplaza `#0f172a` |
| `var(--gray-800)` | `#2D2D2D` | **Gris Texto Principal:** Títulos, cuerpo principal, tablas, encabezados | Reemplaza `#000000`, `#111827`, `#1f2937` |
| `var(--gray-600)` | `#525252` | **Gris Secundario:** Subtítulos y descripciones secundarias | Reemplaza `#4b5563` |
| `var(--gray-500)` | `#6C6C6C` | **Gris Neutro / Borde:** Captions, bordes de botón terciario, iconos secundarios | Reemplaza `#6b7280` |
| `var(--gray-200)` | `#DBDBDB` | **Gris Borde / Deshabilitado:** Bordes neutros de tarjetas, fondos deshabilitados | Reemplaza `#e5e7eb` |
| `var(--gray-100)` | `#F4F4F4` | **Gris Superficie:** Fondos de módulos y contenedores de datos | Reemplaza `#f9fafb`, `#f3f4f6` |
| `var(--yellow-600)` | `#FFC722` | **Amarillo Alerta:** Botón terciario amarillo, advertencias, highlights | Reemplaza `#eab308` |
| `var(--yellow-100)` | `#FFFCE0` | **Amarillo Fondo:** Fondos suaves de tarjetas de advertencia | Reemplaza `#fef9c3` |
| `var(--green-600)` | `#04B58E` | **Verde Éxito:** Validaciones correctas, checks, switches activos | Reemplaza `#22c55e`, `#10b981` |
| `var(--green-100)` | `#F3FFFC` | **Verde Fondo:** Fondos de confirmación y alertas de éxito | Reemplaza `#dcfce7` |

### 6.2 Jerarquía Tipográfica Normativa
- **AMX (`var(--font-amx)`):** Reservada para elementos de identidad:
  - Títulos principales (`.text-h1`, `.text-h2`, `.text-h3`, `.text-h4`).
  - Botones (`.claro-btn-spec` utiliza AMX Bold 16px).
  - Indicadores clave y métricas destacadas.
- **Roboto (`var(--font-roboto)`):** Para lectura, datos e interacción:
  - Textos de cuerpo (`.text-body`, 16px / line-height 20px).
  - Elementos de formulario (labels, inputs, selects, placeholders).
  - Tablas, metadatos, tooltips y captions (`.text-sm` 14px, `.text-xs` 12px).
  - Kickers (`.kicker`, 11px mayúsculas en rojo `#DA291C`).

---

## 7. 🎯 ICONOGRAFÍA OFICIAL

Queda estrictamente prohibido usar SVGs inline externos o librerías de terceros (Lucide, Feather, FontAwesome). Toda iconografía debe utilizar las fuentes y assets locales del proyecto:

### Mapeo Canónico de Font-Icons Comunes (`ico-*`)
| Concepto | Clase Font-Icon Oficial Atlas |
|---|---|
| Búsqueda | `<i class="ico-search"></i>` |
| Filtros / Sliders | `<i class="ico-filter"></i>`, `<i class="ico-sliders"></i>` |
| Descarga / Exportar | `<i class="ico-download"></i>`, `<i class="ico-download-cloud"></i>` |
| Subir / Cargar Archivo | `<i class="ico-upload"></i>`, `<i class="ico-upload-cloud"></i>` |
| Inicio / Home | `<i class="ico-home"></i>` |
| Éxito / Aprobado | `<i class="ico-check"></i>`, `<i class="ico-check-circle"></i>` |
| Alerta / Advertencia | `<i class="ico-alert-triangle"></i>`, `<i class="ico-alert-circle"></i>` |
| Error / Cancelar / Cerrar | `<i class="ico-x"></i>`, `<i class="ico-x-circle"></i>` |
| Información | `<i class="ico-info"></i>`, `<i class="ico-help-circle"></i>` |
| Edición | `<i class="ico-edit-2"></i>`, `<i class="ico-pencil"></i>` |
| Papelera / Borrar | `<i class="ico-trash-2"></i>`, `<i class="ico-trash"></i>` |
| Calendario / Fecha | `<i class="ico-calendar"></i>` |
| Reloj / Tiempo | `<i class="ico-clock"></i>` |
| Chevrons / Flechas | `<i class="ico-chevron-down"></i>`, `<i class="ico-chevron-up"></i>`, `<i class="ico-arrow-left"></i>` |
| Ojo / Contraseña | `<i class="ico-eye"></i>`, `<i class="ico-eye-off"></i>` |
| Carga / Spinner | `<i class="ico-loader animate-spin"></i>` |

---

## 8. 📱 PATRONES RESPONSIVOS UNIVERSALES (DESKTOP VS MÓVIL)

1. **Patrón de Botones en Cabeceras y Toolbars:**
   - **Desktop (`>= 768px`):** Los botones secundarios o de apoyo ("Inicio", "Exportar", "Filtros") muestran su icono + texto con altura de 32px y clase `.claro-btn-spec`.
   - **Móvil (`< 640px`):** Los botones secundarios colapsan automáticamente a formato circular icon-only de **28x28px** (`border-radius: 9999px; padding: 0;`), ocultando la etiqueta de texto y mostrando únicamente el icono centrado en color `#3C3C3C`.
2. **Patrón de Filtros y Botón FAB Flotante:**
   - **Botón FAB Flotante Móvil:** Exclusivo para resoluciones móviles (`max-width: 767px`). En desktop (`>= 768px`), debe estar estrictamente oculto (`display: none !important`).
   - Al desplegarse un panel lateral o drawer de filtros, el botón FAB flotante debe ocultarse obligatoriamente para no obstruir el contenido.
3. **Patrón de Botones de Confirmación en Drawers y Modales:**
   - Dentro de footers de modales o paneles laterales móviles, el botón principal adopta ancho completo (`width: 100%`) y altura expandida de **48px** con estilo primario rojo Atlas para facilitar la interacción táctil.

---

## 9. ⚙️ PROTOCOLO UNIVERSAL DE TRABAJO PARA AGENTES

Al recibir una tarea para crear o rediseñar una pantalla:

1. **Fase de Descubrimiento:**
   - Analizar los archivos existentes, descubrir la ruta de las fuentes e iconos locales en el proyecto y validar el alcance solicitado por el usuario.
2. **Fase de Evaluación de Requerimientos vs Gobernanza:**
   - Verificar si la petición contiene colores, elementos o peticiones no contempladas en Claro Atlas.
   - Si existen elementos fuera del estándar, **notificar al usuario de inmediato y proponer la alternativa oficial** antes de modificar el código.
3. **Fase de Maquetación / Rediseño con Referencia Viva:**
   - Consultar la sección pertinente en `claro-atlas.html` (ej. `#sec-6` para botones, `#sec-5` para formularios).
   - Replicar las clases CSS nativas correspondientes en la vista objetivo.
   - Mantener rigurosamente intactos todos los IDs, nombres de parámetros, eventos y llamadas a endpoints existentes.
4. **Fase de Verificación Responsiva:**
   - Probar que no exista desbordamiento horizontal en resoluciones móviles estándar (360px a 440px).
   - Comprobar que los botones y toolbars adopten las cotas móviles oficiales.

---

## 10. 🧩 MATRIZ DE CONFLICTOS Y DECISIONES

| Situación Encontrada | Decisión Obligatoria del Agente |
|---|---|
| El usuario solicita un color que no está en la paleta Atlas | **Frenar**, notificar que no pertenece al estándar y proponer el token más cercano del catálogo. |
| El usuario solicita una tipografía externa (Google Fonts, etc.) | **Frenar**, indicar que el estándar solo admite AMX y Roboto locales y proponer su aplicación. |
| El usuario pide modificar solo un archivo específico | Trabajar estrictamente dentro de ese archivo utilizando CSS nativo Atlas. |
| Existe un elemento con lógica JS ligada a su ID | No cambiar el ID bajo ninguna circunstancia. |
| Existe un icono de librería externa (Lucide, SVG ajeno) | Mapearlo a su Font-Icon o Color-Icon oficial equivalente de Atlas. |
| Un framework externo (ej. Tailwind) interfiere en los estilos | Desacoplar las clases externas del componente y aplicar las clases nativas de Atlas. |
| Falta un componente complejo no previsto | Armar la estructura con CSS nativo reutilizando los tokens oficiales de color, tipografía y espaciado de Atlas. |

---

## 11. 📋 CHECKLIST OBLIGATORIO ANTES DE ENTREGAR

### Gobernanza y Estándar
- [ ] Todos los colores utilizados corresponden a los tokens oficiales de Claro Atlas.
- [ ] No se utilizaron fuentes diferentes a AMX y Roboto locales.
- [ ] Toda solicitud fuera del estándar fue notificada y consensuada previamente con el usuario.

### Fidelidad Visual con `claro-atlas.html`
- [ ] Los botones respetan las clases canónicas (`.claro-btn-spec`, píldora 32px o 48px en modales).
- [ ] Los inputs y selects respetan la altura de 40px/46px y resetean la flecha nativa.
- [ ] Las alertas y toasts respetan la altura mínima de 56px y su esquema de bordes y fondos.
- [ ] La iconografía utiliza exclusivamente `ico-*` o `evIco-*` del catálogo local.

### Integridad Funcional
- [ ] La lógica JS/TS, llamadas a APIs, eventos y datos no sufrieron ninguna alteración.
- [ ] Todos los formularios conservan sus atributos de negocio (`name`, `type`, `value`).

### Responsividad
- [ ] La vista se visualiza correctamente en escritorio y en pantallas móviles sin scroll horizontal forzado.
- [ ] Los botones de toolbar y filtros siguen las adaptaciones móviles de Atlas.

---

## 12. 🏁 DEFINITION OF DONE

La tarea se considera finalizada únicamente cuando:
1. La pantalla o componente cumple al 100% las especificaciones visuales correspondientes de `claro-atlas.html`.
2. No se introdujeron dependencias CSS externas, CDN ni paquetes de terceros.
3. Se conservó intacta la funcionalidad y lógica operativa preexistente.
4. El resultado fue validado en escritorio y móvil.
5. El agente presenta un resumen conciso de los componentes Atlas incorporados y los archivos intervenidos.
