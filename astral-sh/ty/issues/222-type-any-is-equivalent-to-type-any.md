```yaml
number: 222
title: "`type[Any]` is equivalent to `type & Any`"
type: issue
state: open
author: AlexWaygood
labels:
  - type properties
  - set-theoretic types
assignees: []
created_at: 2025-01-09T18:29:51Z
updated_at: 2025-11-18T16:10:22Z
url: https://github.com/astral-sh/ty/issues/222
synced_at: 2026-01-12T15:54:22Z
```

# `type[Any]` is equivalent to `type & Any`

---

_@AlexWaygood_

The type `type[Any]` is exactly equivalent to `type & Any` (or `type[object] & Any`). Conceptually, these are all equivalent spellings of "a type of unknown size and bounds, but which is known to be no larger than the type `type`[^1]". By the same token, `type[Unknown]` is equivalent to `type & Unknown`; `type[@Todo("do the thing")]` is equivalent to `type & @Todo("do the thing")`.

We currently represent `type[Any]` and `type & Any` using different internal representations in red-knot. We should fix this: equivalent types should have the same internal representation in our model. This helps avoid unnecessary overhead, and also reduces the internal complexity in our model.

Of the two representations, the `type & Any` representation is the better one for us internally, because it's a more general way of representing the type and will allow us to get rid of more complexity in our internal model. However, when _displaying_ the type to users (for example, in the output of `reveal_type`), we should continue to use the notation `type[Any]`, or we'll end up with highly confusing outcomes like this:

```py
from typing import Any

def f(x: type[Any]):
    reveal_type(x)  # type & Any. User is now very confused.
```

Implementing this will require some special-casing in `Type::display()`. We'll want to make sure that the special-casing is also able to understand that `type & Any & T` should be represented as `type[Any] & T` when displayed to the user.

[^1]: The type `type`, of course, describes "All possible instances of the class `type`". It is exactly equivalent to the type `type[object]`, which describes "All possible subclasses of the class `object`".

---

_Comment by @carljm on 2025-01-09 18:39_

One nit: I think we need to consider `Any` and `Unknown` and `Todo` as all (gradually) equivalent to each other, and thus `type & Any` and `type & Unknown` and `type & Todo` as also equivalent. The distinction between `Any` vs `Unknown` vs `Todo` is entirely about provenance/debugging, there is no type level distinction. (And I've been wondering if the `Any` vs `Unknown` distinction is really worth it.)

I realize this violates the aim of having exactly one representation for a given type, but I don't think that's something we'll be able to achieve 100%.

---

_Comment by @AlexWaygood on 2025-01-09 18:42_

> (And I've been wondering if the `Any` vs `Unknown` distinction is really worth it.)

Ah, I still think this will pay off in the long run, but that's getting off-topic

> I realize this violates the aim of having exactly one representation for a given type, but I don't think that's something we'll be able to achieve 100%.

Agreed. We should do it when it's possible, but it won't always be possible, and that's okay

---

_Comment by @AlexWaygood on 2025-01-11 13:20_

Implementing this simplification might be blocked on improving our support for intersections across red-knot. Our support for `type[Any]` is actually pretty good now (some TODOs remain, but not so many). We still have quite a few TODOs for intersections, though, e.g.

https://github.com/astral-sh/ruff/blob/c39ca8fe6d3fb77684d2919a58b77cca95671ab7/crates/red_knot_python_semantic/src/types.rs#L2353-L2358

so if we implemented the simplification right now, I'm guessing quite a few of our tests for `type[Any]` and `type[Unknown]` would start failing. It would be best if we didn't regress functionality like that.

---

_Renamed from "[red-knot] `type[Any]` is equivalent to `type & Any`" to "`type[Any]` is equivalent to `type & Any`" by @MichaReiser on 2025-05-07 15:27_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:20_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-11 07:20_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:01_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
