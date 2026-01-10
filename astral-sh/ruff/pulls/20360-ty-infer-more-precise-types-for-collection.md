```yaml
number: 20360
title: "[ty] Infer more precise types for collection literals "
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/bidirectional-inference
created_at: 2025-09-12T04:33:44Z
updated_at: 2025-09-18T19:31:03Z
url: https://github.com/astral-sh/ruff/pull/20360
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Infer more precise types for collection literals 

---

_Pull request opened by @ibraheemdev on 2025-09-12 04:33_

## Summary

Part of https://github.com/astral-sh/ty/issues/168 and https://github.com/astral-sh/ty/issues/543. Infer more precise types for collection literals (currently, only `list` and `set`). For example,

```py
x = [1, 2, 3] # revealed: list[Unknown | int]
y: list[int] = [1, 2, 3] # revealed: list[int]
```

This could easily be extended to `dict` literals, but I am intentionally limiting scope for now.

---

_Label `ty` added by @ibraheemdev on 2025-09-12 04:33_

---

_Review requested from @carljm by @ibraheemdev on 2025-09-12 04:33_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-09-12 04:33_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-09-12 04:33_

---

_Review requested from @dcreager by @ibraheemdev on 2025-09-12 04:33_

---

_Comment by @github-actions[bot] on 2025-09-12 04:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-17 19:38:23.513997998 +0000
+++ new-output.txt	2025-09-17 19:38:26.563992249 +0000
@@ -36,7 +36,7 @@
 aliases_implicit.py:70:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
 aliases_implicit.py:71:5: error[type-assertion-failure] Argument does not have asserted type `list[bool]`
 aliases_implicit.py:72:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
-aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[@Todo(list literal element type)]` is not allowed in a type expression
+aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[@Todo(list comprehension element type)]` is not allowed in a type expression
 aliases_implicit.py:110:9: error[invalid-type-form] Variable of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not allowed in a type expression
@@ -301,7 +301,6 @@
 directives_reveal_type.py:19:5: error[missing-argument] No argument provided for required parameter `obj` of function `reveal_type`
 directives_reveal_type.py:20:20: error[too-many-positional-arguments] Too many positional arguments to function `reveal_type`: expected 1, got 2
 directives_type_checking.py:11:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
-directives_type_checking.py:18:1: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 directives_type_ignore_file2.py:14:1: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:14:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:19:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
@@ -620,6 +619,7 @@
 literals_literalstring.py:120:22: error[invalid-argument-type] Argument to function `literal_identity` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `TLiteral`
 literals_literalstring.py:130:5: error[invalid-assignment] Object of type `Container[str]` is not assignable to `Container[LiteralString]`
 literals_literalstring.py:134:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `T`
+literals_literalstring.py:138:1: error[invalid-assignment] Object of type `list[str]` is not assignable to `list[LiteralString]`
 literals_literalstring.py:167:1: error[type-assertion-failure] Argument does not have asserted type `A`
 literals_literalstring.py:171:5: error[invalid-assignment] Object of type `list[LiteralString]` is not assignable to `list[str]`
 literals_parameterizations.py:41:15: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
@@ -718,7 +718,6 @@
 overloads_evaluation.py:161:5: error[type-assertion-failure] Argument does not have asserted type `Literal[0, 1]`
 overloads_evaluation.py:234:5: error[type-assertion-failure] Argument does not have asserted type `int`
 overloads_evaluation.py:291:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@example6`
-overloads_evaluation.py:322:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 protocols_class_objects.py:58:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
 protocols_definition.py:114:1: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
@@ -862,5 +861,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 863 diagnostics
+Found 862 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-12 04:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
+ parso/python/diff.py:884:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int, int]`, found `tuple[Unknown | int, ...]`
- Found 74 diagnostics
+ Found 75 diagnostics

attrs (https://github.com/python-attrs/attrs)
- src/attr/exceptions.py:20:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `tuple[str]`
+ src/attr/exceptions.py:20:5: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `tuple[str]`
+ src/attr/validators.py:175:31: error[unresolved-attribute] Type `(Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = int) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = int) -> Match[bytes] | None]) & ~AlwaysFalsy` has no attribute `__name__`
- tests/test_converters.py:350:21: error[invalid-argument-type] Argument to function `to_bool` is incorrect: Expected `str | int`, found `list[@Todo(list literal element type)]`
+ tests/test_converters.py:350:21: error[invalid-argument-type] Argument to function `to_bool` is incorrect: Expected `str | int`, found `list[Unknown]`
+ tests/test_validators.py:870:13: warning[possibly-unbound-attribute] Attribute `__name__` on type `Unknown | ((val: _T@lt) -> @Todo(Inference of subscript on special form)) | ((val: _T@le) -> @Todo(Inference of subscript on special form)) | ((val: _T@ge) -> @Todo(Inference of subscript on special form)) | ((val: _T@gt) -> @Todo(Inference of subscript on special form))` is possibly unbound
- Found 563 diagnostics
+ Found 565 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_core/_subprocesses.py:125:12: error[invalid-return-type] Return type does not match returned value: expected `CompletedProcess[bytes]`, found `CompletedProcess[bytes | None]`
- Found 224 diagnostics
+ Found 225 diagnostics

isort (https://github.com/pycqa/isort)
- isort/output.py:523:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[@Todo(list literal element type)]`
+ isort/output.py:523:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[Unknown]`
- isort/output.py:533:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[@Todo(list literal element type)]`
+ isort/output.py:533:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[Unknown]`
- isort/output.py:541:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[@Todo(list literal element type)]`
+ isort/output.py:541:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Unknown | None | list[Unknown]`

websockets (https://github.com/aaugustin/websockets)
+ src/websockets/frames.py:196:20: error[no-matching-overload] No overload of bound method `join` matches arguments
- src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `@Todo(Type::Intersection.call()) | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[@Todo(list literal element type)]`
+ src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `@Todo(Type::Intersection.call()) | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[Unknown]`
- Found 40 diagnostics
+ Found 41 diagnostics

stone (https://github.com/dropbox/stone)
- stone/backends/python_rsrc/stone_validators.py:714:16: error[invalid-exception-caught] Cannot catch object of type `list[@Todo(list literal element type)]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ stone/backends/python_rsrc/stone_validators.py:714:16: error[invalid-exception-caught] Cannot catch object of type `list[Unknown | <class 'AttributeError'> | <class 'ValueError'>]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/codecs/g711.py:65:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[@Todo(list literal element type)], None]`
+ src/aiortc/codecs/g711.py:65:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
- src/aiortc/codecs/g722.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[@Todo(list literal element type)], None]`
+ src/aiortc/codecs/g722.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`
- src/aiortc/codecs/opus.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[@Todo(list literal element type)], None]`
+ src/aiortc/codecs/opus.py:73:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[bytes], int]`, found `tuple[list[Unknown], None]`

