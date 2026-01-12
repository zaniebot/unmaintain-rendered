```yaml
number: 3123
title: "C408 autofix `dict(x=y)` -> `{\"x\": y}` doesn't respect quote style"
type: issue
state: closed
author: bluetech
labels:
  - bug
assignees: []
created_at: 2023-02-22T13:37:30Z
updated_at: 2023-02-22T15:26:47Z
url: https://github.com/astral-sh/ruff/issues/3123
synced_at: 2026-01-12T15:54:43Z
```

# C408 autofix `dict(x=y)` -> `{"x": y}` doesn't respect quote style

---

_@bluetech_

It always uses `"` style:

https://github.com/charliermarsh/ruff/blob/48005d87f8721be6e655f1a959b46e1b5bf97940/crates/ruff/src/rules/flake8_comprehensions/fixes.rs#L522-L532

---

_Label `bug` added by @charliermarsh on 2023-02-22 14:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-22 15:13_

---

_Closed by @charliermarsh on 2023-02-22 15:26_

---
