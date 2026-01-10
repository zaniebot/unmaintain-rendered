```yaml
number: 2280
title: "Avoid literal promotion when constructing `frozenset` from list/set literal"
type: issue
state: open
author: Jammf
labels:
  - bidirectional inference
assignees: []
created_at: 2025-12-30T19:34:28Z
updated_at: 2026-01-06T02:11:34Z
url: https://github.com/astral-sh/ty/issues/2280
synced_at: 2026-01-10T01:56:41Z
```

# Avoid literal promotion when constructing `frozenset` from list/set literal

---

_Issue opened by @Jammf on 2025-12-30 19:34_

### Summary

Literal promotion is currently performed for `tuple` but not for `frozenset`. This is surprising to me given that they both are immutable and behave somewhat similarly. It could be useful to change `frozenset`, for instance to support the following comparisons:

```python
from typing import reveal_type

# tuple
reveal_type((1, 2))  # Revealed type: `tuple[Literal[1], Literal[2]]`
reveal_type((1, 2) == (1, 2))  # Revealed type: `Literal[True]`

# frozenset
reveal_type(frozenset({1, 2}))  # Revealed type: `frozenset[Unknown | int]`
reveal_type(frozenset({1, 2}) == frozenset({1, 2}))  # Revealed type: `bool`
reveal_type(frozenset({1, 2}) == frozenset({2, 1}))  # Revealed type: `bool`
```

https://play.ty.dev/464c2ef8-e7bc-4e74-938f-3c0f2cd20916

Related: https://github.com/astral-sh/ty/issues/1284#issuecomment-3403747307

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Comment by @carljm on 2025-12-30 21:15_

To clarify terminology: "literal promotion" refers to promoting a type like `Literal[1]` or `Literal[1, 2]` to the wider type `int`. So the examples above show that we do not promote literals in tuple types, but we seemingly do in frozenset.

Why do I say "seemingly"? We actually don't promote literals in the frozenset constructor, because frozenset is covariant. In the examples above, we promote the literals before they ever reach frozenset, when we infer a type for the set literal `{1, 2}` that is passed to the frozenset constructor. We can see the difference in this example:

```py
from typing import Literal

l: set[Literal[1, 2]] = {1, 2}
x = frozenset(l)  # x is now `frozenset[Literal[1, 2]]`
```

So the feature request here is to pass through the type context from the frozenset constructor so that we avoid literal promotion on the inner container, if it's a list/set literal.

This is something that no other type checker does, so I think it's low priority, but I don't see any reason we couldn't do it.

---

_Renamed from "Do not promote literals in `frozenset`" to "Avoid literal promotion when constructing `frozenset` from list/set literal" by @carljm on 2025-12-30 21:16_

---

_Label `bidirectional inference` added by @carljm on 2025-12-30 21:16_

---

_Comment by @Jammf on 2025-12-30 23:45_

Thanks for the clarification. I tried to borrow the terminology from the related issue, but I'm pretty new to it all, so it turned out less precise than I'd hoped. Your description makes sense to me though, that the promotion happens in the set literal, and that was I was actually asking for was to avoid that promotion if the set literal was within a frozenset.

If I'm understanding correctly, it seems like the (implied) other part of the feature request (comparisons between `frozenset` types) might not be feasible.

```python
from typing import Literal, reveal_type

l: set[Literal[1, 2]] = {1, 2}
a: frozenset[Literal[1, 2]] = frozenset(l)
b: frozenset[Literal[1, 2]] = frozenset(l)
reveal_type(a == b)  # Revealed type: `bool`
```

which I think boils down to there not being a way to specify the exact members of a set/frozenset within a type, since `set[Literal[1, 2]]` could mean any of `{1}`,  `{2}`, or `{1, 2}`. I also tried explicitly casting to an Intersection that should mean just `{1, 2}`, which also didn't work:

```python
from typing import Literal, reveal_type, cast
from ty_extensions import Intersection, Not

l: set[Literal[1, 2]] = {1, 2}
a: frozenset(l)
b: frozenset(l)

a = cast("Intersection[frozenset[Literal[1, 2]], Not[frozenset[Literal[1]]], Not[frozenset[Literal[2]]]]", a)
b = cast("Intersection[frozenset[Literal[1, 2]], Not[frozenset[Literal[1]]], Not[frozenset[Literal[2]]]]", b)
reveal_type(a == b)  # Revealed type: `bool`
```

But perhaps a more elegant solution would instead be a change to the typing spec, to allow forms like `frozenset[int, ...]`, `frozenset[Literal[1], Literal[2]]`, so it'd be like an unordered version of `tuple`.

---

_Comment by @carljm on 2026-01-06 02:11_

Yes, I think your analysis is correct that even if we did infer the frozenset type without literal promotion, it still wouldn't enable the equality behavior you're looking for, without major changes to special-case `frozenset` in the type system (which I think is unlikely -- it's not likely something we'd implement in ty without a change to the typing spec mandating it.)

I think your intersection version _could_ be implemented, but it would add a lot of complexity, just to give a more _precise_ result in a case where we already give a _correct_ result. So I'm also skeptical that we'd prioritize this.

I think we should limit this issue to considering the inference and literal promotion question, and not the equality behavior. That could be a separate issue, but I'm not sure I'd bother with that.

---
