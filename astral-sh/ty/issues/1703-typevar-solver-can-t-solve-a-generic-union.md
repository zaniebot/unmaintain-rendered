```yaml
number: 1703
title: "typevar solver can't solve a generic union"
type: issue
state: open
author: suleymanozkeskin
labels:
  - generics
assignees: []
created_at: 2025-12-01T11:37:57Z
updated_at: 2025-12-05T20:11:13Z
url: https://github.com/astral-sh/ty/issues/1703
synced_at: 2026-01-12T15:54:25Z
```

# typevar solver can't solve a generic union

---

_@suleymanozkeskin_

### Summary

# Summary

Incomplete type narrowing with `TypeIs` on generic types after early return

## Description

Type narrowing fails for **generic types** when using a `TypeIs` type guard with an early return pattern. Pyright correctly narrows the type in this scenario, but `ty` does not. The issue does **not** occur with simple (non-generic) classes.

## Minimal Reproducible Example

```python
# exp.py
from typing import Union, Generic, TypeVar
from typing_extensions import TypeIs

T = TypeVar('T', covariant=True)
E = TypeVar('E', covariant=True)


class Ok(Generic[T]):
    def __init__(self, value: T):
        self._value = value
    
    @property
    def ok_value(self) -> T:
        return self._value
    
    def is_err(self) -> bool:
        return False


class Err(Generic[E]):
    def __init__(self, error: E):
        self._error = error
    
    @property
    def err_value(self) -> E:
        return self._error
    
    def is_err(self) -> bool:
        return True


Result = Union[Ok[T], Err[E]]


def is_err(result: Result[T, E]) -> TypeIs[Err[E]]:
    """Type guard to check if a result is an Err"""
    return result.is_err()

# ------------------------------------------------------------

def some_function() -> Result[int, Exception]:
    condition = False
    if condition is False:
        return Err(Exception("Test error"))
    return Ok(1)

def use_some_function():    
    result = some_function()
    
    if is_err(result):
        print(result.err_value)
        return None

    # After the is_err() check and early return,
    # result should be narrowed to Ok[int]
    okay = result.ok_value  # <- ty reports warning here
    print(okay)
```

## Expected Behavior

After the `if is_err(result): return None` check, `result` should be narrowed to `Ok[int]`, making `result.ok_value` valid without warnings.

## Actual Behavior

```bash
uv run ty check exp.py
warning[possibly-missing-attribute]: Attribute `ok_value` may be missing on object of type `(Ok[int] & ~Err[Unknown]) | (Err[Exception] & ~Err[Unknown])`
  --> exp.py:57:12
   |
55 |     # After the is_err() check and early return,
56 |     # result should be narrowed to Ok[int]
57 |     okay = result.ok_value  # <- ty reports warning here
   |            ^^^^^^^^^^^^^^^
58 |     print(okay)
   |
info: rule `possibly-missing-attribute` is enabled by default
```

## Comparison with Pyright

Pyright correctly handles this pattern with 0 errors, 0 warnings for both generic and non-generic types.

- Simple classes with `TypeIs`: Works correctly in both `ty` and Pyright
- Generic classes with `TypeIs`: Fails in `ty`, works in Pyright

## Notes

- Pattern matching (`match`/`case`) works correctly in both type checkers, even with generics
- The issue specifically occurs when combining:
  1. Generic types (e.g., `Union[Ok[T], Err[E]]`)
  2. `TypeIs` type guards
  3. Early return control flow
- Type narrowing appears to be incomplete when the type guard eliminates one branch of a generic union

## Environment

- `ty` version: 0.0.1a29
- Python version: 3.13


### Version

0.0.1a29

---

_Comment by @carljm on 2025-12-02 01:56_

Thanks for the report! This looks like a bug indeed. I think it is a limitation of our current typevar solver; it isn't able to match the generic union parameter type against the union argument and solve both typevars. [This simplified example](https://play.ty.dev/32048e84-3f13-4553-aac7-5d51daccaaa2) (without `TypeIs`, and also without the generic implicit alias, to clarify that's not part of the problem either) shows the root cause:

```py
from typing import Union, Generic, TypeVar

T = TypeVar('T', covariant=True)
E = TypeVar('E', covariant=True)


class Ok(Generic[T]):
    def __init__(self, value: T):
        self._value = value
    
    @property
    def ok_value(self) -> T:
        return self._value
    
    def is_err(self) -> bool:
        return False


class Err(Generic[E]):
    def __init__(self, error: E):
        self._error = error
    
    @property
    def err_value(self) -> E:
        return self._error
    
    def is_err(self) -> bool:
        return True



def err_kind(result: Union[Ok[T], Err[E]]) -> E | None:
    pass

# ------------------------------------------------------------

def f(result: Union[Ok[int], Err[Exception]]):
    reveal_type(err_kind(result))  # reveals `Unknown | None`, should reveal `Exception | None`
```

---

_Label `generics` added by @carljm on 2025-12-02 01:58_

---

_Added to milestone `Beta` by @carljm on 2025-12-02 01:59_

---

_Renamed from "limited type narrowing" to "typevar solver can't solve a generic union" by @carljm on 2025-12-02 02:03_

---

_Removed from milestone `Beta` by @carljm on 2025-12-05 00:40_

---

_Added to milestone `Stable` by @carljm on 2025-12-05 00:40_

---

_Closed by @suleymanozkeskin on 2025-12-05 20:09_

---

_Reopened by @AlexWaygood on 2025-12-05 20:11_

---
