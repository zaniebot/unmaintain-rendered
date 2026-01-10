```yaml
number: 20111
title: "[ty] Infer slightly more precise types for comprehensions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/slightly-better-literals
created_at: 2025-08-27T10:30:30Z
updated_at: 2025-08-27T12:21:49Z
url: https://github.com/astral-sh/ruff/pull/20111
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Infer slightly more precise types for comprehensions

---

_Pull request opened by @AlexWaygood on 2025-08-27 10:30_

## Summary

We currently infer the expression `[1, 2]` as `list[Unknown]`, but we infer the expression `[x for x in range(42)]` as `@Todo`. This seems inconsistent, and the second one seems needlessly imprecise. This PR changes things so that they're both inferred as `list[@Todo]`, and makes similar changes to the inferred types for dicts, sets and generator expressions.

Obviously we hope to address these TODOs pretty soon, but there's still some discussions to be had internally about the best way to go about that. This PR makes our inference more precise now, and (by using `Todo` types instead of `Unknown`) makes it clearer to users when our remaining lack of precision is due to a known limitation in our current model.

## Test Plan

mdtests

---

_Label `ty` added by @AlexWaygood on 2025-08-27 10:30_

---

_Comment by @github-actions[bot] on 2025-08-27 10:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-27 10:42:18.799623048 +0000
+++ new-output.txt	2025-08-27 10:42:21.354647826 +0000
@@ -35,9 +35,10 @@
 aliases_implicit.py:70:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
 aliases_implicit.py:71:5: error[type-assertion-failure] Argument does not have asserted type `list[bool]`
 aliases_implicit.py:72:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
-aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown]` is not allowed in a type expression
+aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[@Todo(list literal element type)]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
-aliases_implicit.py:110:9: error[invalid-type-form] Variable of type `dict[Unknown, Unknown]` is not allowed in a type expression
+aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[@Todo(list comprehension element type)]` is not allowed in a type expression
+aliases_implicit.py:110:9: error[invalid-type-form] Variable of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not allowed in a type expression
 aliases_implicit.py:114:9: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
 aliases_implicit.py:115:10: error[invalid-type-form] Variable of type `Literal[True]` is not allowed in a type expression
 aliases_implicit.py:116:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
@@ -137,7 +138,7 @@
 classes_classvar.py:38:11: error[invalid-type-form] Type qualifier `typing.ClassVar` expected exactly 1 argument, got 2
 classes_classvar.py:39:14: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 classes_classvar.py:40:14: error[unresolved-reference] Name `var` used when not defined
-classes_classvar.py:52:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `list[str]`
+classes_classvar.py:52:5: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to `list[str]`
 classes_classvar.py:55:17: error[invalid-type-form] Type qualifier `typing.ClassVar` is not allowed in type expressions (only in annotation expressions)
 classes_classvar.py:69:23: error[invalid-type-form] `ClassVar` is not allowed in function parameter annotations
 classes_classvar.py:70:12: error[invalid-type-form] `ClassVar` annotations are only allowed in class-body scopes
@@ -852,11 +853,11 @@
 typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
 typeddicts_readonly_inheritance.py:65:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
 typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
-typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
+typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `@Todo(dict literal value type) | None` is not assignable to `str`
 typeddicts_usage.py:23:7: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "director"
 typeddicts_usage.py:24:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal["1982"]`
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 859 diagnostics
+Found 860 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-27 10:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/_make.py:261:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[Unknown, Unknown] | Mapping[str, object]`
+ src/attr/_make.py:261:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)] | Mapping[str, object]`
+ src/attr/_make.py:469:35: warning[possibly-unbound-attribute] Attribute `init` on type `Attribute | Unknown` is possibly unbound
+ src/attr/_make.py:469:59: warning[possibly-unbound-attribute] Attribute `kw_only` on type `Attribute | Unknown` is possibly unbound
+ src/attr/_make.py:481:16: warning[possibly-unbound-attribute] Attribute `alias` on type `Attribute | Unknown` is possibly unbound
+ src/attr/_make.py:483:70: warning[possibly-unbound-attribute] Attribute `name` on type `Attribute | Unknown` is possibly unbound
+ src/attr/_make.py:487:19: warning[possibly-unbound-attribute] Attribute `name` on type `Attribute | Unknown` is possibly unbound
- src/attr/_make.py:1581:5: error[invalid-assignment] Object of type `tuple[@Todo(generator type), ...]` is not assignable to `list[Attribute | Unknown]`
+ src/attr/_make.py:1581:5: error[invalid-assignment] Object of type `tuple[Unknown, ...]` is not assignable to `list[Attribute | Unknown]`
- src/attr/exceptions.py:20:5: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to `tuple[str]`
+ src/attr/exceptions.py:20:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `tuple[str]`
- tests/test_converters.py:350:21: error[invalid-argument-type] Argument to function `to_bool` is incorrect: Expected `str | int`, found `list[Unknown]`
+ tests/test_converters.py:350:21: error[invalid-argument-type] Argument to function `to_bool` is incorrect: Expected `str | int`, found `list[@Todo(list literal element type)]`
- tests/test_next_gen.py:444:25: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `dict[Unknown, Unknown]`
+ tests/test_next_gen.py:444:25: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- Found 556 diagnostics
+ Found 561 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:4103:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 15 diagnostics
+ Found 14 diagnostics

