```yaml
number: 20577
title: "[ty] Update salsa to fix hang when cycle head panics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-panic-bugfix
created_at: 2025-09-25T14:30:13Z
updated_at: 2025-09-25T15:13:09Z
url: https://github.com/astral-sh/ruff/pull/20577
synced_at: 2026-01-12T15:57:05Z
```

# [ty] Update salsa to fix hang when cycle head panics

---

_@MichaReiser_

## Summary

Update Salsa to pull in https://github.com/salsa-rs/salsa/pull/993

## Test Plan

Tested that mypy primer no longer hangs for https://github.com/astral-sh/ruff/pull/20477 when using the new version of Salsa


---

_Label `bug` added by @MichaReiser on 2025-09-25 14:30_

---

_Label `ty` added by @MichaReiser on 2025-09-25 14:30_

---

_Comment by @github-actions[bot] on 2025-09-25 14:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-25 14:52:12.484169514 +0000
+++ new-output.txt	2025-09-25 14:52:15.667168866 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b816)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1327a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b816)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1327a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-25 14:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-09-25 14:51_

---

_Review requested from @carljm by @MichaReiser on 2025-09-25 14:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-09-25 14:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-09-25 14:51_

---

_@MichaReiser reviewed on 2025-09-25 14:51_

---

_Review comment by @MichaReiser on `crates/ty/src/logging.rs`:126 on 2025-09-25 14:51_

Salsa now logs a tracing message with severity `warn` if it hits a cycle that doesn't converge. I don't think we want to show that by default.

---

_@carljm approved on 2025-09-25 14:53_

---

_Comment by @github-actions[bot] on 2025-09-25 15:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2025-09-25 15:13_

---

_Closed by @MichaReiser on 2025-09-25 15:13_

---

_Branch deleted on 2025-09-25 15:13_

---
