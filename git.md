# git

Trabajo solo: no hay revisiones ni flujo de equipo. El objetivo es un historial legible y una rama principal que siempre funciona.

### GIT-001 · Conventional Commits — DEBE

Formato `tipo(alcance): descripción`, en español, en modo imperativo y en minúsculas. Tipos: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `chore`. El alcance es opcional.

**Por qué:** el historial se lee de un vistazo y el CHANGELOG y la versión salen de él.

✔ `feat(admin): agrega atajo ctrl+s para guardar`
✘ `cambios varios` · `Agregado el atajo`

### GIT-002 · Un commit, un cambio — DEBERÍA

Cada commit agrupa un solo cambio lógico. Si la descripción necesita un "y", probablemente son dos.

**Por qué:** se puede revertir o revisar una cosa sin arrastrar otras.

✔ `fix(web): corrige enlaces relativos del markdown`
✘ `feat: agrega búsqueda, cambia tipografía y corrige enlaces`

### GIT-003 · Trabajo directo en main — PUEDE

Los cambios pequeños van directo a `main`. Se usa una rama `exp/<tema>` solo para experimentos o para trabajo que dejaría `main` roto más de una sesión; al terminar se integra con squash y la rama se borra.

**Por qué:** las ramas solo valen la pena cuando protegen `main`; sin equipo, lo demás es ceremonia.

✔ `main` ← commit directo · `exp/nuevo-editor` → squash a `main`
✘ `feature/cambiar-color-boton` que vive tres semanas

### GIT-004 · main siempre funciona — DEBE

No se sube a `main` código que no corre o que deja el sitio roto.

**Por qué:** `main` es lo que está publicado o lo que se publicará; debe poder desplegarse en cualquier momento.

✔ Probar localmente antes del `push`
✘ `wip: a medias, luego lo arreglo` en `main`

### GIT-005 · Versionado semántico — DEBERÍA

Versiones `MAYOR.MENOR.PARCHE` ([SemVer](https://semver.org/lang/es/)). Al publicar se crea el tag `vX.Y.Z`. Un `feat!` o `BREAKING CHANGE` sube la mayor; `feat`, la menor; `fix`, el parche.

**Por qué:** la versión dice qué tan grande es el cambio sin leer el código.

✔ `git tag v1.4.0`
✘ `git tag final2`

### GIT-006 · No reescribir lo publicado — NO DEBE

No se hace `push --force` sobre `main` ni se reescriben commits ya subidos.

**Por qué:** se pierde historial y se rompen los clones en otros equipos.

✔ `git revert <commit>`
✘ `git push --force origin main`

### GIT-007 · .gitignore desde el primer commit — DEBE

Todo repositorio arranca con `.gitignore` que excluye secretos (`.env`, `.env.*`), dependencias (`node_modules/`, `vendor/`), configuración del editor y archivos del sistema (`Thumbs.db`, `.DS_Store`).

**Por qué:** lo que entra una vez al historial ya no se saca limpio.

✔ `.gitignore` en el commit inicial
✘ Agregar `.env` al `.gitignore` después de haberlo subido

### GIT-008 · Fin de línea LF — DEBE

Todo repositorio lleva `.gitattributes` con `* text=auto eol=lf`. La excepción son los scripts de Windows que lo necesitan: `*.cmd text eol=crlf` (igual para `*.bat`).

**Por qué:** en Windows git convierte a CRLF y los diffs muestran archivos completos como cambiados.

✔ `.gitattributes` en la raíz
✘ Depender de que cada equipo tenga bien `core.autocrlf`

### GIT-009 · Commits sin firmas de agentes — NO DEBE

Ni los commits ni los pull requests llevan líneas que agregue una IA o herramienta: `Co-Authored-By` de un agente, ligas a sesiones, ni textos como "Generated with …". El autor es la persona que hace el commit, y el mensaje es solo el de GIT-001.

**Por qué:** el historial registra qué cambió y quién responde por el cambio; las firmas automáticas ensucian el mensaje y hacen aparecer como coautor a quien no responde por el código.

✔ `chore: agrega estado_migracion.cmd para correr el estado con doble clic`
✘ El mismo mensaje con `Co-Authored-By: Claude …` y `Claude-Session: https://…` al final
