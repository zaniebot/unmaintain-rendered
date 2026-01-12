```yaml
number: 220
title: support nonlocal statements
type: issue
state: closed
author: carljm
labels:
  - runtime semantics
assignees: []
created_at: 2025-01-09T18:57:48Z
updated_at: 2025-07-11T16:45:22Z
url: https://github.com/astral-sh/ty/issues/220
synced_at: 2026-01-12T15:54:22Z
```

# support nonlocal statements

---

_@carljm_

_No description provided._

---

_Renamed from "[red-knot] support nonlocal and global statements" to "support nonlocal and global statements" by @MichaReiser on 2025-05-07 15:27_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:20_

---

_Comment by @gusutabopb on 2025-05-21 09:04_

I ran into this today when using a closure to increment an integer from the outer scope.

Here's an MCVE for illustration purposes:

```python
def foo(n: int) -> int:
    count = 0

    def incr():
        nonlocal count
        count += 1

    for _ in range(n):
        incr()
    return count


if __name__ == "__main__":
    print(foo(10))
```

ty (0.0.1a6) reports this false positive:
```
error[unresolved-reference]: Name `count` used when not defined
```


https://play.ty.dev/12eb79c8-4504-43d0-a62c-2e99d8bfc067





---

_Comment by @carljm on 2025-05-30 01:30_

we do support `global` now; repurposing this issue for `nonlocal` support

---

_Renamed from "support nonlocal and global statements" to "support nonlocal statements" by @carljm on 2025-05-30 01:30_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-07-02 17:05_

---

_Comment by @oconnor663 on 2025-07-11 16:45_

Support added in https://github.com/astral-sh/ruff/pull/19112.

---

_Closed by @oconnor663 on 2025-07-11 16:45_

---
