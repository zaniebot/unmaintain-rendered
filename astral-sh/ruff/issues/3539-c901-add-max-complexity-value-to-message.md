```yaml
number: 3539
title: "`C901`: Add `max-complexity` value to message"
type: issue
state: closed
author: ngnpope
labels:
  - good first issue
assignees: []
created_at: 2023-03-15T11:19:36Z
updated_at: 2023-03-16T05:37:27Z
url: https://github.com/astral-sh/ruff/issues/3539
synced_at: 2026-01-12T15:54:43Z
```

# `C901`: Add `max-complexity` value to message

---

_@ngnpope_

Having just seen output like the following:

```
example.py:851:9: C901 `function` is too complex (15)
example.py:851:9: PLR0912 Too many branches (16/15)
example.py:851:9: PLR0915 Too many statements (53/50)
```

It seems like it would be nice to include the value of `max-complexity` in the `C901` message too?

---

_Comment by @charliermarsh on 2023-03-15 19:54_

Yeah makes sense. I feel like all of these would be clearer as `16 > 15`, rather than with the slash.

---

_Label `good first issue` added by @charliermarsh on 2023-03-15 19:54_

---

_Closed by @charliermarsh on 2023-03-16 05:37_

---
