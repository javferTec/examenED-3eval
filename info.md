# Gitflow

## Ramas

- **main**: Contiene el código listo para producción.
- **hotfix**: Corrigen errores críticos en producción.
- **release**: Sirve para preparar el código para lanzar una nueva versión.
- **develop**: Refleja el estado actual del desarrollo.

## Pasos

### Crear Feature
```bash
git switch develop
git checkout -b feature/nombreFeature
```

### Finalizar Feature
```bash
git rebase develop
git push -f
git switch develop
git merge "feature/nombre"
```

### Crear Release
```bash
git switch develop
git checkout -b release/nombreRelease
```

### Finalizar Release
```bash
git switch main
git merge release/nombreRelease
git switch develop
git merge release/nombreRelease
# Se pueden crear etiquetas con la versión
```

### Crear Hotfix
```bash
git switch main
git checkout -b hotfix/nombreHotfix
```

### Finalizar Hotfix
```bash
git switch main
git merge hotfix/nombreHotfix
git switch develop
git merge hotfix/nombreHotfix
```

## Etiquetas

- Etiqueta ligera: `git tag nombreEtiqueta`
- Etiqueta anotada: `git tag -a nombreEtiqueta -m "mensaje X"`
- Pushear la etiqueta: `git push --tags`
