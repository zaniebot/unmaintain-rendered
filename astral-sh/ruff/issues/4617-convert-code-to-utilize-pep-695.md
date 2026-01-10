---
number: 4617
title: (🎁) Convert code to utilize pep 695
type: issue
state: closed
author: KotlinIsland
labels:
  - rule
  - help wanted
  - accepted
  - python312
assignees: []
created_at: 2023-05-24T07:07:36Z
updated_at: 2025-01-22T16:40:06Z
url: https://github.com/astral-sh/ruff/issues/4617
synced_at: 2026-01-10T01:22:43Z
---

# (🎁) Convert code to utilize pep 695

---

_Issue opened by @KotlinIsland on 2023-05-24 07:07_

# input:
```py
from typing import TypeVar, TypeAlias, Generic

T = TypeVar("T", bound=float)
Foo: TypeAlias = str | T | None

class A(Generic[T]):
    ...

def f(t: T):
    ...
```

# output:
```py
type Foo[T: float] = str | T | None

class A[T: float]:
    ...

def f[T: float](t: T):
    ...
```


And also `ParamSpec` and `TypeVarTuple`.

---

_Renamed from "(🎁) Convert code to utalize pep 695" to "(🎁) Convert code to utilize pep 695" by @KotlinIsland on 2023-05-24 07:10_

---

_Comment by @charliermarsh on 2023-05-24 13:01_

Yes, I'd love to support this. (It will take some time, need to start by supporting it in the parser and the semantic model.)

---

_Label `rule` added by @charliermarsh on 2023-05-24 13:01_

---

_Referenced in [astral-sh/ruff#6289](../../astral-sh/ruff/pulls/6289.md) on 2023-08-02 23:46_

---

_Label `accepted` added by @zanieb on 2023-08-02 23:47_

---

_Label `help wanted` added by @zanieb on 2023-08-02 23:47_

---

_Comment by @zanieb on 2023-08-02 23:48_

This is ready to be worked on! I've started in #6289 but there is much more to do :)

---

_Referenced in [astral-sh/ruff#6314](../../astral-sh/ruff/pulls/6314.md) on 2023-08-03 18:36_

---

_Comment by @nathanwhit on 2023-08-21 22:34_

Hello! I'm interested in working on this, if that's okay!

I think I have enough to go on here to round out support for generic type variables, the only snag is with handling type variables that have explicit variance. If a type var has explicit variance, I don't see a way to translate it to the PEP-695 style and _guarantee_ identical semantics. The issue is that the new syntax for generics doesn't have a facility to explicitly set the variance of a type variable, the variance is always inferred.

If we're adhering strictly to the spec (from my reading, at least), only `TypeVar` definitions with `infer_variance=True` should have inferred variance and are therefore definitely safe to upgrade to the new syntax. The trouble here is that I would guess that many `TypeVar` declarations in practice will not have that flag set, which might limit the usefulness of this rule.

If we're being a bit less strict, we could just assume that the variance inference will _usually_ produce the same result, but I'm not sure how acceptable it is to potentially change the meaning of code.

---

_Comment by @zanieb on 2023-08-21 22:39_

Hey @nathanwhit excited you're interested in working on this :)

We can use different applicability levels (#4183) to indicate our certainty that the fix will or will not change the meaning of the code. In this case, we could use an `Automatic` fix when `infer_variance=True` and a `Suggested` fix otherwise. 

I've never seen someone set `infer_variance=True` before though 😬 maybe we can use a heuristic to infer if the variance would be the same?


---

_Comment by @nathanwhit on 2023-08-22 00:11_

Thanks for such a quick response @zanieb!

> We can use different applicability levels (https://github.com/astral-sh/ruff/issues/4183) to indicate our certainty that the fix will or will not change the meaning of the code. In this case, we could use an Automatic fix when infer_variance=True and a Suggested fix otherwise.

Makes sense!

> I've never seen someone set infer_variance=True before though 😬 maybe we can use a heuristic to infer if the variance would be the same?

Now that I'm looking, `infer_variance` is new in PEP-695, so unless someone has partially upgraded for 695 they wouldn't have it set🙃. Yeah, a heuristic would be good (though I can't think of one off the top of my head 😬) since doing it properly is probably out of the question (since it requires a full typechecker).

--

A little awkward, but I dug deeper and I think we don't have to worry about variance for type aliases after all! It turns out that the variance of a type variable is actually ignored for type aliases, all that matters is the variance of the type variable used in the generic class. (This makes sense now that I'm thinking about it, since variance is a [property of a generic type](https://peps.python.org/pep-0484/#covariance-and-contravariance) and generic type aliases aren't actual generic types, just new names for an underlying generic type).

For example:
```python
from typing import Generic, TypeVar

T = TypeVar("T", covariant=True)

class Covariant(Generic[T]):
    pass

S = TypeVar("S", contravariant=True)
Co = Covariant[S]

class MyInt(int):
    pass

foo: Covariant[MyInt] = Covariant[MyInt]()
bar: Covariant[int] = foo
```
This typechecks despite the fact that `S` (used by the `Co` type alias) is defined as contravariant, because all that matters is the variance of `T` (the var used in the actual generic class).

So I think we can safely ignore the variance of a type var used in a type alias. Variance will be relevant again when we support upgrading classes to the new syntax (`class List(Generic[T]):` to `class List[T]`) though.

---

_Referenced in [astral-sh/ruff#6749](../../astral-sh/ruff/pulls/6749.md) on 2023-08-22 00:40_

---

_Label `python312` added by @zanieb on 2023-08-24 20:02_

---

_Referenced in [astral-sh/ruff#10628](../../astral-sh/ruff/issues/10628.md) on 2024-03-28 12:24_

---

_Referenced in [astral-sh/ruff#12542](../../astral-sh/ruff/issues/12542.md) on 2024-07-27 09:04_

---

_Comment by @Goldziher on 2024-07-28 11:45_

I wrote this in https://github.com/astral-sh/ruff/issues/12542

MyPy released support for PEP 695 generics: python/mypy#15238

It would be awesome to do any of the following:

1. get linting errors for code that uses older style generics (TypeVar, ParamSpec etc.) for Python 3.12+

2. get linting errors for code that uses newer style generics and should be backward compatible with lower Python versions

3. automagically upgrade older style generics to use the new syntax





---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-10 14:11_

---

_Assigned to @ntBre by @ntBre on 2025-01-16 17:47_

---

_Unassigned @AlexWaygood by @ntBre on 2025-01-16 17:48_

---

_Referenced in [astral-sh/ruff#15565](../../astral-sh/ruff/pulls/15565.md) on 2025-01-18 04:17_

---

_Referenced in [astral-sh/ruff#15642](../../astral-sh/ruff/issues/15642.md) on 2025-01-21 13:58_

---

_Closed by @ntBre on 2025-01-22 16:35_

---

_Closed by @ntBre on 2025-01-22 16:35_

---

_Comment by @ntBre on 2025-01-22 16:40_

These examples should now be handled by the rules UP046 and UP047 added in #15565, with a few caveats being tracked in #15642.

---