alerta (https://github.com/alerta/alerta)
+ alerta/auth/basic.py:31:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/basic.py:85:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/decorators.py:142:60: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/decorators.py:147:57: error[invalid-argument-type] Argument to function `get_customers` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/github.py:85:115: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/oidc.py:192:95: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/saml.py:100:98: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/views/permissions.py:77:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Scope]`, found `Unknown | list[Unknown | str]`
+ alerta/views/users.py:33:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ tests/test_scopes.py:81:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:82:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:83:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:85:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:86:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:87:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:89:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:90:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:92:59: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:93:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:94:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:95:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:96:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:97:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:99:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:100:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:101:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
- Found 487 diagnostics
+ Found 513 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/pygments/lexers/__init__.py:340:12: error[call-non-callable] Object of type `float` is not callable
+ src/pip/_vendor/resolvelib/resolvers/resolution.py:48:35: error[invalid-argument-type] Argument to function `_has_route_to_root` is incorrect: Expected `Mapping[KT@_build_result | @Todo(dict comprehension value type) | None, Criterion[RT@_build_result, CT@_build_result]]`, found `dict[KT@_build_result, Criterion[RT@_build_result, CT@_build_result]]`
- src/pip/_vendor/urllib3/packages/six.py:1058:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to attribute `submodule_search_locations` on type `ModuleSpec | None`
+ src/pip/_vendor/urllib3/packages/six.py:1058:5: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `submodule_search_locations` on type `ModuleSpec | None`
- Found 434 diagnostics
+ Found 436 diagnostics

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/imports.py:169:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 179 diagnostics
+ Found 178 diagnostics

rich (https://github.com/Textualize/rich)
- tests/test_console.py:225:28: error[invalid-argument-type] Argument to bound method `print_json` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
+ tests/test_console.py:225:28: error[invalid-argument-type] Argument to bound method `print_json` is incorrect: Expected `str | None`, found `list[Unknown | str]`
- tests/test_highlighter.py:19:21: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | Text`, found `list[@Todo(list literal element type)]`
+ tests/test_highlighter.py:19:21: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | Text`, found `list[Unknown]`
+ tests/test_pretty.py:315:5: warning[possibly-unbound-attribute] Attribute `append` on type `Unknown | int | list[Unknown]` is possibly unbound
- tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:17:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:17:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:18:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:18:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:19:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:19:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:20:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:20:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown | str]]`
- tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
- tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
- tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
- tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, @Todo(list literal element type)]]`
+ tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown | str]]`
- Found 308 diagnostics
+ Found 309 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/model.py:812:9: error[invalid-assignment] Object of type `list[Unknown | None]` is not assignable to `list[int] | None`
+ sockeye/model.py:816:56: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[int] | None`
- sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[@Todo(list literal element type)]`, found `list[Any] | None`
+ sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
- sockeye/test_utils.py:224:34: error[invalid-argument-type] Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | @Todo(dict literal value type) | list[@Todo(list literal element type)]`
+ sockeye/test_utils.py:224:34: error[invalid-argument-type] Argument to function `generate_json_input_file_with_tgt_prefix` is incorrect: Expected `list[str]`, found `None | @Todo(dict literal value type) | list[Unknown]`
+ test/unit/test_output_handler.py:46:43: error[invalid-argument-type] Argument is incorrect: Expected `list[list[int | float]] | None`, found `list[Unknown | float]`
- Found 315 diagnostics
+ Found 318 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_generate_schema.py:126:1: error[invalid-assignment] Object of type `list[typing.Tuple | <class 'tuple'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:127:1: error[invalid-assignment] Object of type `list[typing.List | <class 'list'> | <class 'MutableSequence'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:128:1: error[invalid-assignment] Object of type `list[typing.Set | <class 'set'> | <class 'MutableSet'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:129:1: error[invalid-assignment] Object of type `list[typing.FrozenSet | <class 'frozenset'> | <class 'AbstractSet'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:130:1: error[invalid-assignment] Object of type `list[typing.Dict | <class 'dict'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:134:1: error[invalid-assignment] Object of type `list[typing.Type | <class 'type'>]` is not assignable to `list[type]`
+ pydantic/_internal/_generate_schema.py:155:1: error[invalid-assignment] Object of type `list[<class 'deque'> | typing.Deque]` is not assignable to `list[type]`
- Found 754 diagnostics
+ Found 761 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_command_check.py:110:9: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to attribute `unexpectedSuccesses` of type `list[TestCase]`
+ tests/test_commands.py:348:65: error[invalid-argument-type] Argument to function `_print_unknown_command_msg` is incorrect: Expected `bool`, found `Unknown | int`
- tests/test_utils_datatypes.py:281:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `set[@Todo(set literal element type)]`
+ tests/test_utils_datatypes.py:281:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Any]`, found `set[Unknown | int | str | float]`
- Found 1048 diagnostics
+ Found 1050 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/mesh_status.py:117:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, list[str]]`, found `tuple[Unknown | None, list[@Todo(list literal element type)]]`
+ paasta_tools/cli/cmds/mesh_status.py:117:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, list[str]]`, found `tuple[Unknown | None, list[Unknown | str]]`
- paasta_tools/cli/utils.py:416:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[str], str]`, found `tuple[list[str] | list[@Todo(list literal element type)], None | str]`
+ paasta_tools/cli/utils.py:416:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[str], str]`, found `tuple[list[str] | list[Unknown], None | str]`
+ paasta_tools/secret_providers/vault.py:202:21: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `CryptoKey`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ paasta_tools/utils.py:594:9: error[invalid-assignment] Object of type `list[dict[@Todo(dict literal key type), @Todo(dict literal value type)]]` is not assignable to `list[DockerParameter]`
+ paasta_tools/utils.py:601:17: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `DockerParameter`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- Found 869 diagnostics
+ Found 872 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ tests/GithubIntegration.py:100:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AppAuth | None`, found `Unknown | Login`
- Found 315 diagnostics
+ Found 316 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- src/check_jsonschema/utils.py:114:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 57 diagnostics
+ Found 56 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ porcupine/plugins/longlinemarker.py:56:17: error[invalid-assignment] Object of type `list[str]` is not assignable to `list[Literal["bgcolor", "color", "border"]]`
- Found 21 diagnostics
+ Found 22 diagnostics

