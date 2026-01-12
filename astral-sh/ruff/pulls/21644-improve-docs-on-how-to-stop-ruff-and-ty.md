```yaml
number: 21644
title: Improve docs on how to stop Ruff and ty disagreeing with each other
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: alex/override-docs
created_at: 2025-11-26T17:30:17Z
updated_at: 2025-11-27T08:18:23Z
url: https://github.com/astral-sh/ruff/pull/21644
synced_at: 2026-01-12T15:57:30Z
```

# Improve docs on how to stop Ruff and ty disagreeing with each other

---

_@AlexWaygood_

## Summary

Lots of Ruff rules encourage you to make changes that might then cause ty to start complaining about Liskov violations. Most of these Ruff rules already refrain from complaining about a method if they see that the method is decorated with `@override`, but this usually isn't documented. This PR updates the docs of many Ruff rules to note that they refrain from complaining about `@override`-decorated methods, and also adds a similar note to the ty `invalid-method-override` documentation.

Helps with https://github.com/astral-sh/ty/issues/1644#issuecomment-3581663859

## Test Plan

- `uvx prek run -a` locally
- CI on this PR


---

_Review requested from @carljm by @AlexWaygood on 2025-11-26 17:30_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-26 17:30_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-26 17:30_

---

_Label `documentation` added by @AlexWaygood on 2025-11-26 17:30_

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 17:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-26 17:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 17:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
graphql-core (https://github.com/graphql-python/graphql-core)
- tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & dict[Unknown | str, Unknown | str]`
+ tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown | str, Unknown | str] & dict[str, Any]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[Unknown | tuple[str, int, Unknown]] & list[tuple[str, str, str] | tuple[str, int, Unknown]]`
+ ddtrace/appsec/_handlers.py:404:13: error[invalid-argument-type] Argument to bound method `add_configurations` is incorrect: Expected `list[tuple[str, str, str]]`, found `list[tuple[str, str, str] | tuple[str, int, Unknown]] & list[Unknown | tuple[str, int, Unknown]]`

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

_Comment by @astral-sh-bot[bot] on 2025-11-26 17:36_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@carljm approved on 2025-11-26 19:35_

LGTM!

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/diagnostic.rs`:2090 on 2025-11-27 07:27_

Someday this will no longer be required, because Ruff could check itself whether this is an overriden method.

---

_@MichaReiser approved on 2025-11-27 07:27_

---

_Merged by @AlexWaygood on 2025-11-27 08:18_

---

_Closed by @AlexWaygood on 2025-11-27 08:18_

---

_Branch deleted on 2025-11-27 08:18_

---
