---
number: 16272
title: "[red-knot] Incorrect error message when iterating over an object that has an `__iter__` method that might not return an object with a `__next__` method"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
  - diagnostics
assignees: []
created_at: 2025-02-20T13:07:37Z
updated_at: 2025-02-25T14:02:05Z
url: https://github.com/astral-sh/ruff/issues/16272
synced_at: 2026-01-10T01:22:57Z
---

# [red-knot] Incorrect error message when iterating over an object that has an `__iter__` method that might not return an object with a `__next__` method

---

_Issue opened by @AlexWaygood on 2025-02-20 13:07_

### Description

For this snippet:

```py
class Iterator:
    def __next__(self) -> int:
        return 42

class Iterable:
    def __iter__(self) -> Iterator | int:
        return Iterator()

for x in Iterable():
    pass
```

We currently emit this diagnostic:

```
error: lint:not-iterable
  --> /Users/alexw/dev/experiment/foo.py:9:10
   |
 7 |         return Iterator()
 8 |
 9 | for x in Iterable():
   |          ^^^^^^ Object of type `Iterable` is not iterable because its `__iter__` method is possibly unbound
10 |     pass
   |
```

It's correct for us to emit a diagnostic here. However, this is an incorrect error message: `__iter__` here _is_ definitely bound, but it returns an object that might not necessarily have a `__next__` method.

---

_Label `bug` added by @AlexWaygood on 2025-02-20 13:07_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-20 13:07_

---

_Label `diagnostics` added by @AlexWaygood on 2025-02-20 13:13_

---

_Referenced in [astral-sh/ruff#16321](../../astral-sh/ruff/pulls/16321.md) on 2025-02-22 22:05_

---

_Closed by @AlexWaygood on 2025-02-25 14:02_

---