ignite (https://github.com/pytorch/ignite)
- examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:cell 22:7:58: error[invalid-argument-type] Argument to bound method `plot_values` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[@Todo(list literal element type)]`
+ examples/notebooks/Cifar10_Ax_hyperparam_tuning.ipynb:cell 22:7:58: error[invalid-argument-type] Argument to bound method `plot_values` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown | tuple[int, float]]`
+ ignite/handlers/param_scheduler.py:1179:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | ParamGroupScheduler | Unknown]`
+ ignite/handlers/param_scheduler.py:1188:73: error[invalid-argument-type] Argument to bound method `simulate_values` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | ParamGroupScheduler | Unknown]`
+ tests/ignite/distributed/utils/__init__.py:433:25: error[invalid-argument-type] Argument to function `new_group` is incorrect: Expected `list[int]`, found `list[Unknown | str]`
+ tests/ignite/engine/test_deterministic.py:880:12: warning[possibly-unbound-attribute] Attribute `device` on type `Unknown | tuple[Any, ...]` is possibly unbound
+ tests/ignite/handlers/test_base_logger.py:265:5: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to attribute `_events` of type `list[CallableEventWithFilter]`
- tests/ignite/handlers/test_checkpoint.py:52:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[@Todo(list literal element type)]`
+ tests/ignite/handlers/test_checkpoint.py:52:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown | int]`
- tests/ignite/handlers/test_checkpoint.py:568:17: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[@Todo(list literal element type)]`
+ tests/ignite/handlers/test_checkpoint.py:568:17: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Mapping[Unknown, Unknown]`, found `list[Unknown]`
- tests/ignite/handlers/test_checkpoint.py:1077:37: error[invalid-argument-type] Argument to function `load_objects` is incorrect: Expected `str | Mapping[Unknown, Unknown] | Path`, found `list[@Todo(list literal element type)]`
+ tests/ignite/handlers/test_checkpoint.py:1077:37: error[invalid-argument-type] Argument to function `load_objects` is incorrect: Expected `str | Mapping[Unknown, Unknown] | Path`, found `list[Unknown]`
- tests/ignite/handlers/test_fbresearch_logger.py:83:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to attribute `output` on type `Unknown | State`
+ tests/ignite/handlers/test_fbresearch_logger.py:83:5: error[invalid-assignment] Object of type `list[Unknown | float]` is not assignable to attribute `output` on type `Unknown | State`
+ tests/ignite/handlers/test_param_scheduler.py:349:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[Unknown | LinearCyclicalScheduler | int]`
+ tests/ignite/handlers/test_param_scheduler.py:355:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `list[Unknown | int | float]`
+ tests/ignite/handlers/test_param_scheduler.py:367:84: error[invalid-argument-type] Argument to bound method `simulate_values` is incorrect: Expected `list[str] | tuple[str] | None`, found `list[Unknown | int]`
+ tests/ignite/handlers/test_param_scheduler.py:768:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float]]`
+ tests/ignite/handlers/test_param_scheduler.py:771:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[int, float] | tuple[float]]`
+ tests/ignite/handlers/test_param_scheduler.py:777:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float, int]]`
+ tests/ignite/handlers/test_param_scheduler.py:1051:52: error[not-iterable] Object of type `None` is not iterable
+ tests/ignite/handlers/test_param_scheduler.py:1118:52: error[not-iterable] Object of type `None` is not iterable
+ tests/ignite/handlers/test_param_scheduler.py:1171:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[Unknown | int]`
+ tests/ignite/handlers/test_param_scheduler.py:1174:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[Unknown | LinearCyclicalScheduler | str]`
+ tests/ignite/handlers/test_param_scheduler.py:1180:72: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `list[Unknown | int]`
+ tests/ignite/handlers/test_state_param_scheduler.py:151:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float]]`
+ tests/ignite/handlers/test_state_param_scheduler.py:154:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[int, float] | tuple[float]]`
+ tests/ignite/handlers/test_state_param_scheduler.py:160:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float, int]]`
- tests/ignite/handlers/test_visdom_logger.py:141:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
+ tests/ignite/handlers/test_visdom_logger.py:141:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown | str]`
- tests/ignite/handlers/test_visdom_logger.py:181:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
+ tests/ignite/handlers/test_visdom_logger.py:181:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown | str]`
- tests/ignite/handlers/test_visdom_logger.py:242:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
+ tests/ignite/handlers/test_visdom_logger.py:242:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown | str]`
- tests/ignite/handlers/test_visdom_logger.py:366:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[@Todo(list literal element type)]`
+ tests/ignite/handlers/test_visdom_logger.py:366:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `list[Unknown | str]`
+ tests/ignite/metrics/nlp/test_bleu.py:26:29: error[invalid-argument-type] Argument to bound method `_corpus_bleu` is incorrect: Expected `Sequence[Sequence[Sequence[Any]]]`, found `list[Unknown | list[Unknown | int]]`
+ tests/ignite/metrics/test_metric.py:1226:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_numerator` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1227:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_denominator` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1228:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_weight` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1229:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_updated` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1231:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_numerator` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1232:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_denominator` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1233:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_weight` on type `Metric | Unknown`
+ tests/ignite/metrics/test_metric.py:1234:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_updated` on type `Metric | Unknown`
- tests/ignite/metrics/test_running_average.py:17:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Metric | None`, found `list[@Todo(list literal element type)]`
+ tests/ignite/metrics/test_running_average.py:17:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Metric | None`, found `list[Unknown | int]`
- Found 2111 diagnostics
+ Found 2139 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ mkosi/config.py:3132:45: error[invalid-argument-type] Argument to function `make_simple_config_parser` is incorrect: Expected `Sequence[ConfigSetting[object]]`, found `list[ConfigSetting[Any] | ConfigSetting[bool]]`
- Found 99 diagnostics
+ Found 100 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/doctest.py:329:13: error[invalid-assignment] Object of type `list[Unknown | BaseException]` is not assignable to `Sequence[DocTestFailure | UnexpectedException] | None`
- src/_pytest/pytester.py:1140:13: error[invalid-assignment] Object of type `int` is not assignable to attribute `ret` on type `@Todo(list literal element type) | <class 'reprec'>`
+ src/_pytest/pytester.py:1140:13: error[invalid-assignment] Object of type `int` is not assignable to attribute `ret` on type `Unknown | <class 'reprec'>`
- src/_pytest/pytester.py:1145:25: warning[possibly-unbound-attribute] Attribute `getcalls` on type `@Todo(list literal element type) | <class 'reprec'>` is possibly unbound
+ src/_pytest/pytester.py:1145:25: warning[possibly-unbound-attribute] Attribute `getcalls` on type `Unknown | <class 'reprec'>` is possibly unbound
- src/_pytest/pytester.py:1148:20: error[invalid-return-type] Return type does not match returned value: expected `HookRecorder`, found `@Todo(list literal element type) | <class 'reprec'>`
+ src/_pytest/pytester.py:1148:20: error[invalid-return-type] Return type does not match returned value: expected `HookRecorder`, found `Unknown | <class 'reprec'>`
+ src/_pytest/python.py:1096:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `Unknown | list[Unknown | str | _HiddenParam]`
+ src/_pytest/terminal.py:1553:23: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, str, int | None, str]`, found `tuple[int, @Todo(starred expression)]`
+ testing/python/metafunc.py:356:50: error[invalid-argument-type] Argument is incorrect: Expected `Config | None`, found `Any | MockConfig`
+ testing/python/metafunc.py:594:17: error[invalid-argument-type] Argument is incorrect: Expected `Config | None`, found `Any | MockConfig`
+ testing/python/metafunc.py:628:67: error[invalid-argument-type] Argument is incorrect: Expected `Config | None`, found `Any | MockConfig`
+ testing/python/metafunc.py:657:17: error[invalid-argument-type] Argument is incorrect: Expected `Config | None`, found `Any | MockConfig`
+ testing/test_assertrewrite.py:140:28: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `tuple[Literal[3], Literal[0]]` with `Unknown | tuple[Unknown | int, Unknown | int] | tuple[Unknown | int | None, Unknown | int | None]`
+ testing/test_assertrewrite.py:140:38: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | tuple[Unknown | int, Unknown | int] | tuple[Unknown | int | None, Unknown | int | None]` with `tuple[Literal[6], Literal[3]]`
- Found 473 diagnostics
+ Found 482 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
+ dragonchain/transaction_processor/level_1_actions_utest.py:132:38: error[invalid-argument-type] Argument to function `create_block` is incorrect: Expected `list[TransactionModel]`, found `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
- Found 301 diagnostics
+ Found 302 diagnostics

