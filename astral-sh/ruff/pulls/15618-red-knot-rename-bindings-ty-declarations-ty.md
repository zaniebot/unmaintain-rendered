```yaml
number: 15618
title: "[red-knot] Rename `bindings_ty`, `declarations_ty`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/ty-renaming-2
created_at: 2025-01-20T13:53:02Z
updated_at: 2025-01-20T14:23:36Z
url: https://github.com/astral-sh/ruff/pull/15618
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Rename `bindings_ty`, `declarations_ty`

---

_Pull request opened by @sharkdp on 2025-01-20 13:53_

## Summary

Rename two functions with outdated names (they used to return `Type`s):

* `bindings_ty` => `symbol_from_bindings` (returns `Symbol`)
* `declarations_ty` => `symbol_from_declarations` (returns a `SymbolAndQualifiers` result)

I chose `symbol_from_*` instead of `*_symbol` as I found the previous name quite confusing. Especially since `binding_ty` and `declaration_ty` also exist (singular).

## Test Plan

—


---

_Label `red-knot` added by @sharkdp on 2025-01-20 13:53_

---

_Comment by @github-actions[bot] on 2025-01-20 13:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @sharkdp on 2025-01-20 14:06_

---

_Review requested from @carljm by @sharkdp on 2025-01-20 14:06_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-20 14:06_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-20 14:06_

---

_@MichaReiser approved on 2025-01-20 14:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:291 on 2025-01-20 14:12_

```suggestion
/// The type will be a union if there are multiple bindings with different types.
```

---

_@AlexWaygood approved on 2025-01-20 14:12_

---

_Merged by @sharkdp on 2025-01-20 14:23_

---

_Closed by @sharkdp on 2025-01-20 14:23_

---

_Branch deleted on 2025-01-20 14:23_

---
