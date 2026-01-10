```yaml
number: 21777
title: "[ty] Add subdiagnostic hint if the user wrote `X = Any` rather than `X: Any`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/special-form-attr-hint
created_at: 2025-12-03T17:23:59Z
updated_at: 2025-12-03T20:42:23Z
url: https://github.com/astral-sh/ruff/pull/21777
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add subdiagnostic hint if the user wrote `X = Any` rather than `X: Any`

---

_Pull request opened by @AlexWaygood on 2025-12-03 17:23_

Fixes https://github.com/astral-sh/ty/issues/1744

---

_Review requested from @carljm by @AlexWaygood on 2025-12-03 17:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-03 17:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-03 17:24_

---

_Label `ty` added by @AlexWaygood on 2025-12-03 17:24_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-03 17:24_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 17:25_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-03 17:36:22.183723401 +0000
+++ new-output.txt	2025-12-03 17:36:25.927749245 +0000
@@ -915,7 +915,7 @@
 specialtypes_type.py:120:5: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:137:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
 specialtypes_type.py:139:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
-specialtypes_type.py:143:1: error[unresolved-attribute] Object of type `typing.Type` has no attribute `unknown`
+specialtypes_type.py:143:1: error[unresolved-attribute] Special form `typing.Type` has no attribute `unknown`
 specialtypes_type.py:145:1: error[unresolved-attribute] Class `type` has no attribute `unknown`
 specialtypes_type.py:169:21: error[invalid-assignment] Object of type `type` is not assignable to `type[int]`
 specialtypes_type.py:175:16: error[invalid-return-type] Return type does not match returned value: expected `type[T@ClassA]`, found `type`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-03 17:28_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/transactions.py:349:48: error[unresolved-attribute] Object of type `typing.Self` has no attribute `commit`
+ src/prefect/transactions.py:349:48: error[unresolved-attribute] Special form `typing.Self` has no attribute `commit`
- src/prefect/transactions.py:350:38: error[unresolved-attribute] Object of type `typing.Self` has no attribute `commit`
+ src/prefect/transactions.py:350:38: error[unresolved-attribute] Special form `typing.Self` has no attribute `commit`
- src/prefect/transactions.py:352:21: error[unresolved-attribute] Object of type `typing.Self` has no attribute `commit`
+ src/prefect/transactions.py:352:21: error[unresolved-attribute] Special form `typing.Self` has no attribute `commit`
- src/prefect/transactions.py:429:48: error[unresolved-attribute] Object of type `typing.Self` has no attribute `rollback`
+ src/prefect/transactions.py:429:48: error[unresolved-attribute] Special form `typing.Self` has no attribute `rollback`
- src/prefect/transactions.py:430:38: error[unresolved-attribute] Object of type `typing.Self` has no attribute `rollback`
+ src/prefect/transactions.py:430:38: error[unresolved-attribute] Special form `typing.Self` has no attribute `rollback`
- src/prefect/transactions.py:432:21: error[unresolved-attribute] Object of type `typing.Self` has no attribute `rollback`
+ src/prefect/transactions.py:432:21: error[unresolved-attribute] Special form `typing.Self` has no attribute `rollback`
- src/prefect/transactions.py:512:21: error[unresolved-attribute] Object of type `typing.Self` has no attribute `commit`
+ src/prefect/transactions.py:512:21: error[unresolved-attribute] Special form `typing.Self` has no attribute `commit`
- src/prefect/transactions.py:592:21: error[unresolved-attribute] Object of type `typing.Self` has no attribute `rollback`
+ src/prefect/transactions.py:592:21: error[unresolved-attribute] Special form `typing.Self` has no attribute `rollback`

xarray (https://github.com/pydata/xarray)
- xarray/core/treenode.py:654:13: error[unresolved-attribute] Object of type `typing.Self` has no attribute `orphan`
+ xarray/core/treenode.py:654:13: error[unresolved-attribute] Special form `typing.Self` has no attribute `orphan`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5511 diagnostics
+ Found 5512 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-03 17:35_

The ecosystem impact seems fine. Those `Self` diagnostics are false positives related to https://github.com/astral-sh/ty/issues/1124, but this PR doesn't make the error messages any worse for them, and it's unlikely any of the detailed subdiagnostics I'm adding will fire on any of them.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:9121 on 2025-12-03 19:27_

Is it worth reusing this (also used in `is_if_type_checking`)? Fine if not, it's short enough.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:9236 on 2025-12-03 19:29_

:1st_place_medal: 

---

_@sharkdp approved on 2025-12-03 19:31_

Cool — thank you!

---

_@AlexWaygood reviewed on 2025-12-03 20:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:9121 on 2025-12-03 20:42_

I think I'll pass -- it's a tiny routine, and `semantic_index/builder.rs` feels like a long way away from this code.

---

_Merged by @AlexWaygood on 2025-12-03 20:42_

---

_Closed by @AlexWaygood on 2025-12-03 20:42_

---

_Branch deleted on 2025-12-03 20:42_

---
