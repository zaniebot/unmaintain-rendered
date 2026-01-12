```yaml
number: 19197
title: "[ty] Conditionally defined dataclass fields"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/conditionally-defined-fields
created_at: 2025-07-08T08:26:23Z
updated_at: 2025-07-08T14:16:52Z
url: https://github.com/astral-sh/ruff/pull/19197
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Conditionally defined dataclass fields

---

_@sharkdp_

## Summary

Fixes a bug where conditionally defined dataclass fields were previously ignored.

Thanks to @lipefree for reporting this.

## Test Plan

New Markdown tests

---

_Review requested from @carljm by @sharkdp on 2025-07-08 08:26_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-08 08:26_

---

_Review requested from @dcreager by @sharkdp on 2025-07-08 08:26_

---

_Label `ty` added by @sharkdp on 2025-07-08 08:26_

---

_@sharkdp reviewed on 2025-07-08 08:28_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1653 on 2025-07-08 08:28_

In a situation like the following, there is not just one declaration of `x`. There's also an implicit `x: <undefined>` declaration at the start of the `C` scope. Both of them are visible at the end of the scope. The previous check here was filtering out all fields that had declarations that were `<undefined>`.
```py
@dataclass
class C:
    if flag:
        x: int
```

---

_Comment by @github-actions[bot] on 2025-07-08 08:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-07-08 13:53_

---

_Merged by @sharkdp on 2025-07-08 14:16_

---

_Closed by @sharkdp on 2025-07-08 14:16_

---

_Branch deleted on 2025-07-08 14:16_

---
