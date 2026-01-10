---
number: 2005
title: "`typing-modules` should respect relative imports"
type: issue
state: closed
author: charliermarsh
labels:
  - core
assignees: []
created_at: 2023-01-19T22:28:28Z
updated_at: 2023-02-07T02:08:00Z
url: https://github.com/astral-sh/ruff/issues/2005
synced_at: 2026-01-10T01:22:40Z
---

# `typing-modules` should respect relative imports

---

_Issue opened by @charliermarsh on 2023-01-19 22:28_

See: #1976.

---

_Referenced in [astral-sh/ruff#1976](../../astral-sh/ruff/issues/1976.md) on 2023-01-19 22:28_

---

_Label `core` added by @charliermarsh on 2023-01-20 00:09_

---

_Referenced in [astral-sh/ruff#2105](../../astral-sh/ruff/issues/2105.md) on 2023-01-23 14:18_

---

_Referenced in [pypa/cibuildwheel#1405](../../pypa/cibuildwheel/pulls/1405.md) on 2023-01-23 20:09_

---

_Comment by @sbrugman on 2023-01-23 21:32_

Some relevant code sections in case anyone wants to take this (played around a bit, no fix yet):
- Import bindings: https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L3802
- Diagnostic:  https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L3890
- Matching 'Load' e.g.`Literal[...]`: https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L3266
- Checking for `typing-modules`: https://github.com/charliermarsh/ruff/blob/main/src/python/typing.rs#L214
- Relative imports: https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L237

---

_Referenced in [astral-sh/ruff#2613](../../astral-sh/ruff/issues/2613.md) on 2023-02-06 23:18_

---

_Comment by @henryiii on 2023-02-07 01:15_

Closed by https://github.com/charliermarsh/ruff/pull/2615 ?

---

_Referenced in [astral-sh/ruff#2615](../../astral-sh/ruff/pulls/2615.md) on 2023-02-07 02:07_

---

_Comment by @charliermarsh on 2023-02-07 02:08_

Yes, thank you, I linked the wrong issue 🤦 

---

_Closed by @charliermarsh on 2023-02-07 02:08_

---
