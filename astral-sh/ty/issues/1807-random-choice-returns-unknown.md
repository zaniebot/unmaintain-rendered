```yaml
number: 1807
title: "`random.choice` returns `Unknown`"
type: issue
state: open
author: danielpopescu
labels:
  - generics
  - Protocols
assignees: []
created_at: 2025-12-08T13:55:39Z
updated_at: 2026-01-09T03:30:33Z
url: https://github.com/astral-sh/ty/issues/1807
synced_at: 2026-01-12T15:54:25Z
```

# `random.choice` returns `Unknown`

---

_@danielpopescu_

## Summary

`e` is `Unknown` in the small function below (as opposed to `float`). Also, `lst` is typed as `list[int|float]` even though is explicitly declared as `list[float]`...

```py
import random

def foo() -> float:
    lst: list[float] = [1.0, 2.0, 3.0]
    e = random.choice(lst)
    return e
```

https://play.ty.dev/40ae51a7-c846-4234-bd3a-53ee633c8280

### Version

0.0.1-alpha.32

---

_Label `generics` added by @sharkdp on 2025-12-08 14:27_

---

_Label `Protocols` added by @sharkdp on 2025-12-08 14:27_

---

_Comment by @sharkdp on 2025-12-08 14:27_

Thank you for reporting this.

This requires #1714 

---

_Closed by @sharkdp on 2025-12-08 14:27_

---

_Reopened by @sharkdp on 2025-12-08 14:28_

---

_Renamed from "type issues when using random.choice" to "`random.choice` returns `Unknown`" by @sharkdp on 2025-12-08 14:29_

---

_Comment by @carljm on 2025-12-08 17:31_

> lst is typed as list[int|float] even though is explicitly declared as list[float]

This is intentional and specified behavior of the Python type system: https://typing.python.org/en/latest/spec/special-types.html#special-cases-for-float-and-complex

Other type checkers do this as well, they just attempt to "hide" the special case by also (in some cases, at least) _displaying_ the type `int | float` as just `float`. We don't attempt to hide the real type.

---

_Added to milestone `Stable` by @carljm on 2026-01-09 03:30_

---
