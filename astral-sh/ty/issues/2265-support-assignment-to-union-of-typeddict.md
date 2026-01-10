---
number: 2265
title: "Support assignment to union of `TypedDict`"
type: issue
state: open
author: ibraheemdev
labels:
  - bidirectional inference
  - typeddict
assignees: []
created_at: 2025-12-29T22:07:07Z
updated_at: 2026-01-09T05:29:47Z
url: https://github.com/astral-sh/ty/issues/2265
synced_at: 2026-01-10T01:48:23Z
---

# Support assignment to union of `TypedDict`

---

_Issue opened by @ibraheemdev on 2025-12-29 22:07_

We currently only consider the type context if there is a single `TypedDict` element. We should support the following:

```py
from typing import TypedDict

class Foo(TypedDict):
    foo: int

class Bar(TypedDict):
    bar: int

x: Foo | Bar = reveal_type({ "foo": 1 }) # Foo

class FooBar1(TypedDict):
    foo: int
    bar: int

class FooBar2(TypedDict):
    foo: int
    bar: int

x: FooBar1 | FooBar2 = reveal_type({ "foo": 1, "bar": 1 }) # FooBar1 | FooBar2
```

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-12-29 22:07_

---

_Label `typeddict` added by @ibraheemdev on 2025-12-29 22:07_

---

_Added to milestone `Stable` by @carljm on 2025-12-29 22:08_

---

_Comment by @ibraheemdev on 2025-12-29 22:20_

Ah, interestingly, the examples above type check, but fail if I remove the `reveal_type` call, because we attempt to narrow the type context of the generic call.

---

_Assigned to @ibraheemdev by @ibraheemdev on 2026-01-09 05:29_

---
