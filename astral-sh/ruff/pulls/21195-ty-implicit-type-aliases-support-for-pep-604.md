```yaml
number: 21195
title: "[ty] Implicit type aliases: Support for PEP 604 unions"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/implicit-type-aliases
created_at: 2025-11-01T23:25:38Z
updated_at: 2025-11-03T20:50:27Z
url: https://github.com/astral-sh/ruff/pull/21195
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Implicit type aliases: Support for PEP 604 unions

---

_Pull request opened by @sharkdp on 2025-11-01 23:25_

## Summary

Add support for implicit type aliases that use PEP 604 unions:
```py
IntOrStr = int | str

reveal_type(IntOrStr)  # UnionType

def _(int_or_str: IntOrStr):
    reveal_type(int_or_str)  # int | str
```

## Typing conformance

The changes are either removed false positives, or new diagnostics due to known limitations unrelated to this PR.

## Ecosystem impact

Spot checked, a mix of true positives and known limitations.

## Test Plan

New Markdown tests.


---

_Label `ty` added by @sharkdp on 2025-11-01 23:25_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-01 23:25_

---

_Comment by @github-actions[bot] on 2025-11-01 23:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-03 19:01:57.863028190 +0000
+++ new-output.txt	2025-11-03 19:02:01.085061379 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(182f5)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/cdd0b85/src/function/execute.rs:419:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18af5)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -7,8 +7,6 @@
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_explicit.py:45:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
 aliases_explicit.py:49:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_explicit.py:50:5: error[type-assertion-failure] Argument does not have asserted type `int | None`
-aliases_explicit.py:51:5: error[type-assertion-failure] Argument does not have asserted type `list[int | None]`
 aliases_explicit.py:52:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 aliases_explicit.py:53:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
 aliases_explicit.py:54:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
@@ -23,8 +21,6 @@
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_implicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_implicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | None`
-aliases_implicit.py:62:5: error[type-assertion-failure] Argument does not have asserted type `list[int | None]`
 aliases_implicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 aliases_implicit.py:64:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
 aliases_implicit.py:65:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
@@ -47,6 +43,7 @@
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_newtype.py:15:1: error[type-assertion-failure] Argument does not have asserted type `int`
 aliases_newtype.py:18:1: error[invalid-assignment] Object of type `NewType` is not assignable to `type`
+aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `NewType`
 aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `NewType`
 aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 3, got 4
 aliases_type_statement.py:10:19: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 3
@@ -54,6 +51,7 @@
 aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
 aliases_type_statement.py:23:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
 aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `typing.TypeAliasType`
+aliases_type_statement.py:31:22: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.TypeAliasType`
 aliases_type_statement.py:37:22: error[invalid-type-form] Function calls are not allowed in type expressions
 aliases_type_statement.py:38:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_type_statement.py:39:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
@@ -912,11 +910,17 @@
 tuples_type_compat.py:47:5: error[invalid-assignment] Object of type `tuple[Any, ...]` is not assignable to `tuple[int, *tuple[str, ...]]`
 tuples_type_compat.py:62:5: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int, int]`
 tuples_type_compat.py:75:9: error[type-assertion-failure] Argument does not have asserted type `tuple[int]`
+tuples_type_compat.py:76:9: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:80:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str] | tuple[int, int]`
+tuples_type_compat.py:81:9: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:85:9: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str, int]`
+tuples_type_compat.py:86:9: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:101:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int]`
+tuples_type_compat.py:102:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:106:13: error[type-assertion-failure] Argument does not have asserted type `tuple[str, str] | tuple[int, int]`
+tuples_type_compat.py:107:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:111:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int, str, int]`
+tuples_type_compat.py:112:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:126:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int | str, str]`
 tuples_type_compat.py:127:13: error[type-assertion-failure] Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
 tuples_type_compat.py:129:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int | str, int]`
@@ -988,5 +992,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 990 diagnostics
+Found 994 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-01 23:35_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-01 23:35_

---

