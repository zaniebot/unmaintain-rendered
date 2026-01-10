```yaml
number: 20627
title: "[ty] Use fully qualified names to distinguish ambiguous protocols in diagnostics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/protocol-instance-qualified
created_at: 2025-09-29T11:54:58Z
updated_at: 2025-09-29T12:02:08Z
url: https://github.com/astral-sh/ruff/pull/20627
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Use fully qualified names to distinguish ambiguous protocols in diagnostics

---

_Pull request opened by @AlexWaygood on 2025-09-29 11:54_

## Summary

A minor followup to https://github.com/astral-sh/ruff/pull/20603. I'm adding the `internal` label as I don't think it warrants an additional changelog entry

## Test Plan

Added an mdtest that fails on `main`


---

_Review requested from @carljm by @AlexWaygood on 2025-09-29 11:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-29 11:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-29 11:54_

---

_Label `internal` added by @AlexWaygood on 2025-09-29 11:55_

---

_Label `ty` added by @AlexWaygood on 2025-09-29 11:55_

---

_Label `diagnostics` added by @AlexWaygood on 2025-09-29 11:55_

---

_Comment by @github-actions[bot] on 2025-09-29 11:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@sharkdp approved on 2025-09-29 11:58_

Thank you!

---

_Comment by @github-actions[bot] on 2025-09-29 12:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-09-29 12:02_

---

_Closed by @AlexWaygood on 2025-09-29 12:02_

---

_Branch deleted on 2025-09-29 12:02_

---
