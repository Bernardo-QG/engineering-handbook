# database

MySQL en sistemas existentes y Supabase (PostgreSQL) en proyectos nuevos.

### DB-001 · Esquema versionado en el repositorio — DEBE

El esquema completo vive en `db/schema.sql` y cada cambio en `db/migrations/AAAA-MM-DD_descripcion.sql`. Nada se cambia solo desde el panel de la base de datos.

**Por qué:** la base de datos se puede recrear y se sabe qué cambió y cuándo.

✔ `db/migrations/2026-11-21_rls_politicas.sql`
✘ Crear una columna en el panel de Supabase y no anotarla

### DB-002 · Nombres consistentes — DEBE

Dentro de un proyecto, tablas y columnas siguen una sola convención (en proyectos nuevos: `snake_case`, tablas en plural, llave `id`, foráneas `<tabla_en_singular>_id`). En sistemas existentes se respeta la convención que ya tienen.

**Por qué:** las consultas se escriben sin consultar el esquema a cada rato.

✔ `documents.section_id` → `sections.id`
✘ `Documentos.IdSeccion` junto a `tags.document_id`

### DB-003 · RLS en toda tabla expuesta — DEBE

En Supabase, toda tabla accesible desde el navegador tiene Row Level Security activado y políticas explícitas de lectura y escritura.

**Por qué:** la llave pública está en el JS; sin RLS, cualquiera puede leer o borrar todo.

✔ `alter table documents enable row level security;` + políticas por operación
✘ Tabla con RLS apagado "mientras lo termino"

### DB-004 · La llave privada nunca en el cliente — NO DEBE

La `service_role` key (o cualquier credencial con permisos totales) no va en el frontend ni en el repositorio. En el navegador solo se usa la llave publicable.

**Por qué:** esa llave se salta RLS; si se filtra, la base de datos queda abierta.

✔ Llave publicable en `js/config.js`; `service_role` solo en el servidor o en `.env.local`
✘ `createClient(url, "eyJ...service_role...")` en el JS del sitio

### DB-005 · Consultas por conjuntos, no por fila — DEBERÍA

En reportes y listados grandes se usan `JOIN`, subconsultas agrupadas o tablas temporales en lugar de llamar una función o subconsulta por cada fila.

**Por qué:** una función por fila multiplica el tiempo por el número de registros.

✔ `LEFT JOIN (SELECT persona_id, SUM(saldo) ... GROUP BY persona_id) s`
✘ `SELECT CalculaSaldo(id), DameNombre(id) ...` sobre 50 000 filas

### DB-006 · Índices sobre lo que se filtra — DEBERÍA

Las columnas usadas en `WHERE`, `JOIN` y `ORDER BY` frecuentes tienen índice. Antes de optimizar una consulta lenta se revisa con `EXPLAIN`.

**Por qué:** la mayoría de las consultas lentas son un índice que falta.

✔ `EXPLAIN SELECT ...` → agregar índice a `fecha`
✘ Reescribir la consulta tres veces sin ver el plan