_Comment by @github-actions[bot] on 2025-11-01 23:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
janus (https://github.com/aio-libs/janus)
- All checks passed!
+ janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:714:36: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ Found 3 diagnostics

dacite (https://github.com/konradhalas/dacite)
- dacite/types.py:180:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 23 diagnostics
+ Found 22 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/mirror.py:263:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `Unknown | int | None`
+ src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`

websockets (https://github.com/aaugustin/websockets)
+ src/websockets/asyncio/connection.py:487:45: error[invalid-argument-type] Argument to bound method `send_text` is incorrect: Expected `bytes | bytearray | memoryview[Unknown]`, found `(Iterable[str | bytes | bytearray | memoryview[Unknown]] & ~str) | bytes | bytearray | memoryview[Unknown] | (AsyncIterable[str | bytes | bytearray | memoryview[Unknown]] & ~str)`
+ src/websockets/asyncio/connection.py:489:47: error[invalid-argument-type] Argument to bound method `send_binary` is incorrect: Expected `bytes | bytearray | memoryview[Unknown]`, found `(Iterable[str | bytes | bytearray | memoryview[Unknown]] & ~str) | bytes | bytearray | memoryview[Unknown] | (AsyncIterable[str | bytes | bytearray | memoryview[Unknown]] & ~str)`
- src/websockets/legacy/protocol.py:548:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/websockets/sync/connection.py:486:45: error[invalid-argument-type] Argument to bound method `send_text` is incorrect: Expected `bytes | bytearray | memoryview[Unknown]`, found `(Iterable[str | bytes | bytearray | memoryview[Unknown]] & ~str) | bytes | bytearray | memoryview[Unknown]`
+ src/websockets/sync/connection.py:488:47: error[invalid-argument-type] Argument to bound method `send_binary` is incorrect: Expected `bytes | bytearray | memoryview[Unknown]`, found `(Iterable[str | bytes | bytearray | memoryview[Unknown]] & ~str) | bytes | bytearray | memoryview[Unknown]`
- Found 39 diagnostics
+ Found 42 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/long_running_service_tools.py:357:16: error[invalid-return-type] Return type does not match returned value: expected `int`, found `int | None`
+ paasta_tools/long_running_service_tools.py:357:50: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ paasta_tools/long_running_service_tools.py:357:50: error[invalid-argument-type] Argument to function `min` is incorrect: Expected `int`, found `int | None`
- Found 982 diagnostics
+ Found 983 diagnostics

pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/base_checker.py:191:13: error[invalid-assignment] Not enough values to unpack: Expected 4
+ pylint/checkers/base_checker.py:194:13: error[invalid-assignment] Too many values to unpack: Expected 3
- Found 195 diagnostics
+ Found 197 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/responses.py:255:25: warning[possibly-missing-attribute] Attribute `encode` may be missing on object of type `str | bytes | memoryview[Unknown]`
- Found 142 diagnostics
+ Found 143 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/views/alerts.py:332:26: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 525 diagnostics
+ Found 526 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/python.py:1569:16: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | Unknown | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)`
+ src/_pytest/python.py:1569:20: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `Scope | Unknown | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/_pytest/skipping.py:294:56: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type[BaseException] | tuple[type[BaseException], ...] | AbstractRaises[BaseException]`
- src/_pytest/unittest.py:244:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/unittest.py:581:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/unittest.py:591:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/unittest.py:602:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 484 diagnostics
+ Found 481 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/http/request/form.py:109:19: warning[redundant-cast] Value is already of type `Iterable[str]`
+ tests/test_linkextractors.py:642:63: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:100:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:108:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:117:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:126:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:135:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:144:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:155:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
+ tests/test_loader.py:164:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `type | None`
- Found 1689 diagnostics
+ Found 1699 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/porcelain.py:3710:63: error[invalid-argument-type] Argument to bound method `changes_from_tree` is incorrect: Expected `bytes`, found `Unknown | None`
- Found 175 diagnostics
+ Found 176 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/convert/_reduce/_pep/redpep484612646.py:736:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_check/metadata/hint/hintsmeta.py:632:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/cls/pep/clspep3119.py:766:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/func/arg/utilfuncargget.py:224:11: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/arg/utilfuncarglen.py:159:11: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/arg/utilfuncarglen.py:210:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/func/arg/utilfuncarglen.py:288:11: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/arg/utilfuncargtest.py:35:11: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/arg/utilfuncargtest.py:165:11: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/utilfunccodeobj.py:36:11: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/utilfunccodeobj.py:272:37: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/utilfuncfile.py:44:24: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/utilfuncfile.py:69:37: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/func/utilfunctest.py:74:11: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- beartype/_util/hint/pep/proposal/pep484612646.py:248:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/hint/pep/utilpepget.py:285:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 517 diagnostics
+ Found 501 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[_T@_put]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:382:24: error[invalid-argument-type] Argument to function `heappush` is incorrect: Expected `list[Unknown]`, found `Unknown | None | list[Unknown] | deque[Unknown]`
+ tornado/queues.py:382:37: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `_T@_put` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 331 diagnostics
+ Found 332 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/metaschema.py:351:9: error[invalid-assignment] Object of type `(MutableMapping[str, Any] & ~Top[MutableSequence[Unknown]]) | (int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]])` is not assignable to `MutableMapping[str, Any]`
+ schema_salad/python_codegen_support.py:348:9: error[invalid-assignment] Object of type `(MutableMapping[str, Any] & ~Top[MutableSequence[Unknown]]) | (int & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (float & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]] & ~Top[MutableSequence[Unknown]])` is not assignable to `MutableMapping[str, Any]`
+ schema_salad/schema.py:539:21: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `Never`, found `Literal["name"]`
+ schema_salad/schema.py:541:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ schema_salad/schema.py:543:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["name"]` and a value of type `str` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ schema_salad/schema.py:555:51: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `Never`, found `Literal["abstract"]`
+ schema_salad/schema.py:563:17: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["type", "items", "names", "values", "fields"]` and a value of type `MutableMapping[str, Any] | MutableSequence[Any] | str | MutableMapping[str, str] | list[Any | MutableMapping[str, str] | str]` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ schema_salad/schema.py:572:13: error[invalid-assignment] Method `__setitem__` of type `(bound method MutableMapping[str, Any].__setitem__(key: str, value: Any, /) -> None) | (bound method Top[MutableMapping[Unknown, Unknown]].__setitem__(key: Never, value: Never, /) -> None)` cannot be called with a key of type `Literal["symbols"]` and a value of type `list[str | Unknown]` on object of type `MutableMapping[str, Any] | (MutableSequence[Any] & Top[MutableMapping[Unknown, Unknown]]) | (str & Top[MutableMapping[Unknown, Unknown]])`
+ schema_salad/sourceline.py:203:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 188 diagnostics
+ Found 197 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ cki_lib/footer.py:52:45: error[invalid-argument-type] Argument to function `files` is incorrect: Expected `str | ModuleType`, found `str | None`
- Found 187 diagnostics
+ Found 188 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg/psycopg/errors.py:537:20: warning[possibly-missing-attribute] Attribute `error_field` may be missing on object of type `PGresult | dict[int, bytes | None] | None`
- Found 684 diagnostics
+ Found 685 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ koda_validate/serialization/errors.py:66:42: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ koda_validate/serialization/json_schema.py:313:38: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 70 diagnostics
+ Found 72 diagnostics

xarray (https://github.com/pydata/xarray)
+ asv_bench/benchmarks/dataset_io.py:755:25: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `str | PathLike[Any] | ReadBuffer[Unknown] | ... omitted 3 union elements`, found `None`
+ xarray/backends/h5netcdf_.py:465:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | PathLike[Any] | ReadBuffer[Unknown] | ... omitted 3 union elements`
+ xarray/backends/netCDF4_.py:729:36: error[invalid-argument-type] Argument to function `_has_netcdf_ext` is incorrect: Expected `str | PathLike[Unknown]`, found `str | PathLike[Any] | ReadBuffer[Unknown] | ... omitted 3 union elements`
+ xarray/backends/netCDF4_.py:732:43: error[non-subscriptable] Cannot subscript object of type `PathLike[Any]` with no `__getitem__` method
+ xarray/backends/netCDF4_.py:732:43: error[non-subscriptable] Cannot subscript object of type `ReadBuffer[Unknown]` with no `__getitem__` method
+ xarray/backends/netCDF4_.py:732:43: error[non-subscriptable] Cannot subscript object of type `AbstractDataStore` with no `__getitem__` method
+ xarray/compat/npcompat.py:73:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `set[Unknown | <class 'bool'> | <class 'signedinteger'> | ... omitted 5 union elements]`
- xarray/core/dataset.py:9104:32: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `@Todo | None`
+ xarray/core/dataset.py:9104:32: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float]]] | None`
+ xarray/core/dataset.py:9105:52: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Hashable` on object of type `tuple[int | float, int | float]`
+ xarray/core/dataset.py:9105:52: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ xarray/core/dataset.py:9105:52: error[non-subscriptable] Cannot subscript object of type `float` with no `__getitem__` method
- xarray/core/dataset.py:9106:55: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `@Todo | None`
- xarray/core/dataset.py:9108:46: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `@Todo | None`
- xarray/core/dataset.py:9128:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/core/dataset.py:9106:55: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float]]] | None`
+ xarray/core/dataset.py:9108:46: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float]]] | None`
+ xarray/core/dataset.py:9118:21: error[invalid-argument-type] Argument to bound method `pad` is incorrect: Expected `int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float]] | None`, found `Unknown | int | float | ... omitted 4 union elements`
- xarray/core/indexing.py:508:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/core/indexing.py:504:38: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `(ndarray[Any, dtype[generic[Any]]] & ~int & ~Top[integer[Any]]) | bool`
+ xarray/core/variable.py:2224:49: error[invalid-argument-type] Argument to bound method `pad` is incorrect: Expected `int | float | tuple[int | float, int | float] | Mapping[Any, int | float | tuple[int | float, int | float]] | None`, found `Unknown | ReprObject`
- xarray/tests/test_backends.py:719:54: error[non-subscriptable] Cannot subscript object of type `Timestamp` with no `__getitem__` method
- xarray/tests/test_backends.py:5816:54: error[non-subscriptable] Cannot subscript object of type `Timestamp` with no `__getitem__` method
+ xarray/tests/test_backends_api.py:299:17: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `str | PathLike[Any] | ReadBuffer[Unknown] | ... omitted 3 union elements`, found `Dataset`
- xarray/tests/test_coding_times.py:486:55: error[unknown-argument] Argument `dtype` does not match any known parameter of bound method `to_numpy`
- Found 1735 diagnostics
+ Found 1744 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/annotations.py:1521:13: error[invalid-assignment] Object of type `dict[str, _ChoiceT@__init__]` is not assignable to attribute `_choices` of type `dict[str, int | float | str]`
+ tanjun/annotations.py:2724:26: error[invalid-argument-type] Argument to function `parse_annotated_args` is incorrect: Expected `SlashCommand[Any] | MessageCommand[Any]`, found `_CommandUnionT@with_annotated_args & ~AlwaysFalsy`
- Found 143 diagnostics
+ Found 145 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/data/history/datahandlers/idatahandler.py:209:13: error[call-non-callable] Object of type `Timestamp` is not callable
+ freqtrade/data/history/datahandlers/idatahandler.py:210:13: error[call-non-callable] Object of type `Timestamp` is not callable
+ freqtrade/main.py:81:18: error[invalid-argument-type] Argument to function `exit` is incorrect: Expected `str | int | None`, found `Literal[1, 0, 2] | Any | SystemExit`
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:555:49: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:26: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "trade_id"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:556:54: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:561:54: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "pair"
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:562:25: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "pair" - did you mean "data"?
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:562:62: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:567:54: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "reason"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:568:58: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "lock_end_time"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:572:41: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:575:59: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:578:59: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:581:30: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "status"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCExitMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCExitCancelMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "msg"
+ freqtrade/rpc/telegram.py:583:30: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "msg" - did you mean "id"?
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCStatusMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCStrategyMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCProtectionMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCWhitelistMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCEntryMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCCancelMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCAnalyzedDFMsg`: Unknown key "exit_reason"
+ freqtrade/rpc/telegram.py:605:46: error[invalid-key] Invalid key for TypedDict `RPCNewCandleMsg`: Unknown key "exit_reason"
+ freqtrade/strategy/parameters.py:223:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `object`
+ freqtrade/strategy/parameters.py:223:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `object`
+ freqtrade/strategy/parameters.py:275:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `object`
+ freqtrade/strategy/parameters.py:275:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `object`
+ freqtrade/strategy/parameters.py:287:23: error[unsupported-operator] Operator `*` is unsupported between objects of type `object` and `int`
+ freqtrade/strategy/parameters.py:288:24: error[unsupported-operator] Operator `*` is unsupported between objects of type `object` and `int`
- Found 460 diagnostics
+ Found 577 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
+ src/zope/interface/tests/test_sorting.py:50:9: error[invalid-argument-type] Argument to bound method `sort` is incorrect: Argument type `Unknown | <class 'I1'> | <class 'I3'> | ... omitted 4 union elements` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/zope/interface/tests/test_sorting.py:55:9: error[invalid-argument-type] Argument to bound method `sort` is incorrect: Argument type `Unknown | <class 'I1'> | None | ... omitted 5 union elements` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/zope/interface/tests/test_sorting.py:66:9: error[invalid-argument-type] Argument to bound method `sort` is incorrect: Argument type `Unknown | <class 'I1'>` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 410 diagnostics
+ Found 413 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_tests/test_path.py:56:23: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:56:39: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:64:27: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:64:50: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:82:27: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:82:42: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:91:27: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:91:49: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:106:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
+ src/trio/_tests/test_path.py:107:12: error[invalid-type-form] Variable of type `UnionType` is not allowed in a type expression
- Found 582 diagnostics
+ Found 592 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/experimental/pydantic/conversion.py:39:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `object`
- Found 379 diagnostics
+ Found 380 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/config/cc_mounts.py:189:25: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `Unknown | None | int` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 1140 diagnostics
+ Found 1141 diagnostics

