```yaml
number: 20219
title: "[ty] Add support for generic PEP695 type aliases"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - typing
  - ty
assignees: []
merged: true
base: main
head: ibraheem/generic-type-alias
created_at: 2025-09-03T21:07:28Z
updated_at: 2025-09-08T20:26:23Z
url: https://github.com/astral-sh/ruff/pull/20219
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Add support for generic PEP695 type aliases

---

_Pull request opened by @ibraheemdev on 2025-09-03 21:07_

## Summary

Adds support for generic PEP695 type aliases, e.g.,
```python
type A[T] = T
reveal_type(A[int]) # A[int]
```

Resolves https://github.com/astral-sh/ty/issues/677.

---

_Label `typing` added by @ibraheemdev on 2025-09-03 21:07_

---

_Review requested from @carljm by @ibraheemdev on 2025-09-03 21:07_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-09-03 21:07_

---

_Label `ty` added by @ibraheemdev on 2025-09-03 21:07_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-09-03 21:07_

---

_Review requested from @dcreager by @ibraheemdev on 2025-09-03 21:07_

---

_@ibraheemdev reviewed on 2025-09-03 21:09_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:1307 on 2025-09-03 21:09_

For example, I'm not sure if delegating to `value_type` here is correct, because `value_type` returns a `Type::NominalInstance` for the RHS of the alias. I'm not very familiar with the typing hierarchy, but that doesn't seem correct to me.

---

_@ibraheemdev reviewed on 2025-09-03 21:10_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/class.rs`:482 on 2025-09-03 21:10_

Quite an unfortunate name here.

---

_Comment by @github-actions[bot] on 2025-09-03 21:20_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-08 20:03:49.611822703 +0000
+++ new-output.txt	2025-09-08 20:03:52.598894420 +0000
@@ -1,5 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1227a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b416)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1267a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -50,25 +51,6 @@
 aliases_newtype.py:18:1: error[invalid-assignment] Object of type `NewType` is not assignable to `type`
 aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `NewType`
 aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 3, got 4
