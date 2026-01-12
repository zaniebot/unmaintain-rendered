```yaml
number: 13214
title: "[red-knot] Add debug assert to check for duplicate definitions"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/assert-duplicate-definitions
created_at: 2024-09-02T12:46:26Z
updated_at: 2024-09-04T06:02:50Z
url: https://github.com/astral-sh/ruff/pull/13214
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Add debug assert to check for duplicate definitions

---

_@dhruvmanila_

## Summary

Closes: #13085

## Test Plan

`cargo insta test --workspace`


---

_Label `red-knot` added by @dhruvmanila on 2024-09-02 12:46_

---

_Marked ready for review by @dhruvmanila on 2024-09-02 12:52_

---

_Review requested from @carljm by @dhruvmanila on 2024-09-02 12:52_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-09-02 12:52_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-09-02 12:52_

---

_@MichaReiser reviewed on 2024-09-02 13:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:198 on 2024-09-02 13:30_

There are a few (or just one?) places where we call `self.definitions_by_node.extend`. Should we add the assertion there as well?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-09-02 13:42_

---

_@dhruvmanila reviewed on 2024-09-03 05:30_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:198 on 2024-09-03 05:30_

Good point. There's one more place:

https://github.com/astral-sh/ruff/blob/d88c0cc0ebe1e149d43c600d07a512662b137e71/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L320-L335

---

_Comment by @github-actions[bot] on 2024-09-03 05:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-09-03 07:49_

---

_@MichaReiser reviewed on 2024-09-03 07:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:198 on 2024-09-03 07:49_

Ohh, this is the semantic builder and not the type inference builder. But I'm glad it helped to discover another usage :laughing: 

---

_@carljm approved on 2024-09-03 14:19_

---

_Merged by @dhruvmanila on 2024-09-04 05:53_

---

_Closed by @dhruvmanila on 2024-09-04 05:53_

---

_Branch deleted on 2024-09-04 05:53_

---
