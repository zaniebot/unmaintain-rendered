```yaml
number: 21401
title: "[ty] Ensure annotation/type expressions in stub files are always deferred"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/tvar-bound-stubs2
created_at: 2025-11-12T12:41:54Z
updated_at: 2025-11-13T17:37:58Z
url: https://github.com/astral-sh/ruff/pull/21401
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Ensure annotation/type expressions in stub files are always deferred

---

_@AlexWaygood_

## Summary

Inference of type expressions should always be deferred when analysing a stub file: stubs are allowed to (and often do) do things like this:

```py
from typing import TypeVar

T = TypeVar("T", bound=Foo)

class Foo: ...
```

`bound=Foo` here would cause a `NameError` if you tried to run this snippet with an interpreter at runtime, since `Foo` is not yet defined at the point when the `TypeVar` is assigned. But it's nonetheless permitted in a stub file, because stub files are never intended to be executed at runtime. They're just "data files" for the type checker.

This pattern is used in quite a few typeshed stub files. Among other things, the fact that we don't yet support it means that we aren't able to correctly infer the type of the `default=` argument for this `TypeVar` (`int` has not yet been defined at that point in `builtins.pyi`):

https://github.com/astral-sh/ruff/blob/9dd666d67758c9d25a361c7c23d7f8b883b0b05a/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L93

And therefore we think that a bare `memoryview` annotation should signify `memoryview[Unknown]`, whereas in fact it should signify `memoryview[int]`.

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-11-12 12:41_

---

_Renamed from "[ty] Ensure annotation expressions in stub files are always deferred" to "[ty] Ensure annotation/type expressions in stub files are always deferred" by @AlexWaygood on 2025-11-12 12:43_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 12:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-13 13:18:20.973834959 +0000
+++ new-output.txt	2025-11-13 13:18:24.521875587 +0000
@@ -39,7 +39,7 @@
 aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
 aliases_newtype.py:12:1: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
 aliases_newtype.py:18:1: error[invalid-assignment] Object of type `<NewType pseudo-class 'UserId'>` is not assignable to `type`
-aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `<NewType pseudo-class 'UserId'>`
+aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `<NewType pseudo-class 'UserId'>`
 aliases_newtype.py:26:21: error[invalid-base] Cannot subclass an instance of NewType
 aliases_newtype.py:35:1: error[invalid-newtype] The name of a `NewType` (`BadName`) must match the name of the variable it is assigned to (`GoodName`)
 aliases_newtype.py:41:6: error[invalid-type-form] `GoodNewType1` is a `NewType` and cannot be specialized
@@ -54,7 +54,7 @@
 aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
 aliases_type_statement.py:23:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
 aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `typing.TypeAliasType`
-aliases_type_statement.py:31:22: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.TypeAliasType`
+aliases_type_statement.py:31:22: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `typing.TypeAliasType`
 aliases_type_statement.py:37:22: error[invalid-type-form] Function calls are not allowed in type expressions
 aliases_type_statement.py:38:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_type_statement.py:39:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-12 12:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3430:13: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `Sequence[bytes | str | memoryview[Unknown]] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3430:13: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `Sequence[bytes | str | memoryview[int]] | AbstractSet[AnyKeyT@_zaggregate]`
- aioredis/client.py:3898:38: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | bytes | memoryview[Unknown] | ... omitted 3 union elements`
+ aioredis/client.py:3898:38: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | bytes | memoryview[int] | ... omitted 3 union elements`
- aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[Unknown] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[Unknown] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
+ aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
- aioredis/connection.py:933:14: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `int`, in comparing `Literal[b" "]` with `bytes | memoryview[Unknown] | int`
+ aioredis/connection.py:933:14: error[unsupported-operator] Operator `in` is not supported for types `bytes` and `int`, in comparing `Literal[b" "]` with `bytes | memoryview[int] | int`
- aioredis/connection.py:934:26: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `bytes | memoryview[Unknown] | int`
+ aioredis/connection.py:934:26: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `bytes | memoryview[int] | int`

asynq (https://github.com/quora/asynq)
- asynq/batching.pyi:6:26: error[unresolved-reference] Name `BatchItemBase` used when not defined
- asynq/contexts.pyi:5:26: error[unresolved-reference] Name `AsyncContext` used when not defined
- Found 172 diagnostics
+ Found 170 diagnostics

yarl (https://github.com/aio-libs/yarl)
+ tests/test_update_query.py:327:24: error[invalid-argument-type] Argument to bound method `with_query` is incorrect: Expected `None | str | Mapping[str, Sequence[str | SupportsInt] | SupportsInt] | Sequence[tuple[str, Sequence[str | SupportsInt] | SupportsInt]]`, found `memoryview[int]`
+ tests/test_url_query.py:231:26: error[invalid-argument-type] Argument to bound method `update_query` is incorrect: Expected `None | str | Mapping[str, Sequence[str | SupportsInt] | SupportsInt] | Sequence[tuple[str, Sequence[str | SupportsInt] | SupportsInt]]`, found `memoryview[int]`
- Found 36 diagnostics
+ Found 38 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_linkextractors.py:642:63: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_linkextractors.py:642:63: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:100:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:100:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:108:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:108:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:117:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:117:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:126:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:126:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:135:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:135:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:144:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:144:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:155:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:155:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`
- tests/test_loader.py:164:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:164:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `type | None`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/pyutils/is_iterable.py:30:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `list[Unknown | <class 'Collection'>] | Unknown | <class 'Collection'> | tuple[Unknown | <class 'Collection'>, ...]`
+ src/graphql/pyutils/is_iterable.py:30:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `list[Unknown | <class 'Collection'>] | Unknown | <class 'Collection'> | tuple[Unknown | <class 'Collection'>, ...]`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/pack.py:3075:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `(((bytes, /) -> None) & ~(() -> object)) | (((bytes | bytearray | memoryview[Unknown], /) -> int) & ~(() -> object)) | (IO[bytes] & ~(() -> object))`
+ dulwich/pack.py:3075:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `(((bytes, /) -> None) & ~(() -> object)) | (((bytes | bytearray | memoryview[int], /) -> int) & ~(() -> object)) | (IO[bytes] & ~(() -> object))`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/util/ssltransport.py:108:31: error[invalid-argument-type] Argument to bound method `send` is incorrect: Expected `bytes`, found `memoryview[Unknown]`
+ src/urllib3/util/ssltransport.py:108:31: error[invalid-argument-type] Argument to bound method `send` is incorrect: Expected `bytes`, found `memoryview[int]`

