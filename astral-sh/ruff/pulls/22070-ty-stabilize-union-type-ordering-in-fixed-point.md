```yaml
number: 22070
title: "[ty] stabilize union-type ordering in fixed-point iteration"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cycle-union-order
created_at: 2025-12-19T07:49:50Z
updated_at: 2025-12-23T00:16:03Z
url: https://github.com/astral-sh/ruff/pull/22070
synced_at: 2026-01-12T15:57:40Z
```

# [ty] stabilize union-type ordering in fixed-point iteration

---

_@mtshiba_

## Summary

This PR fixes https://github.com/astral-sh/ty/issues/2085.

Based on the reported code, the panicking MRE is:

```python
class Test:
    def __init__(self, x: int):
        self.left = x
        self.right = x
    def method(self):
        self.left, self.right = self.right, self.left
        if self.right:
            self.right = self.right
```

The type inference (`implicit_attribute_inner`) for `self.right` proceeds as follows:

```
0: Divergent(Id(6c07))
1: Unknown | int | (Divergent(Id(1c00)) & ~AlwaysFalsy)
2: Unknown | int | (Divergent(Id(6c07)) & ~AlwaysFalsy) | (Divergent(Id(1c00)) & ~AlwaysFalsy)
3: Unknown | int | (Divergent(Id(1c00)) & ~AlwaysFalsy) | (Divergent(Id(6c07)) & ~AlwaysFalsy)
4: Unknown | int | (Divergent(Id(6c07)) & ~AlwaysFalsy) | (Divergent(Id(1c00)) & ~AlwaysFalsy)
...
```

The problem is that the order of union types is not stable between cycles. To solve this, when unioning the previous union type with the current union type, we should use the previous type as the base and add only the new elements in this cycle (In the current implementation, this unioning order was reversed).

## Test Plan

New corpus test


---

