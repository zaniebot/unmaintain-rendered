```yaml
number: 13401
title: "[red-knot] simplify subtypes from unions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/simplify-union-subtype
created_at: 2024-09-18T23:22:38Z
updated_at: 2025-05-07T15:23:15Z
url: https://github.com/astral-sh/ruff/pull/13401
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] simplify subtypes from unions

---

_Pull request opened by @carljm on 2024-09-18 23:22_

Add `Type::is_subtype_of` method, and simplify subtypes out of unions.


---

_Label `red-knot` added by @carljm on 2024-09-18 23:22_

---

_Review requested from @MichaReiser by @carljm on 2024-09-18 23:22_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-18 23:22_

---

_Comment by @github-actions[bot] on 2024-09-18 23:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:69 on 2024-09-19 02:05_

As soon as you find an element already in the union that you know to be a supertype of the element you're considering adding to the union, you can short-circuit, since you know you won't be adding or removing any elements from the union in that case:

```suggestion
                let mut remove = vec![];
                for element in &self.elements {
                    if ty.is_subtype_of(self.db, *element) {
                        return self;
                    } else if element.is_subtype_of(self.db, ty) {
                        remove.push(*element);
                    }
                }
                for element in remove {
                    self.elements.remove(&element);
                }
                self.elements.insert(ty);
