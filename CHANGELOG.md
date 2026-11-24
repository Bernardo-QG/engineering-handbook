# Changelog

Todos los cambios relevantes del handbook. El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el proyecto usa [Versionado Semántico](https://semver.org/lang/es/).

Una regla nueva sube la versión menor; quitar o cambiar el sentido de una regla sube la mayor; corregir redacción o ejemplos sube el parche.

## [Sin publicar]

### Cambiado

- DB-003: ninguna política de escritura para el rol `anon`; RLS activo no basta.
- COD-003: también aplica a archivos sin referencia (imágenes, scripts, documentos).

## [1.0.0] - 2026-11-24

### Agregado

- Módulos `git` (GIT-001 a GIT-008), `code` (COD-001 a COD-005), `documentation` (DOC-001 a DOC-004), `security` (SEC-001 a SEC-004), `web` (WEB-001 a WEB-006), `database` (DB-001 a DB-006) y `legacy-php` (PHP-001 a PHP-007).
- Formato de reglas con ID, nivel de obligación RFC 2119, porqué y ejemplo correcto e incorrecto.
- `.editorconfig`, `.gitattributes` con fin de línea LF y `.gitignore`.
