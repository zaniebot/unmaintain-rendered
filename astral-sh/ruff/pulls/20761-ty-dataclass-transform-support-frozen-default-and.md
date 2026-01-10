```yaml
number: 20761
title: "[ty] `dataclass_transform`: Support `frozen_default` and `kw_only_default`"
type: pull_request
state: merged
author: fatelei
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-dataclass-transform-overrides
created_at: 2025-10-08T10:17:30Z
updated_at: 2025-10-09T07:34:49Z
url: https://github.com/astral-sh/ruff/pull/20761
synced_at: 2026-01-10T17:34:34Z
```

# [ty] `dataclass_transform`: Support `frozen_default` and `kw_only_default`

---

_Pull request opened by @fatelei on 2025-10-08 10:17_

## Summary

- Add support for eq, kw_only, and frozen parameter overrides in @dataclass_transform
- Previously only order parameter override was supported
- Update test documentation to reflect fixed behavior
- Resolves issue where kw_only_default and frozen_default could not be overridden

closes https://github.com/astral-sh/ty/issues/1260

## Test Plan

New Markdown tests


---

_Review requested from @carljm by @fatelei on 2025-10-08 10:17_

---

_Review requested from @AlexWaygood by @fatelei on 2025-10-08 10:17_

---

_Review requested from @sharkdp by @fatelei on 2025-10-08 10:17_

---

_Review requested from @dcreager by @fatelei on 2025-10-08 10:17_

---

_Renamed from "Fix dataclass_transform parameter overrides for kw_only and frozen" to "[ty] Fix dataclass_transform parameter overrides for kw_only and frozen" by @AlexWaygood on 2025-10-08 10:19_

---

_Label `ty` added by @AlexWaygood on 2025-10-08 10:19_

---

_Comment by @github-actions[bot] on 2025-10-08 10:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-09 07:21:50.600268550 +0000
+++ new-output.txt	2025-10-09 07:21:53.895289947 +0000
@@ -284,8 +284,6 @@
 dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> int`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo(unsupported type[X] special form)`
 dataclasses_transform_field.py:75:16: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
-dataclasses_transform_func.py:53:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
-dataclasses_transform_func.py:53:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_func.py:57:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` of type `str`
 dataclasses_transform_func.py:61:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_func.py:65:36: error[unknown-argument] Argument `salary` does not match any known parameter
@@ -898,5 +896,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 899 diagnostics
+Found 897 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-08 10:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/dataclass_transform_example.py:43:1: error[invalid-assignment] Property `a` defined in `FrozenDefine` is read-only
+ tests/test_annotations.py:448:13: error[missing-argument] No arguments provided for required parameters `x`, `y`
+ tests/test_annotations.py:448:15: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+ tests/test_functional.py:332:13: error[invalid-assignment] Property `x` defined in `SubFrozen` is read-only
- Found 557 diagnostics
+ Found 561 diagnostics

Expression (https://github.com/cognitedata/Expression)
- tests/test_tagged_union.py:93:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 212 diagnostics
+ Found 211 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-10-09 07:18_

Thank you very much!

I made some minor changes myself, such as using the new `parameter_type_by_name` helper, that I just introduced yesterday.

The ecosystem changes are all true positives! (appear within `with pytest.raises(TypeError):` blocks)

---

_Renamed from "[ty] Fix dataclass_transform parameter overrides for kw_only and frozen" to "[ty] `dataclass_transform`: Support `frozen_default` and `kw_only_default`" by @sharkdp on 2025-10-09 07:22_

---

_Merged by @sharkdp on 2025-10-09 07:34_

---

_Closed by @sharkdp on 2025-10-09 07:34_

---
