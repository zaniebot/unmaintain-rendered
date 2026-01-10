```yaml
number: 18452
title: "[ty] Surface matched overload diagnostic directly"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: dhruv/matched-overload-diagnostic
created_at: 2025-06-04T03:32:08Z
updated_at: 2025-06-20T11:41:58Z
url: https://github.com/astral-sh/ruff/pull/18452
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Surface matched overload diagnostic directly

---

_Pull request opened by @dhruvmanila on 2025-06-04 03:32_

## Summary

This PR resolves the way diagnostics are reported for an invalid call to an overloaded function.

If any of the steps in the overload call evaluation algorithm yields a matching overload but it's type checking that failed, the `no-matching-overload` diagnostic is incorrect because there is a matching overload, it's the arguments passed that are invalid as per the signature. So, this PR improves that by surfacing the diagnostics on the matching overload directly.

It also provides additional context, specifically the matching overload where this error occurred and other non-matching overloads. Consider the following example:

```py
from typing import overload


@overload
def f() -> None: ...
@overload
def f(x: int) -> int: ...
@overload
def f(x: int, y: int) -> int: ...
def f(x: int | None = None, y: int | None = None) -> int | None:
    return None


f("a")
```

We get:

<img width="857" alt="Screenshot 2025-06-18 at 11 07 10" src="https://github.com/user-attachments/assets/8dbcaf13-2a74-4661-aa94-1225c9402ea6" />


## Test Plan

Update test cases, resolve existing todos and validate the updated snapshots.


---

_Label `ty` added by @dhruvmanila on 2025-06-04 03:32_

---

_Label `diagnostics` added by @dhruvmanila on 2025-06-04 03:32_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_f…_-_Try_to_cover_all_pos…_-_Cover_non-keyword_re…_(707b284610419a54).snap`:125 on 2025-06-04 03:34_

This seems correct to me as the reveal type of `x` in the following gives us the bound method:

```py
from inspect import getattr_static
from typing import reveal_type


class OverloadExample:
    def f(self, x: str) -> int:
        return 0


f5 = getattr_static(OverloadExample, "f").__get__

# ty: Revealed type: `bound method Literal[3].f(x: str) -> int` [revealed-type]
reveal_type(f5(3))
```

So, it seems like this test is actually incorrect.

The overload matching did yield a matching overload but still it reported `no-matching-overload` diagnostic. You can see this in the following logs which suggests that the matching overload is at index 1 after the arity check and the type checking shouldn't fail either because `Literal[3]` is assignable to `~None`:

```
INFO matching overload index: 1
INFO overload signature: (instance: ~None, owner: type | None = None, /) -> Unknown
INFO argument types: Literal[3]
```

The reason this was happening earlier is that we'd unconditionally report `no-matching-overload` without actually checking whether this is the case. This is the branch where that is happening:

https://github.com/astral-sh/ruff/blob/a2cd6df429a3e76880a48a5eb8816a86c36927ed/crates/ty_python_semantic/src/types/call/bind.rs#L1590

---

_@dhruvmanila reviewed on 2025-06-04 03:34_

---

_Comment by @github-actions[bot] on 2025-06-04 03:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[no-matching-overload] mypy_primer/utils.py:33:12: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] mypy_primer/utils.py:33:16: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Path`

python-chess (https://github.com/niklasf/python-chess)
- error[no-matching-overload] chess/pgn.py:1857:17: No overload of function `read_game` matches arguments
+ error[invalid-argument-type] chess/pgn.py:1857:35: Argument to function `read_game` is incorrect: Expected `() -> BaseVisitor[Unknown]`, found `<class 'SkipVisitor'>`

Expression (https://github.com/cognitedata/Expression)
- error[no-matching-overload] expression/extra/option/pipeline.py:91:12: No overload of function `reduce` matches arguments
+ error[invalid-argument-type] expression/extra/option/pipeline.py:91:19: Argument to function `reduce` is incorrect: Expected `(def Some(value: _T1) -> Option[_T1], (Any, /) -> Option[Any], /) -> def Some(value: _T1) -> Option[_T1]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
- error[no-matching-overload] expression/extra/result/pipeline.py:96:12: No overload of function `reduce` matches arguments
+ error[invalid-argument-type] expression/extra/result/pipeline.py:96:19: Argument to function `reduce` is incorrect: Expected `(def Ok(value: _TSource) -> Result[_TSource, Any], (Any, /) -> Result[Any, Any], /) -> def Ok(value: _TSource) -> Result[_TSource, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
- error[no-matching-overload] tests/test_array.py:307:21: No overload of function `reduce` matches arguments
+ error[invalid-argument-type] tests/test_array.py:307:38: Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
- error[no-matching-overload] tests/test_block.py:274:21: No overload of function `reduce` matches arguments
+ error[invalid-argument-type] tests/test_block.py:274:38: Argument to function `reduce` is incorrect: Expected `(Literal[0], int, /) -> Literal[0]`, found `def folder(x: int, y: int) -> int`
- error[no-matching-overload] tests/test_map.py:49:10: No overload of function `pipe` matches arguments
+ error[invalid-argument-type] tests/test_map.py:49:19: Argument to function `pipe` is incorrect: Expected `(Map[str, int], /) -> Unknown`, found `def to_seq(table: Map[_Key, _Value]) -> Iterable[tuple[_Key, _Value]]`
- error[no-matching-overload] tests/test_parser.py:230:12: No overload of function `pipe` matches arguments
+ error[invalid-argument-type] tests/test_parser.py:232:9: Argument to function `pipe` is incorrect: Expected `(Parser[str], /) -> Unknown`, found `def many(parser: Parser[_A]) -> Parser[Block[_A]]`
- error[no-matching-overload] tests/test_result.py:487:12: No overload of bound method `pipe` matches arguments
+ error[invalid-argument-type] tests/test_result.py:487:24: Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def swap(result: Result[_TSource, _TError]) -> Result[_TError, _TSource]`
- error[no-matching-overload] tests/test_result.py:573:12: No overload of bound method `pipe` matches arguments
+ error[invalid-argument-type] tests/test_result.py:573:24: Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def merge(result: Result[_TSource, _TSource]) -> _TSource`

attrs (https://github.com/python-attrs/attrs)
- error[no-matching-overload] tests/test_converters.py:25:13: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] tests/test_converters.py:25:23: Argument to bound method `__init__` is incorrect: Expected `(Unknown, AttrsInstance, Attribute[Unknown], /) -> Unknown`, found `<class 'int'>`
- error[no-matching-overload] tests/test_converters.py:59:13: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] tests/test_converters.py:59:23: Argument to bound method `__init__` is incorrect: Expected `(Unknown, AttrsInstance, Attribute[Unknown], /) -> Unknown`, found `None`
- error[no-matching-overload] tests/test_converters.py:104:13: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] tests/test_converters.py:104:23: Argument to bound method `__init__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def wrapped(_, __, ___) -> int | float`

dedupe (https://github.com/dedupeio/dedupe)
- error[no-matching-overload] dedupe/convenience.py:77:12: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] dedupe/convenience.py:77:16: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[Unknown]`
+ error[invalid-argument-type] dedupe/convenience.py:77:19: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[Unknown]`
- Found 63 diagnostics
+ Found 64 diagnostics

rich (https://github.com/Textualize/rich)
- error[no-matching-overload] tests/test_tools.py:8:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:9:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:10:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:11:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:17:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:18:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:19:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:20:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:26:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:27:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:28:12: No overload of function `next` matches arguments
- error[no-matching-overload] tests/test_tools.py:29:12: No overload of function `next` matches arguments
+ error[invalid-argument-type] tests/test_tools.py:8:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:9:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:10:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:11:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:17:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:18:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:19:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:20:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:26:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:27:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:28:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ error[invalid-argument-type] tests/test_tools.py:29:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`

