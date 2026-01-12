```yaml
number: 18135
title: "[ty] Merge `SemanticIndexBuilder` impl blocks"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/builder-merge-impls
created_at: 2025-05-16T14:38:29Z
updated_at: 2025-05-16T15:05:04Z
url: https://github.com/astral-sh/ruff/pull/18135
synced_at: 2026-01-12T15:56:13Z
```

# [ty] Merge `SemanticIndexBuilder` impl blocks

---

_@AlexWaygood_

## Summary

just a minor nit followup to https://github.com/astral-sh/ruff/pull/18010 -- put all the non-`Visitor` methods of `SemanticIndexBuilder` in the same impl block rather than having multiple impl blocks

## Test Plan

`cargo build`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-16 14:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-16 14:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-16 14:38_

---

_Label `internal` added by @AlexWaygood on 2025-05-16 14:38_

---

_Label `ty` added by @AlexWaygood on 2025-05-16 14:38_

---

_Comment by @github-actions[bot] on 2025-05-16 14:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-05-16 15:03_

---

_Merged by @AlexWaygood on 2025-05-16 15:05_

---

_Closed by @AlexWaygood on 2025-05-16 15:05_

---

_Branch deleted on 2025-05-16 15:05_

---
