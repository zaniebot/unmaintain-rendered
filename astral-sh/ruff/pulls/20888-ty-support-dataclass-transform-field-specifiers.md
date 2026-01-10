```yaml
number: 20888
title: "[ty] Support dataclass-transform `field_specifiers`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/field-specifiers
created_at: 2025-10-15T12:03:41Z
updated_at: 2025-10-16T18:49:13Z
url: https://github.com/astral-sh/ruff/pull/20888
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Support dataclass-transform `field_specifiers`

---

_Pull request opened by @sharkdp on 2025-10-15 12:03_

## Summary

Add support for the `field_specifiers` parameter on `dataclass_transform` decorator calls.

closes https://github.com/astral-sh/ty/issues/1068

## Conformance test results

All true positives :heavy_check_mark: 

## Ecosystem analysis

* `trio`: this is the kind of change that I would expect from this PR. The code makes use of a dataclass `Outcome` with a `_unwrapped: bool = attr.ib(default=False, eq=False, init=False)` field that is excluded from the `__init__` signature, so we now see a bunch of constructor-call-related errors going away.
* `home-assistant/core`: They have a `domain: str = attr.ib(init=False, repr=False)` field and then use
  ```py
    @domain.default
    def _domain_default(self) -> str:
        # …
  ```
  This accesses the `default` attribute on `dataclasses.Field[…]` with a type of `default: _T | Literal[_MISSING_TYPE.MISSING]`, so we get those "Object of type `_MISSING_TYPE` is not callable" errors. I don't really understand how that is supposed to work. Even if `_MISSING_TYPE` would be absent from that union, what does this try to call? pyright also issues an error and it doesn't seem to work at runtime? So this looks like a true positive?
* `attrs`: Similar here. There are some new diagnostics on code that tries to access `.validator` on a field. This *does* work at runtime, but I'm not sure how that is supposed to type-check (without a [custom plugin](https://github.com/python/mypy/blob/2c6c3959356674262d9b2c2dc43a33486e807a9c/mypy/plugins/attrs.py#L575-L602)). pyright errors on this as well.
* A handful of new false positives because we don't support `alias` yet

## Test Plan

Updated tests.


---

_Label `ty` added by @sharkdp on 2025-10-15 12:03_

---

_Comment by @github-actions[bot] on 2025-10-15 12:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-16 13:58:15.685593376 +0000
+++ new-output.txt	2025-10-16 13:58:19.040608580 +0000
@@ -1,5 +1,5 @@
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(d417)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(16c43)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/execute.rs:402:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(17443)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -279,6 +279,8 @@
 dataclasses_transform_converter.py:121:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 7
 dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> int`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo(unsupported type[X] special form)`
+dataclasses_transform_field.py:64:16: error[unknown-argument] Argument `id` does not match any known parameter
+dataclasses_transform_field.py:75:1: error[missing-argument] No argument provided for required parameter `name`
 dataclasses_transform_field.py:75:16: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 dataclasses_transform_func.py:57:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` of type `str`
 dataclasses_transform_func.py:61:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
@@ -288,6 +290,7 @@
 dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
 dataclasses_transform_func.py:97:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_transform_meta.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
+dataclasses_transform_meta.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_meta.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_meta.py:79:6: error[unsupported-operator] Operator `<` is not supported for types `Customer2` and `Customer2`
@@ -898,5 +901,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 900 diagnostics
+Found 903 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-15 12:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/dataclass_transform_example.py:21:13: info[revealed-type] Revealed type: `(self: DefineConverter, with_converter: int = int) -> None`
+ tests/dataclass_transform_example.py:21:13: info[revealed-type] Revealed type: `(self: DefineConverter, with_converter: int) -> None`
- tests/dataclass_transform_example.py:56:13: info[revealed-type] Revealed type: `(_a: int = Any) -> None`
+ tests/dataclass_transform_example.py:56:13: info[revealed-type] Revealed type: `(_a: int) -> None`
+ tests/test_hooks.py:154:48: error[missing-argument] No argument provided for required parameter `y`
+ tests/test_next_gen.py:142:14: error[unresolved-attribute] Type `dataclasses.Field` has no attribute `validator`
- tests/test_next_gen.py:380:14: error[unresolved-attribute] Type `int` has no attribute `validator`
+ tests/test_next_gen.py:380:14: error[unresolved-attribute] Type `dataclasses.Field` has no attribute `validator`
- Found 559 diagnostics
+ Found 561 diagnostics

