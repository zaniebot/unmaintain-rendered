```yaml
number: 2581
title: "False negative when `match`ing a Protocol"
type: issue
state: open
author: lihu-zhong
labels:
  - Protocols
assignees: []
created_at: 2026-01-21T17:12:58Z
updated_at: 2026-01-21T23:44:43Z
url: https://github.com/astral-sh/ty/issues/2581
synced_at: 2026-01-22T00:08:39Z
```

# False negative when `match`ing a Protocol

---

_@lihu-zhong_

### Summary

**Observed behavior:**

`ty` correctly raises an `invalid-argument-type` error when using a `Protocol` in an `isinstance()` check, but doesn't raise the same error when an equivalent runtime check is done with `match`:

```python
from dataclasses import dataclass
from typing import Any, Protocol


class Foo(Protocol):
    foo: str


@dataclass
class ImplFoo:
    foo: str = "bar"


def isinstance_check(input: Any) -> None:
    if isinstance(input, Foo):
        input.foo


def match_check(input: Any) -> None:
    match input:
        case Foo():
            input.foo


isinstance_check(ImplFoo())
match_check(ImplFoo())
```
In the above code, `ty` raises one error.

**Expected Behavior:**

`ty` should raise the same error on the `match` structure, since it raises the same runtime error. `mypy` misses this as well, but `pyright` catches it.

### Version

0.0.10

---

_Label `Protocols` added by @AlexWaygood on 2026-01-21 18:13_

---

_Comment by @AlexWaygood on 2026-01-21 19:50_

Thanks! Yes, it seems reasonable to expand the rule so that it catches this runtime error. Mypy and pyrefly have the same false negative that we do here, but pyright appears to catch this.

Edit: oops, i see you already analysed what other type checkers do here — thanks!

---

_Comment by @denyszhak on 2026-01-21 23:31_

@AlexWaygood May I take on this one?

---

_Comment by @carljm on 2026-01-21 23:44_

@denyszhak Certainly!

---

_Assigned to @denyszhak by @carljm on 2026-01-21 23:44_

---
