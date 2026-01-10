---
number: 16912
title: "[red-knot] Disambiguate display for intersection types"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2025-03-22T12:08:38Z
updated_at: 2025-03-23T14:18:31Z
url: https://github.com/astral-sh/ruff/issues/16912
synced_at: 2026-01-10T01:22:58Z
---

# [red-knot] Disambiguate display for intersection types

---

_Issue opened by @MichaReiser on 2025-03-22 12:08_

              What about intersection types?

```python
def _(
    c: Intersection[Callable[[Union[int, str]], int], A],
    d: Intersection[A, Callable[[Union[int, str]], int]],
    e: Intersection[A, Callable[[Union[int, str]], int], B],
    f: Intersection[Not[Callable[[int, str], Intersection[int, str]]]]
):
    reveal_type(c)  # revealed: ((int | str, /) -> int) & A
    reveal_type(d)  # revealed: A & ((int | str, /) -> int)
    reveal_type(e)  # revealed: A & ((int | str, /) -> int) & B
    reveal_type(f)  # revealed: ~((int, str, /) -> int & str)
```

_Originally posted by @InSyncWithFoo in https://github.com/astral-sh/ruff/pull/16907#discussion_r2008626273_

Same as https://github.com/astral-sh/ruff/issues/16893 but for intersections

---

_Label `red-knot` added by @MichaReiser on 2025-03-22 12:08_

---

_Comment by @MatthewMckee4 on 2025-03-22 12:15_

I can sort this out

---

_Referenced in [astral-sh/ruff#16914](../../astral-sh/ruff/pulls/16914.md) on 2025-03-22 12:23_

---

_Assigned to @MatthewMckee4 by @AlexWaygood on 2025-03-22 21:09_

---

_Closed by @carljm on 2025-03-23 14:18_

---

_Closed by @carljm on 2025-03-23 14:18_

---
