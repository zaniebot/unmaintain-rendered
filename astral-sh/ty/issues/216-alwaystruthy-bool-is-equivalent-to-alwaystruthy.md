```yaml
number: 216
title: "`AlwaysTruthy | bool` is equivalent to `AlwaysTruthy | Literal[False]`"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - set-theoretic types
assignees: []
created_at: 2025-01-15T20:17:50Z
updated_at: 2025-11-13T00:40:47Z
url: https://github.com/astral-sh/ty/issues/216
synced_at: 2026-01-10T02:06:24Z
```

# `AlwaysTruthy | bool` is equivalent to `AlwaysTruthy | Literal[False]`

---

_Issue opened by @AlexWaygood on 2025-01-15 20:17_

This equivalence follows from the fact that `bool` is equivalent to `Literal[True] | Literal[False]` and from the fact that `Literal[True]` is a subtype of `AlwaysTruthy`. A proof of the fact that these are equivalent can be found in the fact that there are two ways that the union `Literal[False] | Literal[True] | AlwaysTruthy` could be validly simplified:

- `(Literal[False] | Literal[True]) | AlwaysTruthy` -> `bool | AlwaysTruthy`
- `Literal[False] | (Literal[True] | AlwaysTruthy)` -> `Literal[False] | AlwaysTruthy`

We don't currently recognise that `AlwaysTruthy | bool` and `AlwaysTruthy | Literal[False]` are equivalent types; this should be fixed.

---

_Label `bug` added by @AlexWaygood on 2025-01-15 20:17_

---

_Comment by @AlexWaygood on 2025-01-16 10:15_

There are a few of these that we need to recognise, actually:
- `AlwaysTruthy | bool ≡ AlwaysTruthy | Literal[False]`
- `AlwaysFalsy | bool ≡ AlwaysFalsy | Literal[True]`
- `AlwaysTruthy | tuple ≡ AlwaysTruthy | tuple[()]`
- `AlwaysTruthy | LiteralString ≡ AlwaysTruthy | Literal[""]`

---

_Comment by @AlexWaygood on 2025-01-16 10:41_

Some failing tests, if anybody is interested in working on this:

````diff
--- a/crates/red_knot_python_semantic/resources/mdtest/union_types.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/union_types.md
@@ -141,3 +141,17 @@ def _(
     reveal_type(i1)  # revealed: P & Q
     reveal_type(i2)  # revealed: P & Q
 ```
+
+## Equivalent unions
+
+```py
+from typing_extensions import Literal
+from knot_extensions import is_equivalent_to, AlwaysTruthy, AlwaysFalsy, static_assert, Intersection
+
+static_assert(is_equivalent_to(AlwaysTruthy | bool, AlwaysTruthy | Literal[False]))
+static_assert(is_equivalent_to(AlwaysFalsy | bool, AlwaysFalsy | Literal[True]))
+static_assert(is_equivalent_to(AlwaysTruthy | tuple, AlwaysTruthy | tuple[()]))
+static_assert(is_equivalent_to(AlwaysTruthy | LiteralString, AlwaysTruthy | Literal[""]))
+```
````

---

_Comment by @carljm on 2025-01-17 22:29_

This (and the complexity of our existing handling of `bool` in unions and intersections) makes me wonder if it would be simpler if we always eagerly decomposed `bool` to `Literal[False] | Literal[True]` in a union, and then just collected it back to `bool` at display time? We already collect literal types in union display, so collecting these two together and displaying them as `bool` shouldn't be hard.

This would be generally in line with "one internal representation for equivalent things."

This means that intersections with `bool` (e.g. `bool & P`) would technically become `(Literal[True] & P) | (Literal[False] & P)`. But I think this would actually be OK, because `Literal[True]` and `Literal[False]` are both singleton types, which should mean any intersection with either of them always simplifies to either `Never` or to `Literal[True/False]` itself, meaning `bool & P` would never actually remain a union-of-intersections, it must always simplify down to either `Never`, either `Literal[True]` or `Literal[False]`, or `Literal[True] | Literal[False]`.

---

_Comment by @AlexWaygood on 2025-01-17 22:41_

Yes, that sounds reasonable to me! I agree that this one seems like a bit of a pain to work into our existing infrastructure without doing something like that

---

_Comment by @AlexWaygood on 2025-01-20 11:39_

> This (and the complexity of our existing handling of `bool` in unions and intersections) makes me wonder if it would be simpler if we always eagerly decomposed `bool` to `Literal[False] | Literal[True]` in a union, and then just collected it back to `bool` at display time? We already collect literal types in union display, so collecting these two together and displaying them as `bool` shouldn't be hard.

This works for the `bool` case, but the `LiteralString` case is harder to fix because of the fact that we don't have a dedicated type for "all the literal strings except for `Literal[""]`". As well as the invariant I mentioned above (`AlwaysTruthy | LiteralString ≡ AlwaysTruthy | Literal[""]`), the property tests inform me that we also have at least two others that we must respect when simplifying and comparing unions:

- `AlwaysTruthy | LiteralString ≡ AlwaysTruthy | Literal[""]`
- `~AlwaysFalsy | LiteralString ≡ ~AlwaysFalsy | Literal[""]`
- `AlwaysTruthy | AlwaysFalsy | LiteralString ≡ AlwaysTruthy | AlwaysFalsy`

---

_Comment by @AlexWaygood on 2025-01-20 11:53_

Ah... and even with the `bool` case, decomposing `bool` to `Literal[True] | Literal[False]` _only_ in the context of unions causes its own problems. Now `bool` is no longer a subtype of `bool | str`, because `bool` remains `bool` but `bool | str` becomes `Literal[True] | Literal[False] | str`. This causes the stable property test `all_fully_static_type_pairs_are_subtype_of_their_union` to start failing. Should we decompose `bool` to `Literal[True, False]` _everywhere_?

---

_Comment by @carljm on 2025-01-22 20:47_

> Now `bool` is no longer a subtype of `bool | str`

Would this be solved by a special case in `is_equivalent_to` encoding the equivalence of `bool` to `Literal[True] | Literal[False]`?

I guess if our aim is to prefer "one representation for one type" then we should prefer to decompose everywhere or nowhere. But I don't think decomposing everywhere is really an option, as we need the typeshed stub for the `bool` type to tell us a lot of things about the `bool` type.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-25 13:02_

---

_Comment by @Geo5 on 2025-02-12 23:18_

~~I just found a possible bug/disagreement with `mypy` which might be relevant to this discussion: https://github.com/astral-sh/ruff/issues/16129~~ Sorry for the noise, didn't read the docs close enough, you are probably already aware!

---

_Label `bug` added by @MichaReiser on 2025-05-07 15:22_

---

_Renamed from "[red-knot] `AlwaysTruthy | bool` is equivalent to `AlwaysTruthy | Literal[False]`" to "`AlwaysTruthy | bool` is equivalent to `AlwaysTruthy | Literal[False]`" by @MichaReiser on 2025-05-07 15:27_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-11 07:20_

---

_Comment by @JelleZijlstra on 2025-07-12 16:18_

It looks like you currently have an implementation of `is_equivalent` that walks over each type in a separate traversal from other operations like `is_subtype`.

(Terminology: `A = B` means A is equivalent to B, `A <: B` means A is a subtype of B, `Top[A]` is the top and `Bottom[A]` the bottom materialization.)

I think you can achieve a simpler implementation by implementing equivalence in terms of subtyping. For fully static types, `A = B` if and only if `A <: B` and `B <: A`: if two sets are both subsets of each other, they must be equal. For dynamic types, this holds too if you use the appropriate definition of subtyping, but that definition is different from the one you already use in union simplification (which is `Top[A] <: Bottom[B]`). Instead, the definition you need to use here is: given two gradual types A and B, `A <: B <=> Bottom[A] <: Bottom[B] and Top[A] <: Top[B]`. Thus, two gradual types are equivalent under this definition if their bottom materializations as well as their top materializations are equivalent.

Now, this approach requires you to implement subtyping correctly and to have a way to deal with the top and bottom materializations. Regarding subtyping, ty currently fails `static_assert(is_subtype_of(AlwaysTruthy | bool, AlwaysTruthy | Literal[False]))`. The approach I use for this in pycroscope is that certain types support a "decompose" operation; for example, `bool` decomposes into `Literal[True]` and `Literal[False]`. If we're assigning to a union and the assignment appears to fail, we decompose the right-hand side and try assigning the resulting union to the left-hand side. In this example, the important part we're evaluating is `bool <: AlwaysTruthy | Literal[False]`. `bool` isn't a subtype of either of the elements in the union, so we fall back to decomposing into `Literal[True] | Literal[False]`, and now we see that subtyping succeeds, because True is a subtype of AlwaysTruthy and False is False. This technique means we don't have to worry much about normalizing unions.

The other tricky part is the bottom materialization, as we've already talked about this in other issues. If you think directly in set-theoretic terms, the bottom materialization is very often an uninhabited type and therefore the same as Never, which would have the undesirable consequence that lots of gradual types are consistent with each other. I currently think you can get around this by just, well, pretending that the bottom materialization exists separately from Never and treating it like an infinite intersection. This feels unsatisfying and I need to think more about it, but I believe it may work well in practice.

But with these concerns out of the way, what I'm suggesting here enables you to implement the various operations on types in terms of a smaller number of primitives: subtyping and the top/bottom materializations. That should make it easier to ensure the various operations behave consistently with each other.

---

_Comment by @carljm on 2025-07-14 23:50_

I think there are several separable points here. The most relevant to this issue is the concept of "decomposable" types. This concept already exists in the spec (and now also in ty, though it post-dates the prior discussion in this issue) as part of the overload evaluation spec. I think this is a useful concept and I believe it could be used more generally in subtyping to fix this issue. I think that's true independent of whether we also decide to implement equivalence in terms of subtyping of top and bottom materializations. The latter is also interesting, but I don't think it's required to solve this issue, it has some theoretical question marks that you observed, and I'm not convinced yet that it will be a net simplification or a performance improvement over what we have today.

---

_Comment by @carljm on 2025-11-13 00:40_

Not clear to me how critical this is, given we haven't seen any user reports about it. Currently marking it in-scope for stable release as P2, happy to discuss/adjust.

---
