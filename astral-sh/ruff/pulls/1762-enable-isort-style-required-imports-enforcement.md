```yaml
number: 1762
title: "Enable isort-style `required-imports` enforcement"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/add-imports
created_at: 2023-01-10T04:21:01Z
updated_at: 2023-01-12T12:50:40Z
url: https://github.com/astral-sh/ruff/pull/1762
synced_at: 2026-01-12T15:55:07Z
```

# Enable isort-style `required-imports` enforcement

---

_@charliermarsh_

In isort, this is called `add-imports`, but I prefer the declarative name.

The idea is that by adding the following to your `pyproject.toml`, you can ensure that the import is included in all files:

```toml
[tool.ruff.isort]
required-imports = ["from __future__ import annotations"]
```

I mostly reverse-engineered isort's logic for making decisions, though I made some slight tweaks that I think are preferable. A few comments:

- Like isort, we don't enforce this on empty files (like empty `__init__.py`).
- Like isort, we require that the import is at the top-level.
- isort will skip any docstrings, and any comments on the first three lines (I think, based on testing). Ruff places the import after the last docstring or comment in the file preamble (that is: after the last docstring or comment that comes before the _first_ non-docstring and non-comment).

Resolves #1700.


---

_Comment by @charliermarsh on 2023-01-10 04:28_

Two more things here: (1) support straight imports (`import os`, as opposed to `from os import ...`), and (2) add tests.

---

_Merged by @charliermarsh on 2023-01-10 23:12_

---

_Closed by @charliermarsh on 2023-01-10 23:12_

---

_Branch deleted on 2023-01-10 23:12_

---

_Comment by @ashb on 2023-01-12 12:50_

Amazing timing @charliermarsh - I just went looking for this for Airflow

---
