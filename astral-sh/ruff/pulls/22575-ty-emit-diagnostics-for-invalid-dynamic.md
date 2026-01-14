```yaml
number: 22575
title: "[ty] Emit diagnostics for invalid dynamic namedtuple fields"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: charlie/default-show
head: charlie/func-diag
created_at: 2026-01-14T16:04:29Z
updated_at: 2026-01-14T17:06:32Z
url: https://github.com/astral-sh/ruff/pull/22575
synced_at: 2026-01-14T17:37:51Z
```

# [ty] Emit diagnostics for invalid dynamic namedtuple fields

---

_@charliermarsh_

## Summary

Removes some TODOs from the dynamic namedtuple implementation.

---

_Label `ty` added by @charliermarsh on 2026-01-14 16:04_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 16:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-14 16:06:41.920448169 +0000
+++ new-output.txt	2026-01-14 16:06:42.254448975 +0000
@@ -762,6 +762,10 @@
 namedtuples_define_functional.py:37:21: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
 namedtuples_define_functional.py:42:18: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["1"]`
 namedtuples_define_functional.py:43:15: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
+namedtuples_define_functional.py:52:25: error[invalid-named-tuple] Duplicate field name `a` in `namedtuple()`: Field `a` already defined; will raise `ValueError` at runtime
+namedtuples_define_functional.py:53:25: error[invalid-named-tuple] Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime
+namedtuples_define_functional.py:54:25: error[invalid-named-tuple] Field name `def` in `namedtuple()` cannot be a Python keyword: Will raise `ValueError` at runtime
+namedtuples_define_functional.py:55:25: error[invalid-named-tuple] Field name `_d` in `namedtuple()` cannot start with an underscore: Will raise `ValueError` at runtime
 namedtuples_define_functional.py:69:1: error[missing-argument] No argument provided for required parameter `a`
 namedtuples_type_compat.py:22:23: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, int]`
 namedtuples_type_compat.py:23:28: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, str, str]`
@@ -1049,4 +1053,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1051 diagnostics
+Found 1055 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-14 16:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4361 diagnostics
+ Found 4359 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2026-01-14 16:17_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6830 on 2026-01-14 16:17_

Looks like claude still doesn't know how to do let chains 😅 

---

_@MichaReiser reviewed on 2026-01-14 16:18_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6829 on 2026-01-14 16:18_

I think you can use `seen_names.insert(name_str)` here. This adds the name if it doesn't exist yet and otherwise does nothing.

I'd also be somewhat inclined to not build the hash set and instead use a window iterating over `field_names` to see if the same name appears anywhere later in `field_names`. Unless we assume that `field_names` is very large, in which case we want to avoid the `O(n^2)` search

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6842 on 2026-01-14 16:21_

I don't find those comments useful at all. They just repeat the if condition

---

_@MichaReiser reviewed on 2026-01-14 16:21_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:6908 on 2026-01-14 16:21_

NIt: let chains

---

_@MichaReiser reviewed on 2026-01-14 16:21_

---

_@charliermarsh reviewed on 2026-01-14 16:28_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6842 on 2026-01-14 16:28_

I like them :(

---

_Marked ready for review by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @carljm by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-14 16:34_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-14 16:34_

---

_Converted to draft by @charliermarsh on 2026-01-14 16:51_

---

_Comment by @charliermarsh on 2026-01-14 17:06_

(Will mark for review once upstream merges + rebased + addressed Micha's initial comments.)

---
