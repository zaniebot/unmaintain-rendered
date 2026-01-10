```yaml
number: 21790
title: "[ty] Ensure `rename` `CursorTest` calls `can_rename` before renaming"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/rename-tests
created_at: 2025-12-04T12:48:11Z
updated_at: 2025-12-04T13:19:50Z
url: https://github.com/astral-sh/ruff/pull/21790
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Ensure `rename` `CursorTest` calls `can_rename` before renaming

---

_Pull request opened by @MichaReiser on 2025-12-04 12:48_

## Summary

This PR ensures that `CursorTest::rename` calls `can_rename` before renaming a symbol.

It happened multiple times that I added a test that works fine in `CursorTest` but that
then doesn't work in VS Code because `can_rename` returns nope. 

This change ensures that our testing infrastructure more closely resembles the flow 
used by actual LSP clients.

## Test Plan

See updated tests


---

_Label `testing` added by @MichaReiser on 2025-12-04 12:48_

---

_Label `ty` added by @MichaReiser on 2025-12-04 12:48_

---

_Review requested from @Gankra by @MichaReiser on 2025-12-04 12:49_

---

_Marked ready for review by @MichaReiser on 2025-12-04 12:49_

---

_Review requested from @carljm by @MichaReiser on 2025-12-04 12:49_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-04 12:49_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-04 12:49_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-04 12:49_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-04 12:49_

---

_Review request for @carljm removed by @MichaReiser on 2025-12-04 12:49_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-04 12:49_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-12-04 12:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 12:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 12:52_


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

_@sharkdp approved on 2025-12-04 13:05_

---

_@Gankra approved on 2025-12-04 13:15_

---

_Merged by @MichaReiser on 2025-12-04 13:19_

---

_Closed by @MichaReiser on 2025-12-04 13:19_

---

_Branch deleted on 2025-12-04 13:19_

---
