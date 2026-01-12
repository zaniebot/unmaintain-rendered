```yaml
number: 21697
title: "[ty] Emit `invalid-named-tuple` on namedtuple classes that have field names starting with underscores"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/private-namedtuple-field
created_at: 2025-11-29T18:07:23Z
updated_at: 2025-12-01T11:36:03Z
url: https://github.com/astral-sh/ruff/pull/21697
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Emit `invalid-named-tuple` on namedtuple classes that have field names starting with underscores

---

_@AlexWaygood_

## Summary

This raises `TypeError` at runtime and is caught by all other major type checkers

## Test Plan

snapshot/mdtest


---

_Label `ty` added by @AlexWaygood on 2025-11-29 18:07_

---

_Comment by @astral-sh-bot[bot] on 2025-11-29 18:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-01 11:33:22.595865886 +0000
+++ new-output.txt	2025-12-01 11:33:26.305906382 +0000
@@ -752,6 +752,7 @@
 namedtuples_define_class.py:48:22: error[too-many-positional-arguments] Too many positional arguments: expected 4, got 5
 namedtuples_define_class.py:49:23: error[unknown-argument] Argument `other` does not match any known parameter
 namedtuples_define_class.py:69:20: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
+namedtuples_define_class.py:76:5: error[invalid-named-tuple] NamedTuple field `_y` cannot start with an underscore
 namedtuples_define_class.py:86:5: error[invalid-named-tuple] NamedTuple field without default value cannot follow field(s) with default value(s): Field `latitude` defined here without a default value
 namedtuples_define_class.py:121:1: error[type-assertion-failure] Type `Property[int | float]` does not match asserted type `Property[float]`
 namedtuples_define_class.py:122:1: error[type-assertion-failure] Type `int | float` does not match asserted type `float`
@@ -1039,4 +1040,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1041 diagnostics
+Found 1042 diagnostics

```

</details>




---

_Comment by @AlexWaygood on 2025-11-29 18:10_

The new typing-conformance diagnostic is a true positive.

---

_Renamed from "[ty] Emit `invalid-namedtuple` on namedtuple classes that have field names starting with leading underscrores" to "[ty] Emit `invalid-namedtuple` on namedtuple classes that have field names starting with leading underscores" by @AlexWaygood on 2025-11-29 18:10_

---

_Renamed from "[ty] Emit `invalid-namedtuple` on namedtuple classes that have field names starting with leading underscores" to "[ty] Emit `invalid-namedtuple` on namedtuple classes that have field names starting with underscores" by @AlexWaygood on 2025-11-29 18:10_

---

_Renamed from "[ty] Emit `invalid-namedtuple` on namedtuple classes that have field names starting with underscores" to "[ty] Emit `invalid-named-tuple` on namedtuple classes that have field names starting with underscores" by @AlexWaygood on 2025-11-29 18:11_

---

_Comment by @astral-sh-bot[bot] on 2025-11-29 18:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-11-29 18:13_

The changes reported by mypy_primer are all unrelated (https://github.com/astral-sh/ty/issues/1670)

---

_Marked ready for review by @AlexWaygood on 2025-11-29 18:13_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-29 18:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-29 18:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-29 18:13_

---

_@charliermarsh reviewed on 2025-11-30 15:53_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:502 on 2025-11-30 15:53_

Might be worth adding a test for this case, which seems to be allowed:
```python
from typing import NamedTuple

class Foo(NamedTuple):
    bar: int

class Baz(Foo):
    _bop: int
```

---

_@charliermarsh approved on 2025-11-30 15:54_

---

_Merged by @AlexWaygood on 2025-12-01 11:36_

---

_Closed by @AlexWaygood on 2025-12-01 11:36_

---

_Branch deleted on 2025-12-01 11:36_

---
