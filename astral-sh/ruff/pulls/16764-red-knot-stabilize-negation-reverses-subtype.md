```yaml
number: 16764
title: "[red-knot] Stabilize `negation_reverses_subtype_order` property test"
type: pull_request
state: closed
author: mtshiba
labels:
  - ty
assignees: []
base: main
head: fix-intersection-ordering
created_at: 2025-03-15T15:17:22Z
updated_at: 2025-03-17T12:25:04Z
url: https://github.com/astral-sh/ruff/pull/16764
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] Stabilize `negation_reverses_subtype_order` property test

---

_Pull request opened by @mtshiba on 2025-03-15 15:17_

## Summary

From #16641

As stated in this comment (https://github.com/astral-sh/ruff/pull/16641#discussion_r1996153702), the current ordering implementation for intersection types is incorrect. So, I will introduce lexicographic ordering for intersection types.

## Test Plan

As can be seen [here](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A14899-gradual-type-assignability), it appears that the execution time of `red_knot_check_file` will slow down by about 3% when this change is merged. The slow-down itself is unavoidable given that we have been skipping processes that should have been done, but please let me know if there is a better ordering algorithm or some sort of salsa query technique.

---

_Review requested from @carljm by @mtshiba on 2025-03-15 15:17_

---

_Review requested from @MichaReiser by @mtshiba on 2025-03-15 15:17_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-15 15:17_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-15 15:17_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-15 15:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:28 on 2025-03-15 16:38_

nit: for all other functions and methods in red-knot, `db` is always the first argument. We should do that here too, for consistency:

```suggestion
    db: &'db dyn crate::Db,
    left: &Type<'db>,
    right: &Type<'db>,
```

---

_@AlexWaygood reviewed on 2025-03-15 16:40_

Thanks, this looks great! It looks like this change means that the `negation_reverses_subtype_order` property test now passes consistently; could you move it from the `flaky` submodule in `property_tests.rs` to the `stable` submodule?

I verified that it now passes consistently by running `QUICKCHECK_TESTS=2000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::flaky::negation_reverses_subtype_order`, which quickly fails on `main` but not on this PR branch.

---

_Closed by @AlexWaygood on 2025-03-17 07:54_

---

_Reopened by @AlexWaygood on 2025-03-17 07:55_

---

_Comment by @github-actions[bot] on 2025-03-17 08:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-03-17 08:09_

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

_Comment by @AlexWaygood on 2025-03-17 11:54_

> As can be seen [here](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A14899-gradual-type-assignability), it appears that the execution time of `red_knot_check_file` will slow down by about 3% when this change is merged. The slow-down itself is unavoidable given that we have been skipping processes that should have been done, but please let me know if there is a better ordering algorithm or some sort of salsa query technique.

For this change on its own, the benchmarks are in fact [entirely neutral](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Afix-intersection-ordering). I think the slowdown observed on that earlier PR must come from somewhere else.

Thanks for the PR, this is great!

---

_Renamed from "fix incorrect ordering of intersection" to "[red-knot] Stabilize `negation_reverses_subtype_order` property test" by @AlexWaygood on 2025-03-17 12:17_

---

_Closed by @AlexWaygood on 2025-03-17 12:17_

---

_Comment by @AlexWaygood on 2025-03-17 12:19_

I'm so sorry -- I closed this meaning to close and reopen it, but now GitHub isn't letting me reopen it. I'll try to open a fresh PR from this branch.

---

_Comment by @AlexWaygood on 2025-03-17 12:25_

https://github.com/astral-sh/ruff/pull/16801. Sorry again; don't know why this happened :-(

---
