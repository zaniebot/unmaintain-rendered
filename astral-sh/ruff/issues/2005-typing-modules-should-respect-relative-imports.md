```yaml
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
synced_at: 2026-01-12T15:54:42Z
```

# `typing-modules` should respect relative imports

---

_@charliermarsh_

See: #1976.

---

_Label `core` added by @charliermarsh on 2023-01-20 00:09_

---

_Comment by @sbrugman on 2023-01-23 21:32_

Some relevant code sections in case anyone wants to take this (played around a bit, no fix yet):
- Import bindings: https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L3802
- Diagnostic:  https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L3890
- Matching 'Load' e.g.`Literal[...]`: https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L3266
- Checking for `typing-modules`: https://github.com/charliermarsh/ruff/blob/main/src/python/typing.rs#L214
- Relative imports: https://github.com/charliermarsh/ruff/blob/main/src/checkers/ast.rs#L237

---

_Comment by @henryiii on 2023-02-07 01:15_

Closed by https://github.com/charliermarsh/ruff/pull/2615 ?

---

_Comment by @charliermarsh on 2023-02-07 02:08_

Yes, thank you, I linked the wrong issue 🤦 

---

_Closed by @charliermarsh on 2023-02-07 02:08_

---
