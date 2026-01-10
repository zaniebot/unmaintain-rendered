```yaml
number: 19941
title: "[ty] Ban protocols from inheriting from non-protocol generic classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/genericalias-proto-inherit
created_at: 2025-08-16T18:34:25Z
updated_at: 2025-08-16T18:38:45Z
url: https://github.com/astral-sh/ruff/pull/19941
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Ban protocols from inheriting from non-protocol generic classes

---

_Pull request opened by @AlexWaygood on 2025-08-16 18:34_

## Summary

This is extracted from #19866, since I need to do some debugging on that PR but this makes sense as a standalone fix.

Currently we correctly emit a diagnostic on this class, which will fail at runtime (a protocol class cannot inherit from a non-protocol class, and `int` is not a protocol class):

```py
from typing import Protocol

class Foo(int, Protocol): ...
```

but we don't emit a diagnostic on this class (even though it also fails at runtime):

```py
from typing import Protocol

class Foo(list[int], Protocol): ...
```

The reason for this is that `list[int]` is represented in our model as a `Type::GenericAlias` rather than a `Type::ClassLiteral`. This PR fixes that oversight.

## Test Plan

mdtest added


---

_Review requested from @carljm by @AlexWaygood on 2025-08-16 18:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-16 18:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-16 18:34_

---

_Label `bug` added by @AlexWaygood on 2025-08-16 18:34_

---

_Label `ty` added by @AlexWaygood on 2025-08-16 18:34_

---

_Comment by @github-actions[bot] on 2025-08-16 18:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-16 18:36:07.657484137 +0000
+++ new-output.txt	2025-08-16 18:36:07.723484848 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(2fc7e)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(c0bb)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-16 18:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-08-16 18:38_

---

_Closed by @AlexWaygood on 2025-08-16 18:38_

---

_Branch deleted on 2025-08-16 18:38_

---