operator (https://github.com/canonical/operator)
- ops/lib/__init__.py:218:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `dict[Unknown, Unknown]`
+ ops/lib/__init__.py:218:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- ops/lib/__init__.py:240:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `dict[Unknown, Unknown]`
+ ops/lib/__init__.py:240:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/codecs/g711.py:65:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
+ src/aiortc/codecs/g711.py:65:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[@Todo(list literal element type)], None]`
- src/aiortc/codecs/g722.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
+ src/aiortc/codecs/g722.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[@Todo(list literal element type)], None]`
- src/aiortc/codecs/opus.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
+ src/aiortc/codecs/opus.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[@Todo(list literal element type)], None]`

trio (https://github.com/python-trio/trio)
- src/trio/_core/_run.py:2685:48: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `@Todo(Support for `typing.TypeAlias`) | list[Unknown]`
+ src/trio/_core/_run.py:2685:48: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `@Todo(Support for `typing.TypeAlias`) | list[@Todo(list literal element type)]`

werkzeug (https://github.com/pallets/werkzeug)
- tests/test_routing.py:601:5: warning[possibly-unbound-attribute] Attribute `add` on type `Unknown | None` is possibly unbound
+ tests/test_routing.py:601:5: warning[possibly-unbound-attribute] Attribute `add` on type `Unknown | None | set[@Todo(set comprehension element type)]` is possibly unbound
- tests/test_routing.py:603:5: warning[possibly-unbound-attribute] Attribute `discard` on type `Unknown | None` is possibly unbound
+ tests/test_routing.py:603:5: warning[possibly-unbound-attribute] Attribute `discard` on type `Unknown | None | set[@Todo(set comprehension element type)]` is possibly unbound

comtypes (https://github.com/enthought/comtypes)
- comtypes/test/test_w_getopt.py:17:37: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ comtypes/test/test_w_getopt.py:17:37: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
- comtypes/test/test_w_getopt.py:24:59: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ comtypes/test/test_w_getopt.py:24:59: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
- comtypes/test/test_w_getopt.py:29:38: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown]`
+ comtypes/test/test_w_getopt.py:29:38: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`

alerta (https://github.com/alerta/alerta)
- alerta/webhooks/prometheus.py:70:41: warning[possibly-unbound-attribute] Attribute `upper` on type `Unknown | None | Literal["unknown"]` is possibly unbound
+ alerta/webhooks/prometheus.py:70:41: warning[possibly-unbound-attribute] Attribute `upper` on type `@Todo(dict literal value type) | None | Literal["unknown"]` is possibly unbound

rich (https://github.com/Textualize/rich)
- tests/test_console.py:225:28: error[invalid-argument-type] Argument to bound method `print_json` is incorrect: Expected `str | None`, found `list[Unknown]`
+ tests/test_console.py:225:28: error[invalid-argument-type] Argument to bound method `print_json` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
- tests/test_highlighter.py:19:21: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | Text`, found `list[Unknown]`
+ tests/test_highlighter.py:19:21: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | Text`, found `list[@Todo(list literal element type)]`
- tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:17:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:17:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:18:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:18:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:19:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:19:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:20:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:20:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
- tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`
- tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`
- tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`
- tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/data_io.py:506:28: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list comprehension element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list comprehension element type)]]` cannot be called with key of type `int | None` on object of type `list[@Todo(list comprehension element type)]`
+ sockeye/data_io.py:508:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list comprehension element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list comprehension element type)]]` cannot be called with key of type `int | None` on object of type `list[@Todo(list comprehension element type)]`
+ sockeye/data_io.py:513:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list comprehension element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list comprehension element type)]]` cannot be called with key of type `int | None` on object of type `list[@Todo(list comprehension element type)]`
+ sockeye/data_io.py:517:21: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list comprehension element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list comprehension element type)]]` cannot be called with key of type `int | None` on object of type `list[@Todo(list comprehension element type)]`
+ sockeye/data_io.py:519:17: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list comprehension element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list comprehension element type)]]` cannot be called with key of type `int | None` on object of type `list[@Todo(list comprehension element type)]`
+ sockeye/data_io.py:522:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list comprehension element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list comprehension element type)]]` cannot be called with key of type `int | None` on object of type `list[@Todo(list comprehension element type)]`
- sockeye/inference.py:350:50: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~list[Unknown]) | None | @Todo(list comprehension type)`
+ sockeye/inference.py:350:50: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~list[Unknown]) | None | list[@Todo(list comprehension element type)]`
- sockeye/inference.py:352:34: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~list[Unknown]) | None | @Todo(list comprehension type)`
+ sockeye/inference.py:352:34: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & ~list[Unknown]) | None | list[@Todo(list comprehension element type)]`
- sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
+ sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo(list literal element type)]`, found `list[Any] | None`
- sockeye/test_utils.py:105:101: error[not-iterable] Object of type `None | @Todo(list comprehension type)` may not be iterable
+ sockeye/test_utils.py:105:101: error[not-iterable] Object of type `None | list[@Todo(list comprehension element type)]` may not be iterable
- sockeye/test_utils.py:224:34: error[invalid-argument-type] Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | Unknown | list[Unknown]`
+ sockeye/test_utils.py:224:34: error[invalid-argument-type] Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | @Todo(dict literal value type) | list[@Todo(list literal element type)]`
- test/unit/test_data_io.py:313:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Unknown], list[Unknown], list[Unknown]]`, found `tuple[@Todo(list comprehension type), @Todo(list comprehension type), @Todo(list comprehension type) | None]`
+ test/unit/test_data_io.py:313:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Unknown], list[Unknown], list[Unknown]]`, found `tuple[list[@Todo(list comprehension element type)], list[@Todo(list comprehension element type)], list[@Todo(list comprehension element type)] | None]`
- Found 309 diagnostics
+ Found 315 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `@Todo(Type::Intersection.call()) | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[Unknown]`
+ src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `@Todo(Type::Intersection.call()) | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[@Todo(list literal element type)]`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_implementations.py:1324:20: error[invalid-return-type] Return type does not match returned value: expected `_T@_sanitize_collection`, found `dict[@Todo(dict comprehension key type), @Todo(dict comprehension value type)]`
- Found 566 diagnostics
+ Found 567 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/apprise_attachment.py:217:25: warning[possibly-unbound-attribute] Attribute `location` on type `AttachBase | Unknown | list[Unknown]` is possibly unbound
+ apprise/apprise_attachment.py:217:25: warning[possibly-unbound-attribute] Attribute `location` on type `AttachBase | Unknown | list[@Todo(list literal element type)]` is possibly unbound
- apprise/apprise_attachment.py:219:20: warning[possibly-unbound-attribute] Attribute `location` on type `AttachBase | Unknown | list[Unknown]` is possibly unbound
+ apprise/apprise_attachment.py:219:20: warning[possibly-unbound-attribute] Attribute `location` on type `AttachBase | Unknown | list[@Todo(list literal element type)]` is possibly unbound
- apprise/apprise_attachment.py:223:56: warning[possibly-unbound-attribute] Attribute `location` on type `AttachBase | Unknown | list[Unknown]` is possibly unbound
+ apprise/apprise_attachment.py:223:56: warning[possibly-unbound-attribute] Attribute `location` on type `AttachBase | Unknown | list[@Todo(list literal element type)]` is possibly unbound
- apprise/apprise_attachment.py:224:25: warning[possibly-unbound-attribute] Attribute `url` on type `AttachBase | Unknown | list[Unknown]` is possibly unbound
+ apprise/apprise_attachment.py:224:25: warning[possibly-unbound-attribute] Attribute `url` on type `AttachBase | Unknown | list[@Todo(list literal element type)]` is possibly unbound
- apprise/persistent_store.py:1708:20: error[invalid-return-type] Return type does not match returned value: expected `set[str]`, found `dict_keys[Unknown, Unknown]`
+ apprise/persistent_store.py:1708:20: error[invalid-return-type] Return type does not match returned value: expected `set[str]`, found `dict_keys[@Todo(dict literal key type), @Todo(dict literal value type)]`
- apprise/plugins/apprise_api.py:344:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[Unknown, Unknown] | str` is possibly unbound
+ apprise/plugins/apprise_api.py:344:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)] | str` is possibly unbound
+ apprise/plugins/email/base.py:1047:17: error[invalid-assignment] Object of type `list[@Todo(list comprehension element type)]` is not assignable to `set[Unknown] | None`
- apprise/utils/parse.py:800:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `(str & ~AlwaysFalsy) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy) | None | Unknown`
+ apprise/utils/parse.py:800:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `(str & ~AlwaysFalsy) | (@Todo(Type::Intersection.call()) & ~AlwaysFalsy) | None | @Todo(dict literal value type)`
- tests/test_api.py:214:18: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `str | dict[Unknown, Unknown] | NotifyBase | AppriseConfig | ConfigBase | list[str | dict[Unknown, Unknown] | NotifyBase | AppriseConfig | ConfigBase]`, found `set[Unknown]`
+ tests/test_api.py:214:18: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `str | dict[Unknown, Unknown] | NotifyBase | AppriseConfig | ConfigBase | list[str | dict[Unknown, Unknown] | NotifyBase | AppriseConfig | ConfigBase]`, found `set[@Todo(set literal element type)]`
- tests/test_api.py:768:44: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `str | list[str] | None`, found `set[Unknown]`
+ tests/test_api.py:768:44: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `str | list[str] | None`, found `set[@Todo(set literal element type)]`
- tests/test_api.py:777:43: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `str | list[str] | None`, found `set[Unknown]`
+ tests/test_api.py:777:43: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `str | list[str] | None`, found `set[@Todo(set literal element type)]`
- tests/test_api.py:1040:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `_url_identifier` on type `NotifyBase | None`
+ tests/test_api.py:1040:5: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to attribute `_url_identifier` on type `NotifyBase | None`
- tests/test_api.py:1054:5: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `_url_identifier` on type `NotifyBase | None`
+ tests/test_api.py:1054:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to attribute `_url_identifier` on type `NotifyBase | None`
- tests/test_api.py:1064:5: error[invalid-assignment] Object of type `set[Unknown]` is not assignable to attribute `_url_identifier` on type `NotifyBase | None`
+ tests/test_api.py:1064:5: error[invalid-assignment] Object of type `set[@Todo(set literal element type)]` is not assignable to attribute `_url_identifier` on type `NotifyBase | None`
- tests/test_apprise_config.py:265:5: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown]` is possibly unbound
+ tests/test_apprise_config.py:265:5: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- tests/test_apprise_config.py:576:5: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[Unknown, Unknown]` is possibly unbound
+ tests/test_apprise_config.py:576:5: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- tests/test_plugin_twitter.py:464:5: warning[possibly-unbound-attribute] Attribute `append` on type `Unknown | list[Unknown] | None` is possibly unbound
+ tests/test_plugin_twitter.py:464:5: warning[possibly-unbound-attribute] Attribute `append` on type `Unknown | list[@Todo(list literal element type)] | None` is possibly unbound
- Found 1799 diagnostics
+ Found 1800 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/base.py:77:38: warning[possibly-unbound-attribute] Attribute `items` on type `Unknown | None | (Unknown & ~list[Unknown] & ~tuple[Unknown, ...] & ~AlwaysFalsy) | dict[Unknown, Unknown]` is possibly unbound
+ mkdocs/tests/base.py:77:38: warning[possibly-unbound-attribute] Attribute `items` on type `Unknown | None | dict[@Todo(dict comprehension key type), @Todo(dict comprehension value type)] | (Unknown & ~list[Unknown] & ~tuple[Unknown, ...] & ~AlwaysFalsy)` is possibly unbound
- mkdocs/tests/build_tests.py:466:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `meta` on type `Page | None`
+ mkdocs/tests/build_tests.py:466:9: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to attribute `meta` on type `Page | None`
- mkdocs/tests/plugin_tests.py:211:67: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `MkDocsConfig`, found `dict[Unknown, Unknown]`
+ mkdocs/tests/plugin_tests.py:211:67: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `MkDocsConfig`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- mkdocs/tests/plugin_tests.py:211:78: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `Files`, found `list[Unknown]`
+ mkdocs/tests/plugin_tests.py:211:78: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `Files`, found `list[@Todo(list literal element type)]`
- mkdocs/tests/plugin_tests.py:229:67: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `MkDocsConfig`, found `dict[Unknown, Unknown]`
+ mkdocs/tests/plugin_tests.py:229:67: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `MkDocsConfig`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- mkdocs/tests/plugin_tests.py:229:78: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `Files`, found `list[Unknown]`
+ mkdocs/tests/plugin_tests.py:229:78: error[invalid-argument-type] Argument to bound method `on_page_content` is incorrect: Expected `Files`, found `list[@Todo(list literal element type)]`
- mkdocs/tests/plugin_tests.py:238:44: error[invalid-argument-type] Argument to bound method `on_nav` is incorrect: Expected `Navigation`, found `list[Unknown]`
+ mkdocs/tests/plugin_tests.py:238:44: error[invalid-argument-type] Argument to bound method `on_nav` is incorrect: Expected `Navigation`, found `list[@Todo(list literal element type)]`
- mkdocs/tests/plugin_tests.py:238:58: error[invalid-argument-type] Argument to bound method `on_nav` is incorrect: Expected `MkDocsConfig`, found `dict[Unknown, Unknown]`
+ mkdocs/tests/plugin_tests.py:238:58: error[invalid-argument-type] Argument to bound method `on_nav` is incorrect: Expected `MkDocsConfig`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- mkdocs/tests/plugin_tests.py:238:69: error[invalid-argument-type] Argument to bound method `on_nav` is incorrect: Expected `Files`, found `list[Unknown]`
+ mkdocs/tests/plugin_tests.py:238:69: error[invalid-argument-type] Argument to bound method `on_nav` is incorrect: Expected `Files`, found `list[@Todo(list literal element type)]`
- mkdocs/tests/plugin_tests.py:245:68: error[invalid-argument-type] Argument to bound method `on_page_read_source` is incorrect: Expected `MkDocsConfig`, found `dict[Unknown, Unknown]`
+ mkdocs/tests/plugin_tests.py:245:68: error[invalid-argument-type] Argument to bound method `on_page_read_source` is incorrect: Expected `MkDocsConfig`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- mkdocs/tests/plugin_tests.py:252:50: error[invalid-argument-type] Argument to bound method `on_pre_build` is incorrect: Expected `MkDocsConfig`, found `dict[Unknown, Unknown]`
+ mkdocs/tests/plugin_tests.py:252:50: error[invalid-argument-type] Argument to bound method `on_pre_build` is incorrect: Expected `MkDocsConfig`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- mkdocs/tests/plugin_tests.py:257:69: error[invalid-argument-type] Argument to bound method `on_page_markdown` is incorrect: Expected `MkDocsConfig`, found `dict[Unknown, Unknown]`
+ mkdocs/tests/plugin_tests.py:257:69: error[invalid-argument-type] Argument to bound method `on_page_markdown` is incorrect: Expected `MkDocsConfig`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- mkdocs/tests/plugin_tests.py:257:80: error[invalid-argument-type] Argument to bound method `on_page_markdown` is incorrect: Expected `Files`, found `list[Unknown]`
+ mkdocs/tests/plugin_tests.py:257:80: error[invalid-argument-type] Argument to bound method `on_page_markdown` is incorrect: Expected `Files`, found `list[@Todo(list literal element type)]`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/pytest/lazy.py:182:61: error[invalid-argument-type] Argument to function `get_fixtures` is incorrect: Expected `dict[str, Any]`, found `Unknown | dict[str, Any] | None | dict[Unknown, Unknown]`
+ src/schemathesis/pytest/lazy.py:182:61: error[invalid-argument-type] Argument to function `get_fixtures` is incorrect: Expected `dict[str, Any]`, found `Unknown | dict[str, Any] | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`

vision (https://github.com/pytorch/vision)
- setup.py:288:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[Unknown]` is possibly unbound
+ setup.py:288:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[@Todo(list literal element type)]` is possibly unbound
- setup.py:289:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[Unknown]` is possibly unbound
+ setup.py:289:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[@Todo(list literal element type)]` is possibly unbound
- setup.py:413:32: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `Unknown | str | list[str] | list[Unknown]`
+ setup.py:413:32: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[@Todo(list literal element type)]` and `Unknown | str | list[str] | list[@Todo(list literal element type)]`
- setup.py:503:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `Unknown | str | list[str] | list[Unknown]`
+ setup.py:503:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[@Todo(list literal element type)]` and `Unknown | str | list[str] | list[@Todo(list literal element type)]`
- test/test_transforms.py:1258:34: error[invalid-argument-type] Argument to function `rotate` is incorrect: Expected `list[int | float] | None`, found `tuple[Unknown, ...]`
+ test/test_transforms.py:1258:34: error[invalid-argument-type] Argument to function `rotate` is incorrect: Expected `list[int | float] | None`, found `tuple[@Todo(list literal element type), ...]`
- test/test_transforms.py:1929:57: error[invalid-argument-type] Argument to function `perspective` is incorrect: Expected `list[int | float] | None`, found `tuple[Unknown, ...]`
+ test/test_transforms.py:1929:57: error[invalid-argument-type] Argument to function `perspective` is incorrect: Expected `list[int | float] | None`, found `tuple[@Todo(list literal element type), ...]`
- test/test_transforms_v2.py:3824:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `dict[Unknown, Unknown]`
+ test/test_transforms_v2.py:3824:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- test/test_transforms_v2.py:3838:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `list[Unknown]`
+ test/test_transforms_v2.py:3838:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `list[@Todo(list literal element type)]`
- test/test_transforms_v2.py:3923:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | Sequence[int | float]`, found `dict[Unknown, Unknown]`
+ test/test_transforms_v2.py:3923:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | Sequence[int | float]`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- test/test_transforms_v2.py:4517:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[Unknown]`
+ test/test_transforms_v2.py:4517:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[@Todo(list literal element type)]`
- test/test_transforms_v2.py:4520:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[Unknown]`
+ test/test_transforms_v2.py:4520:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[@Todo(list literal element type)]`
- test/test_transforms_v2.py:6981:30: warning[possibly-unbound-attribute] Attribute `keys` on type `dict[Unknown, Unknown] | tuple[Image | Unknown, Literal[1] | Unknown]` is possibly unbound
+ test/test_transforms_v2.py:6981:30: warning[possibly-unbound-attribute] Attribute `keys` on type `dict[@Todo(dict literal key type), @Todo(dict literal value type)] | tuple[Image | Unknown, Literal[1] | Unknown]` is possibly unbound

mkosi (https://github.com/systemd/mkosi)
- mkosi/config.py:282:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/config.py:282:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/config.py:488:16: error[invalid-return-type] Return type does not match returned value: expected `Architecture`, found `Unknown | None`
+ mkosi/config.py:488:16: error[invalid-return-type] Return type does not match returned value: expected `Architecture`, found `@Todo(dict literal value type) | None`
- mkosi/config.py:531:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/config.py:531:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/config.py:553:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/config.py:553:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
+ mkosi/config.py:1916:27: warning[possibly-unbound-attribute] Attribute `section` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ mkosi/config.py:1918:83: warning[possibly-unbound-attribute] Attribute `section` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ mkosi/config.py:1922:24: warning[possibly-unbound-attribute] Attribute `name` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ mkosi/config.py:1924:84: warning[possibly-unbound-attribute] Attribute `name` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ mkosi/config.py:1927:20: warning[possibly-unbound-attribute] Attribute `dest` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ mkosi/config.py:1927:30: warning[possibly-unbound-attribute] Attribute `parse` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ mkosi/config.py:1927:56: warning[possibly-unbound-attribute] Attribute `dest` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ mkosi/config.py:4926:31: warning[possibly-unbound-attribute] Attribute `section` on type `Unknown | None` is possibly unbound
+ mkosi/config.py:4928:87: warning[possibly-unbound-attribute] Attribute `section` on type `Unknown | None` is possibly unbound
+ mkosi/config.py:4932:28: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | None` is possibly unbound
+ mkosi/config.py:4934:88: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | None` is possibly unbound
+ mkosi/config.py:4939:29: warning[possibly-unbound-attribute] Attribute `dest` on type `Unknown | None` is possibly unbound
+ mkosi/config.py:4939:39: warning[possibly-unbound-attribute] Attribute `parse` on type `Unknown | None` is possibly unbound
+ mkosi/config.py:4939:66: warning[possibly-unbound-attribute] Attribute `dest` on type `Unknown | None` is possibly unbound
- mkosi/distributions/arch.py:109:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/arch.py:109:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/azure.py:108:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/azure.py:108:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/centos.py:94:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/centos.py:94:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/debian.py:221:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/debian.py:221:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/fedora.py:229:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/fedora.py:229:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/kali.py:58:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/kali.py:58:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/mageia.py:65:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/mageia.py:65:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/openmandriva.py:61:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/openmandriva.py:61:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- mkosi/distributions/opensuse.py:249:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | None`
+ mkosi/distributions/opensuse.py:249:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `@Todo(dict literal value type) | None`
- Found 87 diagnostics
+ Found 101 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceOracle.py:215:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[dict[str, Any]] | None`, found `tuple[dict[Unknown, Unknown], dict[Unknown, Unknown], dict[Unknown, Unknown], dict[Unknown, Unknown]]`
+ cloudinit/sources/DataSourceOracle.py:215:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[dict[str, Any]] | None`, found `tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], dict[@Todo(dict literal key type), @Todo(dict literal value type)], dict[@Todo(dict literal key type), @Todo(dict literal value type)], dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
- cloudinit/util.py:2006:26: error[not-iterable] Object of type `list[Unknown] | None` may not be iterable
+ cloudinit/util.py:2006:26: error[not-iterable] Object of type `list[@Todo(list literal element type)] | None` may not be iterable
- tests/unittests/config/test_modules.py:96:13: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `list[Unknown]`
+ tests/unittests/config/test_modules.py:96:13: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
- tests/unittests/config/test_modules.py:163:13: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `list[Unknown]`
+ tests/unittests/config/test_modules.py:163:13: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
- tests/unittests/config/test_modules.py:197:13: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `list[Unknown]`
+ tests/unittests/config/test_modules.py:197:13: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
- tests/unittests/sources/test_gce.py:104:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown | None` with `Unknown | None | dict[Unknown, Unknown]`
+ tests/unittests/sources/test_gce.py:104:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown | None` with `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- tests/unittests/sources/test_gce.py:105:28: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | None | dict[Unknown, Unknown]` is possibly unbound
+ tests/unittests/sources/test_gce.py:105:28: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- tests/unittests/sources/test_init.py:759:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `network_json` of type `str | None`
+ tests/unittests/sources/test_init.py:759:9: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to attribute `network_json` of type `str | None`
- tests/unittests/sources/test_init.py:778:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `network_json` of type `str | None`
+ tests/unittests/sources/test_init.py:778:9: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to attribute `network_json` of type `str | None`
- tests/unittests/test_helpers.py:21:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Paths`, found `dict[Unknown, Unknown]`
+ tests/unittests/test_helpers.py:21:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Paths`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- tests/unittests/test_helpers.py:32:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Paths`, found `dict[Unknown, Unknown]`
+ tests/unittests/test_helpers.py:32:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Paths`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`

