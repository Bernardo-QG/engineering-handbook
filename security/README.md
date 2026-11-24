# security

Lo mínimo para no exponer nada, sobre todo en repositorios públicos.

### SEC-001 · Secretos fuera del repositorio — DEBE

Contraseñas, tokens, llaves y credenciales de prueba viven en `.env.local`, ignorado en `.gitignore` desde el primer commit. Se sube un `.env.example` con las claves vacías.

**Por qué:** lo que llega a git se queda en el historial aunque se borre después.

✔ `.env.local` (ignorado) + `.env.example` con `DB_PASSWORD=`
✘ `$password = "Admin123";` en el código

### SEC-002 · Nada de terceros en repos públicos — DEBE

En un repositorio público no van datos de clientes o empleadores: nombres de entidades, servidores, IPs, rutas internas, bases de datos ni datos personales reales. Los ejemplos usan valores ficticios.

**Por qué:** el repo es mío, la información no.

✔ `// v1.2.0-srv1-cliente | ...`
✘ Nombres reales de clientes, servidores o socios

### SEC-003 · Secreto expuesto, secreto rotado — DEBE

Si un secreto llega a subirse, se cambia de inmediato. Borrarlo del historial no basta.

**Por qué:** basta un clon o un bot en minutos para que ya esté copiado.

✔ Cambiar el token y luego limpiar el repo
✘ Solo borrar el archivo en un nuevo commit

### SEC-004 · No confiar en la entrada — DEBE

Todo dato que viene del usuario o de la URL se valida, y en SQL se usa parámetro o escape; nunca concatenación directa.

**Por qué:** la inyección SQL sigue siendo el ataque más común en sistemas web.

✔ `$stmt->bind_param("i", $id)` · `mysqli_real_escape_string($link, $nombre)`
✘ `"... WHERE Nombre='" . $_POST["nombre"] . "'"`
