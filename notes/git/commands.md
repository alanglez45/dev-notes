# Comandos de Git

## Basicos

| Comando | Descripcion |
|---------|-------------|
| `git init` | Inicializa un repositorio nuevo |
| `git clone <url>` | Clona un repositorio remoto |
| `git add <archivo>` | Agrega archivo al staging area |
| `git add .` | Agrega todos los cambios al staging |
| `git commit -m "msg"` | Confirma los cambios del staging |
| `git status` | Muestra el estado del working directory |
| `git log --oneline` | Historial compacto de commits |
| `git log --graph --oneline` | Historial con grafo de ramas |
| `git diff` | Diferencias sin staging |
| `git diff --staged` | Diferencias en el staging |
| `git show <commit>` | Muestra detalles de un commit |

## Ramas

| Comando | Descripcion |
|---------|-------------|
| `git branch` | Lista ramas locales |
| `git branch <nombre>` | Crea una rama nueva |
| `git branch -d <nombre>` | Elimina una rama (segura) |
| `git branch -D <nombre>` | Elimina una rama (forzada) |
| `git branch -a` | Lista todas las ramas (locales + remotas) |
| `git checkout <rama>` | Cambia a otra rama |
| `git switch <rama>` | Cambia a otra rama (forma moderna) |
| `git switch -c <rama>` | Crea y cambia a rama nueva |
| `git merge <rama>` | Fusiona la rama indicada en la actual |
| `git rebase <rama>` | Reaplica commits encima de otra rama |

## Stash

| Comando | Descripcion |
|---------|-------------|
| `git stash` | Guarda cambios sin committear |
| `git stash push -m "msg"` | Guarda con descripcion |
| `git stash list` | Lista todos los stashes |
| `git stash pop` | Aplica y elimina el ultimo stash |
| `git stash apply` | Aplica sin eliminar el stash |
| `git stash drop` | Elimina el ultimo stash |
| `git stash clear` | Elimina todos los stashes |

## Remotos

| Comando | Descripcion |
|---------|-------------|
| `git remote -v` | Lista remotos configurados |
| `git remote add origin <url>` | Agrega un remote |
| `git fetch` | Descarga cambios sin mergear |
| `git pull` | Fetch + merge automatico |
| `git pull --rebase` | Fetch + rebase automatico |
| `git push` | Sube commits al remote |
| `git push -u origin <rama>` | Push y establece upstream |
| `git push --force` | Fuerza push (cuidado) |

## Deshacer cambios

| Comando | Descripcion |
|---------|-------------|
| `git reset HEAD~1` | Deshace ultimo commit (mixed, default) |
| `git reset --soft HEAD~1` | Deshace commit, mantiene cambios en staging |
| `git reset --hard HEAD~1` | Deshace commit, elimina todos los cambios |
| `git revert <commit>` | Crea un commit nuevo que invierte el indicado |
| `git clean -fd` | Elimina archivos sin trackear |

## Avanzados

| Comando | Descripcion |
|---------|-------------|
| `git cherry-pick <commit>` | Aplica un commit especifico en la rama actual |
| `git bisect start` | Inicia busqueda binaria de bug |
| `git bisect good` | Marca commit como bueno |
| `git bisect bad` | Marca commit como malo |
| `git reflog` | Historial de movimientos de HEAD |
| `git blame <archivo>` | Muestra quien modifico cada linea |
| `git tag <nombre>` | Crea un tag en el commit actual |
| `git tag -a <nombre> -m "msg"` | Crea tag con anotacion |
| `git rebase -i HEAD~n` | Rebase interactivo para reordenar/squash |