strawberry (https://github.com/strawberry-graphql/strawberry)
+ warning[unused-ignore-comment] strawberry/federation/object_type.py:89:24: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] strawberry/federation/object_type.py:90:9: Argument to function `type` is incorrect: Argument type `T | None` does not satisfy upper bound of type variable `T`
+ warning[unused-ignore-comment] strawberry/types/object_type.py:396:19: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] strawberry/types/object_type.py:397:9: Argument to function `type` is incorrect: Argument type `T | None` does not satisfy upper bound of type variable `T`
+ warning[unused-ignore-comment] strawberry/types/object_type.py:469:19: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] strawberry/types/object_type.py:470:9: Argument to function `type` is incorrect: Argument type `T | None` does not satisfy upper bound of type variable `T`
- Found 369 diagnostics
+ Found 375 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- error[no-matching-overload] sockeye/data_io.py:1147:58: No overload of function `__new__` matches arguments
- error[no-matching-overload] sockeye/data_io.py:1147:58: No overload of function `__new__` matches arguments
- error[no-matching-overload] sockeye/data_io.py:1147:58: No overload of function `__new__` matches arguments
- error[no-matching-overload] sockeye/data_io.py:1147:58: No overload of function `__new__` matches arguments
- error[no-matching-overload] sockeye/data_io.py:1147:58: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sockeye/data_io.py:1149:62: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[tuple[int | float | None, int | float | None]] | None`
+ error[invalid-argument-type] sockeye/data_io.py:1149:62: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[tuple[int | float | None, int | float | None]] | None`
+ error[invalid-argument-type] sockeye/data_io.py:1149:62: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[tuple[int | float | None, int | float | None]] | None`
+ error[invalid-argument-type] sockeye/data_io.py:1149:62: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[tuple[int | float | None, int | float | None]] | None`
+ error[invalid-argument-type] sockeye/data_io.py:1149:62: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[tuple[int | float | None, int | float | None]] | None`
- error[no-matching-overload] test/unit/test_inference.py:177:75: No overload of function `__new__` matches arguments
- error[no-matching-overload] test/unit/test_inference.py:177:75: No overload of function `__new__` matches arguments
- error[no-matching-overload] test/unit/test_inference.py:177:75: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] test/unit/test_inference.py:177:114: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
+ error[invalid-argument-type] test/unit/test_inference.py:177:114: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
+ error[invalid-argument-type] test/unit/test_inference.py:177:114: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`

trio (https://github.com/python-trio/trio)
- error[no-matching-overload] src/trio/_core/_tests/test_run.py:820:9: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] src/trio/_core/_tests/test_run.py:821:9: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] src/trio/_core/_tests/test_run.py:820:42: Argument to bound method `__init__` is incorrect: Expected `(BaseException, /) -> bool`, found `def no_context(exc: RuntimeError) -> bool`
+ error[invalid-argument-type] src/trio/_core/_tests/test_run.py:821:42: Argument to bound method `__init__` is incorrect: Expected `(BaseException, /) -> bool`, found `def no_context(exc: RuntimeError) -> bool`
- error[no-matching-overload] src/trio/_core/_tests/test_run.py:2133:22: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] src/trio/_core/_tests/test_run.py:2133:40: Argument to bound method `__init__` is incorrect: Expected `(BaseException, /) -> bool`, found `def check_traceback(exc: KeyError) -> bool`
- error[no-matching-overload] src/trio/_tests/test_subprocess.py:370:15: No overload of function `run_process` matches arguments
+ error[invalid-argument-type] src/trio/_tests/test_subprocess.py:370:32: Argument to function `run_process` is incorrect: Expected `bytes | bytearray | memoryview[Unknown] | int | HasFileno | None`, found `Literal["oh no, it's text"]`
- error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:71:5: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:74:5: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] src/trio/_tests/type_tests/raisesgroup.py:71:32: Argument to bound method `__init__` is incorrect: Expected `(BaseException, /) -> bool`, found `def check_filenotfound(exc: FileNotFoundError) -> bool`
+ error[invalid-argument-type] src/trio/_tests/type_tests/raisesgroup.py:74:47: Argument to bound method `__init__` is incorrect: Expected `(BaseException, /) -> bool`, found `def check_filenotfound(exc: FileNotFoundError) -> bool`

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- error[no-matching-overload] aiohttp_devtools/logs.py:32:17: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] aiohttp_devtools/logs.py:32:17: Argument to bound method `__init__` is incorrect: Expected `Formatter[str]`, found `Terminal256Formatter[_T]`

