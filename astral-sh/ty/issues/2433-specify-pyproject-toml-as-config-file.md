```yaml
number: 2433
title: Specify pyproject.toml as config file
type: issue
state: closed
author: TurtleOrangina
labels: []
assignees: []
created_at: 2026-01-10T09:48:40Z
updated_at: 2026-01-10T09:53:36Z
url: https://github.com/astral-sh/ty/issues/2433
synced_at: 2026-01-10T11:36:43Z
```

# Specify pyproject.toml as config file

---

_Issue opened by @TurtleOrangina on 2026-01-10 09:48_

### Issue

I cannot specify the pyproject.toml file that ty should use within the vscode extension. The "ty.configurationFile" setting only accepts `ty.toml`, not `pyproject.toml` (this is true for both CLI and the extension). 

Ideas for a fix (any of these would fix it for me):
- Allow "ty.configurationFile" to point to a pyproject.toml (and --config-file for the CLI similarly, for consistency)
- Enable specifying the "project" CLI argument in the vscode settings (ty.project or ty.args)
- Some kind of discovery of the pyproject.toml that requires not special settings.

### Background

I have a repository that looks like this:

backend/pyproject.toml # python backend
frontend/ # typescript front-end

I open the root of the repository with VSCode, which means that I have to specify the location of the pyproject.toml file for some tools, e.g. `"mypy-type-checker.cwd": "${workspaceFolder}/backend"`.

Since I have enabled some stricter checking for ty in my pyproject.toml, running `uv run ty check` currently gives no errors, while `uv run ty check --project backend` gives me 13 errors. I would like to be able to configure my vscode extension to be as strict as the `--project backend` call.

### Workaround

For now I have moved my ty configuration from pyproject.toml into a ty.toml, which I can specify via ty.configurationFile.

---

_Comment by @MichaReiser on 2026-01-10 09:53_

Thank you 

---

_Closed by @MichaReiser on 2026-01-10 09:53_

---
