```yaml
number: 1383
title: Too many cycle iterations for infinitely wrapped instance of generic class
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-10-17T10:55:46Z
updated_at: 2025-10-22T12:29:12Z
url: https://github.com/astral-sh/ty/issues/1383
synced_at: 2026-01-12T15:54:25Z
```

# Too many cycle iterations for infinitely wrapped instance of generic class

---

_@sharkdp_

This is a heavily modified MRE for the ecosystem panic observed on "bokeh" in https://github.com/astral-sh/ruff/pull/20922. As with most of the previous bug reports sprawling from those PRs, this one is also reproducible on `main`, if we simply add a `self: Self` annotation. The reason why this leads to divergent behavior is probably that the generic context of `Value` keeps track of the recursive nesting (`Value[Value[Value[...[Value[T]]]]]`):

```py
from typing import Self

class Value[T]:
    def __init__(self, value: T) -> None: ...

class C:
    def f(self: Self) -> None:
        self.value = Value(self.value)
```

---

_Assigned to @sharkdp by @sharkdp on 2025-10-17 10:55_

---

_Label `bug` added by @sharkdp on 2025-10-17 10:55_

---

_Label `fatal` added by @sharkdp on 2025-10-17 10:55_

---

_Added to milestone `Beta` by @sharkdp on 2025-10-17 10:56_

---

_Comment by @sharkdp on 2025-10-17 12:50_

I guess the easier example would be
```py
from typing import Self

class C:
    def f(self: Self):
        self.x = [self.x]
```

---

_Comment by @dcreager on 2025-10-17 14:02_

At least the infinite type that we're inferring is correct! 🫠 

---

_Renamed from "Too many cycle iteratons for infinitely wrapped instance of generic class" to "Too many cycle iterations for infinitely wrapped instance of generic class" by @AlexWaygood on 2025-10-17 14:34_

---

_Closed by @sharkdp on 2025-10-22 12:29_

---
