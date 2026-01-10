```yaml
number: 20445
title: "[ty] support type aliases in binary compares"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/aliasbinop
created_at: 2025-09-17T01:26:40Z
updated_at: 2025-09-17T07:33:28Z
url: https://github.com/astral-sh/ruff/pull/20445
synced_at: 2026-01-10T17:40:28Z
```

# [ty] support type aliases in binary compares

---

_Pull request opened by @carljm on 2025-09-17 01:26_

## Summary

Add missing `Type::TypeAlias` clauses to `infer_binary_type_comparison`.

## Test Plan

Added mdtests that failed before.


---

_Label `ty` added by @carljm on 2025-09-17 01:26_

---

_Comment by @github-actions[bot] on 2025-09-17 01:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-17 01:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-09-17 01:59_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-17 01:59_

---

_Review requested from @sharkdp by @carljm on 2025-09-17 01:59_

---

_Review requested from @dcreager by @carljm on 2025-09-17 01:59_

---

_@sharkdp approved on 2025-09-17 07:32_

---

_Merged by @sharkdp on 2025-09-17 07:33_

---

_Closed by @sharkdp on 2025-09-17 07:33_

---

_Branch deleted on 2025-09-17 07:33_

---
