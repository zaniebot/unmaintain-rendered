```yaml
number: 15608
title: "[red-knot] Move `Ty` enum to property tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/move-ty-enum
created_at: 2025-01-20T09:01:55Z
updated_at: 2025-01-20T09:15:38Z
url: https://github.com/astral-sh/ruff/pull/15608
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Move `Ty` enum to property tests

---

_@sharkdp_

## Summary

Move the `Ty` enum into the `property_tests` module, as it was only used in a single place in `types.rs`. We don't *have* to do this (we *might* want to add more Rust-based unit tests in `types.rs` in the future that use `Ty` again?), but if we only use it in the property tests, this feels a bit cleaner and allows us to tailor `Ty` to the specific use case of generating `Arbitrary` types (discussed some ideas with @AlexWaygood recently).

## Test Plan

—

---

_Label `internal` added by @sharkdp on 2025-01-20 09:01_

---

_Label `red-knot` added by @sharkdp on 2025-01-20 09:01_

---

_Review requested from @carljm by @sharkdp on 2025-01-20 09:01_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-20 09:01_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-20 09:01_

---

_Comment by @github-actions[bot] on 2025-01-20 09:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-01-20 09:13_

---

_Merged by @sharkdp on 2025-01-20 09:15_

---

_Closed by @sharkdp on 2025-01-20 09:15_

---

_Branch deleted on 2025-01-20 09:15_

---
