```yaml
number: 17479
title: "[red-knot] colorize concise output diagnostics (#17232)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: red_knot_colorize_concise_output
created_at: 2025-04-19T13:29:15Z
updated_at: 2025-04-29T14:07:16Z
url: https://github.com/astral-sh/ruff/pull/17479
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] colorize concise output diagnostics (#17232)

---

_@Kalmaegi_

## Summary

Fix #17232 

Enable colorized output for diagnostics when using the `--output-format=concise` option. Previously, even with `--color=always`, concise output was not colorized, making it harder to visually distinguish severity and rule information in terminal output.

This change applies appropriate colors and styles to the severity, rule ID, file path, and message in concise output, improving readability and user experience.

The color scheme follows the implementation in `styled_buffer`, ensuring consistency with the detailed output mode.

The implementation uses the `colored` crate to assign:
- Bright cyan for info
- Bright yellow for warning
- Bright red for error and fatal
- Bright white for rule
- Bright red for concise_message


Before:
![image](https://github.com/user-attachments/assets/2681ccb6-f176-4098-bf22-363878c0d7a6)
After:
![image](https://github.com/user-attachments/assets/847b663f-e942-4e3c-8fd3-7897aa73e7fa)


And full mode is :
![image](https://github.com/user-attachments/assets/49a769af-e8d0-4101-9594-8d5d003e78ac)


## Test Plan

If need a Test, i will to write


---

_Review requested from @carljm by @Kalmaegi on 2025-04-19 13:29_

---

_Review requested from @MichaReiser by @Kalmaegi on 2025-04-19 13:29_

---

_Review requested from @AlexWaygood by @Kalmaegi on 2025-04-19 13:29_

---

_Review requested from @sharkdp by @Kalmaegi on 2025-04-19 13:29_

---

_Review requested from @dcreager by @Kalmaegi on 2025-04-19 13:29_

---

_Comment by @github-actions[bot] on 2025-04-20 12:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @github-actions[bot] on 2025-04-20 13:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/common.py:3:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/common.py:3:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_type.py:12:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/data.py:2:39: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/data.py:2:39: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/data.py:4:50: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/data.py:4:50: Unused blanket `type: ignore` directive
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_convert_key.py:17:32: Argument to this function is incorrect: Expected `Data`, found `dict`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/config.py:8:44: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/config.py:11:33: Unused blanket `type: ignore` directive
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:4:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:4:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:14:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:14:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:26:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:26:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:38:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:38:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:50:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_dataclasses.py:50:30: Argument to this function is incorrect: Expected `DataclassInstance | type[DataclassInstance]`, found `Literal[X]`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:4:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:19:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:30:46: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:33:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:44:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:57:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:52:38: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_forward_reference.py:35:32: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_forward_reference.py:43:30: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_forward_reference.py:51:40: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_init_var.py:16:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:71:38: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_init_var.py:35:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:84:41: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_init_var.py:50:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/cache.py:11:71: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/cache.py:11:71: Unused blanket `type: ignore` directive
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:30:46: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:52:38: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_type.py:12:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:4:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:71:38: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:19:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:33:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:84:41: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:44:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_literal.py:57:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_init_var.py:16:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_init_var.py:35:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_init_var.py:50:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/dataclasses.py:17:45: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/dataclasses.py:17:45: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/dataclasses.py:18:41: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/dataclasses.py:18:41: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/config.py:8:44: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/config.py:11:33: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_forward_reference.py:35:32: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_forward_reference.py:43:30: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_forward_reference.py:51:40: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:4:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:4:8: Cannot resolve import `pytest`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:14:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:32:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:43:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:65:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:85:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:95:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:15:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:105:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:26:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:36:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:46:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:114:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:57:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:69:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:84:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:98:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:114:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:124:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:128:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:140:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:139:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:155:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:154:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:165:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:168:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:177:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:182:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_convert_key.py:17:32: Argument to this function is incorrect: Expected `Data`, found `dict`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:5:36: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:7:47: Unused blanket `type: ignore` directive
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:17:16: Type `type` has no attribute `__extra__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:19:16: Type `type` has no attribute `__origin__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:43:30: Type `type` has no attribute `__origin__`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:62:37: Unused blanket `type: ignore` directive
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:64:38: Type `type` has no attribute `__origin__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:76:12: Type `type` has no attribute `__supertype__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:87:16: Type `type` has no attribute `type`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:108:12: Type `type` has no attribute `__args__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:109:21: Type `type` has no attribute `__args__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:128:16: Type `type` has no attribute `__origin__`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:180:54: Unused blanket `type: ignore` directive
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/test_types.py:6:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/test_types.py:6:8: Cannot resolve import `pytest`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:79:21: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[float]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:79:21: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[float]`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:121:24: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[float]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:121:24: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[float]`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:126:28: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[float]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:126:28: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[float]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_types.py:227:29: Argument to this function is incorrect: Expected `type`, found `typing.List`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_types.py:227:29: Argument to this function is incorrect: Expected `type`, found `typing.List`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_types.py:231:33: Argument to this function is incorrect: Expected `type`, found `typing.Dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/test_types.py:231:33: Argument to this function is incorrect: Expected `type`, found `typing.Dict`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:412:27: Operator `|` is unsupported between objects of type `Literal[str]` and `None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/tests/test_types.py:412:27: Operator `|` is unsupported between objects of type `Literal[str]` and `None`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:4:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:14:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:32:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:43:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:65:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:85:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:95:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:105:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:114:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:124:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:140:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:154:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:168:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_union.py:182:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/generics.py:10:55: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/generics.py:10:55: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/generics.py:12:66: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/generics.py:12:66: Unused blanket `type: ignore` directive
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/dacite/generics.py:64:72: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[str]`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/dacite/dacite/generics.py:64:72: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[str]`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/generics.py:96:21: Type `type` has no attribute `__orig_bases__`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/generics.py:96:21: Type `type` has no attribute `__orig_bases__`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:4:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:4:8: Cannot resolve import `pytest`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:15:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:26:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:36:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:46:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:57:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:69:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:84:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:98:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:114:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:128:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:139:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:155:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:165:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_optional.py:177:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:4:8: Cannot resolve import `pytest`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:15:29: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:25:29: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:36:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:47:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:58:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:70:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:88:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:105:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:120:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:130:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:14:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:140:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:28:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:42:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:56:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:66:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:76:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:88:38: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:104:38: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:114:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:154:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:128:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:143:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:153:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:164:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:176:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:187:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:167:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:177:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:197:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:190:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:200:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:215:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:212:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:229:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:248:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:224:23: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:259:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:225:23: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:269:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:279:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:289:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:299:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:309:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:5:36: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:7:47: Unused blanket `type: ignore` directive
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:17:16: Type `type` has no attribute `__extra__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:19:16: Type `type` has no attribute `__origin__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:43:30: Type `type` has no attribute `__origin__`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:62:37: Unused blanket `type: ignore` directive
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:64:38: Type `type` has no attribute `__origin__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:76:12: Type `type` has no attribute `__supertype__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:87:16: Type `type` has no attribute `type`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:108:12: Type `type` has no attribute `__args__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:109:21: Type `type` has no attribute `__args__`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dacite/dacite/types.py:128:16: Type `type` has no attribute `__origin__`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/dacite/dacite/types.py:180:54: Unused blanket `type: ignore` directive
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:5:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:21:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:21:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:31:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:31:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:41:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:41:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:51:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:51:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:61:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:61:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:74:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:74:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:87:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:87:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:97:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:97:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:107:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:107:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:121:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:121:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:133:25: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:133:25: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:147:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:147:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:158:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:158:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:161:24: Argument to this function is incorrect: Expected `int`, found `Literal["test"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:161:24: Argument to this function is incorrect: Expected `int`, found `Literal["test"]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:169:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:169:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:172:24: Argument to this function is incorrect: Expected `int | float`, found `Literal["test"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:172:24: Argument to this function is incorrect: Expected `int | float`, found `Literal["test"]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:181:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:181:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:204:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:204:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:226:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:226:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:4:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:4:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:15:29: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:25:29: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:36:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:47:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:58:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:70:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:88:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:105:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:120:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:130:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:140:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:14:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:28:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:42:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:56:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:66:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:76:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:88:38: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:104:38: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:154:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:114:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:128:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:143:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:153:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:164:22: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:176:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:167:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:187:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:177:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:197:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:190:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:200:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:212:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:215:27: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:224:23: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:229:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:248:22: Argument to this function is incorrect: Expected `Data`, found `dict`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_base.py:225:23: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:259:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:269:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:279:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:289:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:299:27: Argument to this function is incorrect: Expected `Data`, found `dict`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_collection.py:309:27: Argument to this function is incorrect: Expected `Data`, found `dict`

