## Comandos útiles adicionales (Detallados):

### Información y navegación
```bash
# Ver información detallada del repositorio
git remote show origin
# Muestra: URLs, ramas remotas, configuración de push/pull

# Ver commits con estadísticas
git log --stat              # Archivos modificados por commit
git log --shortstat         # Estadísticas resumidas
git log --name-only         # Solo nombres de archivos
git log --name-status       # Nombres y estado (A/M/D)

# Navegación por commits
git show HEAD               # Mostrar último commit
git show HEAD~2             # Mostrar commit de hace 2 commits
git show a1b2c3d            # Mostrar commit específico
git show a1b2c3d:archivo.txt # Mostrar archivo en commit específico

# Buscar en historial
git log --grep="bug"        # Commits con "bug" en mensaje
git log --author="Juan"     # Commits de autor específico
git log --since="2024-01-01" # Commits desde fecha
git log --until="2024-12-31" # Commits hasta fecha
git log --oneline --graph --all # Historial visual completo

# Buscar código en archivos
git grep "función"          # Buscar texto en archivos actuales
git grep "función" HEAD~5   # Buscar en commit específico
git grep -n "función"       # Mostrar números de línea
git grep -i "función"       # Búsqueda case-insensitive
```

### Trabajar con archivos específicos
```bash
# Historial de un archivo específico
git log --follow archivo.txt    # Incluye renombres
git log -p archivo.txt          # Con diferencias
git log --oneline archivo.txt   # Resumido

# Ver quién modificó cada línea (blame)
git blame archivo.txt           # Autor de cada línea
git blame -L 10,20 archivo.txt  # Solo líneas 10-20
git blame -C archivo.txt        # Detectar código movido/copiado

# Comparar archivos entre commits
git diff HEAD~1 HEAD archivo.txt
git diff rama1 rama2 archivo.txt
git diff --word-diff archivo.txt # Diferencias por palabra

# Restaurar archivo de commit específico
git checkout a1b2c3d -- archivo.txt
git restore --source=HEAD~2 archivo.txt
```

### Trabajo con tags (versiones)
```bash
# Crear tags
git tag v1.0                    # Tag ligero
git tag -a v1.0 -m "Versión 1.0" # Tag anotado (recomendado)
git tag -a v1.1 a1b2c3d -m "Versión 1.1" # Tag en commit específico

# Ver tags
git tag                         # Listar todos los tags
git tag -l "v1.*"              # Tags que coinciden con patrón
git show v1.0                  # Información del tag

# Subir tags
git push origin v1.0           # Subir tag específico
git push origin --tags         # Subir todos los tags

# Trabajar con tags
git checkout v1.0              # Ir a versión específica
git checkout -b hotfix-v1.0 v1.0 # Crear rama desde tag

# Eliminar tags
git tag -d v1.0                # Eliminar tag local
git push origin --delete v1.0  # Eliminar tag remoto
```

### Configuración avanzada
```bash
# Configurar alias útiles
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
git config --global alias.lg 'log --oneline --graph --all'

# Configurar comportamiento
git config --global push.default simple
git config --global pull.rebase true
git config --global init.defaultBranch main
git config --global core.autocrlf true    # Windows
git config --global core.autocrlf input   # Mac/Linux

# Configurar editor
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "nano"         # Nano
git config --global core.editor "vim"          # Vim

# Ver toda la configuración
git config --list
git config --global --list
git config --local --list
```

### Comandos para limpieza y mantenimiento
```bash
# Limpiar archivos no rastreados
git clean -n                   # Ver qué se eliminaría (dry run)
git clean -f                   # Eliminar archivos no rastreados
git clean -fd                  # Eliminar archivos y directorios
git clean -fX                  # Eliminar solo archivos en .gitignore
git clean -fx                  # Eliminar todo (incluso .gitignore)

# Optimizar repositorio
git gc                         # Garbage collection
git gc --aggressive            # Limpieza más agresiva
git repack -ad                 # Reempaquetar objetos
git prune                      # Eliminar objetos no referenciados

# Verificar integridad
git fsck --full               # Verificar consistencia
git count-objects -v          # Estadísticas del repositorio
```

