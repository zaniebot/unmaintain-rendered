---
number: 2193
title: support externally patching methods on instances
type: issue
state: open
author: carljm
labels:
  - callables
assignees: []
created_at: 2025-12-23T21:04:18Z
updated_at: 2025-12-24T13:04:42Z
url: https://github.com/astral-sh/ty/issues/2193
synced_at: 2026-01-10T01:52:52Z
---

# support externally patching methods on instances

---

_Issue opened by @carljm on 2025-12-23 21:04_

Initially reported at https://github.com/astral-sh/ty/issues/2134

We should support patching a method on an instance, if the replacement has compatible signature.

https://play.ty.dev/80fd5a68-8c7d-4e73-b5e8-6906a9f9bf61

```py
class RateLimiter:
    def check_limit(self) -> bool:
        return True

def test_lambda_stub_method():
    rl = RateLimiter()

    # Common pattern: stub method with lambda to control return value
    rl.check_limit = lambda: False  # ty: invalid-assignment
```

---

_Added to milestone `Stable` by @carljm on 2025-12-23 21:04_

---

_Label `callables` added by @carljm on 2025-12-23 21:04_

---

_Comment by @carljm on 2025-12-23 21:05_

#350 is also somewhat related

---

_Comment by @AlexWaygood on 2025-12-24 13:04_

I think @sharkdp previously did experiments around this. Possibly in https://github.com/astral-sh/ruff/pull/18275?

---
