```yaml
number: 17545
title: "[red-knot] Consider two instance types disjoint if the underlying classes have disjoint metaclasses"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/instance-disjointness
created_at: 2025-04-22T11:58:43Z
updated_at: 2025-04-22T14:14:12Z
url: https://github.com/astral-sh/ruff/pull/17545
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Consider two instance types disjoint if the underlying classes have disjoint metaclasses

---

_Pull request opened by @AlexWaygood on 2025-04-22 11:58_

## Summary

I noticed we were being inconsistent between `type[]` types and instance types when it came to whether we considered disjointness of the metaclasses when determining the disjointness of two types. Consider the following example:

```py
from typing import final, reveal_type
from knot_extensions import Intersection

@final
class Final1: ...

@final
class Final2: ...

@final
class Meta1(type): ...

@final
class Meta2(type): ...

class UsesMeta1(metaclass=Meta1): ...
class UsesMeta2(metaclass=Meta2): ...

def f(
    a: Intersection[Final1, Final2],
    b: Intersection[type[Final1], type[Final2]],
    c: Intersection[UsesMeta1, UsesMeta2],
    d: Intersection[type[UsesMeta1], type[UsesMeta2]]
):
    reveal_type(a)  # `Never`
    reveal_type(b)  # `Never`
    reveal_type(c)  # `UsesMeta1 & UsesMeta2` on `main`, but should also be `Never`
    reveal_type(d)  # `Never`
```

https://playknot.ruff.rs/d422556b-bce6-407d-80cd-31b602fec12b

If two instance types have disjoint metaclasses, the instance types must themselves be disjoint, as a class that simultaneously subclasses `A` and `B` can only exist if there could exist a metaclass that is a simultaneous subclass of `A`'s metaclass and `B`'s metaclass.

## Test Plan

New mdtests added.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-22 11:58_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-22 11:58_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-22 11:58_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-22 11:58_

---

_Renamed from "[red-knot] Consider two instance types disjoint if their metaclasses are disjoint" to "[red-knot] Consider two instance types disjoint if the underlying classes have disjoint metaclasses" by @AlexWaygood on 2025-04-22 12:00_

---

_Comment by @github-actions[bot] on 2025-04-22 12:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-22 12:08_

neutral on codspeed: https://codspeed.io/astral-sh/ruff/branches/alex%2Finstance-disjointness

---

_@carljm approved on 2025-04-22 14:12_

Excellent!

---

_Merged by @AlexWaygood on 2025-04-22 14:14_

---

_Closed by @AlexWaygood on 2025-04-22 14:14_

---

_Branch deleted on 2025-04-22 14:14_

---
