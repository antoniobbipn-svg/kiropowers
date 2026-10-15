---
inclusion: always
---

# Arquitectura y Contexto del Proyecto (Semilla)

> **Memoria viva** del workspace: base de conocimientos técnicos y arquitectónicos. Este archivo
> es una **semilla**: sus secciones están vacías a propósito. Se pobla con el **análisis inicial
> del workspace** (Regla 11.2) y se mantiene actualizado conforme surja conocimiento nuevo
> (Regla 3). Mientras las secciones sigan vacías, el asistente debe tratar el workspace como
> "sin análisis previo" y proponer el análisis completo como primera sugerencia.

---

## 1. Estructura y Servicios del Workspace

> _Pendiente: listar aquí librerías, microservicios y/o frontends detectados, con su stack y
> una breve descripción. Incluir un diagrama de arquitectura en Mermaid._

<!-- Tabla sugerida:
| Directorio / Microservicio | Tipo / Stack | Descripción |
|---|---|---|
-->

---

## 2. Bases de Datos y Fuentes de Datos

> _Pendiente: documentar esquemas, hosts por entorno (DEV/LOCAL/QA/UAT/STAGING/PROD), tablas
> clave y relaciones. Incluir el **diagrama Entidad-Relación (E-R) en Mermaid** como prioridad._

---

## 3. Infraestructura y Entornos

> _Pendiente: IDE(s), configuración MCP, pipeline de despliegue (ej. Jenkins), convenciones de
> ejecución de comandos y de frontend (recursos locales sin CDN, Claro Atlas)._

### 3.1 Ejecución de Comandos
- Toda ejecución de comandos se rige por la **Regla 9** (PowerShell de Windows). No se duplica aquí.

### 3.2 Convenciones de Frontend y Despliegue
- Recursos locales obligatorios (sin CDN), `package.json` para filtros de Jenkins y estándar
  UX/UI **Claro Atlas** (ver Regla 15 y la carpeta `claro-atlas-2025/`).

---

## 4. Aprendizajes, Quirks & Casos Borde (Gotchas)

> _Pendiente: registrar aquí soluciones a errores complejos, particularidades de datos,
> trazabilidades no obvias y decisiones de diseño conforme se descubran._