-aliases_type_statement.py:17:1: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `bit_count`
-aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
-aliases_type_statement.py:23:7: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `other_attrib`
-aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `typing.TypeAliasType`
-aliases_type_statement.py:37:22: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_type_statement.py:38:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_type_statement.py:39:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
-aliases_type_statement.py:39:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_type_statement.py:40:22: error[invalid-type-form] List comprehensions are not allowed in type expressions
-aliases_type_statement.py:41:22: error[invalid-type-form] Dict literals are not allowed in type expressions
-aliases_type_statement.py:42:22: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_type_statement.py:43:28: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_type_statement.py:44:22: error[invalid-type-form] `if` expressions are not allowed in type expressions
-aliases_type_statement.py:45:22: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
-aliases_type_statement.py:46:23: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
-aliases_type_statement.py:47:23: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_type_statement.py:48:23: error[invalid-type-form] Boolean operations are not allowed in type expressions
-aliases_type_statement.py:49:23: error[fstring-type-annotation] Type expressions cannot use f-strings
-aliases_type_statement.py:80:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
@@ -866,5 +848,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 867 diagnostics
+Found 849 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-03 21:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
+ src/async_utils/corofunc_cache.py:44:39: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/corofunc_cache.py:44:61: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/corofunc_cache.py:48:36: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/corofunc_cache.py:48:58: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/corofunc_cache.py:109:31: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/corofunc_cache.py:109:53: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/corofunc_cache.py:188:31: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/corofunc_cache.py:188:53: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/gen_transform.py:60:53: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/gen_transform.py:106:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:30:29: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:30:46: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:45:39: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:45:65: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:49:36: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:49:62: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:81:27: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:81:50: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:169:31: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:169:57: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:255:31: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ src/async_utils/task_cache.py:255:57: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
- Found 7 diagnostics
+ Found 29 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ tests/arti/internal/test_mappings.py:142:5: error[invalid-assignment] Object of type `Coord` is not assignable to attribute `z` on type `Unknown | int | Coord | TypedBox[Coord]`
+ tests/arti/internal/test_mappings.py:160:9: warning[possibly-unbound-attribute] Attribute `_dne` on type `Coord | TypedBox[Coord]` is possibly unbound
+ tests/arti/internal/test_mappings.py:162:9: error[invalid-assignment] Object of type `Unknown | Coord` is not assignable to attribute `_dne` on type `Coord | TypedBox[Coord]`
+ tests/arti/internal/test_mappings.py:172:5: error[invalid-assignment] Object of type `Unknown | Coord` is not assignable to attribute `coord` on type `Coord | TypedBox[Coord]`
+ tests/arti/internal/test_mappings.py:172:5: warning[possibly-unbound-attribute] Attribute `attribute` on type `Coord | TypedBox[Coord]` is possibly unbound
+ tests/arti/internal/test_mappings.py:172:5: warning[possibly-unbound-attribute] Attribute `child` on type `Coord | TypedBox[Coord]` is possibly unbound
- Found 133 diagnostics
+ Found 139 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/androidtv/entity.py:46:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/androidtv/entity.py:46:49: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/androidtv/entity.py:54:15: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/androidtv/entity.py:55:10: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/cast/media_player.py:94:11: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/cast/media_player.py:95:6: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/hassio/addon_manager.py:37:6: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/hassio/addon_manager.py:37:42: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/hassio/addon_manager.py:43:15: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/hassio/addon_manager.py:44:10: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/heos/media_player.py:144:16: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/heos/media_player.py:144:36: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/heos/media_player.py:147:25: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/heos/media_player.py:147:47: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/http/decorators.py:33:6: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:34:5: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:44:12: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:45:6: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:53:12: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:59:10: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:60:9: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:62:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:67:15: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/http/decorators.py:68:10: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/husqvarna_automower/entity.py:53:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/husqvarna_automower/entity.py:53:46: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/husqvarna_automower/entity.py:56:25: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/husqvarna_automower/entity.py:56:57: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/izone/climate.py:122:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/izone/climate.py:122:46: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/izone/climate.py:123:20: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/izone/climate.py:123:52: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/openhome/media_player.py:71:6: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/openhome/media_player.py:71:44: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/openhome/media_player.py:76:15: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/openhome/media_player.py:77:10: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:607:16: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:607:38: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:613:24: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:613:48: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:621:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:621:45: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:627:24: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:627:55: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:634:10: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:635:6: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:663:16: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:663:36: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:673:14: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:674:10: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:684:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:684:43: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:694:14: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:695:10: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/recorder/util.py:704:10: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/recorder/util.py:708:6: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/roku/helpers.py:31:16: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/roku/helpers.py:31:46: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/roku/helpers.py:35:15: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/roku/helpers.py:36:10: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+ homeassistant/components/sonos/helpers.py:46:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/sonos/helpers.py:46:40: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/sonos/helpers.py:52:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/sonos/helpers.py:52:40: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/sonos/helpers.py:57:16: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/sonos/helpers.py:57:40: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/sonos/helpers.py:60:26: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/components/sonos/helpers.py:60:52: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ homeassistant/helpers/dispatcher.py:72:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ homeassistant/helpers/dispatcher.py:119:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ homeassistant/helpers/dispatcher.py:234:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ homeassistant/helpers/singleton.py:68:32: error[invalid-await] `_Coro | _U@singleton` is not awaitable
+ homeassistant/helpers/singleton.py:83:12: error[invalid-return-type] Return type does not match returned value: expected `(_FuncType, /) -> _FuncType`, found `Overload[(func: (HomeAssistant, /) -> Coroutine[Any, Any, _T@singleton]) -> (HomeAssistant, /) -> Coroutine[Any, Any, _T@singleton], (func: (HomeAssistant, /) -> _U@singleton) -> (HomeAssistant, /) -> _U@singleton]`
- homeassistant/helpers/singleton.py:102:5: error[type-assertion-failure] Argument does not have asserted type `int`
- homeassistant/helpers/singleton.py:103:5: error[type-assertion-failure] Argument does not have asserted type `int`
- homeassistant/helpers/singleton.py:106:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 13345 diagnostics
+ Found 13415 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-09-04 11:30_

