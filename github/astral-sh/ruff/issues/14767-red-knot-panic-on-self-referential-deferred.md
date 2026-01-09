---
number: 14767
title: "[red-knot] Panic on self-referential deferred annotation (call expression)"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - fuzzer
  - ty
assignees: []
created_at: 2024-12-04T09:03:08Z
updated_at: 2025-04-22T16:20:54Z
url: https://github.com/astral-sh/ruff/issues/14767
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Panic on self-referential deferred annotation (call expression)

---

_Issue opened by @dhruvmanila on 2024-12-04 09:03_

The following code panics:
```py
from __future__ import annotations

def foo(a: foo()):
    pass
```

Or, in a `test.pyi` (stub) file without the import:
```pyi
def foo(a: foo()):
    pass
```

This is resolved by https://github.com/astral-sh/ruff/pull/14629.

---

_Label `bug` added by @dhruvmanila on 2024-12-04 09:03_

---

_Label `fuzzer` added by @dhruvmanila on 2024-12-04 09:03_

---

_Label `red-knot` added by @dhruvmanila on 2024-12-04 09:03_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Assigned to @carljm by @carljm on 2025-03-27 19:08_

---

_Referenced in [astral-sh/ruff#17535](../../astral-sh/ruff/pulls/17535.md) on 2025-04-22 01:50_

---

_Closed by @carljm on 2025-04-22 16:20_

---

_Closed by @carljm on 2025-04-22 16:20_

---