mkdocs (https://github.com/mkdocs/mkdocs)
- error[no-matching-overload] mkdocs/config/config_options.py:944:28: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] mkdocs/config/config_options.py:944:28: Argument to bound method `__init__` is incorrect: Expected `SubConfig[LegacyConfig]`, found `SubConfig[ExtraScriptValue]`
- error[no-matching-overload] mkdocs/config/defaults.py:199:18: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] mkdocs/config/defaults.py:199:18: Argument to bound method `__init__` is incorrect: Expected `SubConfig[LegacyConfig]`, found `PropagatingSubConfig[Validation]`
- error[no-matching-overload] mkdocs/tests/config/config_options_tests.py:1585:22: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] mkdocs/tests/config/config_options_tests.py:1585:22: Argument to bound method `__init__` is incorrect: Expected `SubConfig[LegacyConfig]`, found `PropagatingSubConfig[Validation]`

ignite (https://github.com/pytorch/ignite)
- error[no-matching-overload] tests/ignite/distributed/utils/test_native.py:416:29: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] tests/ignite/distributed/utils/test_native.py:416:33: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
- error[no-matching-overload] tests/ignite/metrics/test_hsic.py:168:22: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] tests/ignite/metrics/test_hsic.py:168:28: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `Unknown | int | float`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[no-matching-overload] src/hydra_zen/structured_configs/_implementations.py:2523:49: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:2523:54: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | Mapping[str, None | Sequence[str]]] | None`
- error[no-matching-overload] src/hydra_zen/structured_configs/_implementations.py:3303:49: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] src/hydra_zen/structured_configs/_implementations.py:3303:54: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | Mapping[str, None | Sequence[str]]] | None`
- error[no-matching-overload] src/hydra_zen/third_party/pydantic.py:16:23: No overload of function `validate_call` matches arguments
- warning[unused-ignore-comment] src/hydra_zen/third_party/pydantic.py:17:74: Unused blanket `type: ignore` directive
- error[no-matching-overload] tests/annotations/declarations.py:1240:10: No overload of function `make_custom_builds_fn` matches arguments
+ error[invalid-argument-type] tests/annotations/declarations.py:1240:32: Argument to function `make_custom_builds_fn` is incorrect: Expected `DataclassOptions | None`, found `dict[Unknown, Unknown]`
- Found 598 diagnostics
+ Found 596 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- error[no-matching-overload] Pythonwin/pywin/framework/app.py:268:26: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] Pythonwin/pywin/framework/app.py:268:35: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `str`
- error[no-matching-overload] com/win32com/test/testDynamic.py:61:18: No overload of function `Dispatch` matches arguments
+ error[invalid-argument-type] com/win32com/test/testDynamic.py:61:51: Argument to function `Dispatch` is incorrect: Expected `str | PyIDispatch | tuple[@Todo(Generic tuple specializations), ...] | PyIUnknown`, found `Unknown | PyIID`
- error[no-matching-overload] com/win32comext/shell/demos/create_link.py:64:16: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] com/win32comext/shell/demos/create_link.py:65:13: Argument to function `__new__` is incorrect: Expected `(str, Unknown, /) -> Unknown`, found `None`
- error[no-matching-overload] com/win32comext/shell/test/testShellItem.py:23:22: No overload of function `next` matches arguments
+ error[invalid-argument-type] com/win32comext/shell/test/testShellItem.py:23:27: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `PyIEnumIDlist`
- error[no-matching-overload] com/win32comext/shell/test/testShellItem.py:37:22: No overload of function `next` matches arguments
+ error[invalid-argument-type] com/win32comext/shell/test/testShellItem.py:37:27: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `PyIEnumIDlist`
- error[no-matching-overload] com/win32comext/shell/test/testShellItem.py:60:22: No overload of function `next` matches arguments
+ error[invalid-argument-type] com/win32comext/shell/test/testShellItem.py:60:27: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `PyIEnumIDlist`
- error[no-matching-overload] win32/Demos/win32comport_demo.py:142:28: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/Demos/win32comport_demo.py:142:28: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/Demos/win32comport_demo.py:142:28: No overload of function `ReadFile` matches arguments
+ error[invalid-argument-type] win32/Demos/win32comport_demo.py:142:50: Argument to function `ReadFile` is incorrect: Expected `PyOVERLAPPEDReadBuffer`, found `int`
+ error[invalid-argument-type] win32/Demos/win32comport_demo.py:142:50: Argument to function `ReadFile` is incorrect: Expected `PyOVERLAPPEDReadBuffer`, found `int`
+ error[invalid-argument-type] win32/Demos/win32comport_demo.py:142:50: Argument to function `ReadFile` is incorrect: Expected `PyOVERLAPPEDReadBuffer`, found `int`
- error[no-matching-overload] win32/Demos/win32fileDemo.py:31:16: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/Demos/win32fileDemo.py:31:16: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/Demos/win32fileDemo.py:31:16: No overload of function `ReadFile` matches arguments
+ error[invalid-argument-type] win32/Demos/win32fileDemo.py:31:35: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/Demos/win32fileDemo.py:31:35: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/Demos/win32fileDemo.py:31:35: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
- error[no-matching-overload] win32/test/test_win32file.py:62:24: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:62:24: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:62:24: No overload of function `ReadFile` matches arguments
+ error[invalid-argument-type] win32/test/test_win32file.py:62:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:62:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:62:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
- error[no-matching-overload] win32/test/test_win32file.py:100:25: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:100:25: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:100:25: No overload of function `ReadFile` matches arguments
+ error[invalid-argument-type] win32/test/test_win32file.py:101:13: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:101:13: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:101:13: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
- error[no-matching-overload] win32/test/test_win32file.py:160:24: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:160:24: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:160:24: No overload of function `ReadFile` matches arguments
+ error[invalid-argument-type] win32/test/test_win32file.py:160:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:160:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:160:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
- error[no-matching-overload] win32/test/test_win32file.py:167:24: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:167:24: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:167:24: No overload of function `ReadFile` matches arguments
+ error[invalid-argument-type] win32/test/test_win32file.py:167:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:167:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:167:43: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
- error[no-matching-overload] win32/test/test_win32file.py:338:28: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:338:28: No overload of function `ReadFile` matches arguments
- error[no-matching-overload] win32/test/test_win32file.py:338:28: No overload of function `ReadFile` matches arguments
+ error[invalid-argument-type] win32/test/test_win32file.py:338:47: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:338:47: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`
+ error[invalid-argument-type] win32/test/test_win32file.py:338:47: Argument to function `ReadFile` is incorrect: Expected `int`, found `PyHANDLE`

pwndbg (https://github.com/pwndbg/pwndbg)
- error[no-matching-overload] pwndbg/commands/ida.py:161:14: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] pwndbg/commands/ida.py:161:20: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `Unknown | None`
- error[no-matching-overload] pwndbg/integration/ida.py:280:14: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] pwndbg/integration/ida.py:280:20: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `Unknown | None`
- error[no-matching-overload] pwndbg/integration/ida.py:304:12: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] pwndbg/integration/ida.py:304:16: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`

