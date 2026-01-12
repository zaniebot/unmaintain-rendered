```yaml
number: 152
title: context managers may silence exceptions
type: issue
state: open
author: carljm
labels:
  - control flow
  - runtime semantics
assignees: []
created_at: 2025-03-26T18:12:16Z
updated_at: 2026-01-06T01:47:01Z
url: https://github.com/astral-sh/ty/issues/152
synced_at: 2026-01-12T15:54:22Z
```

# context managers may silence exceptions

---

_@carljm_

_No description provided._

---

_Renamed from "[red-knot] context managers may silence exceptions" to "context managers may silence exceptions" by @MichaReiser on 2025-05-07 15:25_

---

_Label `control flow` added by @AlexWaygood on 2025-05-11 07:32_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:32_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:11_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Assigned to @mtshiba by @mtshiba on 2025-12-31 11:13_

---

_Comment by @AlexWaygood on 2025-12-31 11:17_

For feature parity with mypy/pyright, we need to model the fact that a context manager might suppress an exception if its`__exit__` method returns something that is not always falsey (common return return types for exception-suppressing context managers are `bool`, `bool | None`, `Literal[True]` or `Literal[True] | None`). But it can never suppress an exception if its `__exit__` method returns an always-falsy type such as `None`, `Literal[False]`, or `Literal[False] | None`.

---

_Added to milestone `Stable` by @carljm on 2026-01-06 01:47_

---
