```yaml
number: 1162
title: "Consider improving compatibility with other type checkers around treatment of `Hashable`"
type: issue
state: open
author: AlexWaygood
labels:
  - needs-decision
  - wish
  - set-theoretic types
  - Protocols
assignees: []
created_at: 2025-09-10T10:48:51Z
updated_at: 2025-11-18T16:10:36Z
url: https://github.com/astral-sh/ty/issues/1162
synced_at: 2026-01-12T15:54:24Z
```

# Consider improving compatibility with other type checkers around treatment of `Hashable`

---

_@AlexWaygood_

In https://github.com/astral-sh/ruff/pull/20284, we reworked our logic so that `Hashable` is treated equivalently to `object` in subtyping and assignability checks. This is defensible from a theoretical perspective, since all instances of "exactly `object`" are hashable at runtime, which therefore makes `object` a subtype of `Hashable` by all normal rules of protocol assignability and subtyping. However, rules around hashability in Python do not obey the normal rules: hashable classes often inherit from unhashable ones, and unhashable classes often inherit from hashable ones, flying in the face of the Liskov Substitution Principle. Our current approach to `Hashable` and similar protocols makes these protocols effectively useless; we will not complain about code like this, even though users would expect us to do so (and even though other type checkers issue complaints):

```py
from typing import Hashable

def f(x: Hashable): ...

f([])
```

Given that the Liskov principle is so frequently and fragrantly violated for hashability, in Python's builtin types, standard library, and across a wide range of Python code, it may be worth us seeing if we can roll back https://github.com/astral-sh/ruff/pull/20284 and find another solution to https://github.com/astral-sh/ty/issues/1132 that improves our compatibility with other type checkers. The reason why other type checkers do not run into problems such as #1132 is that they do not eagerly simplify unions of `Hashable` and other types in the same way we do. We should consider doing the same; it would probably lead to more intuitive behaviour from users' perspective, and compatibility with other type checkers is also a useful goal.

---

_Label `wish` added by @AlexWaygood on 2025-09-10 10:48_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-09-10 10:48_

---

_Label `Protocols` added by @AlexWaygood on 2025-09-10 10:48_

---

_Label `needs-decision` added by @carljm on 2025-11-15 01:57_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
