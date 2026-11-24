# legacy-php

Sistemas PHP 5 / MySQL 5 instalados por cliente en distintos servidores, sin control de versiones, donde los cambios se aplican a mano archivo por archivo.

### PHP-001 · Encabezado de versión por archivo — DEBE

Cada archivo modificado lleva al inicio una línea por cambio, la más reciente arriba:
`// vMAYOR.MENOR.PARCHE-<servidor>-<instalación> | AAAA-MM-DD | <iniciales> | descripción`

**Por qué:** sin git, el encabezado es el único historial y dice qué versión tiene cada instalación.

```php
// ✔ v1.1.0-srv1-cliente | 2026-11-18 | BQ | agrega fecha de ingreso a base de clientes
//   v1.0.0-srv1-cliente | 2026-11-11 | BQ | versión inicial

// ✘ modificado por mi 18 nov
```

### PHP-002 · Respaldo antes de reestructurar — DEBE

Antes de migrar o reescribir un archivo se crea `<archivo>_RESPALDO.php` con el original intacto.

**Por qué:** es la única forma de volver atrás si algo falla en producción.

✔ `reportes.php` → `reportes_RESPALDO.php` + nuevo `reportes.php`
✘ Sobrescribir el archivo sin copia

### PHP-003 · Compatibilidad con PHP 5.4 — DEBE

No se usa sintaxis posterior a PHP 5.4: sin `??`, sin `...`, sin tipos escalares ni de retorno, sin `const` con expresiones. Sí se permiten `[]` y closures.

**Por qué:** hay servidores que no se pueden actualizar; un error de sintaxis tumba la página completa.

✔ `isset($x) ? $x : ""`
✘ `$x ?? ""` · `function f(int $a): string`

### PHP-004 · Migración gradual — DEBE

Solo se migra a las clases nuevas el archivo que se toca por un requerimiento. Cada instalación usa las clases que existen en ella; no se mezclan versiones de una misma clase.

**Por qué:** migrar todo de golpe es imposible sin git ni pruebas; mezclar clases provoca redeclaraciones.

✔ Usar `ConsultaSQL()` en una instalación que aún no tiene `Sql::`
✘ Incluir la carpeta de clases nuevas donde ya existen las viejas

### PHP-005 · No asumir que las instalaciones son iguales — DEBE

Antes de llevar un cambio de una instalación a otra, se compara el archivo destino; pueden tener consultas, columnas o funciones distintas.

**Por qué:** el mismo archivo diverge con los años en cada cliente.

✔ Comparar ambos archivos y adaptar el cambio
✘ Copiar el archivo de una instalación a otra

### PHP-006 · Configuración separada de la pantalla — DEBERÍA

Las listas largas de reportes o consultas viven en `<archivo>_config.php` y la pantalla solo las recorre.

**Por qué:** agregar un reporte se vuelve tocar un arreglo, no 50 líneas de HTML y `if`.

✔ `reportes.php` (pantalla) + `reportes_config.php` (arreglo de reportes)
✘ Un `if (isset($_POST[...]))` y un botón escrito a mano por reporte
