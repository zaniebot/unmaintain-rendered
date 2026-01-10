```yaml
number: 20465
title: "[ty] Faster iteration on mdtests"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/faster-mdtest-1
created_at: 2025-09-18T10:44:36Z
updated_at: 2025-09-18T10:56:37Z
url: https://github.com/astral-sh/ruff/pull/20465
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Faster iteration on mdtests

---

_Pull request opened by @sharkdp on 2025-09-18 10:44_

## Summary

This change reduces MD test compilation time from 6s to 3s on my laptop. We don't need to build the unit tests and the corpus tests when we're only interested in Markdown-based tests.

## Test Plan

local benchmarks


---

_Label `internal` added by @sharkdp on 2025-09-18 10:44_

---

_Review requested from @carljm by @sharkdp on 2025-09-18 10:44_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-18 10:44_

---

_Review requested from @dcreager by @sharkdp on 2025-09-18 10:44_

---

_Label `ty` added by @sharkdp on 2025-09-18 10:44_

---

_Comment by @github-actions[bot] on 2025-09-18 10:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood approved on 2025-09-18 10:46_

🚀

---

_Merged by @sharkdp on 2025-09-18 10:48_

---

_Closed by @sharkdp on 2025-09-18 10:48_

---

_Branch deleted on 2025-09-18 10:48_

---

_Comment by @github-actions[bot] on 2025-09-18 10:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---
