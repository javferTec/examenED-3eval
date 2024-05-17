# Gitflow

Gitflow es un flujo de trabajo para Git que define un modelo de ramificación robusto y predecible, diseñado para proyectos de software con ciclos de vida de desarrollo y lanzamientos estructurados. Introducido por Vincent Driessen, Gitflow se basa en ramas específicas que separan el trabajo de desarrollo y las versiones listas para producción, facilitando la gestión del código y la colaboración entre equipos.

## Ramas

- **main**: Contiene el código listo para producción. Es la rama estable donde se realizan los lanzamientos finales.
- **hotfix**: Se utiliza para corregir errores críticos que afectan la versión en producción. Permite aplicar parches urgentes sin interrumpir el flujo de trabajo regular.
- **release**: Sirve para preparar el código para lanzar una nueva versión. Permite realizar pruebas y ajustes finales antes de la liberación.
- **develop**: Refleja el estado actual del desarrollo. Es la rama principal donde se integran todas las nuevas características y cambios antes de ser preparados para el lanzamiento.

## Pasos

### Crear Feature
```bash
git switch develop
git checkout -b feature/nombreFeature
```

### Finalizar Feature
```bash
git rebase develop # Desde la propia feature
git push -f # Desde la propia feature
git switch develop
git merge "feature/nombreFeature"
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

- **Etiqueta ligera**: `git tag nombreEtiqueta`
- **Etiqueta anotada**: `git tag -a nombreEtiqueta -m "mensaje X"`
- **Pushear la etiqueta**: `git push --tags`


[Inicio](../README.md)