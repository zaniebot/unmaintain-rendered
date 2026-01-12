```yaml
number: 20262
title: "[ty] Add backreferences to TypedDict items in diagnostics"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/item-backreferences
created_at: 2025-09-05T08:43:52Z
updated_at: 2025-09-05T10:38:39Z
url: https://github.com/astral-sh/ruff/pull/20262
synced_at: 2026-01-12T15:56:57Z
```

# [ty] Add backreferences to TypedDict items in diagnostics

---

_@sharkdp_

## Summary

Add backreferences to the original item declaration in TypedDict diagnostics:

<img width="632" height="415" alt="image" src="https://github.com/user-attachments/assets/ee19808b-7791-4754-aee7-08d51885031e" />

<img width="917" height="412" alt="image" src="https://github.com/user-attachments/assets/33849d2f-db36-495e-9abf-29b2e8731fce" />


Thanks to @AlexWaygood for the suggestion.

## Test Plan

Updated snapshots

---

_Review requested from @carljm by @sharkdp on 2025-09-05 08:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-05 08:43_

---

_Review requested from @dcreager by @sharkdp on 2025-09-05 08:43_

---

_Label `internal` added by @sharkdp on 2025-09-05 08:43_

---

_Label `ty` added by @sharkdp on 2025-09-05 08:43_

---

_Label `diagnostics` added by @sharkdp on 2025-09-05 08:43_

---

_Comment by @github-actions[bot] on 2025-09-05 08:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-05 08:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1286 on 2025-09-05 09:09_

Oh, nice. It might be possible to simplify this `NamedTuple` subdiagnostic using this new field:

https://github.com/astral-sh/ruff/blob/9e45bfa9fd730dc6a95bd94ced51147cc507cfe6/crates/ty_python_semantic/src/types/diagnostic.rs#L2914-L2938

---

_@AlexWaygood approved on 2025-09-05 09:09_

---

_@sharkdp reviewed on 2025-09-05 09:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1286 on 2025-09-05 09:49_

Indeed! Would you mind taking a short look?

---

_Review requested from @MichaReiser by @sharkdp on 2025-09-05 09:54_

---

_@AlexWaygood approved on 2025-09-05 10:18_

Love it when we can add functionality while deleting LOC 😁

---

_Merged by @sharkdp on 2025-09-05 10:38_

---

_Closed by @sharkdp on 2025-09-05 10:38_

---

_Branch deleted on 2025-09-05 10:38_

---
