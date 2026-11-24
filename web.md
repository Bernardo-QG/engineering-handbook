# web

Sitios estáticos (HTML, CSS y JS) publicados en GitHub Pages o Cloudflare Pages.

### WEB-001 · Sin herramientas de compilación mientras no hagan falta — DEBERÍA

Un sitio empieza como HTML, CSS y JS sin `package.json`, bundlers ni frameworks. Se agregan solo cuando un problema real lo pide.

**Por qué:** menos piezas que mantener; el sitio se abre y se publica tal cual.

✔ `index.html` + `css/` + `js/` publicados directo
✘ React + Vite + 300 MB de `node_modules` para una página de contenido

### WEB-002 · Contenido separado del código — DEBE

El contenido que cambia (listas, textos, catálogos) vive en `data/*.json` o en la base de datos, y el JS solo lo pinta.

**Por qué:** agregar contenido no obliga a tocar lógica ni HTML.

✔ `data/levels.json` leído con `fetch()`
✘ 200 líneas de `<li>` escritas a mano en `index.html`

### WEB-003 · Colores en variables CSS con tema claro y oscuro — DEBERÍA

Los colores se definen una vez en `:root` y se redefinen en `prefers-color-scheme: dark` (y en `[data-theme]` si hay botón de tema).

**Por qué:** cambiar la paleta o soportar tema oscuro es tocar un solo lugar.

```css
/* ✔ */ :root { --bg: #fff; --text: #222; }
        @media (prefers-color-scheme: dark) { :root { --bg: #111; --text: #eee; } }
/* ✘ */ .card { background: #fff; color: #222; }
```

### WEB-004 · Accesibilidad básica — DEBE

Contraste legible, `alt` en imágenes, `lang` en `<html>`, etiquetas en los campos y todo usable con teclado.

**Por qué:** cuesta poco desde el inicio y mucho después.

✔ `<html lang="es">` · `<img alt="Logo de Codex">` · `<button>` para acciones
✘ `<div onclick>` como botón · imágenes sin `alt`

### WEB-005 · Rutas relativas — DEBE

Enlaces, scripts y estilos usan rutas relativas para que el sitio funcione en local, en un subdirectorio de GitHub Pages o en otro dominio.

**Por qué:** `bernardo-qg.github.io/proyecto/` rompe las rutas que empiezan con `/`.

✔ `href="css/styles.css"`
✘ `href="/css/styles.css"`

### WEB-006 · Metadatos al compartir — DEBERÍA

Cada sitio lleva `<title>`, `description`, favicon y `og:image` para que se vea bien al compartir el enlace.

**Por qué:** el enlace es lo primero que ve quien no conoce el proyecto.

✔ `<meta property="og:image" content="img/preview.png">`
✘ Pestaña con el título "Document" y vista previa vacía
