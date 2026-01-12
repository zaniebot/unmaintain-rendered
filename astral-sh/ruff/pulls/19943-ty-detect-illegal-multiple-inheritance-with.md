```yaml
number: 19943
title: "[ty] Detect illegal multiple inheritance with `NamedTuple`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/namedtuple-multiple-inheritance
created_at: 2025-08-16T19:01:32Z
updated_at: 2025-08-18T12:03:02Z
url: https://github.com/astral-sh/ruff/pull/19943
synced_at: 2026-01-12T15:56:51Z
```

# [ty] Detect illegal multiple inheritance with `NamedTuple`

---

_@AlexWaygood_

## Summary

This fails at runtime:

```py
from typing import NamedTuple

class Foo(NamedTuple, object): ...
```

and the typing spec mandates that we should detect that, but we currently don't. This PR adds a new lint for detecting this mistake.

## Test Plan

Mdtest/snapshots


---

_Label `ty` added by @AlexWaygood on 2025-08-16 19:01_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-16 19:01_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-16 19:01_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-16 19:01_

---

_Comment by @github-actions[bot] on 2025-08-16 19:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-18 12:00:59.100097702 +0000
+++ new-output.txt	2025-08-18 12:01:01.560109174 +0000
@@ -672,6 +672,7 @@
 namedtuples_define_class.py:95:1: error[type-assertion-failure] Argument does not have asserted type `int | float`
 namedtuples_define_class.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int | float`
 namedtuples_define_class.py:98:19: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `float`
+namedtuples_define_class.py:105:24: error[invalid-named-tuple] NamedTuple class `Unit` cannot use multiple inheritance except with `Generic[]`
 namedtuples_type_compat.py:22:1: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, int]`
 namedtuples_type_compat.py:23:1: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, str, str]`
 namedtuples_usage.py:34:7: error[index-out-of-bounds] Index 3 is out of bounds for tuple `Point` with length 3
@@ -860,5 +861,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 861 diagnostics
+Found 862 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-16 19:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-16 19:14_

(The new typing conformance suite diagnostic is correct!)

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-16 19:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:458 on 2025-08-18 11:48_

Maybe

```suggestion
    /// An invalidly defined `NamedTuple` class may lead to the type checker
    /// drawing wrong conclusions. It may also lead to `TypeError`s at runtime.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:477 on 2025-08-18 11:49_

```suggestion
    /// ```pycon
    /// >>> from typing import NamedTuple
    /// >>> class Foo(NamedTuple, object): ...
    /// TypeError: can only inherit from a NamedTuple type and Generic
    /// ```
```

---

_@sharkdp approved on 2025-08-18 11:49_

Nice — thank you.

---

_@AlexWaygood reviewed on 2025-08-18 11:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:458 on 2025-08-18 11:51_

```suggestion
    /// An invalidly defined `NamedTuple` class may lead to the type checker
    /// drawing incorrect conclusions. It may also lead to `TypeError`s at runtime.
```

---

_Merged by @AlexWaygood on 2025-08-18 12:03_

---

_Closed by @AlexWaygood on 2025-08-18 12:03_

---

_Branch deleted on 2025-08-18 12:03_

---