colour (https://github.com/colour-science/colour)
- error[no-matching-overload] colour/notation/hexadecimal.py:135:13: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] colour/notation/hexadecimal.py:135:17: Argument to function `__new__` is incorrect: Expected `str | bytes | bytearray`, found `list[Unknown]`

dulwich (https://github.com/dulwich/dulwich)
- error[no-matching-overload] dulwich/porcelain.py:1696:14: No overload of function `make_server` matches arguments
+ error[invalid-argument-type] dulwich/porcelain.py:1697:9: Argument to function `make_server` is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] dulwich/porcelain.py:1698:9: Argument to function `make_server` is incorrect: Expected `int`, found `Unknown | None`
- Found 137 diagnostics
+ Found 138 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[no-matching-overload] mitmproxy/io/tnetstring.py:191:16: No overload of class `str` matches arguments
+ error[invalid-argument-type] mitmproxy/io/tnetstring.py:191:20: Argument to class `str` is incorrect: Expected `bytes | bytearray`, found `memoryview[Unknown]`
- error[no-matching-overload] test/mitmproxy/contentviews/test__view_xml_html.py:20:16: No overload of function `next` matches arguments
+ error[invalid-argument-type] test/mitmproxy/contentviews/test__view_xml_html.py:20:21: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[Token]`

werkzeug (https://github.com/pallets/werkzeug)
- error[no-matching-overload] src/werkzeug/wsgi.py:246:25: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] src/werkzeug/wsgi.py:246:30: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `((() -> None) & ~((...) -> object)) | (Iterable[() -> None] & ~((...) -> object))`
- error[no-matching-overload] tests/test_test.py:757:12: No overload of function `next` matches arguments
+ error[invalid-argument-type] tests/test_test.py:757:17: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[bytes]`

pytest (https://github.com/pytest-dev/pytest)
- error[no-matching-overload] testing/typing_raises_group.py:84:5: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] testing/typing_raises_group.py:87:5: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] testing/typing_raises_group.py:84:34: Argument to bound method `__init__` is incorrect: Expected `((BaseException, /) -> bool) | None`, found `def check_filenotfound(exc: FileNotFoundError) -> bool`
+ error[invalid-argument-type] testing/typing_raises_group.py:87:49: Argument to bound method `__init__` is incorrect: Expected `((BaseException, /) -> bool) | None`, found `def check_filenotfound(exc: FileNotFoundError) -> bool`

scrapy (https://github.com/scrapy/scrapy)
- error[no-matching-overload] tests/test_utils_spider.py:21:16: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] tests/test_utils_spider.py:21:21: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `AsyncGenerator[Unknown, None]`

optuna (https://github.com/optuna/optuna)
- error[no-matching-overload] optuna/study/_multi_objective.py:32:46: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] optuna/study/_multi_objective.py:32:50: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `@Todo(list comprehension type) | list[int | float] | None`
- error[no-matching-overload] optuna/study/study.py:1138:64: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] optuna/study/study.py:1138:82: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[int | float] | None`

mypy (https://github.com/python/mypy)
- error[no-matching-overload] mypy/modulefinder.py:41:34: No overload of function `__new__` matches arguments
- error[no-matching-overload] mypy/modulefinder.py:43:32: No overload of function `__new__` matches arguments
- error[no-matching-overload] mypy/modulefinder.py:45:35: No overload of function `__new__` matches arguments
- error[no-matching-overload] mypy/modulefinder.py:47:36: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] mypy/modulefinder.py:41:38: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr]) -> AnyStr, (path: AnyStr) -> AnyStr]`
+ error[invalid-argument-type] mypy/modulefinder.py:43:36: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr]) -> AnyStr, (path: AnyStr) -> AnyStr]`
+ error[invalid-argument-type] mypy/modulefinder.py:45:39: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr]) -> AnyStr, (path: AnyStr) -> AnyStr]`
+ error[invalid-argument-type] mypy/modulefinder.py:47:40: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr]) -> AnyStr, (path: AnyStr) -> AnyStr]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- error[no-matching-overload] tests/test_frame.py:2524:14: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] tests/test_frame.py:2524:19: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Timestamp`

django-stubs (https://github.com/typeddjango/django-stubs)
+ warning[unused-ignore-comment] tests/assert_type/db/models/test_constraints.py:10:20: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] tests/assert_type/db/models/test_constraints.py:12:5: Argument to bound method `__init__` is incorrect: Expected `None`, found `list[Unknown]`
- Found 467 diagnostics
+ Found 469 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[no-matching-overload] src/bokeh/embed/bundle.py:186:25: No overload of function `__new__` matches arguments
- error[no-matching-overload] src/bokeh/embed/bundle.py:189:26: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] src/bokeh/embed/bundle.py:186:29: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `<class 'URL'>`
+ error[invalid-argument-type] src/bokeh/embed/bundle.py:189:30: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `<class 'URL'>`
- error[no-matching-overload] src/bokeh/layouts.py:571:39: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] src/bokeh/layouts.py:571:43: Argument to function `__new__` is incorrect: Expected `(UIElement, /) -> Unknown`, found `(def traverse(children: list[LayoutDOM], level: int = Literal[0]) -> Unknown) | (def traverse(item: LayoutDOM, top_level: bool = Literal[False]) -> Unknown)`
- error[no-matching-overload] src/bokeh/palettes.py:1676:50: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] src/bokeh/palettes.py:1676:63: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `float64`

openlibrary (https://github.com/internetarchive/openlibrary)
- error[no-matching-overload] openlibrary/core/imports.py:146:20: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] openlibrary/core/imports.py:146:24: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'ImportItem'>`

