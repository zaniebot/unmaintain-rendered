```yaml
number: 20479
title: "[ty] Use `C[T]` instead of `C[Unknown]` for the upper bound of `Self`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1196
created_at: 2025-09-19T08:45:49Z
updated_at: 2025-09-23T12:02:27Z
url: https://github.com/astral-sh/ruff/pull/20479
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Use `C[T]` instead of `C[Unknown]` for the upper bound of `Self`

---

_Pull request opened by @sharkdp on 2025-09-19 08:45_

### Summary

This PR includes two changes, both of which are necessary to resolve https://github.com/astral-sh/ty/issues/1196:

* For a generic class `C[T]`, we previously used `C[Unknown]` as the upper bound of the `Self` type variable. There were two problems with this. For one, when `Self` appeared in contravariant position, we would materialize its upper bound to `Bottom[C[Unknown]]` (which might simplify to `C[Never]` if `C` is covariant in `T`) when accessing methods on `Top[C[Unknown]]`. This would result in `invalid-argument` errors on the `self` parameter. Also, using an upper bound of `C[Unknown]` would mean that inside methods, references to `T` would be treated as `Unknown`. This could lead to false negatives. To fix this, we now use `C[T]` (with a "nested" typevar) as the upper bound for `Self` on `C[T]`.
* In order to make this work, we needed to allow assignability/subtyping of inferable typevars to other types, since we now check assignability of e.g. `C[int]` to `C[T]` (when checking assignability to the upper bound of `Self`) when calling an instance-method on `C[int]` whose `self` parameter is annotated as `self: Self` (or implicitly `Self`, following https://github.com/astral-sh/ruff/pull/18007).

closes https://github.com/astral-sh/ty/issues/1196
closes https://github.com/astral-sh/ty/issues/1208


### Test Plan

Regression tests for both issues.

---

_Label `ty` added by @sharkdp on 2025-09-19 08:45_

---

_Comment by @github-actions[bot] on 2025-09-19 08:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-23 11:27:35.083401635 +0000
+++ new-output.txt	2025-09-23 11:27:38.269417422 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b816)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(12e7a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1327a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-19 09:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:2530:13: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `(...) -> Future[T_Retval@run_async_from_thread]`, found `def run_coroutine_threadsafe[_T](coro: Coroutine[Any, Any, _T@run_coroutine_threadsafe], loop: AbstractEventLoop) -> Future[_T@run_coroutine_threadsafe]`
- Found 227 diagnostics
+ Found 226 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:185:5: error[invalid-assignment] Object of type `staticmethod` is not assignable to `(Any, /) -> bool`
- Found 303 diagnostics
+ Found 302 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/specs/openapi/negative/mutations.py:463:33: error[invalid-parameter-default] Default value of type `def ident[T](x: T@ident) -> T@ident` is not assignable to annotated parameter type `(T@ordered, /) -> Any`
- Found 272 diagnostics
+ Found 271 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- src/typeshed_stats/gather.py:1000:13: error[invalid-argument-type] Argument to bound method `from_lines` is incorrect: Expected `str | ((str, /) -> Pattern)`, found `<class 'GitWildMatchPattern'>`
- Found 25 diagnostics
+ Found 24 diagnostics

Expression (https://github.com/cognitedata/Expression)
- expression/collections/seq.py:637:41: error[invalid-argument-type] Argument to function `init_infinite` is incorrect: Expected `(int, /) -> Unknown`, found `def identity[_A](value: _A@identity) -> _A@identity`
- expression/extra/result/traversable.py:50:21: error[invalid-argument-type] Argument to function `traverse` is incorrect: Expected `(Result[_TSource@sequence, _TError@sequence], /) -> Result[Unknown, Unknown]`, found `def identity[_A](value: _A@identity) -> _A@identity`
- tests/test_array.py:357:1: error[invalid-assignment] Object of type `def singleton[_TSource](value: _TSource@singleton) -> TypedArray[_TSource@singleton]` is not assignable to `(int, /) -> TypedArray[int]`
- tests/test_block.py:335:1: error[invalid-assignment] Object of type `def singleton[_TSource](value: _TSource@singleton) -> Block[_TSource@singleton]` is not assignable to `(int, /) -> Block[int]`
- tests/test_map.py:49:19: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Map[str, int], /) -> Unknown`, found `def to_seq[_Key, _Value](table: Map[_Key@to_seq, _Value@to_seq]) -> Iterable[tuple[_Key@to_seq, _Value@to_seq]]`
- tests/test_option.py:28:21: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Option[Literal[42]], /) -> Unknown`, found `def is_some[_TSource](option: Option[_TSource@is_some]) -> @Todo(`TypeGuard[]` special form)`
- tests/test_option.py:29:21: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Option[Literal[42]], /) -> Unknown`, found `def is_none[_TSource](option: Option[_TSource@is_none]) -> @Todo(`TypeGuard[]` special form)`
- tests/test_parser.py:232:9: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Parser[str], /) -> Unknown`, found `def many[_A](parser: Parser[_A@many]) -> Parser[Block[_A@many]]`
- tests/test_result.py:487:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def swap[_TSource, _TError](result: Result[_TSource@swap, _TError@swap]) -> Result[_TError@swap, _TSource@swap]`
- tests/test_result.py:573:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def merge[_TSource](result: Result[_TSource@merge, _TSource@merge]) -> _TSource@merge`
- tests/test_seq.py:308:20: error[invalid-argument-type] Argument to bound method `choose` is incorrect: Expected `(None | Literal[42], /) -> Option[Unknown]`, found `def of_optional[_TSource](value: _TSource@of_optional | None) -> Option[_TSource@of_optional]`
- tests/test_seq.py:321:1: error[invalid-assignment] Object of type `def singleton[_TSource](item: _TSource@singleton) -> Seq[_TSource@singleton]` is not assignable to `(int, /) -> Seq[int]`
- Found 223 diagnostics
+ Found 211 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/__init__.py:119:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT@notWindows, /) -> _TestFuncT@notWindows`, found `Unknown | MarkDecorator`
- test/__init__.py:126:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT@onlyBrotli, /) -> _TestFuncT@onlyBrotli`, found `Unknown | MarkDecorator`
- test/__init__.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT@notBrotli, /) -> _TestFuncT@notBrotli`, found `Unknown | MarkDecorator`
- test/__init__.py:138:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT@onlyZstd, /) -> _TestFuncT@onlyZstd`, found `Unknown | MarkDecorator`
- test/__init__.py:145:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT@notZstd, /) -> _TestFuncT@notZstd`, found `Unknown | MarkDecorator`
- test/__init__.py:205:12: error[invalid-return-type] Return type does not match returned value: expected `(_TestFuncT@resolvesLocalhostFQDN, /) -> _TestFuncT@resolvesLocalhostFQDN`, found `Unknown | MarkDecorator`
- Found 386 diagnostics
+ Found 380 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/utils.py:290:5: error[invalid-parameter-default] Default value of type `def equivalent[T](first: T@equivalent, second: T@equivalent) -> bool` is not assignable to annotated parameter type `(V@update_safety_check, V@update_safety_check, /) -> bool`
- xarray/core/utils.py:318:5: error[invalid-parameter-default] Default value of type `def equivalent[T](first: T@equivalent, second: T@equivalent) -> bool` is not assignable to annotated parameter type `(V@remove_incompatible_items, V@remove_incompatible_items, /) -> bool`
- xarray/core/utils.py:390:5: error[invalid-parameter-default] Default value of type `def equivalent[T](first: T@equivalent, second: T@equivalent) -> bool` is not assignable to annotated parameter type `(V@dict_equiv, V@dict_equiv, /) -> bool`
- xarray/core/utils.py:417:5: error[invalid-parameter-default] Default value of type `def equivalent[T](first: T@equivalent, second: T@equivalent) -> bool` is not assignable to annotated parameter type `(V@compat_dict_intersection, V@compat_dict_intersection, /) -> bool`
- xarray/core/utils.py:445:5: error[invalid-parameter-default] Default value of type `def equivalent[T](first: T@equivalent, second: T@equivalent) -> bool` is not assignable to annotated parameter type `(V@compat_dict_union, V@compat_dict_union, /) -> bool`
- Found 1600 diagnostics
+ Found 1595 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/core.py:2123:12: error[invalid-return-type] Return type does not match returned value: expected `(T@has_any_role, /) -> T@has_any_role`, found `Check[Unknown]`
- discord/ext/commands/core.py:2156:12: error[invalid-return-type] Return type does not match returned value: expected `(T@bot_has_role, /) -> T@bot_has_role`, found `Check[Unknown]`
- discord/ext/commands/core.py:2185:12: error[invalid-return-type] Return type does not match returned value: expected `(T@bot_has_any_role, /) -> T@bot_has_any_role`, found `Check[Unknown]`
- Found 512 diagnostics
+ Found 509 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_path.py:235:34: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def readlink(self) -> Self@readlink`
- src/trio/_path.py:236:32: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def rename(self, target: str | PurePath) -> Self@rename`
- src/trio/_path.py:237:33: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def replace(self, target: str | PurePath) -> Self@replace`
- src/trio/_path.py:238:33: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def resolve(self, strict: bool = Literal[False]) -> Self@resolve`
- src/trio/_path.py:245:34: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def absolute(self) -> Self@absolute`
- src/trio/_path.py:246:36: error[invalid-argument-type] Argument to function `_wrap_method_path` is incorrect: Expected `(...) -> Path`, found `def expanduser(self) -> Self@expanduser`
- src/trio/_tests/test_testing_raisesgroup.py:390:10: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/trio/_tests/test_threads.py:89:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync[RetT](fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
- src/trio/_tests/test_threads.py:96:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync[RetT](fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
- src/trio/_tests/test_threads.py:104:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run[RetT](afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`
- src/trio/_tests/test_threads.py:112:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run[RetT](afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`
- src/trio/_tests/type_tests/raisesgroup.py:98:5: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/trio/_tests/type_tests/raisesgroup.py:103:5: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/trio/_tests/type_tests/raisesgroup.py:119:5: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ src/trio/_tests/type_tests/raisesgroup.py:92:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/trio/_tests/type_tests/raisesgroup.py:95:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/trio/_tests/type_tests/raisesgroup.py:99:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/trio/_tests/type_tests/raisesgroup.py:102:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/trio/_tests/type_tests/raisesgroup.py:106:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/trio/_tests/type_tests/raisesgroup.py:107:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 673 diagnostics
+ Found 665 diagnostics

mypy (https://github.com/python/mypy)
- mypy/modulefinder.py:41:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr@abspath]) -> AnyStr@abspath, (path: AnyStr@abspath) -> AnyStr@abspath]`
- mypy/modulefinder.py:43:36: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr@abspath]) -> AnyStr@abspath, (path: AnyStr@abspath) -> AnyStr@abspath]`
- mypy/modulefinder.py:45:39: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr@abspath]) -> AnyStr@abspath, (path: AnyStr@abspath) -> AnyStr@abspath]`
- mypy/modulefinder.py:47:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr@abspath]) -> AnyStr@abspath, (path: AnyStr@abspath) -> AnyStr@abspath]`
- Found 1858 diagnostics
+ Found 1854 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/process.py:1182:5: error[invalid-parameter-default] Default value of type `def urljoin[AnyStr](base: AnyStr@urljoin, url: AnyStr@urljoin | None, allow_fragments: bool = Literal[True]) -> AnyStr@urljoin` is not assignable to annotated parameter type `(str, str, /) -> str`
- Found 144 diagnostics
+ Found 143 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/dependencies/python.py:104:33: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Unknown, str, /) -> Unknown`, found `Overload[(a: Sequence[_T@getitem], b: slice[Any, Any, Any], /) -> Sequence[_T@getitem], (a: SupportsGetItem[_K@getitem, _V@getitem], b: _K@getitem, /) -> _V@getitem]`
- unittests/helpers.py:228:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> R@xfail_if_jobname, /) -> (...) -> R@xfail_if_jobname`, found `def expectedFailure[_FT](test_item: _FT@expectedFailure) -> _FT@expectedFailure`
- unittests/linuxliketests.py:712:48: error[invalid-argument-type] Argument to function `copytree` is incorrect: Expected `(str, str, /) -> object`, found `def copyfile[_StrOrBytesPathT](src: @Todo(Support for `typing.TypeAlias`), dst: _StrOrBytesPathT@copyfile, *, follow_symlinks: bool = Literal[True]) -> _StrOrBytesPathT@copyfile`
- Found 850 diagnostics
+ Found 847 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/command/install.py:718:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(path: PathLike[AnyStr@normpath]) -> AnyStr@normpath, (path: AnyOrLiteralStr@normpath) -> AnyOrLiteralStr@normpath]`
- setuptools/_distutils/tests/test_bdist_dumb.py:74:44: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `Overload[(p: PathLike[AnyStr@basename]) -> AnyStr@basename, (p: AnyOrLiteralStr@basename) -> AnyOrLiteralStr@basename]`
- Found 771 diagnostics
+ Found 769 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/catalog/utils/__init__.py:31:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 713 diagnostics
+ Found 714 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/dbg/gdb/__init__.py:1666:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def exit[T](func: () -> T@exit, **kwargs: Any) -> () -> T@exit`
- pwndbg/dbg/gdb/__init__.py:1668:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def cont[T](func: () -> T@cont, **kwargs: Any) -> () -> T@cont`
- pwndbg/dbg/gdb/__init__.py:1670:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def start[T](func: () -> T@start, **kwargs: Any) -> () -> T@start`
- pwndbg/dbg/gdb/__init__.py:1672:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def stop[T](func: () -> T@stop, **kwargs: Any) -> () -> T@stop`
- pwndbg/dbg/gdb/__init__.py:1674:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def new_objfile[T](func: () -> T@new_objfile, **kwargs: Any) -> () -> T@new_objfile`
- pwndbg/dbg/gdb/__init__.py:1676:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def mem_changed[T](func: () -> T@mem_changed, **kwargs: Any) -> () -> T@mem_changed`
- pwndbg/dbg/gdb/__init__.py:1678:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def reg_changed[T](func: () -> T@reg_changed, **kwargs: Any) -> () -> T@reg_changed`
- Found 2529 diagnostics
+ Found 2522 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/tests/test_numeric.py:1079:26: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(Unknown, Unknown, Unknown | int, /) -> Unknown`, found `Overload[(arg1: SupportsRichComparisonT@min, arg2: SupportsRichComparisonT@min, /, *_args: SupportsRichComparisonT@min, *, key: None = None) -> SupportsRichComparisonT@min, (arg1: _T@min, arg2: _T@min, /, *_args: _T@min, *, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None) -> SupportsRichComparisonT@min, (iterable: Iterable[_T@min], *, /, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None, default: _T@min) -> SupportsRichComparisonT@min | _T@min, (iterable: Iterable[_T1@min], *, /, key: (_T1@min, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@min) -> _T1@min | _T2@min]`
- ibis/backends/tests/test_numeric.py:1091:26: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(Unknown, Unknown, Unknown | int, /) -> Unknown`, found `Overload[(arg1: SupportsRichComparisonT@max, arg2: SupportsRichComparisonT@max, /, *_args: SupportsRichComparisonT@max, *, key: None = None) -> SupportsRichComparisonT@max, (arg1: _T@max, arg2: _T@max, /, *_args: _T@max, *, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None) -> SupportsRichComparisonT@max, (iterable: Iterable[_T@max], *, /, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None, default: _T@max) -> SupportsRichComparisonT@max | _T@max, (iterable: Iterable[_T1@max], *, /, key: (_T1@max, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@max) -> _T1@max | _T2@max]`
- Found 3281 diagnostics
+ Found 3279 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:868:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(typing.TypeVar, /) -> Unknown`, found `(def _simple_entrystr[KeyEntry](key: KeyEntry@_simple_entrystr) -> str) | <class 'str'>`
- Found 2249 diagnostics
+ Found 2248 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/nordpool/sensor.py:205:59: error[invalid-argument-type] Argument to function `get_min_max_price` is incorrect: Expected `(int | float, int | float, /) -> int | float`, found `Overload[(arg1: SupportsRichComparisonT@min, arg2: SupportsRichComparisonT@min, /, *_args: SupportsRichComparisonT@min, *, key: None = None) -> SupportsRichComparisonT@min, (arg1: _T@min, arg2: _T@min, /, *_args: _T@min, *, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None) -> SupportsRichComparisonT@min, (iterable: Iterable[_T@min], *, /, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None, default: _T@min) -> SupportsRichComparisonT@min | _T@min, (iterable: Iterable[_T1@min], *, /, key: (_T1@min, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@min) -> _T1@min | _T2@min]`
- homeassistant/components/nordpool/sensor.py:207:48: error[invalid-argument-type] Argument to function `get_min_max_price` is incorrect: Expected `(int | float, int | float, /) -> int | float`, found `Overload[(arg1: SupportsRichComparisonT@min, arg2: SupportsRichComparisonT@min, /, *_args: SupportsRichComparisonT@min, *, key: None = None) -> SupportsRichComparisonT@min, (arg1: _T@min, arg2: _T@min, /, *_args: _T@min, *, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None) -> SupportsRichComparisonT@min, (iterable: Iterable[_T@min], *, /, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None, default: _T@min) -> SupportsRichComparisonT@min | _T@min, (iterable: Iterable[_T1@min], *, /, key: (_T1@min, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@min) -> _T1@min | _T2@min]`
- homeassistant/components/nordpool/sensor.py:208:46: error[invalid-argument-type] Argument to function `get_min_max_price` is incorrect: Expected `(int | float, int | float, /) -> int | float`, found `Overload[(arg1: SupportsRichComparisonT@min, arg2: SupportsRichComparisonT@min, /, *_args: SupportsRichComparisonT@min, *, key: None = None) -> SupportsRichComparisonT@min, (arg1: _T@min, arg2: _T@min, /, *_args: _T@min, *, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None) -> SupportsRichComparisonT@min, (iterable: Iterable[_T@min], *, /, key: (_T@min, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@min, (iterable: Iterable[SupportsRichComparisonT@min], *, /, key: None = None, default: _T@min) -> SupportsRichComparisonT@min | _T@min, (iterable: Iterable[_T1@min], *, /, key: (_T1@min, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@min) -> _T1@min | _T2@min]`
- homeassistant/components/nordpool/sensor.py:215:59: error[invalid-argument-type] Argument to function `get_min_max_price` is incorrect: Expected `(int | float, int | float, /) -> int | float`, found `Overload[(arg1: SupportsRichComparisonT@max, arg2: SupportsRichComparisonT@max, /, *_args: SupportsRichComparisonT@max, *, key: None = None) -> SupportsRichComparisonT@max, (arg1: _T@max, arg2: _T@max, /, *_args: _T@max, *, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None) -> SupportsRichComparisonT@max, (iterable: Iterable[_T@max], *, /, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None, default: _T@max) -> SupportsRichComparisonT@max | _T@max, (iterable: Iterable[_T1@max], *, /, key: (_T1@max, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@max) -> _T1@max | _T2@max]`
- homeassistant/components/nordpool/sensor.py:217:48: error[invalid-argument-type] Argument to function `get_min_max_price` is incorrect: Expected `(int | float, int | float, /) -> int | float`, found `Overload[(arg1: SupportsRichComparisonT@max, arg2: SupportsRichComparisonT@max, /, *_args: SupportsRichComparisonT@max, *, key: None = None) -> SupportsRichComparisonT@max, (arg1: _T@max, arg2: _T@max, /, *_args: _T@max, *, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None) -> SupportsRichComparisonT@max, (iterable: Iterable[_T@max], *, /, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None, default: _T@max) -> SupportsRichComparisonT@max | _T@max, (iterable: Iterable[_T1@max], *, /, key: (_T1@max, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@max) -> _T1@max | _T2@max]`
- homeassistant/components/nordpool/sensor.py:218:46: error[invalid-argument-type] Argument to function `get_min_max_price` is incorrect: Expected `(int | float, int | float, /) -> int | float`, found `Overload[(arg1: SupportsRichComparisonT@max, arg2: SupportsRichComparisonT@max, /, *_args: SupportsRichComparisonT@max, *, key: None = None) -> SupportsRichComparisonT@max, (arg1: _T@max, arg2: _T@max, /, *_args: _T@max, *, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None) -> SupportsRichComparisonT@max, (iterable: Iterable[_T@max], *, /, key: (_T@max, /) -> @Todo(Support for `typing.TypeAlias`)) -> _T@max, (iterable: Iterable[SupportsRichComparisonT@max], *, /, key: None = None, default: _T@max) -> SupportsRichComparisonT@max | _T@max, (iterable: Iterable[_T1@max], *, /, key: (_T1@max, /) -> @Todo(Support for `typing.TypeAlias`), default: _T2@max) -> _T1@max | _T2@max]`
- homeassistant/components/ovo_energy/sensor.py:41:5: error[invalid-assignment] Object of type `Overload[(number: _SupportsRound1[_T@round], ndigits: None = None) -> _T@round, (number: _SupportsRound2[_T@round], ndigits: SupportsIndex) -> _T@round]` is not assignable to `(Unknown, /) -> StateType | datetime`
+ homeassistant/config.py:387:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 13463 diagnostics
+ Found 13457 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/matrices/matrixbase.py:574:23: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Self@hstack, Self@hstack, /) -> Self@hstack`, found `def row_join(self, other: Self@row_join) -> Self@row_join`
- sympy/matrices/matrixbase.py:926:23: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Self@vstack, Self@vstack, /) -> Self@vstack`, found `def col_join(self, other: Self@col_join, /) -> Self@col_join`
+ sympy/polys/polyclasses.py:1289:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sympy/polys/polyclasses.py:2347:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 13595 diagnostics
+ Found 13593 diagnostics

