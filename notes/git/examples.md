# Ejemplos de Git

## Crear repositorio y subir a GitHub

```bash
mkdir mi-proyecto && cd mi-proyecto
git init
git add .
git commit -m "feat: initial commit"
git remote add origin https://github.com/user/repo.git
git push -u origin main
```

## Guardar trabajo a medio hacer

```bash
# Estas trabajando y necesitas cambiar de rama
git stash push -m "wip: avanzando en feature X"

# Cambiar a otra rama, hacer algo, volver
git switch main
# ... trabajo urgente ...

# Volver a tu trabajo
git switch feature/x
git stash pop
```

## Rebase interactivo para limpiar commits

```bash
# Tienes 5 commits messy, quieres convertirlos en 1 o 2
git rebase -i HEAD~5

# Editor se abre:
pick abc1234 feat: add component
pick def5678 fix typo
pick ghi9012 fix typo again
pick jkl3456 feat: add styles
pick mno7890 fix: adjust spacing

# Cambia los ultimos 4 a "squash" o "fixup":
pick abc1234 feat: add component
squash def5678 fix typo
fixup ghi9012 fix typo again
squash jkl3456 feat: add styles
fixup mno7890 fix: adjust spacing
```

## Cherry-pick un commit de otra rama

```bash
# Necesitas un commit especifico de otra rama
git log --oneline feature/otra   # encontrar el hash
git cherry-pick abc1234
```

## Buscar el commit que rompio algo (bisect)

```bash
git bisect start
git bisect bad          # commit actual esta roto
git bisect good abc1234 # este commit funcionaba

# Git te lleva a un commit intermedio
# Probar si funciona:
git bisect good  # o
git bisect bad

# Repetir hasta encontrar el commit
git bisect reset  # volver al estado original
```
