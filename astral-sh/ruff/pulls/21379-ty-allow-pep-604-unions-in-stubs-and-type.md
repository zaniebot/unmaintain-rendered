```yaml
number: 21379
title: "[ty] Allow PEP-604 unions in stubs and `TYPE_CHECKING` blocks prior to 3.10"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/604-stubs
created_at: 2025-11-11T13:15:47Z
updated_at: 2025-11-11T14:33:44Z
url: https://github.com/astral-sh/ruff/pull/21379
synced_at: 2026-01-12T15:57:22Z
```

# [ty] Allow PEP-604 unions in stubs and `TYPE_CHECKING` blocks prior to 3.10

---

_@AlexWaygood_

## Summary

We currently only support implicit PEP-604 type aliases if `python-version` is set to Python 3.10+. But we should also support them on Python <=3.9 if it's a stub file, or some other suite that we know will not be executed at runtime such as an `if TYPE_CHECKING` block.

## Test Plan

Mdtests added.


---

_Label `ty` added by @AlexWaygood on 2025-11-11 13:15_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 13:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 13:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- kornia/core/mixin/image_module.py:115:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:307:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/module.py:121:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/utils/draw.py:377:29: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
- Found 765 diagnostics
+ Found 763 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/distlib/compat.py:326:32: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
+ src/pip/_vendor/pyproject_hooks/_in_process/__init__.py:14:31: error[invalid-argument-type] Argument to function `path` is incorrect: Expected `str | ModuleType`, found `str | None`
+ src/pip/_vendor/pyproject_hooks/_in_process/__init__.py:20:29: error[invalid-argument-type] Argument to function `files` is incorrect: Expected `str | ModuleType`, found `str | None`
+ src/pip/_vendor/requests/models.py:213:29: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
+ src/pip/_vendor/requests/models.py:216:71: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
- Found 573 diagnostics
+ Found 578 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/cmd/commands.py:571:24: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `object`
+ lib/spack/spack/cmd/commands.py:563:34: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/spec.py:1419:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/typing_extensions.py:111:49: error[unsupported-operator] Operator `-` is unsupported between objects of type `~AlwaysFalsy | int` and `int`
- Found 7916 diagnostics
+ Found 7914 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/pyutils/is_iterable.py:30:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `list[Unknown | <class 'Collection'>] | Unknown | <class 'Collection'> | tuple[Unknown | <class 'Collection'>, ...]`
- Found 390 diagnostics
+ Found 391 diagnostics

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/metrics/test_precision_recall_curve.py:177:32: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Tuple`
- tests/ignite/metrics/test_running_average.py:286:17: error[unresolved-attribute] Object of type `int` has no attribute `item`

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/json/__init__.py:15:16: error[invalid-argument-type] Argument to function `files` is incorrect: Expected `str | ModuleType`, found `str | None`
- Found 974 diagnostics
+ Found 975 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_utils.py:98:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/fields.py:666:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/fields.py:743:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/v1/fields.py:684:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
+ pydantic/v1/fields.py:694:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Set`
+ pydantic/v1/fields.py:704:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.FrozenSet`
+ pydantic/v1/fields.py:714:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Deque`
+ pydantic/v1/fields.py:725:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.DefaultDict`
+ pydantic/v1/fields.py:729:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Counter`
- pydantic/v1/schema.py:1081:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/v1/schema.py:1059:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
+ pydantic/v1/schema.py:1072:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Set`
+ pydantic/v1/schema.py:1076:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.FrozenSet`
+ pydantic/v1/schema.py:1084:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Dict`
+ pydantic/v1/types.py:650:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
- pydantic/v1/utils.py:176:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/utils.py:183:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 776 diagnostics
+ Found 781 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ src/urllib3/contrib/emscripten/fetch.py:205:19: error[invalid-argument-type] Argument to function `files` is incorrect: Expected `str | ModuleType`, found `str | None`
- Found 354 diagnostics
+ Found 355 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/dataframe/components.py:198:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/api/pyspark/column_schema.py:156:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/backends/pandas/builtin_checks.py:171:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_dtypes.py:190:9: error[unresolved-attribute] Object of type `Timestamp` has no attribute `to_series`
+ tests/pandas/test_dtypes.py:190:9: error[call-non-callable] Object of type `Timestamp` is not callable
- tests/pandas/test_dtypes.py:194:9: error[unresolved-attribute] Object of type `Period` has no attribute `to_series`
+ tests/pandas/test_dtypes.py:194:9: error[call-non-callable] Object of type `Series[Any]` is not callable
- Found 1638 diagnostics
+ Found 1635 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/config_parser.py:707:56: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ mypy/config_parser.py:713:57: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- mypy/typeshed/stdlib/_asyncio.pyi:47:38: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Generator[Unknown, None, typing.TypeVar]'>` and `<class 'Coroutine[Any, Any, typing.TypeVar]'>`
- mypy/typeshed/stdlib/_codecs.pyi:15:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'dict[int, int]'>` and `<class '_EncodingMap'>`
- mypy/typeshed/stdlib/_codecs.pyi:17:46: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'CodecInfo'>` and `None`
- mypy/typeshed/stdlib/_ctypes.pyi:158:18: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[int]'>` and `<class 'tuple[int, str | None]'>`
- mypy/typeshed/stdlib/_curses.pyi:11:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/_operator.pyi:35:34: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class '_SupportsDunderLE'>` and `<class '_SupportsDunderGE'>`
- mypy/typeshed/stdlib/_pickle.pyi:15:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'tuple[(...) -> Any, tuple[Any, ...]]'>`
- mypy/typeshed/stdlib/_socket.pyi:14:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[Any, ...]'>` and `<class 'str'>`
- mypy/typeshed/stdlib/_ssl.pyi:18:41: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:106:37: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'SupportsDunderLT[Any]'>` and `<class 'SupportsDunderGT[Any]'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:182:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'PathLike[str]'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:183:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'bytes'>` and `<class 'PathLike[bytes]'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:185:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:223:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:252:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:259:33: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'HasFileno'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:318:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[type[BaseException], BaseException, TracebackType]'>` and `<class 'tuple[None, None, None]'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:372:35: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'Buffer'>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:373:33: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'Buffer'>`
- mypy/typeshed/stdlib/_typeshed/dbapi.pyi:8:28: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Any` and `None`
- mypy/typeshed/stdlib/aifc.pyi:17:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'IO[bytes]'>`
- mypy/typeshed/stdlib/array.pyi:14:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/ast.pyi:1096:51: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/ast.pyi:1099:51: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/asyncio/__init__.pyi:1004:33: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Generator[Any, None, typing.TypeVar]'>` and `<class 'Awaitable[typing.TypeVar]'>`
- mypy/typeshed/stdlib/asyncio/__init__.pyi:1005:33: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Generator[Any, None, typing.TypeVar]'>` and `<class 'Coroutine[Any, Any, typing.TypeVar]'>`
- mypy/typeshed/stdlib/asyncio/base_events.pyi:26:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- mypy/typeshed/stdlib/asyncio/base_subprocess.pyi:9:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'IO[Any]'>`
- mypy/typeshed/stdlib/asyncio/events.pyi:69:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- mypy/typeshed/stdlib/asyncio/format_helpers.pyi:13:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'FunctionType'>` and `<class '_HasWrapper'>`
- mypy/typeshed/stdlib/asyncio/streams.pyi:26:78: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Awaitable[None]'>` and `None`
- mypy/typeshed/stdlib/asyncio/tasks.pyi:81:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Future[typing.TypeVar]'>` and `<class 'Generator[Any, None, typing.TypeVar]'>`
- mypy/typeshed/stdlib/asyncio/tasks.pyi:83:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Future[object]'>` and `None`
- mypy/typeshed/stdlib/asyncio/tasks.pyi:412:38: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Generator[Unknown, None, typing.TypeVar]'>` and `<class 'Coroutine[Any, Any, typing.TypeVar]'>`
- mypy/typeshed/stdlib/asyncio/trsock.pyi:13:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'bytearray'>` and `<class 'memoryview'>`
- mypy/typeshed/stdlib/binhex.pyi:19:31: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'IO[bytes]'>`
- mypy/typeshed/stdlib/builtins.pyi:249:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/builtins.pyi:1732:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class '_SupportsPow2[Any, Any]'>` and `<class '_SupportsPow3NoneOnly[Any, Any]'>`
- mypy/typeshed/stdlib/cmath.pyi:12:17: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'SupportsFloat'>` and `<class 'SupportsComplex'>`
- mypy/typeshed/stdlib/contextlib.pyi:41:62: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'BaseException'>` and `None`
- mypy/typeshed/stdlib/contextlib.pyi:41:84: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'TracebackType'>` and `None`
- mypy/typeshed/stdlib/contextlib.pyi:41:107: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- mypy/typeshed/stdlib/contextlib.pyi:178:34: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'BaseException'>` and `None`
- mypy/typeshed/stdlib/contextlib.pyi:178:56: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'TracebackType'>` and `None`
- mypy/typeshed/stdlib/copyreg.pyi:6:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[(...) -> typing.TypeVar, tuple[Any, ...]]'>` and `<class 'tuple[(...) -> typing.TypeVar, tuple[Any, ...], Any | None]'>`
- mypy/typeshed/stdlib/ctypes/__init__.pyi:77:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `None`
- mypy/typeshed/stdlib/ctypes/__init__.pyi:129:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class '_CDLLFuncPointer'>` and `<class '_CFunctionType'>`
- mypy/typeshed/stdlib/ctypes/__init__.pyi:150:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class '_PointerLike'>` and `<class 'Array[Any]'>`
- mypy/typeshed/stdlib/dbm/__init__.pyi:10:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/dbm/__init__.pyi:11:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/dbm/dumb.pyi:9:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/dbm/dumb.pyi:10:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/dis.pyi:44:28: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'MethodType'>` and `<class 'FunctionType'>`
- mypy/typeshed/stdlib/distutils/ccompiler.pyi:7:21: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[str]'>` and `<class 'tuple[str, str | None]'>`
- mypy/typeshed/stdlib/email/__init__.pyi:32:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'tuple[str | None, str | None, str]'>`
- mypy/typeshed/stdlib/email/__init__.pyi:33:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `None`
- mypy/typeshed/stdlib/email/message.pyi:25:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Charset'>` and `<class 'str'>`
- mypy/typeshed/stdlib/enum.pyi:55:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'Iterable[str]'>`
- mypy/typeshed/stdlib/fractions.pyi:8:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'float'>`
- mypy/typeshed/stdlib/http/client.pyi:35:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'SupportsRead[bytes]'>` and `<class 'Iterable[@Todo]'>`
- mypy/typeshed/stdlib/http/cookies.pyi:8:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'Mapping[str, str | @Todo]'>`
- mypy/typeshed/stdlib/imaplib.pyi:21:31: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'list[None]'>` and `<class 'list[bytes | tuple[bytes, bytes]]'>`
- mypy/typeshed/stdlib/importlib/resources/__init__.pyi:18:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'ModuleType'>`
- mypy/typeshed/stdlib/importlib/resources/__init__.pyi:40:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'PathLike[Any]'>`
- mypy/typeshed/stdlib/inspect.pyi:469:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'list[tuple[type, ...]]'>` and `<class 'list[Unknown]'>`
- mypy/typeshed/stdlib/ipaddress.pyi:13:28: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'str'>`
- mypy/typeshed/stdlib/itertools.pyi:24:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'SupportsFloat'>` and `<class 'SupportsInt'>`
- mypy/typeshed/stdlib/logging/__init__.pyi:63:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[type[BaseException], BaseException, TracebackType | None]'>` and `<class 'tuple[None, None, None]'>`
- mypy/typeshed/stdlib/logging/__init__.pyi:64:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `None` and `<class 'bool'>`
- mypy/typeshed/stdlib/logging/__init__.pyi:65:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[object, ...]'>` and `<class 'Mapping[str, object]'>`
- mypy/typeshed/stdlib/logging/__init__.pyi:66:21: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'str'>`
- mypy/typeshed/stdlib/logging/config.pyi:46:35: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class '_FilterConfigurationTypedDict'>` and `<class 'dict[str, Any]'>`
- mypy/typeshed/stdlib/mailbox.pyi:37:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Message'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/math.pyi:10:36: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'SupportsFloat'>` and `<class 'SupportsIndex'>`
- mypy/typeshed/stdlib/math.pyi:98:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/multiprocessing/connection.pyi:12:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'tuple[str, int]'>`
- mypy/typeshed/stdlib/multiprocessing/context.pyi:22:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Lock'>` and `<class 'RLock'>`
- mypy/typeshed/stdlib/nntplib.pyi:22:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'IO[bytes]'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/os/__init__.pyi:1345:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[@Todo, ...]'>` and `<class 'list[bytes]'>`
- mypy/typeshed/stdlib/os/__init__.pyi:1358:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Mapping[bytes, bytes | str]'>` and `<class 'Mapping[str, bytes | str]'>`
- mypy/typeshed/stdlib/pstats.pyi:17:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'float'>`
- mypy/typeshed/stdlib/re.pyi:243:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'RegexFlag'>`
- mypy/typeshed/stdlib/readline.pyi:8:50: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `None`
- mypy/typeshed/stdlib/signal.pyi:67:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'Signals'>`
- mypy/typeshed/stdlib/signal.pyi:68:38: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'FrameType'>` and `None`
- mypy/typeshed/stdlib/socketserver.pyi:35:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'socket'>` and `<class 'tuple[bytes, socket]'>`
- mypy/typeshed/stdlib/sqlite3/__init__.pyi:222:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'SupportsLenAndGetItem[@Todo]'>` and `<class 'Mapping[str, @Todo]'>`
- mypy/typeshed/stdlib/sqlite3/__init__.pyi:224:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `None`
- mypy/typeshed/stdlib/sre_parse.pyi:32:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'list[tuple[_NamedIntConstant, int]]'>` and `<class 'tuple[None, list[Unknown]]'>`
- mypy/typeshed/stdlib/ssl.pyi:48:31: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'dict[str, str | tuple[tuple[tuple[str, str], ...], ...] | tuple[tuple[str, str], ...]]'>` and `<class 'bytes'>`
- mypy/typeshed/stdlib/ssl.pyi:49:61: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `None`
- mypy/typeshed/stdlib/ssl.pyi:49:85: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `None`
- mypy/typeshed/stdlib/statistics.pyi:35:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'float'>` and `<class 'Decimal'>`
- mypy/typeshed/stdlib/statistics.pyi:42:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'float'>`
- mypy/typeshed/stdlib/subprocess.pyi:62:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `None` and `<class 'int'>`
- mypy/typeshed/stdlib/subprocess.pyi:68:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Mapping[bytes, @Todo]'>` and `<class 'Mapping[str, @Todo]'>`
- mypy/typeshed/stdlib/sunau.pyi:5:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'IO[bytes]'>`
- mypy/typeshed/stdlib/sys/__init__.pyi:14:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'int'>`
- mypy/typeshed/stdlib/sys/__init__.pyi:306:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `None`
- mypy/typeshed/stdlib/termios.pyi:7:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'list[int | list[bytes | int]]'>` and `<class 'list[int | list[bytes]]'>`
- mypy/typeshed/stdlib/tkinter/__init__.pyi:177:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'tuple[str]'>`
- mypy/typeshed/stdlib/tkinter/ttk.pyi:42:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'float'>` and `<class 'str'>`
- mypy/typeshed/stdlib/tkinter/ttk.pyi:77:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[@Todo, ...]'>` and `<class 'tuple[Literal["from"], str, str]'>`
- mypy/typeshed/stdlib/tokenize.pyi:130:21: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'TokenInfo'>` and `<class 'Sequence[int | str | tuple[int, int]]'>`
- mypy/typeshed/stdlib/tracemalloc.pyi:75:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[int, int, Sequence[tuple[str, int]], int | None]'>` and `<class 'tuple[int, int, Sequence[tuple[str, int]]]'>`
- mypy/typeshed/stdlib/tty.pyi:15:22: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'IO[str]'>`
- mypy/typeshed/stdlib/turtle.pyi:146:21: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'tuple[int | float, int | float, int | float]'>`
- mypy/typeshed/stdlib/turtle.pyi:163:21: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'float'>`
- mypy/typeshed/stdlib/typing.pyi:924:43: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'bytes'>` and `<class 'bytearray'>`
- mypy/typeshed/stdlib/unittest/mock.pyi:69:25: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'tuple[Any, ...]'>`
- mypy/typeshed/stdlib/urllib/parse.pyi:143:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'Mapping[str, object]'>` and `<class 'Mapping[bytes, object]'>`
- mypy/typeshed/stdlib/uu.pyi:6:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'BinaryIO'>`
- mypy/typeshed/stdlib/wave.pyi:8:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'IO[bytes]'>`
- mypy/typeshed/stdlib/xml/dom/pulldom.pyi:21:31: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'DOMImplementation'>` and `None`
- mypy/typeshed/stdlib/xml/dom/pulldom.pyi:24:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[Literal["START_ELEMENT"], Element]'>` and `<class 'tuple[Literal["END_ELEMENT"], Element]'>`
- mypy/typeshed/stdlib/xml/etree/ElementTree.pyi:262:26: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[str]'>` and `<class 'tuple[str, tuple[str, str]]'>`
- mypy/typeshed/stdlib/xml/sax/expatreader.pyi:12:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<class 'bool'>`
- mypy/typeshed/stdlib/xmlrpc/client.pyi:18:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'bool'>` and `<class 'int'>`
- mypy/typeshed/stdlib/xmlrpc/client.pyi:33:23: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'datetime'>`
- mypy/typeshed/stdlib/xmlrpc/client.pyi:34:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'tuple[str, dict[str, str]]'>` and `<class 'str'>`
- mypy/typeshed/stdlib/xmlrpc/server.pyi:39:5: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class '_DispatchArity0'>` and `<class '_DispatchArity1'>`
- mypy/typeshed/stdlib/zipapp.pyi:8:20: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'Path'>`
- mypyc/irbuild/for_helpers.py:1215:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1833 diagnostics
+ Found 1712 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/hybrid.py:219:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 503 diagnostics
+ Found 502 diagnostics

