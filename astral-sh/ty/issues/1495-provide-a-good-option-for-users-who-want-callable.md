---
number: 1495
title: "provide a good option for users who want callable types with all `FunctionType` attributes"
type: issue
state: open
author: carljm
labels:
  - callables
assignees: []
created_at: 2025-11-06T16:18:22Z
updated_at: 2025-12-23T00:21:58Z
url: https://github.com/astral-sh/ty/issues/1495
synced_at: 2026-01-10T01:52:52Z
---

# provide a good option for users who want callable types with all `FunctionType` attributes

---

_Issue opened by @carljm on 2025-11-06 16:18_

See https://github.com/astral-sh/ty/issues/491#issuecomment-3462267064 for more detailed discussion.

In particular, other type-checkers will assume all Callable types have `FunctionType.__get__`, while simultaneously assuming that they _don't_ necessarily always behave as a bound-method descriptor. This is of course inconsistent, but we may need to do the same for compatibility.

Similar for existence of other `FunctionType` attributes, such as `__name__`.

---

_Label `callables` added by @carljm on 2025-11-06 16:18_

---

_Comment by @MeGaGiGaGon on 2025-11-06 20:47_

My two cents on this is that I would like if there was some way for a warning on these possibly missing attributes to still be issued. I have ran into this several times with using `__name__` in functions that operate over `Callable`s, and eventually end up getting a very confusing error on what seems to be fully type safe code.

---

_Comment by @carljm on 2025-11-06 21:30_

Just a note as we consider this: intersecting with `FunctionType` is one way to avoid these errors, and `isinstance(typed_as_callable, FunctionType):` is one way to synthesize that intersection in ty, without making code incompatible with other type checkers that don't support an explicit spelling for intersections.

[playground demo](https://play.ty.dev/eadacf84-a529-48a2-90ec-468d69185f7d)

---

_Comment by @carljm on 2025-11-07 16:23_

I'm still not clear on whether we should do this. It seems plausible to instead say "what other type checkers are doing here is unsafe, if you want to access these attributes first use `isinstance(..., FunctionType)`? We do need to implement "call" on intersection types, though.

---

_Comment by @AlexWaygood on 2025-11-07 16:28_

> if you want to access these attributes first use `isinstance(..., FunctionType)`?

or narrow the type using a `hasattr` check, yeah

---

_Comment by @carljm on 2025-11-07 16:34_

The downside of isinstance `FunctionType` is that currently we'll infer a Todo type if you try to call the intersection, and once we fix that, I think a correct implementation of calling intersections will result in the call return type being an intersection of `Any` (from the annotated return type on `FunctionType.__call__`) with the return type from your callable type, which is probably not desirable in this case. We could maybe special-case this to treat the return type from `FunctionType.__call__` as `object` instead of `Any`? In a stricter world that's what `FunctionType.__call__` should return.

The downside of using `hasattr` checks is that we'll treat the attribute as having type `object`, instead of the annotated attribute type on `FunctionType`. This is technically correct, because there could be some other callable type with a `__name__` attribute of some other type.

---

_Comment by @AlexWaygood on 2025-11-07 16:48_

> We could maybe special-case this to treat the return type from `FunctionType.__call__` as `object` instead of `Any`? In a stricter world that's what `FunctionType.__call__` should return.

yes, I think we should apply this special case, definitely if it appears in an intersection with a `Callable` type and possibly more broadly

---

_Comment by @DetachHead on 2025-11-13 09:52_

I agree with @MeGaGiGaGon, I don't think all `Callable`s should be treated as `FunctionType` just because all the other type checkers wrongly do so. If ty does do this for the purpose of backwards compatibility, I think there should be an option to disable it. 

Alternatively, perhaps `ty_extensions` could export its own versions of `Callable` and `FunctionType` that work properly, so users can opt into them with an option to ban usage of the unsafe versions from the standard library 

---

_Added to milestone `Stable` by @carljm on 2025-11-13 16:52_

---

_Renamed from "assume (with other type checkers) that all Callable types have all typeshed attributes of `FunctionType`" to "provide a good option for users who want callable types with all `FunctionType` attributes" by @carljm on 2025-11-13 16:53_

---

_Comment by @DetachHead on 2025-12-18 03:19_

another issue i ran into is that there doesn't seem to be any way to use `assert_type` with a function type:

```py
def foo() -> None: ...


assert_type(foo, Callable[[], None]) # error
```

using a protocol or an intersection as suggested in https://github.com/astral-sh/ty/issues/599#issuecomment-2952175801 doesn't seem to work either:

```py
from collections.abc import Callable
from typing import assert_type, Protocol
from types import FunctionType
from ty_extensions import Intersection

class FunctionLike[**P, R](Protocol):
    def __call__(self, *args: P.args, **kwargs: P.kwargs) -> R: ...

    __name__: str

def foo() -> None: ...

assert_type(foo, Callable[[], None]) # error
assert_type(foo, FunctionType[[], None]) # error
assert_type(foo, Intersection[FunctionType, Callable[[], None]]) # error
```
https://play.ty.dev/5dcafceb-e48b-4380-a386-60c1a5d5ea58

---

_Comment by @carljm on 2025-12-23 00:20_

@DetachHead That's more a https://github.com/astral-sh/ty/issues/1762 issue -- the problem is just that we represent some types internally that are more precise than what can be spelled as a type annotation, and that causes difficulty using those types with `assert_type`, because we are currently strict about `assert_type` asserting type equivalence.

Using a protocol or intersection doesn't help as those aren't any more precise than a `Callable` type, they still aren't the same "function literal" type that we track internally.

---
