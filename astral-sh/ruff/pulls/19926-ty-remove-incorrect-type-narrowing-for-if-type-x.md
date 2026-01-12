```yaml
number: 19926
title: "[ty] Remove incorrect type narrowing for `if type(x) is C[int]`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/type-narrow-genericalias
created_at: 2025-08-15T08:31:40Z
updated_at: 2025-08-15T16:52:16Z
url: https://github.com/astral-sh/ruff/pull/19926
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Remove incorrect type narrowing for `if type(x) is C[int]`

---

_@AlexWaygood_

## Summary

For this code, we currently apply incorrect type narrowing:

```py
class A[T]: ...
class B: ...

def f(x: A[int] | B):
    if type(x) is A[int]:
        reveal_type(x)  # revealed: (A[int] & A[Unknown]) | (B & A[Unknown])
    else:
        reveal_type(x)  # revealed: A[int] | B
```

`type(x)` will either be a subclass of `A` or a subclass of `B` at runtime, but `A[int]` isn't either of those things (it's not a class at all, it's a generic alias), so we shouldn't be doing any type narrowing at all if we see a pattern such as `if type(x) is A[int]`. (Well, we _could_ narrow the type to `Never`, since the condition will always resolve to `False`!)

Note that there is another pre-existing bug here that is not fixed by this PR:

```py
class A[T]: ...
class B: ...

def f(x: A[int] | B):
    if type(x) is A:
        reveal_type(x)  # revealed: Never
    else:
        reveal_type(x)  # revealed: A[int] | B
```

I'm not totally sure why we reveal `Never` for the first branch there. I tried quickly to fix it, but couldn't immediately figure it out, and it's a pre-existing issue so for now I just left it; it doesn't seem high-priority.

## Test Plan

Mdtests


---

_Label `ty` added by @AlexWaygood on 2025-08-15 08:31_

---

_Comment by @github-actions[bot] on 2025-08-15 08:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-15 16:40:51.992493042 +0000
+++ new-output.txt	2025-08-15 16:40:52.061493541 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(120b2)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(40bb9)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-15 08:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-08-15 08:35_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-15 08:35_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-15 08:35_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-15 08:35_

---

_Label `bug` added by @AlexWaygood on 2025-08-15 08:36_

---

_@carljm approved on 2025-08-15 16:41_

Thanks!

---

_Merged by @AlexWaygood on 2025-08-15 16:52_

---

_Closed by @AlexWaygood on 2025-08-15 16:52_

---

_Branch deleted on 2025-08-15 16:52_

---