_Label `ty` added by @mtshiba on 2025-12-19 07:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 07:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 07:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- parso/python/pep8.py:258:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:258:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:259:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:259:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:263:17: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:263:17: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:270:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:270:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:271:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:271:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:276:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:276:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:277:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:277:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/ast/interpreter.py:299:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`, found `BaseNode & Top[list[Unknown]]`
+ mesonbuild/ast/interpreter.py:299:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int | Sequence[Divergent] | ... omitted 5 union elements]`, found `BaseNode & Top[list[Unknown]]`
- mesonbuild/ast/interpreter.py:301:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`, found `BaseNode & Top[dict[Unknown, Unknown]]`
+ mesonbuild/ast/interpreter.py:301:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str | int | Sequence[Divergent] | ... omitted 5 union elements]`, found `BaseNode & Top[dict[Unknown, Unknown]]`
- mesonbuild/ast/interpreter.py:556:48: error[invalid-argument-type] Argument to bound method `node_to_runtime_value` is incorrect: Expected `MesonInterpreterObject | BaseNode | Sequence[Divergent] | ... omitted 6 union elements`, found `object`
+ mesonbuild/ast/interpreter.py:556:48: error[invalid-argument-type] Argument to bound method `node_to_runtime_value` is incorrect: Expected `MesonInterpreterObject | BaseNode | str | ... omitted 6 union elements`, found `object`
- mesonbuild/ast/interpreter.py:731:28: error[invalid-argument-type] Argument to function `src_to_abs` is incorrect: Expected `str | IntrospectionFile | UnknownValue`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/ast/interpreter.py:731:28: error[invalid-argument-type] Argument to function `src_to_abs` is incorrect: Expected `str | IntrospectionFile | UnknownValue`, found `str | int | Sequence[Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:1465:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/interpreter/interpreter.py:1465:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `str | int | Sequence[Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:2225:19: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `EnvironmentVariables | dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | str`
+ mesonbuild/interpreter/interpreter.py:2225:19: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `EnvironmentVariables | dict[str, str | int | Sequence[Divergent] | ... omitted 5 union elements] | list[str | int | Sequence[Divergent] | ... omitted 5 union elements] | str`
- mesonbuild/interpreter/interpreter.py:2231:33: error[invalid-argument-type] Argument is incorrect: Expected `EnvironmentVariables | list[Unknown] | dict[Unknown, Unknown] | str | None`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/interpreter/interpreter.py:2231:33: error[invalid-argument-type] Argument is incorrect: Expected `EnvironmentVariables | list[Unknown] | dict[Unknown, Unknown] | str | None`, found `str | int | Sequence[Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:2253:37: error[invalid-argument-type] Argument to bound method `unpack_env_kwarg` is incorrect: Expected `EnvironmentVariables | dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | str`, found `BaseTest`
+ mesonbuild/interpreter/interpreter.py:2253:37: error[invalid-argument-type] Argument to bound method `unpack_env_kwarg` is incorrect: Expected `EnvironmentVariables | dict[str, str | int | Sequence[Divergent] | ... omitted 5 union elements] | list[str | int | Sequence[Divergent] | ... omitted 5 union elements] | str`, found `BaseTest`
- mesonbuild/interpreter/interpreter.py:2748:41: error[invalid-argument-type] Argument to bound method `run_command_impl` is incorrect: Expected `tuple[Executable | ExternalProgram | Compiler | File | str, list[Executable | ExternalProgram | Compiler | File | str]]`, found `tuple[str | ExternalProgram, list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]]`
+ mesonbuild/interpreter/interpreter.py:2748:41: error[invalid-argument-type] Argument to bound method `run_command_impl` is incorrect: Expected `tuple[Executable | ExternalProgram | Compiler | File | str, list[Executable | ExternalProgram | Compiler | File | str]]`, found `tuple[str | ExternalProgram, list[str | int | Sequence[Divergent] | ... omitted 5 union elements]]`
- mesonbuild/interpreter/interpreter.py:3037:46: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `EnvironmentVariables | list[str] | list[list[str]] | ... omitted 3 union elements`, found `str | list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements] | dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`
+ mesonbuild/interpreter/interpreter.py:3037:46: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `EnvironmentVariables | list[str] | list[list[str]] | ... omitted 3 union elements`, found `str | list[str | int | Sequence[Divergent] | ... omitted 5 union elements] | dict[str, str | int | Sequence[Divergent] | ... omitted 5 union elements]`
- mesonbuild/interpreter/interpreter.py:3037:52: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `Literal["set", "prepend", "append"]`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/interpreter/interpreter.py:3037:52: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `Literal["set", "prepend", "append"]`, found `str | int | Sequence[Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:3037:70: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `str`, found `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`
+ mesonbuild/interpreter/interpreter.py:3037:70: error[invalid-argument-type] Argument to function `env_convertor_with_method` is incorrect: Expected `str`, found `str | int | Sequence[Divergent] | ... omitted 5 union elements`
- mesonbuild/interpreter/interpreter.py:3590:36: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreter/interpreter.py:3590:36: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreter/interpreter.py:3603:20: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
+ mesonbuild/interpreter/interpreter.py:3603:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
- mesonbuild/interpreter/interpreter.py:3606:24: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
+ mesonbuild/interpreter/interpreter.py:3606:24: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `InterpreterObject`
- mesonbuild/interpreter/interpreter.py:3606:40: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `~None`
+ mesonbuild/interpreter/interpreter.py:3606:40: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `~None`
- mesonbuild/interpreter/primitives/array.py:59:44: error[invalid-argument-type] Argument to function `check_contains` is incorrect: Expected `list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements]`, found `(Sequence[Divergent] & Top[list[Unknown]]) | (Sequence[Divergent] & Top[list[Unknown]]) | (HoldableObject & Top[list[Unknown]]) | (MesonInterpreterObject & Top[list[Unknown]])`
+ mesonbuild/interpreter/primitives/array.py:59:44: error[invalid-argument-type] Argument to function `check_contains` is incorrect: Expected `list[str | int | Sequence[Divergent] | ... omitted 5 union elements]`, found `(Sequence[Divergent] & Top[list[Unknown]]) | (HoldableObject & Top[list[Unknown]]) | (MesonInterpreterObject & Top[list[Unknown]]) | (Sequence[Divergent] & Top[list[Unknown]])`
- mesonbuild/interpreter/primitives/array.py:123:40: error[invalid-argument-type] Argument to function `flatten` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreter/primitives/array.py:123:40: error[invalid-argument-type] Argument to function `flatten` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/_unholder.py:18:16: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `HoldableObject | int | str | Top[list[Unknown]] | Top[dict[Unknown, Unknown]]`
+ mesonbuild/interpreterbase/_unholder.py:18:16: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `HoldableObject | int | str | Top[list[Unknown]] | Top[dict[Unknown, Unknown]]`
- mesonbuild/interpreterbase/decorators.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseNode, list[Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements], dict[str, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements], SubProject]`, found `tuple[Any, None | Any, None | Any, Any]`
+ mesonbuild/interpreterbase/decorators.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseNode, list[str | int | Sequence[Divergent] | ... omitted 5 union elements], dict[str, str | int | Sequence[Divergent] | ... omitted 5 union elements], SubProject]`, found `tuple[Any, None | Any, None | Any, Any]`
- mesonbuild/interpreterbase/helpers.py:31:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `~Top[list[Unknown]] & ~StringNode`
+ mesonbuild/interpreterbase/helpers.py:31:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `~Top[list[Unknown]] & ~StringNode`
- mesonbuild/interpreterbase/helpers.py:37:30: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:37:30: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:39:20: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `dict[object, Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 6 union elements]`
+ mesonbuild/interpreterbase/helpers.py:39:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `dict[object, str | int | Sequence[Divergent] | ... omitted 6 union elements]`
- mesonbuild/interpreterbase/helpers.py:39:33: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:39:33: error[invalid-argument-type] Argument to function `resolver` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:58:59: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:58:59: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:60:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:60:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/helpers.py:61:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreterbase/helpers.py:61:54: error[invalid-argument-type] Argument to function `stringifyUserArguments` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/interpreterbase.py:295:67: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:295:67: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:310:60: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:310:60: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:363:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:363:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:371:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:371:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:379:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:379:54: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:387:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:387:68: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:396:70: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:396:70: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:420:64: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `None`
+ mesonbuild/interpreterbase/interpreterbase.py:420:64: error[invalid-argument-type] Argument to bound method `operator_call` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `None`
- mesonbuild/interpreterbase/interpreterbase.py:615:41: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/interpreterbase/interpreterbase.py:615:41: error[invalid-argument-type] Argument to bound method `_holderify` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/interpreterbase/interpreterbase.py:632:37: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `(InterpreterObject & ~MutableInterpreterObject) | None | (InterpreterObject & MutableInterpreterObject)`
+ mesonbuild/interpreterbase/interpreterbase.py:632:37: error[invalid-argument-type] Argument to bound method `set_variable` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `(InterpreterObject & ~MutableInterpreterObject) | None | (InterpreterObject & MutableInterpreterObject)`
- mesonbuild/modules/hotdoc.py:135:48: error[invalid-argument-type] Argument to bound method `check_extra_arg_type` is incorrect: Expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `object`
+ mesonbuild/modules/hotdoc.py:135:48: error[invalid-argument-type] Argument to bound method `check_extra_arg_type` is incorrect: Expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `object`
- mesonbuild/optinterpreter.py:131:20: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Divergent] | int | dict[str, Divergent] | ... omitted 5 union elements`, found `int | float`
+ mesonbuild/optinterpreter.py:131:20: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `int | float`
- mesonbuild/rewriter.py:774:83: error[unresolved-attribute] Object of type `(Sequence[Divergent] & ~UnknownValue & ~str) | (int & ~UnknownValue) | (dict[str, Divergent] & ~UnknownValue) | ... omitted 4 union elements` has no attribute `to_abs_path`
+ mesonbuild/rewriter.py:774:83: error[unresolved-attribute] Object of type `(int & ~UnknownValue) | (Sequence[Divergent] & ~UnknownValue & ~str) | (dict[str, Divergent] & ~UnknownValue) | ... omitted 4 union elements` has no attribute `to_abs_path`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:1123:22: error[unresolved-attribute] Object of type `(trigger_job_run_options: TriggerJobRunOptions | None = None) -> DbtCloudJobRun | Coroutine[Any, Any, DbtCloudJobRun]` has no attribute `aio`
+ src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:1123:22: error[unresolved-attribute] Object of type `(trigger_job_run_options: TriggerJobRunOptions | None = ...) -> DbtCloudJobRun | Coroutine[Any, Any, DbtCloudJobRun]` has no attribute `aio`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/feature_selection/_univariate_selection.py:886:16: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | ndarray[tuple[Any, ...], dtype[Unknown]] | None` and `Unknown | float`
+ sklearn/feature_selection/_univariate_selection.py:886:16: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None` and `Unknown | float`
- sklearn/feature_selection/_univariate_selection.py:970:26: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | ndarray[tuple[Any, ...], dtype[Unknown]] | None`
+ sklearn/feature_selection/_univariate_selection.py:970:26: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None`
- sklearn/feature_selection/_univariate_selection.py:1052:16: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | ndarray[tuple[Any, ...], dtype[Unknown]] | None` and `Unknown | int | float`
+ sklearn/feature_selection/_univariate_selection.py:1052:16: error[unsupported-operator] Operator `<` is not supported between objects of type `Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None` and `Unknown | int | float`
- sklearn/feature_selection/_univariate_selection.py:1052:49: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | ndarray[tuple[Any, ...], dtype[Unknown]] | None`
+ sklearn/feature_selection/_univariate_selection.py:1052:49: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | ndarray[tuple[Any, ...], dtype[Any]] | None`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5086 diagnostics
+ Found 5085 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2799 diagnostics
+ Found 2802 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14415 diagnostics
+ Found 14414 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-19 07:54_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 08:00_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 0 | 38 |
| `invalid-return-type` | 0 | 0 | 12 |
| `possibly-missing-attribute` | 0 | 0 | 10 |
| `invalid-assignment` | 0 | 0 | 5 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unsupported-operator` | 0 | 0 | 2 |
| **Total** | **0** | **0** | **69** |




---

_Marked ready for review by @mtshiba on 2025-12-19 13:19_

---

_Review requested from @carljm by @mtshiba on 2025-12-19 13:19_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-19 13:19_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-19 13:19_

---

_Review requested from @dcreager by @mtshiba on 2025-12-19 13:19_

---

_@carljm approved on 2025-12-23 00:15_

---

_Merged by @carljm on 2025-12-23 00:16_

---

_Closed by @carljm on 2025-12-23 00:16_

---
