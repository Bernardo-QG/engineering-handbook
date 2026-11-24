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

El archivo global de instrucciones (`~/.kiro/steering/AGENTS.md`, importado desde `~/.claude/CLAUDE.md`) apunta a este repositorio, de modo que cualquier proyecto lo sigue sin copiarlo. En un proyecto basta con citar el ID de la regla (por ejemplo, "aplica GIT-001").
