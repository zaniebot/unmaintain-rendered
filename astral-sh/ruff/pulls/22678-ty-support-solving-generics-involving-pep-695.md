```yaml
number: 22678
title: "[ty] Support solving generics involving PEP 695 type aliases"
type: pull_request
state: open
author: bxff
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: fix/pep695-type-alias-solver
created_at: 2026-01-18T13:12:15Z
updated_at: 2026-01-21T08:30:58Z
url: https://github.com/astral-sh/ruff/pull/22678
synced_at: 2026-01-21T09:03:02Z
```

# [ty] Support solving generics involving PEP 695 type aliases

---

_@bxff_

## Summary

Fixes type variable inference when a PEP 695 type alias is used as a function parameter.

**Before:**
```python
type MyList[T] = list[T]
def head[T](my_list: MyList[T]) -> T: ...
reveal_type(head([1, 2]))  # Unknown
```

**After:** Correctly infers `int` by expanding `MyList[T]` to `list[T]` during solving.

Changes in `generics.rs`:
- **Early formal expansion**: Expand parameter type aliases immediately so underlying structure is visible for structural matching
- **Late actual expansion**: Expand argument type aliases after direct matching attempts to preserve alias identity for `reveal_type()` and literal promotion
- **Union induction**: Recursively match formal type against each union element to support nested aliases like `ListOfPairs[T] = list[Pair[T]]`

The early/late expansion asymmetry is intentional: formal needs structural visibility; actual needs identity preservation.

## Test Plan

- New tests in `aliases.md` covering simple, nested, and union alias cases
- All 313 existing mdtests pass

Closes https://github.com/astral-sh/ty/issues/1851

---

_Review requested from @carljm by @bxff on 2026-01-18 13:12_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-18 13:12_

---

_Review requested from @sharkdp by @bxff on 2026-01-18 13:12_

---

_Review requested from @dcreager by @bxff on 2026-01-18 13:12_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_fileio.py:190:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`
+ src/anyio/_core/_fileio.py:190:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str | bytes]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`
- src/anyio/_core/_fileio.py:627:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`
+ src/anyio/_core/_fileio.py:627:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str | bytes]`, found `TextIOWrapper[_WrappedBuffer] | BinaryIO | IO[Any]`

