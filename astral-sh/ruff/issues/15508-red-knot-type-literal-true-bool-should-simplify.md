```yaml
number: 15508
title: "[red-knot] `type & ~Literal[True] & bool` should simplify to `Never`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
created_at: 2025-01-15T16:34:39Z
updated_at: 2025-01-16T23:48:54Z
url: https://github.com/astral-sh/ruff/issues/15508
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] `type & ~Literal[True] & bool` should simplify to `Never`

---

_@AlexWaygood_

...but we currently don't do that.

We currently do the following simplifications:

1. `type & bool & ~Literal[True]` -> `Never`
2. `bool & type & ~Literal[True]` -> `Never`
3. `bool & ~Literal[True] & type` -> `Never`
4. `~Literal[True] & bool & type` -> `Never`
5. `type & ~Literal[True] & bool` -> `type & bool` ‼️
6. `~Literal[True] & type & bool` -> `type & bool` ‼️

After much puzzling, I figured out that this is what currently happens for each intersection:
1. `type & bool & ~Literal[True]` -> `(type & bool) & ~Literal[True]` -> `type & Literal[False]` -> `Never`
2. `bool & type & ~Literal[True]` -> `(bool & type) & ~Literal[True]` -> `type & Literal[False]` -> `Never`
3. `bool & ~Literal[True] & type` -> `(bool & ~Literal[True]) & type` -> `Literal[False] & type` -> `Never`
4. `~Literal[True] & bool & type` -> `(~Literal[True] & bool) & type` -> `Literal[False] & type` -> `Never`
5. `type & ~Literal[True] & bool` -> `(type & ~Literal[True]) & bool` -> `type & bool`
6. `~Literal[True] & type & bool` -> `(~Literal[True] & type) & bool` -> `type & bool`

All the simplifications we're doing look correct to me here (simplifying `type & ~Literal[True]` to `type` is accurate given that `type` and `Literal[True]` are disjoint types). It's just that we're missing some additional simplifications that turn out not just be desirable, but actually _necessary for correctness_ given the simplifications we already have in place. Namely, `type & bool` _must_ be recognised as disjoint types, given our simplifications of `bool & ~Literal[True]` -> `Literal[False]` and given that we recognise `type` and `Literal[True]` as being disjoint.

We'll naturally recognise `bool` and `type` as being disjoint once we flesh out our support for `@final` instance types, so there's nothing to do here that we weren't going to do anyway. But it's quite interesting!

---

_Label `red-knot` added by @AlexWaygood on 2025-01-15 16:34_

---

_Label `bug` added by @AlexWaygood on 2025-01-15 16:37_

---

_Comment by @carljm on 2025-01-15 22:46_

Good writeup! This is a consequence of the fact that in some places we have (effectively) hardcoded recognition of `bool` as final, via the fact that it is a union of `Literal[False]` and `Literal[True]`, but we don't yet consistently recognize the implications of its finality. So as you say, once we do, this should fall out correctly.

---

_Closed by @AlexWaygood on 2025-01-16 23:48_

---

_Closed by @AlexWaygood on 2025-01-16 23:48_

---
