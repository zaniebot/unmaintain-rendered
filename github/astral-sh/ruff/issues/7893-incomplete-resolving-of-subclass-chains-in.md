---
number: 7893
title: "Incomplete resolving of subclass chains in `runtime-evaluated-base-classes`"
type: issue
state: closed
author: disrupted
labels:
  - bug
assignees: []
created_at: 2023-10-10T13:01:06Z
updated_at: 2023-11-19T14:49:26Z
url: https://github.com/astral-sh/ruff/issues/7893
synced_at: 2026-01-07T13:12:15-06:00
---

# Incomplete resolving of subclass chains in `runtime-evaluated-base-classes`

---

_Issue opened by @disrupted on 2023-10-10 13:01_

Extracted from https://github.com/astral-sh/ruff/issues/7866#issuecomment-1752954586

Ruff doesn't reliably detect the base class (even within the same file).

given this example
```python
from abc import ABC
from pydantic import BaseModel

class Model(BaseModel):
    x: str


class Empty(BaseModel):
    ...


class Abstract(Empty, ABC):
    inner: Model
```

Ruff evaluates the following classes and their bases (I simply placed some log statements in this function)
https://github.com/astral-sh/ruff/blob/a1509dfc7c2b16f3762545fc802d49f5f03726e2/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L42

```log
class Model  bases: [Some(["pydantic", "BaseModel"])]
class Abstract  bases: [None, Some(["abc", "ABC"])]
```
notice the absence of `Empty` and how `Abstract` then doesn't get detected as `BaseModel`.

Therefore, `flake8-type-checking` rules often result in false positives as it's unable to correctly resolve the base class.

---

_Label `bug` added by @charliermarsh on 2023-10-10 15:56_

---

_Referenced in [astral-sh/ruff#8725](../../astral-sh/ruff/issues/8725.md) on 2023-11-16 18:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-16 18:03_

---

_Referenced in [astral-sh/ruff#8768](../../astral-sh/ruff/pulls/8768.md) on 2023-11-19 14:39_

---

_Closed by @charliermarsh on 2023-11-19 14:49_

---