pyinstrument (https://github.com/joerick/pyinstrument)
+ pyinstrument/vendor/decorator.py:111:36: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `Literal["*"]`
- pyinstrument/vendor/decorator.py:165:31: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None`
+ pyinstrument/vendor/decorator.py:165:31: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None | LiteralString`
- Found 43 diagnostics
+ Found 44 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/llnl/util/filesystem.py:1668:35: error[invalid-argument-type] Argument to function `exists` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1668:35: error[invalid-argument-type] Argument to function `exists` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `Sized | Unknown`
- lib/spack/spack/llnl/util/filesystem.py:1674:25: error[invalid-argument-type] Argument to function `move` is incorrect: Expected `str | PathLike[str]`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1674:25: error[invalid-argument-type] Argument to function `move` is incorrect: Expected `str | PathLike[str]`, found `Sized | Unknown`
+ lib/spack/spack/solver/asp.py:2157:48: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/cmd/deprecate.py:87:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/cmd/deprecate.py:87:44: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/cmd/deprecate.py:89:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/cmd/deprecate.py:90:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/test/concretization/core.py:1576:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | tuple[bool | str, ...] | bool | str`
+ lib/spack/spack/test/concretization/core.py:1576:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bool | str]`, found `Unknown | tuple[bool | str, ...] | bool | str`
- lib/spack/spack/test/concretization/core.py:1589:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | tuple[bool | str, ...] | bool | str`
+ lib/spack/spack/test/concretization/core.py:1589:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bool | str]`, found `Unknown | tuple[bool | str, ...] | bool | str`
+ lib/spack/spack/test/database.py:440:28: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/vendor/jinja2/lexer.py:762:35: error[call-non-callable] Object of type `str` is not callable
+ lib/spack/spack/vendor/jinja2/lexer.py:762:49: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str`, found `str | None`
+ lib/spack/spack/vendor/jsonschema/_legacy_validators.py:121:38: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Never]`, found `list[Unknown | str]`
+ lib/spack/spack/vendor/ruamel/yaml/emitter.py:1022:26: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:454:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str | Any]`, found `str | ((...) -> Any) | tuple[Any, ...] | Any | None`
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:458:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str | Any]`, found `str | ((...) -> Any) | tuple[Any, ...] | (Any & ~None)`
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:460:25: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:461:12: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `str | ((...) -> Any) | tuple[Any, ...] | Any | None`
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:471:75: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `str | ((...) -> Any) | tuple[Any, ...] | Any | None`
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:476:67: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `str | ((...) -> Any) | tuple[Any, ...] | Any | None`
- Found 4344 diagnostics
+ Found 4360 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/etag.py:22:32: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `(Iterable[str] & ~AlwaysFalsy) | tuple[()]`
+ src/werkzeug/datastructures/structures.py:283:20: error[invalid-return-type] Return type does not match returned value: expected `list[V@MultiDict] | list[T@getlist]`, found `list[V@MultiDict | T@getlist]`
- src/werkzeug/datastructures/structures.py:347:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/datastructures/structures.py:415:20: error[invalid-return-type] Return type does not match returned value: expected `dict[K@MultiDict, V@MultiDict] | dict[K@MultiDict, list[V@MultiDict]]`, found `dict[K@MultiDict, V@MultiDict | list[V@MultiDict]]`
+ src/werkzeug/datastructures/structures.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `dict[K@MultiDict, V@MultiDict] | dict[K@MultiDict, list[V@MultiDict]]`, found `dict[K@MultiDict, list[V@MultiDict] | V@MultiDict]`
+ src/werkzeug/datastructures/structures.py:1077:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Never]`, found `(Iterable[str] & ~AlwaysFalsy) | tuple[()]`
+ src/werkzeug/routing/exceptions.py:101:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Never]`, found `(Unknown & ~AlwaysFalsy) | (Mapping[str, Any] & ~AlwaysFalsy) | tuple[()]`
+ src/werkzeug/routing/exceptions.py:130:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Never]`, found `(Unknown & ~AlwaysFalsy) | (Mapping[str, Any] & ~AlwaysFalsy) | tuple[()]`
- Found 407 diagnostics
+ Found 413 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/sdp.py:446:52: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `None | Unknown`
+ src/aiortc/sdp.py:446:52: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `None | Unknown | str`

black (https://github.com/psf/black)
- src/black/trans.py:466:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- src/black/trans.py:980:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- Found 50 diagnostics
+ Found 48 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/pytester.py:1141:28: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `PytesterHelperPlugin`
- Found 413 diagnostics
+ Found 414 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/worktree.py:315:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Iterable[str | bytes | PathLike[str]] | bytes | PathLike[str]`
+ dulwich/worktree.py:315:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str | bytes | PathLike[str] | int]`, found `Iterable[str | bytes | PathLike[str]] | bytes | PathLike[str]`

ignite (https://github.com/pytorch/ignite)
- tests/ignite/distributed/utils/test_native.py:416:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | int | float | ... omitted 3 union elements`
+ tests/ignite/distributed/utils/test_native.py:416:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[int | float | str | Any]`, found `Unknown | int | float | ... omitted 3 union elements`

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/inference.py:379:72: error[invalid-argument-type] Argument to function `sorted` is incorrect: Expected `Iterable[str]`, found `(Unknown & Top[dict[Unknown, Unknown]]) | (RestrictLexicon & Top[dict[Unknown, Unknown]]) | dict[str, RestrictLexicon]`
- Found 415 diagnostics
+ Found 416 diagnostics

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `str`, found `Unknown | str | Confidence`
- pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `str`, found `Unknown | str | Confidence`
- pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `Confidence`, found `Unknown | str | Confidence`
- pylint/checkers/base/name_checker/checker.py:370:59: error[invalid-argument-type] Argument to bound method `_raise_name_warning` is incorrect: Expected `str`, found `Unknown | str | Confidence`
- Found 216 diagnostics
+ Found 212 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/RepositoryAdvisory.py:310:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str]`, found `Iterable[str] | (_NotSetType & Iterable[object])`
- Found 299 diagnostics
+ Found 300 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/pluginmanager.py:132:49: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `Unknown | str`
- Found 18 diagnostics
+ Found 17 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- dedupe/api.py:551:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dedupe/core.py:196:41: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `int | str`
+ dedupe/core.py:196:41: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `int | str`
- Found 52 diagnostics
+ Found 51 diagnostics

