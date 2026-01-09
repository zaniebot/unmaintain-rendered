---
number: 7220
title: "[Feature Request] Alphabetically sort `typing.Literal` types"
type: issue
state: open
author: michaeloliverx
labels:
  - rule
assignees: []
created_at: 2023-09-07T12:00:04Z
updated_at: 2023-09-12T17:19:35Z
url: https://github.com/astral-sh/ruff/issues/7220
synced_at: 2026-01-07T13:12:15-06:00
---

# [Feature Request] Alphabetically sort `typing.Literal` types

---

_Issue opened by @michaeloliverx on 2023-09-07 12:00_

I want the ability to alphabetically `Literal` types e.g.

```python
from typing import Literal

# before
ErrorCode = Literal[
    "foo",
    "bar",
]

# after
ErrorCode = Literal[
    "bar",
    "foo",
]
```

This is related to https://github.com/astral-sh/ruff/issues/2315

I think having the ability to sort a range of things such as enums, literal collections, typed dict keys would be great. The rules should probably live in the `RUF` ruleset and perhaps be enabled / disabled via an action comment.


---

_Comment by @charliermarsh on 2023-09-12 17:19_

Makes sense, very similar to #1198 -- a different case of the same idea.

---

_Label `rule` added by @charliermarsh on 2023-09-12 17:19_

---

_Referenced in [astral-sh/ruff#10085](../../astral-sh/ruff/issues/10085.md) on 2024-02-26 11:45_

---

_Referenced in [astral-sh/ruff#2315](../../astral-sh/ruff/issues/2315.md) on 2024-11-26 15:55_

---
