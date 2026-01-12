```yaml
number: 1685
title: "Investigate `invalid-type-arguments` diagnostics on scipy-stubs"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - generics
assignees: []
created_at: 2025-11-29T22:25:30Z
updated_at: 2025-12-03T16:35:13Z
url: https://github.com/astral-sh/ty/issues/1685
synced_at: 2026-01-12T15:54:25Z
```

# Investigate `invalid-type-arguments` diagnostics on scipy-stubs

---

_@AlexWaygood_

> Currently, if I run `uvx ty check scipy-stubs` in the root of [scipy-stubs](https://github.com/scipy/scipy-stubs/) (just the current master branch), there are 674 diagnostics. At first glance, most of them appear to be false positives related to generic type aliases and type parameter bounds (`invalid-type-arguments`). 

 _Originally posted by @jorenham in [#394](https://github.com/astral-sh/ty/issues/394#issuecomment-3591963961)_

---

_Comment by @aidandj on 2025-11-30 05:00_

_unrelated_

---

_Comment by @dhruvmanila on 2025-12-01 08:15_

I looked at one example and I believe the reason is that the `type[T]` specialization doesn't account for this tuple hack that exists for class specialization:

https://github.com/astral-sh/ruff/blob/a6cbc138d273b86a70ed36e13623d079bd368236/crates/ty_python_semantic/src/types/infer/builder.rs#L10930-L10943

This means that `tuple[int, ...]` works but `type[tuple[int, ...]]` doesn't (https://play.ty.dev/5c58d89d-2254-4465-8a5e-76382c19583b):

```py
# No diagnostics here
x: tuple[int, ...]

# Too many type arguments to class `tuple`: expected 1, got 2 (invalid-type-arguments)
y: type[tuple[int, ...]]
```

cc @ibraheemdev 

---

_Comment by @AlexWaygood on 2025-12-01 08:19_

> I looked at one example and I believe the reason is that the `type[T]` specialization doesn't account for this tuple hack that exists for class specialization:

These might be fixed by https://github.com/astral-sh/ruff/pull/21652?

---

_Comment by @sharkdp on 2025-12-01 09:52_

I think https://github.com/astral-sh/ruff/pull/21652 fixes exactly one diagnostic on scipy-stubs. That made me think: why didn't we see these new diagnostics somewhere, given that scipy-stubs is part of mypy_primer? Turns out we missed this in `good.txt` for some reason. I'll open a PR to fix this => https://github.com/astral-sh/ruff/pull/21721

---

_Comment by @sharkdp on 2025-12-01 10:36_

Thank you for reporting this! I missed this regression in https://github.com/astral-sh/ruff/pull/21553 because we weren't running our ecosystem checks across `scipy-stubs`, which was just an oversight.

One of the problems that I identified here is summarized by this snippet. Pretend `unknown_module` is something that we didn't install as a dependency:
```py
from typing import TypeVar, TypeAlias

from unknown_module import UnknownClass  # type: ignore

T = TypeVar("T")

MyAlias: TypeAlias = UnknownClass[T] | None

a: MyAlias[int]
```
(https://play.ty.dev/b13da612-3348-4fba-9bc4-fc4d7bfe3cfc)

Since we don't know what `UnknownClass` is, we don't know that it's a generic class. We infer `UnknownClass` as `Unknown`, and then we currently fail to find the legacy typevar `T` in `MyAlias`, thinking it would be a non-generic alias.

---

_Label `bug` added by @sharkdp on 2025-12-01 10:36_

---

_Label `generics` added by @sharkdp on 2025-12-01 10:36_

---

_Assigned to @sharkdp by @sharkdp on 2025-12-01 10:36_

---

_Comment by @patrick91 on 2025-12-01 10:46_

I was trying to migrate our codebase to ty and I think I got a similar issue, repro:

```python
from typing import Literal
from collections.abc import AsyncGenerator, Callable


def decorator[Item](
    message: Item,
) -> Callable[[Callable[[], AsyncGenerator[Item]]], Callable[[], AsyncGenerator[Item]]]:
    def inner(fn: Callable[[], AsyncGenerator[Item]]) -> Callable[[], AsyncGenerator[Item]]:
        return fn
    return inner


@decorator(message="hello")
async def my_generator() -> AsyncGenerator[Literal["hello"]]:
    yield "hello"

# this one fails with:
# Argument is incorrect: Expected `() -> AsyncGenerator[Literal["hello"], None]`, found `def my_generator2() -> AsyncGenerator[str, None]` (invalid-argument-type) [Ln 18, Col 1]
@decorator(message="hello")
async def my_generator2() -> AsyncGenerator[str]:
    yield "world"
```

https://play.ty.dev/67bbea2d-a7d1-40a9-bce6-fa6787129244

---

_Comment by @sharkdp on 2025-12-01 10:47_

@patrick91 I don't think this is related. Can you please report this as a new issue?

---

_Comment by @AlexWaygood on 2025-12-01 12:36_

One issue here is that scipy-stubs seems to have several type variables that use generic type aliases as their upper bounds. This is pretty questionable, because you're not allowed to use a generic type as the upper bound of a type variable: it must be specialized. For example, mypy/pyright/pyrefly all complain about this:

```py
from typing import TypeVar

T = TypeVar("T")
U = TypeVar("U", bound=list[T])
```

but it seems like none of them complain about this -- I think they view the generic type alias as having been implicitly default-specialized when it's used as the upper bound of a type variable:

```py
from typing import TypeVar, TypeAlias

_NT = TypeVar("_NT")
_ND: TypeAlias = tuple[_NT, ...]
_ShapeT = TypeVar("_ShapeT", bound=_ND)
```

And I guess that's fair enough, because it seems correct to view `bound=list` as implicitly signifying `bound=list[Any]`, etc. -- viewing the type alias as having been implicitly default-specialized in the `TypeVar` bound is consistent with that principle. We should probably do similarly, if we don't already. If I make this change to my local clone of `scipy-stubs` (substituting generic type aliases in typevar upper bounds with the default-specialized versions of those type aliases), then the number of diagnostics we emit on scipy-stubs drops to 535:

```diff
diff --git a/scipy-stubs/stats/_distribution_infrastructure.pyi b/scipy-stubs/stats/_distribution_infrastructure.pyi
index 0339d96..bead332 100644
--- a/scipy-stubs/stats/_distribution_infrastructure.pyi
+++ b/scipy-stubs/stats/_distribution_infrastructure.pyi
@@ -41,9 +41,9 @@ _IntT_co = TypeVar("_IntT_co", bound=_Int, default=_Int, covariant=True)
 _RealT = TypeVar("_RealT", bound=_Float | _Int, default=_Float | _Int)
 _RealT_co = TypeVar("_RealT_co", bound=_Float | _Int, default=_Float | _Int, covariant=True)
 
-_ShapeT = TypeVar("_ShapeT", bound=_ND, default=_ND)
+_ShapeT = TypeVar("_ShapeT", bound=tuple[Any, ...], default=_ND)
 _ShapeT1 = TypeVar("_ShapeT1", bound=onp.AtLeast1D, default=onp.AtLeast0D[Any])
-_ShapeT_co = TypeVar("_ShapeT_co", bound=_ND, default=_ND, covariant=True)
+_ShapeT_co = TypeVar("_ShapeT_co", bound=tuple[Any, ...], default=_ND, covariant=True)
 
 _DistT0 = TypeVar("_DistT0", bound=_Dist[_0D])
 _DistT1 = TypeVar("_DistT1", bound=_Dist[_1D])
@@ -52,8 +52,8 @@ _DistT2 = TypeVar("_DistT2", bound=_Dist[_2D])
 _DistT_2 = TypeVar("_DistT_2", bound=_Dist[onp.AtMost2D])
 _DistT3 = TypeVar("_DistT3", bound=_Dist[_3D])
 _DistT_3 = TypeVar("_DistT_3", bound=_Dist[onp.AtMost3D])
-_DistT = TypeVar("_DistT", bound=_Dist)
-_DistT_co = TypeVar("_DistT_co", bound=_Dist, default=UnivariateDistribution, covariant=True)
+_DistT = TypeVar("_DistT", bound=_Dist[Any])
+_DistT_co = TypeVar("_DistT_co", bound=_Dist[Any], default=UnivariateDistribution, covariant=True)
 
 _AxesT = TypeVar("_AxesT", bound=_Axes, default=Any)
 
@@ -273,7 +273,7 @@ _Tuple2: TypeAlias = tuple[_T, _T]
 
 _XT = TypeVar("_XT", bound=npc.number, default=npc.number)
 _XT_co = TypeVar("_XT_co", bound=npc.number, default=np.float64, covariant=True)
-_ShapeT0_co = TypeVar("_ShapeT0_co", bound=_ND, default=_ND, covariant=True)
+_ShapeT0_co = TypeVar("_ShapeT0_co", bound=tuple[Any, ...], default=_ND, covariant=True)
 
 _BaseDist0: TypeAlias = _BaseDistribution[_XT, tuple[()]]
 _BaseDist1: TypeAlias = _BaseDistribution[_XT, tuple[int]]
diff --git a/scipy-stubs/stats/_new_distributions.pyi b/scipy-stubs/stats/_new_distributions.pyi
index c985ef3..79eeb3f 100644
--- a/scipy-stubs/stats/_new_distributions.pyi
+++ b/scipy-stubs/stats/_new_distributions.pyi
@@ -40,8 +40,8 @@ _FloatT_co = TypeVar("_FloatT_co", bound=_Float, default=_Float, covariant=True)
 _IntT = TypeVar("_IntT", bound=_Int, default=_Int)
 _IntT_co = TypeVar("_IntT_co", bound=_Int, default=_Int, covariant=True)
 
-_ShapeT = TypeVar("_ShapeT", bound=_ND, default=_ND)
-_ShapeT_co = TypeVar("_ShapeT_co", bound=_ND, default=_ND, covariant=True)
+_ShapeT = TypeVar("_ShapeT", bound=tuple[Any, ...], default=_ND)
+_ShapeT_co = TypeVar("_ShapeT_co", bound=tuple[Any, ...], default=_ND, covariant=True)
```

---

_Comment by @jorenham on 2025-12-01 13:28_

The type parameter of the generic `_ND` type has a default (`int` in both cases), so that's why I thought it was ok to use it as upper bound: 

https://github.com/scipy/scipy-stubs/blob/27ae8dccd60b0387477a9c1ba763fc79a532f9d1/scipy-stubs/stats/_distribution_infrastructure.pyi#L100-L105
https://github.com/scipy/scipy-stubs/blob/27ae8dccd60b0387477a9c1ba763fc79a532f9d1/scipy-stubs/stats/_new_distributions.pyi#L24-L29



---

_Comment by @AlexWaygood on 2025-12-01 13:33_

yes, to be clear, I do think there's a ty bug here :-)

We should be default-specializing `_ND` (turning it from `tuple[_NT, ...]` to `tuple[int, ...]`) when we see it appearing as the upper bound of a type variable, the same as mypy/pyright do.

---

_Comment by @jorenham on 2025-12-01 13:37_

I think I'll just change it either way and apply your patch, since it's somewhat sketchy (typing-spec-wise) and could be misinterpreted because of that.

---

_Added to milestone `Beta` by @carljm on 2025-12-01 21:18_

---

_Comment by @sharkdp on 2025-12-03 09:11_

Since this ticket was opened, we have merged:

* [#21652](https://github.com/astral-sh/ruff/pull/21652), which fixed 1 `invalid-type-arguments` diagnostic in `scipy-stubs` (see [here](https://cjm-typetuple.ecosystem-663.pages.dev/diff))
* [#21730](https://github.com/astral-sh/ruff/pull/21730), which fixed 235 `invalid-type-arguments` diagnostics in `scipy-stubs` (see [here](https://david-fix-specialized-unknow.ecosystem-663.pages.dev/diff))
* [#21765](https://github.com/astral-sh/ruff/pull/21765), which fixed 232 other type-alias related diagnostics in `scipy-stubs` (see [here](https://david-default-specialize-1.ecosystem-663.pages.dev/diff))

There are certainly still problems remaining (e.g. due to missing `TypeVarTuple` support, `ParamSpec` support, `Concatenate` support), but I think it would be best if we addressed those in dedicated tickets. I'm closing this for now, but feel free to open new issues or comment here if someone thinks this should be reopened.

---

_Closed by @sharkdp on 2025-12-03 09:11_

---

_Comment by @jorenham on 2025-12-03 09:13_

That's so awesome; thanks a lot all! 

---

_Comment by @jorenham on 2025-12-03 16:35_

It's 0 errors now 😄:
https://github.com/scipy/scipy-stubs/pull/1020

---