sphinx (https://github.com/sphinx-doc/sphinx)
- error[no-matching-overload] sphinx/config.py:642:16: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sphinx/config.py:642:26: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Collection[type] & ~AlwaysFalsy & ~type & ~set[Unknown]) | (ENUM & ~AlwaysFalsy & ~type & ~set[Unknown])`
- error[no-matching-overload] sphinx/pycode/parser.py:123:47: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sphinx/pycode/parser.py:123:52: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `~int & ~str`

static-frame (https://github.com/static-frame/static-frame)
- error[no-matching-overload] static_frame/core/index_hierarchy.py:509:39: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] static_frame/core/index_hierarchy.py:509:45: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `(int & ~Literal[1]) | None`

streamlit (https://github.com/streamlit/streamlit)
- error[no-matching-overload] lib/streamlit/config.py:39:41: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] lib/streamlit/config.py:39:41: Argument to bound method `__init__` is incorrect: Expected `dict[str, _VT]`, found `OrderedDict[_KT, _VT]`
+ error[invalid-argument-type] lib/streamlit/config.py:40:5: Argument to bound method `__init__` is incorrect: Expected `_VT`, found `Literal["Special test section just used for unit tests."]`
- error[no-matching-overload] lib/streamlit/elements/json.py:123:20: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] lib/streamlit/elements/json.py:123:25: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `(~ChainMap[Unknown, Unknown] & ~MappingProxyType[Unknown, Unknown] & ~UserDict[Unknown, Unknown]) | (Unknown & ~ChainMap[Unknown, Unknown] & ~MappingProxyType[Unknown, Unknown] & ~UserDict[Unknown, Unknown]) | dict[Unknown, Unknown]`
- error[no-matching-overload] lib/streamlit/elements/widgets/slider.py:742:35: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] lib/streamlit/elements/widgets/slider.py:742:45: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Any & ~None) | Literal[0] | list[Unknown]`
- error[no-matching-overload] lib/streamlit/elements/widgets/slider.py:884:26: No overload of function `__new__` matches arguments
- error[no-matching-overload] lib/streamlit/elements/widgets/slider.py:889:26: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] lib/streamlit/elements/widgets/slider.py:884:49: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Any & ~None) | Literal[0] | list[Unknown] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] lib/streamlit/elements/widgets/slider.py:889:49: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Any & ~None) | Literal[0] | list[Unknown] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[no-matching-overload] lib/streamlit/elements/widgets/slider.py:910:26: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] lib/streamlit/elements/widgets/slider.py:910:51: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Any & ~None) | Literal[0] | list[Unknown] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- Found 3289 diagnostics
+ Found 3290 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[no-matching-overload] mesonbuild/cargo/interpreter.py:153:18: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] mesonbuild/cargo/interpreter.py:153:22: Argument to bound method `__init__` is incorrect: Argument type `Iterable[_T]` does not satisfy constraints of type variable `_UnknownKeysT`

