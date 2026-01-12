```yaml
number: 18249
title: "[ty] Implement Python's floor division semantics for `Literal` `int`s"
type: pull_request
state: merged
author: brandtbucher
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: floor-division
created_at: 2025-05-22T06:24:50Z
updated_at: 2025-05-22T14:42:30Z
url: https://github.com/astral-sh/ruff/pull/18249
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Implement Python's floor division semantics for `Literal` `int`s

---

_@brandtbucher_

Division works differently in Python than in Rust. If the result is negative and there is a remainder, the division rounds down (instead of towards zero). The remainder needs to be adjusted to compensate so that `(lhs // rhs) * rhs + (lhs % rhs) == lhs`.

Fixes astral-sh/ty#481. @AlexWaygood.

---

_Review requested from @carljm by @brandtbucher on 2025-05-22 06:24_

---

_Review requested from @AlexWaygood by @brandtbucher on 2025-05-22 06:24_

---

_Review requested from @sharkdp by @brandtbucher on 2025-05-22 06:24_

---

_Review requested from @dcreager by @brandtbucher on 2025-05-22 06:24_

---

_Renamed from "[ty] Implement Python's floor division semantics for `Literal` ints" to "[ty] Implement Python's floor division semantics for `Literal` `int`s" by @brandtbucher on 2025-05-22 06:25_

---

_Comment by @github-actions[bot] on 2025-05-22 06:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `bug` added by @AlexWaygood on 2025-05-22 12:06_

---

_Label `ty` added by @AlexWaygood on 2025-05-22 12:06_

---

_@carljm approved on 2025-05-22 14:42_

Looks good! Verified the asserted results in tests match actual runtime results, and the code looks right. Thank you!

---

_Merged by @carljm on 2025-05-22 14:42_

---

_Closed by @carljm on 2025-05-22 14:42_

---