optuna (https://github.com/optuna/optuna)
- tests/importance_tests/fanova_tests/test_tree.py:117:10: error[unsupported-operator] Operator `-` is unsupported between objects of type `list[@Todo(list literal element type)]` and `floating[Any]`
+ tests/importance_tests/fanova_tests/test_tree.py:117:10: error[unsupported-operator] Operator `-` is unsupported between objects of type `list[Unknown]` and `floating[Any]`
- tests/test_cli.py:1218:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1219:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1220:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params"]` on object of type `list[@Todo(list literal element type)]`
+ tests/test_cli.py:1218:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
+ tests/test_cli.py:1219:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
+ tests/test_cli.py:1220:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
- tests/test_cli.py:1274:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1275:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params_x"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1276:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params_y"]` on object of type `list[@Todo(list literal element type)]`
+ tests/test_cli.py:1274:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
+ tests/test_cli.py:1275:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["params_x"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
+ tests/test_cli.py:1276:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["params_y"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
- tests/test_cli.py:1314:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
- tests/test_cli.py:1315:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["params"]` on object of type `list[@Todo(list literal element type)]`
+ tests/test_cli.py:1314:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
+ tests/test_cli.py:1315:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["params"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
- tests/test_cli.py:1352:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(list literal element type), (s: slice[Any, Any, Any], /) -> list[@Todo(list literal element type)]]` cannot be called with key of type `Literal["number"]` on object of type `list[@Todo(list literal element type)]`
+ tests/test_cli.py:1352:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)], (s: slice[Any, Any, Any], /) -> list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]]` cannot be called with key of type `Literal["number"]` on object of type `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/gen_test.py:955:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tornado/test/httpclient_test.py:521:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown, Unknown].__setitem__(key: Unknown, value: Unknown, /) -> None) | (bound method HTTPHeaders.__setitem__(name: str, value: str) -> None)` cannot be called with a key of type `Literal["User-Agent"]` and a value of type `Unknown | str | bytes` on object of type `Unknown | dict[Unknown, Unknown] | HTTPHeaders`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_misc.py:327:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[@Todo(list literal element type)]`
+ tests/test_misc.py:327:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[Unknown]`
- tests/test_misc.py:330:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[@Todo(list literal element type)]`
+ tests/test_misc.py:330:41: error[invalid-argument-type] Argument to function `booltostr` is incorrect: Expected `bool`, found `list[Unknown | str]`

spack (https://github.com/spack/spack)
- lib/spack/spack/ci/__init__.py:1138:9: error[unresolved-attribute] Type `list[@Todo(list literal element type)]` has no attribute `extent`
+ lib/spack/spack/ci/__init__.py:1138:9: error[unresolved-attribute] Type `list[Unknown]` has no attribute `extent`
- lib/spack/spack/config.py:1073:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `bytes`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `tuple[object, ...]`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `tuple[str, ...]`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `tuple[str, ...]`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `bytes`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `tuple[str, ...]`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/_compat.py:139:53: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `tuple[str, ...]`, found `Unknown | int`
+ lib/spack/spack/vendor/attr/validators.py:186:31: error[unresolved-attribute] Type `(Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = int) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = int) -> Match[bytes] | None]) & ~AlwaysFalsy` has no attribute `__name__`
- lib/spack/spack/vendor/six.py:982:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to attribute `submodule_search_locations` on type `ModuleSpec | None`
+ lib/spack/spack/vendor/six.py:982:5: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `submodule_search_locations` on type `ModuleSpec | None`
- Found 7520 diagnostics
+ Found 7529 diagnostics

artigraph (https://github.com/artigraph/artigraph)
- tests/arti/artifacts/test_artifact.py:135:27: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Annotation, ...]`, found `list[@Todo(list literal element type)]`
+ tests/arti/artifacts/test_artifact.py:135:27: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Annotation, ...]`, found `list[Unknown | Ann2]`
- tests/arti/artifacts/test_artifact.py:135:53: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Statistic, ...]`, found `list[@Todo(list literal element type)]`
+ tests/arti/artifacts/test_artifact.py:135:53: error[invalid-argument-type] Argument is incorrect: Expected `tuple[Statistic, ...]`, found `list[Unknown | Stat2]`
- tests/arti/internal/test_models.py:102:16: error[invalid-argument-type] Argument is incorrect: Expected `tuple[int, ...]`, found `list[@Todo(list literal element type)]`
+ tests/arti/internal/test_models.py:102:16: error[invalid-argument-type] Argument is incorrect: Expected `tuple[int, ...]`, found `list[Unknown | int]`
- tests/arti/types/test_types.py:100:51: error[invalid-argument-type] Argument is incorrect: Expected `frozenset[Any]`, found `frozenset[@Todo(set literal element type)] | list[@Todo(set literal element type)] | tuple[@Todo(set literal element type), ...]`
+ tests/arti/types/test_types.py:100:51: error[invalid-argument-type] Argument is incorrect: Expected `frozenset[Any]`, found `frozenset[Unknown | float] | list[Unknown | float] | tuple[Unknown | float, ...]`