artigraph (https://github.com/artigraph/artigraph)
- src/arti/internal/mappings.py:124:24: error[missing-argument] No argument provided for required parameter `root`
- src/arti/internal/mappings.py:124:34: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Literal["open"]`
- tests/arti/types/test_types.py:146:73: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | tuple[str]`
- tests/arti/types/test_types.py:146:73: error[invalid-argument-type] Argument is incorrect: Expected `set[str]`, found `Unknown | tuple[str]`
- tests/arti/types/test_types.py:146:73: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | tuple[str]`
- tests/arti/types/test_types.py:150:36: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | tuple[str]`
- tests/arti/types/test_types.py:150:36: error[invalid-argument-type] Argument is incorrect: Expected `set[str]`, found `Unknown | tuple[str]`
- tests/arti/types/test_types.py:150:36: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | tuple[str]`
- Found 146 diagnostics
+ Found 138 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/commands/menu.py:608:19: error[missing-argument] No arguments provided for required parameters `_name`, `_type`
+ tanjun/context/autocomplete.py:236:13: error[missing-argument] No arguments provided for required parameters `_name`, `_value`
- Found 113 diagnostics
+ Found 115 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_channel.py:203:50: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `SendType@MemorySendChannel`
- src/trio/_channel.py:295:50: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ClosedResourceError`
- src/trio/_channel.py:303:54: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `EndOfChannel`
- src/trio/_channel.py:447:50: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ClosedResourceError`
- src/trio/_channel.py:455:54: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `BrokenResourceError`
- src/trio/_core/_io_common.py:28:54: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `BaseException`
- src/trio/_core/_io_kqueue.py:287:58: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ClosedResourceError`
- src/trio/_core/_parking_lot.py:303:21: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `BrokenResourceError`
+ src/trio/_core/_run.py:628:35: error[missing-argument] No argument provided for required parameter `_scope`
- src/trio/_core/_run.py:1760:38: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `TrioInternalError`
+ src/trio/_core/_run.py:1749:80: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_core/_run.py:1925:31: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `None`
- src/trio/_core/_run.py:2055:33: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `RuntimeError`
- src/trio/_core/_run.py:2071:33: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `None`
- src/trio/_core/_run.py:2169:56: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `BaseException`
- src/trio/_core/_run.py:2705:48: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `@Todo | list[Unknown]`
- src/trio/_core/_run.py:2884:43: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `BaseException`
- src/trio/_core/_run.py:2963:46: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `KeyboardInterrupt`
- src/trio/_core/_tests/test_ki.py:406:50: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal[1]`
- src/trio/_core/_tests/test_run.py:246:54: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ValueError`
- src/trio/_core/_tests/test_run.py:253:54: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal[0]`
- src/trio/_core/_tests/test_run.py:1030:65: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal[1]`
- src/trio/_core/_tests/test_run.py:1287:66: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ValueError`
- src/trio/_core/_tests/test_run.py:1321:31: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ValueError`
- src/trio/_core/_tests/test_run.py:1352:66: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ValueError`
- src/trio/_core/_tests/test_run.py:1391:31: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ValueError`
- src/trio/_core/_tests/test_run.py:2431:64: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `None`
- src/trio/_core/_tests/test_run.py:2439:42: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal["be free!"]`
- src/trio/_core/_tests/test_run.py:2448:68: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `KeyError`
- src/trio/_core/_tests/test_run.py:2452:42: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `ValueError`
- src/trio/_core/_tests/test_run.py:2459:79: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `None`
- src/trio/_core/_traps.py:308:41: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal["reattaching"]`
- src/trio/_core/_traps.py:310:35: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Literal["reattaching"]`
- src/trio/_tests/test_timeouts.py:100:54: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `None`
- src/trio/_threads.py:205:35: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `RunFinishedError`
- src/trio/_threads.py:379:70: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `Value[Unknown] | Error`
- Found 645 diagnostics
+ Found 613 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-docker/prefect_docker/worker.py:950:35: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-docker/tests/test_worker.py:1187:9: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-kubernetes/tests/test_observer.py:78:42: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-kubernetes/tests/test_observer.py:155:42: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-redis/tests/test_messaging.py:452:30: error[missing-argument] No argument provided for required parameter `root`
- src/integrations/prefect-redis/tests/test_ordering.py:23:12: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/cli/variable.py:163:57: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | str | int | ... omitted 5 union elements`
- src/prefect/cli/variable.py:163:57: error[invalid-argument-type] Argument is incorrect: Expected `set[str]`, found `Unknown | str | int | ... omitted 5 union elements`
- src/prefect/cli/variable.py:163:57: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | str | int | ... omitted 5 union elements`
- src/prefect/cli/variable.py:165:57: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | str | int | ... omitted 5 union elements`
- src/prefect/cli/variable.py:165:57: error[invalid-argument-type] Argument is incorrect: Expected `set[str]`, found `Unknown | str | int | ... omitted 5 union elements`
- src/prefect/cli/variable.py:165:57: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Unknown | str | int | ... omitted 5 union elements`
- src/prefect/client/schemas/responses.py:469:16: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/events/related.py:36:9: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/events/related.py:51:12: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/events/utilities.py:96:27: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Any | str | dict[str, str] | dict[str, Any] | None`
- src/prefect/events/utilities.py:96:27: error[invalid-argument-type] Argument is incorrect: Expected `set[str]`, found `Any | str | dict[str, str] | dict[str, Any] | None`
- src/prefect/events/utilities.py:96:27: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any] | None`, found `Any | str | dict[str, str] | dict[str, Any] | None`
- src/prefect/runner/runner.py:1052:17: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1068:26: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1101:17: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1110:13: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/runner/runner.py:1126:26: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:143:20: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:201:20: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:217:30: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/actions.py:233:30: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/schemas/automations.py:432:25: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/events/schemas/automations.py:448:21: error[missing-argument] No argument provided for required parameter `root`
- src/prefect/server/models/work_queues.py:397:17: error[invalid-argument-type] Argument is incorrect: Expected `set[str]`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
- src/prefect/server/models/work_queues.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `set[str]`, found `dict[str, Unknown] | dict[str, Unknown | bool]`
- Found 3173 diagnostics
+ Found 3142 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/device_tracker/legacy.py:409:12: error[missing-argument] No argument provided for required parameter `config`
+ homeassistant/helpers/entity_registry.py:218:5: error[call-non-callable] Object of type `_MISSING_TYPE` is not callable
+ homeassistant/helpers/entity_registry.py:443:5: error[call-non-callable] Object of type `_MISSING_TYPE` is not callable
- Found 13758 diagnostics
+ Found 13761 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~35MB
+     struct fields = ~36MB
-     memo metadata = ~98MB
+     memo metadata = ~103MB

```
</details>


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-15 13:23_

