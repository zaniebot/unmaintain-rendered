```yaml
number: 674
title: "specialized heterogeneous tuple doesn't specialize its methods"
type: issue
state: closed
author: carljm
labels:
  - bug
  - generics
assignees: []
created_at: 2025-06-17T23:59:52Z
updated_at: 2025-06-23T06:37:10Z
url: https://github.com/astral-sh/ty/issues/674
synced_at: 2026-01-10T02:08:20Z
```

# specialized heterogeneous tuple doesn't specialize its methods

---

_Issue opened by @carljm on 2025-06-17 23:59_

### Summary

When we specialize a list, its methods are also specialized as expected, but the same isn't true for a heterogeneous tuple:

```py
from typing import reveal_type

def f(alist: list[int], atuple: tuple[int]):
    reveal_type(alist)  # revealed: list[int]
    reveal_type(alist.__iter__)  # revealed: bound method list[int].__iter__() -> Iterator[int]
    reveal_type(alist.__iter__())  # revealed: Iterator[int]

    reveal_type(atuple)  # revealed: tuple[int]
    # The Unknown in the next two lines should be int
    reveal_type(atuple.__iter__)  # revealed: bound method tuple[int].__iter__() -> Iterator[Unknown]
    reveal_type(atuple.__iter__())  # revealed: Iterator[Unknown]
```

https://play.ty.dev/adcf727b-4cd6-4c52-94de-fe647dde540c

### Version

_No response_

---

_Added to milestone `Beta` by @carljm on 2025-06-17 23:59_

---

_Label `bug` added by @carljm on 2025-06-17 23:59_

---

_Label `generics` added by @carljm on 2025-06-17 23:59_

---

_Comment by @AlexWaygood on 2025-06-18 00:08_

The bug is only present for heterogeneous tuples, which makes sense, since they need to be special-cased far more than homogeneous tuples. I guess we should be falling back to the heterogeneous tuple's homogeneous-tuple supertype (`tuple[int, ...]` for something like `tuple[int, int]`; `tuple[int | str, ...]` for something like `tuple[int, str, int]`, etc), but instead it looks like we end up falling back to the default specialisation of tuple

---

_Renamed from "specialized tuple doesn't specialize its methods" to "specialized heterogeneous tuple doesn't specialize its methods" by @AlexWaygood on 2025-06-18 00:08_

---

_Comment by @Kroppeb on 2025-06-18 19:29_

Calling a single element tuple "heterogeneous" feels weird

---

_Comment by @dcreager on 2025-06-19 22:04_

This should be fixed by https://github.com/astral-sh/ruff/pull/18600

---

_Assigned to @dcreager by @carljm on 2025-06-20 14:56_

---

_Comment by @sharkdp on 2025-06-23 06:37_

This seems to work now

---

_Closed by @sharkdp on 2025-06-23 06:37_

---
