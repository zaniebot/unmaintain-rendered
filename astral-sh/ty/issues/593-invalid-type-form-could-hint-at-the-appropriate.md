```yaml
number: 593
title: "`invalid-type-form` could hint at the appropriate type"
type: issue
state: closed
author: charliermarsh
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-06-06T12:51:37Z
updated_at: 2025-06-08T23:40:06Z
url: https://github.com/astral-sh/ty/issues/593
synced_at: 2026-01-12T15:54:23Z
```

# `invalid-type-form` could hint at the appropriate type

---

_@charliermarsh_

At least for the common case of `[int]`, we could suggest `list[int]`:

```
error[invalid-type-form]: List literals are not allowed in this context in a type expression
 --> autobot/schematics/use_generator/before.py:4:32
  |
4 | def compute_squares(n: int) -> [int]:
  |                                ^^^^^
5 |     squares: list[int] = []
6 |     for i in range(n):
  |
info: See the following page for a reference on valid type expressions:
info: https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions
info: rule `invalid-type-form` is enabled by default
```

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-06 12:54_

---

_Label `help wanted` added by @AlexWaygood on 2025-06-06 13:00_

---

_Comment by @j178 on 2025-06-06 23:52_

Another common mistake like this happens with `tuple` in the return type:

```py
def f() -> (int, bool): pass
```

---

_Comment by @benbaror on 2025-06-08 06:07_

I can try to do this as my 1st PR

---

_Closed by @AlexWaygood on 2025-06-08 23:40_

---
