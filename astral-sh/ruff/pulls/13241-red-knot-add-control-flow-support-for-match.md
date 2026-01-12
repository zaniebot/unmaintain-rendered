```yaml
number: 13241
title: "[red-knot] Add control flow support for match statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/match-control-flow
created_at: 2024-09-04T12:25:31Z
updated_at: 2024-09-09T20:44:21Z
url: https://github.com/astral-sh/ruff/pull/13241
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Add control flow support for match statement

---

_@dhruvmanila_

## Summary

This PR adds support for control flow for match statement.

It also adds the necessary infrastructure required for narrowing constraints in case blocks and implements the logic for `PatternMatchSingleton` which is either `None` / `True` / `False`. Even after this the inferred type doesn't get simplified completely, there's a TODO for that in the test code.

## Test Plan

Add test cases for control flow for (a) when there's a wildcard pattern and (b) when there isn't. There's also a test case to verify the narrowing logic.


---

_Label `red-knot` added by @dhruvmanila on 2024-09-04 12:25_

---

_Comment by @codspeed-hq[bot] on 2024-09-04 12:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-control-flow)

### Merging #13241 will **not alter performance**

<sub>Comparing <code>dhruv/match-control-flow</code> (540ba2a) with <code>main</code> (ac720cd)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-09-09 18:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2024-09-09 18:16_

---

_Review requested from @carljm by @dhruvmanila on 2024-09-09 18:16_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-09-09 18:17_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-09-09 18:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:235 on 2024-09-09 18:55_

Can we implement a custom constructor on `PatternConstraint` that encapsulates a bit more of this? Details like `count::Count::default()` and creating `AstNodeRef` don't feel like things we should need to be worrying about here in the builder.

(Probably there's existing code in here like `add_standalone_expression` that the same comment could be made about...)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:648 on 2024-09-09 18:59_

Can't we fairly easily address this TODO right now, just by making this an `if let` and putting a bunch of the below code inside the `if`? (Or alternatively just returning early here instead of panicking, if all the below code would go inside the `if`)

If we do this in this PR, we should also add a test.

But if it's not trivial it's also fine to do in another PR. I would just prefer if we avoid adding panics like this, if we can.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3542 on 2024-09-09 19:05_

This feels like the same test as the previous one, effectively? Is there value in having both?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3877 on 2024-09-09 19:16_

Thinking through the simplification we do, I'm pretty sure this is actually what we we end up with, because we normalize to disjunctive normal form (union of intersections), so `(None | 1) & None` distributes to `(None & None) | (1 & None)` which simplifies to `(None | (1 & None))`, which flattens into the outer union, giving:

```suggestion
        // `Literal[0] | None | (Literal[1] & None)`
```

The simplification we are missing here is knowing that `1 & None` is a subtype of `None`, and therefore simplifies out of the union entirely, leaving just `0 | None` in the union, which is the right answer.

(I added the wrapping parens in the union and intersection displays locally and verified what I've said here is actually correct this time!)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:35 on 2024-09-09 19:18_

I don't think expressions need to be privileged in the naming here, we can rename to `all_narrowing_constraints_for_expression` for better parity.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:186 on 2024-09-09 19:24_

An intersection with only one positive value in it always simplifies back to just that value, so we don't need to build an intersection at all here, it's a no-op. Given that intersection builders allocate, this might help with the regression.
```suggestion
            self.constraints.insert(symbol, ty);
```

---

_Review comment by @carljm on `crates/ruff_python_ast/src/nodes.rs`:3139 on 2024-09-09 19:25_

Does this deserve tests of its own?

---

_@carljm approved on 2024-09-09 19:26_

This looks great! Probably the most important comment is the intersection builder one which may help with perf, everything looks correct to me.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:88 on 2024-09-09 19:29_

We should only do this if we've actually found a case below that we know how to handle, otherwise it's wasted Salsa work.

---

_@carljm reviewed on 2024-09-09 19:29_

---

_@dhruvmanila reviewed on 2024-09-09 19:33_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3542 on 2024-09-09 19:33_

There's an additional definition that's outside the match statement and wanted to make sure that it's being accounted for and the new logic doesn't interfere in any way.

---

_@dhruvmanila reviewed on 2024-09-09 19:36_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:648 on 2024-09-09 19:36_

Yeah, that should be trivial to address in this PR.

---

_@dhruvmanila reviewed on 2024-09-09 19:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:3139 on 2024-09-09 19:43_

Yeah, makes sense.

---

_@dhruvmanila reviewed on 2024-09-09 19:53_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:3139 on 2024-09-09 19:53_

Thanks, that did uncover a bug where the first case pattern in the above example wasn't being considered a wildcard.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-09-09 19:56_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:235 on 2024-09-09 20:43_

I'll add this in a follow-up PR.

---

_@dhruvmanila reviewed on 2024-09-09 20:44_

---

_Merged by @dhruvmanila on 2024-09-09 20:44_

---

_Closed by @dhruvmanila on 2024-09-09 20:44_

---

_Branch deleted on 2024-09-09 20:44_

---
