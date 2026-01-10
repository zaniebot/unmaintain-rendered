```yaml
number: 2343
title: Subscript fails only when using PEP-695 style union type aliases
type: issue
state: closed
author: mtshiba
labels:
  - bug
  - type aliases
assignees: []
created_at: 2026-01-05T14:12:53Z
updated_at: 2026-01-05T14:59:25Z
url: https://github.com/astral-sh/ty/issues/2343
synced_at: 2026-01-10T01:56:41Z
```

# Subscript fails only when using PEP-695 style union type aliases

---

_Issue opened by @mtshiba on 2026-01-05 14:12_

For example (https://play.ty.dev/47f4b905-76f0-4b56-8df4-1e8b484c073b):

```python
class C:
    def __getitem__(self, index: int) -> int:
        return 1

    def __add__(self, other: int) -> int:
        return 1

class D:
    def __getitem__(self, index: int) -> int:
        return 1

    def __add__(self, other: int) -> int:
        return 1

type CD = C | D
ImplicitCD = C | D

def f(x1: C | D, x2: CD, x3: ImplicitCD):
    reveal_type(x1[1])  # ok
    reveal_type(x2[1])  # error
    reveal_type(x3[1])  # ok

    reveal_type(x1 + 1)  # ok
    reveal_type(x2 + 1)  # ok
    reveal_type(x3 + 1)  # ok

```



---

_Label `bug` added by @mtshiba on 2026-01-05 14:12_

---

_Label `type aliases` added by @mtshiba on 2026-01-05 14:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-05 14:37_

---

_Closed by @mtshiba on 2026-01-05 14:59_

---

_Closed by @mtshiba on 2026-01-05 14:59_

---
