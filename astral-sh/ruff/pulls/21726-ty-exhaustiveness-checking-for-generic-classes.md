```yaml
number: 21726
title: "[ty] Exhaustiveness checking for generic classes"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1702
created_at: 2025-12-01T12:01:49Z
updated_at: 2025-12-01T12:52:38Z
url: https://github.com/astral-sh/ruff/pull/21726
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Exhaustiveness checking for generic classes

---

_@sharkdp_

## Summary

We had tests for this already, but they used generic classes that were bivariant in their type parameter, and so this case wasn't captured.

closes https://github.com/astral-sh/ty/issues/1702

## Test Plan

Updated Markdown tests


---

_Label `bug` added by @sharkdp on 2025-12-01 12:01_

---

_Label `ty` added by @sharkdp on 2025-12-01 12:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/exhaustiveness_checking.md`:287 on 2025-12-01 12:02_

bivariance strikes again

---

_@sharkdp reviewed on 2025-12-01 12:02_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pylint (https://github.com/pycqa/pylint)
- pylint/checkers/typecheck.py:2177:30: warning[possibly-unresolved-reference] Name `msg` used when possibly not defined
- pylint/checkers/typecheck.py:2201:30: warning[possibly-unresolved-reference] Name `msg` used when possibly not defined
- Found 214 diagnostics
+ Found 212 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 44 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/go2rtc/__init__.py:321:26: warning[possibly-unresolved-reference] Name `value` used when possibly not defined
- Found 14527 diagnostics
+ Found 14526 diagnostics

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

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-01 12:21_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 12:30_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-unresolved-reference` | 0 | 3 | 0 |
| `unsupported-base` | 3 | 0 | 0 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **3** | **5** | **0** |

**[Full report with detailed diff](https://david-fix-1702.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1702.ecosystem-663.pages.dev/timing))




---

_Renamed from "[ty] Exhaustivness checking for generic classes" to "[ty] Exhaustiveness checking for generic classes" by @AlexWaygood on 2025-12-01 12:41_

---

_Marked ready for review by @sharkdp on 2025-12-01 12:42_

---

_Review requested from @carljm by @sharkdp on 2025-12-01 12:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-01 12:42_

---

_Review requested from @dcreager by @sharkdp on 2025-12-01 12:42_

---

_@AlexWaygood approved on 2025-12-01 12:49_

---

_Merged by @sharkdp on 2025-12-01 12:52_

---

_Closed by @sharkdp on 2025-12-01 12:52_

---

_Branch deleted on 2025-12-01 12:52_

---