poetry (https://github.com/python-poetry/poetry)
+ tests/puzzle/test_solver.py:614:9: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[Literal["a", "b", "download-package", "install-package"]]`
- Found 934 diagnostics
+ Found 935 diagnostics

antidote (https://github.com/Finistere/antidote)
- tests/core/test_wiring.py:152:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 310 diagnostics
+ Found 309 diagnostics

Expression (https://github.com/cognitedata/Expression)
+ tests/test_map.py:142:34: error[invalid-argument-type] Argument to bound method `try_get_value` is incorrect: Expected `list[Literal[1, 2]]`, found `list[int]`
+ tests/test_map.py:143:34: error[invalid-argument-type] Argument to bound method `try_get_value` is incorrect: Expected `list[Literal[1, 2]]`, found `list[int]`
+ tests/test_map.py:144:34: error[invalid-argument-type] Argument to bound method `try_get_value` is incorrect: Expected `list[Literal[1, 2]]`, found `list[int]`
- Found 227 diagnostics
+ Found 230 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ comtypes/server/automation.py:38:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[1]`
- comtypes/test/test_w_getopt.py:17:37: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
+ comtypes/test/test_w_getopt.py:17:37: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown | str]`
- comtypes/test/test_w_getopt.py:24:59: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
+ comtypes/test/test_w_getopt.py:24:59: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown | str]`
- comtypes/test/test_w_getopt.py:29:38: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[@Todo(list literal element type)]`
+ comtypes/test/test_w_getopt.py:29:38: error[invalid-argument-type] Argument to function `w_getopt` is incorrect: Expected `str`, found `list[Unknown | str]`
- Found 401 diagnostics
+ Found 402 diagnostics

vision (https://github.com/pytorch/vision)
- setup.py:291:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[@Todo(list literal element type)]` is possibly unbound
+ setup.py:291:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[Unknown]` is possibly unbound
- setup.py:292:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[@Todo(list literal element type)]` is possibly unbound
+ setup.py:292:20: warning[possibly-unbound-attribute] Attribute `copy` on type `Unknown | str | list[str] | list[Unknown]` is possibly unbound
- setup.py:416:32: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[@Todo(list literal element type)]` and `Unknown | str | list[str] | list[@Todo(list literal element type)]`
- setup.py:506:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[@Todo(list literal element type)]` and `Unknown | str | list[str] | list[@Todo(list literal element type)]`
+ setup.py:416:32: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown | str]` and `Unknown | str | list[str] | list[Unknown]`
+ setup.py:506:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `Unknown | str | list[str] | list[Unknown]`
- test/test_transforms.py:1258:34: error[invalid-argument-type] Argument to function `rotate` is incorrect: Expected `list[int | float] | None`, found `tuple[@Todo(list literal element type), ...]`
+ test/test_transforms.py:1258:34: error[invalid-argument-type] Argument to function `rotate` is incorrect: Expected `list[int | float] | None`, found `tuple[Unknown | int, ...]`
- test/test_transforms.py:1929:57: error[invalid-argument-type] Argument to function `perspective` is incorrect: Expected `list[int | float] | None`, found `tuple[@Todo(list literal element type), ...]`
+ test/test_transforms.py:1929:57: error[invalid-argument-type] Argument to function `perspective` is incorrect: Expected `list[int | float] | None`, found `tuple[Unknown | int, ...]`
+ test/test_transforms_v2.py:3693:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int] | None`, found `list[Unknown | int | float]`
+ test/test_transforms_v2.py:3699:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int] | None`, found `list[Unknown | float]`
- test/test_transforms_v2.py:3838:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `list[@Todo(list literal element type)]`
+ test/test_transforms_v2.py:3838:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `list[Unknown | int]`
- test/test_transforms_v2.py:4555:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[@Todo(list literal element type)]`
+ test/test_transforms_v2.py:4555:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[Unknown | int]`
- test/test_transforms_v2.py:4558:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[@Todo(list literal element type)]`
+ test/test_transforms_v2.py:4558:64: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `list[Unknown | int]`
+ test/test_transforms_v2.py:4714:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int]`, found `list[Unknown | int | float]`
+ test/test_transforms_v2.py:4720:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Sequence[int]`, found `list[Unknown | float]`
+ test/test_transforms_v2.py:6323:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | Sequence[int | float] | None`, found `object`
+ test/test_transforms_v2.py:6486:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[int | float]`, found `Unknown | None | int | list[Unknown | int]`
+ test/test_transforms_v2.py:6763:40: warning[possibly-unbound-attribute] Attribute `__name__` on type `Unknown | ((inpt: Unknown) -> Unknown) | ((inpt: Unknown) -> int) | ((pic, mode=None) -> Unknown) | ((inpt: Unknown, displacement: Unknown, interpolation: InterpolationMode | int = InterpolationMode, fill: Unknown = None) -> Unknown) | ((inpt: Unknown, num_output_channels: int = int) -> Unknown)` is possibly unbound
+ torchvision/transforms/functional.py:1205:9: error[invalid-assignment] Object of type `list[Unknown | (list[int | float] & Number) | float]` is not assignable to `list[int | float]`
+ torchvision/transforms/functional.py:1225:13: error[invalid-assignment] Object of type `list[Unknown | int | float]` is not assignable to `list[int] | None`
- torchvision/transforms/functional.py:1226:45: error[invalid-argument-type] Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `list[int] | list[@Todo(list literal element type)]`
+ torchvision/transforms/functional.py:1226:45: error[invalid-argument-type] Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `list[int] | None`
+ torchvision/transforms/v2/functional/_geometry.py:649:9: error[invalid-assignment] Object of type `list[Unknown | (list[int | float] & Number) | float]` is not assignable to `list[int | float]`
- Found 1458 diagnostics
+ Found 1468 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/test_connection.py:774:23: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
+ tests/test_connection.py:777:74: warning[possibly-unbound-attribute] Attribute `guc` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
+ tests/test_connection.py:789:30: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
+ tests/test_connection.py:792:74: warning[possibly-unbound-attribute] Attribute `guc` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
+ tests/test_connection_async.py:775:23: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
+ tests/test_connection_async.py:778:74: warning[possibly-unbound-attribute] Attribute `guc` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
+ tests/test_connection_async.py:790:37: warning[possibly-unbound-attribute] Attribute `name` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
+ tests/test_connection_async.py:794:59: warning[possibly-unbound-attribute] Attribute `guc` on type `Unknown | ParamDef | ParameterSet` is possibly unbound
- Found 693 diagnostics
+ Found 701 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ tests/hypotheses/test_hypotheses.py:282:33: error[invalid-argument-type] Argument to bound method `two_sample_ttest` is incorrect: Expected `str`, found `Unknown | str | int | None`
- tests/pandas/test_checks.py:353:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/pandas/test_dtypes.py:170:1: error[invalid-assignment] Object of type `list[tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | int]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | int | None]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | float]] | @Todo(starred expression) | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | complex]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | bool]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | bool | None]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], list[Unknown | str]] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], Unknown] | tuple[dict[@Todo(dict literal key type), @Todo(dict literal value type)], Series[Any]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`
+ tests/pandas/test_model.py:1643:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, Unknown] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
+ tests/pandas/test_model.py:1687:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, Unknown] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
+ tests/pandas/test_model.py:1714:59: error[invalid-argument-type] Argument to function `from_records` is incorrect: Expected `ndarray[Unknown, Unknown] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`, found `list[Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]]`
- tests/strategies/test_strategies.py:1004:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1586 diagnostics
+ Found 1589 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/backends/api.py:1736:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/computation/rolling.py:251:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/core/dataarray.py:450:21: error[invalid-assignment] Object of type `list[Unknown | (Any & ~DataArray & ~Top[Series[Any]] & ~DataFrame) | (ReprObject & ~Top[Series[Any]] & ~DataFrame)]` is not assignable to `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo(Support for `typing.TypeAlias`), Unknown]] | Mapping[Unknown, Unknown] | None`
- xarray/core/variable.py:1457:27: warning[possibly-unbound-attribute] Attribute `values` on type `(Unknown & ~str) | list[@Todo(list literal element type)]` is possibly unbound
+ xarray/core/variable.py:1457:27: warning[possibly-unbound-attribute] Attribute `values` on type `(Unknown & ~str) | list[Unknown | (Unknown & str)]` is possibly unbound
- xarray/core/variable.py:2207:32: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | Literal[False] | list[@Todo(list literal element type)]`
+ xarray/core/variable.py:2207:32: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | Literal[False] | list[Unknown | bool]`
- xarray/core/variable.py:2216:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | Literal[False] | list[@Todo(list literal element type)]`
+ xarray/core/variable.py:2216:46: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | Literal[False] | list[Unknown | bool]`
+ xarray/structure/combine.py:634:9: error[invalid-assignment] Object of type `list[Unknown | Sequence[str | DataArray | Index[Any] | None] | DataArray | None]` is not assignable to `Sequence[str | DataArray | Index[Any] | None] | DataArray | None`
- xarray/tests/test_backends.py:7417:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_concat.py:1412:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_concat.py:1417:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_dask.py:1173:56: error[invalid-argument-type] Argument to function `map_blocks` is incorrect: Expected `Mapping[str, Any] | None`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dask.py:1173:56: error[invalid-argument-type] Argument to function `map_blocks` is incorrect: Expected `Mapping[str, Any] | None`, found `list[Unknown | int]`
+ xarray/tests/test_dataarray.py:2582:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Sequence[SequenceNotStr[Hashable]]`, found `list[Unknown | Index[Any]]`
- xarray/tests/test_groupby.py:84:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/tests/test_indexing.py:336:54: error[unresolved-attribute] Type `object` has no attribute `kind`
+ xarray/tests/test_indexing.py:338:54: error[unresolved-attribute] Type `object` has no attribute `kind`
+ xarray/tests/test_variable.py:2697:53: warning[possibly-unbound-attribute] Attribute `dtype` on type `Unknown | list[Unknown | list[Unknown | int]] | DataFrame | float64 | str_` is possibly unbound

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/exchange/binance.py:52:37: error[invalid-argument-type] Invalid argument to key "order_props_in_contracts" with declared type `list[Literal["amount", "cost", "filled", "remaining"]]` on TypedDict `FtHas`: value of type `list[Unknown | str]`
+ freqtrade/exchange/exchange.py:161:37: error[invalid-argument-type] Invalid argument to key "order_props_in_contracts" with declared type `list[Literal["amount", "cost", "filled", "remaining"]]` on TypedDict `FtHas`: value of type `list[Unknown | str]`
- freqtrade/freqai/data_kitchen.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DataFrame, DataFrame]`, found `tuple[Unknown, Unknown | list[@Todo(list literal element type)]]`
+ freqtrade/freqai/data_kitchen.py:299:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DataFrame, DataFrame]`, found `tuple[Unknown, Unknown | list[Unknown]]`
+ freqtrade/optimize/optimize_reports/optimize_reports.py:492:...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2025-09-12 04:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=WallTime)

### Merging #20360 will **degrade performances by 26.86%**

<sub>Comparing <code>ibraheem/bidirectional-inference</code> (52abdb2) with <code>main</code> (05622ae)</sub>



### Summary

`❌ 3` regressions  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` large[sympy] `` | 41.5 s | 43.4 s | -4.18% |
| ❌ | `` medium[colour-science] `` | 7 s | 9.5 s | -26.86% |
| ❌ | `` medium[pandas] `` | 26.4 s | 30.9 s | -14.35% |


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-12 07:37_