vision (https://github.com/pytorch/vision)
+ references/classification/utils.py:449:73: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `list[type] | None`
+ test/datasets_utils.py:591:56: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `Unknown | None`
- test/datasets_utils.py:651:58: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((str, /) -> Any) & ~AlwaysFalsy) | (def open(fp: @Todo | IO[bytes], mode: Literal["r"] = Literal["r"], formats: list[str] | tuple[str, ...] | None = None) -> ImageFile)`
+ test/datasets_utils.py:651:58: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((str, /) -> Any) & ~AlwaysFalsy) | (def open(fp: str | bytes | PathLike[str] | PathLike[bytes] | IO[bytes], mode: Literal["r"] = Literal["r"], formats: list[str] | tuple[str, ...] | None = None) -> ImageFile)`
+ torchvision/prototype/datasets/utils/_encoded.py:54:33: error[invalid-argument-type] Argument to function `open` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes] | IO[bytes]`, found `ReadOnlyTensorBuffer`
- Found 1500 diagnostics
+ Found 1503 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dask/prefect_dask/task_runners.py:242:44: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ src/integrations/prefect-databricks/prefect_databricks/rest.py:46:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
+ src/integrations/prefect-email/prefect_email/credentials.py:54:24: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `Enum`
+ src/integrations/prefect-gcp/prefect_gcp/models/cloud_run_v2.py:339:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.List`
+ src/integrations/prefect-kubernetes/tests/test_credentials.py:172:50: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
+ src/integrations/prefect-kubernetes/tests/test_credentials.py:185:50: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
+ src/integrations/prefect-kubernetes/tests/test_credentials.py:197:50: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
- Found 3301 diagnostics
+ Found 3308 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/datasets/_openml.py:71:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `Exception`
+ sklearn/externals/_numpydoc/docscrape.py:687:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ sklearn/externals/_numpydoc/docscrape.py:745:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ sklearn/impute/_base.py:516:47: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ sklearn/impute/_base.py:589:35: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ sklearn/linear_model/_glm/glm.py:212:53: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `<class 'float64'>` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- sklearn/neighbors/_lof.py:514:60: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | Literal[1] | None` and `Literal[1]`
+ sklearn/neighbors/_lof.py:288:40: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `Unknown | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 2538 diagnostics
+ Found 2544 diagnostics

