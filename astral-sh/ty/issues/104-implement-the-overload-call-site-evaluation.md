```yaml
number: 104
title: Implement the overload call site evaluation algorithm
type: issue
state: closed
author: dhruvmanila
labels:
  - overloads
assignees: []
created_at: 2025-05-01T19:33:19Z
updated_at: 2025-06-27T15:36:03Z
url: https://github.com/astral-sh/ty/issues/104
synced_at: 2026-01-10T02:07:35Z
```

# Implement the overload call site evaluation algorithm

---

_Issue opened by @dhruvmanila on 2025-05-01 19:33_

Reference spec: https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation

---

_Renamed from "Implement the call site evaluation algorithm as mentioned in the spec" to "[red-knot] Implement the overload call site evaluation algorithm" by @dhruvmanila on 2025-05-01 19:33_

---

_Comment by @dhruvmanila on 2025-05-01 19:35_

~I'm marking this as "Alpha" milestone because I want to try to implement some parts of it before then if possible otherwise the complete implementation would be completed before the "Beta" milestone. Happy to move it directly to "Beta" if other prefer.~

Moved to beta as I don't think I'll have time to implement any part of it.

---

_Comment by @JelleZijlstra on 2025-05-06 01:50_

Here's a concrete example of an issue with overloads I ran into trying out ty on some sample code:

```py
from typing import reveal_type

def x(func):
    func = getattr(func, "__func__", func)
    reveal_type(func)
```
This [produces](https://playknot.ruff.rs/97839168-8660-478a-8d6d-303b3faee560) `Any | None` but should be `Any`. [Definition in typeshed](https://github.com/python/typeshed/blob/4265ee7c72f476e7156949e55784fd82b40e6953/stdlib/builtins.pyi#L1444) for reference.

(Just mentioning this as an example that probably a good number of people trying out the alpha will run into.)

---

_Renamed from "[red-knot] Implement the overload call site evaluation algorithm" to "Implement the overload call site evaluation algorithm" by @MichaReiser on 2025-05-07 15:24_

---

_Label `overloads` added by @AlexWaygood on 2025-05-10 18:01_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-05-21 15:28_

---

_Comment by @dhruvmanila on 2025-06-12 12:19_

> This [produces](https://playknot.ruff.rs/97839168-8660-478a-8d6d-303b3faee560) `Any | None` but should be `Any`. [Definition in typeshed](https://github.com/python/typeshed/blob/4265ee7c72f476e7156949e55784fd82b40e6953/stdlib/builtins.pyi#L1444) for reference.

Just to confirm that this should produce `Any` because the overload matching is ambiguous and so the return type should be `Any`, is this correct?

---

_Comment by @JelleZijlstra on 2025-06-12 13:55_

I think so, I haven't fully worked the example through the spec but Any is clearly the right answer from a user perspective.

---

_Comment by @dhruvmanila on 2025-06-13 04:37_

I think it should be `Any` according to the spec. 

Pasting the definition from the above mentioned linked commit here for better visibility:
```py
@overload
def getattr(o: object, name: str, /) -> Any: ...

# While technically covered by the last overload, spelling out the types for None, bool
# and basic containers help mypy out in some tricky situations involving type context
# (aka bidirectional inference)
@overload
def getattr(o: object, name: str, default: None, /) -> Any | None: ...
@overload
def getattr(o: object, name: str, default: bool, /) -> Any | bool: ...
@overload
def getattr(o: object, name: str, default: list[Any], /) -> Any | list[Any]: ...
@overload
def getattr(o: object, name: str, default: dict[Any, Any], /) -> Any | dict[Any, Any]: ...
@overload
def getattr(o: object, name: str, default: _T, /) -> Any | _T: ...
```

And, the call:
```py
from typing import reveal_type

def x(func):
    func = getattr(func, "__func__", func)
    reveal_type(func)
```

The argument types are `(Unknown, Literal["__func__"], Unknown)` (can substitute `Unknown` with `Any`).

According to the spec:
1. Arity check would filter out the first overload because it only accepts two arguments
2. Type checking would keep all the remaining overloads
3. Step 3 is skipped since step 2 did not result in errors for all overloads
4. Step 4 has no effect, since neither overload has a variadic parameter
5. For simplicity, we'll only consider the third argument as that's where the parameter types differ and is more relevant here. All materializations of `Unknown` are not assignable to any of the (first) 4 (out of the 5) remaining overloads, it's assignable to only the last overload. This means that none of the overloads are filtered out. Here, the return types of the remaining overloads are not equivalent, so the final return type of this call would be `Any`

This is what ty does in https://github.com/astral-sh/ruff/pull/18607.

---

_Comment by @dhruvmanila on 2025-06-27 15:36_

I'm going to mark this as resolved as there's only one step remaining and I think it's easy enough once argument splatting has landed on `main` and it has it's own issue to keep track of.

A follow-up here w.r.t. generics is #669.

---

_Closed by @dhruvmanila on 2025-06-27 15:36_

---