---

_Comment by @github-actions[bot] on 2025-09-12 07:41_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 329 | 1 | 289 |
| `unsupported-operator` | 90 | 0 | 40 |
| `possibly-unbound-attribute` | 98 | 0 | 27 |
| `invalid-assignment` | 39 | 0 | 30 |
| `unused-ignore-comment` | 0 | 53 | 0 |
| `invalid-return-type` | 11 | 0 | 36 |
| `parameter-already-assigned` | 30 | 0 | 0 |
| `non-subscriptable` | 27 | 0 | 0 |
| `no-matching-overload` | 12 | 0 | 0 |
| `not-iterable` | 6 | 0 | 6 |
| `possibly-unbound-implicit-call` | 9 | 0 | 2 |
| `unresolved-attribute` | 10 | 0 | 1 |
| `type-assertion-failure` | 0 | 8 | 0 |
| `call-non-callable` | 6 | 0 | 0 |
| `index-out-of-bounds` | 4 | 0 | 0 |
| `division-by-zero` | 2 | 0 | 0 |
| `invalid-exception-caught` | 0 | 0 | 1 |
| `invalid-key` | 1 | 0 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| **Total** | **676** | **62** | **432** |

**[Full report with detailed diff](https://ibraheem-bidirectional-infer.ecosystem-663.pages.dev/diff)**


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:1 on 2025-09-12 07:53_

I'd love to see some tests for nested collections here -- e.g.

```py
x: list[list[int]] = [[], [42]]
y: list[tuple[str | int, ...]] = [(1, 2), ("foo", "bar"), ()]
z: list[tuple[list[Any], ...]] = [([],), ([1, 2], [3, 4]), (["foo"], ["bar"])]
```

It's obviously okay if they don't all pass right now -- we can add TODOs next to them

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:112 on 2025-09-12 08:06_

these all pass on `main` (https://play.ty.dev/9844d26d-f2b1-45ca-a6d9-4bd317376ffd) because we will always prefer the declared type over the inferred type for a symbol's "public type" as observed by an external scope. What doesn't pass on `main` is an mdtest like this, where all the `reveal_type` calls are in the same scope that the collection literals are created in:

```py
import typing

a: list[int] = [1, 2, 3]
reveal_type(a)  # revealed: list[int]

b: list[int | str] = [1, 2, 3]
reveal_type(b)  # revealed: list[int | str]

c: typing.List[int] = [1, 2, 3]
reveal_type(c)  # revealed: list[int]

d: list[typing.Any] = []
reveal_type(d)  # revealed: list[Any]

e: set[int] = {1, 2, 3}
reveal_type(e)  # revealed: set[int]

f: set[int | str] = {1, 2, 3}
reveal_type(f)  # revealed: set[int | str]

g: typing.Set[int] = {1, 2, 3}
reveal_type(g)  # revealed: set[int]
```

https://play.ty.dev/eadf4952-5909-41ee-b836-09f1bef7f6ac

That's because the thing that this PR is changing is that we now use the declared type as context to inform the inferred type that we produce for local-scope accesses of a variable

---

_@AlexWaygood reviewed on 2025-09-12 08:09_

Very nice!

---

_Comment by @AlexWaygood on 2025-09-12 08:11_

Would you be able to look through the mypy_primer diff and check that it's mostly as expected? If you use the ecosystem analyzer rich diff [here](https://ibraheem-bidirectional-infer.ecosystem-663.pages.dev/diff), it links directly to the line each new diagnostic is being emitted on

---

_@ibraheemdev reviewed on 2025-09-12 14:24_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:148 on 2025-09-12 14:24_

The error message here is slightly odd, I would expect it to be "Object of type `list[Literal[1, 2, 3]]` is not...". Should we infer the RHS without a type context if it's not assignable to the declared type?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5274 on 2025-09-12 21:18_

Even in the short term (e.g. for the beta) we'll want to improve this to `list[Unknown | Literal[1, 2, 3]]` instead of the todo type. That obviously doesn't necessarily have to be done in the same PR, I only mention it because it suggests that we might want to organize the code in such a way that "inference with a context" and "inference without a context" are less separated into different paths, because we'll need the inferred types of the elements either way. (But this can obviously also be changed in a future PR.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:148 on 2025-09-12 21:26_

Yes, probably if any of the inferred literal types are not subtypes of the context element type, we should infer without regard for the context, in order to have a more intuitive error message.

---

_@carljm approved on 2025-09-12 21:29_

Looks like a good first step to me!

---

_Comment by @carljm on 2025-09-12 21:32_

Oh, I do think some analysis of the ecosystem report here would be good, if there are any common causes of false positives we should prioritize addressing next. I did check the conformance suite report -- the changes are both good.

---

_Comment by @ibraheemdev on 2025-09-12 22:54_

I ended up rewriting most of the code to support better inference for unannotated collection literals as well. We now infer the type of a collection literal based on the inferred type of each element, as well as the type annotation if provided, or `Unknown` if not. For example,

```py
x = [1, 2, 3] # revealed: list[Unknown | Literal[1, 2, 3]]
x: list[int] = [1, 2, 3] # revealed: list[int]
```

---

_Renamed from "[ty] Infer more precise types for annotated collection literals " to "[ty] Infer more precise types for collection literals " by @ibraheemdev on 2025-09-12 22:55_

---

_@ibraheemdev reviewed on 2025-09-12 23:03_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:110 on 2025-09-12 23:03_

@dcreager is this something that you expect the new constraint solver to be able to simplify?

---

_Comment by @codspeed-hq[bot] on 2025-09-12 23:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=Instrumentation)

### Merging #20360 will **degrade performances by 8.1%**

<sub>Comparing <code>ibraheem/bidirectional-inference</code> (52abdb2) with <code>main</code> (05622ae)</sub>



### Summary

`❌ 2` regressions  
`✅ 41` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidirectional-inference?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` attrs `` | 364 ms | 392.1 ms | -7.16% |
| ❌ | `` hydra-zen `` | 722.1 ms | 785.7 ms | -8.1% |


---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-13 00:29_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-13 00:29_

---

_@ibraheemdev reviewed on 2025-09-15 18:21_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 18:21_

Without this, ty (seemingly) hangs when checking [files that contain large 2D list literals](https://github.com/colour-science/colour/blob/develop/colour/models/datasets/pointer_gamut.py#L52), even in release mode. This means `[1, 2, 3]` is inferred as `list[Unknown | int]` instead of `list[Unknown | Literal[1, 2, 3]]`.

We could potentially use a heuristic before falling back? 

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-15 18:29_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-15 18:29_

---

_@ibraheemdev reviewed on 2025-09-15 18:32_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 18:32_

This does mean we now fail to typecheck:
```py
x: list[LiteralString] = ["a", "b"]
```
Though we may be able to special case this.

---

_@ibraheemdev reviewed on 2025-09-15 18:59_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 18:59_

Hmm, this seems to still be problematic for large lists of tuples. The runtime of `ty check` on `rich` has increased from 0.6s to 4s, largely due to [this file](https://github.com/Textualize/rich/blob/master/rich/_cell_widths.py). Checking that file takes >3s in release mode, and >80s in debug mode, which is why CI is timing out.

Note that the initial commit, which was restricted to annotated collection literals, does not seem to introduce any regressions (as annotated collection literals are typically quite simple).

---

_@carljm reviewed on 2025-09-15 19:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 19:19_

> This does mean we now fail to typecheck:
> 
> ```python
> x: list[LiteralString] = ["a", "b"]
> ```

Seems like we should be able to use `LiteralString` instead of `str` as the fallback type for string literals?

> this seems to still be problematic for large lists of tuples

We may need to disallow inferring precise literal types for tuple members when nested? It seems pyright does something like this: `(0, 0, 0)` is inferred as `tuple[Literal[0], Literal[0], Literal[0]]`, but as soon as it is even `[(0, 0, 0)]` it becomes `list[tuple[int, int, int]]`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 19:21_

I could also see potentially doing it at the union level, so that we simplify two tuples of literals to a single tuple of the common fallback type at each element position? This is related to https://github.com/astral-sh/ty/issues/493, but also seems like it might get complex more quickly.

It would also be OK to pare this PR back down to just the annotated case, if you want to get that landed before looking more deeply into this performance issue.

---

_@carljm reviewed on 2025-09-15 19:21_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-15 20:14_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-15 20:14_

---

_@ibraheemdev reviewed on 2025-09-15 21:19_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 21:19_

The tuple literal promotion seems to fix most of the hangs, but scipy seems to still be hanging in mypy-primer and the ecosystem-analyzer. Weirdly, mypy-primer passes for me locally, without a large regression compared to main, if I run the exact same command as CI, so I'm not exactly sure why it's hanging.

---

_@ibraheemdev reviewed on 2025-09-15 22:09_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 22:09_

I'm able to reproduce it now, I suspect that the new types introduced by this PR are exposing a combinatorial explosion bug somewhere else when checking scipy. From the backtrace it seems related to `Type::normalized_impl`.

---

_@carljm reviewed on 2025-09-15 22:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 22:19_

It's a surprise to get infinite hang from `Type::normalized_impl` without getting a stack overflow. But it may be possible. Or it may just be taking a very long time to normalize some deeply-nested type, but not actually be in an infinite loop?

Typically infinite loops in `Type::normalized_impl` should be caught by the `NormalizedVisitor`.

---

_@carljm reviewed on 2025-09-15 22:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 22:20_

Let me know (and post the repro instructions) if you'd like a second pair of eyes on this.

---

_@ibraheemdev reviewed on 2025-09-15 22:36_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 22:36_

It looks like the culprit is [this call to `enumerate`](https://github.com/scipy/scipy/blob/c5c862df0fbe70f1c0dbeb07c85c5d98ec348e96/scipy/optimize/_tstutils.py#L968). The inferred type of the argument after this PR is:
```py
list[
  Unknown
    | list[Unknown | (def fun1(x) -> Unknown) | list[Unknown | int] | float | int]
    | list[Unknown | (def fun1(x) -> Unknown) | list[Unknown | int | float] | float | int]
    | list[Unknown | (def fun2(x) -> Unknown) | list[Unknown | float] | int]
    | list[Unknown | (def fun3(x) -> Unknown) | list[Unknown | int] | int]
    | list[Unknown | (def fun3(x) -> Unknown) | list[Unknown | int | float] | int]
    | list[Unknown | (def fun4(x) -> Unknown) | list[Unknown | int] | int]
    | list[Unknown | (def fun4(x) -> Unknown) | list[Unknown | int | float] | int]
    | list[Unknown | (def fun5(x) -> Unknown) | list[Unknown | int] | int]
    | list[Unknown | (def fun6(x) -> Unknown) | list[Unknown | int | float] | int]
    | list[Unknown | (def fun7(x) -> Unknown) | list[Unknown | int] | int]
    | list[Unknown | (def fun8(x) -> Unknown) | list[Unknown | float | int] | float | int]
    | list[Unknown | (def fun9(x) -> Unknown) | list[Unknown | float | int] | float | int]
]
```

And the backtrace shows deeply nested calls to `Type::normalized_impl` inside `infer_call_expression`. Running `ty check scipy/optimize/_tstutils.py` from this PR takes 40s in release mode, and much longer in debug mode.

---

_@ibraheemdev reviewed on 2025-09-15 23:12_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 23:12_

I suppose promoting the function types to their `Callable` signature would also fix this?

---

_@carljm reviewed on 2025-09-15 23:37_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-15 23:37_

Not sure; it depends on where the runtime explosion in `normalized` is coming from. That type doesn't look unreasonably large. And abstracting the function types would shrink it some, but wouldn't reduce its nesting level at all. 

---

_@ibraheemdev reviewed on 2025-09-16 20:27_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5373 on 2025-09-16 20:27_

That seems to have fixed it.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-16 20:28_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-16 20:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:315 on 2025-09-16 22:50_

I would remove these TODOs; they now refer just to removing the `Unknown |` from un-annotated generic collection literals, and there are a lot more tests than just these that will change if/when we do that.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/nested.md`:323 on 2025-09-16 22:50_

Also these TODOs

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1144 on 2025-09-16 22:52_

Does this mean we still error on `x: list[LiteralString] = ["foo"]`? Can we add a test for that (with TODO if we still error on it)?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1139 on 2025-09-16 22:53_

Given the comment on `literal_fallback_instance` above, I think we could probably just add the `FunctionLiteral` fallback there and keep it to just the one method? Or did you try that and observe undesirable effects?

AFAIK `literal_fallback_instance` is just used for inference of generic classes from constructor calls, which is closely related to what we use `literal_annotation_type` for. So I don't see much semantic justification for separating them.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6006 on 2025-09-16 22:55_

Similar to above, maybe just keep it to one `PromoteLiterals` type mapping, unless there are strong reasons we need both?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5308 on 2025-09-16 23:02_

This will need some generalization (in a subsequent PR) to handle dict literals (with two type parameters).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5338 on 2025-09-16 23:05_

Should we have a TODO here for pushing down the nested context? I saw some related TODOs in tests.

---

_@carljm approved on 2025-09-16 23:07_

Looks great, thank you! Just a few nits inline, but I think it's ready to land.

---

_@ibraheemdev reviewed on 2025-09-17 00:36_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:1139 on 2025-09-17 00:36_

The reason I separated them is that the expected `literal_fallback_instance` for a `FunctionLiteral` would be `FunctionType`. Returning `Callable` is slightly different, and I thought that might be confusing, e.g. [see this branch](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types.rs#L1713) that I would expect to be able to group with the `literal_fallback_instance` branch above it, if `literal_fallback_instance` had an implementation for `Type::FunctionLiteral`.

I also wasn't sure if we want to change the semantics of generic class constructors, which is the one place we currently use `TypeMapping::PromoteLiteral`. If you think it's still fine to change the semantics of `TypeMapping::PromoteLiteral` instead of add a separate operation, I'm happy to do that.

---

_@carljm reviewed on 2025-09-17 00:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1139 on 2025-09-17 00:49_

I see, makes sense.

I do think we only need one `PromoteLiterals` mapping. The way that we promote literals should be consistent between generic class instantiations and generic collection inference (the latter is really just kind of a special case of the former), and I think, for that case, promoting functions to callable types is better than promoting them to `FunctionType`. (There could be an argument for "callable-type & types.FunctionType" as the best promotion and fallback type for functions, but that would require that we special-case around the overly-forgiving definition of `types.FunctionType.__call__` in typeshed; definitely something for another time.)

All that said, I see your point that `FunctionType` would also be a reasonable "instance fallback", and that we have at least one case where the fallback to `FunctionType` is what we'd want for functions. So I could see keeping two methods here, in which case a) I'd name them `literal_fallback_instance` and `literal_promotion_type` (I think the latter more clearly describes how it is used than "literal_annotation_type"), and b) I'd probably go ahead and add `FunctionType` fallback to the former as well, and clean up the spot you linked (though that's maybe scope creep for this PR and could be done separately.)

