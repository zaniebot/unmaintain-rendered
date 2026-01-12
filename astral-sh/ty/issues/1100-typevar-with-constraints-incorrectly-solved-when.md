```yaml
number: 1100
title: "TypeVar with constraints incorrectly solved when `Unknown` argument is passed in"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - generics
assignees: []
created_at: 2025-08-27T11:31:34Z
updated_at: 2025-11-13T07:36:32Z
url: https://github.com/astral-sh/ty/issues/1100
synced_at: 2026-01-12T15:54:24Z
```

# TypeVar with constraints incorrectly solved when `Unknown` argument is passed in

---

_@AlexWaygood_

### Summary

```py
def f[T: (str, bytes)](x: T) -> T:
    return x

def g(x):
    reveal_type(f(x))  # revealed: str
```

https://play.ty.dev/dfbe3306-e4f1-46c1-a888-d8b178dc86b0

`str` feels much too confident here -- I think this should either be `Unknown` or `str | bytes`?

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-08-27 11:31_

---

_Label `generics` added by @AlexWaygood on 2025-08-27 11:31_

---

_Comment by @carljm on 2025-08-27 14:21_

It should be `Unknown` -- `str | bytes` would violate the gradual guarantee. Mypy reveals `Any` here.

I think this will be rolled into #623 but we can keep it open so we remember to verify it's fixed.

---

_Added to milestone `Stable` by @carljm on 2025-11-13 07:36_

---
