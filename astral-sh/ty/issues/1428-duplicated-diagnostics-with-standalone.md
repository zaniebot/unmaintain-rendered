```yaml
number: 1428
title: Duplicated diagnostics with standalone expressions in overloads
type: issue
state: closed
author: ibraheemdev
labels:
  - bug
  - overloads
assignees: []
created_at: 2025-10-24T03:39:26Z
updated_at: 2025-10-24T17:21:40Z
url: https://github.com/astral-sh/ty/issues/1428
synced_at: 2026-01-12T15:54:25Z
```

# Duplicated diagnostics with standalone expressions in overloads

---

_@ibraheemdev_

We currently silence diagnostics during multi-inference for overload arguments, but this doesn't work for standalone expressions.

```py
from typing import overload

@overload
def f(_: str): ...

@overload
def f(_: str): ...

def f(_: str): ...

def _(a: object, b: object):
    f(f"{'a' if a > b else 'b'}")
```
```
error[unsupported-operator]: Operator `>` is not supported for types `object` and `object`
  --> x.py:12:17
   |
11 | def _(a: object, b: object):
12 |     f(f"{'a' if a > b else 'b'}")
   |                 ^^^^^
   |
info: rule `unsupported-operator` is enabled by default

error[unsupported-operator]: Operator `>` is not supported for types `object` and `object`
  --> x.py:12:17
   |
11 | def _(a: object, b: object):
12 |     f(f"{'a' if a > b else 'b'}")
   |                 ^^^^^
```

---

_Label `bug` added by @ibraheemdev on 2025-10-24 03:39_

---

_Renamed from "Duplicated diagnostics with standlone expressions in overloads" to "Duplicated diagnostics with standalone expressions in overloads" by @AlexWaygood on 2025-10-24 10:00_

---

_Label `overloads` added by @AlexWaygood on 2025-10-24 10:00_

---

_Closed by @ibraheemdev on 2025-10-24 17:21_

---