ignite (https://github.com/pytorch/ignite)
- examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:cell 22:7:58: error[invalid-argument-type] Argument to bound method `plot_values` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown]`
+ examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:cell 22:7:58: error[invalid-argument-type] Argument to bound method `plot_values` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[@Todo(list literal element type)]`
- tests/ignite/handlers/test_checkpoint.py:52:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown]`
+ tests/ignite/handlers/test_checkpoint.py:52:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[@Todo(list literal element type)]`
- tests/ignite/handlers/test_checkpoint.py:568:17: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown]`
+ tests/ignite/handlers/test_checkpoint.py:568:17: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[@Todo(list literal element type)]`
- tests/ignite/handlers/test_checkpoint.py:1077:37: error[invalid-argument-type] Argument to function `load_objects` is incorrect: Expected `str | Mapping[Unknown, Unknown] | Path`, found `list[Unknown]`
+ tests/ignite/handlers/test_checkpoint.py:1077:37: error[invalid-argument-type] Argument to function `load_objects` is incorrect: Expected `str | Mapping[Unknown, Unknown] | Path`, found `list[@Todo(list literal element type)]`
- tests/ignite/handlers/test_fbresearch_logger.py:79:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `output` on type `Unknown | State`
+ tests/ignite/handlers/test_fbresearch_logger.py:79:5: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to attribute `output` on type `Unknown | State`
- tests/ignite/handlers/test_fbresearch_logger.py:83:5: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `output` on type `Unknown | State`
+ tests/ignite/handlers/test_fbresearch_logger.py:83:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to attribute `output` on type `Unknown | State`
- tests/ignite/handlers/test_visdom_logger.py:141:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown]`
+ tests/ignite/handlers/test_visdom_logger.py:141:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
- tests/ignite/handlers/test_visdom_logger.py:181:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown]`
+ tests/ignite/handlers/test_visdom_logger.py:181:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
- tests/ignite/handlers/test_visdom_logger.py:242:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown]`
+ tests/ignite/handlers/test_visdom_logger.py:242:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
- tests/ignite/handlers/test_visdom_logger.py:366:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown]`
+ tests/ignite/handlers/test_visdom_logger.py:366:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
- tests/ignite/metrics/test_running_average.py:17:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Metric | None`, found `list[Unknown]`
+ tests/ignite/metrics/test_running_average.py:17:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Metric | None`, found `list[@Todo(list literal element type)]`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_misc.py:324:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[Unknown]`
+ tests/test_misc.py:324:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[@Todo(list literal element type)]`
- tests/test_misc.py:327:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[Unknown]`
+ tests/test_misc.py:327:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[@Todo(list literal element type)]`

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/typeinfo.py:74:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `unsigned` on type `Unknown | ModuleType`
+ pwndbg/aglib/typeinfo.py:74:5: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to attribute `unsigned` on type `Unknown | ModuleType`
- pwndbg/aglib/typeinfo.py:85:5: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `signed` on type `Unknown | ModuleType`
+ pwndbg/aglib/typeinfo.py:85:5: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to attribute `signed` on type `Unknown | ModuleType`
- pwndbg/commands/killthreads.py:59:26: error[not-iterable] Object of type `list[int] | None | @Todo(list comprehension type)` may not be iterable
+ pwndbg/commands/killthreads.py:59:26: error[not-iterable] Object of type `list[int] | None | list[@Todo(list comprehension element type)]` may not be iterable
- pwndbg/commands/killthreads.py:72:62: error[not-iterable] Object of type `list[int] | None | @Todo(list comprehension type)` may not be iterable
+ pwndbg/commands/killthreads.py:72:62: error[not-iterable] Object of type `list[int] | None | list[@Todo(list comprehension element type)]` may not be iterable
- pwndbg/commands/plist.py:395:33: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[int] | None | list[Unknown]`
+ pwndbg/commands/plist.py:395:33: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[int] | None | list[@Todo(list literal element type)]`
- pwndbg/dbg/gdb/__init__.py:1587:21: error[unsupported-operator] Operator `-` is unsupported between objects of type `None | Unknown` and `None | Unknown`
+ pwndbg/dbg/gdb/__init__.py:1587:21: error[unsupported-operator] Operator `-` is unsupported between objects of type `None | @Todo(list literal element type)` and `None | @Todo(list literal element type)`
- pwndbg/dbg/lldb/__init__.py:1518:21: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `Type`
+ pwndbg/dbg/lldb/__init__.py:1518:21: error[invalid-assignment] Object of type `@Todo(dict literal value type) | None` is not assignable to `Type`

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
+ src/typeshed_stats/gather.py:1000:13: error[invalid-argument-type] Argument to bound method `from_lines` is incorrect: Expected `str | ((str, /) -> Pattern)`, found `<class 'GitWildMatchPattern'>`
- Found 24 diagnostics
+ Found 25 diagnostics