Not a full review, but the new `too-many-positional-arguments` diagnostics in the ecosystem diff indicate that there is some problem with `ParamSpec`s. For example:
```py
from typing import Callable

type ReturnsInt[**P] = Callable[P, int]

def call[**P](callable: ReturnsInt[P]) -> None:  # ty: too-many-positional-arguments
    pass
```

---

_@sharkdp reviewed on 2025-09-04 11:33_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:1 on 2025-09-04 11:33_

We also have `resources/mdtest/pep695_type_aliases.md`. Should these two files be merged?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:246 on 2025-09-04 18:27_

Did you consider keeping this a struct, and making just the `origin` field be an enum? Even if we need to add additional variants in the future, I can't imagine a generic alias not having a specialization, and it looks like a lot of the copied logic below is actually shared.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class_base.rs`:88 on 2025-09-04 18:30_

You can fold these match statements together like you do below in `display.rs`:

```suggestion
            Type::GenericAlias(GenericAlias::ClassLiteral(generic)) => {
                Some(Self::Class(ClassType::Generic(generic)))
            }
            Type::GenericAlias(GenericAlias::TypeAlias(_)) => None,
```

(Though if you revert `GenericAlias` back to being a struct per above, then you will need to keep this as two separate matches)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:1739 on 2025-09-04 18:32_

Is this spurious? I don't see any other changes to this method or the type being matched on that would require this

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8825 on 2025-09-04 18:33_

I'd suggest adding a `TODO` comment that the limitation to PEP 695 type aliases is temporary, and only because that's the only kind of type alias that we've attached a generic context to yet.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8889 on 2025-09-04 18:38_

I think the "keep `GenericAlias` as a struct" suggestion would simplify this a bit, too. You'd end up with a `GenericAliasOrigin` enum, which wraps either a class literal or a type alias, and you could update `infer_explicit_class_specialization` to take in that instead of a `ClassLiteral`. That would eliminate the need for this callback (and the two new methods here), since the two callbacks would be combined into a new `GenericAliasOrigin::apply_specialization` method.

---

_Comment by @ibraheemdev on 2025-09-04 18:46_

@sharkdp I believe that's https://github.com/astral-sh/ty/issues/157. All the generics infrastructure in this PR is reused from generic classes, which emit a similar error:

```py
class ReturnsInt[**P]:
    x: Callable[P, int]

def call[**P](callable: ReturnsInt[P]) -> None:  # ty: too-many-positional-arguments
    pass
````



---

_Review requested from @MichaReiser by @ibraheemdev on 2025-09-04 18:58_

---

_@dcreager reviewed on 2025-09-04 18:58_

Looking at how pyright [handles this](https://pyright-play.net/?enableExperimentalFeatures=true&code=MYGwhgzhAEDCDaAVAugLmgOiwKGwEwFMAzaAfSWQAph0EUBKVbaF6AJwIDcCwRSAXAJ4AHAtXq4O3XgJFjYEqTz5DRlBAEsAdv2QTcqgtAAaFaAF44FbIegBNC9Aj82uQiXIpKAD3SmGTKzsXMqyat767mSUguh2jMysSjKGMfrJKnKUxoohKVmm2rq50plq8UA), there might a simpler way to do this that punts on several of the questions we've had.

```py
class C[T]: ...

def _[T](c: C[T]):
    reveal_type(c)  # revealed: C[T@_]

reveal_type(C)  # revealed: type[C[Unknown]]
reveal_type(C[int])  # revealed: type[C[int]]


type X[T] = C[T]
type Y = str

def _[T](x: X[T]):
    reveal_type(x)  # revealed: C[T@_]

def _(y: Y):
    reveal_type(y)  # revealed: str