apprise (https://github.com/caronc/apprise)
+ tests/test_plugin_email.py:515:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
+ tests/test_plugin_email.py:612:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `Unknown | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
+ tests/test_plugin_email.py:624:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
+ tests/test_plugin_growl.py:323:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'NotifyGrowl'> | bool`
+ tests/test_plugin_growl.py:367:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `Unknown | None | <class 'NotifyGrowl'> | bool`
+ tests/test_plugin_growl.py:376:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'NotifyGrowl'> | bool`
- Found 2469 diagnostics
+ Found 2475 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/pool.py:398:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pymongo/synchronous/pool.py:398:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 538 diagnostics
+ Found 536 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/jaraco/collections/__init__.py:40:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- setuptools/_vendor/typeguard/_checkers.py:938:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- setuptools/_vendor/typing_extensions.py:2889:53: error[unsupported-operator] Operator `-` is unsupported between objects of type `(Unknown & ~AlwaysFalsy) | (_Sentinel & ~AlwaysFalsy) | int` and `int`
- setuptools/_vendor/typing_extensions.py:2906:21: error[unsupported-operator] Operator `-=` is unsupported between objects of type `_Sentinel & ~AlwaysFalsy` and `int`
- Found 1181 diagnostics
+ Found 1177 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/contrib/internal/aioredis/patch.py:130:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `Unknown | None`
+ ddtrace/contrib/internal/kafka/patch.py:189:92: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `Unknown | None`
+ ddtrace/internal/telemetry/writer.py:151:60: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `Unknown | EnvVariable[float]` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- ddtrace/internal/telemetry/writer.py:151:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | EnvVariable[float] | Literal[10]`
+ ddtrace/internal/telemetry/writer.py:151:60: error[invalid-argument-type] Argument to function `min` is incorrect: Expected `Literal[10]`, found `Unknown | EnvVariable[float]`
+ ddtrace/llmobs/_evaluators/ragas/faithfulness.py:168:43: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
+ tests/contrib/subprocess/test_subprocess.py:709:41: error[invalid-argument-type] Argument to function `spawnv` is incorrect: Expected `tuple[@Todo, ...] | list[bytes] | list[str] | ... omitted 5 union elements`, found `Literal["invalid"]`
+ tests/profiling/collector/test_stack.py:945:60: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ tests/profiling/collector/test_stack.py:950:60: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ tests/profiling/collector/test_stack.py:965:60: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 8206 diagnostics
+ Found 8214 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/bigquery/tests/system/test_client.py:107:9: error[unresolved-attribute] Object of type `Timestamp` has no attribute `dt`
+ ibis/backends/impala/tests/test_udf.py:388:28: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Literal`
+ ibis/common/patterns.py:807:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `T_co@CoercedTo`
- ibis/formats/pandas.py:224:28: error[unresolved-attribute] Object of type `Timestamp` has no attribute `dt`
- ibis/formats/pandas.py:226:28: error[unresolved-attribute] Object of type `Timestamp` has no attribute `dt`
+ ibis/tests/expr/test_table.py:1360:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Union`


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-11 13:22_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 13:28_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 0 | 126 | 0 |
| `invalid-argument-type` | 42 | 1 | 0 |
| `unused-ignore-comment` | 0 | 19 | 0 |
| `unresolved-attribute` | 0 | 6 | 0 |
| `call-non-callable` | 2 | 0 | 0 |
| **Total** | **44** | **152** | **0** |

**[Full report with detailed diff](https://alex-604-stubs.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-604-stubs.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-11-11 14:11_

The ecosystem impact looks like basically what I'd expect.

---

_Marked ready for review by @AlexWaygood on 2025-11-11 14:11_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-11 14:11_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-11 14:11_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-11 14:11_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:279 on 2025-11-11 14:21_

```suggestion
are permitted even if the target version is set to Python 3.9 or earlier.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:302 on 2025-11-11 14:23_

:eyes:

Are you trying to test if we are actually reviewing your code? :smile: 

---

_@sharkdp approved on 2025-11-11 14:23_

Nice, thank you!

---

_@AlexWaygood reviewed on 2025-11-11 14:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:302 on 2025-11-11 14:28_

A man gets bored of naming everything `Foo` and `Bar`

---

_Merged by @AlexWaygood on 2025-11-11 14:33_

---

_Closed by @AlexWaygood on 2025-11-11 14:33_

---

_Branch deleted on 2025-11-11 14:33_

---
