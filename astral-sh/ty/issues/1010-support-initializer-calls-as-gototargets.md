```yaml
number: 1010
title: Support initializer calls as GotoTargets
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-08-15T14:36:23Z
updated_at: 2025-09-02T18:49:16Z
url: https://github.com/astral-sh/ty/issues/1010
synced_at: 2026-01-12T15:54:24Z
```

# Support initializer calls as GotoTargets

---

_@Gankra_

```py
class MyClass:
    def __init__(self, val):
        self.val = val

x = MyClass(val)
    ^^^^^^^
```

Currently all of these symbols are treated as references to MyClass instead of their concrete initializer invocation.

This would empower:
* goto-definition
* goto-declaration
* goto-type-definition 
* docstrings


---

_Added to milestone `Beta` by @Gankra on 2025-08-15 14:36_

---

_Label `server` added by @Gankra on 2025-08-15 14:36_

---

_Assigned to @Gankra by @Gankra on 2025-08-15 15:35_

---

_Closed by @Gankra on 2025-09-02 18:49_

---
