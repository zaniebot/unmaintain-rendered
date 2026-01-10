```yaml
number: 1261
title: Excessive runtime in specialization of generic method with union of functions
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - performance
  - generics
assignees: []
created_at: 2025-09-26T09:59:34Z
updated_at: 2025-09-29T11:52:56Z
url: https://github.com/astral-sh/ty/issues/1261
synced_at: 2026-01-10T02:06:25Z
```

# Excessive runtime in specialization of generic method with union of functions

---

_Issue opened by @sharkdp on 2025-09-26 09:59_

The following snippet (distilled from an [ecosystem example on pandera](https://github.com/unionai-oss/pandera/blob/660095b7c985e45173f48b02a96655779158357a/tests/test_inspection_utils.py#L97), modified to make it self-contained and failing on `main`) causes an excessively large runtime which scales badly with both the number of `.merge` calls and the number of union elements:

```py
from __future__ import annotations
from typing import Self, reveal_type

class C[T]:
    value: T

    def __init__(self, v: T) -> None:
        self.value = v

    def merge(self: Self, other: Self) -> C[T]:
        raise NotImplementedError


class A1:
    def method(self): ...
class A2:
    def method(self): ...
class A3:
    def method(self): ...

def _(a: A1 | A2 | A3):
    c = C(a.method)
    reveal_type(c.merge(c).merge(c).merge(c))
```

This seems to be caused by unnecessary extra work when applying the specialization.


This currently blocks the merge of https://github.com/astral-sh/ruff/pull/20517 (`typing.Self` for the first parameter of instance methods)

---

_Label `bug` added by @sharkdp on 2025-09-26 09:59_

---

_Label `generics` added by @sharkdp on 2025-09-26 09:59_

---

_Assigned to @sharkdp by @sharkdp on 2025-09-26 10:02_

---

_Label `performance` added by @AlexWaygood on 2025-09-26 11:30_

---

_Comment by @sharkdp on 2025-09-26 11:43_

The debug-print of the `C[(bound method A1.method() -> Unknown) | (bound method A2.method() -> Unknown) | (bound method A3.method() -> Unknown)]` type (after going through `c.merge(c).merge(c).merge(c)`, which, in principle, shouldn't change the type) is 10 MiB large, or 45k lines long.

---

_Comment by @carljm on 2025-09-26 14:29_

This smells a bit like something that might be solved by abstracting more eagerly to a general callable type instead of a singleton bound-method type? We already do this in generic container inference.

---

_Added to milestone `Beta` by @carljm on 2025-09-26 14:30_

---

_Comment by @sharkdp on 2025-09-29 11:25_

closed by https://github.com/astral-sh/ruff/pull/20596

---

_Closed by @sharkdp on 2025-09-29 11:25_

---

_Comment by @AlexWaygood on 2025-09-29 11:28_

Should we add a regression test (or a regression benchmark) for this?

---

_Comment by @sharkdp on 2025-09-29 11:51_

I think it's okay *not* to add a benchmark for this, because as soon as https://github.com/astral-sh/ruff/pull/20517 is merged, regressing on this would cause timeouts in several projects across the ecosystem, and huge slowdowns in others. If you'd prefer a benchmark that you can run locally, though, I can certainly add one.

---

_Comment by @AlexWaygood on 2025-09-29 11:52_

No, that's fine, that makes sense! It's probably good to avoid exponential explosion in the number of benchmarks we have, as well as avoiding exponential explosion in the benchmarks themselves 😆

---
