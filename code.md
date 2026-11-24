# code

Código simple que se entienda al volver a él meses después.

### COD-001 · Nombres descriptivos en inglés — DEBERÍA

Variables, funciones, archivos y tablas nuevas en inglés, con nombres que digan qué son. Sin abreviaturas salvo las universales (`id`, `url`, `db`). Los comentarios y la documentación van en español.

**Por qué:** el nombre es la primera documentación; el inglés es el idioma de las herramientas y de los ejemplos.

✔ `calculateMonthlyBalance()` · `$pendingRemittances`
✘ `calc2()` · `$datos` · `$x`

### COD-002 · Un comentario por función — DEBE

Cada función lleva un comentario breve que explica qué hace y por qué existe, no cómo lo hace línea por línea.

**Por qué:** el código ya dice el cómo; el porqué se pierde si no se escribe.

```php
// ✔ Agrupa las alertas por persona y periodo: la UIF pide un solo reporte por socio al mes.
function groupAlerts(...)

// ✘ Recorre el arreglo y suma 1
function groupAlerts(...)
```

### COD-003 · Sin código muerto — DEBE

No se deja código comentado "por si acaso" ni funciones sin uso. Para recuperarlo está git.

**Por qué:** el código muerto confunde y nadie se atreve a borrarlo después.

✔ Borrar el bloque y explicarlo en el commit
✘ `/* versión anterior ... 80 líneas ... */`

### COD-004 · Simple antes que ingenioso — DEBERÍA

Se elige la solución más directa. No se abstrae hasta que el mismo patrón aparece por tercera vez.

**Por qué:** una abstracción prematura cuesta más de mantener que el código repetido.

✔ Dos funciones parecidas y claras
✘ Una clase genérica con cinco parámetros para dos casos

### COD-005 · Formato uniforme — DEBERÍA

Todo proyecto lleva `.editorconfig`: UTF-8, fin de línea LF, sangría de 4 espacios (2 en HTML, CSS, JS y JSON) y salto de línea final.

**Por qué:** evita diferencias falsas en los diffs y problemas de acentos.

El fin de línea en git lo asegura GIT-008.

✔ `.editorconfig` en la raíz desde el primer commit
✘ Archivos mezclando CRLF/LF y tabs/espacios
