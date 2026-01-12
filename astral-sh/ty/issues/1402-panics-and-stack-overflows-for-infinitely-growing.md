```yaml
number: 1402
title: Panics and stack overflows for infinitely growing parameter default types
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-10-20T11:06:03Z
updated_at: 2025-10-21T17:13:38Z
url: https://github.com/astral-sh/ty/issues/1402
synced_at: 2026-01-12T15:54:25Z
```

# Panics and stack overflows for infinitely growing parameter default types

---

_@sharkdp_

On the type-of-`self` PR, we panic on `setuptools` due to an example similar to the following (minified and modified). This is also reproducible on `main`:

```py
class C:
    def f(self: "C"):
        self.x = lambda a=self.x: a
```

I assume this happens because we build up an infinitely growing type for the type of the parameter default.

Lambdas are certainly not the only syntactical construct affected here. For example, this stack overflows:

```py
class C:
    def f(self: "C"):
        def inner(a: int=self.x):
            return

        self.x = inner

reveal_type(C().f)
```

---

_Added to milestone `Beta` by @sharkdp on 2025-10-20 11:06_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-20 11:06_

---

_Label `bug` added by @sharkdp on 2025-10-20 11:06_

---

_Label `fatal` added by @sharkdp on 2025-10-20 11:06_

---

_Closed by @sharkdp on 2025-10-21 17:13_

---
