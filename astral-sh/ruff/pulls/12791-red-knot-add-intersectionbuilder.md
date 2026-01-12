```yaml
number: 12791
title: "[red-knot] add IntersectionBuilder"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/union-intersection
created_at: 2024-08-09T23:17:28Z
updated_at: 2024-08-12T18:56:06Z
url: https://github.com/astral-sh/ruff/pull/12791
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] add IntersectionBuilder

---

_@carljm_

For type narrowing, we'll need intersections (since applying type narrowing is just a type intersection.)

Add `IntersectionBuilder`, along with some tests for it and `UnionBuilder` (renamed from `UnionTypeBuilder`).

We use smart builders to ensure that we always keep these types in disjunctive normal form (DNF). That means that we never have deeply nested trees of unions and intersections: unions flatten into unions, intersections flatten into intersections, and intersections distribute over unions, so the most complex tree we can ever have is a union of intersections. We also never have a single-element union or a single-positive-element intersection; these both just simplify to the contained type.

Maintaining these invariants means that `UnionBuilder` doesn't necessarily end up building a `Type::Union` (e.g. if you only add a single type to the union, it'll just return that type instead), and `IntersectionBuilder` doesn't necessarily build a `Type::Intersection` (if you add a union to the intersection, we distribute the intersection over that union, and `IntersectionBuilder` will end up returning a `Type::Union` of intersections). 

We also simplify intersections by ensuring that if a type and its negation are both in an intersection, they simplify out. (In future this should also respect subtyping, not just type identity, but we don't have subtyping yet.) We do implement subtyping of `Never` as a special case for now.

Most of this PR is unused for now until type narrowing lands; I'm just breaking it out to reduce the review fatigue of a single massive PR.


---

_Label `red-knot` added by @carljm on 2024-08-09 23:17_

---

_Review requested from @MichaReiser by @carljm on 2024-08-09 23:17_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-09 23:17_

---

_Comment by @github-actions[bot] on 2024-08-09 23:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:296 on 2024-08-10 00:00_

```suggestion
            1 => self.elements[0],
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:318 on 2024-08-10 00:01_

nit: Does `Type` add anything here? `IntersectionBuilder`/`IntersectionBuilderInner` might be slightly shorter than `IntersectionTypeBuilder`/`InnerIntersectionTypeBuilder`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:352 on 2024-08-10 00:12_

hmm, I find the `.fold()` here quite hard to grok. I suppose it's just personal preference, but I'd find this easier to parse:

```suggestion
            let mut builder = IntersectionTypeBuilder::empty(self.db);
            for element in union.elements(self.db) {
                let new_union_element = self.clone().add_positive(element);
                builder
                    .intersections
                    .extend(new_union_element.intersections);
            }
            builder
```

I _think_ it's equivalent? Your tests seem to pass, anyway :-)

(Same comment applies for your `add_negative()` method below)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:468 on 2024-08-10 00:13_

```suggestion
            (1, 0) => self.positive[0],
```

---

_@AlexWaygood reviewed on 2024-08-10 00:14_

---

_Comment by @AlexWaygood on 2024-08-10 00:15_

