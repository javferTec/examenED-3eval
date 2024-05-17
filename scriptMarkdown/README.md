```markdown
# GITFLOW AUTOMATIZADO (HISTÓRICO LINEAL) - ENTORNOS DE DESARROLLO

## Funcionamiento

Para simplificar el flujo de trabajo con GitFlow, se han creado una serie de comandos personalizados para automatizar las tareas comunes. A continuación se detallan los comandos disponibles:

- `git newfeature <nombre>`: Crea una nueva rama de característica (feature) a partir de la rama `develop`. Verifica si la rama de característica ya existe y, si no, cambia a `develop` (creándola desde `main` si es necesario), hace un rebase con `develop` y crea la nueva rama de característica a partir de ahí.
- `git finishfeature`: Finaliza la rama de característica actual, fusionándola con la rama `develop`. Verifica si la rama actual es una rama de característica, realiza un rebase con `develop`, fusiona la rama de característica actual en `develop` y actualiza `develop`.
- `git newrelease <nombre>`: Crea una nueva rama de release a partir de la rama `develop`. Verifica si la rama de release ya existe y, si no, verifica si `develop` existe (creándola desde `main` si es necesario), cambia a `develop`, y crea la nueva rama de release a partir de ahí.
- `git finishrelease`: Finaliza la rama de release actual, fusionándola con las ramas `main` y `develop`, y etiquetando la versión. Verifica si la rama actual es una rama de release, fusiona la rama de release actual en `main` y `develop`, crea una etiqueta para la versión y elimina la rama de release.
- `git newhotfix <nombre>`: Crea una nueva rama de hotfix a partir de la rama `main`. Verifica si la rama de hotfix ya existe y, si no, cambia a `main` y crea la nueva rama de hotfix a partir de ahí.
- `git finishhotfix`: Finaliza la rama de hotfix actual, fusionándola con las ramas `main` y `develop`. Verifica si la rama actual es una rama de hotfix, realiza un rebase con `main`, fusiona la rama de hotfix actual en `main` y `develop`, y elimina la rama de hotfix.

### Ramas HotFix

Las ramas de hotfix se utilizan para corregir problemas críticos en la producción. Es importante finalizarlas rápidamente y fusionarlas tanto con `main` como con `develop` para mantener la integridad del código en todas las ramas.

## [Alias]

Para facilitar el uso de estos comandos, puedes añadir los siguientes alias a tu configuración de Git:

```ini
[Alias]
    newfeature = "!f() { \
        branch_name=\"feature/$1\"; \
        if git show-ref --verify --quiet \"refs/heads/$branch_name\"; then \
            echo \"La rama $branch_name ya existe\"; \
        else \
            git switch develop || (git switch -c develop main && git push -u origin develop && echo \"Nueva rama develop creada y pusheada\"); \
            git rebase develop && git switch develop && git checkout -b $branch_name; \
        fi \
    }; f"

    finishfeature = "!f() { \
        current_branch=$(git rev-parse --abbrev-ref HEAD); \
        if [[ \"$current_branch\" == feature/* ]]; then \
            git rebase develop && git switch develop && git pull && git merge $current_branch; \
        else \
            echo \"Debes estar en una rama de feature para finalizar la característica\"; \
        fi \
    }; f"

    newrelease = "!f() { \
        branch_name=\"release/$1\"; \
        if git show-ref --verify --quiet \"refs/heads/$branch_name\"; then \
            echo \"La rama $branch_name ya existe\"; \
        else \
            if ! git show-ref --verify --quiet \"refs/heads/develop\"; then \
            git switch -c develop main; \
            echo \"Nueva rama develop creada\"; \
            fi; \
            git switch develop && git checkout -b $branch_name && git rebase develop; \
        fi \
    }; f"

    finishrelease = "!f() { \
        current_branch=$(git rev-parse --abbrev-ref HEAD); \
        if [[ \"$current_branch\" == release/* ]]; then \
            release_name=\"${current_branch#release/}\"; \
            git switch main && git merge $current_branch && \
            git switch develop && git merge $current_branch && \
            git tag -a $release_name -m \"Release $release_name\" && \
            echo \"Etiqueta $release_name creada y release finalizada\"; \
        else \
            echo \"Debes estar en una rama de release para finalizar la release\"; \
        fi \
    }; f"

    newhotfix = "!f() { \
        branch_name=\"hotfix/$1\"; \
        if git show-ref --verify --quiet \"refs/heads/$branch_name\"; then \
            echo \"La rama $branch_name ya existe\"; \
        else \
            git switch main && git checkout -b $branch_name; \
        fi \
    }; f"

    finishhotfix = "!f() { \
        current_branch=$(git rev-parse --abbrev-ref HEAD); \
        if [[ \"$current_branch\" == hotfix/* ]]; then \
            git rebase main && git switch main && git merge $current_branch && \
            git switch develop && git merge $current_branch; \
        else \
            echo \"Debes estar en una rama de hotfix para finalizar el hotfix\"; \
        fi \
    }; f"

```

### Auto setup remote branch with push.autoSetupRemote

Para configurar automáticamente el seguimiento de ramas remotas al hacer push, puedes ejecutar el siguiente comando:

```bash
git config --global --add --bool push.autoSetupRemote true
```
