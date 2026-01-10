```yaml
number: 1407
title: "ty doesn't check if all union variants support a given binary operation"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - typing semantics
assignees: []
created_at: 2025-10-20T17:20:26Z
updated_at: 2025-10-20T17:51:17Z
url: https://github.com/astral-sh/ty/issues/1407
synced_at: 2026-01-10T02:06:25Z
```

# ty doesn't check if all union variants support a given binary operation

---

_Issue opened by @MichaReiser on 2025-10-20 17:20_

### Summary

```py
from __future__ import annotations

class Test:
    def __add__(self, other: Other, /) -> Other:
        return other

class Other:
    def __add__(self, other: Test, /) -> Other:
        return self

a: Test | Other = Test()

a + Other()
```

I would expect that ty reports an error for `a + Other()` because `Other` doesn't implement `add` with `Other` as its right hand side 


ty correctly reports an error if I change `a` to `a: Other = Other()`

https://play.ty.dev/c81ed42f-456e-4561-9825-8a6f0b9a15ff

### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-10-20 17:20_

---

_Label `typing semantics` added by @MichaReiser on 2025-10-20 17:20_

---

_Comment by @sharkdp on 2025-10-20 17:51_

> a: Test | Other = Test()
> 
> a + Other()

`a` in the lower expression is just `Test` (the inferred type). You need to actually have an *inferred* type of `Test | Other` if you want this to work. Or use a nested scope, which sees the declared type:
```py
a: Test | Other = Test()

def _():
    a + Other()
```

For examples like these, it's often easiest to go without a concrete value:
```py
def _(a: Test | Other):
    a + Other()
```

Both of these do emit an error, and in your example, we should not emit an error, because the flow-sensitive type of `a` is just `Test`.

---

_Closed by @sharkdp on 2025-10-20 17:51_

---
