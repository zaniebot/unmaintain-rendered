```yaml
number: 215
title: "`tuple & AlwaysFalsy` should simplify to `tuple[()]`"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - type properties
  - set-theoretic types
assignees: []
created_at: 2025-01-16T10:28:50Z
updated_at: 2025-08-22T13:11:42Z
url: https://github.com/astral-sh/ty/issues/215
synced_at: 2026-01-10T02:06:24Z
```

# `tuple & AlwaysFalsy` should simplify to `tuple[()]`

---

_Issue opened by @AlexWaygood on 2025-01-16 10:28_

Here's what these types represent:
- `tuple`: the set of all possible runtime instances of the class `tuple` (including instances of tuple subclasses)
- `AlwaysFalsy`: the set of all possible runtime objects whose truthiness can be statically determined to always be false
- `tuple[()]`: the set of all possible zero-length tuples (including zero-length instances of tuple subclasses)
- `tuple & AlwaysFalsy`: the set of all possible runtime instances of the class `tuple` (including instances of tuple subclasses) whose truthiness can be statically determined to always be false

Tuple instances (and instances of any tuple subclass, assuming the subclass conforms to the Liskov principle) are only ever falsy if they are 0-length, so the set of possible objects represented by `tuple & AlwaysFalsy` is exactly the same as the set of possible objects represented by `tuple[()]`. ~~By the same token, `tuple & ~AlwaysTruthy` should also simplify to `tuple[()]`~~.

The same can _not_ be said for things like `bytes & AlwaysFalsy` (this does _not_ simplify to `Literal[b""]`), because `bytes` can be subclassed, and instances of `bytes` subclasses can also be falsy if they are empty bytestrings, but for a type to inhabit a literal `bytes` type its `__class__` must be exactly `bytes`, not a subclass of `bytes`. Heterogeneous tuple types are different from `Literal` types in this way, in that they can be inhabited by subclasses of `tuple` as well as literal tuples.

We should adapt the logic in `InnerIntersectionBuilder` to eagerly perform this simplification when we see the intersection `tuple & AlwaysFalsy`.

---

_Comment by @AlexWaygood on 2025-01-17 13:15_

> ~By the same token, `tuple & ~AlwaysTruthy` should also simplify to `tuple[()]`~.

I initially wrote this, but it's incorrect. There are many possible tuple objects with ambiguous truthiness; the `tuple` type is bigger than the union of all _heterogeneous tuple_ types. So `tuple & ~AlwaysTruthy` is not an equivalent type to `tuple & AlwaysFalsy`; it's actually equivalent to something like `(tuple & AlwaysFalsy) | (tuple & NeitherAlwaysFalsyNorAlwaysTruthy)`.

---

_Comment by @carljm on 2025-01-17 22:23_

> Tuple instances (and instances of any tuple subclass, assuming the subclass conforms to the Liskov principle) are only ever falsy if they are 0-length

Is this accurate? How does Liskov guarantee this? The `tuple.__bool__` method can only be known to return `bool` (do we have enough expressiveness in the type system to define an overload for `__bool__` that says it must return `Literal[False]` if its type parameters are empty? typeshed doesn't currently do that, in any case), so I'm not sure Liskov can tell us anything about what the `__bool__` method of a subclass of `tuple` returns, or what relationship that has to its element types.

> Heterogeneous tuple types are different from `Literal` types in this way, in that they can be inhabited by subclasses of `tuple` as well as literal tuples.

If this and the above are both true, then I'm not sure that we can actually say that the type `tuple[()]` even inhabits `AlwaysFalsy`, because it can be inhabited by an instance of some `tuple` subclass whose `__bool__` always returns `true`, even when empty.

---

_Comment by @carljm on 2025-01-17 22:41_

As an illustration, here's [pyright getting this wrong](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQFD0AmhwUwAFDAFywCuCCoQDaHAJQBdMd3pQ5mNj1nyVRGHxAooAFRB8Gqwus1QAYpQDODegGMKZCxagBZONoFDhAKm0SuHkR8pGRUWNioqACMwMGoqDisKYDEoAFoAPiho2JCVeTUNLV19RkJseF5Xd0FA9RrRSQkoAF4XNwCOcTFGIlJKGkRCDk4yhHgxMSA).

It allows the type `MyTuple[*tuple[()]]` to be assigned to the type `tuple[()]` (correctly, according to "the tuple type is not a literal type, it includes subclasses"), and it also allows `MyTuple` to define a `__bool__` that always returns `True` (also correctly, per typeshed's definition of the `tuple` type), and then it considers an empty tuple to always be falsy (intuitive, and true if we don't consider subclasses, but wrong because of the combination of the prior two), and thus ends up inferring `Literal[False]` as the return type when at runtime this code returns `True`.

It's a bit sad if the conclusion here is that we can't safely consider the empty tuple type a subtype of `AlwaysFalsy`. But I think that is the only correct conclusion, assuming our heterogenous-tuple type is what we infer from a `tuple[...]` annotation.

We could of course _also_ implement a `LiteralTuple` type, for which we could consider the empty literal-tuple `AlwaysFalsy` (and all non-empty ones `AlwaysTruthy`). But I'm not sure how useful such a type would be, since we could only infer it for actual tuple literals. 

Or we could have a special-case rule forbidding subclasses of `tuple` from overriding `__bool__` or `__len__`, but that's pretty weird.

---

_Renamed from "[red-knot] `tuple & AlwaysFalsy` should simplify to `tuple[()]`" to "`tuple & AlwaysFalsy` should simplify to `tuple[()]`" by @MichaReiser on 2025-05-07 15:27_

---

_Label `type properties` added by @AlexWaygood on 2025-05-11 07:24_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-11 07:24_

---

_Comment by @sharkdp on 2025-05-23 09:43_

Whoever decides to work on this eventually, please update the following sections in our tests (in case tests would not fail anyway):
- https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md#truthiness
- https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/type_compendium/always_truthy_falsy.md#open-questions

---

_Comment by @carljm on 2025-05-28 18:22_

> Or we could have a special-case rule forbidding subclasses of `tuple` from overriding `__bool__` or `__len__`, but that's pretty weird.

Our latest thinking here is that this may actually be the best option, pragmatically.

---

_Label `bug` added by @dhruvmanila on 2025-05-30 08:41_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:11_

---
