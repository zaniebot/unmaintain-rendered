```yaml
number: 21016
title: "[ty] Disable panicking mdtest"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/revert-tuple-panic-test
created_at: 2025-10-21T12:29:21Z
updated_at: 2025-10-21T12:40:25Z
url: https://github.com/astral-sh/ruff/pull/21016
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Disable panicking mdtest

---

_Pull request opened by @sharkdp on 2025-10-21 12:29_

## Summary

Only run the "pull types" test after performing the "actual" mdtest. We observed that the order matters. There is currently one mdtest which panics when checked in the CLI or the playground. With this change, it also panics in the mdtest suite.

reopens https://github.com/astral-sh/ty/issues/837?

---

_Review requested from @carljm by @sharkdp on 2025-10-21 12:29_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-21 12:29_

---

_Review requested from @dcreager by @sharkdp on 2025-10-21 12:29_

---

_Review requested from @MichaReiser by @sharkdp on 2025-10-21 12:29_

---

_Label `testing` added by @sharkdp on 2025-10-21 12:29_

---

_Label `ty` added by @sharkdp on 2025-10-21 12:29_

---

_Comment by @github-actions[bot] on 2025-10-21 12:31_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-21 12:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-10-21 12:34_

---

_@AlexWaygood approved on 2025-10-21 12:35_

---

_Merged by @sharkdp on 2025-10-21 12:40_

---

_Closed by @sharkdp on 2025-10-21 12:40_

---

_Branch deleted on 2025-10-21 12:40_

---
