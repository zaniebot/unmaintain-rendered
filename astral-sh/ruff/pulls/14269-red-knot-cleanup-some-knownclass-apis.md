```yaml
number: 14269
title: "[red-knot] Cleanup some `KnownClass` APIs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/knownclass-api
created_at: 2024-11-11T11:50:03Z
updated_at: 2024-11-11T12:03:53Z
url: https://github.com/astral-sh/ruff/pull/14269
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Cleanup some `KnownClass` APIs

---

_Pull request opened by @AlexWaygood on 2024-11-11 11:50_

## Summary

- Rename `KnownClass::to_class` to `KnownClass::to_class_literal`. This is an oversight that should have been done as part of #14108.
- Rename KnownClass::maybe_from_module` to `KnownClass::try_from_module`. I think this fits better with typical Rust naming conventions.
- Inline `KnownClass::from_name`. I don't think it would ever produce desirable results if you called this method from outside of `KnownClass::try_from_module` (because you also need to check which module the class is coming from, for correctness), so inlining it seems best here.
- Make use of the `CoreStdlibModule` enum in `red_knot_python_semantic/src/stdlib.rs` to reduce code duplication and reduce hardcoded strings.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-11-11 11:50_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-11 11:50_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-11 11:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-11 11:50_

---

_Merged by @AlexWaygood on 2024-11-11 11:54_

---

_Closed by @AlexWaygood on 2024-11-11 11:54_

---

_Branch deleted on 2024-11-11 11:54_

---

_Comment by @github-actions[bot] on 2024-11-11 12:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
