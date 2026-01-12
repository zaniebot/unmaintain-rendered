```yaml
number: 17147
title: "[red-knot] Type narrowing for assertions"
type: issue
state: closed
author: sharkdp
labels:
  - ty
assignees: []
created_at: 2025-04-02T11:28:02Z
updated_at: 2025-04-10T14:15:53Z
url: https://github.com/astral-sh/ruff/issues/17147
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Type narrowing for assertions

---

_@sharkdp_

It is fairly common (often in test code, but also in regular code) to use `assert`ions to encode invariants that are hard/impossible to detect statically. So we should be able to perform type narrowing via `assert` statements:

```py
def f(x: str | None):
    assert x is not None
    reveal_type(x)  # Should be 'str'
```
(https://playknot.ruff.rs/d44edb95-1204-484c-b6c8-2a11ba6cef51)

---

_Label `red-knot` added by @sharkdp on 2025-04-02 11:28_

---

_Added to milestone `Red Knot Alpha` by @sharkdp on 2025-04-02 11:28_

---

_Comment by @MatthewMckee4 on 2025-04-02 11:43_

I'd like to take this on if this is okay for a contributor?

---

_Assigned to @MatthewMckee4 by @sharkdp on 2025-04-02 12:39_

---

_Closed by @carljm on 2025-04-10 14:15_

---

_Closed by @carljm on 2025-04-10 14:15_

---