---

_@ibraheemdev reviewed on 2025-09-17 02:44_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5338 on 2025-09-17 02:44_

I hadn't considered that supporting nested context was as simple as just passing the inner type down here! I implemented that and removed the TODOs.

---

_@ibraheemdev reviewed on 2025-09-17 02:45_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:5263 on 2025-09-17 02:45_

I'm a little surprised we don't follow this pattern anywhere else for type-checking a tuple, is there a better way of doing this?

---

_@ibraheemdev reviewed on 2025-09-17 03:21_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:1144 on 2025-09-17 03:21_

Yeah, I'm a little hesitant about `LiteralString` being the default promotion type, because I think this is actually a bigger issue with unconditionally promoting the types of the RHS. For example, `x: list[Literal["a"]] = ["a"]` should also type-check (and mypy and pyright both handle this correctly). I think to make this work properly we would need to thread the type-context down to the literal promotions visitor, which seems non-trivial. I left a test with a TODO for now.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5263 on 2025-09-17 17:10_

I don't think we currently have any cases where we need to check a tuple spec against a known fixed set of elements; we are generally comparing a full tuple spec to another full tuple spec (e.g. because the RHS is another tuple type). So in a sense this scenario is kind of the `Tuple::Fixed` branch of `VariableLengthTuple::has_relation_to_impl` (or maybe it's the `Tuple::Variable` branch of `FixedLengthTuple::has_relation_to_impl`?) -- but even in those cases we handle it a bit differently, we don't ever create a single iterator where we repeat variadic elements to stretch to a certain length.

