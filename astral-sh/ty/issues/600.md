```yaml
number: 600
title: "`FunctionType` is inherently unsafe"
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2025-06-07T08:27:34Z
updated_at: 2025-06-11T00:56:58Z
url: https://github.com/astral-sh/ty/issues/600
synced_at: 2026-01-10T02:34:10Z
```

# `FunctionType` is inherently unsafe

---

_Issue opened by @KotlinIsland on 2025-06-07 08:27_

### Summary

descriptors can be unsafe:
```py
class A: 
    pass

class B(A):
    def __get__(self, *_) -> 1:
        return 1

class C:
    a: A = B()

C.a  # static: A, runtime: 1
```
we can avoid this be reporting an error on the signature of `__get__`: "`__get__`s return type must be assignable to the super classes `__get__` (defaulting to the type of the superclass in it's absense)"

unfortunatly, `FunctionType` commits this sin, and it is an invalid subtype of `Callable`:

```py
class A:
    c: Callable[[object], None] = lambda self: None
A().c  # static: (object) -> None, runtime: () -> None
```

see: https://play.ty.dev/79255fa6-667e-4fa8-b6f2-32225f783bbd

# prior art
basedmypy resolved this problem fairly well by have special cased errors for these assignments to classes for `FunctionType` and `Callable`. the issue is that the corpus of existing code is written under the broken mypy semantics that all `Callable`s are actually `FunctionType`s


perhaps ideally we could just report that `FunctionType` is incompatible with `Callable`:

```py
_: Callable[[], None] = lambda: None  # error: FunctionType is incompatible with Callable, please try again in a few minutes
```


### Version

_No response_

---

_Renamed from "`FunctionType` is inherantly unsafe" to "`FunctionType` is inherently unsafe" by @KotlinIsland on 2025-06-07 13:34_

---

_Comment by @carljm on 2025-06-09 17:07_

Relevant discussion (focused primarily on the `__get__` unsoundness): https://discuss.python.org/t/when-should-we-assume-callable-types-are-method-descriptors/92938

---

_Comment by @carljm on 2025-06-10 16:11_

I am going to close this as a duplicate of #491 -- it is the same issue, I don't think this is independently actionable.

---

_Closed by @carljm on 2025-06-10 16:11_

---

_Comment by @KotlinIsland on 2025-06-11 00:56_

> I don't think this is independently actionable.

i'll raise the issue regarding descriptors in general

---
