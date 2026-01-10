```yaml
number: 21595
title: "[ty] Add code action to ignore diagnostic on the current line"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/add-ty-ignore
created_at: 2025-11-23T16:17:13Z
updated_at: 2025-11-29T14:53:17Z
url: https://github.com/astral-sh/ruff/pull/21595
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add code action to ignore diagnostic on the current line

---

_Pull request opened by @MichaReiser on 2025-11-23 16:17_

## Summary

Add an LSP code action to suppress a diagnostic on the current line by inserting a new `ty:ignore` comment or extending the codes of an existing `ty:ignore` comment.

* Skip existing `ty: ignore` comments with a reason.

## Test Plan

This needs integration tests

https://github.com/user-attachments/assets/129204dd-10d2-4bb4-b42a-1b0105647f17



---

_Label `server` added by @MichaReiser on 2025-11-23 16:17_

---

_Label `ty` added by @MichaReiser on 2025-11-23 16:17_

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 16:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@MichaReiser reviewed on 2025-11-23 16:19_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/code_action.rs`:40 on 2025-11-23 16:19_

TODO: Remove the debug traces

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 16:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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

_Comment by @astral-sh-bot[bot] on 2025-11-28 17:22_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@MichaReiser reviewed on 2025-11-28 21:04_

---

_Review comment by @MichaReiser on `.config/nextest.toml`:13 on 2025-11-28 21:04_

I think this is no longer needed, since I increased the request wait limit in e2e tests.

---

_Marked ready for review by @MichaReiser on 2025-11-28 21:29_

---

_Review requested from @carljm by @MichaReiser on 2025-11-28 21:29_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-28 21:29_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-28 21:29_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-28 21:29_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-28 21:30_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-28 21:30_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-28 21:30_

---

_Review requested from @Gankra by @MichaReiser on 2025-11-28 21:30_

---

_@MichaReiser reviewed on 2025-11-28 21:30_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/suppressions/type_ignore.md`:88 on 2025-11-28 21:30_

I didn't change the behavior for interpolated strings but I wrote this mdtest to document it, because it wasn't entirely clear to me when working on the fix.

---

_Review comment by @Gankra on `crates/ty_ide/src/code_action.rs`:382 on 2025-11-28 21:36_

Absolutely wild that this is valid syntax

---

_Review comment by @Gankra on `crates/ty_ide/src/code_action.rs`:218 on 2025-11-28 21:39_

Terrified of ever working on ruff

---

_@Gankra approved on 2025-11-28 21:40_

---

_@MichaReiser reviewed on 2025-11-29 14:41_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/code_action.rs`:382 on 2025-11-29 14:41_

I was always more surprised that this wasn't valid syntax pre Python 3.12

---

_Merged by @MichaReiser on 2025-11-29 14:41_

---

_Closed by @MichaReiser on 2025-11-29 14:41_

---

_Branch deleted on 2025-11-29 14:41_

---

_Comment by @sharkdp on 2025-11-29 14:53_

Fantastic!

---
