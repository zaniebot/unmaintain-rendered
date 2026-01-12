```yaml
number: 17762
title: "[red-knot] More informative hover-types for assignments"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/hover-type-for-assignments
created_at: 2025-05-01T09:44:06Z
updated_at: 2025-05-07T15:20:08Z
url: https://github.com/astral-sh/ruff/pull/17762
synced_at: 2026-01-12T15:56:05Z
```

# [red-knot] More informative hover-types for assignments

---

_@sharkdp_

## Summary

closes https://github.com/astral-sh/ruff/issues/17122

## Test Plan

* New hover tests
* Opened the playground locally and saw that new hover-types are shown as expected.

---

_Label `red-knot` added by @sharkdp on 2025-05-01 09:44_

---

_Comment by @github-actions[bot] on 2025-05-01 09:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-05-01 09:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3348 on 2025-05-01 09:52_

This (and a similar change for other annotated assignments above) might be a bit controversial. It's related to https://github.com/astral-sh/ruff/issues/17239 in that it's a matter of using the declared type vs the inferred type. If there is no right hand side, this seems fine. But if there is (`x: int = 1`), we now show `int` when hovering over `x`. We would previously show `Never`, so `int` seems like an improvement. But it might be unexpected that we show `Literal[1]` if you do `reveal_type(x)` after this assignment.

Should we change this to show the right-hand side value type for now? (if there is one)

---

_Marked ready for review by @sharkdp on 2025-05-01 09:55_

---

_Review requested from @carljm by @sharkdp on 2025-05-01 09:55_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-01 09:55_

---

_Review requested from @dcreager by @sharkdp on 2025-05-01 09:55_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-01 09:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_ide/src/hover.rs`:470 on 2025-05-01 10:30_

woah, it's my first time looking at tests like these. This is cool! 😃

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3348 on 2025-05-01 10:30_

> Should we change this to show the right-hand side value type for now? (if there is one)

That feels like it makes sense to me? I can see arguments both ways, though; I don't have a strong opinion

---

_@AlexWaygood approved on 2025-05-01 10:30_

Nice!

---

_@sharkdp reviewed on 2025-05-01 14:26_

---

_Review comment by @sharkdp on `crates/red_knot_ide/src/hover.rs`:470 on 2025-05-01 14:26_

I agree: I also congratulated Micha on that test suite when I discovered it :smiley: 

---

_Review comment by @carljm on `crates/red_knot_ide/src/hover.rs`:516 on 2025-05-01 17:06_

It seems maybe inconsistent that we show `Literal[2]` here (not `int`, even though `C.attr` is annotated as `int`), but we show `int` for `x: int = 2`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3348 on 2025-05-01 17:08_

I commented above on a case with attributes where it feels like the behavior would be more consistent if we showed inferred type here as well?

Ultimately though I think neither one is "obviously wrong" and I don't think it's critical either way, so I wouldn't spend a lot of time on it if using the inferred type is harder.

---

_@carljm approved on 2025-05-01 17:08_

Nice!

---

_@sharkdp reviewed on 2025-05-01 18:01_

---

_Review comment by @sharkdp on `crates/red_knot_ide/src/hover.rs`:516 on 2025-05-01 18:01_

I changed it so that we now show `Literal[2]` for `x: int = 2`

---

_@sharkdp reviewed on 2025-05-01 18:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3348 on 2025-05-01 18:02_

Ok, we're using the inferred type now.

---

_Merged by @sharkdp on 2025-05-01 18:33_

---

_Closed by @sharkdp on 2025-05-01 18:33_

---

_Branch deleted on 2025-05-01 18:33_

---
