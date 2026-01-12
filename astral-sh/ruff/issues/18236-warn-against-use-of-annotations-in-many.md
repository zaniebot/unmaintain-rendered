```yaml
number: 18236
title: "Warn against use of `.__annotations__` (in many circumstances)"
type: issue
state: open
author: JelleZijlstra
labels:
  - rule
assignees: []
created_at: 2025-05-21T02:18:57Z
updated_at: 2025-05-26T19:19:02Z
url: https://github.com/astral-sh/ruff/issues/18236
synced_at: 2026-01-12T15:54:56Z
```

# Warn against use of `.__annotations__` (in many circumstances)

---

_@JelleZijlstra_

### Summary

Directly using the `.__annotations__` attribute of an object is unsafe in a few circumstances:

- On classes with custom metaclasses, various odd things can happen (https://peps.python.org/pep-0749/#pre-existing-bugs). On Python 3.14 and newer this only happens if at least one of the classes involved uses `from __future__ import annotations`; on earlier versions it can always happen.
- Accessing `__annotations__` on an instance of an annotated class works in released versions of Python (most of the time) but won't work in Python 3.14.
- (Minor) Accessing `__annotations__` on arbitrary type objects will raise AttributeError on static types, e.g. `int.__annotations__`.

However, it is always safe to access `__annotations__` on function and module objects.

Therefore, it probably makes sense for Ruff to warn against any use of the `.__annotations__` attribute, perhaps with an exception for objects known to be functions or modules.

Similar to #17853.

---

_Label `rule` added by @MichaReiser on 2025-05-26 19:19_

---
