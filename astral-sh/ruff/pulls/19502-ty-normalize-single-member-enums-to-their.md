```yaml
number: 19502
title: "[ty] Normalize single-member enums to their instance type"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/normalize-single-member-enums
created_at: 2025-07-23T07:17:21Z
updated_at: 2025-07-23T08:14:22Z
url: https://github.com/astral-sh/ruff/pull/19502
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Normalize single-member enums to their instance type

---

_Pull request opened by @sharkdp on 2025-07-23 07:17_

## Summary

Fixes https://github.com/astral-sh/ty/issues/874

Labeling this as `internal`, since we haven't released the enum-expansion feature.

## Test Plan

New Markdown tests

---

_Review requested from @carljm by @sharkdp on 2025-07-23 07:17_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-23 07:17_

---

_Review requested from @dcreager by @sharkdp on 2025-07-23 07:17_

---

_Label `internal` added by @sharkdp on 2025-07-23 07:17_

---

_Label `ty` added by @sharkdp on 2025-07-23 07:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1070 on 2025-07-23 07:20_

I chose this direction, because the other seems more costly in terms of performance: If we wanted to normalize `Single` => `Literal[Single.VALUE]`, we would have to check every nominal instance type, to see if it's an enum — which requires a MRO traversal.

---

_@sharkdp reviewed on 2025-07-23 07:20_

---

_Comment by @github-actions[bot] on 2025-07-23 07:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-07-23 07:23_

Nice! Could you double-check the property tests still pass before merging? I think the relevant ones for this particular issue are still marked as flaky, but still 

---

_Comment by @sharkdp on 2025-07-23 07:47_

> Could you double-check the property tests still pass before merging?

Oh, good call — checked now. Everything still passing. We also didn't have any single-member enums in the pool of types. I added one now.

---

_Merged by @sharkdp on 2025-07-23 08:14_

---

_Closed by @sharkdp on 2025-07-23 08:14_

---

_Branch deleted on 2025-07-23 08:14_

---