stone (https://github.com/dropbox/stone)
- stone/backends/python_rsrc/stone_validators.py:714:16: error[invalid-exception-caught] Cannot catch object of type `list[Unknown]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ stone/backends/python_rsrc/stone_validators.py:714:16: error[invalid-exception-caught] Cannot catch object of type `list[@Todo(list literal element type)]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ test/test_backend.py:373:29: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ test/test_backend.py:374:29: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ test/test_backend.py:376:17: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ test/test_backend.py:380:17: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ test/test_backend.py:469:30: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ test/test_backend.py:471:22: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ test/test_backend.py:495:30: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
+ test/test_backend.py:497:17: warning[possibly-unbound-attribute] Attribute `fields` on type `@Todo(dict comprehension value type) | None` is possibly unbound
- Found 78 diagnostics
+ Found 86 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/data_kitchen.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DataFrame, DataFrame]`, found `tuple[Unknown, Unknown | list[Unknown]]`
+ freqtrade/freqai/data_kitchen.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DataFrame, DataFrame]`, found `tuple[Unknown, Unknown | list[@Todo(list literal element type)]]`
- freqtrade/freqai/data_kitchen.py:827:17: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `(dict[str, dict[str, DataFrame]] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ freqtrade/freqai/data_kitchen.py:827:17: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, dict[str, DataFrame]].__setitem__(key: str, value: dict[str, DataFrame], /) -> None) | (bound method dict[@Todo(dict literal key type), @Todo(dict literal value type)].__setitem__(key: @Todo(dict literal key type), value: @Todo(dict literal value type), /) -> None)` cannot be called with a key of type `Unknown` and a value of type `DataFrame` on object of type `(dict[str, dict[str, DataFrame]] & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- freqtrade/freqai/data_kitchen.py:830:21: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None) | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None)` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown, Unknown]` on object of type `(dict[str, DataFrame] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ freqtrade/freqai/data_kitchen.py:830:21: error[invalid-assignment] Method `__setitem__` of type `(bound method dict[str, DataFrame].__setitem__(key: str, value: DataFrame, /) -> None) | (bound method dict[@Todo(dict literal key type), @Todo(dict literal value type)].__setitem__(key: @Todo(dict literal key type), value: @Todo(dict literal value type), /) -> None)` cannot be called with a key of type `Unknown` and a value of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` on object of type `(dict[str, DataFrame] & ~AlwaysFalsy) | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`

optuna (https://github.com/optuna/optuna)
- optuna/study/_multi_objective.py:26:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `@Todo(list comprehension type) | list[int | float] | None`
- optuna/study/_multi_objective.py:32:50: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `@Todo(list comprehension type) | list[int | float] | None`
- tests/importance_tests/fanova_tests/test_tree.py:117:10: error[unsupported-operator] Operator `-` is unsupported between objects of type `list[Unknown]` and `floating[Any]`
+ tests/importance_tests/fanova_tests/test_tree.py:117:10: error[unsupported-operator] Operator `-` is unsupported between objects of type `list[@Todo(list literal element type)]` and `floating[Any]`
- tests/test_cli.py:1218:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1218:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1219:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1219:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1220:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1220:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1274:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1274:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1275:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params_x"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1275:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params_x"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1276:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params_y"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1276:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params_y"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1314:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1314:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1315:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1315:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1352:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown]`
+ tests/test_cli.py:1352:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
- Found 563 diagnostics
+ Found 561 diagnostics

isort (https://github.com/pycqa/isort)
- isort/output.py:523:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[Unknown]`
+ isort/output.py:523:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[@Todo(list literal element type)]`
- isort/output.py:533:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[Unknown]`
+ isort/output.py:533:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[@Todo(list literal element type)]`
- isort/output.py:541:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[Unknown]`
+ isort/output.py:541:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[@Todo(list literal element type)]`

pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/demos/dump_clipboard.py:31:33: error[invalid-argument-type] Argument to bound method `QueryGetData` is incorrect: Expected `PyFORMATETC`, found `tuple[Unknown, Unknown, Unknown, Unknown, @Todo(list comprehension type)]`
+ com/win32com/demos/dump_clipboard.py:31:33: error[invalid-argument-type] Argument to bound method `QueryGetData` is incorrect: Expected `PyFORMATETC`, found `tuple[Unknown, Unknown, Unknown, Unknown, @Todo(list comprehension element type)]`
- com/win32com/demos/dump_clipboard.py:36:37: err...*[Comment body truncated]*

---

_Comment by @AlexWaygood on 2025-08-27 10:36_

The conformance test suite diff is good -- we're now correctly emitting an error on [this line](https://github.com/python/typing/blob/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance/tests/aliases_implicit.py#L109)

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-27 10:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:78 on 2025-08-27 10:48_

asserting the full error message here is now slightly more complicated because Todo types are rendered differently in release builds, and our mdtest regex that usually deals with that didn't seem able to handle a Todo type nested inside two pairs of square brackets. It didn't seem essential for us to assert the full error message here, so I just simplified the test

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/directives/assert_never.md`:41 on 2025-08-27 10:49_

`<!-- snapshot-diagnostics -->` is enabled in this section, and it gets complicated to have snapshots for diagnostics that talk about Todo types because of the fact that Todo types are rendered differently in release builds. It seemed like we already had good test coverage of `assert_never()` diagnostics in this section even without these lines, so I just removed them

---

_@AlexWaygood reviewed on 2025-08-27 10:49_

---

_Marked ready for review by @AlexWaygood on 2025-08-27 10:49_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-27 10:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-27 10:49_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-27 10:49_

---

_Comment by @github-actions[bot] on 2025-08-27 10:50_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 11 | 2 | 273 |
| `unsupported-operator` | 5 | 0 | 63 |
| `invalid-assignment` | 10 | 0 | 55 |
| `possibly-unbound-attribute` | 31 | 0 | 33 |
| `invalid-return-type` | 5 | 0 | 54 |
| `not-iterable` | 1 | 0 | 16 |
| `possibly-unbound-implicit-call` | 0 | 0 | 11 |
| `non-subscriptable` | 6 | 0 | 2 |
| `type-assertion-failure` | 0 | 5 | 0 |
| `unresolved-attribute` | 1 | 0 | 1 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| `call-non-callable` | 0 | 0 | 1 |
| `invalid-exception-caught` | 0 | 0 | 1 |
| `no-matching-overload` | 1 | 0 | 0 |
| **Total** | **71** | **9** | **510** |

**[Full report with detailed diff](https://alex-slightly-better-literal.ecosystem-663.pages.dev/diff)**


---

_Comment by @AlexWaygood on 2025-08-27 10:57_

I haven't done a detailed analysis of the primer report -- I can do if we want, but from a brief skim through it looks like basically what we'd expect. We're getting more diagnostics because we now understand that dict comprehensions create dicts and list comprehensions create lists, and dicts and lists (unlike `Unknown`) do not have all arbitrary attributes. It doesn't look like we're running into any false positives that would be fixed by bidirectional inference, for example -- specializing these containers with dynamic types works around that issue.

---

_@sharkdp approved on 2025-08-27 12:12_

This makes sense to me!

---

_Merged by @AlexWaygood on 2025-08-27 12:21_

---

_Closed by @AlexWaygood on 2025-08-27 12:21_

---

_Branch deleted on 2025-08-27 12:21_

---
