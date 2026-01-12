```yaml
number: 95
title: typevar identity should not matter for generic function subtyping/assignability
type: issue
state: closed
author: carljm
labels:
  - bug
  - generics
  - type properties
assignees: []
created_at: 2025-05-06T22:03:37Z
updated_at: 2025-12-12T23:48:57Z
url: https://github.com/astral-sh/ty/issues/95
synced_at: 2026-01-12T15:54:22Z
```

# typevar identity should not matter for generic function subtyping/assignability

---

_@carljm_

When we do subtype/assignability checks within a generic scope, we don't consider two different typevars to be equivalent, even if they have the same bounds/constraints, because they could specialize to two different types.

But when we compare two unspecialized generic signatures for assignability/subtyping, this consideration does not apply. Two identical signatures, except that they use a different typevar (with the same bounds/constraints), are the same signature; typevar identity is irrelevant. In [this example](https://play.ty.dev/2654dd86-46c1-4a34-9056-e017e560ab30), all six assertions should pass; currently only the first and third and fifth pass:

```py
from typing import TypeVar, Callable

from ty_extensions import is_assignable_to, is_subtype_of, static_assert

T = TypeVar("T")
U = TypeVar("U")

static_assert(is_assignable_to(Callable[[T], T], Callable[[T], T]))
static_assert(is_assignable_to(Callable[[T], T], Callable[[U], U]))

static_assert(is_subtype_of(Callable[[T], T], Callable[[T], T]))
static_assert(is_subtype_of(Callable[[T], T], Callable[[U], U]))

static_assert(is_equivalent_to(Callable[[T], T], Callable[[T], T]))
static_assert(is_equivalent_to(Callable[[T], T], Callable[[U], U]))
```

---

_Label `bug` added by @carljm on 2025-05-06 22:03_

---

_Comment by @carljm on 2025-05-06 22:05_

cc @dcreager 

---

_Comment by @carljm on 2025-05-06 22:08_

I _think_ that generic callable signatures are the only place where an unspecialized typevar can appear in a context where we need to do subtyping/assignability checks on it based on, effectively, a form of "gradual equivalence" (i.e. if these two typevars can specialize to the same set of types, they are equivalent) rather the than strict equivalence we need to use within a generic scope (if these two typevars can possibly specialize to different types, they are not equivalent). So I think this can probably be handled within `Signature`? Though it may be cleanest by using a strategy parameter to `Type` equivalency/subtyping/assignability methods, not sure.

---

_Label `bug` added by @MichaReiser on 2025-05-07 15:17_

---

_Renamed from "[ty] typevar identity should not matter for generic function subtyping/assignability" to "typevar identity should not matter for generic function subtyping/assignability" by @MichaReiser on 2025-05-07 15:24_

---

_Label `generics` added by @carljm on 2025-05-08 17:01_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 11:02_

---

_Comment by @LaBatata101 on 2025-05-30 23:00_

I'm working on this

---

_Comment by @dcreager on 2025-05-31 01:05_

> Though it may be cleanest by using a strategy parameter to `Type` equivalency/subtyping/assignability methods, not sure.

I don't think this will be enough. In your

```py
is_assignable_to(Callable[[T], T], Callable[[U], U])
```

example, we have to make sure not just that `T` and `U` have compatible bounds/constraints, but that `T` is "lined up" with `U` everywhere that it occurs.

I think we can leverage our naive solver for this. Instantiate a `SpecializationBuilder`, call `infer` on the lhs and rhs types, and see if the specialization that you get maps each lhs typevar to an rhs typevar, and that every lhs typevar has a specialization.

---

_Comment by @dcreager on 2025-05-31 01:07_

> I think we can leverage our naive solver for this. Instantiate a `SpecializationBuilder`, call `infer` on the lhs and rhs types, and see if the specialization that you get maps each lhs typevar to an rhs typevar, and that every lhs typevar has a specialization.

Or maybe, infer the specialization, _apply_ it to the lhs type, and then compare them for assignability etc as we're already doing, since then lhs and rhs would be using the same typevars.

(And that makes me think you're right that this can be handled in `Signature`)

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:44_

---

_Assigned to @LaBatata101 by @carljm on 2025-08-05 14:53_

---

_Unassigned @LaBatata101 by @carljm on 2025-08-15 14:43_

---

_Assigned to @dcreager by @carljm on 2025-08-15 15:47_

---

_Comment by @sharkdp on 2025-09-22 08:18_

This example seems related/the same, but could probably be added as a "use case" test that doesn't involve `static_assert`/`is_assignable_to`:

```py
from typing import Callable

def invoke[A, B](fn: Callable[[A], B], value: A) -> B:
    return fn(value)

def identity[T](x: T) -> T:
    return x

invoke(identity, 1)
```

---

_Comment by @carljm on 2025-12-05 00:38_

It looks like https://github.com/astral-sh/ruff/pull/21551 alone won't solve this; bumping this out post-beta in that case, since it's not as high-priority as e.g. solving #1314 

---

_Removed from milestone `Beta` by @carljm on 2025-12-05 00:38_

---

_Added to milestone `Stable` by @carljm on 2025-12-05 00:38_

---

_Comment by @carljm on 2025-12-12 23:42_

I confused myself here with an over-simplified repro that also depends on #1136. The actual issue that I was intending to describe here is already fixed in main, I suspect by the `inferable_typevars` work a while back. Here's a playground to demonstrate: https://play.ty.dev/2c664b0c-cc40-41d0-b695-7778b65e6295

There is one remaining issue I see with equivalence, but I think it'll be clearer to open a new issue for that.

---

_Closed by @carljm on 2025-12-12 23:42_

---

_Comment by @carljm on 2025-12-12 23:48_

Filed #1872 for the remaining equivalence issue.

---