cloud-init (https://github.com/canonical/cloud-init)
- error[no-matching-overload] cloudinit/net/network_state.py:827:32: No overload of bound method `get` matches arguments
+ error[invalid-argument-type] cloudinit/net/network_state.py:827:53: Argument to bound method `get` is incorrect: Expected `str`, found `Unknown | None`

prefect (https://github.com/PrefectHQ/prefect)
- error[no-matching-overload] src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:821:38: No overload of bound method `get` matches arguments
+ error[invalid-argument-type] src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:821:65: Argument to bound method `get` is incorrect: Expected `UUID`, found `UUID | None`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- error[no-matching-overload] sklearn/_loss/tests/test_loss.py:978:15: No overload of function `minimize_scalar` matches arguments
- error[no-matching-overload] sklearn/_loss/tests/test_loss.py:992:15: No overload of function `minimize` matches arguments
+ error[invalid-argument-type] sklearn/_loss/tests/test_loss.py:978:46: Argument to function `minimize_scalar` is incorrect: Expected `_MinimizeScalarOptionsBracketed | None`, found `dict[Unknown, Unknown]`
+ error[invalid-argument-type] sklearn/_loss/tests/test_loss.py:996:13: Argument to function `minimize` is incorrect: Expected `_MinimizeOptions | None`, found `dict[Unknown, Unknown]`
- error[no-matching-overload] sklearn/cluster/_kmeans.py:1539:33: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sklearn/cluster/_kmeans.py:1539:37: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `None | Unknown`
- error[no-matching-overload] sklearn/cluster/tests/test_k_means.py:874:16: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sklearn/cluster/tests/test_k_means.py:963:16: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sklearn/cluster/tests/test_k_means.py:874:20: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
+ error[invalid-argument-type] sklearn/cluster/tests/test_k_means.py:963:20: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
- error[no-matching-overload] sklearn/datasets/_samples_generator.py:1146:34: No overload of function `__new__` matches arguments
- error[no-matching-overload] sklearn/datasets/_samples_generator.py:1146:34: No overload of function `__new__` matches arguments
- error[no-matching-overload] sklearn/datasets/_samples_generator.py:1146:34: No overload of function `__new__` matches arguments
- error[no-matching-overload] sklearn/datasets/_samples_generator.py:1146:34: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sklearn/datasets/_samples_generator.py:1146:60: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Real) | (float & ~Real) | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/datasets/_samples_generator.py:1146:60: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Real) | (float & ~Real) | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/datasets/_samples_generator.py:1146:60: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Real) | (float & ~Real) | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] sklearn/datasets/_samples_generator.py:1146:60: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Real) | (float & ~Real) | @Todo(Support for `typing.TypeAlias`)`
- error[no-matching-overload] sklearn/externals/array_api_extra/_lib/_funcs.py:512:24: No overload of function `min` matches arguments
- error[no-matching-overload] sklearn/externals/array_api_extra/_lib/_funcs.py:512:45: No overload of function `max` matches arguments
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
- error[no-matching-overload] sklearn/feature_extraction/image.py:343:47: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sklearn/feature_extraction/image.py:343:52: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~Number) | Literal[8] | tuple[Unknown, ...]`
- error[no-matching-overload] sklearn/linear_model/_glm/tests/test_glm.py:42:14: No overload of function `minimize` matches arguments
+ error[invalid-argument-type] sklearn/linear_model/_glm/tests/test_glm.py:43:39: Argument to function `minimize` is incorrect: Expected `_MinimizeOptions | None`, found `dict[Unknown, Unknown]`
- error[no-matching-overload] sklearn/linear_model/tests/test_coordinate_descent.py:457:29: No overload of function `min` matches arguments
- error[no-matching-overload] sklearn/linear_model/tests/test_coordinate_descent.py:474:29: No overload of function `min` matches arguments
+ error[invalid-argument-type] sklearn/linear_model/tests/test_coordinate_descent.py:457:33: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | float`
+ error[invalid-argument-type] sklearn/linear_model/tests/test_coordinate_descent.py:474:33: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | float`
- error[no-matching-overload] sklearn/linear_model/tests/test_quantile.py:157:11: No overload of function `minimize` matches arguments
+ error[invalid-argument-type] sklearn/linear_model/tests/test_quantile.py:162:9: Argument to function `minimize` is incorrect: Expected `_MinimizeOptions | None`, found `dict[Unknown, Unknown]`
- error[no-matching-overload] sklearn/mixture/tests/test_gaussian_mixture.py:1089:28: No overload of function `__new__` matches arguments
- error[no-matching-overload] sklearn/mixture/tests/test_gaussian_mixture.py:1089:28: No overload of function `__new__` matches arguments
- error[no-matching-overload] sklearn/mixture/tests/test_gaussian_mixture.py:1089:28: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sklearn/mixture/tests/test_gaussian_mixture.py:1089:49: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
+ error[invalid-argument-type] sklearn/mixture/tests/test_gaussian_mixture.py:1089:49: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
+ error[invalid-argument-type] sklearn/mixture/tests/test_gaussian_mixture.py:1089:49: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
- error[no-matching-overload] sklearn/utils/_encode.py:323:16: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sklearn/utils/_encode.py:323:21: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Array`
- error[no-matching-overload] sklearn/utils/validation.py:2780:36: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sklearn/utils/validation.py:2780:40: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Any | None`

rotki (https://github.com/rotki/rotki)
- error[no-matching-overload] rotkehlchen/tests/db/test_db.py:447:16: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] rotkehlchen/tests/db/test_db.py:447:20: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `list[EvmToken] | None`
- error[no-matching-overload] rotkehlchen/tests/exchanges/test_binance.py:800:32: No overload of function `next` matches arguments
- error[no-matching-overload] rotkehlchen/tests/exchanges/test_binance.py:802:32: No overload of function `next` matches arguments
+ error[invalid-argument-type] rotkehlchen/tests/exchanges/test_binance.py:800:37: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `def get_fiat_deposit_result() -> Unknown`
+ error[invalid-argument-type] rotkehlchen/tests/exchanges/test_binance.py:802:37: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `def get_fiat_withdraw_result() -> Unknown`
- error[no-matching-overload] rotkehlchen/tests/exchanges/test_kucoin.py:94:59: No overload of function `next` matches arguments
+ error[invalid-argument-type] rotkehlchen/tests/exchanges/test_kucoin.py:94:64: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `def get_response() -> Unknown`

