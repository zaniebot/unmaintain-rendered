```yaml
number: 20956
title: "[ty] Fix panic when attempting to validate the members of a protocol that inherits from a protocol in another module"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/proto-panic
created_at: 2025-10-18T13:35:56Z
updated_at: 2025-10-18T14:01:48Z
url: https://github.com/astral-sh/ruff/pull/20956
synced_at: 2026-01-12T15:57:13Z
```

# [ty] Fix panic when attempting to validate the members of a protocol that inherits from a protocol in another module

---

_@AlexWaygood_

## Summary

This fixes a panic that could occur when trying to validate a protocol's members, if the protocol had unannotated assignments in its class body and inherited from a protocol that was defined in another module. We were attempting to lookup the superclass protocol in the wrong file, which could lead to panics and incorrect behaviour.

## Test Plan

Added an mdtest that causes us to panic on `main`


---

_Label `bug` added by @AlexWaygood on 2025-10-18 13:35_

---

_Label `ty` added by @AlexWaygood on 2025-10-18 13:35_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-18 13:35_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-18 13:35_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-18 13:35_

---

_Comment by @github-actions[bot] on 2025-10-18 13:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-18 13:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@MichaReiser approved on 2025-10-18 13:59_

---

_Merged by @AlexWaygood on 2025-10-18 14:01_

---

_Closed by @AlexWaygood on 2025-10-18 14:01_

---

_Branch deleted on 2025-10-18 14:01_

---
