```yaml
number: 653
title: Support narrowing generic upper bounds
type: issue
state: open
author: thejchap
labels:
  - bug
  - generics
assignees: []
created_at: 2025-06-13T23:06:27Z
updated_at: 2025-11-13T07:38:25Z
url: https://github.com/astral-sh/ty/issues/653
synced_at: 2026-01-12T15:54:23Z
```

# Support narrowing generic upper bounds

---

_@thejchap_

### Summary

No error should be emitted on this snippet:

```py
class A:
    pass

class B:
    x: int = 0

def _[C: A | B](c: C):
    if isinstance(c, B):
        reveal_type(c)
        c.x = 42
```

More discussion here: https://github.com/astral-sh/ruff/pull/18527#discussion_r2142659512

Example: https://play.ty.dev/62d91bdb-cbf1-4758-8f2c-6a26722c9f47

---

_Label `generics` added by @carljm on 2025-06-14 05:36_

---

_Label `bug` added by @carljm on 2025-06-14 05:36_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 07:38_

---
