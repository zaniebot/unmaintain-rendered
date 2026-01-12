```yaml
number: 21699
title: "[ty] Don't introduce invalid syntax when autofixing override-of-final-method"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
  - ty
assignees: []
merged: true
base: main
head: alex/delete-statement
created_at: 2025-11-29T22:50:34Z
updated_at: 2025-11-30T14:42:08Z
url: https://github.com/astral-sh/ruff/pull/21699
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Don't introduce invalid syntax when autofixing override-of-final-method

---

_@AlexWaygood_

## Summary

This didn't actually turn out to be too hard afterall.

- If the overriding definition isn't a function, mark the autofix as display-only (we don't know what we're dealing with)
- If any function overloads don't have the `StmtClassDef` node as their immediate parent in the AST (e.g. one or more overloads is inside a `StmtIf` node), mark the fix as display-only 
- If the overriding definition is the only statement in the class body, replace the overriding definition with `pass` rather than just deleting the range of the overriding definition

Fixes https://github.com/astral-sh/ty/issues/1678

## Test Plan

Updated snapshtos


---

_Review requested from @carljm by @AlexWaygood on 2025-11-29 22:50_

---

_Label `fixes` added by @AlexWaygood on 2025-11-29 22:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-29 22:50_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-29 22:50_

---

_Label `ty` added by @AlexWaygood on 2025-11-29 22:50_

---

_Comment by @astral-sh-bot[bot] on 2025-11-29 22:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-29 22:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics

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

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:3914 on 2025-11-30 08:49_

I'm leaning towards not showing the fix in that case because, I think,  the is_only detection uses the wrong parent in that case and the more correct fix is to delete the entire outer statement, or the statement itself.

DisplayOnlyFixes are also never shown anywhere. They're just dead code

---

_@MichaReiser approved on 2025-11-30 08:50_

Thank you

---

_@AlexWaygood reviewed on 2025-11-30 12:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3914 on 2025-11-30 12:55_

oh, I assumed making it display-only was what you were asking for in https://github.com/astral-sh/ruff/pull/21646#discussion_r2573103184 (we don't have an `Applicability::Manual` :-)

But sure, I'll just get rid of the fix in these cases.

> DisplayOnlyFixes are also never shown anywhere. They're just dead code

What's the purpose of `Applicability::DisplayOnly` if they aren't shown anywhere? I figured that if/when we show fixes in the CLI rendering of diagnostics, the diff would be helpful for users in telling them what we were asking for (you mentioned in https://github.com/astral-sh/ruff/pull/21646 that you found it confusing that the diagnostic was only omitted on one overload, but that to fix the diagnostic you actually had to remove _all_ overloads).

---

_Merged by @AlexWaygood on 2025-11-30 13:40_

---

_Closed by @AlexWaygood on 2025-11-30 13:40_

---

_Branch deleted on 2025-11-30 13:40_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:3914 on 2025-11-30 14:42_

The idea was to only show them in the CLI but we never made it that far. There were just too many design decisions.

---

_@MichaReiser reviewed on 2025-11-30 14:42_

---
