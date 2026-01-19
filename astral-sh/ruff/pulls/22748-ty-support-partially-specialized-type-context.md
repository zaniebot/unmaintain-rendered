```yaml
number: 22748
title: "[ty] Support partially specialized type context"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: ibraheem/bidi-diagnostics
head: ibraheem/partial-tcx
created_at: 2026-01-19T23:14:57Z
updated_at: 2026-01-19T23:28:30Z
url: https://github.com/astral-sh/ruff/pull/22748
synced_at: 2026-01-19T23:36:38Z
```

# [ty] Support partially specialized type context

---

_@ibraheemdev_

## Summary

We currently ignore type context that contains inferable type variables. Instead, we can specialize these to `Unknown`, and ignore `Unknown` annotations when inferring from the type context, while still using the rest of the type.

Part of https://github.com/astral-sh/ty/issues/2521. This PR is stacked on https://github.com/astral-sh/ruff/pull/22643.

---

_Label `ty` added by @ibraheemdev on 2026-01-19 23:14_

---

_Review requested from @carljm by @ibraheemdev on 2026-01-19 23:14_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2026-01-19 23:14_

---

_Review requested from @sharkdp by @ibraheemdev on 2026-01-19 23:14_

---

_Review requested from @dcreager by @ibraheemdev on 2026-01-19 23:14_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 23:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 23:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
+ lib/spack/spack/llnl/util/filesystem.py:1626:51: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(((...) -> Any, str, tuple[type[BaseException], BaseException, TracebackType], /) -> object) | None`, found `Unknown | bool`
+ lib/spack/spack/llnl/util/filesystem.py:1635:33: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(((...) -> Any, str, tuple[type[BaseException], BaseException, TracebackType], /) -> object) | None`, found `Unknown | bool`
- Found 4344 diagnostics
+ Found 4346 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tests/test_tools.py:72:39: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:72:39: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[None | int]`
- asynq/tests/test_tools.py:74:34: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:74:34: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[None | int]`
- asynq/tests/test_tools.py:82:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilterfalse]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:82:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilterfalse]`, found `list[None | int]`
- asynq/tests/test_tools.py:90:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asift]`, found `list[Unknown | None | int]`
+ asynq/tests/test_tools.py:90:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asift]`, found `list[None | int]`
- asynq/tests/test_tools.py:95:45: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | None]`
+ asynq/tests/test_tools.py:95:45: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[None]`
- asynq/tests/test_tools.py:96:44: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:96:44: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[int]`
- asynq/tests/test_tools.py:98:58: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | None | str]`
+ asynq/tests/test_tools.py:98:58: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[None | str]`
- asynq/tests/test_tools.py:103:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | None]`
+ asynq/tests/test_tools.py:103:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[None]`
- asynq/tests/test_tools.py:104:37: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | bool | None]`
+ asynq/tests/test_tools.py:104:37: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[bool | None]`
- asynq/tests/test_tools.py:105:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:105:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[int]`
- asynq/tests/test_tools.py:110:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int | None]`
+ asynq/tests/test_tools.py:110:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[int | None]`
- asynq/tests/test_tools.py:112:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:112:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[int]`
- asynq/tests/test_tools.py:113:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | list[Unknown | int]]`
+ asynq/tests/test_tools.py:113:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[list[Unknown | int]]`
- asynq/tests/test_tools.py:127:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | int | None]`
+ asynq/tests/test_tools.py:127:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[int | None]`
- asynq/tests/test_tools.py:129:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:129:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[int]`
- asynq/tests/test_tools.py:130:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | list[Unknown | int]]`
+ asynq/tests/test_tools.py:130:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[list[Unknown | int]]`
- asynq/tests/test_typing.py:62:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int]`
+ asynq/tests/test_typing.py:62:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[int]`

beartype (https://github.com/beartype/beartype)
+ beartype/_util/path/utilpathcopy.py:243:52: error[invalid-argument-type] Argument to function `copytree` is incorrect: Expected `None | ((str, list[str], /) -> Iterable[str])`, found `Unknown | bool`
+ beartype/_util/path/utilpathcopy.py:243:52: error[invalid-argument-type] Argument to function `copytree` is incorrect: Expected `(str, str, /) -> object`, found `Unknown | bool`
- Found 495 diagnostics
+ Found 497 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/contrib/service_shard_update.py:283:25: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | str`
+ paasta_tools/contrib/service_shard_update.py:283:25: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `str | list[Unknown]`