vision (https://github.com/pytorch/vision)
- references/classification/utils.py:449:73: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `list[type] | None`
+ references/classification/utils.py:449:73: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `list[type] | None`
- references/depth/stereo/transforms.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]]`, found `tuple[tuple[Unknown, Unknown], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
+ references/depth/stereo/transforms.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None], tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None]]`, found `tuple[tuple[Unknown, Unknown], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
- references/depth/stereo/transforms.py:477:25: warning[possibly-missing-attribute] Attribute `unsqueeze` may be missing on object of type `(Unknown & ~None) | ndarray[@Todo, Unknown]`
+ references/depth/stereo/transforms.py:477:25: warning[possibly-missing-attribute] Attribute `unsqueeze` may be missing on object of type `(Unknown & ~None) | ndarray[@Todo, dtype[Any]]`
- references/depth/stereo/transforms.py:485:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
+ references/depth/stereo/transforms.py:485:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None], tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
- references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[None | Unknown, ...] | tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]` is not assignable to `tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]`
+ references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[None | Unknown, ...] | tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None]` is not assignable to `tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None]`
- references/depth/stereo/transforms.py:601:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None], tuple[Unknown | ndarray[@Todo, Unknown] | None, Unknown | ndarray[@Todo, Unknown] | None]]`, found `tuple[tuple[Unknown, Unknown], tuple[None | Unknown, None | Unknown], tuple[None | Unknown, ...]]`
+ references/depth/stereo/transforms.py:601:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None], tuple[Unknown | ndarray[@Todo, dtype[Any]] | None, Unknown | ndarray[@Todo, dtype[Any]] | None]]`, found `tuple[tuple[Unknown, Unknown], tuple[None | Unknown, None | Unknown], tuple[None | Unknown, ...]]`
- test/datasets_utils.py:591:56: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `Unknown | None`
+ test/datasets_utils.py:591:56: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `Unknown | None`
- test/datasets_utils.py:861:21: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `ndarray[@Todo, Unknown] | None`
+ test/datasets_utils.py:861:21: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `ndarray[@Todo, dtype[Any]] | None`
- torchvision/transforms/functional.py:145:64: warning[possibly-missing-attribute] Attribute `ndim` may be missing on object of type `Image | ndarray[@Todo, Unknown]`
+ torchvision/transforms/functional.py:145:64: warning[possibly-missing-attribute] Attribute `ndim` may be missing on object of type `Image | ndarray[@Todo, dtype[Any]]`

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_model.py:1687:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, Unknown] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[Unknown | str, Unknown | str | float]]`
+ tests/pandas/test_model.py:1687:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, dtype[Any]] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[Unknown | str, Unknown | str | float]]`
- tests/pandas/test_model.py:1731:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, Unknown] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[Unknown | str, Unknown | str | float]]`
+ tests/pandas/test_model.py:1731:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, dtype[Any]] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[Unknown | str, Unknown | str | float]]`
- tests/pandas/test_model.py:1758:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, Unknown] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[Unknown | str, Unknown | str | float]]`
+ tests/pandas/test_model.py:1758:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, dtype[Any]] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[Unknown | str, Unknown | str | float]]`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/io/tnetstring.py:191:20: error[invalid-argument-type] Argument to class `str` is incorrect: Expected `bytes | bytearray`, found `memoryview[Unknown]`
+ mitmproxy/io/tnetstring.py:191:20: error[invalid-argument-type] Argument to class `str` is incorrect: Expected `bytes | bytearray`, found `memoryview[int]`

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/pq/_pq_ctypes.pyi:13:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- psycopg/psycopg/pq/_pq_ctypes.pyi:111:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- psycopg/psycopg/pq/_pq_ctypes.pyi:119:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- psycopg/psycopg/pq/_pq_ctypes.pyi:123:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 656 diagnostics
+ Found 652 diagnostics

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/_ctypes.pyi:12:28: error[unresolved-reference] Name `_CData` used when not defined
- mypy/typeshed/stdlib/argparse.pyi:30:38: error[unresolved-reference] Name `Action` used when not defined
- mypy/typeshed/stdlib/argparse.pyi:31:54: error[unresolved-reference] Name `ArgumentParser` used when not defined
- mypy/typeshed/stdlib/contextlib.pyi:42:34: error[unresolved-reference] Name `AbstractContextManager` used when not defined
- mypy/typeshed/stdlib/ctypes/__init__.pyi:43:32: error[unresolved-reference] Name `CDLL` used when not defined
+ mypy/typeshed/stdlib/ctypes/wintypes.pyi:124:29: error[invalid-argument-type] Argument to class `_CField` is incorrect: Expected `_CData`, found `typing.TypeVar`
- mypy/typeshed/stdlib/distutils/cmd.pyi:29:40: error[unresolved-reference] Name `Command` used when not defined
- mypy/typeshed/stdlib/enum.pyi:42:53: error[unresolved-reference] Name `Enum` used when not defined
- mypy/typeshed/stdlib/http/cookies.pyi:8:49: error[unresolved-reference] Name `Morsel` used when not defined
- mypy/typeshed/stdlib/inspect.pyi:469:59: error[unresolved-reference] Name `_ClassTreeItem` used when not defined
- mypy/typeshed/stdlib/ipaddress.pyi:10:20: error[unresolved-reference] Name `IPv4Address` used when not defined
- mypy/typeshed/stdlib/ipaddress.pyi:10:33: error[unresolved-reference] Name `IPv6Address` used when not defined
- mypy/typeshed/stdlib/ipaddress.pyi:11:20: error[unresolved-reference] Name `IPv4Network` used when not defined
- mypy/typeshed/stdlib/ipaddress.pyi:11:33: error[unresolved-reference] Name `IPv6Network` used when not defined
- mypy/typeshed/stdlib/logging/__init__.pyi:360:35: error[unresolved-reference] Name `LoggerAdapter` used when not defined
- mypy/typeshed/stdlib/marshal.pyi:22:13: error[unresolved-reference] Name `_Marshallable` used when not defined
- mypy/typeshed/stdlib/marshal.pyi:26:17: error[unresolved-reference] Name `_Marshallable` used when not defined
- mypy/typeshed/stdlib/pathlib/__init__.pyi:21:34: error[unresolved-reference] Name `PurePath` used when not defined
- mypy/typeshed/stdlib/sqlite3/__init__.pyi:217:38: error[unresolved-reference] Name `Cursor` used when not defined
- mypy/typeshed/stdlib/sre_parse.pyi:28:60: error[unresolved-reference] Name `SubPattern` used when not defined
- mypy/typeshed/stdlib/sre_parse.pyi:29:47: error[unresolved-reference] Name `SubPattern` used when not defined
- mypy/typeshed/stdlib/sre_parse.pyi:29:59: error[unresolved-reference] Name `SubPattern` used when not defined
- mypy/typeshed/stdlib/sre_parse.pyi:31:45: error[unresolved-reference] Name `SubPattern` used when not defined
- mypy/typeshed/stdlib/sre_parse.pyi:32:59: error[unresolved-reference] Name `SubPattern` used when not defined
- mypy/typeshed/stdlib/tkinter/__init__.pyi:278:26: error[unresolved-reference] Name `Misc` used when not defined
- mypy/typeshed/stdlib/tkinter/__init__.pyi:280:48: error[unresolved-reference] Name `Misc` used when not defined
- mypy/typeshed/stdlib/tkinter/__init__.pyi:280:62: error[unresolved-reference] Name `Misc` used when not defined
- mypy/typeshed/stdlib/unittest/_log.pyi:7:26: error[unresolved-reference] Name `_LoggingWatcher` used when not defined
- mypy/typeshed/stdlib/warnings.pyi:24:37: error[unresolved-reference] Name `WarningMessage` used when not defined
- mypy/typeshed/stdlib/warnings.pyi:24:74: error[unresolved-reference] Name `WarningMessage` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:25:26: error[unresolved-reference] Name `Node` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:27:96: error[unresolved-reference] Name `DocumentFragment` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:31:11: error[unresolved-reference] Name `DocumentFragment` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:32:7: error[unresolved-reference] Name `Attr` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:33:7: error[unresolved-reference] Name `Element` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:34:7: error[unresolved-reference] Name `ProcessingInstruction` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:35:7: error[unresolved-reference] Name `CharacterData` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:36:7: error[unresolved-reference] Name `Text` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:37:7: error[unresolved-reference] Name `Comment` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:38:7: error[unresolved-reference] Name `CDATASection` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:39:7: error[unresolved-reference] Name `Entity` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:40:7: error[unresolved-reference] Name `Notation` used when not defined
- mypy/typeshed/stdlib/xml/dom/minidom.pyi:155:96: error[unresolved-reference] Name `DocumentFragment` used when not defined
- mypy/typeshed/stdlib/xmlrpc/client.pyi:25:13: error[unresolved-reference] Name `_Marshallable` used when not defined
- mypy/typeshed/stdlib/xmlrpc/client.pyi:30:7: error[unresolved-reference] Name `DateTime` used when not defined
- mypy/typeshed/stdlib/xmlrpc/client.pyi:31:7: error[unresolved-reference] Name `Binary` used when not defined
- Found 1704 diagnostics
+ Found 1660 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/_gp/optim_mixed.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, Unknown], int | float, bool]`, found `tuple[@Todo, ndarray[Unknown, Unknown], Literal[True]]`
+ optuna/_gp/optim_mixed.py:116:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float, bool]`, found `tuple[@Todo, ndarray[Unknown, dtype[Any]], Literal[True]]`
- optuna/_gp/optim_mixed.py:137:43: error[invalid-argument-type] Argument to function `find_nearest_index` is incorrect: Expected `int | float`, found `ndarray[Unknown, Unknown]`
+ optuna/_gp/optim_mixed.py:137:43: error[invalid-argument-type] Argument to function `find_nearest_index` is incorrect: Expected `int | float`, found `ndarray[Unknown, dtype[Any]]`
- optuna/_gp/optim_mixed.py:329:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, Unknown], int | float]`, found `tuple[ndarray[Unknown, Unknown], ndarray[Unknown, Unknown]]`
- optuna/_gp/optim_sample.py:19:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, Unknown], int | float]`, found `tuple[Unknown | ndarray[Unknown, Unknown], ndarray[Unknown, Unknown]]`
+ optuna/_gp/optim_mixed.py:329:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float]`, found `tuple[ndarray[Unknown, dtype[Any]], ndarray[Unknown, dtype[Any]]]`
+ optuna/_gp/optim_sample.py:19:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float]`, found `tuple[Unknown | ndarray[Unknown, dtype[Any]], ndarray[Unknown, dtype[Any]]]`
- optuna/_gp/search_space.py:193:56: error[invalid-argument-type] Argument to bound method `to_external_repr` is incorrect: Expected `int | float`, found `ndarray[Unknown, Unknown]`
+ optuna/_gp/search_space.py:193:56: error[invalid-argument-type] Argument to bound method `to_external_repr` is incorrect: Expected `int | float`, found `ndarray[Unknown, dtype[Any]]`
- optuna/importance/_ped_anova/scott_parzen_estimator.py:111:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, ndarray[Unknown, Unknown]]`, found `tuple[@Todo, signedinteger[Unknown]]`
+ optuna/importance/_ped_anova/scott_parzen_estimator.py:111:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, ndarray[Unknown, dtype[Any]]]`, found `tuple[@Todo, signedinteger[Unknown]]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/datasets/_openml.py:71:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `Exception`
+ sklearn/datasets/_openml.py:71:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `Exception`

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/experimental/pydantic/conversion.py:39:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `object`
+ strawberry/experimental/pydantic/conversion.py:39:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `object`

xarray (https://github.com/pydata/xarray)
- xarray/backends/zarr.py:857:23: warning[possibly-missing-attribute] Attribute `chunks` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:857:23: warning[possibly-missing-attribute] Attribute `chunks` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:858:54: warning[possibly-missing-attribute] Attribute `chunks` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:858:54: warning[possibly-missing-attribute] Attribute `chunks` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:864:36: warning[possibly-missing-attribute] Attribute `compressors` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:864:36: warning[possibly-missing-attribute] Attribute `compressors` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:865:32: warning[possibly-missing-attribute] Attribute `filters` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:865:32: warning[possibly-missing-attribute] Attribute `filters` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:866:31: warning[possibly-missing-attribute] Attribute `shards` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:866:31: warning[possibly-missing-attribute] Attribute `shards` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:870:48: warning[possibly-missing-attribute] Attribute `serializer` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:870:48: warning[possibly-missing-attribute] Attribute `serializer` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:874:35: warning[possibly-missing-attribute] Attribute `compressor` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:874:35: warning[possibly-missing-attribute] Attribute `compressor` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:875:32: warning[possibly-missing-attribute] Attribute `filters` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:875:32: warning[possibly-missing-attribute] Attribute `filters` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:882:16: warning[possibly-missing-attribute] Attribute `fill_value` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:882:16: warning[possibly-missing-attribute] Attribute `fill_value` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/backends/zarr.py:883:44: warning[possibly-missing-attribute] Attribute `fill_value` may be missing on object of type `Unknown | ndarray[@Todo, Unknown]`
+ xarray/backends/zarr.py:883:44: warning[possibly-missing-attribute] Attribute `fill_value` may be missing on object of type `Unknown | ndarray[@Todo, dtype[Any]]`
- xarray/coding/times.py:1169:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[T_DuckArray@_eagerly_encode_cf_datetime, str, str]`, found `tuple[Unknown | ndarray[@Todo, Unknown], str, str]`
+ xarray/coding/times.py:1169:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[T_DuckArray@_eagerly_encode_cf_datetime, str, str]`, found `tuple[Unknown | ndarray[@Todo, dtype[Any]], str, str]`
- xarray/compat/npcompat.py:73:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `set[Unknown | <class 'bool'> | <class 'signedinteger'> | ... omitted 5 union elements]`
+ xarray/compat/npcompat.py:73:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `set[Unknown | <class 'bool'> | <class 'signedinteger'> | ... omitted 5 union elements]`
- xarray/core/dataarray.py:158:29: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]] | Mapping[Unknown, Unknown]`
+ xarray/core/dataarray.py:158:29: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, dtype[Any]]] | Mapping[Unknown, Unknown]`
- xarray/core/dataarray.py:182:25: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]] & ~Coordinates) | (Mapping[Unknown, Unknown] & ~Coordinates) | None`
+ xarray/core/dataarray.py:182:25: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `(Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, dtype[Any]]] & ~Coordinates) | (Mapping[Unknown, Unknown] & ~Coordinates) | None`
- xarray/core/dataarray.py:215:37: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown], (index: slice[Any, Any, Any]) -> Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]]]` cannot be called with key of type `Hashable` on object of type `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]]`
+ xarray/core/dataarray.py:215:37: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: int) -> Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, dtype[Any]], (index: slice[Any, Any, Any]) -> Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, dtype[Any]]]]` cannot be called with key of type `Hashable` on object of type `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, dtype[Any]]]`
- xarray/core/dataarray.py:216:33: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]] | Mapping[Unknown, Unknown]`
+ xarray/core/dataarray.py:216:33: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, dtype[Any]]] | Mapping[Unknown, Unknown]`
- xarray/groupers.py:640:12: error[invalid-return-type] Return type does not match returned value: expected `ndarray[@Todo, Unknown]`, found `signedinteger[@Todo] | @Todo`
+ xarray/groupers.py:640:12: error[invalid-return-type] Return type does not match returned value: expected `ndarray[@Todo, dtype[Any]]`, found `signedinteger[@Todo] | @Todo`
- xarray/namedarray/_array_api.py:82:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/namedarray/_array_api.py:115:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/namedarray/_typing.py:57:5: error[invalid-argument-type] Argument to class `dtype` is incorrect: Expected `generic[Any]`, found `typing.TypeVar`
+ xarray/namedarray/_typing.py:59:20: error[invalid-argument-type] Argument to class `dtype` is incorrect: Expected `generic[Any]`, found `typing.TypeVar`
+ xarray/namedarray/_typing.py:217:33: error[invalid-argument-type] Argument to class `dtype` is incorrect: Expected `generic[Any]`, found `typing.TypeVar`
- xarray/namedarray/core.py:556:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/namedarray/core.py:574:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/namedarray/core.py:1162:31: error[invalid-argument-type] Argument to class `dtype` is incorrect: Expected `generic[Any]`, found `typing.TypeVar`
- xarray/tests/test_indexing.py:336:54: error[unresolved-attribute] Object of type `object` has no attribute `kind`
- xarray/tests/test_indexing.py:338:54: error[unresolved-attribute] Object of type `object` has no attribute `kind`
- xarray/tests/test_plot.py:2256:14: error[unresolved-attribute] Object of type `ndarray[@Todo, Unknown]` has no attribute `get_title`
+ xarray/tests/test_plot.py:2256:14: error[unresolved-attribute] Object of type `ndarray[@Todo, dtype[Any]]` has no attribute `get_title`
- xarray/tests/test_plot.py:2290:20: error[unresolved-attribute] Object of type `ndarray[@Todo, Unknown]` has no attribute `has_data`
+ xarray/tests/test_plot.py:2290:20: error[unresolved-attribute] Object of type `ndarray[@Todo, dtype[Any]]` has no attribute `has_data`
- xarray/tests/test_plot.py:2291:20: error[unresolved-attribute] Object of type `ndarray[@Todo, Unknown]` has no attribute `get_visible`
+ xarray/tests/test_plot.py:2291:20: error[unresolved-attribute] Object of type `ndarray[@Todo, dtype[Any]]` has no attribute `get_visible`
- Found 1780 diagnostics
+ Found 1778 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/plugins/simplepush.py:207:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray | memoryview[Unknown]`, found `Unknown | None | bytes`
+ apprise/plugins/simplepush.py:207:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray | memoryview[int]`, found `Unknown | None | bytes`
- tests/test_plugin_email.py:515:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
+ tests/test_plugin_email.py:515:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `(Unknown & ~None) | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
- tests/test_plugin_email.py:612:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `Unknown | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
+ tests/test_plugin_email.py:612:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `Unknown | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
- tests/test_plugin_email.py:624:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
+ tests/test_plugin_email.py:624:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `(Unknown & ~None) | <class 'TypeError'> | <class 'NotifyEmail'> | str | bool`
- tests/test_plugin_growl.py:323:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'NotifyGrowl'> | bool`
+ tests/test_plugin_growl.py:323:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `(Unknown & ~None) | <class 'NotifyGrowl'> | bool`
- tests/test_plugin_growl.py:367:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `Unknown | None | <class 'NotifyGrowl'> | bool`
+ tests/test_plugin_growl.py:367:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `Unknown | None | <class 'NotifyGrowl'> | bool`
- tests/test_plugin_growl.py:376:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `(Unknown & ~None) | <class 'NotifyGrowl'> | bool`
+ tests/test_plugin_growl.py:376:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `(Unknown & ~None) | <class 'NotifyGrowl'> | bool`

