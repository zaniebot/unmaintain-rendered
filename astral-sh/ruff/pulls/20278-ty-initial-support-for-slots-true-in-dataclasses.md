```yaml
number: 20278
title: "[ty] initial support for `slots=True` in dataclasses"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
assignees: []
merged: true
base: main
head: thejchap/dataclass-slots
created_at: 2025-09-06T01:42:18Z
updated_at: 2025-09-07T17:25:35Z
url: https://github.com/astral-sh/ruff/pull/20278
synced_at: 2026-01-12T15:56:58Z
```

# [ty] initial support for `slots=True` in dataclasses

---

_@thejchap_

## Summary

https://github.com/astral-sh/ty/issues/111
https://docs.python.org/3/library/dataclasses.html

this pr adds initial support for `slots=True` in the `dataclass` decorator. synthesizes `__slots__` as returning a fixed-length tuple of `str`s (field names)

## Test Plan

- new mdtests
- typing conformance diffs look to be true positives - https://diffswarm.dev/d-01k4eb7wx0wqecmxpvxxnch9ag

---

_Comment by @github-actions[bot] on 2025-09-06 01:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-07 17:09:46.191064468 +0000
+++ new-output.txt	2025-09-07 17:09:48.844087184 +0000
@@ -221,8 +221,6 @@
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
 dataclasses_postinit.py:28:7: error[unresolved-attribute] Type `DC1` has no attribute `x`
 dataclasses_postinit.py:29:7: error[unresolved-attribute] Type `DC1` has no attribute `y`
-dataclasses_slots.py:56:1: error[unresolved-attribute] Type `<class 'DC5'>` has no attribute `__slots__`
-dataclasses_slots.py:57:1: error[unresolved-attribute] Type `DC5` has no attribute `__slots__`
 dataclasses_slots.py:66:1: error[unresolved-attribute] Type `<class 'DC6'>` has no attribute `__slots__`
 dataclasses_slots.py:69:1: error[unresolved-attribute] Type `DC6` has no attribute `__slots__`
 dataclasses_transform_class.py:60:8: error[missing-argument] No argument provided for required parameter `not_a_field` of bound method `__init__`
@@ -868,5 +866,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 869 diagnostics
+Found 867 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-06 01:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @thejchap on 2025-09-06 01:57_

---

_Review requested from @carljm by @thejchap on 2025-09-06 01:57_

---

_Review requested from @AlexWaygood by @thejchap on 2025-09-06 01:57_

---

_Review requested from @sharkdp by @thejchap on 2025-09-06 01:57_

---

_Review requested from @dcreager by @thejchap on 2025-09-06 01:57_

---

_Renamed from "[ty] support `slots=True` for dataclasses" to "[ty] initial support for `slots=True` in dataclasses" by @thejchap on 2025-09-06 01:58_

---

_Label `ty` added by @ntBre on 2025-09-06 02:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:549 on 2025-09-07 15:30_

```suggestion
If a dataclass is defined with `slots=True`, the `__slots__` attribute is generated as a tuple. It
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2292 on 2025-09-07 15:39_

At some point we will need to know the precise string values of `__slots__`, because at some point we will want to emit an diagnostic on code like this:

```py
@dataclass(slots=True)
class F:
    x: int

    def method(self):
        self.y = 42  # will raise an exception because `y` is not in `__slots__`
```

We don't need to add that diagnostic in this PR (and we'll want to emit the diagnostic on non-dataclasses as well as dataclasses). But ISTM that we might as well infer this now as a tuple of precise string-literal types, rather than as a tuple of `str`s? It will help support that future feature.

```suggestion
                has_dataclass_param(DataclassParams::SLOTS).then(|| {
                    let fields = self.fields(db, specialization, field_policy);
                    let slots = fields.keys().map(|name| Type::string_literal(db, name));
                    Type::heterogeneous_tuple(db, slots)
                })
```

---

_@AlexWaygood reviewed on 2025-09-07 15:45_

Thanks, this is great! A couple of small points below.

Could you also add something like this test case to `https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/instance_layout_conflict.md` (it already passes on your branch!):

```py
from dataclasses import dataclass

@dataclass(slots=True)
class F: ...

@dataclass(slots=True)
class G: ...

class H(F, G): ...  # fine because both classes have empty `__slots__`

@dataclass(slots=True)
class I:
    x: int

@dataclass(slots=True)
class J:
    y: int

class K(I, J): ...  # error: [instance-layout-conflict]
```

And something like this test case to `https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md` (it also already passes on your branch!):

```py
from dataclasses import dataclass
from ty_extensions import is_disjoint_from, static_assert

@dataclass(slots=True)
class F: ...

@dataclass(slots=True)
class G: ...

@dataclass(slots=True)
class I:
    x: int

@dataclass(slots=True)
class J:
    y: int

# A dataclass with empty `__slots__` is not disjoint from another dataclass with `__slots__`
static_assert(not is_disjoint_from(F, G))
static_assert(not is_disjoint_from(F, I))
static_assert(not is_disjoint_from(G, I))
static_assert(not is_disjoint_from(F, J))
static_assert(not is_disjoint_from(G, J))

# But two dataclasses with non-empty `__slots__` are disjoint
static_assert(is_disjoint_from(I, J))
```

---

_@thejchap reviewed on 2025-09-07 17:08_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/class.rs`:2292 on 2025-09-07 17:08_

ah! great - makes sense. thanks

---

_@AlexWaygood approved on 2025-09-07 17:10_

Thanks!

---

_Merged by @AlexWaygood on 2025-09-07 17:25_

---

_Closed by @AlexWaygood on 2025-09-07 17:25_

---
