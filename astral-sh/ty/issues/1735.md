```yaml
number: 1735
title: "Flow-sensitive narrowing fails for `type(a) is int` check in `if/else` on union types"
type: issue
state: closed
author: 5j9
labels: []
assignees: []
created_at: 2025-12-03T03:11:40Z
updated_at: 2025-12-03T03:19:33Z
url: https://github.com/astral-sh/ty/issues/1735
synced_at: 2026-01-10T01:56:40Z
```

# Flow-sensitive narrowing fails for `type(a) is int` check in `if/else` on union types

---

_Issue opened by @5j9 on 2025-12-03 03:11_

### Summary

Code sample:
https://play.ty.dev/cc4b267b-5d4f-4915-a049-01ce6cc76728
```python
def f(a: int | str) -> int | str:
    if type(a) is int:
        a //= 2
    else:
        # Error: Attribute 'strip' may be missing on object of type 'int | str'
        a = a.strip() 
    return a
```

The type checker fails to correctly narrow the type of variable a inside the else block when the initial check uses `type(a) is int` on a union type.

Given the function signature `def f(a: int | str) -> int | str:`, in the else block, `a` must be narrowed to str, but the type checker reports a `typossibly-missing-attribute` error, treating `a` as `int | str`.


Expected Behavior / Workaround

Using `isinstance` works correctly:
```python
def f(a: int | str) -> int | str:
    if isinstance(a, int): # Correctly narrows to 'int' here
        a //= 2
    else:
        # Correctly narrows to 'str' here
        a = a.strip()
    return a
```

### Version

ty 0.0.1-alpha.29 (0c3cae494 2025-11-28)

---

_Comment by @carljm on 2025-12-03 03:19_

Hi! Thanks for trying out ty.

This type error from ty is correct. The type `int` in the annotation `a: int | str` is inhabited by many possible objects whose `type()` is not `int` -- it includes instances of subclasses of `int`, as well as instances of `bool`. `f(True)` is a valid call which will enter the `else` clause in the body of `f`.

This is precisely why `isinstance` works here but `type() is ...` does not. `isinstance(a, int)` will return `True` for all inhabitants of the type `int`; `type(a) is int` will not.

---

_Closed by @carljm on 2025-12-03 03:19_

---
