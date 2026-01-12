```yaml
number: 696
title: "Type of class object is narrowed to `Never`"
type: issue
state: closed
author: JelleZijlstra
labels:
  - control flow
assignees: []
created_at: 2025-06-24T13:11:00Z
updated_at: 2025-11-14T15:20:20Z
url: https://github.com/astral-sh/ty/issues/696
synced_at: 2026-01-12T15:54:23Z
```

# Type of class object is narrowed to `Never`

---

_@JelleZijlstra_

### Summary

Example:

```python
from typing import reveal_type

class X:
    pass

def f(x: X) -> int:
    match x:
        case X():
            return 1
        case _:
            reveal_type(X)
```
https://play.ty.dev/19d83388-93e2-47b5-875f-9786b4d2b076

ty here reveals Never for the type of X (the class object), which seems wrong. The branch is unreachable, but that shouldn't affect the type of (uppercase) X. Unrelated types are not narrowed; `reveal_type(int)` still gives `<class 'int'>`.

### Version

_No response_

---

_Comment by @sharkdp on 2025-06-24 13:48_

Thank you for reporting this.

> The branch is unreachable, but that shouldn't affect the type of (uppercase) X.

We generally infer `Never` for symbols in unreachable code if the use of the symbol is not reachable from the binding/definition. For example:
```py
x = 1

if False:
    reveal_type(x)  # Never
```

This is our current solution to silencing (most) diagnostics in unreachable code. This helps us solve cases like the ones described in this [test section](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/unreachable.md#conflicting-type-information).

This case is slightly different because we use a binding/definition from an outer scope. We still infer `Never` for somewhat [more complex reasons](https://github.com/astral-sh/ruff/blob/0edbd6c390a16677a1214a29f835444a1d522172/crates/ty_python_semantic/src/place.rs#L812-L861):
```py
x = 1

def _():
    if False:
        reveal_type(x)  # Never
```
As I just realized, this is not 100% consistent, because we do not infer `Never` here:
```py
x = 1

def _():
    if False:
        def _():
            reveal_type(x)  # Unknown | Literal[1]
```

> Unrelated types are not narrowed; `reveal_type(int)` still gives `<class 'int'>`.

Yes, this is not related to narrowing (lowercase `x` is narrowed, but uppercase `X` should not be narrowed at all). It's only related to reachability. Accessing builtins in unreachable code is another case where we do not infer `Never` for symbols. I haven't seen any cases where this causes problems, though.

---

_Label `control flow` added by @sharkdp on 2025-06-24 13:48_

---

_Comment by @AlexWaygood on 2025-06-24 14:02_

> Accessing builtins in unreachable code is another case where we do not infer `Never` for symbols. I haven't seen any cases where this causes problems, though.

It seems to mean that we infer `Unknown` in the `reveal_type` call here if you use `--python-version=3.10` or lower -- I suppose `Never` would be more consistent?

```py
import sys

if sys.version_info >= (3, 11):
    reveal_type(ExceptionGroup)
```

https://play.ty.dev/cc68bcc2-543a-4ddb-bda3-0efbe22d014c

---

_Renamed from "Type of class object is narrowed to never" to "Type of class object is narrowed to `Never`" by @AlexWaygood on 2025-10-06 16:13_

---

_Comment by @carljm on 2025-11-14 15:20_

I think our underlying behavior here is still the same, but today we silence `reveal_type` in unreachable code, like pyright does. Not 100% sure if this is the best behavior long-term, but I don't have a clear sense of what is actionable in this issue, so going to close it.

---

_Closed by @carljm on 2025-11-14 15:20_

---
