# Problemas y soluciones

## 1. Undo ultimo commit (sin subir)

```bash
# Mantener cambios en staging (listos para recommittear)
git reset --soft HEAD~1

# Mantener cambios en working directory (sin staging)
git reset HEAD~1

# Eliminar todo (CUIDADO: no se puede deshacer)
git reset --hard HEAD~1
```

## 2. Undo commit que ya se pusheo

```bash
# Opcion segura: crear commit que invierte el cambio
git revert <commit-hash>
git push

# Opcion destructiva (no recomendado si otros tienen el commit)
git reset --hard HEAD~1
git push --force
```

## 3. Resolver conflictos de merge

```bash
git merge feature/rama
# CONFLICT en archivo.js

# 1. Abrir el archivo y buscar los conflictos:
<<<<<<< HEAD
 codigo de tu rama
=======
 codigo de la otra rama
>>>>>>> feature/rama

# 2. Editar: quedarte con lo que quieras, eliminar los markers

# 3. Marcar como resuelto
git add archivo.js

# 4. Completar el merge
git commit -m "merge: resolve conflicts"
```

## 4. Recuperar commit perdido (reflog)

```bash
# Ver historial de movimientos de HEAD
git reflog

# Output:
# abc1234 HEAD@{0}: reset: moving to HEAD~3
# def5678 HEAD@{1}: commit: feat: mi feature
# ghi9012 HEAD@{2}: commit: add styles

# Recuperar el commit
git cherry-pick def5678
# o
git checkout def5678
git switch -c rama-recuperada
```

## 5. Separar un commit en varios

```bash
# Reset el commit pero mantener los cambios
git reset --soft HEAD~1

# Ahora selecciona que archivos committear
git add archivo1.js
git commit -m "feat: parte 1"

git add archivo2.js
git commit -m "feat: parte 2"
```

## 6. Reordenar commits

```bash
# Rebase interactivo
git rebase -i HEAD~3

# En el editor, cambia el orden de las lineas:
pick abc1234 segundo commit
pick def5678 primer commit
pick ghi9012 tercer commit
```

## 7. Quitar archivo del historial completo

```bash
# Eliminar un archivo de todos los commits
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch archivo-secreto.env" \
  --prune-empty -- --all

# Limpiar referencias
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Agregar a .gitignore para que no vuelva
echo "archivo-secreto.env" >> .gitignore
git add .gitignore
git commit -m "chore: ignore sensitive file"
```

## 8. Fix detached HEAD

```bash
# Estas en detached HEAD y quieres guardar estos cambios
git switch -c rama-nueva   # crear rama desde HEAD actual

# O volver a la rama anterior
git switch main
```

## 9. Saltar un commit en rebase

```bash
git rebase -i HEAD~5

# Cambiar "pick" a "skip" en el commit que quieres saltar:
pick abc1234 commit 1
skip def5678 commit 2  # este se salta
pick ghi9012 commit 3
```

## 10. Cambiar mensaje del ultimo commit

```bash
# Solo si no se pusheo
git commit --amend -m "nuevo mensaje"

# Si ya se pusheo (cuidado con force push)
git commit --amend -m "nuevo mensaje"
git push --force
```

## 11. Eliminar un archivo del staging (sin borrar del disco)

```bash
git reset HEAD archivo.js
```

## 12. Ver cambios sin committear

```bash
# Todos los cambios
git diff

# Solo los que estan en staging
git diff --staged

# Cambios en un archivo especifico
git diff archivo.js

# Resumen de cambios
git diff --stat
```