pylox (https://github.com/sco1/pylox)
+ pylox/builtins/py_builtins.py:175:32: error[invalid-argument-type] Argument to function `mean` is incorrect: Argument type `Unknown | None` does not satisfy constraints (`int | float`, `Decimal`, `Fraction`) of type variable `_NumberT`
+ pylox/builtins/py_builtins.py:175:32: error[invalid-argument-type] Argument to function `mean` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/builtins/py_builtins.py:192:34: error[invalid-argument-type] Argument to function `median` is incorrect: Argument type `Unknown | None` does not satisfy constraints (`int | float`, `Decimal`, `Fraction`) of type variable `_NumberT`
+ pylox/builtins/py_builtins.py:192:34: error[invalid-argument-type] Argument to function `median` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
+ pylox/builtins/py_builtins.py:216:16: error[invalid-return-type] Return type does not match returned value: expected `int | float`, found `Unknown | None | int | float`
+ pylox/builtins/py_builtins.py:326:33: error[invalid-argument-type] Argument to function `stdev` is incorrect: Argument type `Unknown | None` does not satisfy constraints (`int | float`, `Decimal`, `Fraction`) of type variable `_NumberT`
+ pylox/builtins/py_builtins.py:326:33: error[invalid-argument-type] Argument to function `stdev` is incorrect: Expected `Iterable[int | float]`, found `dict[Unknown, Unknown] | Unknown | deque[Unknown | None]`
- Found 48 diagnostics
+ Found 55 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`
+ tests/test_pytypes.py:457:31: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[int | str | Unknown | tuple[str, int]]`, found `Unknown | bytes | bytearray | ... omitted 5 union elements`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/openapi/checks.py:118:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Unknown | list[str | int]`
- src/schemathesis/specs/openapi/checks.py:491:42: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `set[Unknown | int] | set[int]`
- Found 282 diagnostics
+ Found 280 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
- tornado/gen.py:532:47: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]]`, found `dict_values[object, object] | (Sequence[None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]] & ~Top[dict[Unknown, Unknown]]) | (Mapping[Any, None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]] & ~Top[dict[Unknown, Unknown]])`
+ tornado/gen.py:532:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(object, /) -> _asyncio.Future[Unknown]`, found `def convert_yielded(yielded: None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | concurrent.futures._base.Future[Unknown]) -> _asyncio.Future[Unknown]`

vision (https://github.com/pytorch/vision)
- test/test_datasets.py:461:37: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | str | tuple[str, ...] | int`
+ test/test_datasets.py:461:37: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `Unknown | str | tuple[str, ...] | int`
- test/test_datasets.py:461:74: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | str | tuple[str, ...] | int`
+ test/test_datasets.py:461:74: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `Unknown | str | tuple[str, ...] | int`
- torchvision/ops/poolers.py:283:34: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int] | list[int] | int`
+ torchvision/ops/poolers.py:283:34: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[int]`, found `tuple[int] | list[int] | int`
- torchvision/transforms/v2/_container.py:149:19: warning[division-by-zero] Cannot divide object of type `int` by zero
- torchvision/transforms/v2/_container.py:149:19: warning[division-by-zero] Cannot divide object of type `float` by zero
- Found 1403 diagnostics
+ Found 1401 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/study/_multi_objective.py:37:50: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[int | float] | None`
+ optuna/study/_multi_objective.py:37:50: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[int | float]`, found `Unknown | list[int | float] | None`
- optuna/study/study.py:496:13: error[invalid-argument-type] Argument to function `_optimize` is incorrect: Expected `tuple[type[Exception], ...]`, found `tuple[object, ...]`
+ optuna/study/study.py:496:25: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[type[Exception]]`, found `Iterable[type[Exception]] | (type[Exception] & Iterable[object])`
- optuna/visualization/_rank.py:268:68: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[int | float] | None`
+ optuna/visualization/_rank.py:268:68: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[int | float]`, found `Unknown | list[int | float] | None`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/net/http/url.py:52:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ test/mitmproxy/proxy/tutils.py:68:64: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Command | Event]`, found `(Command & Iterable[object]) | (Event & Iterable[object]) | Iterable[Command | Event]`
+ test/mitmproxy/proxy/tutils.py:68:67: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Command | Event]`, found `(Command & Iterable[object]) | (Event & Iterable[object]) | Iterable[Command | Event]`
- Found 2143 diagnostics
+ Found 2144 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/waiting.py:266:17: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `tuple[int] | tuple[()]`
- psycopg/psycopg/waiting.py:267:17: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `tuple[int] | tuple[()]`
- Found 652 diagnostics
+ Found 650 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/_internal/__init__.py:470:88: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `(Sequence[CommandOption] & ~AlwaysFalsy) | tuple[()]`
+ tanjun/_internal/__init__.py:470:94: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Never]`, found `(Sequence[CommandOption] & ~AlwaysFalsy) | tuple[()]`
- Found 131 diagnostics
+ Found 133 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ koda_validate/tuple.py:261:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((tuple[T1@typed] | tuple[T1@typed, T2@typed] | tuple[T1@typed, T2@typed, T3@typed] | ... omitted 5 union elements, /) -> CoercionErr | ContainerErr | ExtraKeysErr | ... omitted 10 union elements) | None`, found `((tuple[T1@typed], /) -> CoercionErr | ContainerErr | ExtraKeysErr | ... omitted 10 union elements) | None | ((tuple[T1@typed, T2@typed], /) -> CoercionErr | ContainerErr | ExtraKeysErr | ... omitted 10 union elements) | ... omitted 6 union elements`
- Found 405 diagnostics
+ Found 406 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ bson/__init__.py:1097:77: error[invalid-argument-type] Argument to function `_bson_to_dict` is incorrect: Expected `CodecOptions[_DocumentType@decode | dict[str, Any]]`, found `CodecOptions[_DocumentType@decode] | CodecOptions[dict[str, Any]]`
- bson/__init__.py:1378:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ bson/__init__.py:1332:39: error[invalid-argument-type] Argument to function `_bson_to_dict` is incorrect: Expected `CodecOptions[_DocumentType@decode_iter | dict[str, Any]]`, found `CodecOptions[_DocumentType@decode_iter] | CodecOptions[dict[str, Any]]`
+ pymongo/asynchronous/command_cursor.py:351:20: error[invalid-return-type] Return type does not match returned value: expected `_DocumentType@AsyncCommandCursor | None`, found `Unknown | Mapping[str, Any]`
- pymongo/asynchronous/encryption.py:441:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AsyncMongoClient[Mapping[str, Any]] | None`, found `None | AsyncMongoClient[_DocumentTypeArg@__init__]`
+ pymongo/asynchronous/encryption.py:441:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AsyncMongoClient[Unknown | Mapping[str, Any]] | None`, found `None | AsyncMongoClient[_DocumentTypeArg@__init__]`
+ pymongo/synchronous/command_cursor.py:351:20: error[invalid-return-type] Return type does not match returned value: expected `_DocumentType@CommandCursor | None`, found `Unknown | Mapping[str, Any]`
- pymongo/synchronous/encryption.py:438:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MongoClient[Mapping[str, Any]] | None`, found `None | MongoClient[_DocumentTypeArg@__init__]`
+ pymongo/synchronous/encryption.py:438:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MongoClient[Unknown | Mapping[str, Any]] | None`, found `None | MongoClient[_DocumentTypeArg@__init__]`
- Found 440 diagnostics
+ Found 443 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:2871:22: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ValueError | ExceptionGroup[ValueError]`
- Found 487 diagnostics
+ Found 486 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/vs2010backend.py:558:42: error[invalid-argument-type] Argument to bound method `gen_vcxproj` is incorrect: Expected `BuildTarget`, found `CustomTarget | BuildTarget`
+ mesonbuild/backend/vs2010backend.py:560:33: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, Path, str, MachineChoice]`, found `tuple[str, PurePath, str, MachineChoice]`
- Found 2157 diagnostics
+ Found 2159 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/utils.py:243:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 542 diagnostics
+ Found 541 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
+ src/zope/interface/_flatten.py:35:14: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `None`
- src/zope/interface/tests/test_declarations.py:2057:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | str | ((*interfaces) -> Unknown) | InterfaceClass`
+ src/zope/interface/tests/test_declarations.py:2057:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str]`, found `Unknown | str | ((*interfaces) -> Unknown) | InterfaceClass`
- Found 422 diagnostics
+ Found 423 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/command_line_tool.py:1253:46: error[invalid-argument-type] Argument to function `shortname` is incorrect: Expected `str`, found `Unknown | None | int | ... omitted 4 union elements`
- Found 252 diagnostics
+ Found 253 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/sources/DataSourceOracle.py:371:27: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["macAddr"]` on object of type `str`
+ cloudinit/sources/DataSourceOracle.py:372:28: error[unresolved-attribute] Object of type `str` has no attribute `get`
+ cloudinit/sources/DataSourceOracle.py:374:23: error[unresolved-attribute] Object of type `str` has no attribute `get`
+ cloudinit/sources/DataSourceOracle.py:384:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["ipv6Addresses"]` on object of type `str`
+ cloudinit/sources/DataSourceOracle.py:387:48: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["subnetCidrBlock"]` on object of type `str`
+ cloudinit/sources/DataSourceOracle.py:396:20: error[unresolved-attribute] Object of type `str` has no attribute `get`
+ cloudinit/sources/DataSourceOracle.py:401:36: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["privateIp"]` on object of type `str`
+ cloudinit/sources/DataSourceOracle.py:406:20: error[unresolved-attribute] Object of type `str` has no attribute `get`
+ cloudinit/sources/DataSourceOracle.py:411:36: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["ipv6Addresses"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1007:16: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `str | Unknown | dict[Unknown | str, Unknown | str | int]`
+ tests/unittests/test_ds_identify.py:1012:40: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["out"]` on object of type `str`
+ tests/unittests/test_ds_identify.py:1012:40: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `Unknown | str | int`
+ tests/unittests/test_ds_identify.py:1485:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["name"]` on object of type `str`
- Found 1169 diagnostics
+ Found 1182 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/compilers/C/base.py:449:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, list[tuple[str] | tuple[str, str | None]], list[str]]`, found `tuple[str | None, list[tuple[str] | tuple[str, str | None]] | Unknown, list[str] | list[Unknown]]`
+ setuptools/_distutils/compilers/C/base.py:449:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, list[tuple[str] | tuple[str, str | None]], list[str]]`, found `tuple[str | None, list[tuple[str] | tuple[str, str | None]] | Unknown, list[str] | Unknown]`
- setuptools/_distutils/version.py:173:41: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Unknown | tuple[int, ...]`
- setuptools/command/editable_wheel.py:420:19: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes, /) -> str | bytes`, found `Overload[(path: str) -> str, (path: bytes) -> bytes, [AnyStr](path: PathLike[AnyStr]) -> AnyStr]`
+ setuptools/command/editable_wheel.py:420:19: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str | bytes | Path, /) -> str | bytes | Path`, found `Overload[(path: str) -> str, (path: bytes) -> bytes, [AnyStr](path: PathLike[AnyStr]) -> AnyStr]`
- setuptools/command/editable_wheel.py:420:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str | bytes]`, found `Unknown | list[Path]`
- Found 1131 diagnostics
+ Found 1129 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/federation/enum.py:102:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `Authenticated`
+ strawberry/federation/enum.py:105:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `Inaccessible`
+ strawberry/federation/enum.py:108:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `Policy`
+ strawberry/federation/enum.py:111:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `RequiresScopes`
+ strawberry/federation/enum.py:114:27: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Never]`, found `GeneratorType[Tag, None, None]`
+ strawberry/relay/types.py:894:17: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[NodeType@ListConnection]`, found `Iterable[NodeType@ListConnection] | (AsyncIterable[NodeType@ListConnection] & Iterable[object])`
- Found 345 diagnostics
+ Found 351 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:151:16: error[invalid-return-type] Return type does not match returned value: expected `R@PrefectRayFuture`, found `(R@PrefectRayFuture & ~Coroutine[object, Never, object]) | (Exception & ~Coroutine[object, Never, object]) | Unknown`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:151:16: error[invalid-return-type] Return type does not match returned value: expected `R@PrefectRayFuture`, found `R@PrefectRayFuture | Exception | Unknown`
- src/prefect/_vendor/croniter/croniter.py:1080:21: error[unsupported-operator] Operator `+=` is not supported between objects of type `list[LiteralString]` and `list[str | Unknown | int]`
+ src/prefect/_vendor/croniter/croniter.py:1080:21: error[unsupported-operator] Operator `+=` is not supported between objects of type `list[str]` and `list[str | Unknown | int]`
+ src/prefect/cli/artifact.py:84:45: warning[possibly-missing-attribute] Attribute `latest_id` may be missing on object of type `Artifact | ArtifactCollection | Unknown`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/flow_engine.py:531:13: error[invalid-argument-type] Argument to bound method `create_flow_run` is incorrect: Expected `Flow[(...), R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine] | Unknown]`, found `Flow[P@FlowRunEngine, R@FlowRunEngine] | Flow[P@FlowRunEngine, Coroutine[Any, Any, R@FlowRunEngine]]`
+ src/prefect/flow_engine.py:1115:13: error[invalid-argument-type] Argument to bound method `create_flow_run` is incorrect: Expected `Flow[(...), R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine] | Unknown]`, found `Flow[P@AsyncFlowRunEngine, R@AsyncFlowRunEngine] | Flow[P@AsyncFlowRunEngine, Coroutine[Any, Any, R@AsyncFlowRunEngine]]`
- src/prefect/server/models/flow_runs.py:582:15: error[missing-argument] No argument provided for required parameter `db`
+ src/prefect/server/models/flow_runs.py:582:15: error[unresolved-attribute] Object of type `OrchestrationContext[FlowRun, FlowRunPolicy]` has no attribute `validate_proposed_state`
- src/prefect/server/models/task_runs.py:556:15: error[missing-argument] No argument provided for required parameter `db`
+ src/prefect/server/models/task_runs.py:556:15: error[unresolved-attribute] Object of type `OrchestrationContext[TaskRun, TaskRunPolicy]` has no attribute `validate_proposed_state`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5411 diagnostics
+ Found 5414 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/commands/plist.py:418:33: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[int] | None | list[Unknown | int]`
+ pwndbg/commands/plist.py:418:33: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[int | Unknown]`, found `list[int] | None | list[Unknown | int]`

