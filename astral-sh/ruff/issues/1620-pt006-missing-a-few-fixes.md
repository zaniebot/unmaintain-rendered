```yaml
number: 1620
title: PT006 missing a few fixes
type: issue
state: closed
author: edgarrmondragon
labels:
  - fixes
assignees: []
created_at: 2023-01-04T02:56:34Z
updated_at: 2023-01-04T03:52:52Z
url: https://github.com/astral-sh/ruff/issues/1620
synced_at: 2026-01-12T15:54:41Z
```

# PT006 missing a few fixes

---

_@edgarrmondragon_

Currently supported:

- `"param1,param2"` -> `("param1", "param2")`
- `"param1,param2"` -> `["param1", "param2"]`
- `("param1",)` -> `"param1"`
- `["param1"]` -> `"param1"`

Missing fixes:

- `("param1", "param2")` -> `"param1,param2"`
- `["param1", "param2"]` -> `"param1,param2"`
- `("param1", "param2")` -> `["param1", "param2"]`
- `["param1", "param2"]` -> `("param1", "param2")`

The first two should only apply when all the sequence elements are string literals, and the last two should preserve each element's expressions.

Related #1592

---

_Label `autofix` added by @charliermarsh on 2023-01-04 03:08_

---

_Closed by @charliermarsh on 2023-01-04 03:52_

---
