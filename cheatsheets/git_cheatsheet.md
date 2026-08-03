# Git & GitHub Cheatsheet

Guía rápida de Git y GitHub organizada por secciones, para el control de versiones local y el trabajo con repositorios remotos. Incluye configuración, ramas, remotos, historial, deshacer cambios, la CLI oficial de GitHub (`gh`), pull requests y trucos útiles.

## Índice

- [1. Configuración inicial](#1-configuración-inicial)
- [2. Crear y clonar repositorios](#2-crear-y-clonar-repositorios)
- [3. Cambios y commits](#3-cambios-y-commits)
- [4. Ramas](#4-ramas)
- [5. Fusionar e integrar](#5-fusionar-e-integrar)
- [6. Remotos y sincronización](#6-remotos-y-sincronización)
- [7. Historial y log](#7-historial-y-log)
- [8. Deshacer cambios](#8-deshacer-cambios)
- [9. Stash](#9-stash)
- [10. Tags y versiones](#10-tags-y-versiones)
- [11. GitHub CLI (`gh`)](#11-github-cli-gh)
- [12. Pull requests y revisión](#12-pull-requests-y-revisión)
- [13. Trucos y atajos útiles](#13-trucos-y-atajos-útiles)
- [14. Comandos que conviene memorizar primero](#14-comandos-que-conviene-memorizar-primero)
- [15. Buenas prácticas](#15-buenas-prácticas)

## 1. Configuración inicial

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git config --global user.name` | Define el nombre de autor | `git config --global user.name "Ana García"` |
| `git config --global user.email` | Define el email de autor | `git config --global user.email ana@example.com` |
| `git config --list` | Muestra la configuración actual | `git config --list` |
| `git config --global core.editor` | Fija el editor por defecto | `git config --global core.editor code` |
| `git config --global init.defaultBranch` | Rama por defecto al iniciar | `git config --global init.defaultBranch main` |
| `git config --global alias.<nombre>` | Crea un alias | `git config --global alias.lg "log --oneline --graph"` |

### Autenticación en GitHub

```bash
gh auth login
gh auth status
git config --global credential.helper cache
```

- La CLI `gh` gestiona la autenticación de forma cómoda y guarda las credenciales.
- También se puede autenticar con HTTPS y un token o con claves SSH.

[↑ ir al índice](#índice)

## 2. Crear y clonar repositorios

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git init` | Inicia un repositorio | `git init` |
| `git clone` | Clona un repositorio remoto | `git clone https://github.com/usuario/repo.git` |
| `git clone --depth 1` | Clona solo el último commit (shallow) | `git clone --depth 1 https://github.com/usuario/repo.git` |
| `git remote add` | Añade un remoto | `git remote add origin https://github.com/usuario/repo.git` |
| `gh repo create` | Crea un repositorio en GitHub | `gh repo create mi-repo --public --source=. --push` |
| `gh repo clone` | Clona un repositorio | `gh repo clone usuario/repo` |

[↑ ir al índice](#índice)

## 3. Cambios y commits

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git status` | Estado del working tree | `git status` |
| `git status -s` | Estado resumido | `git status -s` |
| `git add` | Añade archivos al staging | `git add .` |
| `git add -p` | Añade porciones interactivas | `git add -p app.py` |
| `git commit` | Crea un commit | `git commit -m "fix: mensaje"` |
| `git commit -am` | Añade y commitea archivos rastreados | `git commit -am "refactor: limpieza"` |
| `git commit --amend` | Corrige el último commit | `git commit --amend -m "nuevo mensaje"` |
| `git diff` | Cambios sin stagear | `git diff` |
| `git diff --staged` | Cambios stageados | `git diff --staged` |
| `git rm` | Elimina un archivo y lo registra | `git rm archivo.txt` |
| `git mv` | Mueve o renombra y lo registra | `git mv viejo.txt nuevo.txt` |

### Consejos

- Revisar `git diff --staged` antes de crear el commit.
- Mantener cada commit pequeño y con un único propósito.

[↑ ir al índice](#índice)

## 4. Ramas

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git branch` | Lista ramas locales | `git branch` |
| `git branch -a` | Lista ramas locales y remotas | `git branch -a` |
| `git branch -vv` | Muestra ramas y su seguimiento | `git branch -vv` |
| `git branch <rama>` | Crea una rama | `git branch feat/nueva` |
| `git checkout -b <rama>` | Crea y cambia a la rama | `git checkout -b feat/nueva` |
| `git switch -c <rama>` | Alternativa moderna para crear y cambiar | `git switch -c feat/nueva` |
| `git switch <rama>` | Alternativa moderna a `checkout` | `git switch main` |
| `git checkout <rama>` | Cambia de rama | `git checkout main` |
| `git branch -d <rama>` | Borra una rama ya fusionada | `git branch -d feat/vieja` |
| `git branch -D <rama>` | Fuerza el borrado de una rama | `git branch -D feat/descartada` |
| `git branch -m <nuevo>` | Renombra la rama actual | `git branch -m feat/nuevo-nombre` |

[↑ ir al índice](#índice)

## 5. Fusionar e integrar

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git merge <rama>` | Fusiona una rama en la actual | `git merge feat/nueva` |
| `git merge --no-ff <rama>` | Fusiona creando un commit de merge | `git merge --no-ff feat/nueva` |
| `git rebase <rama>` | Reaplica commits sobre otra base | `git rebase main` |
| `git rebase -i` | Rebase interactivo (squash, reword...) | `git rebase -i HEAD~5` |
| `git cherry-pick <commit>` | Aplica un commit concreto | `git cherry-pick abc1234` |
| `git mergetool` | Abre la herramienta de resolución de conflictos | `git mergetool` |

### En caso de conflicto

- Editar los archivos marcados y resolver las diferencias.
- Añadir y commitear con `git add` y `git commit` tras resolver.
- Abortar una operación con `git merge --abort` o `git rebase --abort`.

[↑ ir al índice](#índice)

## 6. Remotos y sincronización

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git remote -v` | Muestra remotos configurados | `git remote -v` |
| `git remote add origin <url>` | Añade un remoto | `git remote add origin https://github.com/u/r.git` |
| `git remote set-url origin <url>` | Cambia la URL del remoto | `git remote set-url origin https://github.com/u/r.git` |
| `git fetch` | Descarga cambios sin fusionar | `git fetch origin` |
| `git pull` | Descarga y fusiona (`fetch` + `merge`) | `git pull origin main` |
| `git pull --rebase` | Descarga y reaplica sobre la base | `git pull --rebase origin main` |
| `git push` | Sube la rama actual | `git push` |
| `git push -u origin <rama>` | Sube y fija seguimiento de la rama | `git push -u origin feat/nueva` |
| `git push --tags` | Sube todos los tags | `git push --tags` |
| `git push --force-with-lease` | Fuerza el push de forma segura | `git push --force-with-lease` |

### Flujo habitual

```bash
git pull --rebase
git push -u origin main
```

[↑ ir al índice](#índice)

## 7. Historial y log

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git log` | Historial de commits | `git log` |
| `git log --oneline` | Resumen en una línea por commit | `git log --oneline` |
| `git log --graph` | Historial con gráfico de ramas | `git log --graph --oneline --all` |
| `git log -p` | Muestra los diffs en el log | `git log -p -2` |
| `git log --author="<nombre>"` | Filtra por autor | `git log --author="ana"` |
| `git show <commit>` | Detalle de un commit | `git show abc1234` |
| `git blame <archivo>` | Autor y commit por línea | `git blame app.py` |
| `git grep "<patrón>"` | Busca en archivos versionados | `git grep "TODO"` |
| `git shortlog -sn` | Resumen de commits por autor | `git shortlog -sn` |

[↑ ir al índice](#índice)

## 8. Deshacer cambios

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git restore <archivo>` | Descarta cambios sin stagear | `git restore app.py` |
| `git restore --staged <archivo>` | Saca un archivo del staging | `git restore --staged app.py` |
| `git checkout -- <archivo>` | Forma clásica de descartar cambios | `git checkout -- app.py` |
| `git reset <archivo>` | Quita del staging sin tocar el contenido | `git reset app.py` |
| `git reset --soft HEAD~1` | Deshace el commit manteniendo el staging | `git reset --soft HEAD~1` |
| `git reset --hard HEAD~1` | Deshace el commit y descarta cambios | `git reset --hard HEAD~1` |
| `git revert <commit>` | Crea un commit que invierte otro | `git revert abc1234` |
| `git clean -fd` | Borra archivos sin rastrear | `git clean -fd` |
| `git reflog` | Registro de movimientos de HEAD | `git reflog` |

### Advertencia

- `git reset --hard` y `git clean -fd` destruyen trabajo; conviene confirmar con `git status` y `git stash` antes de usarlos.

[↑ ir al índice](#índice)

## 9. Stash

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git stash` | Guarda los cambios temporalmente | `git stash` |
| `git stash -u` | Incluye archivos sin rastrear | `git stash -u` |
| `git stash list` | Lista los stashes | `git stash list` |
| `git stash show` | Muestra los cambios de un stash | `git stash show -p stash@{0}` |
| `git stash pop` | Recupera y elimina el último stash | `git stash pop` |
| `git stash apply` | Recupera sin eliminar el stash | `git stash apply stash@{1}` |
| `git stash drop` | Elimina un stash | `git stash drop stash@{0}` |

[↑ ir al índice](#índice)

## 10. Tags y versiones

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `git tag` | Lista los tags | `git tag` |
| `git tag <nombre>` | Crea un tag ligero | `git tag v1.0.0` |
| `git tag -a` | Crea un tag anotado | `git tag -a v1.0.0 -m "Release 1.0"` |
| `git checkout <tag>` | Explora el estado de un tag | `git checkout v1.0.0` |
| `git push origin <tag>` | Sube un tag concreto | `git push origin v1.0.0` |
| `git push origin --tags` | Sube todos los tags | `git push origin --tags` |
| `git tag -d <tag>` | Borra un tag local | `git tag -d v1.0.0` |
| `gh release create` | Crea un release en GitHub | `gh release create v1.0.0 --generate-notes` |

[↑ ir al índice](#índice)

## 11. GitHub CLI (`gh`)

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `gh auth login` | Autentica con GitHub | `gh auth login` |
| `gh auth status` | Muestra el estado de autenticación | `gh auth status` |
| `gh repo view` | Muestra el repositorio actual | `gh repo view` |
| `gh issue list` | Lista issues | `gh issue list --assignee @me` |
| `gh issue create` | Crea un issue | `gh issue create --title "Bug" --body "..."` |
| `gh pr list` | Lista pull requests | `gh pr list --state open` |
| `gh pr view` | Muestra el detalle de un PR | `gh pr view 123` |
| `gh pr checkout` | Cambia a la rama de un PR | `gh pr checkout 123` |
| `gh pr diff` | Muestra el diff de un PR | `gh pr diff 123` |
| `gh pr review` | Revisa un PR | `gh pr review 123 --approve` |
| `gh pr merge` | Fusiona un PR | `gh pr merge 123 --squash` |
| `gh gist create` | Crea un gist | `gh gist create --public app.py` |
| `gh browse` | Abre el repositorio en el navegador | `gh browse` |

### Publicar una rama

```bash
gh pr create --fill
gh pr list --web
```

[↑ ir al índice](#índice)

## 12. Pull requests y revisión

### Flujo de trabajo típico

```bash
git switch -c fix/error-404
git add -A
git commit -m "fix: corregido error 404"
git push -u origin fix/error-404
gh pr create --title "fix: error 404" --fill
```

### Comandos de revisión

| Comando | Qué hace | Ejemplo |
|---|---|---|
| `gh pr create` | Crea una pull request | `gh pr create --fill` |
| `gh pr view` | Ve el detalle de un PR | `gh pr view 123` |
| `gh pr diff` | Muestra los cambios del PR | `gh pr diff 123` |
| `gh pr review` | Deja una revisión | `gh pr review 123 --approve` |
| `gh pr checks` | Estado de los checks | `gh pr checks 123` |
| `gh pr close` | Cierra el PR sin fusionar | `gh pr close 123` |
| `gh pr merge` | Fusiona el PR | `gh pr merge 123 --squash --delete-branch` |

- Usar nombres de rama descriptivos y commits pequeños.
- Revisar el diff con `gh pr diff` antes de fusionar.
- Fusionar con squash si se prefiere un historial limpio.

[↑ ir al índice](#índice)

## 13. Trucos y atajos útiles

- `git log --oneline --graph --all --decorate`: vista completa del historial.
- `git log --all --grep="fix"`: busca commits por mensaje.
- `git shortlog -sn`: contribuciones por autor.
- `git commit --amend`: corrige el mensaje o el contenido del último commit.
- `git diff HEAD`: compara con el último commit.
- `git push --force-with-lease`: fuerza el push evitando pisar trabajo ajeno.

### Aliases recomendados

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --decorate --all"
```

[↑ ir al índice](#índice)

## 14. Comandos que conviene memorizar primero

```bash
git status
git add -p
git commit -m "mensaje"
git branch -a
git switch -c rama
git pull --rebase
git push -u origin rama
git log --oneline --graph --all
git stash
git reset --hard HEAD~1
```

[↑ ir al índice](#índice)

## 15. Buenas prácticas

- Hacer commits pequeños, atómicos y con mensajes claros (feat/fix/refactor...).
- No subir secretos ni credenciales; usar `.gitignore` para archivos generados.
- Revisar `git diff --staged` antes de commitear.
- Sincronizar con frecuencia: `git pull --rebase` antes de `git push`.
- Usar una rama por cambio y pull requests para integrar en `main`.
- Proteger la rama principal y aprobar los PRs tras revisar el diff.
