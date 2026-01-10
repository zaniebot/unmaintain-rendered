```yaml
number: 245
title: handle sealed types
type: issue
state: closed
author: carljm
labels:
  - type properties
assignees: []
created_at: 2024-08-05T18:29:57Z
updated_at: 2025-05-11T07:16:26Z
url: https://github.com/astral-sh/ty/issues/245
synced_at: 2026-01-10T02:34:09Z
```

# handle sealed types

---

_Issue opened by @carljm on 2024-08-05 18:29_

Some types have a finite number of possible inhabitants. Examples include `builtins.bool` (the inhabitants are `Literal[True]` and `Literal[False]`) and enum types. For such types, we should consider the union of all possible inhabitants as equivalent to the type itself (so for example `Literal[True, False]`) is equivalent to `builtins.bool`). We should also take this equivalence into account when resolving negated intersections (so for example `builtins.bool & ~Literal[True]` is equivalent to `Literal[False]`; practically this will come up in type narrowing.)

Tasks here, in the order we'll likely get to them:

- [x] resolve `Literal[True] | Literal[False]` to `builtins.bool`
- [x] resolve `builtins.bool` back to `Literal[True] | Literal[False]` when intersection eliminates a type inhabitant
- [ ] extend the above handling to enum types, when we add them


---

_Comment by @AlexWaygood on 2024-08-05 18:37_

Other examples of sealed types are the various singletons available in `builtins` and the stdlib:
- `NotImplemented` (the only possible instance of `types.NotImplementedType`)
- `None` (the only possible instance of `types.NoneType`)
- `...` (also aliased as `Ellipsis`: the only possible instance of `types.EllipsisType`)
- `typing.NoDefault` (opaque; no public type exposed anywhere)

---

_Comment by @carljm on 2024-10-18 21:39_

Singleton types are a degenerate case of sealed types. They are sealed, but they don't require the handling discussed in this issue, since there is no equivalence to a union (except in the sense that all types are equivalent to a union of just that type, but we don't allow building size-one unions.)

---

_Renamed from "[red-knot] handle sealed types" to "handle sealed types" by @MichaReiser on 2025-05-07 15:27_

---

_Comment by @AlexWaygood on 2025-05-11 07:16_

I think the only non-singleton, non-enum sealed type that we have to worry about in practice is `bool`. There are [some rough edges there](https://github.com/astral-sh/ty/issues/216), but in general we support the sealedness of `bool` pretty well at this point. Specific issues with the sealedness of `bool` are probably better tackled in standalone issues; support for enums is covered by https://github.com/astral-sh/ty/issues/183.

Closing as completed.

---

_Closed by @AlexWaygood on 2025-05-11 07:16_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:16_

---
