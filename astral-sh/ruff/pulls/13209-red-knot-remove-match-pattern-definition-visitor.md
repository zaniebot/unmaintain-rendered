```yaml
number: 13209
title: "[red-knot] Remove match pattern definition visitor"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/match-def-visitor
created_at: 2024-09-02T09:37:20Z
updated_at: 2024-09-03T18:19:54Z
url: https://github.com/astral-sh/ruff/pull/13209
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Remove match pattern definition visitor

---

_@dhruvmanila_

## Summary

This PR is based on this discussion: https://github.com/astral-sh/ruff/pull/13147#discussion_r1739408653.

**Todo**

- [x] Add documentation for `MatchPatternState`

## Test Plan

`cargo insta test` and `cargo clippy`


---

_Label `red-knot` added by @dhruvmanila on 2024-09-02 09:37_

---

_Review requested from @carljm by @dhruvmanila on 2024-09-02 09:37_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-09-02 09:37_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-09-02 09:37_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index.rs`:1155 on 2024-09-02 09:38_

This isn't related to the change in this PR but something that's good to test for.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:39 on 2024-09-02 09:38_

Any recommendation for the name here (`MatchPatternState`)?

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:829 on 2024-09-02 09:39_

I think I might want to limit the `pattern_state` to be `Some` only during the `pattern` visit. Currently, it's `Some` during the entire `MatchCase` visit.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:831 on 2024-09-02 09:39_

This includes a bunch of `unwrap`s similar to this. Let me think about if there's a way to avoid them.

---

_@dhruvmanila reviewed on 2024-09-02 09:40_

---

_Comment by @github-actions[bot] on 2024-09-02 09:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-03 07:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:39 on 2024-09-03 07:55_

I'm leaning towards naming it `CurrentMatchCase` to match `current_assignment`. It makes it clear that both follow the same schema. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:819 on 2024-09-03 07:56_

Should we add a debug assertion here that `self.pattern_state` is `None` before entering or is it possible to have nested patterns (in which case we would need to restore the old pattern instead of setting it to `None`). 

For example, what about

```python
match "a":
	case x:
		match "b"
			case y:
				print("Blub")

```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:831 on 2024-09-03 07:59_

I would probably move the `self.pattern_state` out of the let arms to the top of the function so that we only have a single `unwrap` 

(at the top of `visit_pattern)

```
let mut pattern_state = self.pattern_state.as_mut().unwrap();
```

I guess this doesn't work because the borrow checker will get all mad at you because we call other methods on `self`. 

My normal solution would be to just `take` the value when entering the method and restore it when exiting, but I think that doesn't work as well because the value must be set for `walk_pattern` in case there are nested patterns.

---

_@MichaReiser approved on 2024-09-03 08:02_

---

_@dhruvmanila reviewed on 2024-09-03 08:24_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:819 on 2024-09-03 08:24_

Good point. I need to consider the nested case such as the one you've mentioned.

---

_@dhruvmanila reviewed on 2024-09-03 08:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:819 on 2024-09-03 08:26_

Actually, it shouldn't matter because the context is only relevant when visiting the pattern. So, we can just reset it when visiting the case body. https://github.com/astral-sh/ruff/pull/13209#discussion_r1740620907

---

_@dhruvmanila reviewed on 2024-09-03 08:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:819 on 2024-09-03 08:28_

I'll make this less confusing by inlining the `walk_match_case` function here and reset it right after visiting the pattern.

---

_@MichaReiser reviewed on 2024-09-03 08:29_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:819 on 2024-09-03 08:29_

Makes sense. We just need to be careful to re-set the current value before we first walk the body, and only then visit later match cases.

---

_@dhruvmanila reviewed on 2024-09-03 08:39_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:831 on 2024-09-03 08:39_

> I guess this doesn't work because the borrow checker will get all mad at you because we call other methods on `self`.
> 
> My normal solution would be to just `take` the value when entering the method and restore it when exiting, but I think that doesn't work as well because the value must be set for `walk_pattern` in case there are nested patterns.

Correct on both points.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:831 on 2024-09-03 08:43_

Another solution would be to override the `visit_pattern` to accept the state as a parameter but that means that the `walk_pattern` logic needs to be inlined as well to pass in the parameter.

---

_@dhruvmanila reviewed on 2024-09-03 08:43_

---

_Merged by @dhruvmanila on 2024-09-03 08:53_

---

_Closed by @dhruvmanila on 2024-09-03 08:53_

---

_Branch deleted on 2024-09-03 08:53_

---

_Comment by @carljm on 2024-09-03 17:47_

Is there a need for `CurrentMatchCase` to be separate from `CurrentAssignment`, or could it just be an enum variant of `CurrentAssignment`?

---

_Comment by @dhruvmanila on 2024-09-03 18:08_

> Is there a need for `CurrentMatchCase` to be separate from `CurrentAssignment`, or could it just be an enum variant of `CurrentAssignment`?

That's a good point. The reason for using a separate struct is because the definition isn't based on `ExprName` but using an `Identifier`. We _could_ include it in `CurrentAssignment` but it can't be used inside `visit_expr` and it would involve two unwrap call (a) for `current_assignment` being an `Option` and (b) for `CurrentAssignment` to get the expected variant (e.g., `self.current_assignment.unwrap().as_match_case().unwrap()`).

---

_Comment by @carljm on 2024-09-03 18:19_

Makes sense! That seems like a solid enough reason to me.

---