(Have only skimmed so far; will look in more depth over the weekend or next week :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:352 on 2024-08-10 13:19_

Some added docs here would also be great. Even after rewriting it in imperative style, it took me a _little_ bit of time to figure out what was going on here. I believe you're applying the transformation where `A & (B | C)` -> `(A & B) | (A & C)`. Correct?

---

_@AlexWaygood reviewed on 2024-08-10 13:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:406 on 2024-08-10 13:20_

```suggestion
#[derive(Debug, Clone, Default)]
struct InnerIntersectionTypeBuilder<'db> {
    positive: FxOrderSet<Type<'db>>,
    negative: FxOrderSet<Type<'db>>,
}

impl<'db> InnerIntersectionTypeBuilder<'db> {
    fn new() -> Self {
        Self::default()
    }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:411 on 2024-08-10 13:21_

I guess `inter` is short for `intersection` here, but at first glance I assumed it was a typo and you meant to write `inner` :P

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:454 on 2024-08-10 13:26_

This isn't correct: `None` intersects with `object`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:448 on 2024-08-10 13:26_

subtyping or assignability? ;)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:436 on 2024-08-10 13:27_

I assume you don't both with `.shrink_to_fit()` calls here because it'll probably cost more than it's worth for such a frequently called method, and you'll reclaim the memory in the `build()` method anyway. Is that right?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:591 on 2024-08-10 13:34_

You might want to add a comment here about how we could simplify this further once we have greater understanding of assignability/subtyping; otherwise it might be confusing if somebody else tries to implement it later and this test starts failing.

...Hmm, but perhaps that applies for quite a few tests being added here. Not sure.

---

_@AlexWaygood approved on 2024-08-10 13:35_

Overall this looks great. I love the builder interfaces.

---

_@carljm reviewed on 2024-08-11 04:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:318 on 2024-08-11 04:53_

Yep, agreed, I'll remove `Type` from both of these names.

---

_@carljm reviewed on 2024-08-11 04:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:352 on 2024-08-11 04:57_

I think it is preference, and I prefer the fold :) I'll add comments to see if I can make it clearer, though.

---

_@carljm reviewed on 2024-08-11 04:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:448 on 2024-08-11 04:59_

Subtyping. Intersections and unions can't be simplified based on assignability. `T & Any` doesn't simplify; that was the whole point of my talk! ;)

---

_@carljm reviewed on 2024-08-11 05:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:436 on 2024-08-11 05:00_

Right, it only makes sense to `shrink_to_fit` when we are turning things immutable.

---

_@carljm reviewed on 2024-08-11 05:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:591 on 2024-08-11 05:02_

Ah hm, yeah, good point -- my favorite option would be to rework this to use types where it is correct not to simplify; that might actually not be too hard, using `Any`. I'll make that change, thanks for pointing this out!

---

_@carljm reviewed on 2024-08-11 05:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:591 on 2024-08-11 05:08_

I also noticed while updating the tests that the self-canceling code should not apply to `Any` or `Unknown`: `Any & ~Any` does not simplify to `Never`, because the two `Any` are both unknown types and might not be the same type; it should instead simplify to `Any`. I just added a TODO for this, it doesn't need to be fixed in this PR.

---

_@carljm reviewed on 2024-08-11 05:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:454 on 2024-08-11 05:08_

Ah shoot, great catch. There actually isn't any test in this PR depending on this clause, so I just removed it for now.

---

_Comment by @carljm on 2024-08-11 05:25_

Thanks for the great review!

---

_Comment by @codspeed-hq[bot] on 2024-08-11 05:30_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/union-intersection)

### Merging #12791 will **not alter performance**

<sub>Comparing <code>cjm/union-intersection</code> (106e2d8) with <code>main</code> (4b9ddc4)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Renamed from "[red-knot] add IntersectionTypeBuilder" to "[red-knot] add IntersectionBuilder" by @AlexWaygood on 2024-08-11 11:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:395 on 2024-08-12 07:48_

Not sure if it's worth short-circuiting here if `intersection` contains exactly one element (to avoid allocating a hash set)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:265 on 2024-08-12 07:50_

It might be helpful to turn some of your PR comments into a document. For example, that a union builder isn't guaranteed to return a union. It might be useful to extract the builders into their own `types/builder` module so that you can make your PR comment a module level comment.

---

_@MichaReiser approved on 2024-08-12 07:50_

---

_@carljm reviewed on 2024-08-12 16:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:395 on 2024-08-12 16:18_

Yeah, this makes sense! One interesting Rust note here, `build` consumes the builder, I wasn't sure of the best way to take ownership of the one element in a vec when I know it has only one element; `self.intersections[0].build(db)` obviously doesn't work because I can't move out of the vec. I settled on `self.intersections.pop().unwrap().build(db)` which also requires having `build` take `mut self` instead of `self` -- all that should be fine, just wondered if there was a simpler way I was missing.

---

_@MichaReiser reviewed on 2024-08-12 16:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:395 on 2024-08-12 16:30_

Not really. Maybe `into_iter().next().unwrap()` but that's equally bad. 

---

_Merged by @carljm on 2024-08-12 18:56_

---

_Closed by @carljm on 2024-08-12 18:56_

---

_Branch deleted on 2024-08-12 18:56_

---