zipp (https://github.com/jaraco/zipp)
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/zipp/zipp/compat/py310.py:10:23: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/zipp/zipp/compat/py310.py:10:23: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/zipp/zipp/compat/overlay.py:37:47: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/zipp/zipp/compat/overlay.py:37:47: Unused blanket `type: ignore` directive
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/zipp/zipp/glob.py:4:26: Operator `*` is unsupported between objects of type `str` and `bool`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/zipp/zipp/glob.py:4:26: Operator `*` is unsupported between objects of type `str` and `bool`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:175:31: Argument to this function is incorrect: Expected `SizedBuffer | str`, found `Literal[b""]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:175:31: Argument to this function is incorrect: Expected `SizedBuffer | str`, found `Literal[b""]`

bidict (https://github.com/jab/bidict)
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/docs/conftest.py:14:8: Cannot resolve import `pytest`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/conftest.py:7:6: Cannot resolve import `hypothesis`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/conftest.py:7:6: Cannot resolve import `hypothesis`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/docs/conftest.py:14:8: Cannot resolve import `pytest`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/docs/conf.py:50:12: Cannot resolve import `sphinx_copybutton`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/docs/conf.py:50:12: Cannot resolve import `sphinx_copybutton`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/bidict/docs/conf.py:72:10: Type `<module 'bidict'>` has no attribute `metadata`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/bidict/docs/conf.py:72:10: Type `<module 'bidict'>` has no attribute `metadata`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/bidict/docs/conf.py:73:13: Type `<module 'bidict'>` has no attribute `metadata`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/bidict/docs/conf.py:73:13: Type `<module 'bidict'>` has no attribute `metadata`
+ error[lint:invalid-type-form] /tmp/mypy_primer/projects/bidict/bidict/_iter.py:34:7: Type qualifier `typing.Final` is not allowed in type expressions (only in annotation expressions)
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/bidict/bidict/_iter.py:50:34: Object of type `None` is not callable
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:24:29: Module `collections.abc` has no member `Set`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:24:29: Module `collections.abc` has no member `Set`
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:157:24: Name `arg` used when possibly not defined
+ warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:157:24: Name `arg` used when possibly not defined
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/bidict/bidict/_iter.py:34:7: Type qualifier `typing.Final` is not allowed in type expressions (only in annotation expressions)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/bidict/bidict/_iter.py:50:34: Object of type `None` is not callable
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/microbenchmarks.py:20:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/microbenchmarks.py:20:8: Cannot resolve import `pytest`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/microbenchmarks.py:25:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[deque]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/microbenchmarks.py:25:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[deque]`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/bidict/tests/bidict_test_fixtures.py:185:42: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/bidict/tests/bidict_test_fixtures.py:185:42: Unused blanket `type: ignore` directive
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:31:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:31:8: Cannot resolve import `pytest`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:32:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:32:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:33:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:33:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:34:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:34:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:35:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:35:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:36:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:36:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:37:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:37:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:38:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:38:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:39:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:39:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:40:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:40:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:41:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:41:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:42:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:42:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:43:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:43:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:44:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:44:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:45:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:45:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:46:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:46:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:47:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:47:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:48:6: Cannot resolve import `bidict_test_fixtures`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:48:6: Cannot resolve import `bidict_test_fixtures`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:49:6: Cannot resolve import `hypothesis`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:49:6: Cannot resolve import `hypothesis`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:50:6: Cannot resolve import `hypothesis`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:50:6: Cannot resolve import `hypothesis`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:51:6: Cannot resolve import `hypothesis`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:51:6: Cannot resolve import `hypothesis`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:52:6: Cannot resolve import `hypothesis.stateful`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:52:6: Cannot resolve import `hypothesis.stateful`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:53:6: Cannot resolve import `hypothesis.stateful`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:53:6: Cannot resolve import `hypothesis.stateful`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:54:6: Cannot resolve import `hypothesis.stateful`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:54:6: Cannot resolve import `hypothesis.stateful`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:55:6: Cannot resolve import `hypothesis.stateful`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:55:6: Cannot resolve import `hypothesis.stateful`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:56:6: Cannot resolve import `hypothesis.stateful`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:56:6: Cannot resolve import `hypothesis.stateful`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:57:6: Cannot resolve import `hypothesis.strategies`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:57:6: Cannot resolve import `hypothesis.strategies`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:58:6: Cannot resolve import `hypothesis.strategies`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:58:6: Cannot resolve import `hypothesis.strategies`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:59:6: Cannot resolve import `hypothesis.strategies`
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:59:6: Cannot resolve import `hypothesis.strategies`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:88:25: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[OnDup]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:88:25: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[OnDup]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:129:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:129:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:130:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:130:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:135:17: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:135:17: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:136:17: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:136:17: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:178:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:178:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:179:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:179:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:189:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:189:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:190:13: Argument to this function is incorrect: Expected `(...) -> Any`, found `partial`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/...*[Comment body truncated]*

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-04-20 13:38_

---

_Review request for @carljm removed by @carljm on 2025-04-21 14:50_

---

_Label `cli` added by @dhruvmanila on 2025-04-21 16:06_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-21 16:06_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:65 on 2025-04-22 11:33_

We should respect the `config.color` option even for concise diagnostics similar to how it's done for the full diagnostic format here

https://github.com/astral-sh/ruff/blob/bd5b8a9ec6a1877c6d457c1f2e52e57ea37679de/crates/ruff_db/src/diagnostic/render.rs#L45-L49

and it would be great if we could reuse the same colors as for the full output format. Unfortunately, they're defined in the annotate_snippets crate. 

One option is to have our own `Stylesheet` struct in the `diagnostic` crate with a `plain` and `colored` (or `styled`) constructor with which we then initialize the renderer used in the full output format (by calling all the setter methods).

Unless @BurntSushi prefers to make `Stylesheet` public

---

_@MichaReiser reviewed on 2025-04-22 11:33_

---

_Review comment by @BurntSushi on `crates/red_knot/tests/cli.rs`:1089 on 2025-04-23 13:03_

It looks like these snapshots have some extra output in them?

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:65 on 2025-04-23 13:05_

Yeah for me, I think making sure we respect `config.color` is a blocker to merging this PR. That would make this a strict improvement I think.

But I do think that further synchronizing the colors can be done in another PR. And I also think it would be good to do. I'm not sure if we want to expose the ability for callers to set individual colors, but at least having two public profiles `styled` and `plain` makes sense to me. (I think this is the same thing you're saying @MichaReiser.)

