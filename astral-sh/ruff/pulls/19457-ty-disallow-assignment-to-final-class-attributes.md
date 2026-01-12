```yaml
number: 19457
title: "[ty] Disallow assignment to `Final` class attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/final-attributes
created_at: 2025-07-21T10:59:07Z
updated_at: 2025-07-21T12:27:57Z
url: https://github.com/astral-sh/ruff/pull/19457
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Disallow assignment to `Final` class attributes

---

_@sharkdp_

## Summary

Emit errors for the following assignments:
```py
class C:
    CLASS_LEVEL_CONSTANT: Final[int] = 1

C.CLASS_LEVEL_CONSTANT = 2
C().CLASS_LEVEL_CONSTANT = 2
```

## Test Plan

Updated and new MD tests

---

_Label `ty` added by @sharkdp on 2025-07-21 10:59_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-21 10:59_

---

_Comment by @github-actions[bot] on 2025-07-21 11:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-21 11:03_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-21 11:03_

---

_Marked ready for review by @sharkdp on 2025-07-21 11:03_

---

_Review requested from @carljm by @sharkdp on 2025-07-21 11:03_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-21 11:03_

---

_Review requested from @dcreager by @sharkdp on 2025-07-21 11:03_

---

_@sharkdp reviewed on 2025-07-21 11:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:203 on 2025-07-21 11:03_

Implementing this for implicit instance attributes is much more involved, so I'm putting this PR up as a first increment.

---

_Comment by @github-actions[bot] on 2025-07-21 11:11_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No changes detected ✅
**[Full report with detailed diff](https://david-final-attributes.ecosystem-663.pages.dev/diff)**


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:3366 on 2025-07-21 11:13_

```suggestion
        // Emit a diagnostic if a `Final` attribute was assigned to and `emit_diagnostics` is `true`.
        // Return a boolean value that indicates whether the attribute was `Final`.
        // Note that the value returned may be `true` even if no diagnostic was emitted.
        let check_assignment_to_final = |qualifiers: TypeQualifiers| -> bool {
```

---

_@AlexWaygood reviewed on 2025-07-21 11:14_

---

_@AlexWaygood approved on 2025-07-21 11:14_

---

_@sharkdp reviewed on 2025-07-21 12:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:3366 on 2025-07-21 12:13_

Thank you. I went with something a bit easier here (I'd rather skip over the `emit_diagnostics` subtlety), but also renamed the closure to `invalid_assignment_to_final` to make it more clear what's happening at the call sites.

---

_Merged by @sharkdp on 2025-07-21 12:27_

---

_Closed by @sharkdp on 2025-07-21 12:27_

---

_Branch deleted on 2025-07-21 12:27_

---