### Hooks de Git (automatización)
```bash
# Ubicación de hooks
ls .git/hooks/
# Archivos: pre-commit, post-commit, pre-push, etc.

# Ejemplo de pre-commit hook (ejecutar tests antes de commit)
# Crear archivo .git/hooks/pre-commit:
#!/bin/sh
npm test
if [ $? -ne 0 ]; then
  echo "Tests fallaron. Commit cancelado."
  exit 1
fi

# Hacer ejecutable
chmod +x .git/hooks/pre-commit
```

### Subtrees y Submodules
```bash
# Submodules (para incluir otros repositorios)
git submodule add https://github.com/usuario/libreria.git lib
git submodule update --init --recursive
git submodule foreach git pull origin main

# Subtrees (alternativa a submodules)
git subtree add --prefix=lib https://github.com/usuario/libreria.git main
git subtree pull --prefix=lib https://github.com/usuario/libreria.git main
```

### Comandos de diagnóstico y recuperación
```bash
# Encontrar commits perdidos
git reflog                     # Historial de cambios de HEAD
git reflog expire --expire=now --all # Limpiar reflog
git fsck --unreachable         # Objetos no alcanzables

# Recuperar commits eliminados
git reflog
git checkout a1b2c3d           # Ir al commit perdido
git checkout -b recuperacion   # Crear rama desde ahí

# Reparar repositorio corrupto
git fsck --full                # Verificar integridad
git gc --aggressive            # Limpiar y optimizar
```

### Trabajo con patches
```bash
# Crear patches
git format-patch HEAD~3        # Crear patches de últimos 3 commits
git format-patch main..feature # Patches de diferencias entre ramas
git diff > cambios.patch       # Crear patch de cambios actuales

# Aplicar patches
git apply cambios.patch        # Aplicar patch
git am *.patch                 # Aplicar patches con commits
```

### Comandos de comparación avanzados
```bash
# Comparar ramas
git diff main..feature         # Diferencias entre ramas
git diff main...feature        # Diferencias desde ancestro común
git log main..feature          # Commits en feature no en main
git log --left-right main...feature # Commits únicos en cada rama

# Estadísticas de diferencias
git diff --stat main feature
git diff --numstat main feature
git diff --shortstat main feature

# Comparar con herramientas externas
git difftool                   # Usar herramienta externa
git mergetool                  # Resolver conflictos con herramienta externa
```# Manual de Git y GitHub para Principiantes

## ¿Qué es Git y por qué es importante?

Git es un sistema de control de versiones que te permite rastrear los cambios en tu código a lo largo del tiempo. Es como tener un "historial de guardado" súper potente que te permite:

- Ver qué cambios hiciste y cuándo
- Volver a versiones anteriores de tu código
- Colaborar con otros programadores sin conflictos
- Mantener diferentes versiones de tu proyecto

GitHub es una plataforma en línea que usa Git para alojar repositorios de código, facilitando la colaboración y el intercambio de proyectos.

## Instalación y Configuración Inicial