---

_@BurntSushi requested changes on 2025-04-23 13:05_

---

_Comment by @MichaReiser on 2025-04-28 06:58_

@Kalmaegi do you want to follow up on the PR feedback or would you prefer for me or @BurntSushi to take over? We would like to land this change this week if possible.

---

_Comment by @Kalmaegi on 2025-04-28 09:56_

> @Kalmaegi do you want to follow up on the PR feedback or would you prefer for me or @BurntSushi to take over? We would like to land this change this week if possible.

I would be happy to improve this. Can start the task in a few hours. I misunderstood previously; I thought that confirmation was required before starting the task XD.

---

_Converted to draft by @Kalmaegi on 2025-04-28 16:44_

---

_Comment by @Kalmaegi on 2025-04-28 16:45_

Hi@MichaReiser @BurntSushi,
I‘ve just pushed a very basic version of the new feature. I would really appreciate it if you could take a quick look and let me know if my overall approach makes sense. If everything looks good, I’ll continue refining and expanding the implementation.

Thank you for your feedback！

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:14 on 2025-04-28 16:59_

Is this file not included here?

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:76 on 2025-04-28 16:59_

I would expect the stylesheet to be used here in some capacity.

---

_@BurntSushi requested changes on 2025-04-28 16:59_

---

_Comment by @Kalmaegi on 2025-04-28 17:24_

 I have only updated the logic for the full format branch. If this approach is correct, I will continue to improve the logic for the concise branch.