I think the way you are doing this here makes sense. I do think there'd be an argument for encapsulating it as a new method `Tuple::elements_as_length` (maybe there's a better name?), which would make this method shorter and maybe clearer. But we could also wait to do that until/unless we have a second need for it.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:142 on 2025-09-17 17:13_

Have you done a review of the ecosystem analyzer results? It's good to do in general just to catch bugs, but specifically it would be good to know if this is actually coming up in the ecosystem, to get a sense of how much we should prioritize fixing it.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1144 on 2025-09-17 17:22_

Makes sense.

> I think to make this work properly we would need to thread the type-context down to the literal promotions visitor

I guess this is because we might want to promote some literals but not others, e.g. if we have `x: list[int | Literal["a"]] = ["a", <massive list of integer literals>]`?

It seems like the simpler cases could be handled via a heuristic of "if annotated element type is a literal, don't promote literals on the RHS", but that wouldn't handle the mixed union case well.

---

_@carljm approved on 2025-09-17 17:22_

---

_@AlexWaygood reviewed on 2025-09-17 17:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5263 on 2025-09-17 17:40_

This looks to me very much like the code we have in `VariableLengthTuple::resize()`. You might be able to avoid `match`ing on the inner tuple altogether by just using `Tuple::resize()`.

---

_@ibraheemdev reviewed on 2025-09-17 19:53_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:1144 on 2025-09-17 19:53_

> I guess this is because we might want to promote some literals but not others, e.g. if we have `x: list[int | Literal["a"]] = ["a", <massive list of integer literals>]`?

I think there are more straightforwards examples than the union case, e.g. `x: list[tuple[Literal[1]]] = [(1,)]`.

One of the issues is that we promote the literals of the RHS _recursively_ when inferring the elements of a list/set, because we currently infer `(1,)` to be `tuple[Literal[1]]`, but `[(1,)]` to be `list[tuple[int]]`. That means the logic for any heuristic would need to go in the type-mapping visitor, which would then need access to the type-context.

This is actually already an issue in the other place we use `PromoteLiterals`:
```py
class X[T]:
    def __init__(self, x: T):
        self.x = x

# ty: Object of type `tuple[X[int]]` is not assignable to `tuple[X[Literal[1]]]`
x: tuple[X[Literal[1]]] = (X(1), )
```

So i think the bug is somewhat unrelated to the changes here.

---

_Comment by @ibraheemdev on 2025-09-17 20:33_

I went through the ecosystem report, and most of the changes seem to be cases where we are able to correctly catch more type errors.

There were one or two cases where the literal promotion issue came up, but as discussed I think that can be addressed in a separate PR.

There were also a couple instances where lists were being used as heterogenous types, e.g.,
```py
def y(a: int, b: int, c: str): ...
y(*[1, 2, "3"])
``` 

pyright actually doesn't error on this, because it infers `list[Unknown]`, but we infer `list[Unknown | int | str]`, and so error.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-09-17 20:35_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-09-17 20:35_

---

_Comment by @carljm on 2025-09-17 21:55_

> pyright actually doesn't error on this, because it infers `list[Unknown]`

Interesting that pyright gives up in that situation. Mypy infers `list[object]` and also errors.

---

_Merged by @ibraheemdev on 2025-09-17 22:51_

---

_Closed by @ibraheemdev on 2025-09-17 22:51_

---

_Branch deleted on 2025-09-17 22:51_

---

_@carljm reviewed on 2025-09-18 00:22_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1144 on 2025-09-18 00:22_

https://github.com/astral-sh/ty/issues/1198

---
