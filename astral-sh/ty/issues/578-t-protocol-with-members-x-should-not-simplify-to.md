```yaml
number: 578
title: "`T & <protocol with members 'x'>` should not simplify to `T` if `T.x` is possibly unbound"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - Protocols
assignees: []
created_at: 2025-06-03T18:04:14Z
updated_at: 2025-06-04T19:39:16Z
url: https://github.com/astral-sh/ty/issues/578
synced_at: 2026-01-12T15:54:23Z
```

# `T & <protocol with members 'x'>` should not simplify to `T` if `T.x` is possibly unbound

---

_@AlexWaygood_

### Summary

Consider the following snippet:

```py
def b() -> bool:
    return False

class Foo:
    if b():
        x: int = 42

def f(y: Foo):
    if hasattr(y, "x"):
        reveal_type(y)  # revealed: Foo
        print(y.x)  # error[possibly-unbound-attribute]: Attribute `x` on type `Foo` is possibly unbound
```

https://play.ty.dev/4171e13d-f9e5-432d-baa9-fc25f414647e

In the `hasattr()` branch, we narrow the type of `y` to `Foo & <protocol with members 'x'>`, but then immediately simplify that intersection to `Foo`, since we know that all `Foo` members have an `x` member. But this simplification is incorrect, because it causes us to then emit a false positive when attempting to access the `x` member: we report the attribute as possibly unbound, even though the user just checked that it isn't!

We should only simplify these synthesized-protocol intersections if another element in the intersection has a _definitely bound_ attribute of the relevant name.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-06-03 18:04_

---

_Label `bug` added by @AlexWaygood on 2025-06-03 18:04_

---

_Label `Protocols` added by @AlexWaygood on 2025-06-03 18:04_

---

_Closed by @AlexWaygood on 2025-06-04 19:39_

---