arviz (https://github.com/arviz-devs/arviz)
+ arviz/plots/backends/bokeh/kdeplot.py:192:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ arviz/plots/backends/bokeh/kdeplot.py:229:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 822 diagnostics
+ Found 824 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ lib/Crypto/Util/_raw_api.py:125:47: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/Crypto/Util/_raw_api.py:125:47: error[invalid-argument-type] Argument to function `max` is incorrect: Expected `int`, found `Unknown | None`
- Found 1776 diagnostics
+ Found 1778 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/cookiejar.py:235:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aiohttp/cookiejar.py:230:13: error[invalid-assignment] Object of type `ItemsView[object, object]` is not assignable to `Mapping[str, str | BaseCookie[str] | Morsel[Any]] | Iterable[tuple[str, str | BaseCookie[str] | Morsel[Any]]] | BaseCookie[str]`
+ aiohttp/cookiejar.py:290:17: error[invalid-assignment] Method `__setitem__` of type `bound method SimpleCookie.__setitem__(key: str, value: str | Morsel[str]) -> None` cannot be called with a key of type `str` and a value of type `(BaseCookie[str] & Top[Morsel[Unknown]]) | Morsel[Any] | Morsel[str]` on object of type `SimpleCookie`
+ aiohttp/web.py:340:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | (Iterable[str | PathLike[str]] & PathLike[object]) | PathLike[str]`
- Found 181 diagnostics
+ Found 183 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
+ python/egglog/examples/jointree.py:50:16: error[invalid-argument-type] Argument to function `rule` is incorrect: Expected `Fact | BaseExpr`, found `bool`
+ python/egglog/examples/jointree.py:52:27: error[invalid-argument-type] Argument to function `rule` is incorrect: Expected `Fact | BaseExpr`, found `bool`
+ python/egglog/examples/jointree.py:52:44: error[invalid-argument-type] Argument to function `rule` is incorrect: Expected `Fact | BaseExpr`, found `bool`
- Found 1376 diagnostics
+ Found 1379 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/test/utils.pyi:23:28: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- Found 481 diagnostics
+ Found 480 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/auxs/calibtools.py:982:25: error[invalid-argument-type] Argument to function `log` is incorrect: Expected `SupportsFloat | SupportsIndex`, found `Unknown | int | float | property`
- hydpy/cythons/modelutils.py:382:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/indexes/test_indexes.py:436:9: error[type-assertion-failure] Argument does not have asserted type `IntervalIndex[Interval[Timedelta]]`
- tests/indexes/test_indexes.py:444:9: error[type-assertion-failure] Argument does not have asserted type `IntervalIndex[Interval[Timedelta]]`
- tests/indexes/test_indexes.py:454:9: error[type-assertion-failure] Argument does not have asserted type `IntervalIndex[Interval[Timedelta]]`
- tests/indexes/test_indexes.py:464:9: error[type-assertion-failure] Argument does not have asserted type `IntervalIndex[Interval[Timedelta]]`
- tests/indexes/test_indexes.py:472:9: error[type-assertion-failure] Argument does not have asserted type `IntervalIndex[Interval[Timedelta]]`
- tests/indexes/test_indexes.py:973:9: error[type-assertion-failure] Argument does not have asserted type `IntervalIndex[Interval[Timedelta]]`
- tests/indexes/test_indexes.py:982:9: error[type-assertion-failure] Argument does not have asserted type `IntervalIndex[Interval[Timedelta]]`
- tests/indexes/test_indexes.py:1467:11: error[unresolved-attribute] Object of type `Timestamp` has no attribute `array`
- tests/indexes/test_indexes.py:1468:11: error[type-assertion-failure] Argument does not have asserted type `DatetimeArray`
- tests/scalars/test_scalars.py:1353:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1354:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1355:12: warning[possibly-missing-attribute] Attribute `all` may be missing on object of type `bool | @Todo`
- tests/scalars/test_scalars.py:1365:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1366:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1367:12: error[unresolved-attribute] Object of type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1373:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1374:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1375:12: warning[possibly-missing-attribute] Attribute `all` may be missing on object of type `bool | @Todo`
- tests/scalars/test_scalars.py:1385:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1386:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1387:12: error[unresolved-attribute] Object of type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1393:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1394:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1395:12: warning[possibly-missing-attribute] Attribute `all` may be missing on object of type `bool | @Todo`
- tests/scalars/test_scalars.py:1399:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1400:20: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/scalars/test_scalars.py:1401:12: error[unresolved-attribute] Object of type `bool` has no attribute `all`
- tests/scalars/test_scalars.py:1669:11: error[type-assertion-failure] Argument does not have asserted type `Series[bool]`
- tests/scalars/test_scalars.py:1670:11: error[type-assertion-failure] Argument does not have asserted type `Series[bool]`
- tests/scalars/test_scalars.py:1671:11: error[type-assertion-failure] Argument does not have asserted type `Series[bool]`
- tests/series/test_series.py:129:5: error[invalid-assignment] Object of type `Timestamp` is not assignable to `DatetimeIndex`
+ tests/series/test_series.py:129:5: error[invalid-assignment] Object of type `Series[Timestamp]` is not assignable to `DatetimeIndex`
- tests/test_pandas.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_pandas.py:51:9: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_pandas.py:58:9: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_pandas.py:96:9: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_resampler.py:330:15: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_resampler.py:331:15: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_resampler.py:332:15: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_resampler.py:334:13: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_resampler.py:338:13: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_resampler.py:342:13: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_resampler.py:352:11: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_timefuncs.py:115:11: error[type-assertion-failure] Argument does not have asserted type `Series[bool]`
- tests/test_timefuncs.py:116:11: error[type-assertion-failure] Argument does not have asserted type `Series[bool]`
- tests/test_timefuncs.py:117:11: error[type-assertion-failure] Argument does not have asserted type `Series[bool]`
- tests/test_timefuncs.py:177:11: error[type-assertion-failure] Argument does not have asserted type `Timestamp`
- tests/test_timefuncs.py:177:23: error[unresolved-attribute] Object of type `Timestamp` has no attribute `iloc`
+ tests/test_timefuncs.py:202:11: error[type-assertion-failure] Argument does not have asserted type `DatetimeIndex`
+ tests/test_timefuncs.py:222:11: error[type-assertion-failure] Argument does not have asserted type `DatetimeIndex`
- tests/test_timefuncs.py:908:9: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_timefuncs.py:913:9: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_timefuncs.py:913:35: error[unknown-argument] Argument `na_value` does not match any known parameter of bound method `to_numpy`
- tests/test_timefuncs.py:959:9: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_timefuncs.py:959:35: error[unknown-argument] Argument `dtype` does not match any known parameter of bound method `to_numpy`
- tests/test_timefuncs.py:959:48: error[unknown-argument] Argument `copy` does not match any known parameter of bound method `to_numpy`
- tests/test_timefuncs.py:991:9: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_timefuncs.py:991:35: error[unknown-argument] Argument `dtype` does not match any known parameter of bound method `to_numpy`
- tests/test_timefuncs.py:996:9: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_timefuncs.py:996:35: error[unknown-argument] Argument `dtype` does not match any known parameter of bound method `to_numpy`
- tests/test_timefuncs.py:1364:11: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_timefuncs.py:1385:11: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_timefuncs.py:1392:9: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_timefuncs.py:1406:9: error[type-assertion-failure] Argument does not have asserted type `Series[Timestamp]`
- tests/test_timefuncs.py:1876:14: error[unresolved-attribute] Object of type `Timestamp` has no attribute `iloc`
- tests/test_timefuncs.py:1877:11: error[type-assertion-failure] Argument does not have asserted type `Timestamp`
- tests/test_timefuncs.py:1877:23: error[unresolved-attribute] Object of type `Timestamp` has no attribute `iloc`
- tests/test_timefuncs.py:1899:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Timestamp`
- Found 5476 diagnostics
+ Found 5414 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ lib/streamlit/elements/lib/color_util.py:222:33: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[int | float, int | float, int | float]` with length 3
- Found 168 diagnostics
+ Found 169 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/colors/color.py:297:13: error[invalid-assignment] Too many values to unpack: Expected 3
+ src/bokeh/colors/color.py:300:13: error[invalid-assignment] Not enough values to unpack: Expected 4
- src/bokeh/embed/standalone.py:254:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/bokeh/embed/notebook.py:53:78: error[invalid-parameter-default] Default value of type `<class 'FromCurdoc'>` is not assignable to annotated parameter type `Theme | FromCurdoc | None`
+ src/bokeh/embed/standalone.py:262:9: error[invalid-assignment] Object of type `list[object]` is not assignable to `Model | Sequence[Model] | dict[str, Model]`
+ src/bokeh/embed/standalone.py:266:76: error[invalid-argument-type] Argument to function `standalone_docs_json_and_render_items` is incorrect: Expected `Model | Document | Sequence[Model | Document]`, found `Model | Sequence[Model] | dict[str, Model]`
- Found 612 diagnostics
+ Found 616 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ com/win32com/test/testvb.py:172:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ com/win32comext/adsi/demos/test.py:235:27: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ win32/Demos/win32netdemo.py:231:64: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 2607 diagnostics
+ Found 2610 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/custom_partitioning.py:487:60: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ jax/_src/ffi.py:492:34: error[invalid-argument-type] Argument to function `_result_avals` is incorrect: Expected `Sequence[DuckTypedArray | AbstractToken]`, found `(DuckTypedArray & Sequence[object]) | (AbstractToken & Sequence[object]) | Sequence[DuckTypedArray | AbstractToken]`
- jax/_src/ffi.py:490:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/ffi.py:496:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ jax/_src/numpy/lax_numpy.py:1039:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ jax/_src/pallas/mosaic/sc_core.py:369:13: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
+ jax/_src/pallas/mosaic_gpu/core.py:285:13: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
+ jax/_src/pallas/mosaic_gpu/lowering.py:908:9: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Sequence[Hashable]` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ jax/_src/pallas/mosaic_gpu/lowering.py:1022:33: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Sequence[Hashable]` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ jax/_src/pallas/mosaic_gpu/lowering.py:1592:15: error[invalid-argument-type] Argument to bound method `load_untiled` is incorrect: Expected `TiledLayout`, found `(WGSplatFragLayout & ~WGStridedFragLayout) | (TiledLayout & ~WGStridedFragLayout)`
+ jax/example_libraries/stax.py:137:24: error[invalid-argument-type] Argument to function `standardize` is incorrect: Expected `int | Sequence[int] | None`, found `tuple[Unknown | tuple[Literal[0], Literal[1], Literal[2]]] | Unknown | tuple[Literal[0], Literal[1], Literal[2]]`
+ jax/experimental/mosaic/gpu/equations.py:239:6: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Variable | Constant | LeastReplicated | ... omitted 6 union elements`
+ jax/experimental/mosaic/gpu/equations.py:396:13: error[invalid-argument-type] Argument to function `splat_is_compatible_with_tiled` is incorrect: Expected `WGSplatFragLayout`, found `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/equations.py:396:28: error[invalid-argument-type] Argument to function `splat_is_compatible_with_tiled` is incorrect: Expected `TiledLayout`, found `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/equations.py:644:26: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Variable, Constant]`, found `tuple[Variable | Constant | LeastReplicated | ... omitted 6 union elements, Variable | Constant | LeastReplicated | ... omitted 6 union elements]`
+ jax/experimental/mosaic/gpu/equations.py:646:26: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Variable, Constant]`, found `tuple[Variable | Constant | LeastReplicated | ... omitted 6 union elements, Variable | Constant | LeastReplicated | ... omitted 6 union elements]`
- jax/experimental/mosaic/gpu/equations.py:343:7: error[type-assertion-failure] Argument does not have asserted type `Never`
- jax/experimental/mosaic/gpu/equations.py:606:7: error[type-assertion-failure] Argument does not have asserted type `Never`
- jax/experimental/mosaic/gpu/equations.py:644:26: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Variable, Constant]`, found `tuple[@Todo | Unsatisfiable, @Todo | Unsatisfiable]`
- jax/experimental/mosaic/gpu/equations.py:646:26: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Variable, Constant]`, found `tuple[@Todo | Unsatisfiable, @Todo | Unsatisfiable]`
- jax/experimental/mosaic/gpu/equations.py:703:11: error[type-assertion-failure] Argument does not have asserted type `Never`
- jax/experimental/mosaic/gpu/equations.py:720:11: error[type-assertion-failure] Argument does not have asserted type `Never`
- jax/experimental/mosaic/gpu/equations.py:976:9: error[type-assertion-failure] Argument does not have asserted type `Never`
- jax/experimental/mosaic/gpu/layouts.py:336:14: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `None`
- jax/experimental/mosaic/gpu/layouts.py:336:14: error[type-assertion-failure] Argument does not have asserted type `Never`
+ jax/experimental/mosaic/gpu/layouts.py:250:43: error[invalid-argument-type] Argument to function `splat_is_compatible_with_tiled` is incorrect: Expected `WGSplatFragLayout`, found `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/layouts.py:252:12: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/layouts.py:256:43: error[invalid-argument-type] Argument to function `splat_is_compatible_with_tiled` is incorrect: Expected `WGSplatFragLayout`, found `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/layouts.py:258:29: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/layouts.py:307:43: error[invalid-argument-type] Argument to function `splat_is_compatible_with_tiled` is incorrect: Expected `WGSplatFragLayout`, found `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/layouts.py:309:12: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/layouts.py:313:43: error[invalid-argument-type] Argument to function `splat_is_compatible_with_tiled` is incorrect: Expected `WGSplatFragLayout`, found `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/mosaic/gpu/layouts.py:315:29: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `WGSplatFragLayout | WGStridedFragLayout | TiledLayout`
+ jax/experimental/pallas/ops/gpu/attention.py:551:23: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ jax/experimental/pallas/ops/gpu/attention.py:552:24: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ jax/experimental/pallas/ops/gpu/attention.py:553:22: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ jax/experimental/pallas/ops/gpu/attention.py:554:23: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- jax/experimental/pallas/ops/gpu/attention.py:590:36: error[no-matching-overload] No overload of function `cdiv` matches arguments
- jax/experimental/pallas/ops/gpu/attention.py:594:11: error[unsupported-operator] Operator `*` is unsupported between objects of type `int | None | Unknown` and `int | None | Un...*[Comment body truncated]*

---

_Comment by @github-actions[bot] on 2025-11-01 23:41_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 493 | 8 | 13 |
| `invalid-key` | 108 | 0 | 0 |
| `type-assertion-failure` | 2 | 54 | 0 |
| `unused-ignore-comment` | 1 | 34 | 0 |
| `invalid-type-form` | 14 | 14 | 0 |
| `invalid-assignment` | 14 | 0 | 2 |
| `possibly-missing-attribute` | 7 | 4 | 4 |
| `unsupported-operator` | 2 | 11 | 1 |
| `unresolved-attribute` | 0 | 13 | 0 |
| `invalid-return-type` | 2 | 5 | 0 |
| `non-subscriptable` | 5 | 2 | 0 |
| `unknown-argument` | 0 | 6 | 0 |
| `call-non-callable` | 2 | 0 | 0 |
| `index-out-of-bounds` | 1 | 1 | 0 |
| `no-matching-overload` | 1 | 1 | 0 |
| `invalid-parameter-default` | 1 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **654** | **153** | **20** |

**[Full report with detailed diff](https://david-implicit-type-aliases.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-type-aliases.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-02 14:21_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-02 14:21_

---

_Renamed from "[ty] Implicit type aliases: support for unions" to "[ty] Implicit type aliases: support for PEP 604 unions" by @sharkdp on 2025-11-02 14:24_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-02 17:01_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-02 17:01_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-02 18:09_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-02 18:09_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/ty_walltime.rs`:149 on 2025-11-03 13:13_

