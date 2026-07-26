# Flujo de trabajo

## Feature Branch

```bash
# 1. Crear rama desde main
git switch main
git pull
git switch -c feature/nombre

# 2. Trabajar y commitear
git add .
git commit -m "feat: descripcion"

# 3. Push al remote
git push -u origin feature/nombre

# 4. Crear Pull Request / Merge Request

# 5. Despues del merge, limpiar
git switch main
git pull
git branch -d feature/nombre
git push origin --delete feature/nombre
```

## Hotfix

```bash
# 1. Desde main, crear rama de hotfix
git switch main
git pull
git switch -c hotfix/bug-description

# 2. Fix y commit
git add .
git commit -m "fix: descripcion del bug"

# 3. Merge a main
git switch main
git merge hotfix/bug-description
git push

# 4. Limpiar
git branch -d hotfix/bug-description
```
