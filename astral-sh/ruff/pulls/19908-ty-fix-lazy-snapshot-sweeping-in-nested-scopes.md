```yaml
number: 19908
title: "[ty] fix lazy snapshot sweeping in nested scopes"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-lazy-nested-narrowing
created_at: 2025-08-14T05:58:52Z
updated_at: 2025-08-15T01:28:33Z
url: https://github.com/astral-sh/ruff/pull/19908
synced_at: 2026-01-12T15:56:50Z
```

# [ty] fix lazy snapshot sweeping in nested scopes

---

_@mtshiba_

## Summary

This PR closes astral-sh/ty#955.

## Test Plan

New test cases in `narrowing/conditionals/nested.md`.


---

_Review requested from @carljm by @mtshiba on 2025-08-14 05:58_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-08-14 05:58_

---

_Review requested from @sharkdp by @mtshiba on 2025-08-14 05:58_

---

_Review requested from @dcreager by @mtshiba on 2025-08-14 05:58_

---

_Comment by @github-actions[bot] on 2025-08-14 06:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 06:00:34.776618614 +0000
+++ new-output.txt	2025-08-14 06:00:34.846619163 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1da61)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(50b5)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-14 06:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/hooks.py:66:29: warning[redundant-cast] Value is already of type `str`
- Found 271 diagnostics
+ Found 270 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@mtshiba reviewed on 2025-08-14 06:14_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:249 on 2025-08-14 06:14_

Upon closer analysis, it seems that whether or not narrowing is applied to `x` depends on whether the closure is exposed or just consumed in the scope (This PR does not take this into account and always assumes that the closure is exposed).

```python
def f(x: str | None):
    def _():
        if x is not None:
            def closure():
                reveal_type(x)  # revealed: str | None
            return closure  # expose a closure
    x = None

def f(x: str | None):
    def _():
        if x is not None:
            def closure():
                reveal_type(x)  # revealed: str
            return closure()  # just consume a closure
    x = None
```

To achieve this, an additional mechanism would be required to determine whether a closure is exposed (pyright does not seem to implement this). This would likely be a future work.

---

_Label `ty` added by @AlexWaygood on 2025-08-14 12:39_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:249 on 2025-08-15 00:49_

Yes, I think in general we consider escape analysis for closures to be out of scope. Perhaps we could tackle it someday, but it's not needed now.

---

_@carljm approved on 2025-08-15 00:52_

Looks great, thank you!

---

_Merged by @carljm on 2025-08-15 00:52_

---

_Closed by @carljm on 2025-08-15 00:52_

---

_Branch deleted on 2025-08-15 01:28_

---