---

_Comment by @github-actions[bot] on 2025-10-15 13:29_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 52 | 0 |
| `missing-argument` | 5 | 21 | 0 |
| `call-non-callable` | 2 | 0 | 0 |
| `unresolved-attribute` | 1 | 0 | 1 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **9** | **73** | **1** |

**[Full report with detailed diff](https://david-field-specifiers.ecosystem-663.pages.dev/diff)** ([timing results](https://david-field-specifiers.ecosystem-663.pages.dev/timing))


---

_@sharkdp reviewed on 2025-10-15 14:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:605 on 2025-10-15 14:24_

the code inside this branch has just moved

---

_@sharkdp reviewed on 2025-10-15 14:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:4561 on 2025-10-15 14:43_

This is the key change. When we see an annotated assignment like the one for `id` in `C` …
```py
def fancy_field(*, init: bool = True, kw_only: bool = False) -> Any: ...
@dataclass_transform(field_specifiers=(fancy_field,))

def fancy_model[T](cls: type[T]) -> type[T]:
    # ...
    return cls

@fancy_model
class Person:
    id: int = fancy_field(init=False)
```
… we temporarily store a list of "active" field-specifiers (for this class: `[fancy_field]`) in the inference builder. Later, when inferring the call expression on the right-hand side of that assignment, we can pass down this list of field-specifiers into the call resolution logic in order to specialize calls to `fancy_field`.

---

_Marked ready for review by @sharkdp on 2025-10-15 14:43_

---

_Review requested from @carljm by @sharkdp on 2025-10-15 14:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-15 14:43_

---

_Review requested from @dcreager by @sharkdp on 2025-10-15 14:43_

---

_Comment by @sharkdp on 2025-10-15 15:15_

Support for metaclass-based models is actually missing here (and we generally don't support baseclass-based models yet). I think it makes sense to do this [in a second step](https://github.com/astral-sh/ruff/pull/20896), actually.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:632 on 2025-10-15 15:27_

nit: it's a pre-existing issue, but most of our bitflags in this repo use bitshifts rather than octal to define the flags, which I find more readable, e.g.

https://github.com/astral-sh/ruff/blob/98d27c412810e157f8a65ea75726d66676628225/crates/ruff_python_ast/src/nodes.rs#L812-L823

---

_@sharkdp reviewed on 2025-10-15 15:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:602 on 2025-10-15 15:42_

Hm, this is probably too naive for overloaded functions (which is common for field specifiers)

---

_Converted to draft by @sharkdp on 2025-10-15 15:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:697 on 2025-10-15 17:01_

Why are we falling back to the Python `None` type here -- wouldn't we normally fall back to `Unknown` for this kind of thing? Seems probably worth a comment?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:610 on 2025-10-15 17:08_

I guess all these `.unwrap_or(None)` calls could also be `.unwrap_or_default()` calls, but IDK if that's actually clearer

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:283 on 2025-10-15 17:12_

why `2`, specifically?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4550 on 2025-10-15 17:21_

(`enclosing_scope.node().as_class()?` looks very cheap; I don't think it's any faster to explicitly check `enclosing_scope.kind()` before doing that call)

```suggestion
                let enclosing_scope = index.scope(scope.file_scope_id(db));
                let class_node = enclosing_scope.node().as_class()?;
                let class_definition = index.expect_single_definition(class_node);
                binding_type(db, class_definition)
                    .as_class_literal()?
                    .dataclass_params(db)
                    .map(|params| SmallVec::from(params.field_specifiers(db)))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4561 on 2025-10-15 17:39_

nice solution!

...Though I wonder what happens if you have nested classes with different dataclass_transform functions... does such a concept even make sense? It might be an interesting test, anyway... curious what other type checkers do for this kind of thing...

```py
def fancy_field(*, init: bool = True, kw_only: bool = False) -> Any: ...

def other_fancy_field(*, init: bool = True, kw_only: bool = False) -> Any: ...

@dataclass_transform(field_specifiers=(fancy_field,))
def fancy_model[T](cls: type[T]) -> type[T]:
    # ...
    return cls

@dataclass_transform(field_specifiers=(other_fancy_field,))
def other_fancy_model[T](cls: type[T]) -> type[T]:
    # ...
    return cls

@fancy_model
class Person:
    id: int = fancy_field(init=False)

    @other_fancy_model
    class Whatever:
        id: int = other_fancy_field(init=False)
        is_this_legal: str = fancy_field(init=False)
```

---

_@AlexWaygood reviewed on 2025-10-15 17:43_

---

_@sharkdp reviewed on 2025-10-16 10:37_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:602 on 2025-10-16 10:37_

... no, that's fine

---

_@sharkdp reviewed on 2025-10-16 10:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:697 on 2025-10-16 10:39_

Just plain wrong. I was using `Type::none(db)` as the fallback representation for missing field specifiers on this branch initially. That's my only explanation for why I possibly wrote that ;-)

---

_@sharkdp reviewed on 2025-10-16 10:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:610 on 2025-10-16 10:39_

This is pre-existing code, and I think I like the explicit form better?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:283 on 2025-10-16 10:45_

Maybe 3 would be better, so we can still handle Pydantic models inline? Otherwise, 1 also seems reasonable if we only want to be able to handle standard dataclasses inline.

* Standard dataclasses use 1 field specifier
* attrs uses 2 field specifiers
* pydantic uses 3 field specifiers
* SQLAlchemy uses 9 field specifiers, but I certainly don't want to reserve space for 9 `Type`s in every type inference builder, just in case.

---

_@sharkdp reviewed on 2025-10-16 10:45_

---

_@AlexWaygood reviewed on 2025-10-16 10:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:283 on 2025-10-16 10:55_

1, 2, or 3 all seem like reasonable choices, in that case! But I'd love for this to be captured in a comment, so other people reading through the source code don't have the same question as me ;)

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-16 11:18_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-16 11:19_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-16 11:32_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-16 11:32_

---

_@sharkdp reviewed on 2025-10-16 12:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:4561 on 2025-10-16 12:43_

Added a test case for this. Did not yet look into how all kinds of libraries handle this, but we implement the behavior that makes the most sense to me: models only react to field-specifiers of their own dataclass transformer.

It works because nested classes will use their own `TypeInferenceBuilder`.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-16 12:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-16 12:43_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-16 13:46_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-16 13:46_

---

_Marked ready for review by @sharkdp on 2025-10-16 13:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:518 on 2025-10-16 18:42_

[insert "all your base are belong to us" joke]

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:702 on 2025-10-16 18:44_

I am sort-of morbidly curious about what _does_ happen to our inference if you try to define a dataclass, with `dataclasses.field` for one of the fields, and you're using a custom typeshed that doesn't have `dataclasses.field` in it. That might be an interesting test, but it's almost certainly not worth spending time on it right now 😄

---

_@AlexWaygood approved on 2025-10-16 18:46_

Thank you!

---

_Merged by @sharkdp on 2025-10-16 18:49_

---

_Closed by @sharkdp on 2025-10-16 18:49_

---

_Branch deleted on 2025-10-16 18:49_

---
