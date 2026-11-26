# Changelog

Todos los cambios relevantes del handbook. El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el proyecto usa [Versionado Semántico](https://semver.org/lang/es/).

Una regla nueva sube la versión menor; quitar o cambiar el sentido de una regla sube la mayor; corregir redacción o ejemplos sube el parche.

## [Sin publicar]

### Agregado

- PHP-008 a PHP-011: entrada limpia a SQL, funciones comunes en clases, consultas por `Sql` y arranque con `guard_modulo`, según la migración gradual en curso.
- PHP-012 a PHP-015: base explícita en consultas manuales, verificar menú + archivo + permiso, inventario del servidor y separar el entregable de auditoría del análisis interno.
- PHP-016 a PHP-023: base canónica única, variaciones de entidad en configuración, capas de dependencia, dos bitácoras, retornos predecibles, orden de migración, idioma de los nombres (patrón en inglés, negocio en español) y forma de los nombres (PSR-1). Sustituyen los documentos de estándares que había en cada instalación.

### Cambiado

- COD-001: en sistemas con negocio en español, los términos de negocio se quedan en español (PHP-022).
- PHP-009: las clases salen de la base canónica (PHP-016), ya no de la instalación más avanzada.
- PHP-004: define qué significa migrar un archivo.
- DB-003: ninguna política de escritura para el rol `anon`; RLS activo no basta.
- COD-001: lo nuevo en inglés; lo existente en español no se renombra solo por idioma, se traduce al reescribirlo.
- WEB-005 y WEB-006: `og:image`, `og:url` y `canonical` van con URL absoluta.
- COD-003: también aplica a archivos sin referencia (imágenes, scripts, documentos).

## [1.0.0] - 2026-11-24

### Agregado

- Módulos `git` (GIT-001 a GIT-008), `code` (COD-001 a COD-005), `documentation` (DOC-001 a DOC-004), `security` (SEC-001 a SEC-004), `web` (WEB-001 a WEB-006), `database` (DB-001 a DB-006) y `legacy-php` (PHP-001 a PHP-007).
- Formato de reglas con ID, nivel de obligación RFC 2119, porqué y ejemplo correcto e incorrecto.
- `.editorconfig`, `.gitattributes` con fin de línea LF y `.gitignore`.