zulip (https://github.com/zulip/zulip)
- error[no-matching-overload] zerver/actions/message_edit.py:684:41: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] zerver/actions/message_edit.py:684:45: Argument to function `__new__` is incorrect: Expected `(Self, /) -> Unknown`, found `def user_info(um: UserMessage) -> dict[str, Any]`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[no-matching-overload] tests/appsec/iast/aspects/test_side_effects.py:137:9: No overload of function `__new__` matches arguments
- error[no-matching-overload] tests/appsec/iast/aspects/test_side_effects.py:145:9: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] tests/appsec/iast/aspects/test_side_effects.py:137:15: Argument to function `__new__` is incorrect: Expected `str`, found `MagicMethodsException`
+ error[invalid-argument-type] tests/appsec/iast/aspects/test_side_effects.py:145:15: Argument to function `__new__` is incorrect: Expected `str`, found `None`
- error[no-matching-overload] tests/tracer/test_encoders.py:817:12: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] tests/tracer/test_encoders.py:905:15: No overload of function `getattr` matches arguments
+ error[invalid-argument-type] tests/tracer/test_encoders.py:817:17: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ListStringTable`
+ error[invalid-argument-type] tests/tracer/test_encoders.py:905:33: Argument to function `getattr` is incorrect: Expected `str`, found `str | None`

scipy (https://github.com/scipy/scipy)
- error[no-matching-overload] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:24: No overload of function `min` matches arguments
- error[no-matching-overload] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:45: No overload of function `max` matches arguments
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
- error[no-matching-overload] scipy/integrate/_ivp/common.py:248:31: No overload of function `__new__` matches arguments
- error[no-matching-overload] scipy/integrate/_ivp/common.py:248:31: No overload of function `__new__` matches arguments
- error[no-matching-overload] scipy/integrate/_ivp/common.py:248:31: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] scipy/integrate/_ivp/common.py:248:39: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)] | Unknown`
+ error[invalid-argument-type] scipy/integrate/_ivp/common.py:248:39: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)] | Unknown`
+ error[invalid-argument-type] scipy/integrate/_ivp/common.py:248:39: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)] | Unknown`
- error[no-matching-overload] scipy/integrate/_quadpack_py.py:577:20: No overload of function `min` matches arguments
+ error[invalid-argument-type] scipy/integrate/_quadpack_py.py:577:24: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
- error[no-matching-overload] scipy/ndimage/_measurements.py:563:24: No overload of function `__new__` matches arguments
- error[no-matching-overload] scipy/ndimage/_measurements.py:563:24: No overload of function `__new__` matches arguments
- error[no-matching-overload] scipy/ndimage/_measurements.py:563:24: No overload of function `__new__` matches arguments
- error[no-matching-overload] scipy/ndimage/_measurements.py:563:24: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:41: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:41: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:41: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:41: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:45: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:45: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:45: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-argument-type] scipy/ndimage/_measurements.py:563:45: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
- error[no-matching-overload] scipy/ndimage/tests/test_measurements.py:410:13: No overload of bound method `rand` matches arguments
+ error[invalid-argument-type] scipy/ndimage/tests/test_measurements.py:410:28: Argument to bound method `rand` is incorrect: Expected `int`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)]`
- error[no-matching-overload] scipy/sparse/_dok.py:169:29: No overload of function `__new__` matches arguments
- error[no-matching-overload] scipy/sparse/_dok.py:169:29: No overload of function `__new__` matches arguments
- error[no-matching-overload] scipy/sparse/_dok.py:169:29: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] scipy/sparse/_dok.py:169:33: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)] | zip[Unknown]`
+ error[invalid-argument-type] scipy/sparse/_dok.py:169:33: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)] | zip[Unknown]`
+ error[invalid-argument-type] scipy/sparse/_dok.py:169:33: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `signedinteger[@Todo(Support for `typing.TypeAlias`)] | zip[Unknown]`
- error[no-matching-overload] scipy/special/_orthogonal.py:135:38: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] scipy/special/_orthogonal.py:135:49: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
- error[no-matching-overload] scipy/special/tests/test_nan_inputs.py:29:20: No overload matches arguments
+ error[invalid-argument-type] scipy/special/tests/test_nan_inputs.py:29:49: Argument to bound method `__call__` is incorrect: Expected `str`, found `(...) -> Unknown`
- error[no-matching-overload] scipy/stats/tests/test_distributions.py:4236:16: No overload of function `fromarrays` matches arguments
+ error[invalid-argument-type] scipy/stats/tests/test_distributions.py:4236:40: Argument to function `fromarrays` is incorrect: Expected `None`, found `Literal["x,pdf,a,b"]`
- error[no-matching-overload] scipy/stats/tests/test_distributions.py:5554:16: No overload of function `fromarrays` matches arguments
- error[no-matching-overload] scipy/stats/tests/test_distributions.py:5598:16: No overload of function `fromarrays` matches arguments
+ error[invalid-argument-type] scipy/stats/tests/test_distributions.py:5554:42: Argument to function `fromarrays` is incorrect: Expected `None`, found `Literal["x,p,alpha,beta,pct"]`
+ error[invalid-argument-type] scipy/stats/tests/test_distributions.py:5598:42: Argument to function `fromarrays` is incorrect: Expected `None`, found `Literal["x,p,alpha,beta,pct"]`
- error[no-matching-overload] scipy/stats/tests/test_distributions.py:10117:16: No overload of function `fromarrays` matches arguments
+ error[invalid-argument-type] scipy/stats/tests/test_distributions.py:10117:42: Argument to function `fromarrays` is incorrect: Expected `None`, found `Literal["x,pdf,rho,gamma"]`
- error[no-matching-overload] tools/generate_f2pymod.py:193:14: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] tools/generate_f2pymod.py:193:20: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `None | int`
- Found 7644 diagnostics
+ Found 7648 diagnostics

sympy (https://github.com/sympy/sympy)
- error[no-matching-overload] sympy/calculus/tests/test_euler.py:71:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/calculus/tests/test_euler.py:71:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/calculus/tests/test_euler.py:71:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/calculus/tests/test_euler.py:71:15: No overload of function `symbols` matches arguments
+ error[invalid-argument-type] sympy/calculus/tests/test_euler.py:71:46: Argument to function `symbols` is incorrect: Expected `int`, found `tuple[Any, Any]`
+ error[invalid-argument-type] sympy/calculus/tests/test_euler.py:71:46: Argument to function `symbols` is incorrect: Expected `int`, found `tuple[Any, Any]`
+ error[invalid-argument-type] sympy/calculus/tests/test_euler.py:71:46: Argument to function `symbols` is incorrect: Expected `int`, found `tuple[Any, Any]`
+ error[invalid-argument-type] sympy/calculus/tests/test_euler.py:71:46: Argument to function `symbols` is incorrect: Expected `int`, found `tuple[Any, Any]`
- error[no-matching-overload] sympy/combinatorics/prufer.py:279:23: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sympy/combinatorics/prufer.py:279:29: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `None | Unknown`
- error[no-matching-overload] sympy/core/function.py:325:39: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sympy/core/function.py:325:43: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `object`
- error[no-matching-overload] sympy/core/mul.py:1015:24: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/core/mul.py:1015:24: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/core/mul.py:1015:24: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sympy/core/mul.py:1015:28: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'factorial'>`
+ error[invalid-argument-type] sympy/core/mul.py:1015:28: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'factorial'>`
+ error[invalid-argument-type] sympy/core/mul.py:1015:28: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'factorial'>`
- error[no-matching-overload] sympy/core/power.py:1505:21: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/power.py:1505:21: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/power.py:1505:21: No overload of function `symbols` matches arguments
+ error[invalid-argument-type] sympy/core/power.py:1505:48: Argument to function `symbols` is incorrect: Expected `int`, found `list[Unknown]`
+ error[invalid-argument-type] sympy/core/power.py:1505:48: Argument to function `symbols` is incorrect: Expected `int`, found `list[Unknown]`
+ error[invalid-argument-type] sympy/core/power.py:1505:48: Argument to function `symbols` is incorrect: Expected `int`, found `list[Unknown]`
- error[no-matching-overload] sympy/core/tests/test_args.py:3162:30: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_args.py:3165:30: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_args.py:3171:30: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_args.py:3178:30: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_args.py:3181:30: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_args.py:3184:30: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_args.py:3190:30: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sympy/core/tests/test_args.py:3162:35: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ImmutableDenseMatrix`
+ error[invalid-argument-type] sympy/core/tests/test_args.py:3165:35: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ImmutableDenseMatrix`
+ error[invalid-argument-type] sympy/core/tests/test_args.py:3171:35: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ImmutableDenseMatrix`
+ error[invalid-argument-type] sympy/core/tests/test_args.py:3178:35: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ImmutableSparseMatrix`
+ error[invalid-argument-type] sympy/core/tests/test_args.py:3181:35: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ImmutableSparseMatrix`
+ error[invalid-argument-type] sympy/core/tests/test_args.py:3184:35: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ImmutableSparseMatrix`
+ error[invalid-argument-type] sympy/core/tests/test_args.py:3190:35: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `ImmutableSparseMatrix`
- error[no-matching-overload] sympy/core/tests/test_basic.py:259:12: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_basic.py:259:12: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_basic.py:259:12: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sympy/core/tests/test_basic.py:259:16: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Function'>`
+ error[invalid-argument-type] sympy/core/tests/test_basic.py:259:16: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Function'>`
+ error[invalid-argument-type] sympy/core/tests/test_basic.py:259:16: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Function'>`
- error[no-matching-overload] sympy/core/tests/test_containers.py:26:12: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_containers.py:28:12: No overload of bound method `__init__` matches arguments
+ error[invalid-argument-type] sympy/core/tests/test_containers.py:26:16: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Basic`
+ error[invalid-argument-type] sympy/core/tests/test_containers.py:28:16: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Basic`
- error[no-matching-overload] sympy/core/tests/test_eval.py:92:12: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_eval.py:92:12: No overload of function `__new__` matches arguments
- error[no-matching-overload] sympy/core/tests/test_eval.py:92:12: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] sympy/core/tests/test_eval.py:92:16: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Function'>`
+ error[invalid-argument-type] sympy/core/tests/test_eval.py:92:16: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Function'>`
+ error[invalid-argument-type] sympy/core/tests/test_eval.py:92:16: Argument to function `__new__` is incorrect: Expected `(Unknown, /) -> Unknown`, found `<class 'Function'>`
- error[no-matching-overload] sympy/core/tests/test_match.py:520:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/tests/test_match.py:520:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/tests/test_match.py:520:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/tests/test_match.py:520:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/tests/test_match.py:535:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/tests/test_match.py:535:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/tests/test_match.py:535:15: No overload of function `symbols` matches arguments
- error[no-matching-overload] sympy/core/tests/test_match.py:53...*[Comment body truncated]*

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:412 on 2025-06-18 06:15_