### Instalar Git
- **Windows**: Descarga desde [git-scm.com](https://git-scm.com)
- **Mac**: Usar Homebrew (`brew install git`) o descargar desde el sitio oficial
- **Linux**: `sudo apt install git` (Ubuntu/Debian) o `sudo yum install git` (CentOS/RHEL)

### Configuración inicial
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-email@ejemplo.com"
```

## Conceptos Fundamentales

### Repository (Repositorio)
Es la carpeta donde Git guarda todo el historial de tu proyecto. Puede estar en tu computadora (local) o en GitHub (remoto).

### Commit
Un "commit" es como una fotografía de tu código en un momento específico. Cada commit tiene un mensaje que describe qué cambios se hicieron.

### Branch (Rama)
Las ramas te permiten trabajar en diferentes características sin afectar el código principal. La rama principal se llama `main` o `master`.

### Working Directory, Staging Area, Repository
- **Working Directory**: Donde trabajas y modificas archivos
- **Staging Area**: Donde preparas los cambios antes de confirmarlos
- **Repository**: Donde se guardan permanentemente los commits

## Comandos Esenciales de Git (Detallados)

### Inicializar un repositorio
```bash
git init
```
**Qué hace**: Convierte una carpeta normal en un repositorio Git, creando una carpeta oculta `.git` que contiene toda la información del control de versiones.

**Cuándo usar**: Al empezar un nuevo proyecto desde cero.

**Ejemplo práctico**:
```bash
mkdir mi-proyecto
cd mi-proyecto
git init
# Salida: Initialized empty Git repository in /ruta/mi-proyecto/.git/
```

### Ver el estado de tu repositorio
```bash
git status
```
**Qué hace**: Muestra el estado actual del repositorio, incluyendo archivos modificados, archivos en staging, y archivos no rastreados.

**Ejemplo de salida**:
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   index.html

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
        modified:   style.css

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        script.js
```

### Agregar archivos al staging area
```bash
git add archivo.txt          # Agregar un archivo específico
git add .                    # Agregar todos los archivos del directorio actual
git add *.js                 # Agregar todos los archivos .js
git add carpeta/            # Agregar todos los archivos de una carpeta
git add -A                  # Agregar todos los archivos del repositorio
git add -u                  # Agregar solo archivos ya rastreados que fueron modificados
```

**Diferencias importantes**:
- `git add .` - Solo archivos del directorio actual y subdirectorios
- `git add -A` - Todos los archivos del repositorio completo
- `git add -u` - Solo archivos ya conocidos por Git que fueron modificados

**Ejemplo práctico**:
```bash
# Tienes estos archivos: index.html (modificado), new-file.js (nuevo), deleted-file.txt (eliminado)
git add index.html          # Solo index.html va al staging
git add .                   # index.html y new-file.js van al staging
git add -A                  # index.html, new-file.js, y la eliminación de deleted-file.txt
```

### Hacer un commit
```bash
git commit -m "Mensaje descriptivo del cambio"
git commit                  # Abre editor para mensaje largo
git commit -am "Mensaje"    # Add + commit para archivos ya rastreados
git commit --amend          # Modificar el último commit
```

**Opciones útiles**:
- `-m "mensaje"` - Mensaje corto en línea
- `-a` - Automatically stage archivos modificados (no nuevos)
- `--amend` - Modificar el último commit (cambiar mensaje o añadir archivos)

**Ejemplo práctico**:
```bash
git add .
git commit -m "Añadir página de inicio con navegación básica"

# O más rápido para archivos ya rastreados:
git commit -am "Corregir error en función de validación"
```

### Ver el historial de commits
```bash
git log                      # Historial completo con detalles
git log --oneline           # Una línea por commit
git log --graph             # Gráfico de ramas
git log --stat              # Estadísticas de archivos modificados
git log -p                  # Muestra las diferencias en cada commit
git log --since="2024-01-01" # Commits desde una fecha
git log --author="Juan"     # Commits de un autor específico
git log -n 5               # Solo los últimos 5 commits
git log --grep="bug"       # Commits con "bug" en el mensaje
```

**Ejemplo de salida**:
```bash
# git log --oneline
a1b2c3d (HEAD -> main) Añadir función de login
e4f5g6h Corregir error en validación
i7j8k9l Crear página de inicio

# git log --graph --oneline
* a1b2c3d (HEAD -> main) Añadir función de login
* e4f5g6h Corregir error en validación
* i7j8k9l Crear página de inicio
```

### Ver diferencias
```bash
git diff                    # Diferencias entre working directory y staging
git diff --staged           # Diferencias entre staging y último commit
git diff HEAD               # Diferencias entre working directory y último commit
git diff archivo.txt        # Diferencias solo en un archivo
git diff HEAD~1             # Diferencias con el commit anterior
git diff rama1 rama2        # Diferencias entre dos ramas
git diff --name-only        # Solo nombres de archivos modificados
```

**Ejemplo de salida**:
```bash
# git diff
diff --git a/index.html b/index.html
index 1234567..abcdefg 100644
--- a/index.html
+++ b/index.html
@@ -1,4 +1,4 @@
 <html>
 <head>
-    <title>Mi Página</title>
+    <title>Mi Página Web</title>
 </head>
```

### Trabajar con ramas
```bash
git branch                  # Ver todas las ramas locales
git branch -r              # Ver ramas remotas
git branch -a              # Ver todas las ramas (locales y remotas)
git branch nueva-rama       # Crear una nueva rama
git branch -d rama-vieja    # Eliminar una rama (solo si está fusionada)
git branch -D rama-vieja    # Forzar eliminación de rama
git checkout nueva-rama     # Cambiar a una rama
git checkout -b nueva-rama  # Crear y cambiar a una rama nueva
git switch nueva-rama       # Cambiar a una rama (comando más nuevo)
git switch -c nueva-rama    # Crear y cambiar a una rama nueva
```

**Ejemplo práctico**:
```bash
# Crear una nueva funcionalidad
git checkout -b feature/login
# Trabajar en la rama...
git add .
git commit -m "Implementar sistema de login"

# Volver a main y fusionar
git checkout main
git merge feature/login
git branch -d feature/login  # Eliminar la rama ya fusionada
```

### Fusionar ramas (merge)
```bash
git merge rama-origen       # Fusionar rama-origen en la rama actual
git merge --no-ff rama      # Fusionar sin fast-forward (crea commit de merge)
git merge --squash rama     # Fusionar todos los commits como uno solo
git merge --abort           # Cancelar merge en caso de conflictos
```

**Ejemplo con conflictos**:
```bash
git merge feature/nueva-funcionalidad
# Si hay conflictos, Git te muestra:
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.

# Resolver conflictos manualmente en los archivos, luego:
git add .
git commit -m "Resolver conflictos de merge"
```

### Deshacer cambios
```bash
# Descartar cambios en working directory
git checkout -- archivo.txt    # Archivo específico
git checkout -- .              # Todos los archivos
git restore archivo.txt         # Comando más nuevo (equivalente)

# Quitar archivos del staging area
git reset HEAD archivo.txt     # Archivo específico
git reset HEAD                 # Todos los archivos
git restore --staged archivo.txt # Comando más nuevo

# Deshacer commits
git reset --soft HEAD~1        # Mantener cambios en staging
git reset --mixed HEAD~1       # Mantener cambios en working directory
git reset --hard HEAD~1        # ELIMINAR todos los cambios (¡CUIDADO!)

# Revertir un commit específico (crea nuevo commit)
git revert a1b2c3d             # Revierte el commit a1b2c3d
```

**Diferencias importantes en reset**:
- `--soft`: Mueve HEAD pero mantiene staging y working directory
- `--mixed` (default): Mueve HEAD y resetea staging, mantiene working directory
- `--hard`: Mueve HEAD y resetea staging y working directory (¡PELIGROSO!)

### Trabajar con archivos específicos
```bash
# Ver historial de un archivo
git log -- archivo.txt
git log -p -- archivo.txt      # Con diferencias

# Ver quién modificó cada línea
git blame archivo.txt

# Mostrar archivo en commit específico
git show HEAD~2:archivo.txt
git show a1b2c3d:archivo.txt

# Recuperar archivo de commit específico
git checkout a1b2c3d -- archivo.txt
```

## Trabajando con GitHub (Comandos Remotos Detallados)

### Crear una cuenta
Ve a [github.com](https://github.com) y crea tu cuenta gratuita.

### Trabajar con repositorios remotos
```bash
# Ver repositorios remotos configurados
git remote -v

# Agregar un repositorio remoto
git remote add origin https://github.com/tu-usuario/tu-repositorio.git
git remote add upstream https://github.com/usuario-original/repositorio.git

# Cambiar URL del repositorio remoto
git remote set-url origin https://github.com/nuevo-usuario/repositorio.git

# Eliminar un repositorio remoto
git remote remove origin

# Renombrar un repositorio remoto
git remote rename origin nuevo-nombre
```

### Clonar un repositorio existente
```bash
git clone https://github.com/usuario/repositorio.git
git clone https://github.com/usuario/repositorio.git nuevo-nombre
git clone --branch rama-especifica https://github.com/usuario/repositorio.git
git clone --depth 1 https://github.com/usuario/repositorio.git  # Clone superficial
```

**Opciones útiles**:
- `--branch` - Clonar una rama específica
- `--depth 1` - Clonar solo el commit más reciente (más rápido)
- `--recursive` - Clonar incluyendo submódulos

### Sincronizar cambios - FETCH vs PULL
```bash
# FETCH: Descargar cambios SIN fusionar
git fetch origin            # Descargar desde origin
git fetch --all            # Descargar desde todos los remotos
git fetch origin rama-name  # Descargar una rama específica

# PULL: Descargar Y fusionar (equivale a fetch + merge)
git pull                    # Desde la rama remota configurada
git pull origin main        # Desde rama específica de origin
git pull --rebase          # Usar rebase en lugar de merge
git pull --no-rebase       # Forzar merge (útil si tienes rebase configurado)
```

**Diferencia importante**:
- `git fetch` - Solo descarga, no modifica tu código
- `git pull` - Descarga y fusiona automáticamente

### Subir cambios - PUSH detallado
```bash
# Push básico
git push                    # Subir rama actual al remoto configurado
git push origin main        # Subir rama main a origin
git push origin rama-name   # Subir rama específica

# Push con opciones
git push -u origin main     # Subir Y configurar tracking (solo primera vez)
git push --set-upstream origin nueva-rama  # Equivalente a -u
git push --force           # Forzar push (¡CUIDADO!)
git push --force-with-lease # Forzar push más seguro
git push --all             # Subir todas las ramas
git push --tags            # Subir todos los tags
git push origin --delete rama-name  # Eliminar rama remota

# Push de tags
git push origin v1.0       # Subir un tag específico
git push origin --tags     # Subir todos los tags
```

### Trabajar con ramas remotas
```bash
# Ver ramas remotas
git branch -r              # Solo ramas remotas
git branch -a              # Todas las ramas (locales y remotas)

# Crear rama local desde remota
git checkout -b local-rama origin/rama-remota
git switch -c local-rama origin/rama-remota

# Hacer seguimiento de rama remota
git branch --set-upstream-to=origin/main main

# Eliminar referencia a rama remota eliminada
git remote prune origin
```

### Resolver conflictos en pull/merge
```bash
# Cuando hay conflictos en pull:
git pull origin main
# Git te muestra:
Auto-merging archivo.txt
CONFLICT (content): Merge conflict in archivo.txt
Automatic merge failed; fix conflicts and then commit the result.

# Los archivos con conflictos se ven así:
<<<<<<< HEAD
Tu código local
=======
Código del repositorio remoto
>>>>>>> commit-hash

# Después de resolver manualmente:
git add archivo.txt
git commit -m "Resolver conflictos de merge"
```

### Comandos avanzados para colaboración
```bash
# Configurar push default
git config push.default current    # Push solo rama actual
git config push.default simple     # Push solo si hay tracking

# Trabajar con forks
git remote add upstream https://github.com/usuario-original/repo.git
git fetch upstream
git merge upstream/main

# Crear pull request desde línea de comandos (requiere gh CLI)
gh pr create --title "Título" --body "Descripción"
```

### Autenticación con GitHub
```bash
# Configurar credenciales (una vez)
git config --global credential.helper store

# Para repositorios privados, usar token personal:
# 1. Generar token en GitHub Settings > Developer settings > Personal access tokens
# 2. Usar token como contraseña cuando Git la solicite
# 3. O configurar en la URL:
git remote set-url origin https://tu-token@github.com/usuario/repo.git
```

## Flujo de Trabajo Detallado

### Para un proyecto nuevo (paso a paso):
```bash
# 1. Crear directorio y navegar
mkdir mi-proyecto
cd mi-proyecto

# 2. Inicializar Git
git init
# Salida: Initialized empty Git repository in /ruta/mi-proyecto/.git/

# 3. Crear archivo README inicial
echo "# Mi Proyecto" > README.md

# 4. Agregar archivo al staging
git add README.md
# O agregar todos: git add .

# 5. Verificar estado
git status
# Salida: Changes to be committed: new file: README.md

# 6. Hacer primer commit
git commit -m "Primer commit: agregar README"

# 7. Crear repositorio en GitHub (desde la web)
# Copiar URL del repositorio creado

# 8. Conectar con GitHub
git remote add origin https://github.com/tu-usuario/mi-proyecto.git

# 9. Verificar conexión
git remote -v
# Salida: origin https://github.com/tu-usuario/mi-proyecto.git (fetch)
#         origin https://github.com/tu-usuario/mi-proyecto.git (push)

# 10. Subir código (primera vez)
git push -u origin main
# -u establece tracking para futuros git push
```

### Para trabajo diario (rutina detallada):
```bash
# 1. Siempre empezar actualizando
git status                  # Ver estado actual
git pull origin main       # Descargar últimos cambios

# 2. Verificar en qué rama estás
git branch
# Salida: * main (el asterisco indica rama actual)

# 3. Trabajar en tu código
# ... editar archivos ...

# 4. Verificar cambios antes de commitear
git status                  # Ver archivos modificados
git diff                    # Ver diferencias específicas
git diff archivo.txt        # Ver cambios en archivo específico

# 5. Agregar cambios al staging
git add archivo.txt         # Archivo específico
# O para todos los cambios:
git add .

# 6. Verificar staging area
git status
# Salida: Changes to be committed: modified: archivo.txt

# 7. Commitear con mensaje descriptivo
git commit -m "Corregir validación de email en formulario de registro"

# 8. Subir cambios
git push origin main
# O simplemente: git push (si ya tienes tracking configurado)
```

### Para trabajar en una nueva característica (feature branch):
```bash
# 1. Asegurarte de estar en main y actualizado
git checkout main
git pull origin main

# 2. Crear nueva rama para la característica
git checkout -b feature/sistema-login
# Naming convention: feature/nombre-descriptivo

# 3. Verificar que estás en la nueva rama
git branch
# Salida: * feature/sistema-login
#         main

# 4. Trabajar en la nueva característica
# ... desarrollar código ...

# 5. Commitear cambios en la rama feature
git add .
git commit -m "Implementar validación de credenciales"

# 6. Más trabajo y commits según necesites
git add .
git commit -m "Agregar manejo de errores en login"

# 7. Subir rama feature a GitHub
git push -u origin feature/sistema-login

# 8. Cuando la característica esté lista, volver a main
git checkout main
git pull origin main        # Asegurarse de tener última versión

# 9. Fusionar la característica
git merge feature/sistema-login
# O usar: git merge --no-ff feature/sistema-login (para mantener historial)

# 10. Subir main actualizado
git push origin main

# 11. Eliminar rama feature (ya no necesaria)
git branch -d feature/sistema-login      # Local
git push origin --delete feature/sistema-login  # Remoto
```

### Para colaborar en proyecto existente:
```bash
# 1. Clonar el repositorio
git clone https://github.com/usuario/proyecto.git
cd proyecto

# 2. Ver ramas disponibles
git branch -a
# Salida: * main
#         remotes/origin/main
#         remotes/origin/develop

# 3. Crear tu rama de trabajo
git checkout -b feature/mi-contribucion

# 4. Hacer tus cambios y commits
git add .
git commit -m "Agregar nueva funcionalidad"

# 5. Antes de hacer push, actualizar con cambios remotos
git checkout main
git pull origin main
git checkout feature/mi-contribucion
git rebase main             # O git merge main

# 6. Subir tu rama
git push -u origin feature/mi-contribucion

# 7. Crear Pull Request desde GitHub web interface
```

### Para trabajar con un fork:
```bash
# 1. Clonar tu fork
git clone https://github.com/tu-usuario/proyecto-fork.git
cd proyecto-fork

# 2. Agregar repositorio original como upstream
git remote add upstream https://github.com/usuario-original/proyecto.git

# 3. Verificar remotos
git remote -v
# Salida: origin    https://github.com/tu-usuario/proyecto-fork.git (fetch)
#         origin    https://github.com/tu-usuario/proyecto-fork.git (push)
#         upstream  https://github.com/usuario-original/proyecto.git (fetch)
#         upstream  https://github.com/usuario-original/proyecto.git (push)

# 4. Mantener tu fork actualizado
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

# 5. Crear rama para tu contribución
git checkout -b feature/mi-mejora

# 6. Desarrollar y commitear
git add .
git commit -m "Implementar mejora solicitada"

# 7. Subir a tu fork
git push -u origin feature/mi-mejora

# 8. Crear Pull Request desde GitHub hacia el repositorio original
```

## Mejores Prácticas

### Mensajes de commit claros
- **Bueno**: "Añadir función de login de usuario"
- **Malo**: "cambios" o "fix"

### Commits pequeños y frecuentes
Es mejor hacer muchos commits pequeños que un commit gigante con muchos cambios.

### Usar ramas para características nuevas
No trabajes directamente en `main`. Crea una rama para cada nueva característica.

### Revisar antes de hacer commit
Siempre usa `git status` y `git diff` antes de hacer commit para ver qué estás guardando.

## Comandos de Emergencia (Detallados)

### Guardar trabajo temporalmente - STASH
```bash
# Guardar cambios temporalmente
git stash                   # Guardar cambios con mensaje automático
git stash push -m "Mensaje" # Guardar con mensaje personalizado
git stash -u               # Incluir archivos no rastreados
git stash --include-untracked # Equivalente a -u

# Ver stashes guardados
git stash list
# Salida ejemplo:
# stash@{0}: WIP on main: a1b2c3d último commit
# stash@{1}: On feature: trabajo en progreso

# Aplicar stash
git stash pop              # Aplicar último stash y eliminarlo
git stash apply            # Aplicar último stash sin eliminarlo
git stash apply stash@{1}  # Aplicar stash específico
git stash drop             # Eliminar último stash
git stash drop stash@{1}   # Eliminar stash específico
git stash clear            # Eliminar todos los stashes
```

**Cuándo usar stash**:
- Necesitas cambiar de rama rápidamente
- Quieres hacer pull pero tienes cambios sin commitear
- Experimentaste algo que no quieres commitear aún

### Recuperar trabajo perdido
```bash
# Ver commits "perdidos" (no referenciados)
git reflog                 # Historial completo de HEAD
git reflog --all          # Historial de todas las referencias

# Recuperar commit específico
git checkout a1b2c3d       # Ir al commit
git cherry-pick a1b2c3d    # Aplicar commit a rama actual
git merge a1b2c3d         # Fusionar commit

# Recuperar rama eliminada
git reflog
git checkout -b rama-recuperada a1b2c3d  # Donde a1b2c3d era el último commit
```

### Modificar historial - RESET vs REVERT
```bash
# RESET: Mover HEAD (modifica historial)
git reset --soft HEAD~1    # Mantener cambios en staging
git reset --mixed HEAD~1   # Mantener cambios en working directory (default)
git reset --hard HEAD~1    # ELIMINAR todo (¡PELIGROSO!)

# REVERT: Crear commit que deshace cambios (NO modifica historial)
git revert HEAD            # Revertir último commit
git revert a1b2c3d         # Revertir commit específico
git revert --no-edit HEAD  # Revertir sin abrir editor
git revert HEAD~3..HEAD    # Revertir últimos 3 commits
```

**Cuándo usar cada uno**:
- `git reset`: Para cambios locales que NO has subido (push)
- `git revert`: Para cambios que YA subiste al repositorio remoto

### Limpiar repositorio
```bash
# Ver qué archivos se eliminarían
git clean -n               # Dry run - solo mostrar
git clean -f               # Eliminar archivos no rastreados
git clean -f -d            # Eliminar archivos y directorios
git clean -f -x            # Eliminar incluso archivos en .gitignore
git clean -i               # Modo interactivo
```

### Reparar problemas comunes
```bash
# Si el repositorio está corrupto
git fsck --full

# Si hay problemas con el index
git read-tree HEAD
git checkout-index -f -a

# Si push es rechazado por historiales divergentes
git pull --rebase origin main  # Rebase local sobre remoto
# O si estás seguro de tu versión:
git push --force-with-lease    # Más seguro que --force

# Si commiteas en rama incorrecta
git reset --soft HEAD~1       # Deshacer commit pero mantener cambios
git stash                     # Guardar cambios
git checkout rama-correcta    # Cambiar a rama correcta
git stash pop                 # Aplicar cambios
git commit -m "Mensaje"       # Commitear en rama correcta
```

### Cambiar commits ya hechos
```bash
# Cambiar mensaje del último commit
git commit --amend -m "Nuevo mensaje"

# Agregar archivos al último commit
git add archivo-olvidado.txt
git commit --amend --no-edit

# Cambiar múltiples commits (REBASE INTERACTIVO)
git rebase -i HEAD~3          # Cambiar últimos 3 commits
# En el editor que se abre:
# pick a1b2c3d Primer commit
# reword e4f5g6h Segundo commit    # Cambiar mensaje
# edit i7j8k9l Tercer commit       # Editar commit

# Cuando Git se detiene en 'edit':
# Hacer cambios necesarios
git add .
git commit --amend
git rebase --continue
```

### Comandos de diagnóstico
```bash
# Ver configuración actual
git config --list
git config --global --list
git config --local --list

# Ver información del repositorio
git remote show origin
git branch -vv                # Ramas con información de tracking
git log --oneline --graph --all --decorate

# Verificar integridad
git fsck
git count-objects -v
```

## Recursos Adicionales

### Archivos importantes:
- **`.gitignore`**: Lista de archivos que Git debe ignorar
- **`README.md`**: Documentación de tu proyecto

### Ejemplo de .gitignore completo por tecnología:

#### JavaScript/Node.js
```gitignore
# Dependencias
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Archivos de producción
dist/
build/
*.min.js
*.min.css

# Variables de entorno
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# Cache
.npm
.yarn-integrity
.eslintcache

# Logs
logs/
*.log
```

#### Python
```gitignore
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# Virtual environments
venv/
env/
ENV/
.venv

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# PyInstaller
*.manifest
*.spec

# Jupyter Notebook
.ipynb_checkpoints

# pyenv
.python-version

# pytest
.pytest_cache/

# Coverage reports
htmlcov/
.coverage
.coverage.*
coverage.xml
```

#### Java
```gitignore
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs
hs_err_pid*

# IDE
.idea/
*.iml
*.ipr
*.iws
.vscode/

# Maven
target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties

# Gradle
.gradle/
build/
```

#### General (todos los proyectos)
```gitignore
# Archivos del sistema
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Archivos temporales
*.tmp
*.temp
*.swp
*.swo
*~

# Archivos de configuración local
config.local.*
*.local

# Archivos de backup
*.backup
*.bak
*.old

# Archivos de IDE
.vscode/
.idea/
*.sublime-*

# Archivos de prueba
test-results/
coverage/
```

### Comandos para trabajar con .gitignore:
```bash
# Crear .gitignore básico
touch .gitignore

# Agregar .gitignore al repositorio
git add .gitignore
git commit -m "Agregar .gitignore"

# Ignorar archivos ya rastreados
echo "archivo.txt" >> .gitignore
git rm --cached archivo.txt
git commit -m "Dejar de rastrear archivo.txt"

# Ver qué archivos serían ignorados
git status --ignored

# Verificar por qué un archivo es ignorado
git check-ignore -v archivo.txt

# Forzar agregar archivo ignorado
git add -f archivo.txt
```

### Comandos útiles adicionales:
```bash
git remote -v              # Ver repositorios remotos
git branch -r              # Ver ramas remotas
git fetch                  # Descargar cambios sin fusionar
git cherry-pick commit-id  # Aplicar un commit específico
```

## Conclusión

Git y GitHub son herramientas fundamentales en el desarrollo de software moderno. Con estos comandos básicos y este flujo de trabajo, tendrás una base sólida para empezar a usar control de versiones en tus proyectos.

Recuerda: la práctica hace al maestro. Empieza con proyectos pequeños y ve incorporando gradualmente más funcionalidades de Git según las necesites.

### Próximos pasos:
1. Practica con un proyecto personal
2. Aprende sobre Pull Requests en GitHub
3. Explora herramientas gráficas como GitKraken o SourceTree
4. Investiga sobre Git Flow para proyectos más grandes