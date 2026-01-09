---
number: 7647
title: "Jupyter notebooks from project environment don't have access to `--with` requirements"
type: issue
state: open
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-09-23T17:54:40Z
updated_at: 2024-10-07T20:08:48Z
url: https://github.com/astral-sh/uv/issues/7647
synced_at: 2026-01-07T13:12:17-06:00
---

# Jupyter notebooks from project environment don't have access to `--with` requirements

---

_Issue opened by @charliermarsh on 2024-09-23 17:54_

To repro:

- `uv init`
- `uv add jupyter`
- `uv run --with pydantic jupyer lab`

Within the notebook, `import pydantic` will fail. The issue is that `jupyter` resolves to the executable in the project environment, and that environment doesn't have access to the `--with` environment (i.e., `sys.path` is the project environment, with no mention of the `--with` environment).

---

_Label `bug` added by @charliermarsh on 2024-09-23 17:54_

---

_Referenced in [astral-sh/uv#7699](../../astral-sh/uv/pulls/7699.md) on 2024-09-26 08:35_

---

_Comment by @bluss on 2024-10-07 20:08_

Note for later, `uv run --with pydantic jupyter console` has the same issue and is faster to run.

I'm thinking of this issue but jupyter and jupyterlab are not easily fixed to use layered environments. They use `sys.prefix` to find their installed data in `.venv/share/jupyter` and `.venv/share/jupyter/lab`. (Environment overrides exists but it's ad-hoc.) `jupyter --paths --debug` can be a useful command.

---
