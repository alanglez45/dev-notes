# Conceptos de Git

## Qué es Git

Git es un sistema de control de versiones distribuido. Registra cambios en archivos a lo largo del tiempo para poder consultar qué cambió, quién lo hizo, cuándo, y volver a versiones anteriores.

- **Distribuido:** cada desarrollador tiene una copia completa del historial
- **Snapshots:** cada commit es una instantánea del estado del proyecto
- **Branching:** permite crear ramas para trabajar en features aisladas

## Áreas de trabajo

```
Working Directory  →  Staging Area  →  Repository  →  Remote
   (archivos)          (git add)       (git commit)   (git push)
```

- **Working Directory:** archivos en tu disco
- **Staging Area:** archivos preparados para el proximo commit
- **Repository:** historial completo de commits
- **Remote:** repositorio en GitHub/GitLab/etc.

## Qué es un Commit

Un commit es un guardado del estado del proyecto en un momento dado. Cada commit tiene un hash único, un autor, un mensaje y apunta al commit anterior.

```bash
git add archivo.js                    # preparar cambios
git commit -m "feat: descripcion"     # crear commit
```

## HEAD

`HEAD` apunta al commit actual (la punta de la rama donde estas).

```
HEAD~1 = un commit atras
HEAD~2 = dos commits atras
HEAD~n = n commits atras
```

## Merge vs Rebase

| | Merge | Rebase |
|--|-------|--------|
| **Resultado** | Crea un commit de merge | Reaplica commits encima |
| **Historial** | Conserva la forma real | Historial lineal |
| **Conflictos** | Se resuelven una vez | Puede requerir resolver en cada commit |
| **Uso** | Ramas compartidas / produccion | Ramas personales / feature branches |

**Regla de oro:** No hacer rebase en ramas que otros estan usando.

## Qué es un Merge

Un merge es el proceso de combinar los cambios de dos ramas en una sola. Cuando terminas de trabajar en una feature branch, haces merge para integrar ese trabajo en main.

```bash
git switch main
git merge feature/nueva-funcionalidad
```

## Fast-forward Merge

Cuando la rama destino no tiene commits nuevos, Git solo mueve el puntero:

```
Antes:
main:    A → B → C
feature:       C → D

Merge (fast-forward):
main:    A → B → C → D
```

## Three-way Merge

Cuando ambas ramas tienen commits nuevos, Git crea un commit de merge:

```
Antes:
main:    A → B → C → E
feature:       C → D

Merge:
main:    A → B → C → E → M (merge commit)
                  └── D ──┘
```

## Detached HEAD

Cuando `HEAD` apunta directamente a un commit en vez de a una rama. Puede pasar al hacer `git checkout <commit-hash>`.

```
HEAD → commit C (no a una rama)

Solucion:
git switch -c nueva-rama   # crear rama desde ahi
git switch main            # volver a una rama
```

## .gitignore

Archivo que le dice a Git que ignorar.

```gitignore
# Dependencias
node_modules/

# Build
dist/
build/

# Env
.env
.env.local

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
```