ignite (https://github.com/pytorch/ignite)
- ignite/handlers/param_scheduler.py:1181:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | ParamGroupScheduler | Unknown]`
+ ignite/handlers/param_scheduler.py:1181:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | ParamGroupScheduler]`
- ignite/handlers/param_scheduler.py:1191:73: error[invalid-argument-type] Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | ParamGroupScheduler | Unknown]`
+ ignite/handlers/param_scheduler.py:1191:73: error[invalid-argument-type] Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | ParamGroupScheduler]`
- ignite/metrics/metric.py:647:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2034 diagnostics
+ Found 2033 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

mypy (https://github.com/python/mypy)
- mypyc/irbuild/util.py:35:44: error[invalid-assignment] Object of type `frozenset[str]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`
+ mypyc/irbuild/util.py:35:44: error[invalid-assignment] Object of type `frozenset[Unknown | str]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`

Expression (https://github.com/cognitedata/Expression)
+ expression/collections/maptree.py:113:25: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance | Unknown, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
+ expression/collections/maptree.py:121:27: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance | Unknown, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
+ expression/collections/maptree.py:136:29: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Unknown | Key@rebalance, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
+ expression/collections/maptree.py:141:61: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Unknown | Key@rebalance, object]]`, found `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`
- Found 202 diagnostics
+ Found 206 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/tests/test_backends.py:3758:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, bool]`
+ xarray/tests/test_backends.py:3758:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`
+ xarray/tests/test_backends.py:3775:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, bool]`
+ xarray/tests/test_backends.py:3775:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MutableMapping[str, bool | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`, found `dict[str, dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int]]]`
+ xarray/tests/test_combine.py:659:13: error[no-matching-overload] No overload of function `combine_nested` matches arguments
- Found 1763 diagnostics
+ Found 1768 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/context/menu.py:57:96: error[invalid-assignment] Object of type `frozenset[CommandType]` is not assignable to `frozenset[Literal[CommandType.USER, CommandType.MESSAGE]]`
+ tanjun/context/menu.py:57:96: error[invalid-assignment] Object of type `frozenset[Unknown | CommandType]` is not assignable to `frozenset[Literal[CommandType.USER, CommandType.MESSAGE]]`

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:2871:22: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ValueError | ExceptionGroup[ValueError]`
- Found 487 diagnostics
+ Found 486 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/sources/helpers/aliyun.py:174:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
+ cloudinit/sources/helpers/aliyun.py:202:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `int`
+ cloudinit/sources/helpers/aliyun.py:205:12: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | dict[Unknown, Unknown]`
+ cloudinit/sources/helpers/aliyun.py:206:25: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | int | dict[Unknown, Unknown]`
+ cloudinit/sources/helpers/aliyun.py:207:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ cloudinit/sources/helpers/aliyun.py:208:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ cloudinit/sources/helpers/aliyun.py:209:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ cloudinit/sources/helpers/aliyun.py:210:13: error[not-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/unittests/sources/test_maas.py:146:16: warning[possibly-missing-attribute] Attribute `decode` may be missing on object of type `Unknown | str | bytes`
- Found 1169 diagnostics
+ Found 1178 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/interpreter/compiler.py:161:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/interpreter/type_checking.py:278:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[list[BuildTarget | CustomTarget | CustomTargetIndex] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/interpreter/type_checking.py:313:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | list[str] | dict[str, str | int | list[str]] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/interpreter/type_checking.py:540:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | None | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | str, str | tuple[str, str]]`
+ mesonbuild/interpreter/type_checking.py:553:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | File | CustomTarget | ... omitted 4 union elements, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/interpreter/type_checking.py:652:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[list[Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/interpreter/type_checking.py:857:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[bool | str | None | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/modules/gnome.py:1129:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | bool | @Todo | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | bool, str | tuple[str, str]]`
+ mesonbuild/modules/gnome.py:1133:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | bool | @Todo | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | bool, str | tuple[str, str]]`
+ mesonbuild/optinterpreter.py:183:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[bool | str | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/optinterpreter.py:224:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[bool | str | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ mesonbuild/optinterpreter.py:249:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[SupportsInt | str | Buffer | ... omitted 4 union elements, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ unittests/internaltests.py:1451:88: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[list[Unknown] | str | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | str, str | tuple[str, str]]`
+ unittests/internaltests.py:1451:122: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[list[Unknown] | str | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | str, str | tuple[str, str]]`
+ unittests/internaltests.py:1452:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[dict[Unknown, Unknown] | str | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | str, str | tuple[str, str]]`
+ unittests/internaltests.py:1452:143: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[dict[Unknown, Unknown] | str | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | str, str | tuple[str, str]]`
+ unittests/internaltests.py:1453:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[bool | str | @Todo | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | bool, str | tuple[str, str]]`
+ unittests/internaltests.py:1458:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | None | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | str, str | tuple[str, str]]`
+ unittests/internaltests.py:1459:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | None | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type | str, str | tuple[str, str]]`
+ unittests/internaltests.py:1461:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[dict[Unknown, Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ unittests/internaltests.py:1463:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[dict[Unknown, Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ unittests/internaltests.py:1465:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | int | dict[Unknown, Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ unittests/internaltests.py:1466:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str | int | dict[Unknown, Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
+ unittests/internaltests.py:1468:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[list[Unknown] | ContainerTypeInfo | type, str | tuple[str, str]] | None`, found `dict[ContainerTypeInfo | type, str | tuple[str, str]]`
- Found 2156 diagnostics
+ Found 2180 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/book_providers.py:829:9: error[invalid-argument-type] Argument to function `multisort_best` is incorrect: Expected `list[tuple[Literal["min", "max"], (tuple[Edition, AbstractBookProvider[Unknown] | None] | Unknown, /) -> int | float]]`, found `list[Unknown | tuple[str, (rec) -> Unknown]]`
- openlibrary/catalog/add_book/__init__.py:1096:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- openlibrary/catalog/add_book/__init__.py:1099:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1146 diagnostics
+ Found 1143 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
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
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/datasets/_openml.py:569:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `str | dict[Unknown, Unknown] | list[str] | tuple[int, int] | None`
+ sklearn/tests/test_metaestimators_metadata_routing.py:493:26: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | <class 'MultiOutputRegressor'> | str | ... omitted 48 union elements`
+ sklearn/utils/_param_validation.py:664:38: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[_InstancesOf | Interval | _NanConstraint | _PandasNAConstraint]`, found `list[_InstancesOf | _NoneConstraint]`
- Found 2423 diagnostics
+ Found 2426 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/telemetry/writer.py:80:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["dd-api-key"]` and value of type `(Unknown & ~AlwaysFalsy) | (EnvVariable[None] & ~AlwaysFalsy)` on object of type `dict[str, str]`
- tests/contrib/langgraph/conftest.py:331:16: error[invalid-return-type] Return type does not match returned value: expected `State`, found `dict[Unknown | str, Unknown | list[Unknown]]`
+ tests/contrib/langgraph/conftest.py:331:16: error[invalid-return-type] Return type does not match returned value: expected `State`, found `dict[Unknown | str, Unknown | list[Unknown | int]]`
- Found 8413 diagnostics
+ Found 8414 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/models/callbacks.py:212:21: error[invalid-assignment] Object of type `Required[Unknown]` is not assignable to `HasProps`
+ src/bokeh/models/callbacks.py:212:21: error[invalid-assignment] Object of type `Required[HasProps]` is not assignable to `HasProps`

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/bigquery/tests/unit/test_compiler.py:273:26: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/bigquery/tests/unit/test_compiler.py:273:26: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, str]`
- ibis/backends/databricks/tests/test_datatypes.py:19:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/databricks/tests/test_datatypes.py:19:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | str | Struct]`
- ibis/backends/flink/tests/test_ddl.py:262:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:262:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown]`
- ibis/backends/flink/tests/test_ddl.py:456:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:456:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | String | Int64 | Float64]`
- ibis/backends/flink/tests/test_ddl.py:484:34: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:484:34: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | String | Int64]`
- ibis/backends/flink/tests/test_ddl.py:486:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/tests/test_ddl.py:486:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | String | Int64 | Float64]`
- ibis/backends/oracle/tests/test_datatypes.py:29:45: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/oracle/tests/test_datatypes.py:29:45: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/backends/oracle/tests/test_datatypes.py:53:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/oracle/tests/test_datatypes.py:53:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/backends/tests/test_join.py:207:57: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/tests/test_join.py:207:57: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/expr/tests/test_api.py:20:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_api.py:20:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str, str]`
- ibis/expr/tests/test_format.py:366:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_format.py:366:24: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown | str, Unknown]`
- ibis/expr/tests/test_newrels.py:66:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:66:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Boolean | Int64 | Float64 | String]`
- ibis/expr/tests/test_newrels.py:86:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:86:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64]`
- ibis/expr/tests/test_newrels.py:91:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:91:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64]`
- ibis/expr/tests/test_newrels.py:96:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:96:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64]`
- ibis/expr/tests/test_newrels.py:107:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:107:39: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64]`
- ibis/expr/tests/test_newrels.py:116:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_newrels.py:116:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int8 | Float64 | Int32]`
- ibis/expr/tests/test_schema.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | String | Float64 | Boolean]`
- ibis/expr/tests/test_schema.py:172:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:172:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | str]`
- ibis/expr/tests/test_schema.py:224:5: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:224:5: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | str]`
- ibis/expr/tests/test_schema.py:317:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:317:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | Array[Unknown]]`
- ibis/expr/tests/test_schema.py:325:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:325:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | String | Int64 | Float64]`
- ibis/expr/tests/test_schema.py:326:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:326:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | String | Float64 | Boolean | Date]`
- ibis/expr/tests/test_schema.py:327:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:327:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | Float64 | String]`
- ibis/expr/tests/test_schema.py:328:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:328:20: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | Float64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:330:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:330:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | String | Float64]`
- ibis/expr/tests/test_schema.py:332:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:332:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | String | Int64 | ... omitted 3 union elements]`
- ibis/expr/tests/test_schema.py:334:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:334:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64]`
- ibis/expr/tests/test_schema.py:335:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:335:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Boolean | Date]`
- ibis/expr/tests/test_schema.py:336:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:336:32: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | Boolean | Date]`
- ibis/expr/tests/test_schema.py:365:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:365:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:376:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:376:30: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:393:38: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:393:38: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | String | Boolean]`
- ibis/expr/tests/test_schema.py:419:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:419:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | String | Timestamp]`
- ibis/expr/tests/test_schema.py:581:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/expr/tests/test_schema.py:581:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | Int32 | ... omitted 14 union elements]`
- ibis/expr/types/generic.py:1553:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown]`
+ ibis/expr/types/generic.py:1553:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown | Value[Unknown, Any]]`
- ibis/expr/types/generic.py:1754:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown]`
+ ibis/expr/types/generic.py:1754:35: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, Value[Unknown, Any]]`, found `dict[Unknown, Unknown | Value[Unknown, Any]]`
- ibis/formats/pandas.py:158:55: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/pandas.py:158:55: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown]`
- ibis/formats/polars.py:156:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/polars.py:156:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/formats/polars.py:162:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/polars.py:162:43: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/formats/tests/test_pandas.py:120:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:120:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | String | Boolean | Float64]`
- ibis/formats/tests/test_pandas.py:147:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:147:9: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | Int64 | String | Boolean | Float64]`
- ibis/formats/tests/test_pandas.py:424:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:424:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown]`
- ibis/formats/tests/test_pandas.py:440:25: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/formats/tests/test_pandas.py:440:25: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | str]`
- ibis/tests/expr/test_table.py:1087:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/tests/expr/test_table.py:1087:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | str]`
- ibis/tests/expr/test_table.py:1150:29: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/tests/expr/test_table.py:1150:29: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown | str, Unknown | str]`