xarray (https://github.com/pydata/xarray)
+ xarray/backends/api.py:1701:26: warning[redundant-cast] Value is already of type `str`
+ xarray/backends/zarr.py:1091:25: error[invalid-assignment] Object of type `dict[str, dict[Unknown | str, Unknown | bool] | bool | dict[str, bool]]` is not assignable to `dict[str, bool] | dict[str, dict[str, bool]]`
+ xarray/backends/zarr.py:1093:25: error[invalid-assignment] Object of type `dict[str, (Unknown & ~None) | bool | dict[str, bool]]` is not assignable to `dict[str, bool] | dict[str, dict[str, bool]]`
- xarray/core/variable.py:2217:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | Literal[False] | list[Hashable]`
+ xarray/core/variable.py:2217:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Hashable]`, found `Unknown | Literal[False] | list[Hashable]`
+ xarray/structure/combine.py:763:65: error[invalid-argument-type] Argument to function `_infer_concat_order_from_coords` is incorrect: Expected `list[Dataset] | list[DataTree]`, found `list[Dataset | DataTree]`
- Found 1762 diagnostics
+ Found 1766 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/sparse/test_extract.pyi:37:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:38:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:39:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:40:1: error[type-assertion-failure] Type `bsr_array[floating[_32Bit]]` does not match asserted type `bsr_array[Any]`
- tests/sparse/test_extract.pyi:41:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:42:1: error[type-assertion-failure] Type `csc_array[floating[_32Bit]]` does not match asserted type `csc_array[Any]`
- tests/sparse/test_extract.pyi:43:1: error[type-assertion-failure] Type `csr_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `csr_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:44:1: error[type-assertion-failure] Type `dia_array[floating[_32Bit]]` does not match asserted type `dia_array[Any]`
- tests/sparse/test_extract.pyi:45:1: error[type-assertion-failure] Type `dok_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `dok_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:46:1: error[type-assertion-failure] Type `lil_array[floating[_32Bit]]` does not match asserted type `lil_array[Any]`
- tests/sparse/test_extract.pyi:59:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:60:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:61:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:62:1: error[type-assertion-failure] Type `bsr_array[floating[_32Bit]]` does not match asserted type `bsr_array[Any]`
- tests/sparse/test_extract.pyi:63:1: error[type-assertion-failure] Type `coo_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `coo_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:64:1: error[type-assertion-failure] Type `csc_array[floating[_32Bit]]` does not match asserted type `csc_array[Any]`
- tests/sparse/test_extract.pyi:65:1: error[type-assertion-failure] Type `csr_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `csr_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:66:1: error[type-assertion-failure] Type `dia_array[floating[_32Bit]]` does not match asserted type `dia_array[Any]`
- tests/sparse/test_extract.pyi:67:1: error[type-assertion-failure] Type `dok_array[floating[_32Bit], tuple[int, int]]` does not match asserted type `dok_array[Any, tuple[int, int]]`
- tests/sparse/test_extract.pyi:68:1: error[type-assertion-failure] Type `lil_array[floating[_32Bit]]` does not match asserted type `lil_array[Any]`
- Found 1304 diagnostics
+ Found 1284 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ lib/Crypto/SelfTest/Hash/test_CMAC.py:434:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal[4]` and value of type `dict[str, str | ModuleType]` on object of type `list[str | ModuleType]`
- Found 1326 diagnostics
+ Found 1327 diagnostics

altair (https://github.com/vega/altair)
+ altair/datasets/_reader.py:427:16: error[invalid-return-type] Return type does not match returned value: expected `Reader[IntoDataFrameT@reader, IntoFrameT@reader] | Reader[IntoDataFrameT@reader, LazyFrame[Unknown]]`, found `Reader[IntoDataFrameT@reader, IntoFrameT@reader | LazyFrame[Unknown]]`
+ altair/datasets/_reader.py:429:16: error[invalid-return-type] Return type does not match returned value: expected `Reader[IntoDataFrameT@reader, IntoFrameT@reader] | Reader[IntoDataFrameT@reader, LazyFrame[Unknown]]`, found `Reader[IntoDataFrameT@reader, IntoFrameT@reader | LazyFrame[Unknown]]`
+ tests/test_datasets.py:247:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[ChunkedArray[Any]]`, found `Unknown | Index[str] | list[ChunkedArray[Any]]`
- Found 1058 diagnostics
+ Found 1061 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/worker.py:146:24: error[invalid-argument-type] Argument to function `set_result` is incorrect: Expected `Future[Literal[True]]`, found `Future[bool] | (Unknown & ~None)`
- Found 181 diagnostics
+ Found 180 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/embed/standalone.py:280:19: error[invalid-assignment] Object of type `list[str | RenderRoot]` is not assignable to `list[str] | list[RenderRoot]`
+ src/bokeh/layouts.py:294:36: error[invalid-argument-type] Argument to function `_parse_children_arg` is incorrect: Argument type `UIElement | None` does not satisfy upper bound `LayoutDOM` of type variable `L`
- src/bokeh/layouts.py:298:20: error[invalid-assignment] Object of type `list[Sequence[Unknown]]` is not assignable to `list[UIElement | None] | list[list[UIElement | None]]`
+ src/bokeh/layouts.py:298:20: error[invalid-assignment] Object of type `list[Sequence[Unknown] | UIElement | None | list[UIElement | None]]` is not assignable to `list[UIElement | None] | list[list[UIElement | None]]`
+ src/bokeh/server/tornado.py:295:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Application & Top[(...) -> object]) | (Unknown & Top[(...) -> object])`
+ src/bokeh/server/tornado.py:295:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Application & Top[(...) -> object]) | (Unknown & Top[(...) -> object])`
- Found 876 diagnostics
+ Found 880 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_iast/taint_sinks/code_injection.py:113:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `None | Unknown | dict[str, Any]`
+ ddtrace/appsec/_iast/taint_sinks/code_injection.py:113:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[str]`, found `None | Unknown | dict[str, Any]`
- ddtrace/contrib/internal/subprocess/patch.py:320:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[Unknown] | None | list[str] | list[str | Unknown]`
+ ddtrace/contrib/internal/subprocess/patch.py:320:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown | str]`, found `Unknown | list[Unknown] | None | list[str] | list[str | Unknown]`
- ddtrace/internal/settings/_core.py:42:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | None`, found `ChainMap[str, str]`
+ ddtrace/internal/settings/_core.py:42:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | None`, found `ChainMap[str | Unknown, str | Unknown]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/externals/array_api_extra/_lib/_funcs.py:129:18: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Array]`, found `(Array & tuple[object, ...]) | tuple[Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_funcs.py:855:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[int]`, found `tuple[int, int] | (Sequence[tuple[int, int]] & ~int)`
+ sklearn/neighbors/_classification.py:307:28: warning[possibly-missing-attribute] Attribute `take` may be missing on object of type `Unknown | list[Unknown]`
+ sklearn/neighbors/_classification.py:402:44: warning[possibly-missing-attribute] Attribute `size` may be missing on object of type `Unknown | list[Unknown]`
+ sklearn/neighbors/_classification.py:848:44: warning[possibly-missing-attribute] Attribute `size` may be missing on object of type `Unknown | list[Unknown]`
+ sklearn/neighbors/_classification.py:849:49: warning[possibly-missing-attribute] Attribute `size` may be missing on object of type `Unknown | list[Unknown]`
+ sklearn/neighbors/_classification.py:854:66: warning[possibly-missing-attribute] Attribute `size` may be missing on object of type `Unknown | list[Unknown]`
+ sklearn/neighbors/_classification.py:858:52: warning[possibly-missing-attribute] Attribute `size` may be missing on object of type `Unknown | list[Unknown]`
- Found 2423 diagnostics
+ Found 2431 diagnostics

cryptography (https://github.com/pyca/cryptography)
+ src/cryptography/x509/extensions.py:1562:16: error[invalid-return-type] Return type does not match returned value: expected `list[IPv4Address | IPv6Address | IPv4Network | IPv6N

... (truncated 252 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2026-01-18 13:21_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-18 13:28_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:33_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 101 | 31 | 82 |
| `invalid-return-type` | 15 | 2 | 8 |
| `type-assertion-failure` | 0 | 20 | 0 |
| `possibly-missing-attribute` | 16 | 0 | 1 |
| `unused-ignore-comment` | 0 | 15 | 0 |
| `invalid-assignment` | 5 | 1 | 6 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `unresolved-attribute` | 7 | 0 | 0 |
| `no-matching-overload` | 4 | 0 | 0 |
| `unsupported-operator` | 2 | 0 | 2 |
| `division-by-zero` | 0 | 2 | 0 |
| `missing-argument` | 0 | 2 | 0 |
| `unresolved-reference` | 0 | 2 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **152** | **75** | **106** |


**[Full report with detailed diff](https://4d9dab59.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://4d9dab59.ty-ecosystem-ext.pages.dev/timing))



---

_Renamed from "[ty] Support solving generics involving PEP 695 type aliases (#1851)" to "[ty] Support solving generics involving PEP 695 type aliases" by @AlexWaygood on 2026-01-18 16:22_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:2085 on 2026-01-21 02:28_

This seems unrelated to the type alias change? Handling of unions is a known limitation of the current constraint solver, I don't think it should be addressed in this PR.

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:1748 on 2026-01-21 02:31_

I'd prefer if we put this branch into the match statement directly (as the first branch, if necessary).

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:2078 on 2026-01-21 02:32_

```suggestion
            // Expand type aliases in the actual type.
            //
            // This is placed at the end of the match block to avoid expanding the type alias
            // when it can be matched directly against a type variable in the formal type,
            // e.g., `reveal_type(alias)` should reveal the type alias, not its value type.
```

---

_@ibraheemdev reviewed on 2026-01-21 02:33_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-21 08:30_

---