reveal_type(X)  # revealed: TypeAliasType
reveal_type(X[int])  # revealed: TypeAliasType
reveal_type(Y)  # revealed: TypeAliasType
```

Note that in the last two function bodies, the revealed type does not mention the type alias at all. So in ty terms, `NominalInstance` does not need to wrap the (specialized) type alias.

Similarly, the revealed type of `X[int]` is `TypeAliasType`, and so `GenericAlias` doesn't need to wrap the type alias either.

This suggests that we can roll back all of the changes to `GenericAlias` and `NominalInstance`, and just make sure that `in_type_expression` propagates the generic context through when it resolves a type alias into its value type.

---

[I made several comments below about the current implementation, which might become moot given the above, but I'm keeping them here in case my analysis is wrong and we do need to keep those changes]

---

_Comment by @dcreager on 2025-09-04 19:02_

> and so `GenericAlias` doesn't need to wrap the type alias either

And in particular, this reminds me of an earlier draft of this that you had shown me, where `PEP695TypeAlias` had an `Option<Specialization>` field, since we do need _some_ way to distinguish `X` from `X[int]` in my example. And then `in_type_expression` would check if the type alias is generic, and if it is, fall back on the default specialization if that field is `None`.

---

_@carljm reviewed on 2025-09-06 01:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:1 on 2025-09-06 01:08_

Possibly, although this file is specific to generic PEP 695 aliases.

---

_Review comment by @carljm on `crates/ty_ide/src/hover.rs`:1592 on 2025-09-06 01:09_

This comment should be removed.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:47 on 2025-09-06 01:16_

I don't think this example should be an error. In this case the name `int`, within the scope of this alias, is bound to a typevar, shadowing the global name `int`.

The names in brackets here are not references, they are targets to which a (typevar) value will be bound in this scope.

This shadowing is unnecessarily confusing, but it's not an error.

Here's this example [working in pyright](https://pyright-play.net/?code=C4TwDgpgBA4g2gSwHbALpQLxQDYIM7CIqoCwAUOQCYQBmUA%2BgBQDmAXLHAQE6oCUr5KEKhcIANwgBDbPVCQWvIA).

I think this test can just be removed? Any identifier is valid here, and anything invalid will be a syntax error.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3605 on 2025-09-06 01:20_

We could implement `Default` for `TryBoolVisitor` to avoid repeating this a few places.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5598 on 2025-09-06 01:27_

Is there a reason we need to eagerly unpack specialized aliases when they appear in type expressions, where we don't for non-generic aliases (see below, we wrap them in a lazy `Type::TypeAlias` instead).

I think this eager unpacking will result in Salsa query cycles for an example like `type Rec[T] = T | list[Rec[T]]`, because we will eagerly unpack the alias, recursively, and type inference of the RHS will diverge.

It looks to me like we can just delete this new arm entirely and just let the below arm do its thing?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:43 on 2025-09-06 01:34_

What about when we use a generic type alias in a type expression without an explicit specialization? This should result in implicitly specializing to `Unknown`:

```py
type G[T] = list[T]

def _(g: G):
    reveal_type(g)  # should reveal `list[Unknown]`
