```yaml
number: 11671
title: "[red-knot] extract helper functions in inference tests"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cfg3
created_at: 2024-06-01T02:00:44Z
updated_at: 2024-06-03T23:46:06Z
url: https://github.com/astral-sh/ruff/pull/11671
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] extract helper functions in inference tests

---

_Pull request opened by @carljm on 2024-06-01 02:00_

There's a lot of repeat boilerplate in the type inference tests; this cuts it down a lot.


---

_Review requested from @MichaReiser by @carljm on 2024-06-01 02:00_

---

_Review request for @MichaReiser removed by @carljm on 2024-06-01 02:00_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-01 02:00_

---

_Review requested from @MichaReiser by @carljm on 2024-06-01 02:00_

---

_Label `red-knot` added by @carljm on 2024-06-01 02:01_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types/infer.rs`:274 on 2024-06-03 14:50_

It feels kinda unfortunate that we have to convert the type to a string in order to assert that it is what we expect. But I agree that this is for the best, as it's going to allow us to write much more readable tests.

---

_@AlexWaygood approved on 2024-06-03 14:50_

This is great!

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:258 on 2024-06-03 15:22_

Could you use `resolve_global_module` instead`?

---

_@MichaReiser approved on 2024-06-03 15:23_

---

_@carljm reviewed on 2024-06-03 23:18_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:274 on 2024-06-03 23:18_

On the other hand, we may eventually want to write tests based on `reveal_type` or `assert_type` anyway, which also use the textual representation of a type.

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:258 on 2024-06-03 23:23_

Good point! Done. I think this code predated `resolve_global_symbol`, and I just rearranged the existing code here without thinking about it too much.

---

_@carljm reviewed on 2024-06-03 23:23_

---

_Comment by @github-actions[bot] on 2024-06-03 23:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-06-03 23:46_

---

_Closed by @carljm on 2024-06-03 23:46_

---

_Branch deleted on 2024-06-03 23:46_

---