materialize (https://github.com/MaterializeInc/materialize)
+ test/terraform/mzcompose.py:452:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- Found 513 diagnostics
+ Found 514 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bottom[Bus[Any]] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bottom[Bus[Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc, Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Bottom[Yarn[Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1822 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/core/tests/test_subs.py:558:12: error[no-matching-overload] No overload of bound method `subs` matches arguments
- Found 15627 diagnostics
+ Found 15628 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_frame.py:3552:38: error[missing-typed-dict-key] Missing required key 'formats' in TypedDict `_DTypeDict` constructor
+ tests/frame/test_frame.py:3552:38: error[missing-typed-dict-key] Missing required key 'names' in TypedDict `_DTypeDict` constructor
+ tests/frame/test_frame.py:3552:39: error[invalid-key] Unknown key "col1" for TypedDict `_DTypeDict`: Unknown key "col1"
+ tests/frame/test_frame.py:3552:56: error[invalid-key] Unknown key "col2" for TypedDict `_DTypeDict`: Unknown key "col2"
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/indexes/test_indexes.py:1592:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `Unknown`
+ tests/indexes/test_indexes.py:1592:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `DatetimeIndex`
- tests/test_styler.py:170:17: error[invalid-argument-type] Argument to bound method `set_table_styles` is incorrect: Expected `dict[Unknown, list[CSSDict]] | list[CSSDict] | None`, found `list[Unknown | dict[Unknown | str, Unknown | str | list[Unknown | tuple[str, str]]]]`
- Found 4451 diagnostics
+ Found 4452 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/constants/location_details.py:99:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ rotkehlchen/constants/location_details.py:101:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ rotkehlchen/constants/location_details.py:103:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
+ rotkehlchen/tests/unit/globaldb/test_gl

... (truncated 56 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-19 23:22_

---

_Comment by @codspeed-hq[bot] on 2026-01-19 23:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fpartial-tcx?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 6.59%**

<sub>Comparing <code>ibraheem/partial-tcx</code> (1f98abe) with <code>ibraheem/bidi-diagnostics</code> (d6a2820)</sub>



### Summary

`❌ 1` regressed benchmark  
`✅ 16` untouched benchmarks  
`⏩ 36` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fpartial-tcx?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fpartial-tcx?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.9 s | 8.5 s | -6.59% |

[^skipped]: 36 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fpartial-tcx?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---