```

---

_@AlexWaygood approved on 2024-09-19 02:07_

---

_@carljm reviewed on 2024-09-19 03:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:69 on 2024-09-19 03:24_

Ugh, yes thanks! I had this in mind when writing it, then the tests passed and I totally forgot. Will fix. 

---

_Merged by @carljm on 2024-09-19 05:06_

---

_Closed by @carljm on 2024-09-19 05:06_

---

_Branch deleted on 2024-09-19 05:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:49 on 2024-09-19 05:54_

Nit: It could be worth to call `union.reserve` here based on the assumption that most elements aren't overlapping

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-09-19 05:56_

Does it still make sense to use a hash set for `elements`, now that the "uniqueness" is no longer determined by `Eq` and `Hash` but by `is_subtype_of`? I get the impression that we could as well use a `Vec`. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-09-19 05:59_

This makes `add` `O(n^2)` where `n` is the number of removed elements. It's probably still a very low number. It might be worth to store the removed elements in an HashSet and then using `self.elements.retain`

---

_@MichaReiser reviewed on 2024-09-19 05:59_

---

_@AlexWaygood reviewed on 2024-09-19 09:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-09-19 09:58_

I considered suggesting a `HashSet` for `remove`, but then I realised that it's not necessary to use a HashSet to guarantee that all items in `remove` are unique. Because of the way it's constructed, we know that all items in `remove` are also members of `self.elements`, and `self.elements` is a `HashSet`

---

_@AlexWaygood reviewed on 2024-09-19 10:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-09-19 10:00_

This explains why mypy uses a Python `list` for the inner list of types in its `Union` representation, rather than a Python `set` (something I've been wondering about recently)

---

_@MichaReiser reviewed on 2024-09-19 10:25_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-09-19 10:25_

It's less about the uniquness but more about using a more efficient removal method that avoids copying the array elements `n` times where `n` is the number of removed elements. 

We can probably do something more clever once `self.elements` is a `Vec` because we know that the elements in `remove` and `self.elements` are in the same order. But it may also just not be worth it (hashing isn't free either). 

The cheapest would be to use `swap_remove` but that changes the order of the elements. Not sure if we're fine with that.

---

_Comment by @MichaReiser on 2024-09-19 11:46_

This change makes `UnionBuilder::map` `O(N^2)` but I don't think there's a way to avoid that

---

_@carljm reviewed on 2024-09-19 14:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:65 on 2024-09-19 14:46_

I don't think a high cardinality for `remove` will be common, but it certainly could happen (e.g. if you have many subtypes of the same base type all in a union, and then you add the common base type), so it's worth considering. Good comments!

I've replaced the `FxOrderSet` with a `Vec`, and I'm now doing a paired iteration to build up a new vec in a single pass with only the elements we want to keep (and using `with_capacity` to avoid ever needing to resize it). Will push a PR in a bit.

---

_Comment by @carljm on 2024-09-19 14:47_

> This change makes `UnionBuilder::map` `O(N^2)` but I don't think there's a way to avoid that

I assume you mean `UnionBuilder::add`? There is no `UnionBuilder::map`.

But yes, I agree; it's now `O(n^2)`, and I don't think that's avoidable.

EDIT: oh, I'm guessing you actually meant `UnionType::map`. `UnionBuilder::add` is not really `O(n^2)`; it is `O(n*m)` if you add a union to the builder.

---

_Comment by @AlexWaygood on 2024-09-19 14:50_

If we switch to using a `Vec` for the elements of a union, that will make tests such as `union.contains(Type::Any)` O(n), right? Not sure how important that is to consider

---

_Comment by @carljm on 2024-09-19 14:53_

> If we switch to using a `Vec` for the elements of a union, that will make tests such as `union.contains(Type::Any)` O(n), right? Not sure how important that is to consider

I don't think in general this is an operation we will need often, for the same reason -- the question will usually be about subtyping or assignability (or equivalence), not simple type equality.

It's possible we will have the specific case of needing to know if Any/Unknown is in the union, but I think if that's an issue we could store an extra boolean flag on every union (and potentially even leave the actual Any/Unknown entry out of it) for less cost than the cost of the `FxOrderSet`.

---

_Comment by @MichaReiser on 2024-09-19 14:56_

> EDIT: oh, I'm guessing you actually meant UnionType::map. UnionBuilder::add is not really O(n^2); it is O(n*m) if you add a union to the builder.

Yes sorry. I still think it is because we loop over `n` elements and loops over all elements that have been added to this point. So it's probably `n(n-1)/2`

---

_Comment by @carljm on 2024-09-19 14:59_

> I still think it is because we loop over `n` elements and loops over all elements that have been added to this point. So it's probably `n(n-1)/2`

Yes, I agree that this is accurate for `UnionType::map`.

I was correcting myself about `UnionBuilder::add`, which is itself only `O(n)`, or `O(n*m)` if we are adding another union (which won't ever be the case from `UnionType::map`.)

---

_@hauntsaninja reviewed on 2024-09-25 08:29_

---

_Review comment by @hauntsaninja on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2024-09-25 08:29_

In equivalent mypy code, I had to add a special fast path for literals. You can do better than quadratic for unions with lots of literals of the same type, which turns out to be a thing in the wild

---

_@carljm reviewed on 2024-09-25 17:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2024-09-25 17:54_

Interesting, thanks for pointing this out! I took a look at that optimization in mypy.

I think the case this would optimize is where a union already contains e.g. `str`, and then we try to add lots of string literal types to it, every one of which is redundant because its a subtype of `str`. Rather than going through all existing union members to check if each literal is a subtype of any of them, we can keep a hash-set of "types present in this union which have literal forms" and do an O(1) contains check against that set as the first step when adding a literal type to the union. Framed in more general terms, it's identifying that a certain set of common types have a single super-type that is most likely to rule them out of the union, and so we optimize checking for that most likely super-type by identity.

This makes sense; I'd prefer to wait to add this kind of optimization until we see it crop up in a real-world codebase and can evaluate the actual impact of the optimization in our case, but it's definitely a useful idea to keep in mind.

---

_@AlexWaygood reviewed on 2024-09-25 17:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2024-09-25 17:56_

I think what would be useful is if we added one (or more) benchmarks based on a real-world codebase that makes heavy use of large literals. (I.e., pydantic.)

---

_@hauntsaninja reviewed on 2024-09-25 19:17_

---

_Review comment by @hauntsaninja on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2024-09-25 19:17_

That's not quite the right description, it's also useful when the union doesn't contain the supertype (i.e. str). For instance, say if you were combining two unions that you knew consisted only of literal types, you could use a set union, which is linear. The mypy optimisation I added is basically that, but also works when there are non-literal types thrown in as well. Fair enough on waiting though!

---

_@carljm reviewed on 2024-09-25 19:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2024-09-25 19:24_

Oh, thanks, yeah, I misread the code. The set is `unduplicated_literal_fallbacks`, not `duplicated_literal_fallbacks`. So it looks like it's optimizing only the case you described; the mirror image of the case I described.

---

_@AlexWaygood reviewed on 2024-09-29 16:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2024-09-29 16:16_

I've created a new issue collating some of the perf issues mypy and pyright have encountered relating to unions: https://github.com/astral-sh/ruff/issues/13549

---

_@hauntsaninja reviewed on 2024-09-29 20:22_

---

_Review comment by @hauntsaninja on `crates/red_knot_python_semantic/src/types/builder.rs`:55 on 2024-09-29 20:22_

Nice!
I think there are some mypy PRs missing from the list, so if you're interested in code I'd make sure to look at main.
I'll also make it such that if you're interested in real world use cases you should only have to look at primer, looks like there are 1-2 things I never actually added.

---