This test case was missing in the original implementation so just wanted to make sure that we still follow the algorithm correctly with the logic in this PR.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/diagnostics/union_call.md`:106 on 2025-06-18 06:16_

Refer to my other comment here: https://github.com/astral-sh/ruff/pull/18452/files#r2125498558

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1107 on 2025-06-18 06:22_

For step 2, the matching overload would have no error.

For step 3, there would be either multiple matching overloads or no matching overloads. For the former, the return type is populated via `overload_call_return_type` field.

For step 5, the matching overload is determined by step 6 if it's unambiguous and is the first overload remaining after the filtering process. This is done by the `return_type` method.

Technically, we could populate this field even for other steps and then it could be used in other methods but I don't think it's worth it right now. 

For additional context related to this, the `matching_overloads` / `matching_overloads_mut` method differs slightly in that it would not return the matching overload because there was a type checking error but this field would populate the matching overload even in that case. This field could possibly be used in the `return_type` method instead of relying on `overload_call_return_type` and `matching_overloads` but we'd still need to differentiate between union-ing the return types for step 3 and picking the first return type for step 6.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1626 on 2025-06-18 06:23_

I added this as the `FunctionType` is present right there.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2695 on 2025-06-18 06:24_

This is a bit unfortunate.

We require `FunctionType` because that will provide us the exact `OverloadLiteral` to which we can ask for the `parameter_span` for the `invalid-argument-type` error.

---

_@dhruvmanila reviewed on 2025-06-18 06:24_

---

_Marked ready for review by @dhruvmanila on 2025-06-18 06:24_

---

_Review requested from @carljm by @dhruvmanila on 2025-06-18 06:24_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-06-18 06:24_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-06-18 06:24_

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-18 06:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/diagnostics/union_call.md`:106 on 2025-06-18 22:50_

I guess the intent of this test is to show examples of all possible call diagnostics, so a change more in the spirit of the test might be to add an overloaded callable where we do fail to match any overload? (And it could probably be simpler now -- I think this test was written before we actually supported `@overload`.)

---

_@carljm approved on 2025-06-18 22:55_

This looks great, thank you! Big improvement in the usefulness of these diagnostics.

---

_@dhruvmanila reviewed on 2025-06-19 05:14_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/diagnostics/union_call.md`:106 on 2025-06-19 05:14_

Thanks, that makes sense, I've expanded the test case with two cases of overloads: `no-matching-overload` and `invalid-argument-type` (for a single matching overload).

---

_Comment by @dhruvmanila on 2025-06-19 05:29_

I ended up adding dedicated snapshot diagnostic tests under `./diagnostics/single_matching_overload.md` similar to `./diagnostics/no_matching_overload.md` with two cases - simple and excessive number of overloads.

---

_Review requested from @BurntSushi by @dhruvmanila on 2025-06-19 05:31_

---

_Comment by @dhruvmanila on 2025-06-19 05:32_

Sorry, I totally missed requesting @BurntSushi review. I'll probably go ahead and merge this but happy to address any feedback in a follow-up PR.

---

_Closed by @MichaReiser on 2025-06-19 09:43_

---

_Reopened by @MichaReiser on 2025-06-19 09:43_

---

_Merged by @dhruvmanila on 2025-06-20 03:06_

---

_Closed by @dhruvmanila on 2025-06-20 03:06_

---

_Branch deleted on 2025-06-20 03:06_

---

_@BurntSushi reviewed on 2025-06-20 11:41_

LGTM! Nice!

---
