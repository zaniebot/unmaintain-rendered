```yaml
number: 1212
title: infer type of parameter from super type
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2025-09-20T06:41:50Z
updated_at: 2025-09-22T20:12:48Z
url: https://github.com/astral-sh/ty/issues/1212
synced_at: 2026-01-10T02:06:25Z
```

# infer type of parameter from super type

---

_Issue opened by @KotlinIsland on 2025-09-20 06:41_

### Summary

```py
class A:
    def f(self, i: int): ...

class B(A):
    def f(self, i):
        reveal_type(i)  # Unknown
```

here i would expect `int`

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-09-22 12:36_

Why would you expect `int` here? I could write a Liskov-compliant subclass of `A` like this, since parameter types are contravariant:

```py
class A:
    def f(self, i: int): ...

class B(A):
    def f(self, i: object):
        reveal_type(i)
```

Without explicit annotations, we can't guess about what the user's intentions are here without breaking the gradual guarantee.

---

_Comment by @sharkdp on 2025-09-22 12:43_

This seems similar to https://github.com/astral-sh/ty/issues/1213, and I'd like to close it for the same reason.

---

_Closed by @sharkdp on 2025-09-22 12:43_

---

_Comment by @KotlinIsland on 2025-09-22 13:29_

> the gradual guarantee

i don't care about gradual typing, i fully type everything i write. if i wanted `object` i would annotate it as such. inferring `int` here is intuitive and ergonomic

---

_Comment by @carljm on 2025-09-22 20:11_

"Missing annotation means dynamic/gradual typing" and "missing annotation means best-guess type inference" are incompatible semantics for the Python type system. While Python type checkers have arguably been a bit fence-sitting between the two for attribute or variable annotations, there has never been any doubt for function arguments: the former interpretation was chosen ten years ago. If you want everything fully typed in Python, you must annotate all your function arguments.

---
