```yaml
number: 21952
title: "[ty] fix hover type on named expression target"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/target
created_at: 2025-12-12T17:20:44Z
updated_at: 2025-12-12T17:32:36Z
url: https://github.com/astral-sh/ruff/pull/21952
synced_at: 2026-01-12T15:57:37Z
```

# [ty] fix hover type on named expression target

---

_@carljm_

## Summary

What it says on the tin.

## Test Plan

Added hover test.


---

_Label `ty` added by @carljm on 2025-12-12 17:20_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-12 17:20_

---

_Review requested from @sharkdp by @carljm on 2025-12-12 17:20_

---

_Review requested from @dcreager by @carljm on 2025-12-12 17:20_

---

_Review requested from @MichaReiser by @carljm on 2025-12-12 17:20_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 17:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 17:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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

_@zsol approved on 2025-12-12 17:25_

<a href="https://gitme.me/image?url=https%3A%2F%2Fmedia3.giphy.com%2Fmedia%2FeEXxfHQJ0dWOrctI55%2Fgiphy.gif&token=nice" data-gitmeme-token="nice"><img src="https://media3.giphy.com/media/eEXxfHQJ0dWOrctI55/giphy.gif" title="Created by gitme.me with /nice"
        alt="nice"/></a>

---

_@AlexWaygood approved on 2025-12-12 17:30_

I'm curious, where were we previously getting the `Never` type from? We must have stored a type for the expression somewhere, right?

---

_Merged by @carljm on 2025-12-12 17:30_

---

_Closed by @carljm on 2025-12-12 17:30_

---

_Branch deleted on 2025-12-12 17:30_

---

_Comment by @carljm on 2025-12-12 17:32_

> I'm curious, where were we previously getting the `Never` type from?

It comes from `infer_name_expression`. I looked at getting rid of the `Never` there since ideally we never want it, but I don't want to introduce a panic in some edge case by adding an `unreachable!()`, and currently that method has to return a type. Maybe ideally it could return `None` to say "don't store anything" but that'd be a bigger change than I want to take on right now.

---
