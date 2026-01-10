```yaml
number: 19597
title: "[ty] Minor: test isolation"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/separate-tests
created_at: 2025-07-28T12:54:41Z
updated_at: 2025-07-28T13:53:01Z
url: https://github.com/astral-sh/ruff/pull/19597
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Minor: test isolation

---

_Pull request opened by @sharkdp on 2025-07-28 12:54_

## Summary

Split the "Generator functions" tests into two parts. The first part (synchronous) refers to a function called `i` from a function `i2`. But `i` is later redeclared in the asynchronous part, which was probably not intended.

---

_Review requested from @carljm by @sharkdp on 2025-07-28 12:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-28 12:54_

---

_Review requested from @dcreager by @sharkdp on 2025-07-28 12:54_

---

_Label `internal` added by @sharkdp on 2025-07-28 12:54_

---

_Label `testing` added by @sharkdp on 2025-07-28 12:54_

---

_Label `ty` added by @sharkdp on 2025-07-28 12:54_

---

_Comment by @github-actions[bot] on 2025-07-28 12:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-28 12:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-07-28 13:01_

---

_Merged by @sharkdp on 2025-07-28 13:52_

---

_Closed by @sharkdp on 2025-07-28 13:52_

---

_Branch deleted on 2025-07-28 13:53_

---