---

_Comment by @BurntSushi on 2025-04-28 17:35_

I think it looks fine?

Please do try to get CI green. As of now, it looks like there is code missing that I can't see.

Doing reviews like this piecemeal can be tricky without seeing the full picture, and it's hard to keep track of what I should comment on and what I shouldn't.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-29 06:38_

---

_Marked ready for review by @Kalmaegi on 2025-04-29 12:00_

---

_Comment by @Kalmaegi on 2025-04-29 12:02_

@BurntSushi I have improved the logic and it works well now. However, I didn't build a full Renderable object—I've just handled the sheet coloring in a simple way.

---

_Comment by @MichaReiser on 2025-04-29 12:02_

@Kalmaegi I see that CI is passing now. Thank you. Is the PR ready to review?

---

_Comment by @Kalmaegi on 2025-04-29 12:28_

> @Kalmaegi I see that CI is passing now. Thank you. Is the PR ready to review?



Yes, I’m ready, but there are two points I’m not sure about—I’ve mentioned them in the review. Any help would be greatly appreciated.

---

_Comment by @MichaReiser on 2025-04-29 12:34_

I'm reviewing this now

---

_@MichaReiser approved on 2025-04-29 12:46_

This looks good, thank you. I pushed a few changes. Mainly to avoid some unnecessary allocations and to use emphasis less often

---

_Comment by @MichaReiser on 2025-04-29 12:47_

I leave merging to @BurntSushi who's blocking the merge right now ;)

---

_Comment by @Kalmaegi on 2025-04-29 12:57_

> This looks good, thank you. I pushed a few changes. Mainly to avoid some unnecessary allocations and to use emphasis less often

Thank you for your help, I'm glad I could help

---

_@BurntSushi requested changes on 2025-04-29 13:04_

This LGTM, but it looks like this PR adds a file called `output.txt` to the root of the repo? It looks like it's just log output. Could you remove it please? Then this should be good to go.

---

_Comment by @BurntSushi on 2025-04-29 13:06_

Nevermind, I sometimes forget that I can just push straight to PR branches and make changes like this. I did that.

Also, @MichaReiser, it looks like you added the `output.txt` file. :P 

---

_@BurntSushi approved on 2025-04-29 13:06_

---

_Comment by @MichaReiser on 2025-04-29 14:07_

> Also, @MichaReiser, it looks like you added the output.txt file. :P

Whoops. Thanks for catching this! This is how I debug salsa panics

---

_Merged by @MichaReiser on 2025-04-29 14:07_

---

_Closed by @MichaReiser on 2025-04-29 14:07_

---
