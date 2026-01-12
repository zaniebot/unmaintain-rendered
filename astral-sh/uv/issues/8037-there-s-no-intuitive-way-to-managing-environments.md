```yaml
number: 8037
title: "There's no intuitive way to Managing Environments."
type: issue
state: closed
author: farzbood
labels: []
assignees: []
created_at: 2024-10-09T07:42:47Z
updated_at: 2024-10-16T13:38:06Z
url: https://github.com/astral-sh/uv/issues/8037
synced_at: 2026-01-12T15:59:19Z
```

# There's no intuitive way to Managing Environments.

---

_@farzbood_

Hi,
I couldn't find any command or options to **Manage the Environments** created once by `uv venv `and needed to be **Modified/Removed/Switched Between** appropriately, like the ones available in [Poetry](https://python-poetry.org/docs/managing-environments/).

---

_Comment by @ilyagr on 2024-10-09 08:46_

You are probably looking for the `uv pip` commands, e.g. `uv pip list`, `uv pip install`, ...

I kind of wish `uv pip` was renamed to `uv env`, and `uv venv` was `uv env create`. `uv pip` could still exist as an alias for `uv env`, as can `uv venv`.

---

_Comment by @farzbood on 2024-10-09 11:49_

No, `pip `commands are about managing the **dependencies** of the project installed in the `env`, not the **virtual environment** itself.
I'm looking for commands like: `poetry env remove`, `poetry env use`, ... to manage the `env`. ([check the link to the Poetry docs](https://python-poetry.org/docs/managing-environments/).) that let you associate more than one `env `to a project and switch between them, remove unwanted ones, etc.

P.S
How can I Install the latest `[Python 3.13 (free-threaded) build](https://github.com/indygreg/python-build-standalone/releases/tag/20241008)` distribution (**Windows10 x86_64**), with uv? I couldn't find a way to tell that to `uv python install`

---

_Comment by @zanieb on 2024-10-09 13:41_

We only allow a single project environment at this time. Related #1495 

See https://github.com/astral-sh/uv/issues/8019

---

_Closed by @charliermarsh on 2024-10-16 13:38_

---
