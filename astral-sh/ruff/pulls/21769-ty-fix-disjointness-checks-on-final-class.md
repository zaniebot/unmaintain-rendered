```yaml
number: 21769
title: "[ty] Fix disjointness checks on `@final` class instances"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/final-class-disjointness
created_at: 2025-12-03T06:01:18Z
updated_at: 2025-12-10T19:17:25Z
url: https://github.com/astral-sh/ruff/pull/21769
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Fix disjointness checks on `@final` class instances

---

_Pull request opened by @ibraheemdev on 2025-12-03 06:01_

## Summary

This was left unfinished in https://github.com/astral-sh/ruff/pull/21167. This is required to fix our disjointness checks with type-of a final class, which is currently broken, and blocking https://github.com/astral-sh/ty/issues/159.

---

_Label `ty` added by @ibraheemdev on 2025-12-03 06:01_

---

_Review requested from @carljm by @ibraheemdev on 2025-12-03 06:01_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-03 06:01_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-03 06:01_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-03 06:01_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 06:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-03 06:05_


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

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/response.py:289:16: error[invalid-return-type] Return type does not match returned value: expected `bytes`, found `bytes | (memoryview[bytes] & ~memoryview[int])`
+ src/urllib3/response.py:289:16: error[invalid-return-type] Return type does not match returned value: expected `bytes`, found `bytes | memoryview[bytes]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5512 diagnostics
+ Found 5511 diagnostics

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

_@carljm approved on 2025-12-03 06:42_

Lovely!

---

_@ibraheemdev reviewed on 2025-12-03 10:19_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:1259 on 2025-12-03 10:19_

I suppose this is only correct for covariant/contravariant types. Bivariant types are never disjoint, but I'm not sure what the corresponding relationship is for invariant types.

---

_@AlexWaygood reviewed on 2025-12-03 10:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1259 on 2025-12-03 10:38_

I haven't looked through this PR yet but on this point — note that `Sequence[int]` and `Sequence[str]` are not disjoint, even though `str` and `int` are disjoint, because `Sequence[Never]` is a subtype of both, and `Sequence[Never]` is an inhabited type: its subtypes include `tuple[()]` (inhabited by the runtime object `()`) and `list[Never]` (which can be inhabited by the runtime object `[]`)

---

_@AlexWaygood reviewed on 2025-12-03 10:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1259 on 2025-12-03 10:39_

However, `list[bool]` _is_ disjoint from `list[int]`, even though `int` is not disjoint from `bool`: there are no possible mutual subtypes of the two types; it is impossible for any runtime object to inhabit both types simultaneously

---

_Merged by @ibraheemdev on 2025-12-10 19:17_

---

_Closed by @ibraheemdev on 2025-12-10 19:17_

---

_Branch deleted on 2025-12-10 19:17_

---
