```yaml
number: 12689
title: "[red-knot] Infer float and complex literal expressions"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/infer-float-complex
created_at: 2024-08-05T12:13:53Z
updated_at: 2024-08-06T06:33:26Z
url: https://github.com/astral-sh/ruff/pull/12689
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Infer float and complex literal expressions

---

_@dhruvmanila_

## Summary

This PR implements type inference for float and complex literal expressions.

## Test Plan

Add test cases for both types.


---

_Label `red-knot` added by @dhruvmanila on 2024-08-05 12:13_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-05 12:13_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-05 12:13_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-05 12:13_

---

_Comment by @github-actions[bot] on 2024-08-05 12:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-08-05 13:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1662 on 2024-08-05 13:01_

Hmm, this doesn't look correct to me. `Literal[int]` implies that the `b` symbol is literally assigned to the class object `builtins.int` (e.g. `b = int`). But that's not true; it's assigned to an instance of `int` (`b = 9223372036854775808`), not the `int` class itself.

---

_@dhruvmanila reviewed on 2024-08-05 13:51_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1662 on 2024-08-05 13:51_

Yes, I think this is currently a limitation and must be resolved separately as is the case for other literal types as well (`Literal[set]`, `Literal[tuple]`, etc.).

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1662 on 2024-08-05 13:59_

That seems quite an important limitation for us to resolve; I'd be inclined to defer making changes to `infer_number_literal_expression()` until it is resolved, in that case. I'm not sure what the use is of adding type inference for `float` and `complex` expressions if we infer the wrong type entirely ;-)

---

_@AlexWaygood reviewed on 2024-08-05 13:59_

---

_@dhruvmanila reviewed on 2024-08-05 14:15_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1662 on 2024-08-05 14:15_

Yeah, I think that makes sense. I'll put this in draft for the time being.

---

_Converted to draft by @dhruvmanila on 2024-08-05 14:15_

---

_Review request for @carljm removed by @dhruvmanila on 2024-08-05 14:15_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-08-05 14:15_

---

_@carljm approved on 2024-08-05 20:20_

Yeah, sorry, this was my oversight in a previous PR. Rebasing on https://github.com/astral-sh/ruff/pull/12695 should allow fixing this; LGTM with that change.

---

_Marked ready for review by @dhruvmanila on 2024-08-06 06:19_

---

_Merged by @dhruvmanila on 2024-08-06 06:24_

---

_Closed by @dhruvmanila on 2024-08-06 06:24_

---

_Branch deleted on 2024-08-06 06:24_

---
