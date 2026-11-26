# engineering-handbook

Reglas y buenas prácticas con las que trabajo en mis proyectos personales. Responde una sola pregunta: **¿cómo trabajo?**

Cada proyecto adopta solo los módulos que ocupa. Las reglas sirven para aplicarlas a mano o como instrucciones para una IA.

## Módulos

| Módulo | Prefijo | Cubre |
|---|---|---|
| [git](git.md) | GIT | Commits, ramas y versiones |
| [code](code.md) | COD | Nombres, comentarios y estilo |
| [documentation](documentation.md) | DOC | README, CHANGELOG y ADR |
| [security](security.md) | SEC | Secretos y datos sensibles |
| [web](web.md) | WEB | Sitios estáticos y su publicación |
| [database](database.md) | DB | Esquema, migraciones y consultas |
| [legacy-php](legacy-php.md) | PHP | Sistemas PHP 5 sin control de versiones |

## Formato de una regla

```
### GIT-001 · Título — DEBE

Qué se pide, en una o dos líneas.

**Por qué:** la razón, en una o dos líneas.

✔ ejemplo correcto
✘ ejemplo incorrecto
```

Niveles de obligación ([RFC 2119](https://www.rfc-editor.org/rfc/rfc2119)):

- **DEBE / NO DEBE**: obligatorio; romperlo es un error.
- **DEBERÍA**: lo normal; se puede omitir con una razón clara.
- **PUEDE**: opcional.

Los IDs no se reutilizan. Si una regla se elimina, su ID queda retirado.

## Uso con IA

El archivo global de Claude (`~/.claude/CLAUDE.md`) apunta a este repositorio, de modo que cualquier proyecto lo sigue sin copiarlo. En un proyecto basta con citar el ID de la regla (por ejemplo, "aplica GIT-001").

Claude lee los archivos al iniciar cada sesión, así que un cambio en el handbook aplica desde la siguiente sin instalar nada. En una computadora nueva:

1. Clonar el repositorio en `~/Desktop/PROJECTS/engineering-handbook`.
2. Agregar a `~/.claude/CLAUDE.md` la importación del handbook: `@~/Desktop/PROJECTS/engineering-handbook/README.md` y una línea igual por cada módulo que deba aplicarse siempre (por ejemplo `.../git.md`).
3. Para traer cambios hechos en otra computadora: `git pull`.
