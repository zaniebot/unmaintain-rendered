```yaml
number: 1217
title: generic class with single constructor input can be inferred as a distributed union
type: issue
state: closed
author: KotlinIsland
labels:
  - generics
assignees: []
created_at: 2025-09-20T07:16:03Z
updated_at: 2025-10-28T03:16:33Z
url: https://github.com/astral-sh/ty/issues/1217
synced_at: 2026-01-12T15:54:24Z
```

# generic class with single constructor input can be inferred as a distributed union

---

_@KotlinIsland_

### Summary

```py
class A[T]:
    def __init__(self, value: T):
        self._value = value

def f(value: int | str):
    _: A[int] | A[str] = A(value)  # Object of type `A[int | str]` is not assignable to `A[int] | A[str]`
```

the value here could be safely inferred as `A[int] | A[str]`, as there is only one input value

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-09-20 11:59_

---

_Comment by @sharkdp on 2025-09-22 09:18_

Thank you for reporting this.

> the value here could be safely inferred as `A[int] | A[str]`, as there is only one input value

It seems to me like there are additional requirements that need to be fulfilled in order for that property to hold (not just the "one input value" constraint)? It might work for `int` and `str`, as there is an instance lay-out conflict, but for general classes, I don't think it's true that `A(value)` with `value: X | Y` should be assignable to `A[X] | A[Y]`, because there could be subclasses that derive from `X` and `Y`, and `value` might be an instance of those subclasses.

It doesn't seem like any other type checker allows your snippet to pass without errors. Do you have a concrete real-world use case where you see this problem?

---

_Comment by @KotlinIsland on 2025-09-22 23:31_

i did run into this at one point, but i can't remember the specifics, i think it was creating a list

in the case of covariant types `A[int] | A[str]` is a subtype of `A[int | str]`. so we would be inferring the more specific type

---

_Comment by @KotlinIsland on 2025-10-28 03:16_

> It seems to me like there are additional requirements that need to be fulfilled in order for that property to hold

yeah, it's more complicated than i initially thought, and now i think not worth it

---

_Closed by @KotlinIsland on 2025-10-28 03:16_

---
