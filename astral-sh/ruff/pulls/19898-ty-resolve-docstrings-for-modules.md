```yaml
number: 19898
title: "[ty] resolve docstrings for modules"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/moduledoc
created_at: 2025-08-13T16:00:33Z
updated_at: 2025-08-13T16:24:03Z
url: https://github.com/astral-sh/ruff/pull/19898
synced_at: 2026-01-12T15:56:49Z
```

# [ty] resolve docstrings for modules

---

_@Gankra_

This also reintroduces the `ResolvedDefinition::Module` variant because reverse-engineering it in several places is a bit confusing. In an ideal world we wouldn't have `ResolvedDefinition::FileWithRange` as it kinda kills the ability to do richer analysis, so I want to chip away at its scope wherever I can (currently it's used to point at asname parts of import statements when doing `ImportAliasResolution::PreserveAliases`, and also keyword arguments).

This also makes a kind of odd change to allow a hover to *only* produce a docstring. This works around an oddity where hovering over a module name in an import fails to resolve to a `ty` even though hovering over uses of that imported name *does*.

The two fixed tests reflect the two interesting cases here.

---

_Review requested from @carljm by @Gankra on 2025-08-13 16:00_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-13 16:00_

---

_Review requested from @sharkdp by @Gankra on 2025-08-13 16:00_

---

_Review requested from @dcreager by @Gankra on 2025-08-13 16:00_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-13 16:00_

---

_Label `server` added by @Gankra on 2025-08-13 16:00_

---

_Label `ty` added by @Gankra on 2025-08-13 16:00_

---

_Comment by @Gankra on 2025-08-13 16:01_

<img width="585" height="93" alt="Screenshot 2025-08-13 at 12 01 04 PM" src="https://github.com/user-attachments/assets/d39adf19-0732-4e43-ac9e-b861c0da9179" />
<img width="427" height="73" alt="Screenshot 2025-08-13 at 12 01 14 PM" src="https://github.com/user-attachments/assets/e1f95768-3946-4d5a-b52e-890180454346" />
<img width="749" height="68" alt="Screenshot 2025-08-13 at 12 01 25 PM" src="https://github.com/user-attachments/assets/ca026264-d789-4593-96ab-31330a82828c" />


---

_Comment by @github-actions[bot] on 2025-08-13 16:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 16:02:21.506598690 +0000
+++ new-output.txt	2025-08-13 16:02:21.573598867 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(70bc)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1b8bf)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-13 16:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-08-13 16:21_

---

_Merged by @Gankra on 2025-08-13 16:24_

---

_Closed by @Gankra on 2025-08-13 16:24_

---

_Branch deleted on 2025-08-13 16:24_

---
