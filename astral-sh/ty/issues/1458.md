```yaml
number: 1458
title: Class pattern matching fails for final non-bivariant classes
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - narrowing
assignees: []
created_at: 2025-10-30T18:07:36Z
updated_at: 2025-10-30T20:44:53Z
url: https://github.com/astral-sh/ty/issues/1458
synced_at: 2026-01-10T02:06:25Z
```

# Class pattern matching fails for final non-bivariant classes

---

_Issue opened by @MeGaGiGaGon on 2025-10-30 18:07_

### Summary

If you try to pattern match on a final non-bivariant class using a [class pattern](https://docs.python.org/3/reference/compound_stmts.html#class-patterns), that match arm will be treated as never, despite being taken at runtime and working if any of the conditions are removed.

https://play.ty.dev/b6317b80-39f5-4e45-af1e-977eea84c3ae
```py
from typing import final, assert_never

@final
class FinalCovariant[T]:
    def __init__(self, value: T):
        self._value = value

def uses_final_covariant(value: FinalCovariant[int]):
    match value:
        case FinalCovariant():
            reveal_type(value)  # Should not be `Never`, but is
            assert_never()  # Should not pass, but does
```

<details>
<summary>Examples showing similar code working, and runtime test</summary>

https://play.ty.dev/6e0c778e-50fa-4239-a81e-747ac9351f3a
```py
from typing import final, assert_never

@final
class FinalCovariant[T]:
    def __init__(self, value: T):
        self._value = value

def uses_final_covariant(value: FinalCovariant[int]):
    match value:
        case FinalCovariant():
            reveal_type(value)  # Should not be `Never`, but is
            assert_never()  # Should not pass, but does



class Covariant[T]:
    def __init__(self, value: T):
        self._value = value

def uses_covariant(value: Covariant[int]):
    match value:
        case Covariant():
            reveal_type(value)  # Is not `Never`
            assert_never()  # Correctly does not pass


@final
class FinalBivariant[T]: ...

def uses_final_bivariant(value: FinalBivariant[int]):
    match value:
        case FinalBivariant():
            reveal_type(value)  # Is not `Never`
            assert_never()  # Correctly does not pass
```

```pycon
>>> from typing import final, assert_never)
...
... @final
... class FinalCovariant[T]:
...     def __init__(self, value: T):
...         self._value = value
...
... def uses_final_covariant(value: FinalCovariant[int]):
...     match value:
...         case FinalCovariant():
...             reveal_type(value)  # Should not be `Never`, but is
...             assert_never()  # Should not pass, but does
...
>>> from typing import reveal_type
>>> uses_final_covariant(FinalCovariant(1))
Runtime type is 'FinalCovariant'
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    uses_final_covariant(FinalCovariant(1))
    ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^
  File "<python-input-2>", line 12, in uses_final_covariant
    assert_never()  # Should not pass, but does
    ~~~~~~~~~~~~^^
TypeError: assert_never() missing 1 required positional argument: 'arg'
```

### Version

playground (5139f76d1)

---

_Comment by @carljm on 2025-10-30 18:42_

Thanks for the report!

Without `@final` we still get a wrong result, just differently wrong. We get `Covariant[int] & Covariant[Unknown]`, but we don't simplify to `Never` because the type isn't final.

I think this should be a fairly simple fix: this `match` narrowing needs to use the top materialization rather than the unknown-materialization, like we do for `isinstance` narrowing on a generic type.

---

_Added to milestone `GA` by @carljm on 2025-10-30 18:42_

---

_Label `bug` added by @carljm on 2025-10-30 18:42_

---

_Label `narrowing` added by @carljm on 2025-10-30 18:42_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-30 19:10_

---

_Closed by @AlexWaygood on 2025-10-30 20:44_

---
