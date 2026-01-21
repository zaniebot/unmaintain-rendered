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
updated_at: 2026-01-21T18:13:07Z
url: https://github.com/astral-sh/ty/issues/2581
synced_at: 2026-01-21T19:04:47Z
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