freqtrade [makes use of a pattern](https://github.com/freqtrade/freqtrade/blob/3cfa366ec9c676abf9da0b3c40d8f3eb43d1e2d2/freqtrade/rpc/telegram.py#L549C1-L549C100) where a union of typeddicts is narrowed by using a special tag (`msg["type"] == RPCMessageType.ENTRY`). We don't support this yet, leading to ~100 new diagnostics in a single file (but nowhere else in the ecosystem).

---

_@sharkdp reviewed on 2025-11-03 13:13_

---

_@sharkdp reviewed on 2025-11-03 13:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:32 on 2025-11-03 13:17_

This is unrelated to unions, just something I thought of while writing the tests below. Probably not super important to support :smile: 

---

_Comment by @github-actions[bot] on 2025-11-03 13:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2025-11-03 14:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6594 on 2025-11-03 14:12_

These are now hard errors. Opaque instances of `UnionType` can not be used in a type expression (see corresponding test)

---

_@sharkdp reviewed on 2025-11-03 14:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:7782 on 2025-11-03 14:15_

For consistency with the branches above, I would like to change this to `types.UnionType`, but I will do that in a separate step, because it causes many downstream changes.

---

_@sharkdp reviewed on 2025-11-03 14:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class_base.rs`:174 on 2025-11-03 14:21_

This is different from the `Type::Union` branch above. `UnionType` instances are not allowed as bases.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-03 14:25_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-03 14:25_

---

_Renamed from "[ty] Implicit type aliases: support for PEP 604 unions" to "[ty] Support for PEP 604 unions (in implicit type aliases)" by @sharkdp on 2025-11-03 14:26_

---

_Renamed from "[ty] Support for PEP 604 unions (in implicit type aliases)" to "[ty] Implicit type aliases: Support for PEP 604 unions" by @sharkdp on 2025-11-03 14:26_

---

_Marked ready for review by @sharkdp on 2025-11-03 14:34_

---

_Review requested from @carljm by @sharkdp on 2025-11-03 14:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-03 14:34_

---

_Review requested from @dcreager by @sharkdp on 2025-11-03 14:34_

---

_Closed by @sharkdp on 2025-11-03 15:14_

---

_Reopened by @sharkdp on 2025-11-03 15:14_

---

_@AlexWaygood reviewed on 2025-11-03 15:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:32 on 2025-11-03 15:44_

It's a one-line fix 😆

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md b/crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md
index 904921e7b3..7e7f2fd59f 100644
--- a/crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md
+++ b/crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md
@@ -22,11 +22,8 @@ f(1)
 ```py
 MyNone = None
 
-# TODO: this should not be an error
-# error: [invalid-type-form] "Variable of type `None` is not allowed in a type expression"
 def g(x: MyNone):
-    # TODO: this should be `None`
-    reveal_type(x)  # revealed: Unknown
+    reveal_type(x)  # revealed: None
 
 g(None)
 ```
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 7d4bf07d97..aaa120f053 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -6565,6 +6565,7 @@ impl<'db> Type<'db> {
             Type::Dynamic(_) => Ok(*self),
 
             Type::NominalInstance(instance) => match instance.known_class(db) {
+                Some(KnownClass::NoneType) => Ok(Type::none(db)),
                 Some(KnownClass::TypeVar) => Ok(todo_type!(
                     "Support for `typing.TypeVar` instances in type expressions"
                 )),
````

may as well just fix it in this PR?

---

_@sharkdp reviewed on 2025-11-03 15:53_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:32 on 2025-11-03 15:53_

I was worried about a case like
```py
def _(n: None):
    x: int | n = 1
```
which shouldn't actually be allowed? I'll propose this as a separate change once this lands.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:197 on 2025-11-03 15:56_

correct -- this was one of the primary motivations for PEP 613

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8911 on 2025-11-03 16:01_

what's the motivation for using a boxed array rather than just two separate fields?

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 7d4bf07d97..a7dc98d47c 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -7651,7 +7651,7 @@ fn walk_known_instance_type<'db, V: visitor::TypeVisitor<'db> + ?Sized>(
         }
         KnownInstanceType::UnionType(union_type) => {
             for element in union_type.elements(db) {
-                visitor.visit_type(db, *element);
+                visitor.visit_type(db, element);
             }
         }
     }
@@ -8907,27 +8907,22 @@ impl<'db> TypeVarBoundOrConstraints<'db> {
 #[salsa::interned(debug)]
 #[derive(PartialOrd, Ord)]
 pub struct UnionTypeInstance<'db> {
-    #[returns(deref)]
-    elements: Box<[Type<'db>; 2]>,
+    left: Type<'db>,
+    right: Type<'db>,
 }
 
 impl get_size2::GetSize for UnionTypeInstance<'_> {}
 
 impl<'db> UnionTypeInstance<'db> {
-    pub(crate) fn from_elements(
-        db: &'db dyn Db,
-        left: Type<'db>,
-        right: Type<'db>,
-    ) -> UnionTypeInstance<'db> {
-        UnionTypeInstance::new(db, Box::new([left, right]))
+    pub(crate) fn elements(self, db: &'db dyn Db) -> [Type<'db>; 2] {
+        [self.left(db), self.right(db)]
     }
 
     pub(crate) fn normalized_impl(self, db: &'db dyn Db, visitor: &NormalizedVisitor<'db>) -> Self {
-        let elements = self.elements(db);
-        UnionTypeInstance::from_elements(
+        UnionTypeInstance::new(
             db,
-            elements[0].normalized_impl(db, visitor),
-            elements[1].normalized_impl(db, visitor),
+            self.left(db).normalized_impl(db, visitor),
+            self.right(db).normalized_impl(db, visitor),
         )
     }
 }
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index faa3b3cac8..5330b2830b 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -8474,7 +8474,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                     Some(left_ty)
                 } else {
                     Some(Type::KnownInstance(KnownInstanceType::UnionType(
-                        UnionTypeInstance::from_elements(
+                        UnionTypeInstance::new(
                             self.db(),
                             convert_none_type(left_ty),
                             convert_none_type(right_ty),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:104 on 2025-11-03 16:02_

do we also need to add a branch for type aliases here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:174 on 2025-11-03 16:02_

can we add an explicit test for that?

---

_@AlexWaygood reviewed on 2025-11-03 16:02_

awesome!

---

_@sharkdp reviewed on 2025-11-03 18:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:8911 on 2025-11-03 18:55_

This struct was initially not interned, so I needed a `Box` anyway. But now that its interned anyway, I should go back to what I had previously, thanks.

---

_@sharkdp reviewed on 2025-11-03 20:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/instance.rs`:104 on 2025-11-03 20:16_

I was mainly trying to replace the previous `ty.as_nominal_instance().is_some_and(|instance| instance.class(db).is_known(…))` pattern, so.. no?

---

_@AlexWaygood reviewed on 2025-11-03 20:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:104 on 2025-11-03 20:22_

yes... I have been wondering if we need to update all of our `as_*` methods along these lines, though... e.g.

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index e79282a35d..f094763fc5 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -1013,9 +1013,10 @@ impl<'db> Type<'db> {
         any_over_type(db, self, &|ty| matches!(ty, Type::TypeVar(_)), false)
     }
 
-    pub(crate) const fn as_class_literal(self) -> Option<ClassLiteral<'db>> {
+    pub(crate) const fn as_class_literal(self, db: &'db dyn Db) -> Option<ClassLiteral<'db>> {
         match self {
             Type::ClassLiteral(class_type) => Some(class_type),
+            Type::TypeAlias(alias) => alias.value_type(db).as_class_literal(),
             _ => None,
         }
     }
```

Since in general, you should always treat a type alias to `T` the same as you treat `T` itself.

But yes, maybe we should just change all these together.

---

_Comment by @AlexWaygood on 2025-11-03 20:40_

Quite a few new diagnostics on this branch have the same cause as those I analysed as happening on `kornia` in https://github.com/astral-sh/ruff/pull/20107#issuecomment-3411347073. I.e., we now emit a diagnostic on things like this, because we now correctly understand typeshed's annotation for `isinstance()` and we correctly do not view the symbol `typing.Dict` as being an instance of `type`:

```py
import typing

isinstance({}, typing.Dict)
```

I think we should fix this, but that fixing it might need some deep special casing of `isinstance()`. We cannot simply treat `typing.Dict` as an alias to `dict`, because it is not, and has different behaviour in some situations:

```pycon
>>> import typing
>>> typing.Dict()
Traceback (most recent call last):
  File "<python-input-7>", line 1, in <module>
    typing.Dict()
    ~~~~~~~~~~~^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1315, in __call__
    raise TypeError(f"Type {self._name} cannot be instantiated; "
                    f"use {self.__origin__.__name__}() instead")
TypeError: Type Dict cannot be instantiated; use dict() instead
```

This doesn't need to be fixed in this PR, but we might want to prioritise fixing it as a followup.

---

_Comment by @AlexWaygood on 2025-11-03 20:44_

The new `invalid-type-form` errors on `trio` and `zulip` also suggest that we might want to prioritise adding support for `type[]` in implicit type aliases, e.g.

```py
X = type[int] | type[str]
y: X
```

---

_@AlexWaygood approved on 2025-11-03 20:44_

---

_Comment by @sharkdp on 2025-11-03 20:50_

> adding support for `type[]` in implicit type aliases

Yes, I saw that. I will update the https://github.com/astral-sh/ty/issues/221 ticket with a list of things that we need to support, and put that near the top of the list.

---

_Merged by @sharkdp on 2025-11-03 20:50_

---

_Closed by @sharkdp on 2025-11-03 20:50_

---

_Branch deleted on 2025-11-03 20:50_

---