CPython (Argument Clinic) (https://github.com/python/cpython)
- Lib/idlelib/tree.py:436:9: error[no-matching-overload] No overload of bound method `sort` matches arguments
- Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_itertools.py:146:31: error[no-matching-overload] No overload of function `__new__` matches arguments
- Lib/test/test_itertools.py:148:31: error[no-matching-overload] No overload of function `__new__` matches arguments
- Lib/tokenize.py:124:22: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `def escape[AnyStr](pattern: AnyStr@escape) -> AnyStr@escape`
- Found 23904 diagnostics
+ Found 23900 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
- Lib/idlelib/tree.py:436:9: error[no-matching-overload] No overload of bound method `sort` matches arguments
- Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_itertools.py:146:31: error[no-matching-overload] No overload of function `__new__` matches arguments
- Lib/test/test_itertools.py:148:31: error[no-matching-overload] No overload of function `__new__` matches arguments
- Lib/tokenize.py:124:22: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `def escape[AnyStr](pattern: AnyStr@escape) -> AnyStr@escape`
- Found 23904 diagnostics
+ Found 23900 diagnostics

CPython (peg_generator) (https://github.com/python/cpython)
- Lib/idlelib/tree.py:436:9: error[no-matching-overload] No overload of bound method `sort` matches arguments
- Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2142:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2144:34: error[invalid-argument-type] Argument to bound method `__lt__` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[Unknown]` of type variable `Self`
+ Lib/test/test_ipaddress.py:2538:66: error[invalid-argument-type] Argument to bound method `address_exclude` is incorrect: Argument type `IPv4Address | IPv6Address` does not satisfy upper bound `_BaseNetwork[_A@_BaseNetwork]` of type variable `Self`
- Lib/test/test_itertools.py:146:31: error[no-matching-overload] No overload of function `__new__` matches arguments
- Lib/test/test_itertools.py:148:31: error[no-matching-overload] No overload of function `__new__` matches arguments
- Lib/tokenize.py:124:22: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `def escape[AnyStr](pattern: AnyStr@escape) -> AnyStr@escape`
- Found 23903 diagnostics
+ Found 23899 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-19 09:02_

---

_Comment by @github-actions[bot] on 2025-09-19 09:07_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 40 | 0 |
| `invalid-return-type` | 0 | 17 | 0 |
| `unused-ignore-comment` | 9 | 1 | 0 |
| `invalid-parameter-default` | 0 | 7 | 0 |
| `invalid-assignment` | 0 | 5 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| **Total** | **10** | **70** | **0** |

**[Full report with detailed diff](https://david-fix-1196.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-19 11:42_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-19 11:42_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-19 12:03_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-19 12:03_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-19 16:21_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-19 16:21_

---

_Comment by @sharkdp on 2025-09-19 17:06_

It's just an endless rabbithole. Now we pick the wrong overload of `datetime.__sub__` when called with a `timedelta` for some reason. Probably because of the `skip_assignability_check` hack — which allows `timedelta` to be incorrectly assigned to `Self@datetime`

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-22 13:38_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-22 13:38_

---

_@sharkdp reviewed on 2025-09-22 13:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/overloads.md`:140 on 2025-09-22 13:45_

This is a new test case adapted from a `datetime`/`timedelta` example observed in the ecosystem.

The lower of these two assertions was failing with the induct-into-bounds-of-typevars approach, because we would pick the first overload and return `Foo3 | Literal[1]`, if assignability to the upper bound of `Self` was not properly checked. And if we properly check the assignability to the upper bound of `Self`, we're getting `false` from the missing `has_relation_to_impl` branch, and can't fix https://github.com/astral-sh/ty/issues/1196. So at that point, we need to bring back all of the subtyping/assignability adjustments. And then the induction into the bounds of `Self` doesn't have any observable effect anymore, so I reverted it.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-22 14:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-22 14:43_

---

_Marked ready for review by @sharkdp on 2025-09-22 14:56_

---

_Review requested from @carljm by @sharkdp on 2025-09-22 14:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-22 14:56_

---

_Review requested from @dcreager by @sharkdp on 2025-09-22 14:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:369 on 2025-09-23 01:21_

```suggestion
## Passing generic functions to generic functions
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:328 on 2025-09-23 01:23_

Why do we need the explicit `Self` annotations in this test?

Is this the right way to write the tests long-term (i.e. once we support implicit-type-of-self properly)? If not, should we have a TODO here to remove these annotations?

---

_@carljm approved on 2025-09-23 01:55_

Probably best for @dcreager to review this, since it's taking some initial steps in the direction of the full constraint accumulation PR. But if you're blocked by landing it, or @dcreager has already taken a look at it in offline consultation, it looks good to me as a step in the right direction.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6734 on 2025-09-23 02:02_

We discussed this synchronously, but can you add a bit more context here for when the `None` case will be invoked? (The `Some` case is because a generic function might reference other typevars from enclosing generic scopes, and we only want to make _that function's_ typevars as inferable. But once we have decided that a particular typevar should be marked inferable, then any typevars mentioned in its default values (and bound/constraints if that approach had turned out to be fruitful) should be unconditionally marked as inferable)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:7692 on 2025-09-23 02:04_

And then a bit of copy/paste from that commentary here where we switch from the `Some` case to the `None` case

---

_@dcreager approved on 2025-09-23 02:06_

We discussed offline a couple of different possibilities, and I'm convinced that this is the right expedient solution to unblock the type-of-self work.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-09-23 06:49_

---

_@sharkdp reviewed on 2025-09-23 11:11_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/isinstance.md`:328 on 2025-09-23 11:11_

> Why do we need the explicit `Self` annotations in this test?

With these explicit `Self` annotations, these all become regression tests for https://github.com/astral-sh/ty/issues/1196. I could have added new tests, but would have had to duplicate the different variance examples from here. Hopefully `self` and `self: Self` will soon be exactly equivalent (?), so yes, we will be able to remove these soon. I added TODO comments.

---

_Merged by @sharkdp on 2025-09-23 12:02_

---

_Closed by @sharkdp on 2025-09-23 12:02_

---

_Branch deleted on 2025-09-23 12:02_

---
