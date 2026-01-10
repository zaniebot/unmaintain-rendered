```yaml
number: 16803
title: "[red-knot] Fix fully static check for callable type"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-static-fix
created_at: 2025-03-17T14:19:05Z
updated_at: 2025-03-17T14:31:32Z
url: https://github.com/astral-sh/ruff/pull/16803
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] Fix fully static check for callable type

---

_Pull request opened by @dhruvmanila on 2025-03-17 14:19_

## Summary

This PR fixes a bug in the check for fully static callable type where we would skip unannotated parameter type.

## Test Plan

Add tests using the new `CallableTypeFromFunction` special form.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-17 14:19_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-17 14:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4585 on 2025-03-17 14:21_

I think we need to make the same change for the `signature.return_ty` code immediately below this?

---

_@AlexWaygood reviewed on 2025-03-17 14:21_

---

_@dhruvmanila reviewed on 2025-03-17 14:23_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4585 on 2025-03-17 14:23_

I don't think so because `is_some_and` would return false if the `return_ty` is `None` which would make it not fully static. It's only in this case because we short-circuit using `any`.

---

_@AlexWaygood reviewed on 2025-03-17 14:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4585 on 2025-03-17 14:25_

Sorry, you're quite right!

---

_@AlexWaygood approved on 2025-03-17 14:25_

---

_Comment by @github-actions[bot] on 2025-03-17 14:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @dhruvmanila on 2025-03-17 14:31_

---

_Closed by @dhruvmanila on 2025-03-17 14:31_

---

_Branch deleted on 2025-03-17 14:31_

---
