```yaml
number: 19480
title: "[ty] Disallow `Final` in function parameter/return-type annotations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/disallow-final-in-parameters
created_at: 2025-07-22T08:31:20Z
updated_at: 2025-07-22T11:15:21Z
url: https://github.com/astral-sh/ruff/pull/19480
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Disallow `Final` in function parameter/return-type annotations

---

_@sharkdp_

## Summary

Disallow `Final` in function parameter- and return-type annotations.

[Typing spec](https://typing.python.org/en/latest/spec/qualifiers.html#uppercase-final):

> `Final` may only be used in assignments or variable annotations. Using it in any other position is an error. In particular, `Final` can’t be used in annotations for function arguments

## Test Plan

Updated MD test


---

_Label `ty` added by @sharkdp on 2025-07-22 08:31_

---

_Review requested from @carljm by @sharkdp on 2025-07-22 08:31_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-22 08:31_

---

_Review requested from @dcreager by @sharkdp on 2025-07-22 08:31_

---

_Comment by @github-actions[bot] on 2025-07-22 08:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:2698 on 2025-07-22 09:23_

I believe the same goes for `ClassVar` FWIW; we could add that rule as well, while we're here?

---

_@AlexWaygood approved on 2025-07-22 09:23_

---

_Converted to draft by @sharkdp on 2025-07-22 10:04_

---

_Renamed from "[ty] Disallow `Final` in function parameter annotations" to "[ty] Disallow `Final` in function parameter/return-type annotations" by @sharkdp on 2025-07-22 11:07_

---

_@sharkdp reviewed on 2025-07-22 11:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:2698 on 2025-07-22 11:12_

> I believe the same goes for `ClassVar` FWIW; we could add that rule as well, while we're here?

ClassVar should be disallowed in many more positions as well. I'll write up a ticket. Seems like a good beginner task?

---

_Marked ready for review by @sharkdp on 2025-07-22 11:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:2698 on 2025-07-22 11:13_

Sounds good!

---

_@AlexWaygood reviewed on 2025-07-22 11:13_

---

_@AlexWaygood approved on 2025-07-22 11:14_

---

_Merged by @sharkdp on 2025-07-22 11:15_

---

_Closed by @sharkdp on 2025-07-22 11:15_

---

_Branch deleted on 2025-07-22 11:15_

---
