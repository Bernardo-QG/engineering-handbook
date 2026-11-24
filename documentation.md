# documentation

Solo la documentación que alguien (incluido yo) va a leer. Nada de relleno.

### DOC-001 · README esencial — DEBE

Todo proyecto tiene un `README.md` con: qué es, cómo correrlo y cómo está organizado. Lo demás va en `docs/` solo si hace falta.

**Por qué:** es lo primero que se abre; si no responde esas tres preguntas, no sirve.

✔ Descripción en dos líneas, comandos para correrlo, árbol de carpetas principal
✘ Historia del proyecto, badges y secciones vacías "Próximamente"

### DOC-002 · CHANGELOG — DEBE

`CHANGELOG.md` en formato [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/), en español, con la sección `[Sin publicar]` arriba y las versiones con fecha. En proyectos con un formato propio y mucho tiempo, se respeta el existente.

**Por qué:** el historial de git es para el código; el CHANGELOG es para quien usa el proyecto.

```
✔ ## [1.4.0] - 2026-11-23
  ### Agregado
  - Búsqueda de documentos con atajo `/`.

✘ ## Versión nueva (noviembre)
  - cosas
```

### DOC-003 · ADR para decisiones difíciles de revertir — DEBERÍA

Las decisiones de arquitectura (hosting, base de datos, framework, autenticación) se registran en `docs/adr/NNNN-titulo.md` con contexto, decisión y consecuencias.

**Por qué:** dentro de un año no recordaré por qué se eligió una cosa y no otra.

✔ `docs/adr/0003-hosting-en-cloudflare-pages.md`
✘ La decisión solo en un chat o en la cabeza

### DOC-004 · Fechas ISO 8601 — DEBE

Toda fecha escrita en documentos, nombres de archivo y changelogs usa `AAAA-MM-DD`.

**Por qué:** no es ambigua y se ordena sola.

✔ `2026-11-23` · `2026-11-21_secciones.sql`
✘ `23/11/26` · `nov-23`
