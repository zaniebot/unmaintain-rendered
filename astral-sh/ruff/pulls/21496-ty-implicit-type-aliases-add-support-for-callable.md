```yaml
number: 21496
title: "[ty] Implicit type aliases: Add support for `Callable`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/implicit-type-aliases-callable
created_at: 2025-11-17T08:17:37Z
updated_at: 2025-11-18T08:06:07Z
url: https://github.com/astral-sh/ruff/pull/21496
synced_at: 2026-01-12T15:57:25Z
```

# [ty] Implicit type aliases: Add support for `Callable`

---

_@sharkdp_

## Summary

Add support for `Callable` special forms in implicit type aliases.

## Typing conformance

Four new tests are passing

## Ecosystem impact

* All of the `invalid-type-form` errors are from libraries that use `mypy_extensions` and do something like `Callable[[NamedArg("x", str)], int]`.
* A handful of new false positives because we do not support generic specializations of implicit type aliases, yet. But other
* Everything else looks like true positives or known limitations

## Test Plan

New Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-11-17 08:17_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-17 08:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:937 on 2025-11-17 08:18_

All of the code here has just been extracted into a new function. Nothing changed.

---

_@sharkdp reviewed on 2025-11-17 08:18_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 08:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-17 08:25:46.292225605 +0000
+++ new-output.txt	2025-11-17 08:25:49.741221166 +0000
@@ -9,23 +9,19 @@
 aliases_explicit.py:52:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 aliases_explicit.py:53:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
 aliases_explicit.py:54:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
