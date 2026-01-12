```yaml
number: 21968
title: "[ty] Add \"qualify ...\" code fix for undefined references"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/qualify
created_at: 2025-12-13T21:57:23Z
updated_at: 2025-12-15T15:14:38Z
url: https://github.com/astral-sh/ruff/pull/21968
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Add "qualify ..." code fix for undefined references

---

_@Gankra_

## Summary

If `import warnings` exists in the file, we will suggest an edit of `deprecated -> warnings.deprecated` as "qualify warnings.deprecated"

## Test Plan

Should test more cases...


---

_Label `server` added by @AlexWaygood on 2025-12-13 21:57_

---

_Label `ty` added by @AlexWaygood on 2025-12-13 21:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 21:59_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-13 22:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5121 diagnostics
+ Found 5122 diagnostics

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

_Marked ready for review by @Gankra on 2025-12-14 23:19_

---

_Review requested from @carljm by @Gankra on 2025-12-14 23:19_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-14 23:19_

---

_Review requested from @sharkdp by @Gankra on 2025-12-14 23:19_

---

_Review requested from @dcreager by @Gankra on 2025-12-14 23:19_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-14 23:19_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/code_action.rs`:501 on 2025-12-15 06:41_

This fix here looks off. Any chance we're not using `dedent` on the source? 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:617 on 2025-12-15 06:44_

Do we apply any upper bound on how many actions we show? I'm a bit worried that we now show twice as many code actions in non-trivial cases (e.g. multiple modules exporting the same module), and this can quickly become very noisy for users. 

---

_@MichaReiser reviewed on 2025-12-15 06:45_

---

_@Gankra reviewed on 2025-12-15 14:43_

---

_Review comment by @Gankra on `crates/ty_ide/src/completion.rs`:617 on 2025-12-15 14:43_

In practice the "qualify..." options will be much rarer because the implementation refuses to show them if it would require editing your imports. As future work we could look into implementing rust-analyzer's more complex UX where you only ever get "import X" and "qualify X" and then e.g. when you select "import" if there was actually multiple possible imports it brings up a secondary menu to select which one you meant.

---

_@Gankra reviewed on 2025-12-15 14:51_

---

_Review comment by @Gankra on `crates/ty_ide/src/code_action.rs`:501 on 2025-12-15 14:51_

Good catch, fixed

---

_@MichaReiser approved on 2025-12-15 15:02_

---

_Merged by @Gankra on 2025-12-15 15:14_

---

_Closed by @Gankra on 2025-12-15 15:14_

---

_Branch deleted on 2025-12-15 15:14_

---
