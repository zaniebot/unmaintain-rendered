```yaml
number: 17789
title: Add .pixi to default excluded folders
type: issue
state: closed
author: dennis-wey
labels: []
assignees: []
created_at: 2025-05-02T09:43:48Z
updated_at: 2025-05-03T13:48:44Z
url: https://github.com/astral-sh/ruff/issues/17789
synced_at: 2026-01-12T15:54:56Z
```

# Add .pixi to default excluded folders

---

_@dennis-wey_

According to https://docs.astral.sh/ruff/settings/#exclude ruff excludes the following folders by default:

> [".bzr", ".direnv", ".eggs", ".git", ".git-rewrite", ".hg", ".mypy_cache", ".nox", ".pants.d", ".pytype", ".ruff_cache", ".svn", ".tox", ".venv", "__pypackages__", "_build", "buck-out", "dist", "node_modules", "venv"]

It would be great if .pixi can be added to this list. pixi is a a package manager (https://pixi.sh/latest/), which creates the environment inside the project in the .pixi folder.

This probably won't affect most users, as pixi would be most likely inside the `.gitignore` file (and even has `.pixi/.gitignore`). See https://docs.astral.sh/ruff/settings/#respect-gitignore

However this only works, when the project is set up using git and this mechanism also don't work when running ruff before the very first git commit. Therefore, I think it makes sense to also add .pixi folder to the above list.

---

_Comment by @MichaReiser on 2025-05-03 13:48_

We want to keep the list to a minimum. Users can easily add their own excludes using `extend-exclude` or add `.pixi` to their `.gitignore`. We can reconsider this once pixi has more widespread adoption.

---

_Closed by @MichaReiser on 2025-05-03 13:48_

---
