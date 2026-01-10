```yaml
number: 20446
title: "[ty] detect cycles in binary comparison inference"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/binopcycle
created_at: 2025-09-17T01:59:14Z
updated_at: 2025-09-17T07:45:27Z
url: https://github.com/astral-sh/ruff/pull/20446
synced_at: 2026-01-10T17:40:28Z
```

# [ty] detect cycles in binary comparison inference

---

_Pull request opened by @carljm on 2025-09-17 01:59_

## Summary

Catch infinite recursion in binary-compare inference.

Fixes the stack overflow in `graphql-core` in mypy-primer.

## Test Plan

Added two tests that stack-overflowed before this PR.


---

_Label `ty` added by @carljm on 2025-09-17 01:59_

---

_Comment by @github-actions[bot] on 2025-09-17 02:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-17 02:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-09-17 02:11_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-17 02:11_

---

_Review requested from @sharkdp by @carljm on 2025-09-17 02:11_

---

_Review requested from @dcreager by @carljm on 2025-09-17 02:11_

---

_@AlexWaygood approved on 2025-09-17 06:17_

---

_Merged by @sharkdp on 2025-09-17 07:45_

---

_Closed by @sharkdp on 2025-09-17 07:45_

---

_Branch deleted on 2025-09-17 07:45_

---