setuptools (https://github.com/pypa/setuptools)
- setuptools/__init__.py:191:16: error[invalid-return-type] Return type does not match returned value: expected `Command`, found `str | Command`
- Found 1182 diagnostics
+ Found 1181 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- arviz/data/inference_data.py:731:31: error[call-non-callable] Object of type `ndarray[@Todo, Unknown]` is not callable
+ arviz/data/inference_data.py:731:31: error[call-non-callable] Object of type `ndarray[@Todo, dtype[Any]]` is not callable
- arviz/stats/ecdf_utils.py:27:12: error[invalid-return-type] Return type does not match returned value: expected `ndarray[@Todo, Unknown]`, found `float64`
+ arviz/stats/ecdf_utils.py:27:12: error[invalid-return-type] Return type does not match returned value: expected `ndarray[@Todo, dtype[Any]]`, found `float64`
- arviz/stats/stats.py:1521:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ExtensionArray | ndarray[@Todo, Unknown] | Index | ... omitted 4 union elements`, found `list[str]`
+ arviz/stats/stats.py:1521:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ExtensionArray | ndarray[@Todo, dtype[Any]] | Index | ... omitted 4 union elements`, found `list[str]`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-email/prefect_email/credentials.py:54:24: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `Enum`
+ src/integrations/prefect-email/prefect_email/credentials.py:54:24: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `Enum`

colour (https://github.com/colour-science/colour)
- colour/plotting/tests/test_common.py:200:16: error[unresolved-attribute] Object of type `ndarray[@Todo, Unknown]` has no attribute `subfigures`
+ colour/plotting/tests/test_common.py:200:16: error[unresolved-attribute] Object of type `ndarray[@Todo, dtype[Any]]` has no attribute `subfigures`

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/contrib/admin/options.pyi:147:44: error[unresolved-reference] Name `ModelAdmin` used when not defined
+ django-stubs/contrib/sessions/base_session.pyi:18:23: error[invalid-argument-type] Argument to class `BaseSessionManager` is incorrect: Expected `AbstractBaseSession`, found `typing.Self`
- django-stubs/contrib/sessions/base_session.pyi:8:26: error[unresolved-reference] Name `AbstractBaseSession` used when not defined
- django-stubs/contrib/sessions/models.pyi:5:26: error[unresolved-reference] Name `Session` used when not defined
- django-stubs/db/models/base.pyi:12:32: error[unresolved-reference] Name `Model` used when not defined
- django-stubs/db/models/enums.pyi:20:32: error[unresolved-reference] Name `ChoicesType` used when not defined
- django-stubs/db/models/fields/__init__.pyi:32:26: error[unresolved-reference] Name `Field` used when not defined
- django-stubs/db/models/query.pyi:22:62: error[unresolved-reference] Name `QuerySet` used when not defined
- django-stubs/db/models/query.pyi:22:103: error[unresolved-reference] Name `QuerySet` used when not defined
- django-stubs/template/context.pyi:13:46: error[unresolved-reference] Name `Context` used when not defined
- django-stubs/utils/safestring.pyi:6:28: error[unresolved-reference] Name `SafeData` used when not defined
- django-stubs/utils/tree.pyi:6:33: error[unresolved-reference] Name `Node` used when not defined
- Found 475 diagnostics
+ Found 465 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- win32/test/test_pywintypes.py:86:31: error[invalid-argument-type] Argument to function `IID` is incorrect: Expected `str`, found `memoryview[Unknown]`
+ win32/test/test_pywintypes.py:86:31: error[invalid-argument-type] Argument to function `IID` is incorrect: Expected `str`, found `memoryview[int]`

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/cythons/pointerutils.pyi:5:33: error[unresolved-reference] Name `Double` used when not defined
- hydpy/cythons/pointerutils.pyi:5:41: error[unresolved-reference] Name `PDouble` used when not defined
- Found 662 diagnostics
+ Found 660 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:861:44: error[invalid-argument-type] Argument to class `dtype` is incorrect: Expected `generic[Any]`, found `typing.TypeVar`
+ pandas-stubs/_typing.pyi:865:48: error[invalid-argument-type] Argument to class `dtype` is incorrect: Expected `generic[Any]`, found `typing.TypeVar`
+ pandas-stubs/_typing.pyi:877:53: error[invalid-argument-type] Argument to class `dtype` is incorrect: Expected `generic[Any]`, found `typing.TypeVar`
- Found 6019 diagnostics
+ Found 6022 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/impala/tests/test_udf.py:388:28: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Literal`
+ ibis/backends/impala/tests/test_udf.py:388:28: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `typing.Literal`
- ibis/common/patterns.py:807:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `T_co@CoercedTo`
+ ibis/common/patterns.py:807:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `T_co@CoercedTo`
- ibis/tests/expr/test_table.py:1360:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Union`
+ ibis/tests/expr/test_table.py:1360:36: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Divergent, ...]`, found `typing.Union`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/has_props.pyi:40:51: error[unresolved-reference] Name `HasProps` used when not defined
- Found 655 diagnostics
+ Found 654 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/_libs/tslibs/offsets.pyi:23:46: error[unresolved-reference] Name `BaseOffset` used when not defined
- pandas/core/algorithms.py:1062:32: error[invalid-argument-type] Argument to function `_reconstruct_data` is incorrect: Argument type `Any | tuple[@Todo, ndarray[@Todo, Unknown]]` does not satisfy constraints (`ExtensionArray`, `ndarray[@Todo, Unknown]`) of type variable `ArrayLikeT`
+ pandas/core/algorithms.py:1062:32: error[invalid-argument-type] Argument to function `_reconstruct_data` is incorrect: Argument type `Any | tuple[@Todo, ndarray[@Todo, dtype[Any]]]` does not satisfy constraints (`ExtensionArray`, `ndarray[@Todo, dtype[Any]]`) of type variable `ArrayLikeT`
- pandas/core/arrays/arrow/array.py:1749:22: error[unresolved-attribute] Object of type `ndarray[@Todo, Unknown]` has no attribute `to_numpy`
+ pandas/core/arrays/arrow/array.py:1749:22: error[unresolved-attribute] Object of type `ndarray[@Todo, dtype[Any]]` has no attribute `to_numpy`
- pandas/core/arrays/boolean.py:262:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, Unknown], ndarray[@Todo, Unknown]]`, found `tuple[@Todo, @Todo | None]`
+ pandas/core/arrays/boolean.py:262:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, dtype[Any]], ndarray[@Todo, dtype[Any]]]`, found `tuple[@Todo, @Todo | None]`
- pandas/core/arrays/datetimelike.py:1771:59: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `dtype[timedelta64[timedelta | int | None]]`, found `str`
- pandas/core/arrays/interval.py:401:35: warning[possibly-missing-attribute] Attribute `_ensure_matching_resos` may be missing on object of type `ExtensionArray | ndarray[@Todo, Unknown] | Unknown`
+ pandas/core/arrays/interval.py:401:35: warning[possibly-missing-attribute] Attribute `_ensure_matching_resos` may be missing on object of type `ExtensionArray | ndarray[@Todo, dtype[Any]] | Unknown`
- pandas/core/arrays/period.py:1454:19: warning[possibly-missing-attribute] Attribute `freqstr` may be missing on object of type `Literal["Q"] | Unknown`
+ pandas/core/arrays/period.py:1464:45: error[invalid-argument-type] Argument to function `freq_to_dtype_code` is incorrect: Expected `BaseOffset`, found `None`
- pandas/core/arrays/period.py:1469:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, Unknown], BaseOffset]`, found `tuple[@Todo, Literal["Q"] | Unknown]`
+ pandas/core/arrays/period.py:1469:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, dtype[Any]], BaseOffset]`, found `tuple[@Todo, BaseOffset | Unknown | None]`
- pandas/core/construction.py:625:18: error[unresolved-attribute] Object of type `object` has no attribute `kind`
- pandas/core/dtypes/dtypes.py:1109:24: error[invalid-return-type] Return type does not match returned value: expected `BaseOffset`, found `str | (Unknown & ~None)`
- pandas/core/frame.py:804:16: error[unresolved-attribute] Object of type `object` has no attribute `names`
- pandas/core/groupby/grouper.py:545:31: warning[possibly-missing-attribute] Attribute `categories` may be missing on object of type `(Unknown & ~ndarray[object, object]) | (Index & ~ndarray[object, object])`
+ pandas/core/groupby/grouper.py:545:31: warning[possibly-missing-attribute] Attribute `categories` may be missing on object of type `(Unknown & ~ndarray[object, dtype[object]]) | (Index & ~ndarray[object, dtype[object]])`
- pandas/core/groupby/grouper.py:546:50: error[invalid-argument-type] Argument to function `recode_for_groupby` is incorrect: Expected `Categorical`, found `(Unknown & ~ndarray[object, object]) | (Index & ~ndarray[object, object])`
+ pandas/core/groupby/grouper.py:546:50: error[invalid-argument-type] Argument to function `recode_for_groupby` is incorrect: Expected `Categorical`, found `(Unknown & ~ndarray[object, dtype[object]]) | (Index & ~ndarray[object, dtype[object]])`
- pandas/core/groupby/grouper.py:623:26: warning[possibly-missing-attribute] Attribute `categories` may be missing on object of type `Unknown | Index | ndarray[@Todo, Unknown] | Categorical`
+ pandas/core/groupby/grouper.py:623:26: warning[possibly-missing-attribute] Attribute `categories` may be missing on object of type `Unknown | Index | ndarray[@Todo, dtype[Any]] | Categorical`
- pandas/core/groupby/grouper.py:626:46: warning[possibly-missing-attribute] Attribute `codes` may be missing on object of type `Unknown | Index | ndarray[@Todo, Unknown] | Categorical`
+ pandas/core/groupby/grouper.py:626:46: warning[possibly-missing-attribute] Attribute `codes` may be missing on object of type `Unknown | Index | ndarray[@Todo, dtype[Any]] | Categorical`
- pandas/core/groupby/grouper.py:635:27: warning[possibly-missing-attribute] Attribute `isna` may be missing on object of type `Unknown | Index | ndarray[@Todo, Unknown] | Categorical`
+ pandas/core/groupby/grouper.py:635:27: warning[possibly-missing-attribute] Attribute `isna` may be missing on object of type `Unknown | Index | ndarray[@Todo, dtype[Any]] | Categorical`
- pandas/core/groupby/grouper.py:645:59: warning[possibly-missing-attribute] Attribute `codes` may be missing on object of type `Unknown | Index | ndarray[@Todo, Unknown] | Categorical`
+ pandas/core/groupby/grouper.py:645:59: warning[possibly-missing-attribute] Attribute `codes` may be missing on object of type `Unknown | Index | ndarray[@Todo, dtype[Any]] | Categorical`
- pandas/core/groupby/grouper.py:649:62: warning[possibly-missing-attribute] Attribute `ordered` may be missing on object of type `Unknown | Index | ndarray[@Todo, Unknown] | Categorical`
+ pandas/core/groupby/grouper.py:649:62: warning[possibly-missing-attribute] Attribute `ordered` may be missing on object of type `Unknown | Index | ndarray[@Todo, dtype[Any]] | Categorical`
- pandas/core/groupby/grouper.py:651:21: warning[possibly-missing-attribute] Attribute `codes` may be missing on object of type `Unknown | Index | ndarray[@Todo, Unknown] | Categorical`
+ pandas/core/groupby/grouper.py:651:21: warning[possibly-missing-attribute] Attribute `codes` may be missing on object of type `Unknown | Index | ndarray[@Todo, dtype[Any]] | Categorical`
- pandas/core/indexes/base.py:6186:69: error[invalid-argument-type] Argument to bound method `_reindex_non_unique` is incorrect: Expected `Index`, found `(Unknown & Index) | ndarray[@Todo, Unknown]`
+ panda

... (truncated 679 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~36MB
+     struct metadata = ~38MB


```

