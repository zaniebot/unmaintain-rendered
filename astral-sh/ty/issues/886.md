---
number: 886
title: "\"Find references\" doesn't find overloads"
type: issue
state: open
author: UnboundVariable
labels:
  - bug
  - server
  - overloads
assignees: []
created_at: 2025-07-25T01:52:19Z
updated_at: 2026-01-06T10:47:38Z
url: https://github.com/astral-sh/ty/issues/886
synced_at: 2026-01-10T01:51:14Z
---

# "Find references" doesn't find overloads

---

_Issue opened by @UnboundVariable on 2025-07-25 01:52_

### Summary

When using the "go to references" or "find all references" feature in the language server, if you point to the name of an overloaded function or method, I would expect all overloads to be found in addition to the overload's implementation. Currently, only the implementation is found.

### Version

_No response_

---

_Label `bug` added by @UnboundVariable on 2025-07-25 01:52_

---

_Assigned to @UnboundVariable by @UnboundVariable on 2025-07-25 01:52_

---

_Label `server` added by @dhruvmanila on 2025-07-25 04:01_

---

_Added to milestone `GA` by @MichaReiser on 2025-07-25 06:16_

---

_Comment by @InSyncWithFoo on 2025-07-25 16:48_

Don't you mean, when `textDocument/references` is triggered on an overload's function name, ty should only return references <em>specific</em> to that overload?

```python
@overload
def f() -> None: ...
@overload
def f(v: str) -> None: ...
#   ^ Case 1: Overload function name
def f(v: str = '') -> None: ...
#   ^ Case 2: Implementation function name

f()       # Only 2
f('foo')  # Both 1 and 2
```

---

_Label `overloads` added by @AlexWaygood on 2025-10-06 16:30_

---

_Unassigned @UnboundVariable by @MichaReiser on 2025-11-14 08:37_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-12-05 14:03_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:36_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:36_

---

_Comment by @AdemFr on 2026-01-06 10:47_

Not sure if also relevant / similar issue: https://github.com/astral-sh/ty/issues/2362

---
