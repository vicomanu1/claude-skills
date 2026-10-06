# claude-skills

Skills para Claude Code con convenciones de trabajo en Azure DevOps.

| Skill | Para qué |
|---|---|
| `fc-conventions` | Work items, nombres de rama, commits, PRs (título y plantillas) y revisión de código |

## Requisitos

- [Node.js](https://nodejs.org/) **22.20 o superior** (lo pide el CLI `skills`). Recomendado: la LTS actual. Revisa tu versión con `node -v`.
- Claude Code.

## Instalar

```powershell
npx skills add vicomanu1/claude-skills -g -a claude-code -y
```

- `-g`: a nivel de usuario, sirve en todos tus repos.
- `-a claude-code`: la instala para Claude Code.
- `-y`: sin preguntas.

Para instalar solo una skill: agrega `-s fc-conventions`.

La skill queda disponible desde el siguiente chat de Claude Code. Se activa sola al nombrar ramas, escribir commits o armar/revisar PRs, o se llama con `/fc-conventions`.

## Actualizar

```powershell
npx skills update -g
```

## Ver o quitar

```powershell
npx skills ls -g
npx skills rm -g fc-conventions
```

## Problemas comunes

**Error de versión de Node** (`EBADENGINE`, `Unsupported engine`, `SyntaxError` o similar al correr `npx skills`): tu Node es menor a 22.20. Actualízalo:

```powershell
winget upgrade OpenJS.NodeJS.LTS     # si lo instalaste con winget
# o descarga el instalador LTS desde https://nodejs.org/
```

Si usas nvm-windows:

```powershell
nvm install lts
nvm use lts
```

Cierra y abre la terminal, y confirma con `node -v`.

## Agregar o cambiar una skill

Cada skill va en `skills/<nombre>/SKILL.md`. Si cambia una convención en la wiki, actualiza el `SKILL.md` correspondiente en el mismo momento.
