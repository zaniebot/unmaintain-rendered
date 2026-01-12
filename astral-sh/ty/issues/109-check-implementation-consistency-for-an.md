```yaml
number: 109
title: Check implementation consistency for an overloaded function
type: issue
state: open
author: dhruvmanila
labels:
  - help wanted
  - overloads
assignees: []
created_at: 2025-04-30T18:56:06Z
updated_at: 2025-06-11T01:01:14Z
url: https://github.com/astral-sh/ty/issues/109
synced_at: 2026-01-12T15:54:22Z
```

# Check implementation consistency for an overloaded function

---

_@dhruvmanila_

The task here is to check whether the overloaded signatures is consistent with the implementation signature if it's present. Specifically, from the spec:

> If an overload implementation is defined, type checkers should validate that it is consistent with all of its associated overload signatures. The implementation should accept all potential sets of arguments that are accepted by the overloads and should produce all potential return types produced by the overloads. In typing terms, this means the input signature of the implementation should be [assignable](https://typing.python.org/en/latest/spec/glossary.html#term-assignable) to the input signatures of all overloads, and the return type of all overloads should be assignable to the return type of the implementation.
> 
> If the implementation is inconsistent with its overloads, a type checker should report an error.
>
> https://typing.python.org/en/latest/spec/overload.html#implementation-consistency

The following function is the one where all overload checks happen:

https://github.com/astral-sh/ruff/blob/bd5fb826bce149d8a834767e6c175c2e4d90105a/crates/red_knot_python_semantic/src/types/infer.rs#L990-L990

Specifically, it loops over each of the overloaded function that needs to be checked here:

https://github.com/astral-sh/ruff/blob/bd5fb826bce149d8a834767e6c175c2e4d90105a/crates/red_knot_python_semantic/src/types/infer.rs#L1036-L1039

which are used to perform various checks.

**Notes:**
* We can reuse the [`INVALID_OVERLOAD`](https://github.com/astral-sh/ruff/blob/bd5fb826bce149d8a834767e6c175c2e4d90105a/crates/red_knot_python_semantic/src/types/diagnostic.rs#L487-L487) lint rule to raise a diagnostic for this check


---

_Label `help wanted` added by @dhruvmanila on 2025-04-30 18:56_

---

_Comment by @LaBatata101 on 2025-04-30 20:48_

Can I try this one?

---

_Comment by @dhruvmanila on 2025-04-30 21:09_

Sure!

---

_Assigned to @LaBatata101 by @dhruvmanila on 2025-04-30 21:10_

---

_Renamed from "[red-knot] Check implementation consistency for an overloaded function" to "Check implementation consistency for an overloaded function" by @MichaReiser on 2025-05-07 15:25_

---

_Label `overloads` added by @AlexWaygood on 2025-05-10 18:04_

---

_Comment by @j178 on 2025-06-05 15:22_

Hi, I ran into an issue when playing with this.

When using `Signature::is_assignable_to(f1, f2)`, it's supposed to check if  the **return type** of `f1` is assignable to the return type of `f2`, and if the **input type** of `f2` is assignable to the input type `f1`.

Then the following snippet will error becase the implementation input type (`str|int`) is not assignable to `int` or `str`:

```py
from typing import overload

@overload
def f(x: int) -> int: pass
@overload
def f(x: str) -> str: pass
def f(x: str|int) -> str|int:  # error: [invalid-overload]
    return x
```

but neither `mypy` nor `pyright` do not report error on this: [mypy playground](https://mypy-play.net/?mypy=latest&python=3.12&gist=afa534e1eae815ceab3f91930c562acb), [pyright playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMpgBuApiADZgCGAJgDRQDClZZlARmcQFBcACRpCjS7ViwKMAAUADwBcmFDACUUALQA%2BBTHkJKAZz18B5KtRFiJM%2BXpggVGqDZA79h0eKlzHtgD6plappOforyUADEUChgUKTgIFxQSVAgxDAAriAoUNJcQA)

I also tried this code in `pyright`: [pyright playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMpgBuApiADZgCGAJgFC0ACRpFNt1xwUwAFAB4AuTChgBKKAFoAfMJhCElAM6LGzclTocuvQVEUwQ46XoPylKrd35CARnBjFFRmXYeKBtKF6ghiMAK4gKFB8tEA)

```py
from typing import overload

@overload
def f(x: int) -> int: pass
@overload
def f(x: str) -> str: pass
def f(x: bytes) -> bytes:
    return x
```

Here’s what pyright says:
```
Overloaded implementation is not consistent with signature of overload 1
  Type "(x: bytes) -> bytes" is not assignable to type "(x: int) -> int"
    Parameter 1: type "int" is incompatible with type "bytes"
      "int" is not assignable to "bytes"
    Function return type "int" is incompatible with type "bytes"
      "int" is not assignable to "bytes"  (reportInconsistentOverload)
```

Looks like that pyright and mypy are checking both input type AND return type of each overload are assignable to the corresponding types in the implementation?

---

_Comment by @carljm on 2025-06-06 00:12_

@j178 Yes, signature assignability is not actually the right operation here, the sense is reversed for parameters vs return type. We sorted this out in https://github.com/astral-sh/ruff/pull/17800 but I guess we didn't update the issue description here.

Note also that @LaBatata101 already has a good PR up for this in https://github.com/astral-sh/ruff/pull/17800 -- it's just waiting on a different fix so it doesn't add too many false positives.

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 01:01_

---

_Added to milestone `GA` by @carljm on 2025-06-11 01:01_

---
