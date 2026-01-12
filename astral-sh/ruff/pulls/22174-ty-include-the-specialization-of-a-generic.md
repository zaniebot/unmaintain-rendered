```yaml
number: 22174
title: "[ty] Include the specialization of a generic `TypedDict` as part of its display"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/generic-typeddict-display
created_at: 2025-12-24T14:33:31Z
updated_at: 2025-12-24T14:39:48Z
url: https://github.com/astral-sh/ruff/pull/22174
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Include the specialization of a generic `TypedDict` as part of its display

---

_@AlexWaygood_

## Summary

This is the easy bit of https://github.com/astral-sh/ty/issues/2190

## Test Plan

mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-12-24 14:33_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-24 14:33_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-24 14:33_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-24 14:33_

---

_@charliermarsh approved on 2025-12-24 14:34_

---

_Comment by @astral-sh-bot[bot] on 2025-12-24 14:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-24 14:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2806 diagnostics
+ Found 2803 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/_glm/tests/test_glm.py:51:9: error[invalid-argument-type] Argument to function `root` is incorrect: Expected `_RootOptionsHybr | _RootOptionsLM | _RootOptionsJacBase | ... omitted 6 union elements`, found `dict[Unknown | str, Unknown]`
+ sklearn/linear_model/_glm/tests/test_glm.py:51:9: error[invalid-argument-type] Argument to function `root` is incorrect: Expected `_RootOptionsHybr | _RootOptionsLM | _RootOptionsJacBase[_JacOptionsBroyden] | ... omitted 6 union elements`, found `dict[Unknown | str, Unknown]`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/_specs.pyi:72:49: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'ValueDict | FieldDict | ExprDict'>` is already specialized
+ src/bokeh/_specs.pyi:72:49: error[non-subscriptable] Cannot subscript non-generic type: `<types.UnionType special-form 'ValueDict[ValueType, UnitsType] | FieldDict[ValueType, UnitsType] | ExprDict[ValueType, UnitsType]'>` is already specialized
- src/bokeh/models/annotations/geometry.py:250:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners | UndefinedType | IntrinsicType`, found `Literal[0]`
+ src/bokeh/models/annotations/geometry.py:250:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners[Property[Unknown]] | UndefinedType | IntrinsicType`, found `Literal[0]`
- src/bokeh/models/annotations/html/labels.py:78:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY | ... omitted 4 union elements`, found `Literal[0]`
+ src/bokeh/models/annotations/html/labels.py:78:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY[Property[Unknown]] | ... omitted 4 union elements`, found `Literal[0]`
- src/bokeh/models/annotations/html/labels.py:85:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners | UndefinedType | IntrinsicType`, found `Literal[0]`
+ src/bokeh/models/annotations/html/labels.py:85:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners[Property[Unknown]] | UndefinedType | IntrinsicType`, found `Literal[0]`
- src/bokeh/models/annotations/labels.py:87:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY | ... omitted 4 union elements`, found `Literal[0]`
+ src/bokeh/models/annotations/labels.py:87:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY[Property[Unknown]] | ... omitted 4 union elements`, found `Literal[0]`
- src/bokeh/models/annotations/labels.py:94:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners | UndefinedType | IntrinsicType`, found `Literal[0]`
+ src/bokeh/models/annotations/labels.py:94:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners[Property[Unknown]] | UndefinedType | IntrinsicType`, found `Literal[0]`
- src/bokeh/models/annotations/legends.py:510:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY | ... omitted 4 union elements`, found `Literal[10]`
+ src/bokeh/models/annotations/legends.py:510:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY[Property[Unknown]] | ... omitted 4 union elements`, found `Literal[10]`
- src/bokeh/models/annotations/legends.py:516:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners | UndefinedType | IntrinsicType`, found `Literal[0]`
+ src/bokeh/models/annotations/legends.py:516:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners[Property[Unknown]] | UndefinedType | IntrinsicType`, found `Literal[0]`
- src/bokeh/models/glyphs.py:226:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners | UndefinedType | IntrinsicType`, found `Literal[0]`
+ src/bokeh/models/glyphs.py:226:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners[Property[Unknown]] | UndefinedType | IntrinsicType`, found `Literal[0]`
- src/bokeh/models/glyphs.py:1391:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners | UndefinedType | IntrinsicType`, found `Literal[0]`
+ src/bokeh/models/glyphs.py:1391:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners[Property[Unknown]] | UndefinedType | IntrinsicType`, found `Literal[0]`
- src/bokeh/models/glyphs.py:1656:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY | ... omitted 4 union elements`, found `Literal[0]`
+ src/bokeh/models/glyphs.py:1656:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown]] | XY[Property[Unknown]] | ... omitted 4 union elements`, found `Literal[0]`
- src/bokeh/models/glyphs.py:1663:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners | UndefinedType | IntrinsicType`, found `Literal[0]`
+ src/bokeh/models/glyphs.py:1663:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Property[Unknown] | tuple[Property[Unknown], Property[Unknown], Property[Unknown], Property[Unknown]] | Corners[Property[Unknown]] | UndefinedType | IntrinsicType`, found `Literal[0]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5107 diagnostics
+ Found 5106 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @AlexWaygood on 2025-12-24 14:39_

---

_Closed by @AlexWaygood on 2025-12-24 14:39_

---

_Branch deleted on 2025-12-24 14:39_

---