-aliases_explicit.py:55:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> int`
 aliases_explicit.py:56:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, /) -> str`
 aliases_explicit.py:57:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, str, /) -> None`
 aliases_explicit.py:59:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
-aliases_explicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
 aliases_explicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_implicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 aliases_implicit.py:64:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...] | list[str]`
 aliases_implicit.py:65:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int, int, int, str]`
-aliases_implicit.py:66:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> int`
 aliases_implicit.py:67:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, /) -> str`
 aliases_implicit.py:68:5: error[type-assertion-failure] Argument does not have asserted type `(int, str, str, /) -> None`
 aliases_implicit.py:70:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
 aliases_implicit.py:71:5: error[type-assertion-failure] Argument does not have asserted type `list[bool]`
-aliases_implicit.py:72:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[<class 'int'> | Unknown]` is not allowed in a type expression
@@ -993,5 +989,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 995 diagnostics
+Found 991 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-17 08:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ mypy_primer/git_utils.py:66:56: error[invalid-argument-type] Argument to function `get_revision_for_revision_or_date` is incorrect: Expected `str`, found `@Todo | (str & ~(() -> object)) | (((Path, /) -> Awaitable[str]) & ~(() -> object))`
- Found 3 diagnostics
+ Found 4 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ src/attrs/__init__.pyi:67:21: error[unsupported-operator] Operator `|` is unsupported between objects of type `GenericAlias` and `<class 'Sequence[@Todo]'>`
+ tests/test_annotations.py:262:17: error[no-matching-overload] No overload of function `attrib` matches arguments
+ tests/test_annotations.py:333:16: error[no-matching-overload] No overload of function `pipe` matches arguments
+ tests/test_annotations.py:388:16: error[no-matching-overload] No overload of function `optional` matches arguments
+ tests/test_converters.py:171:24: error[call-non-callable] Object of type `Converter[Any, Any]` is not callable
+ tests/test_converters.py:171:33: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 3
+ tests/test_converters.py:239:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:240:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:309:24: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:319:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:320:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
- Found 583 diagnostics
+ Found 594 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/client.py:4114:9: error[invalid-assignment] Object of type `dict[Unknown, Any | None]` is not assignable to `dict[bytes | str | memoryview[int], (dict[str, str], /) -> Awaitable[None]]`
- Found 24 diagnostics
+ Found 25 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/directives.py:298:5: error[invalid-assignment] Object of type `list[Unknown | ~str]` is not assignable to `((...) -> None) | str | list[((...) -> None) | str] | None`
+ lib/spack/spack/directives.py:299:37: error[not-iterable] Object of type `((...) -> None) | str | list[((...) -> None) | str] | None` may not be iterable
+ lib/spack/spack/directives.py:321:26: error[not-iterable] Object of type `((...) -> None) | str | list[((...) -> None) | str] | None` may not be iterable
+ lib/spack/spack/directives.py:322:9: error[call-non-callable] Object of type `str` is not callable
+ lib/spack/spack/llnl/util/lock.py:754:41: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 3
- lib/spack/spack/vendor/attr/__init__.pyi:45:33: error[unresolved-reference] Name `Attribute` used when not defined
- lib/spack/spack/vendor/attr/__init__.pyi:47:25: error[unresolved-reference] Name `Attribute` used when not defined
- lib/spack/spack/vendor/attr/__init__.pyi:50:33: error[unresolved-reference] Name `Attribute` used when not defined
- Found 7906 diagnostics
+ Found 7908 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/pkg_resources/__init__.py:2744:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/pip/_vendor/rich/_log_render.py:62:59: error[invalid-argument-type] Argument to bound method `strftime` is incorrect: Expected `str`, found `(str & ~(() -> object)) | (((datetime, /) -> Text) & ~(() -> object)) | (Unknown & ~(() -> object))`
+ src/pip/_vendor/rich/text.py:624:51: error[invalid-argument-type] Argument is incorrect: Expected `str | Style`, found `(@Todo & ~None) | (((str, /) -> str | Style | None) & ~AlwaysFalsy & ~(() -> object)) | (str & ~AlwaysFalsy & ~(() -> object)) | (Style & ~AlwaysFalsy & ~(() -> object))`
- src/pip/_vendor/truststore/_api.py:31:69: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`

websockets (https://github.com/aaugustin/websockets)
+ src/websockets/legacy/server.py:634:37: error[invalid-argument-type] Argument to bound method `update` is incorrect: Expected `Mapping[str, str] | Iterable[tuple[str, str]] | SupportsKeysAndGetItem`, found `(Mapping[str, str] & ~(() -> object)) | (Iterable[tuple[str, str]] & ~(() -> object)) | (SupportsKeysAndGetItem & ~(() -> object)) | (((str, Headers, /) -> Mapping[str, str] | Iterable[tuple[str, str]] | SupportsKeysAndGetItem) & ~(() -> object)) | (@Todo & ~None)`
- Found 40 diagnostics
+ Found 41 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/type/definition.py:388:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/type/definition.py:390:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/type/definition.py:392:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/type/definition.py:1333:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 412 diagnostics
+ Found 408 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_core/actions/invocation.py:155:28: error[invalid-argument-type] Argument to function `is_async_fn` is incorrect: Expected `((...) -> @Todo) | None`, found `object`
+ kopf/_core/engines/admission.py:267:40: error[call-non-callable] Object of type `object` is not callable
+ kopf/_core/intents/callbacks.py:40:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:41:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:42:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:43:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:44:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:45:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:46:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:47:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:48:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:55:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:56:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:57:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:58:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:59:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:60:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:61:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:62:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:63:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:64:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:65:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:66:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:67:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:68:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:69:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:76:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:77:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:78:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:79:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:80:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:81:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:82:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:83:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:84:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:85:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:86:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:87:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:88:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:89:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:90:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:91:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:92:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:99:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:100:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:101:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:102:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:103:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:104:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:105:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:106:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:107:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:108:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:109:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:110:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:111:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:112:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:113:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:114:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:115:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:116:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:117:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:118:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:119:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:120:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:127:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:128:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:129:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:130:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:131:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:132:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:133:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:134:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:135:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:136:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:137:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:138:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:139:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:140:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:141:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:142:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:143:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:144:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:145:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:146:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:147:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:154:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:155:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:156:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:157:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:158:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:159:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:160:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:161:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:162:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:163:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:164:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:165:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:166:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:167:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:168:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:169:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:170:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:171:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:172:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:179:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:180:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:181:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:182:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:183:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:184:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:185:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:186:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:187:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:188:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:189:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:190:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:191:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:192:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:193:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:194:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:201:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:202:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:203:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:204:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:205:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:206:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:207:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:208:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:209:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:210:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:211:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:212:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:213:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:214:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:215:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:216:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:217:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:218:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:219:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:220:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:227:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:228:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:229:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:230:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:231:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:232:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:233:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:234:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:235:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:236:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:237:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:238:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:239:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:240:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:241:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:242:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/callbacks.py:243:13: error[invalid-type-form] Function calls are not allowed in type expressions
+ kopf/_core/intents/registries.py:276:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_via_pykube(*, logger: @Todo, **_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:285:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_via_client(*, logger: @Todo, **_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:297:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_with_kubeconfig(**_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:306:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_with_service_account(**_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:559:12: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20
+ kopf/_core/reactor/subhandling.py:72:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `object`
- Found 60 diagnostics
+ Found 224 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_code/source.py:40:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 440 diagnostics
+ Found 439 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/bounce_lib.py:45:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/bounce_lib.py:49:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/bounce_lib.py:50:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/bounce_lib.py:51:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/bounce_lib.py:52:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/check_services_replication_tools.py:59:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/check_services_replication_tools.py:63:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/check_services_replication_tools.py:67:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/check_services_replication_tools.py:68:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/check_services_replication_tools.py:153:38: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4
+ paasta_tools/check_services_replication_tools.py:154:21: error[unknown-argument] Argument `instance_config` does not match any known parameter
+ paasta_tools/check_services_replication_tools.py:155:21: error[unknown-argument] Argument `pods_by_service_instance` does not match any known parameter
+ paasta_tools/check_services_replication_tools.py:156:21: error[unknown-argument] Argument `replication_checker` does not match any known parameter
+ paasta_tools/check_services_replication_tools.py:157:21: error[unknown-argument] Argument `dry_run` does not match any known parameter
+ paasta_tools/cli/cmds/status.py:126:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/cmds/status.py:130:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/cmds/status.py:131:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/cmds/status.py:132:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/cmds/status.py:133:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/cmds/status.py:134:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:601:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:605:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:606:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:607:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:614:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:618:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:619:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:620:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:621:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:628:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:632:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:633:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:634:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:641:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:645:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:646:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:647:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:648:9: error[invalid-type-form] Function calls are not allowed in type expressions
+ paasta_tools/cli/utils.py:665:33: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, /) -> list[tuple[str, str]]`, found `None`
+ paasta_tools/cli/utils.py:665:39: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, /) -> InstanceConfig`, found `None`
+ paasta_tools/cli/utils.py:696:44: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, /) -> list[tuple[str, str]]`, found `None`
+ paasta_tools/cli/utils.py:696:50: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, /) -> LongRunningServiceConfig`, found `None`
+ paasta_tools/cli/utils.py:749:12: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4, 5
+ paasta_tools/cli/utils.py:750:9: error[unknown-argument] Argument `service` does not match any known parameter
+ paasta_tools/cli/utils.py:751:9: error[unknown-argument] Argument `instance` does not match any known parameter
+ paasta_tools/cli/utils.py:752:9: error[unknown-argument] Argument `cluster` does not match any known parameter
+ paasta_tools/cli/utils.py:753:9: error[unknown-argument] Argument `load_deployments` does not match any known parameter
+ paasta_tools/cli/utils.py:754:9: error[unknown-argument] Argument `soa_dir` does not match any known parameter
+ paasta_tools/cli/utils.py:968:32: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4
+ paasta_tools/cli/utils.py:969:17: error[unknown-argument] Argument `service` does not match any known parameter
+ paasta_tools/cli/utils.py:970:17: error[unknown-argument] Argument `cluster` does not match any known parameter
+ paasta_tools/cli/utils.py:971:17: error[unknown-argument] Argument `soa_dir` does not match any known parameter
+ paasta_tools/cli/utils.py:972:17: error[unknown-argument] Argument `instance_type` does not match any known parameter
+ paasta_tools/cli/utils.py:977:23: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4, 5
+ paasta_tools/cli/utils.py:978:21: error[unknown-argument] Argument `service` does not match any known parameter
+ paasta_tools/cli/utils.py:979:21: error[unknown-argument] Argument `instance` does not match any known parameter
+ paasta_tools/cli/utils.py:980:21: error[unknown-argument] Argument `cluster` does not match any known parameter
+ paasta_tools/cli/utils.py:981:21: error[unknown-argument] Argument `soa_dir` does not match any known parameter
+ paasta_tools/cli/utils.py:982:21: error[unknown-argument] Argument `load_deployments` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:638:18: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4, 5
+ paasta_tools/instance/kubernetes.py:639:9: error[unknown-argument] Argument `service` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:640:9: error[unknown-argument] Argument `instance` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:641:9: error[unknown-argument] Argument `cluster` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:642:9: error[unknown-argument] Argument `soa_dir` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:643:9: error[unknown-argument] Argument `load_deployments` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1173:18: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4, 5
+ paasta_tools/instance/kubernetes.py:1174:9: error[unknown-argument] Argument `service` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1175:9: error[unknown-argument] Argument `instance` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1176:9: error[unknown-argument] Argument `cluster` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1177:9: error[unknown-argument] Argument `soa_dir` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1178:9: error[unknown-argument] Argument `load_deployments` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1353:18: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4, 5
+ paasta_tools/instance/kubernetes.py:1354:9: error[unknown-argument] Argument `service` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1355:9: error[unknown-argument] Argument `instance` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1356:9: error[unknown-argument] Argument `cluster` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1357:9: error[unknown-argument] Argument `soa_dir` does not match any known parameter
+ paasta_tools/instance/kubernetes.py:1358:9: error[unknown-argument] Argument `load_deployments` does not match any known parameter
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | LongRunningServiceConfig | ServiceNamespaceConfig | ... omitted 3 union elements`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `str`, found `str | LongRunningServiceConfig | ServiceNamespaceConfig | ... omitted 3 union elements`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `LongRunningServiceConfig`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `LongRunningServiceConfig`, found `str | LongRunningServiceConfig | ServiceNamespaceConfig | ... omitted 3 union elements`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `ServiceNamespaceConfig`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `ServiceNamespaceConfig`, found `str | LongRunningServiceConfig | ServiceNamespaceConfig | ... omitted 3 union elements`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `Future[Unknown]`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `Future[Unknown]`, found `str | LongRunningServiceConfig | ServiceNamespaceConfig | ... omitted 3 union elements`
- paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `bool`, found `str | @Todo | ServiceNamespaceConfig | Task[Unknown] | bool`
+ paasta_tools/instance/kubernetes.py:1393:13: error[invalid-argument-type] Argument to function `mesh_status` is incorrect: Expected `bool`, found `str | LongRunningServiceConfig | ServiceNamespaceConfig | ... omitted 3 union elements`
+ paasta_tools/metrics/metastatus_lib.py:591:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[typing.TypeVar], /) -> Sequence[typing.TypeVar]`
+ paasta_tools/metrics/metastatus_lib.py:606:9: error[invalid-assignment] Object of type `Sequence[typing.TypeVar]` is not assignable to `Sequence[_GenericNodeT@group_slaves_by_key_func]`
+ paasta_tools/metrics/metastatus_lib.py:606:35: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[typing.TypeVar]`, found `Sequence[_GenericNodeT@group_slaves_by_key_func]`
+ paasta_tools/metrics/metastatus_lib.py:608:36: error[no-matching-overload] No overload of function `__new__` matches arguments
+ paasta_tools/metrics/metastatus_lib.py:769:41: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `_GenericNodeT@filter_slaves`
+ paasta_tools/metrics/metastatus_lib.py:776:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[typing.TypeVar], /) -> Sequence[typing.TypeVar]`
+ paasta_tools/metrics/metastatus_lib.py:815:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[typing.TypeVar], /) -> Sequence[typing.TypeVar]`
+ paasta_tools/setup_kubernetes_cr.py:349:30: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4, 5
+ paasta_tools/setup_kubernetes_cr.py:350:21: error[unknown-argument] Argument `service` does not match any known parameter
+ paasta_tools/setup_kubernetes_cr.py:351:21: error[unknown-argument] Argument `instance` does not match any known parameter
+ paasta_tools/setup_kubernetes_cr.py:352:21: error[unknown-argument] Argument `cluster` does not match any known parameter
+ paasta_tools/setup_kubernetes_cr.py:353:21: error[unknown-argument] Argument `load_deployments` does not match any known parameter
+ paasta_tools/setup_kubernetes_cr.py:354:21: error[unknown-argument] Argument `soa_dir` does not match any known parameter
+ paasta_tools/utils.py:2916:42: error[invalid-argument-type] Argument to function `signal` is incorrect: Expected `((int, FrameType | None, /) -> Any) | int | None`, found `def signal_handler(signum: int, frame: FrameType) -> None`
+ paasta_tools/utils.py:2917:43: error[invalid-argument-type] Argument to function `signal` is incorrect: Expected `((int, FrameType | None, /) -> Any) | int | None`, found `def signal_handler(signum: int, frame: FrameType) -> None`
+ paasta_tools/utils.py:3513:58: error[invalid-argument-type] Argument to function `signal` is incorrect: Expected `((int, FrameType | None, /) -> Any) | int | None`, found `bound method Self@__enter__.handle_timeout(signum: int, frame: FrameType) -> None`
+ paasta_tools/utils.py:4139:47: error[invalid-argument-type] Argument to function `signal` is incorrect: Expected `((int, FrameType | None, /) -> Any) | int | None`, found `def _handle_timeout(signum: int, frame: FrameType) -> None`
- Found 991 diagnostics
+ Found 1085 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_utils_console.py:41:12: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `@Todo | None`
+ tests/test_utils_console.py:27:12: error[unresolved-attribute] Object of type `((...) -> None) & (() -> object)` has no attribute `__name__`
+ tests/test_utils_console.py:34:12: error[unresolved-attribute] Object of type `((...) -> None) & (() -> object)` has no attribute `__name__`
+ tests/test_utils_console.py:41:12: error[unresolved-attribute] Object of type `((...) -> None) | None` has no attribute `__name__`
- Found 1704 diagnostics
+ Found 1706 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/_exception_handler.py:59:34: error[invalid-await] `@Todo | Response | Awaitable[Response]` is not awaitable
+ starlette/_exception_handler.py:59:42: error[invalid-argument-type] Argument is incorrect: Expected `Request`, found `Request | WebSocket`
+ starlette/_exception_handler.py:59:42: error[invalid-argument-type] Argument is incorrect: Expected `WebSocket`, found `Request | WebSocket`
+ starlette/middleware/errors.py:176:38: error[invalid-await] `@Todo | Response | Awaitable[Response]` is not awaitable
+ starlette/middleware/errors.py:176:51: error[invalid-argument-type] Argument is incorrect: Expected `WebSocket`, found `Request`
- starlette/testclient.py:390:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- starlette/testclient.py:391:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_applications.py:133:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_applications.py:574:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `def _middleware_factory(app: @Todo, arg: str) -> @Todo`
- tests/test_applications.py:575:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `(@Todo, str, /) -> @Todo`
+ tests/test_applications.py:574:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `def _middleware_factory(app: (MutableMapping[str, Any], () -> Awaitable[MutableMapping[str, Any]], (MutableMapping[str, Any], /) -> Awaitable[None], /) -> Awaitable[None], arg: str) -> (MutableMapping[str, Any], () -> Awaitable[MutableMapping[str, Any]], (MutableMapping[str, Any], /) -> Awaitable[None], /) -> Awaitable[None]`
+ tests/test_applications.py:575:24: error[invalid-argument-type] Argument to bound method `add_middleware` is incorrect: Expected `_MiddlewareFactory[Unknown]`, found `((MutableMapping[str, Any], () -> Awaitable[MutableMapping[str, Any]], (MutableMapping[str, Any], /) -> Awaitable[None], /) -> Awaitable[None], str, /) -> (MutableMapping[str, Any], () -> Awaitable[MutableMapping[str, Any]], (MutableMapping[str, Any], /) -> Awaitable[None], /) -> Awaitable[None]`
- tests/test_exceptions.py:74:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_testclient.py:211:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_testclient.py:251:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_testclient.py:273:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 224 diagnostics
+ Found 222 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/_log_render.py:62:59: error[invalid-argument-type] Argument to bound method `strftime` is incorrect: Expected `str`, found `(str & ~(() -> object)) | (((datetime, /) -> Text) & ~(() -> object)) | (Unknown & ~(() -> object))`
+ rich/text.py:624:51: error[invalid-argument-type] Argument is incorrect: Expected `str | Style`, found `(@Todo & ~None) | (((str, /) -> str | Style | None) & ~AlwaysFalsy & ~(() -> object)) | (str & ~AlwaysFalsy & ~(() -> object)) | (Style & ~AlwaysFalsy & ~(() -> object))`
- Found 344 diagnostics
+ Found 346 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/checkmake.py:132:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_check/checkmake.py:187:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_decor/_nontype/_decordescriptor.py:153:9: error[invalid-argument-type] Argument to function `beartype_func` is incorrect: Argument type `((Any, /) -> Any) | None` does not satisfy upper bound `type | ((...) -> Any) | classmethod[Unknown, Unknown, Unknown] | property | staticmethod[Unknown, Unknown]` of type variable `BeartypeableT`
+ beartype/_decor/_nontype/_decordescriptor.py:268:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_decor/decorcache.py:74:16: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `BeartypeableT@beartype`
+ beartype/_decor/decorcache.py:130:5: error[invalid-assignment] Invalid subscript assignment with key of type `BeartypeConf` and value of type `def _beartype_confed(obj: BeartypeableT@beartype) -> BeartypeableT@beartype` on object of type `dict[BeartypeConf, (typing.TypeVar, /) -> typing.TypeVar]`
+ beartype/_decor/decorcache.py:133:12: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `def _beartype_confed(obj: BeartypeableT@beartype) -> BeartypeableT@beartype`
- beartype/_decor/decorcore.py:133:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_decor/decormain.py:93:20: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `BeartypeableT@beartype`
+ beartype/_decor/decormain.py:102:16: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `def _beartype_optimized[BeartypeableT](obj: BeartypeableT@_beartype_optimized) -> BeartypeableT@_beartype_optimized`
+ beartype/bite/kind/infercallable.py:562:18: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 499 diagnostics
+ Found 504 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/object_store.py:1717:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/object_store.py:1744:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/object_store.py:2082:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dulwich/web.py:732:51: error[invalid-argument-type] Argument is incorrect: Expected `StartResponse`, found `StartResponse | StartResponse`
+ dulwich/web.py:761:34: error[invalid-argument-type] Argument is incorrect: Expected `StartResponse`, found `StartResponse | StartResponse`
+ dulwich/web.py:786:34: error[invalid-argument-type] Argument is incorrect: Expected `StartResponse`, found `StartResponse | StartResponse`

