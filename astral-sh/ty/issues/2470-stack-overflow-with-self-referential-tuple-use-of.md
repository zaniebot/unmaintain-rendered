```yaml
number: 2470
title: Stack overflow with self referential tuple + use of list(...) constructor
type: issue
state: open
author: BaxHugh
labels:
  - fatal
  - type aliases
assignees: []
created_at: 2026-01-12T16:23:44Z
updated_at: 2026-01-12T16:25:46Z
url: https://github.com/astral-sh/ty/issues/2470
synced_at: 2026-01-12T18:23:17Z
```

# Stack overflow with self referential tuple + use of list(...) constructor

---

_@BaxHugh_

### Summary

## Summary

Ty crashes with a stack overflow error when analyzing certain recursive type aliases that include `tuple[RecursiveT, ...]` combined with specific type inference patterns. The crash occurs during type checking and results in the process terminating with:

```
thread '<unknown>' (<some-id>) has overflowed its stack fatal runtime error: stack overflow, aborting
```

The issue appears to be triggered specifically by the combination of:
1. A recursive type alias containing a tuple type i.e. `tuple[RecursiveT, ...]` or `tuple[RecursiveT, RecursiveT]`.
2. Using `list(...)`, `tuple(...)`, `dict(...)` constructors - other similar constructors not tried.

I think this is probably the same issue relating to the recursive type inference issues addressed in Ruff PR #20566.

### Reproduction Steps

Run `ty check` on any of the crashing examples below, note `list(...)` used here, but testing with `dict(...)` and `tuple(...)` also reproduced the error. 

**Command:** `ty check does_crash_1.py`

### Examples that Crash

**does_crash_1.py:** (recursive type is a union including tuple)
```python
type RecursiveT = int | list[RecursiveT] | tuple[RecursiveT, ...]


def foo(a: int, b: int) -> RecursiveT:
    some_intermediate_var = (a, b)
    return list(some_intermediate_var)
```

**does_crash_2.py:** (variadic tuple type only in the recursive type)
```python
type RecursiveT = tuple[RecursiveT, ...]  # | int | list[RecursiveT]


def foo(a: int, b: int) -> RecursiveT:
    some_intermediate_var = (a, b)
    return list(some_intermediate_var)
```

**does_crash_3.py:** (non-variadic tuple type only in the recursive type)
```python
type RecursiveT = tuple[RecursiveT, RecursiveT]  # | int | list[RecursiveT]


def foo(a: int, b: int) -> RecursiveT:
    some_intermediate_var = (a, b)
    return list(some_intermediate_var)
```

### Examples that Don't Crash

These examples help illustrate the specific conditions that trigger the crash, as these ones don't crash (it's not relevant that they don't pass the type check)

**does_not_crash_1.py** (removing `tuple[RecursiveT, ...]` prevents crash):
```python
type RecursiveT = int | list[RecursiveT]  # | tuple[RecursiveT, ...]


def foo(a: int, b: int) -> RecursiveT:
    some_intermediate_var = (a, b)
    return list(some_intermediate_var)
```

**does_not_crash_2.py** (using list comprehension instead of `list()` constructor prevents crash):
```python
type RecursiveT = int | list[RecursiveT] | tuple[RecursiveT, ...]


def foo(a: int, b: int) -> RecursiveT:
    some_intermediate_var = (a, b)
    return [v for v in some_intermediate_var]  # noqa: C416
```

**does_not_crash_3.py** (using generic function instead of recursive type alias prevents crash):
```python
def foo[T](a: T, b: T) -> tuple[T, ...]:
    some_intermediate_var = (a, b)
    return list(some_intermediate_var)
```

### Analysis

The pattern suggests the crash occurs when ty attempts to infer types in a recursive context involving:
- Tuple types.
- Constructors such as `list(...)`, `tuple(...)`, `dict(...)`.
- A function return type that matches the recursive type alias


### Version

ty 0.0.11

---

_Label `fatal` added by @AlexWaygood on 2026-01-12 16:25_

---

_Label `type aliases` added by @AlexWaygood on 2026-01-12 16:25_

---
