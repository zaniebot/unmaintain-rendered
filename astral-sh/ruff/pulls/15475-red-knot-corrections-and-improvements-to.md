```yaml
number: 15475
title: "[red-knot] Corrections and improvements to intersection simplification"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/truthy-bools
created_at: 2025-01-14T14:03:49Z
updated_at: 2025-01-14T18:15:40Z
url: https://github.com/astral-sh/ruff/pull/15475
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Corrections and improvements to intersection simplification

---

_Pull request opened by @AlexWaygood on 2025-01-14 14:03_

## Summary

I noticed that we were incorrectly oversimplifying certain intersections, for example:

```py
from knot_extensions import Intersection, Not, AlwaysTruthy

def g(
    x: Intersection[str, Not[AlwaysTruthy]]
):
    if isinstance(x, bool):
        reveal_type(x)  # revealed: Literal[False]
```

Here, `bool & ~AlwaysTruthy` can indeed be simplified to `Literal[False]`, and therefore `str & ~AlwaysTruthy & bool` can be simplified to `str & Literal[False]`, which then simplifies further to `Never` since `str` and `Literal[False]` are disjoint. But that's not what we're doing in the example above: instead, we're attempting to simplify `str` out of the intersection as part of the initial simplification, which then means that we never realise further along in the intersection simplification algorithm that the entire intersection simplifies to `Never`.

This PR fixes the bug, and also adds some more simplifications for intersections that we weren't doing previously:

- `bool & AlwaysTruthy` -> `Literal[True]`
- `bool & AlwaysFalsy` -> `Literal[False]`
- `LiteralString & AlwaysTruthy` -> `LiteralString & ~Literal[""]`
- `LiteralString & AlwaysFalsy` -> `Literal[""]`

This furthers our aim of having equivalent types use identical internal representations wherever it's feasible to do so.

## Test Plan

- `cargo test -p red_knot_python_semantic --test mdtest`
- `uvx pre-commit run -a`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-14 14:03_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-14 14:03_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-14 14:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-14 14:03_

---

_Comment by @github-actions[bot] on 2025-01-14 14:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2025-01-14 14:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:694 on 2025-01-14 14:10_

Whether this is a "simplification" of `LiteralString & AlwaysTruthy` is of course questionable, since a positive intersection is in some sense "simpler" than a negated intersection. I don't really mind much which one we go with, but I do think it's useful to pick one of them as our "standard representation" of the type, since the two are equivalent.

Neither is _particularly_ user-friendly in its display right now, but users will probably be more familiar with `Literal` types than they are with `AlwaysTruthy` or `AlwaysFalsy`, so I weakly prefer simplifying to `LiteralString & ~Literal[""]` rather than `LiteralString & AlwaysTruthy`

---

_Comment by @AlexWaygood on 2025-01-14 14:11_

2% speedup on the Codspeed incremental benchmark?? I'll take it! https://codspeed.io/astral-sh/ruff/branches/alex%2Ftruthy-bools

---

_@AlexWaygood reviewed on 2025-01-14 14:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:415 on 2025-01-14 14:18_

I suspect the codspeed improvement is because we're now doing fewer Salsa lookups eagerly due to changes like this (`KnownClass::Bool.to_instance(db)` eagerly looked up the `bool` type in the Salsa `db`) whereas the new `contains_bool()` closure here is lazier

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:660 on 2025-01-14 15:10_

Would it make sense to replace `str` in these examples with an arbitrary `class P: ...` to make it clear that this is not related to a property of `str`?

---

_@sharkdp reviewed on 2025-01-14 15:10_

---

_@sharkdp reviewed on 2025-01-14 15:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:654 on 2025-01-14 15:12_

You *could* write all of the new tests in this test suite using
```py
static_assert(is_equivalent_to(Intersection[bool, AlwaysTruthy], Literal[True]))
```
because they're all fully-static types. I think it makes them easier to read. On the other hand, it would make them dependent on a correct implementation of `is_equivalent_to`, so feel free to leave them in the current form if you think it makes them more independent.

---

_@AlexWaygood reviewed on 2025-01-14 15:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:654 on 2025-01-14 15:19_

I think I weakly prefer the tests the way they are, because I don't just want to test that `bool & AlwaysTruthy` is _equivalent_ to `Literal[True]` (that could be done with a special case in `Type::is_equivalent_to`); I want to test that `bool & AlwaysTruthy` is _eagerly simplified_ to `Literal[True]`.

Do my tests actually test that? Well... no, they actually test that the intersection is simplified to a type that `Type::display` displays as `"Literal[True]"`. The type could always be special-cased in `Type::display`; all mdtests are to some extent integration tests rather than unit tests if we're being pedantic about it. But it still feels slightly _closer_ to what I'd _like_ to test here to keep them as they are.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:660 on 2025-01-14 15:21_

good call

---

_@AlexWaygood reviewed on 2025-01-14 15:21_

---

_@MichaReiser reviewed on 2025-01-14 15:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:363 on 2025-01-14 15:27_

I think it's now possible that `index` is incorrect because `FxHashSet` doesn't preserve insertion order. That means, it can now happen that the index order is `3, 1, 5` 

I'm also a bit confused why it's no longer necessary to iterate in reverse order? 

---

_@sharkdp reviewed on 2025-01-14 15:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/intersection_types.md`:694 on 2025-01-14 15:27_

> Whether this is a "simplification" of `LiteralString & AlwaysTruthy` is of course questionable

I think it is!

It's not just a question of displaying types to users, I think there's potentially also more we can do with `~Literal[""]`, for example in narrowing.

---

_@AlexWaygood reviewed on 2025-01-14 15:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:363 on 2025-01-14 15:33_

Oh, really good point. Yeah, this is incorrect. I think we have some missing test coverage here.

---

_@AlexWaygood reviewed on 2025-01-14 15:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:363 on 2025-01-14 15:50_

should be fixed in https://github.com/astral-sh/ruff/pull/15475/commits/3bf1a6aa600d4090322c695248bb83b8e78d79d8 -- I switched to a `BTreeSet`, went back to iterating in reverse order, and added a test that would have caught this bug

---

_@MichaReiser reviewed on 2025-01-14 16:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:363 on 2025-01-14 16:20_

I wonder if it's worth using a `BTreeSet` or if we could keep using a `Vec` and filter out duplicate values when iterating (by simply storing the last removed index and skipping if it remains the same). 

I don't know which one will perform better. That probably depends on how common duplicates are

---

_@AlexWaygood reviewed on 2025-01-14 16:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:363 on 2025-01-14 16:25_

`to_remove` might have been populated by multiple different loops over `self.positive` and `self.negative` with my refactor, so this isn't just about deduplicating. It's also about ensuring the collection of removable indices stays in sorted order.

I could instead use a single `smallvec` for each loop, which would be a bit closer to the current code. It would be a _bit_ more repetitive but might be more performant due to fewer allocations 🤷

---

_@AlexWaygood reviewed on 2025-01-14 17:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:363 on 2025-01-14 17:52_

On closer examination, I think the `BTreeSet` was both unnecessary and possibly buggy... I've removed it. Thanks for pushing me on this :-)

---

_@carljm approved on 2025-01-14 18:09_

This looks good to me, no notes!

Thank you!

---

_Merged by @AlexWaygood on 2025-01-14 18:15_

---

_Closed by @AlexWaygood on 2025-01-14 18:15_

---

_Branch deleted on 2025-01-14 18:15_

---