pybind11 (https://github.com/pybind/pybind11)
- pybind11/setup_helpers.py:492:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pybind11/setup_helpers.py:500:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 213 diagnostics
+ Found 211 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
+ dedupe/datamodel.py:94:26: warning[redundant-cast] Value is already of type `(Any, Any, /) -> int | float | Sequence[int | float]`
+ dedupe/predicates.py:86:29: error[unresolved-attribute] Object of type `(Any, /) -> frozenset[str]` has no attribute `__name__`
+ dedupe/variables/base.py:100:56: error[unresolved-attribute] Object of type `(Any, Any, /) -> int | float` has no attribute `__name__`
- Found 50 diagnostics
+ Found 53 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/GithubIntegration.py:148:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 290 diagnostics
+ Found 289 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ inttests/test_messagequeue.py:89:30: error[invalid-argument-type] Argument to bound method `consume_messages` is incorrect: Expected `(dict[Unknown, Unknown], str, dict[str, str], ((bool, /) -> None) | None, bool, /) -> None`, found `CallbackMock`
+ inttests/test_messagequeue.py:94:30: error[invalid-argument-type] Ar

... (truncated 522 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-17 08:23_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-form` | 190 | 0 | 0 |
| `invalid-argument-type` | 135 | 0 | 21 |
| `unused-ignore-comment` | 1 | 69 | 0 |
| `unresolved-attribute` | 59 | 0 | 6 |
| `invalid-context-manager` | 0 | 0 | 42 |
| `unknown-argument` | 40 | 0 | 0 |
| `invalid-assignment` | 17 | 0 | 1 |
| `possibly-missing-attribute` | 11 | 1 | 0 |
| `missing-argument` | 10 | 0 | 0 |
| `no-matching-overload` | 10 | 0 | 0 |
| `call-non-callable` | 8 | 0 | 0 |
| `non-subscriptable` | 0 | 8 | 0 |
| `invalid-return-type` | 7 | 0 | 0 |
| `not-iterable` | 5 | 0 | 0 |
| `unsupported-operator` | 3 | 1 | 1 |
| `invalid-parameter-default` | 4 | 0 | 0 |
| `type-assertion-failure` | 4 | 0 | 0 |
| `unresolved-reference` | 0 | 3 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `redundant-cast` | 2 | 0 | 0 |
| `too-many-positional-arguments` | 2 | 0 | 0 |
| **Total** | **510** | **82** | **71** |

**[Full report with detailed diff](https://david-implicit-type-aliases-r1vv.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-type-aliases-r1vv.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-17 08:23_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-17 08:24_

---

_Comment by @codspeed-hq[bot] on 2025-11-17 08:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-callable?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21496 will **degrade performances by 4.91%**

<sub>Comparing <code>david/implicit-type-aliases-callable</code> (07d7b5c) with <code>main</code> (d9fc0f0)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-callable?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.5 s | 21.5 s | -4.91% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-callable?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @sharkdp on 2025-11-17 08:44_

---

_Review requested from @carljm by @sharkdp on 2025-11-17 08:44_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-17 08:44_

---

_Review requested from @dcreager by @sharkdp on 2025-11-17 08:44_

---

_@carljm approved on 2025-11-17 15:52_

🎉 

---

_Merged by @sharkdp on 2025-11-18 08:06_

---

_Closed by @sharkdp on 2025-11-18 08:06_

---

_Branch deleted on 2025-11-18 08:06_

---
