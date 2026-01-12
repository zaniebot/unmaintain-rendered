```yaml
number: 1750
title: "`tuple[Any, ...]` not assignable to `tuple[int, *tuple[int, ...]]`"
type: issue
state: closed
author: jorenham
labels:
  - bug
  - typing semantics
assignees: []
created_at: 2025-12-03T21:46:12Z
updated_at: 2025-12-05T19:04:25Z
url: https://github.com/astral-sh/ty/issues/1750
synced_at: 2026-01-12T15:54:25Z
```

# `tuple[Any, ...]` not assignable to `tuple[int, *tuple[int, ...]]`

---

_@jorenham_

### Summary

```py
from typing import Any

def f(shape: tuple[int, *tuple[int, ...]], /): ...

any_shape: tuple[Any, ...]
f(any_shape)  # ❌
```

```
Argument to function `f` is incorrect: Expected `tuple[int, *tuple[int, ...]]`, found `tuple[Any, ...]` (invalid-argument-type)
```

https://play.ty.dev/87952f02-8ba2-4d51-beb5-2e5920486b1e

### Version

_No response_

---

_Label `bug` added by @carljm on 2025-12-03 21:49_

---

_Label `typing semantics` added by @carljm on 2025-12-03 21:50_

---

_Added to milestone `Stable` by @carljm on 2025-12-03 21:50_

---

_Comment by @AlexWaygood on 2025-12-03 21:52_

Hmm yes, we should consider this to be assignable due to the exception the spec carves out:

> The type `tuple[Any, ...]` is special in that it is [consistent](https://typing.python.org/en/latest/spec/glossary.html#term-consistent) with all tuple types, and [assignable](https://typing.python.org/en/latest/spec/glossary.html#term-assignable) to a tuple of any length. This is useful for gradual typing. The type `tuple` (with no type arguments provided) is equivalent to `tuple[Any, ...]`.

https://typing.python.org/en/latest/spec/tuples.html#tuple-type-form

(In general, `tuple[T,...]` is not assignable to `tuple[T, *tuple[T, ...]]`

---

_Comment by @jorenham on 2025-12-03 21:54_

Yes exactly. The use-case is for annotating numpy arrays with unknown shape, i.e. as gradual shape-type.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-05 03:04_

---

_Closed by @charliermarsh on 2025-12-05 19:04_

---
