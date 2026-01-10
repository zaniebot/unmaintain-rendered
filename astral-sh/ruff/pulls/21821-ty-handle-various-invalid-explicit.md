```yaml
number: 21821
title: "[ty] Handle various invalid explicit specializations for `ParamSpec`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dhruv/avoid-assertion
created_at: 2025-12-06T06:56:27Z
updated_at: 2025-12-08T05:20:42Z
url: https://github.com/astral-sh/ruff/pull/21821
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Handle various invalid explicit specializations for `ParamSpec`

---

_Pull request opened by @dhruvmanila on 2025-12-06 06:56_

## Summary

fixes: https://github.com/astral-sh/ty/issues/1788

## Test Plan

Add new mdtests.


---

_Label `bug` added by @dhruvmanila on 2025-12-06 06:56_

---

_Review requested from @carljm by @dhruvmanila on 2025-12-06 06:56_

---

_Label `ty` added by @dhruvmanila on 2025-12-06 06:56_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-12-06 06:56_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-06 06:56_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-06 06:56_

---

_Comment by @astral-sh-bot[bot] on 2025-12-06 07:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-06 07:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

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

_Renamed from "[ty] Handle various invalid specializations for `ParamSpec`" to "[ty] Handle various invalid explicit specializations for `ParamSpec`" by @dhruvmanila on 2025-12-06 07:03_

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-12-06 09:20_

---

_Comment by @astral-sh-bot[bot] on 2025-12-06 09:29_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-base` | 0 | 2 | 0 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **0** | **4** | **0** |

**[Full report with detailed diff](https://dhruv-avoid-assertion.ecosystem-663.pages.dev/diff)** ([timing results](https://dhruv-avoid-assertion.ecosystem-663.pages.dev/timing))




---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:269 on 2025-12-06 10:06_

I'm guessing you meant to switch it on again here after switching it off a few lines above?

```suggestion
<!-- blacken-docs:on -->
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:255 on 2025-12-06 10:07_

```suggestion
<!-- blacken-docs:on -->
```

---

_@AlexWaygood reviewed on 2025-12-06 10:08_

---

_@dhruvmanila reviewed on 2025-12-06 16:40_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:269 on 2025-12-06 16:40_

Oops, yes ty!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md`:249 on 2025-12-07 14:00_

you could add a comment here saying that this is "poor taste", and that users shouldn't write their annotations like this. But that we allow it anyway because `OnlyParamSpec[(int, str)]` has the same AST as `OnlyParamSpec[int, str]` (according to CPython, at least), and because mypy/pyright allow it

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md`:233 on 2025-12-07 14:01_

same here, you could add a comment explaining why we support this even though it's "highly distasteful" 😆

---

_@AlexWaygood approved on 2025-12-07 14:01_

thank you!

---

_Merged by @dhruvmanila on 2025-12-08 05:20_

---

_Closed by @dhruvmanila on 2025-12-08 05:20_

---

_Branch deleted on 2025-12-08 05:20_

---
