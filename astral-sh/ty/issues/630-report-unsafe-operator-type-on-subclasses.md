```yaml
number: 630
title: report unsafe operator type on subclasses
type: issue
state: open
author: KotlinIsland
labels:
  - runtime semantics
assignees: []
created_at: 2025-06-11T01:08:12Z
updated_at: 2025-11-18T16:10:31Z
url: https://github.com/astral-sh/ty/issues/630
synced_at: 2026-01-12T15:54:23Z
```

# report unsafe operator type on subclasses

---

_@KotlinIsland_

### summary

```py
class A:
    def __add__(self, other: A) -> int:
        return 1


class B(A):
    def __radd__(self, other: A) -> str:
        return "B"


def f(a1: A, a2: A):
    x: int = a1 + a2
    print(x)  # static: int, runtime: "B"

f(A(), B())
```

here, the right hand subtype rule will be used, and `B.__radd__` will be called first, resulting in `"B"`, not `1`

if the supertype additionally defines a `__radd__`, then it can be incompatible with it's `__add__`, as all the information about the operation will be known

to ensure safety we must report when the subtypes `__radd__` in incompatable with the supertypes `__add__`, when the super type does not define a `__radd__`. but the right hand subtype rule complicates things...

## i see two options here:


1. always report a subtypes incompatible right hand operator as unsafe (mypy mode):
    ```py
    class A:
         def __add__(self, other: object) -> int: ...
         def __radd__(self, other: object) -> str: ...
    
    class B(A):
         def __radd__(self, other: object) -> str: ... # error: unsafe due to right hand subtype rule

    def f(a1: A = A(), a2: A = B()):
        # always assume the right hand can not declare an incompatible type
        a1 + a2  # static: int, runtime: str, but that's okay, we gave an error
    ``` 

2. always form a union when the lower bound is not known (my preference):
    ```py
    class A:
         def __add__(self, other: object) -> int: ...
         def __radd__(self, other: object) -> str: ...
    
         def __sub__(self, other: object) -> int: ...

         def __or__(self, other: object) -> int: ...

    class B(A):
         def __radd__(self, other: object) -> str: ... # no error, valid override of A.__radd__

         def __rsub__(self, other: object) -> Literal[1]: ... # not error, compatiable with A.__add__

         def __ror__(self, other: object) -> str: ...  # error, incompatible with A.__or__

    def f(a1: A = A(), a2: A = B()):
        # when we don't know what `a2` is
        a1 + a2  # static: int | str, runtime: str

        # we definitely know that rhs is a subtype of lhs
        A() + B()  # static: str, runtime: str

        # we definitely know that rhs is not a subtype of lhs
        a1 + A()  # static: int, runtime: int
    ```

i think the second option is more powerful and expressive, and supports more usecases without overly widening types. and can also be enhanced when this insight can be implemented to specialise these cases

## this also applies to inplace operators
```py
class A:
    def __add__(self, other: A) -> int:
        return 1
class B(A):
    def __iadd__(self, other: A) -> str:   # expect error: `__iadd__` must be compatable with `A.__add__`, as `A` does not define `__iadd__`
        return "a"

def f(a1: A, a2: A):
    a = a1
    a += a2
    print(a)  # static: int, runtime: "a"
f(B(), A())
```
note that the righthand subtype rule applies __after__ `__iadd__`, so this case is much more straightforward to support

---

_Label `runtime semantics` added by @AlexWaygood on 2025-06-11 07:17_

---

_Comment by @carljm on 2025-09-09 16:51_

For reference, mypy and pyright both also give `int` here; mypy also gives the "unsafe operator definitions" diagnostic requested in this issue.

---

_Comment by @KotlinIsland on 2025-09-09 23:26_

mypy doesn't fully meet the request here:
```py
class A:
    def __add__(self, other: A) -> int:
        return 1
    def __radd__(self, other: A) -> str:
        return "a"
class B(A):
    def __radd__(self, other: A) -> str:  # error: Signatures of "__radd__" of "B" and "__add__" of "A" are unsafely overlapping
        return "a"
```

this can be perfectly valid, if the analysis of the operation is modified to account for it

---

_Renamed from "report unsafe operator type on subclasses" to "right hand operator used when inappropriate/ report unsafe operator type on subclasses" by @KotlinIsland on 2025-09-09 23:39_

---

_Renamed from "right hand operator used when inappropriate/ report unsafe operator type on subclasses" to "report unsafe operator type on subclasses" by @KotlinIsland on 2025-09-09 23:46_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 15:15_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
