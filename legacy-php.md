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

Solo se migra el archivo que se toca por un requerimiento. Migrar significa llevarlo a las clases comunes (PHP-009 a PHP-011). Cada instalación usa lo que existe en ella: donde aún no hay clases comunes, se siguen usando las funciones viejas y no se incluye la carpeta nueva.

**Por qué:** migrar todo de golpe es imposible sin git ni pruebas; mezclar versiones de una misma función provoca redeclaraciones.

✔ Usar `ConsultaSQL()` en una instalación que aún no tiene `Sql::`
✘ Incluir `clases/funcionesarmadas/` donde ya están declaradas las funciones viejas

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

### PHP-007 · Probar el cambio aparte antes de reemplazar — DEBERÍA

Un cambio grande se prueba en una copia con otro nombre (`reporte2.php`) antes de reemplazar el archivo que usan los usuarios.

**Por qué:** sin ambiente de pruebas, la copia es la única forma de validar en el servidor real sin afectar a nadie.

✔ `reporte2.php` validado → se renombra a `reporte.php`
✘ Editar directo el archivo en uso a media jornada

### PHP-008 · Entrada a SQL siempre limpia — DEBE

Todo valor que viene de `$_POST`, `$_GET` o `$_SESSION` se limpia antes de entrar a una consulta: `(int)` o `(float)` para números e ids, y escape para texto (`Sql::escape()` donde ya hay clases comunes, `mysqli_real_escape_string` o `mysql_real_escape_string` según la conexión donde no).

**Por qué:** sin sentencias preparadas en PHP 5, el escape y el casteo son la única defensa contra la inyección SQL.

```php
// ✔
$id     = (int) $_POST["IdPersona"];
$nombre = Sql::escape($_POST["Nombre"]);
Sql::run("SELECT ... WHERE IdPersona = $id AND Nombre = '$nombre'");

// ✘
ConsultaSQL($link, "SELECT ... WHERE IdPersona = " . $_POST["IdPersona"]);
```

### PHP-009 · Funciones comunes en clases — DEBE

Lo que se usa en varios módulos vive en una clase por archivo dentro de `clases/funcionesarmadas/` (`Sql`, `Texto`, `Utils`, `Logger`, `ClienteHttp`…), protegida con `if (!class_exists(...))`. El archivo de funciones viejo solo las incluye, y la función vieja queda como envoltorio `@deprecated` que llama a la clase hasta que nadie la use. Una clase se adapta de la instalación más avanzada y su encabezado lo dice.

**Por qué:** una sola implementación por instalación, sin romper los archivos que todavía no se migran.

```php
// ✔ clases/funcionesarmadas/sql.php
// v1.0.0-srv2-cliente | 2026-07-23 | BQ | version inicial adaptada de srv1-cliente
if (!class_exists('Sql')) { class Sql { ... } }

/** @deprecated Usar Sql::run(). */
function ConsultaSQL($pLink, $pCadenaSQL) { Sql::init($pLink); return Sql::run($pCadenaSQL); }

// ✘ Copiar ConsultaSQL() dentro de cada módulo con pequeñas diferencias
```

### PHP-010 · Consultas por la clase Sql — DEBE

En archivos migrados, toda consulta pasa por `Sql::run()` (SELECT), `Sql::uno()` (una fila) o `Sql::exec()` (INSERT, UPDATE, DELETE). La clase usa la extensión que tenga el servidor (`mysql` o `mysqli`) y manda los errores y la consulta al `Logger`, nunca a la pantalla.

**Por qué:** cambiar de extensión o de versión de PHP se hace en un solo archivo, y el usuario no ve tablas ni columnas en un error.

✔ `$persona = Sql::uno("SELECT ... LIMIT 1");`
✘ `mysql_query($sql) or die(mysql_error());`

### PHP-011 · Todo módulo arranca con el guard — DEBE

En archivos migrados, la primera línea después del encabezado incluye `clases/guard_modulo.php`, que inicia la sesión, valida que el usuario esté autenticado, carga las clases comunes y las dependencias que declare el módulo.

**Por qué:** la validación de sesión no depende de que cada archivo la copie bien.

```php
// ✔
$dependencias = ["pld.php"];
include("clases/guard_modulo.php");

// ✘ session_start(); include(...); if (!isset($_SESSION[...])) ... copiado en cada archivo
```


### PHP-012 · Base explícita en consultas manuales — DEBE

En consultas manuales a un servidor con varias instalaciones, cada tabla lleva el nombre de la base (`base.tabla`).

**Por qué:** varias bases comparten servidor y esquema; una consulta en la base equivocada devuelve datos válidos pero de otro cliente, y la conclusión sale mal sin ningún error.

✔ `SELECT * FROM cliente.grupomenu;`
✘ `SELECT * FROM grupomenu;` con la base que haya quedado seleccionada

### PHP-013 · Una funcionalidad existe si cumple las tres capas — DEBE

Para afirmar que una instalación tiene una funcionalidad se verifican: la opción en el menú de la base (`mymenugenerador.external_link`), el archivo en el servidor y el permiso del perfil (`grupomenu`).

**Por qué:** el menú puede apuntar a archivos que no existen, un archivo puede existir sin opción de menú y una opción puede no estar asignada al perfil que la necesita.

✔ "Reportes PLD: opción en el menú, `reporteslavadodedinero.php` en el servidor, asignada a Oficial de Cumplimiento"
✘ "Sí existe, está el archivo en la carpeta"

### PHP-014 · Inventario del servidor, no de la copia local — DEBE

Antes de concluir que un archivo existe o falta, se toma la lista de archivos del servidor de esa instalación.

**Por qué:** la copia local solo tiene lo que se ha tocado por requerimientos y no refleja lo instalado.

✔ `find . -type f -iname '*.php' -printf '%P\t%s\t%TY-%Tm-%Td\n' | sort > ~/<instalacion>_php.txt`
✘ Revisar solo la carpeta local y reportar como faltante lo que no aparece

### PHP-015 · Auditoría: entregable externo separado del análisis interno — DEBERÍA

Lo que se entrega a la entidad o al auditor describe rutas de menú y funcionamiento. Los archivos, tablas, líneas de código y hallazgos internos van en un documento interno de la instalación.

**Por qué:** al auditor le sirve la ruta en pantalla, no el código, y los hallazgos internos se corrigen antes de exponerlos.

✔ "Catalogos › Personas › Personas: no permite guardar sin CURP"
✘ "`clientes.php` línea 241 valida `strlen($_POST["CURP"])`"
