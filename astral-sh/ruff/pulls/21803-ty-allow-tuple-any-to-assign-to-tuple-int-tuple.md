```yaml
number: 21803
title: "[ty] Allow `tuple[Any, ...]` to assign to `tuple[int, *tuple[int, ...]]`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2025-12-05T03:09:03Z
updated_at: 2025-12-09T11:07:04Z
url: https://github.com/astral-sh/ruff/pull/21803
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Allow `tuple[Any, ...]` to assign to `tuple[int, *tuple[int, ...]]`

---

_Pull request opened by @charliermarsh on 2025-12-05 03:09_

## Summary

Closes https://github.com/astral-sh/ty/issues/1750.


---

_Review requested from @carljm by @charliermarsh on 2025-12-05 03:09_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-05 03:09_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-05 03:09_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-05 03:09_

---

_Label `ty` added by @charliermarsh on 2025-12-05 03:09_

---

_Comment by @astral-sh-bot[bot] on 2025-12-05 03:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-05 19:01:00.800057224 +0000
+++ new-output.txt	2025-12-05 19:01:04.523089702 +0000
@@ -934,7 +934,6 @@
 tuples_type_compat.py:32:10: error[invalid-assignment] Object of type `tuple[int, *tuple[int, ...]]` is not assignable to `tuple[int]`
 tuples_type_compat.py:33:10: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int]`
 tuples_type_compat.py:43:22: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int]`
-tuples_type_compat.py:47:40: error[invalid-assignment] Object of type `tuple[Any, ...]` is not assignable to `tuple[int, *tuple[str, ...]]`
 tuples_type_compat.py:62:26: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int, int]`
 tuples_type_compat.py:75:9: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
 tuples_type_compat.py:80:9: error[type-assertion-failure] Type `tuple[str, str] | tuple[int, int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
@@ -1031,4 +1030,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1033 diagnostics
+Found 1032 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-05 03:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
+ scipy-stubs/optimize/_constraints.pyi:56:50: warning[unused-ignore-comment] Unused `ty: ignore` directive
+ scipy-stubs/sparse/_base.pyi:120:98: warning[unused-ignore-comment] Unused `ty: ignore` directive
+ scipy-stubs/sparse/_base.pyi:933:108: warning[unused-ignore-comment] Unused `ty: ignore` directive
+ scipy-stubs/sparse/_base.pyi:943:108: warning[unused-ignore-comment] Unused `ty: ignore` directive
+ scipy-stubs/sparse/linalg/_expm_multiply.pyi:22:67: warning[unused-ignore-comment] Unused `ty: ignore` directive
- Found 96 diagnostics
+ Found 101 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5518 diagnostics
+ Found 5517 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-05 03:13_

The conformance change is correct.

---

_Comment by @charliermarsh on 2025-12-05 03:15_

Confused by the change in `scikit-build-core` and `dd-trace-py` though.

---

_Comment by @carljm on 2025-12-05 03:55_

Those two are known unstable diagnostics currently, not related to this PR. 

---

_Comment by @charliermarsh on 2025-12-05 04:02_

Oh let's go, thank you.

---

_@AlexWaygood requested changes on 2025-12-05 17:15_

This PR regresses some cases. This assertion passes on `main`, but fails on your PR branch:

```py
from typing import Any
from ty_extensions import static_assert, is_assignable_to

static_assert(not is_assignable_to(tuple[*tuple[Any, ...], Any], tuple[*tuple[Any, ...], Any, Any]))
```

The spec carves out a _very_ specific special case here for `tuple[Any, ...]` in particular. The special case does _not_ apply to "mixed" tuples that have `Any` as their variable-length element but also have suffixes and prefixes

---

_Comment by @AlexWaygood on 2025-12-05 17:18_

Oh hang on, maybe I'm wrong. The bit at the top of the "tuples" section of the spec suggests that this is a very specific special case. But lower down, the spec also says

> If an unpacked `*tuple[Any, ...]` is embedded within another tuple, that portion of the tuple is [consistent](https://typing.python.org/en/latest/spec/glossary.html#term-consistent) with any tuple of any length.

---

_@AlexWaygood approved on 2025-12-05 17:22_

Okay, this looks great to me. Thank you!!

The only thing I think would be great to add would be to add the test case that I initially thought this PR was regressing 😆 I.e., let's add this assertion:

```py
from typing import Any
from ty_extensions import static_assert, is_assignable_to

# `*tuple[Any, ...]` can materialize to a tuple of any length as a special case,
# so this passes:
static_assert(is_assignable_to(tuple[*tuple[Any, ...], Any], tuple[*tuple[Any, ...], Any, Any]))
```

It looks like we already link to the relevant parts of the spec in that mdtest

---

_Merged by @charliermarsh on 2025-12-05 19:04_

---

_Closed by @charliermarsh on 2025-12-05 19:04_

---

_Branch deleted on 2025-12-05 19:04_

---

_Label `bug` added by @dhruvmanila on 2025-12-09 11:07_

---