```

But I don't see any tests for it, and I don't think the PR currently handles it correctly; instead we just fail to specialize at all, and allow a typevar to leak out, revealing `list[T@G]` above.

We should also have a test like the above but with a typevar default (`type G[T = int] = list[T]`) -- in this case we should specialize to `list[int]` when no explicit specialization is given.

(We already have code for creating the `default_specialization` for a generic context, so we shouldn't need to write new code to handle typevar defaults vs Unknown -- but we should have tests for both cases.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:5598 on 2025-09-06 01:36_

We might, though, need some additional handling here in case of a type alias with a generic context but without a specialization. Such an alias, if used in a type expression, should be implicitly "default specialized" (that is, specialized to the default for each typevar, or `Unknown` for typevars with no default.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6900 on 2025-09-06 01:40_

This is fine for now, but ultimately we will need to support all kinds of generic type aliases, so it might be a bit more forward-thinking to provide generalized API on `TypeAliasType` that matches variants internally, and avoid matching specifically on `TypeAliasType::PEP695` externally? But it's not a big deal, can always be changed later when we add support for other kinds of generic type aliases.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8823 on 2025-09-06 01:44_

Similar to my comment above, weak preference here to keep this TODO internal, not match here on specific kinds of aliases, and expose general API on `TypeAliasType` itself that we can use here without worrying about the details of which kinds of generic aliases are actually supported.

---

_@carljm reviewed on 2025-09-06 01:46_

Looking great! A few comments.

---

_@ibraheemdev reviewed on 2025-09-08 18:18_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:47 on 2025-09-08 18:18_

I took this from [the test in `generics/classes.md`](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/generics/pep695/classes.md?plain=1#L43). Is this different, or should that be removed as well?

---

_@ibraheemdev reviewed on 2025-09-08 18:20_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:3605 on 2025-09-08 18:20_

I wanted to avoid implementing `Default` for `Truthiness` because that felt like a potential footgun (and `TryBoolVisitor` is just a type alias so can't implement `Default` directly).

---

_@sharkdp reviewed on 2025-09-08 18:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:1 on 2025-09-08 18:47_

Yes. And I don't care too much about the structure/layout of the tests. Just wanted do let you know that there are some generics-related tests in that other file as well.

---

_Comment by @ibraheemdev on 2025-09-08 19:16_

The typing conformance suite is now panicking with `value_type(_): too many cycle iterations` on `aliases_type_statement.md` because of:
```py
type RecursiveTypeAlias1[T] = T | list[RecursiveTypeAlias1[T]]
r1_1: RecursiveTypeAlias1[int] = 1
```

This looks like https://github.com/astral-sh/ty/issues/256, which I believe is unrelated to this PR. The ecosystem failures also all seem to be cases where recognizing generic type aliases leads to more detailed diagnostics for underlying semantics that we do not support.

---

_@carljm reviewed on 2025-09-08 19:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:47 on 2025-09-08 19:28_

No, that test is similarly bogus and should be removed.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3605 on 2025-09-08 19:36_

I don't think you need to implement `Default` for `Truthiness` in order to implement it for `CycleDetector<TryBool, Type<'db>, Result<Truthiness, BoolError<'db>>>` (and you can `impl Default for TryBoolVisitor` as a shortcut for the latter). I could be missing something but I'm pretty sure I've done this before, see e.g. https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=8123cdc184c2e6a38c393f541dfbd151 (`V` doesn't implement `Default`)

---

_Comment by @carljm on 2025-09-08 19:42_

> The typing conformance suite is now panicking with `value_type(_): too many cycle iterations` on `aliases_type_statement.md`

Let me look at this. We avoid this already for PEP 695 aliases in general by creating a type alias type for the assignment without resolving the RHS, and deferring resolution of the RHS as part of `value_type` on the alias. It seems like something in this PR must be breaking this (i.e., resolving the RHS too eagerly) for generic aliases. If necessary we can fix it in a separate follow-up, but it is something that I would expect to work at this point, it's not an expected failure.



---

_@carljm reviewed on 2025-09-08 19:42_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:3605 on 2025-09-08 20:00_

You run into the orphan rule because `CycleDetector<_, _, R: Default>` implements `Default`, and `R` here is an external type (`Result<_, _>`). I don't think it's a huge deal but I'm happy to implement `Default` for `Truthiness` if you think that's better.

---

_@ibraheemdev reviewed on 2025-09-08 20:00_

---

_@carljm reviewed on 2025-09-08 20:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3605 on 2025-09-08 20:06_

Ah makes sense! No I think it's fine as-is, just wanted to understand :)

---

_Comment by @carljm on 2025-09-08 20:14_

Hmm I see the issue -- it's not that we resolve the RHS too eagerly in general, it's that when subscripting we resolve the value-type of the alias at that point to apply the specialization. We may need to allow `Type::TypeAlias` to carry a lazy specialization with it in order to support this. In any case, having looked at it I agree this isn't an issue we should try to solve in this PR.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/aliases.md`:1 on 2025-09-08 20:19_

Good point. I think we can look at this as a follow-up.

---

_@carljm approved on 2025-09-08 20:26_

Looks great!

---

_Merged by @carljm on 2025-09-08 20:26_

---

_Closed by @carljm on 2025-09-08 20:26_

---

_Branch deleted on 2025-09-08 20:26_

---