</details>




---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-12 12:46_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 12:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `no-matching-overload` | 229 | 0 | 0 |
| `invalid-argument-type` | 25 | 33 | 103 |
| `unresolved-attribute` | 0 | 9 | 59 |
| `unresolved-reference` | 0 | 62 | 0 |
| `possibly-missing-attribute` | 0 | 3 | 40 |
| `unsupported-operator` | 20 | 12 | 11 |
| `invalid-return-type` | 4 | 3 | 20 |
| `non-subscriptable` | 0 | 0 | 8 |
| `unused-ignore-comment` | 0 | 8 | 0 |
| `invalid-assignment` | 0 | 0 | 6 |
| `not-iterable` | 0 | 5 | 0 |
| `index-out-of-bounds` | 0 | 0 | 3 |
| `call-non-callable` | 0 | 0 | 1 |
| **Total** | **278** | **135** | **251** |

**[Full report with detailed diff](https://alex-tvar-bound-stubs2.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-tvar-bound-stubs2.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-11-12 12:54_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftvar-bound-stubs2?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21401 will **degrade performances by 4.55%**

<sub>Comparing <code>alex/tvar-bound-stubs2</code> (5d1bcb0) with <code>main</code> (04ab917)</sub>



### Summary

`❌ 2` regressions  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftvar-bound-stubs2?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` ty_micro[complex_constrained_attributes_1] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftvar-bound-stubs2?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_1%3A%3Aty_micro%5Bcomplex_constrained_attributes_1%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 66.3 ms | 69.3 ms | -4.33% |
| ❌ | Simulation | [`` ty_micro[complex_constrained_attributes_2] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftvar-bound-stubs2?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_2%3A%3Aty_micro%5Bcomplex_constrained_attributes_2%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 66.2 ms | 69.4 ms | -4.55% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftvar-bound-stubs2?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @AlexWaygood on 2025-11-12 17:52_

This new `dedupe` diagnostic is very weird:

```diff
+ dedupe/convenience.py:88:9: error[invalid-assignment] Object of type `property` is not assignable to `int`
```

Here's a minimal repro:

````md

`stub.pyi`:

```pyi
from typing_extensions import TypeVar, Generic, Any

_IntegerT_co = TypeVar("_IntegerT_co", bound=integer)

class integer: ...

class iinfo(Generic[_IntegerT_co]):
    @property
    def max(self) -> int: ...
    def __new__(cls, dtype: _IntegerT_co | Any) -> iinfo[_IntegerT_co]: ...
```

`foo.py`:

```py
# This should be `int`!
reveal_type(iinfo("int").max)  # revealed: property
```
````

Hmmm, okay, this is reproducible on `main`, without even any stubs or stringized annotations: https://play.ty.dev/ecc94b2c-e353-4733-bfd9-bf457251324b

---

_Comment by @AlexWaygood on 2025-11-12 18:17_

All the new `jax` diagnostics, and many of the new `scipy`/`pandas` diagnostics, appear to be due to the same pre-existing bug as the `dedupe` diagnostic.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-13 11:47_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-13 11:47_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-13 12:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-13 12:06_

---

_Comment by @AlexWaygood on 2025-11-13 12:50_

## Ecosystem analysis

Overall this gets rid of a lot of `unresolved-reference` and `unresolved-attribute` errors; it appears to significantly improve our understanding of types from stub files and reduce the number of variables inferred as `Unknown`.

There are lots of new `no-matching-overload` diagnostics on scipy, e.g.

```diff
+ scipy/interpolate/tests/test_bary_rational.py:173:42: error[no-matching-overload] No overload of function `assert_allclose` matches arguments
```

These appear due to a bug in numpy's stubs. That `scipy` call is [here](https://github.com/scipy/scipy/blob/aa4eddcab554bdd41320952c5eb3050989bb9ca0/scipy/interpolate/tests/test_bary_rational.py#L173), and we can see that it passes `atol=TOL` to `assert_allclose`. Adding a `reveal_type` call to that line indicates that we infer the type of `TOL` to be `floating[Any] | float64`. That seems to be accurate:

```
% uvx --with=scipy --with=pytest python3.14
Installed 7 packages in 35ms
Python 3.14.0 (main, Oct 10 2025, 12:54:13) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from scipy.interpolate.tests.test_bary_rational import TOL
>>> type(TOL)
<class 'numpy.float64'>
```

But none of [numpy's overloads for `assert_allclose`](https://github.com/numpy/numpy/blob/5d4dc2ab393e4e4d4cd0a2b77e3fe9f84487e409/numpy/testing/_private/utils.pyi#L325-L348) permit a type of `float64` or `floating` to be passed to the `atol` or `rtol` parameters; all overloads for the function use `float` for those parameters.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-13 13:18_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-13 13:18_

---

_Marked ready for review by @AlexWaygood on 2025-11-13 13:18_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-13 13:18_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-13 13:18_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-13 13:18_

---

_Comment by @AlexWaygood on 2025-11-13 13:40_

I believe the performance regression is mostly due to the fact that we now accurately understand a lot more typeshed types that use forward references. Note that the regression is most significant on microbenchmarks and tomllib, i.e., the smaller codebases in which inferring typeshed types will take up a greater proportion of the overall type-checking time.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:33 on 2025-11-13 17:04_

In general, setting the deferred state without a paired revert back to the previous deferred state is not right; it will cause the new deferred state to leak back up the stack, outside of this type expression inference.

I feel like there might be some refactor called for here between this method and `infer_type_expression_with_state`, but I haven't quite sorted yet what it should be...

---

_@carljm approved on 2025-11-13 17:06_

Thank you!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:33 on 2025-11-13 17:13_

> In general, setting the deferred state without a paired revert back to the previous deferred state is not right; it will cause the new deferred state to leak back up the stack, outside of this type expression inference.

Yes, but it can never leak so far as leak into a different file, correct? And when you're inferring types for one scope in a stub file, you know that all other scopes in that stub file will also be in a stub file. So I think this is fine for this specific scenario.

My first draft of this PR just switched a bunch of callsites from `self.infer_type_expression(expr)` to `self.infer_type_expression_with_state(expr, state)` where `state` was `DeferredExpressionState::Deferred` if we were in a stub file. But that doesn't feel like a robust way of doing it at all, because you have to remember to do that every time, and there were _loads_ of callsites where we were forgetting to do that.

---

_@AlexWaygood reviewed on 2025-11-13 17:13_

---

_@AlexWaygood reviewed on 2025-11-13 17:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:33 on 2025-11-13 17:14_

> I feel like there might be some refactor called for here between this method and `infer_type_expression_with_state`, but I haven't quite sorted yet what it should be...

Yeah, I also looked briefly at whether a refactor would be possible, but it seemed nontrivial...

---

_Merged by @AlexWaygood on 2025-11-13 17:14_

---

_Closed by @AlexWaygood on 2025-11-13 17:14_

---

_Branch deleted on 2025-11-13 17:14_

---

_@carljm reviewed on 2025-11-13 17:16_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:33 on 2025-11-13 17:16_

> Yes, but it can never leak so far as leak into a different file, correct? And when you're inferring types for one scope in a stub file, you know that all other scopes in that stub file will also be in a stub file. So I think this is fine for this specific scenario.

I don't think it's fine, because the updated `DeferredExpressionState::Deferred` that is set here can leak into non-type-expressions in that stub file. And (at least in this PR) it is not the intent that _all_ name references in stub files are deferred, only those in type expressions.  

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:33 on 2025-11-13 17:37_

Oh, good point. Argh, that seems serious enough that I maybe wouldn't have approved if I'd been reviewing 😆 Will fix in a followup!

---

_@AlexWaygood reviewed on 2025-11-13 17:37_

---
