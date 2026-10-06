---
name: fc-conventions
description: Convenciones de trabajo en Azure DevOps para work items, nombres de rama (feature/bugfix/hotfix), mensajes de commit (Conventional Commits), pull requests (título y plantillas) y revisión de código. Usar siempre que se cree o nombre una rama, se redacte o revise un mensaje de commit, se arme el título o la descripción de un PR, se revise un PR, o se elija qué tipo de work item registrar.
---

# Convenciones de trabajo, commits y ramas

La fuente oficial es la wiki del equipo (sección *Guía de contribución*: Convención de commits, Nombres de ramas, Pull requests, Revisión de código). Si el usuario indica una regla distinta a las de aquí, gana la del usuario o la wiki.

Al aplicar estas reglas: **propón** los nombres, mensajes y comandos. No hagas commit, push ni crees PRs salvo que el usuario lo pida explícitamente.

## Work items

| Work item | Uso |
|---|---|
| Epic | Iniciativa grande con varios entregables |
| Feature | Módulo o conjunto de funcionalidades |
| Requerimiento | Funcionalidad, mejora o entregable concreto de desarrollo |
| Task | Parte de la ejecución de un Requerimiento o Bug |
| Bug | Defecto de software que hay que corregir |
| Issue | Incidente operativo (ligado al ticket de la mesa de ayuda, si existe) |
| Actividad | Trabajo operativo/administrativo/técnico independiente |

Orden para elegir: ¿incidente en operación? → Issue (+ Bug si hay defecto). ¿Defecto? → Bug. ¿Iniciativa amplia? → Epic. ¿Módulo? → Feature. ¿Entregable de desarrollo? → Requerimiento. ¿Parte de otro WI? → Task. ¿Trabajo independiente? → Actividad.

## Ramas

Formato: `<tipo>/<id-workitem>-<descripcion-kebab-case>`

| Tipo | Cuándo | Sale de | PR hacia |
|---|---|---|---|
| `feature` | Funcionalidad nueva o mejora (Requerimiento, Task, Actividad) | `release/X.Y.Z` vigente | `release/X.Y.Z` |
| `bugfix` | Error que **aún no está en producción** | `release/X.Y.Z` vigente | `release/X.Y.Z` |
| `hotfix` | Error **urgente en producción** | `main` | `main`, y luego llevar la corrección a `release/X.Y.Z` |

- Todo en minúscula, sin `#`, espacios ni tildes. Una rama por work item.
- El tipo va completo: `feature`, nunca `feat`.
- Si hay duda entre `bugfix` y `hotfix`, es `bugfix`.
- `main` solo recibe PRs desde `release/X.Y.Z` (los hace el responsable de la versión) o desde `hotfix/...`.

Ejemplos: `feature/1234-login-entra-id`, `bugfix/1302-token-expirado`, `hotfix/1350-servicio-permisos`.

## Commits (Conventional Commits)

Formato: `<tipo>(<módulo>): <descripción>` (módulo opcional si el cambio es general).

| Tipo | Uso |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de un bug |
| `refactor` | Cambio interno sin alterar comportamiento |
| `perf` | Mejora de rendimiento |
| `style` | Solo formato, sin cambios de lógica |
| `docs` | Documentación |
| `test` | Pruebas |
| `build` | Compilación y dependencias |
| `ci` | Pipelines |
| `revert` | Revierte un commit |
| `chore` | Mantenimiento del repo (.gitignore, configuración) |

- Descripción en imperativo, minúscula, sin punto final, ~72 caracteres máx.
- Tipos completos, sin abreviar (`refactor`, no `ref`).
- Ruptura de compatibilidad: `!` tras el tipo (`feat(auth)!: ...`) o footer `BREAKING CHANGE: <detalle>`.
- **No** poner el work item en el commit (ni `AB#1234` ni `#1234`): se enlaza en el PR desde Azure DevOps y el número ya va en la rama.
- Rama `feature/` → commits `feat` (más `test`, `docs`, `refactor`…); ramas `bugfix/` y `hotfix/` → commits `fix`.

```
feat(auth)!: reemplazar autenticación local por Entra ID

BREAKING CHANGE: se elimina el endpoint /auth/login local
```

## Pull requests

- Título con el mismo formato del commit (`<tipo>(<módulo>): <descripción>`, `!` si rompe compatibilidad). Corregir el que Azure autocompleta.
- Un PR por work item; enlazar el work item en el panel **Work items** del PR (único lugar donde se enlaza); abrir como **Draft** si no está listo; compilación y pruebas en verde antes de pedir revisión.
- Merge con ≥1 aprobación de alguien distinto al autor, comentarios bloqueantes resueltos y rama al día con su destino. Borrar la rama después del merge.
- Plantilla de descripción (en *Description → Add a template*; viven en `.azuredevops/PULL_REQUEST_TEMPLATE/` de cada repo):

| Rama | Plantilla | Secciones |
|---|---|---|
| `feature/...` | `feature` | Descripción de la funcionalidad, Historia de usuario, Detalles de implementación, Checklist, Capturas |
| `bugfix/...`, `hotfix/...` | `bugfix` | Descripción del bug (comportamiento con el bug / corregido, paso a paso), Checklist |
| Otra (`release` → `main`, docs, ci…) | `estandar` | Descripción, Tipo de cambio, Cambios realizados, Checklist, Capturas |

Cuando redactes una descripción de PR, usa las secciones de la plantilla que corresponde y llénalas con lo que cambia el diff. Si el repo tiene sus propias plantillas en `.azuredevops/PULL_REQUEST_TEMPLATE/`, léelas y usa esas.

## Revisión de código

Cada comentario empieza con una etiqueta:

| Etiqueta | Bloquea merge |
|---|---|
| `issue:` (bug, seguridad, incumplimiento de convenciones) | Sí |
| `question:` | Sí |
| `suggestion:` | No |
| `nit:` | No |
| `praise:` | No |

Si el PR incumple una convención (título, rama, commits, plantilla, work item sin enlazar), el comentario es `issue:`, **cita la página de la wiki** que aplica (Convención de commits, Nombres de ramas, Pull requests o Revisión de código) y muestra cómo debería quedar. Si no conoces la URL de la wiki, pídesela al usuario en vez de inventarla.

```
issue: la rama no sigue el formato `<tipo>/<id-workitem>-<descripcion>`. Ver [Nombres de ramas](<link a la página de la wiki>).
Debería ser: `feature/1234-login-entra-id`
```

Comentar sobre el código, no la persona; explicar el porqué y proponer alternativa. Quien abre el comentario lo resuelve.
