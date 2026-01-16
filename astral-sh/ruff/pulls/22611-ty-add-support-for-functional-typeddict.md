```yaml
number: 22611
title: "[ty] Add support for functional `TypedDict`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: charlie/functional-dict
head: charlie/functional-typed
created_at: 2026-01-15T20:31:56Z
updated_at: 2026-01-16T01:47:08Z
url: https://github.com/astral-sh/ruff/pull/22611
synced_at: 2026-01-16T02:04:10Z
```

# [ty] Add support for functional `TypedDict`

---

_@charliermarsh_

## Summary

(Not ready for review.)

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 20:33_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

The percentage of diagnostics emitted that were expected errors decreased from 69.52% to 69.51%. The percentage of expected errors that received a diagnostic increased from 58.44% to 58.61%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 675 | 677 | +2 | ⏫ (✅) |
| False Positives | 296 | 297 | +1 | ⏫ (❌) |
| False Negatives | 480 | 478 | -2 | ⏬ (✅) |
| Total Diagnostics | 971 | 974 | 3 | ⏫ |
| Precision | 69.52% | 69.51% | -0.01% | ⏬ (❌) |
| Recall | 58.44% | 58.61% | +0.17% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [typeddicts_alt_syntax.py:35:72](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/typeddicts_alt_syntax.py#L35) | unknown-argument | unknown-argument: Argument `other` does not match any known parameter of `TypedDict` |
| [typeddicts_alt_syntax.py:45:43](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/typeddicts_alt_syntax.py#L45) | invalid-argument-type | invalid-argument-type: Invalid argument to key "year" with declared type `int` on TypedDict `Movie2`: value of type `Literal[""]` |
| [typeddicts_extra_items.py:22:47](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/typeddicts_extra_items.py#L22) | invalid-key | invalid-key: Unknown key "year" for TypedDict `MovieFunctional`: Unknown key "year" |


</details>

### False positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [typeddicts_extra_items.py:21:47](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/typeddicts_extra_items.py#L21) | invalid-key | invalid-key: Unknown key "novel_adaptation" for TypedDict `MovieFunctional`: Unknown key "novel_adaptation" |


</details>

### True positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [typeddicts_type_consistency.py:101:14](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance//tests/typeddicts_type_consistency.py#L101) | invalid-assignment | invalid-assignment: Object of type `Unknown | None` is not assignable to `str` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-15 20:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
isort (https://github.com/pycqa/isort)
- isort/output.py:534:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `@Todo | None | list[Unknown]`
+ isort/output.py:534:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Any | None | list[Unknown]`
- isort/output.py:544:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `@Todo | None | list[Unknown]`
+ isort/output.py:544:25: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Any | None | list[Unknown]`
- isort/output.py:552:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `@Todo | None | list[Unknown]`
+ isort/output.py:552:29: error[invalid-argument-type] Argument to function `import_statement` is incorrect: Expected `Sequence[str]`, found `Any | None | list[Unknown]`

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/utilities/test_build_client_schema.py:90:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:498:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:673:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:682:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:987:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_build_client_schema.py:1002:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 641 diagnostics
+ Found 635 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/utils/logging.py:48:25: error[invalid-key] Unknown key "()" for TypedDict `_FormatterConfigurationTypedDict`: Unknown key "()"
+ alerta/utils/logging.py:57:25: error[invalid-key] Unknown key "()" for TypedDict `_FormatterConfigurationTypedDict`: Unknown key "()"
+ alerta/utils/logging.py:60:25: error[invalid-key] Unknown key "()" for TypedDict `_FormatterConfigurationTypedDict`: Unknown key "()"
+ alerta/utils/logging.py:61:25: error[invalid-key] Unknown key "facility" for TypedDict `_FormatterConfigurationTypedDict`: Unknown key "facility"
- Found 620 diagnostics
+ Found 624 diagnostics

ppb-vector (https://github.com/ppb/ppb-vector)
- ppb_vector/__init__.py:567:16: error[invalid-return-type] Return type does not match returned value: expected `Vector`, found `int | float`
- ppb_vector/__init__.py:588:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Vector, Vector]`, found `tuple[int | float, Vector]`
+ ppb_vector/__init__.py:188:45: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["x"]` and `(Sequence[SupportsFloat] & Top[Mapping[Unknown, object]] & ~Vector) | VectorLikeDict`
+ ppb_vector/__init__.py:188:62: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["y"]` and `(Sequence[SupportsFloat] & Top[Mapping[Unknown, object]] & ~Vector) | VectorLikeDict`
+ ppb_vector/__init__.py:188:83: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Sequence[SupportsFloat] & Top[Mapping[Unknown, object]] & ~Vector) | VectorLikeDict`
- tests/test_length.py:41:20: error[unresolved-attribute] Object of type `int | float` has no attribute `length`
- tests/test_project.py:50:8: error[unresolved-attribute] Object of type `int | float` has no attribute `isclose`
- tests/test_scalar_multiplication.py:10:28: error[unresolved-attribute] Object of type `int | float` has no attribute `x`
- tests/test_scalar_multiplication.py:11:28: error[unresolved-attribute] Object of type `int | float` has no attribute `y`
- tests/test_scalar_multiplication.py:19:12: error[unresolved-attribute] Object of type `int | float` has no attribute `isclose`
- tests/test_scalar_multiplication.py:24:12: error[unresolved-attribute] Object of type `int | float` has no attribute `isclose`
- tests/test_scalar_multiplication.py:32:20: error[unresolved-attribute] Object of type `int | float` has no attribute `length`
- Found 21 diagnostics
+ Found 15 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
+ dragonchain/lib/database/redisearch_utest.py:71:55: error[invalid-argument-type] Argument to function `create_transaction_index` is incorrect: Expected `Iterable[custom_index] | None`, found `list[dict[Unknown | str, Unknown | str]]`
+ dragonchain/lib/database/redisearch_utest.py:71:56: error[missing-typed-dict-key] Missing required key 'options' in TypedDict `custom_index` constructor
- dragonchain/lib/dto/api_key_model_utest.py:265:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["transactions"]` on object of type `str`
- dragonchain/lib/dto/api_key_model_utest.py:265:9: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
+ dragonchain/lib/dto/api_key_model.py:175:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `permissions_doc`, found `permissions_doc | dict[Unknown | str, Unknown | str | bool | dict[Unknown, Unknown]]`
+ dragonchain/lib/dto/api_key_model.py:209:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `permissions_doc`, found `permissions_doc | dict[Unknown | str, Unknown | str | bool | dict[Unknown, Unknown]]`
+ dragonchain/transaction_processor/level_3_actions_utest.py:247:86: error[invalid-argument-type] Argument to function `verify_blocks` is incorrect: Expected `L1Headers`, found `dict[Unknown | str, Unknown | str]`
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:63: error[invalid-argument-type] Argument to function `verify_blocks` is incorrect: Expected `L1Headers`, found `dict[Unknown | str, Unknown | int | str]`
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:73: error[invalid-argument-type] Invalid argument to key "dc_id" with declared type `str` on TypedDict `L1Headers`: value of type `Literal[123]`
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:90: error[invalid-argument-type] Invalid argument to key "block_id" with declared type `str` on TypedDict `L1Headers`: value of type `Literal[124]`
+ dragonchain/webserver/helpers_utest.py:169:47: error[invalid-argument-type] Argument to function `verify_custom_indexes_options` is incorrect: Expected `Iterable[custom_index]`, found `list[dict[Unknown | str, Unknown | str]]`
+ dragonchain/webserver/helpers_utest.py:169:48: error[missing-typed-dict-key] Missing required key 'options' in TypedDict `custom_index` constructor
+ dragonchain/webserver/helpers_utest.py:172:47: error[invalid-argument-type] Argument to function `verify_custom_indexes_options` is incorrect: Expected `Iterable[custom_index]`, found `list[dict[Unknown | str, Unknown | str]]`
+ dragonchain/webserver/helpers_utest.py:172:48: error[missing-typed-dict-key] Missing required key 'options' in TypedDict `custom_index` constructor
+ dragonchain/webserver/helpers_utest.py:175:47: error[invalid-argument-type] Argument to function `verify_custom_indexes_options` is incorrect: Expected `Iterable[custom_index]`, found `list[dict[Unknown | str, Unknown | str]]`
+ dragonchain/webserver/helpers_utest.py:175:48: error[missing-typed-dict-key] Missing required key 'options' in TypedDict `custom_index` constructor
+ dragonchain/webserver/lib/api_keys_utest.py:48:66: error[invalid-argument-type] Argument to function `create_api_key_v1` is incorrect: Expected `permissions_doc | None`, found `dict[Unknown | str, Unknown | str]`
+ dragonchain/webserver/lib/api_keys_utest.py:48:87: error[missing-typed-dict-key] Missing required key 'default_allow' in TypedDict `permissions_doc` constructor
+ dragonchain/webserver/lib/api_keys_utest.py:48:87: error[missing-typed-dict-key] Missing required key 'permissions' in TypedDict `permissions_doc` constructor
+ dragonchain/webserver/lib/api_keys_utest.py:48:87: error[missing-typed-dict-key] Missing required key 'version' in TypedDict `permissions_doc` constructor
+ dragonchain/webserver/lib/api_keys_utest.py:48:88: error[invalid-key] Unknown key "wind" for TypedDict `permissions_doc`: Unknown key "wind"
+ dragonchain/webserver/lib/api_keys_utest.py:91:59: error[invalid-argument-type] Argument to function `update_api_key_v1` is incorrect: Expected `permissions_doc | None`, found `dict[Unknown | str, Unknown | str]`
+ dragonchain/webserver/lib/api_keys_utest.py:91:59: error[missing-typed-dict-key] Missing required key 'default_allow' in TypedDict `permissions_doc` constructor
+ dragonchain/webserver/lib/api_keys_utest.py:91:59: error[missing-typed-dict-key] Missing required key 'permissions' in TypedDict `permissions_doc` constructor
+ dragonchain/webserver/lib/api_keys_utest.py:91:59: error[missing-typed-dict-key] Missing required key 'version' in TypedDict `permissions_doc` constructor
+ dragonchain/webserver/lib/api_keys_utest.py:91:60: error[invalid-key] Unknown key "definitely" for TypedDict `permissions_doc`: Unknown key "definitely"
- Found 436 diagnostics
+ Found 458 diagnostics

operator (https://github.com/canonical/operator)
+ ops/_private/harness.py:980:9: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `dict[Unknown | str, Unknown | str | list[Unknown]]` on object of type `dict[int, _RelationEntities]`
+ ops/_private/harness.py:1640:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[str | None, int | None]` and value of type `dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str]]]] | list[str]]` on object of type `dict[tuple[str | None, int | None], _NetworkDict]`
+ ops/_private/harness.py:3351:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | LayerDict | None`, found `str | (LayerDict & Top[dict[Unknown, Unknown]]) | (Layer & Top[dict[Unknown, Unknown]])`
+ ops/charm.py:2094:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["multiple"]` and `_StorageMetaDict`
- ops/model.py:3730:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ ops/model.py:3729:16: error[invalid-return-type] Return type does not match returned value: expected `_StatusDict`, found `dict[Unknown | str, Unknown | str]`
+ ops/model.py:3843:16: error[invalid-return-type] Return type does not match returned value: expected `_NetworkDict`, found `dict[Unknown | str, Unknown | list[dict[Unknown | str, Unknown | str | list[dict[Unknown | str, Unknown | str] | Unknown]] | Unknown] | list[str]]`
+ ops/model.py:3844:31: error[invalid-argument-type] Invalid argument to key "bind-addresses" with declared type `list[BindAddressDict]` on TypedDict `_NetworkDict`: value of type `list[dict[Unknown | str, Unknown | str | list[dict[Unknown | str, Unknown | str] | Unknown]] | Unknown]`
- ops/pebble.py:630:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ops/pebble.py:720:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ops/pebble.py:789:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ ops/pebble.py:884:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:946:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:963:28: error[invalid-assignment] Object of type `(ServiceDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to `ServiceDict`
+ ops/pebble.py:1033:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:1101:26: error[invalid-assignment] Object of type `(CheckDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to `CheckDict`
+ ops/pebble.py:1148:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:1165:28: error[unresolved-attribute] Object of type `ExecDict` has no attribute `items`
+ ops/pebble.py:1173:27: error[invalid-assignment] Cannot assign value of type `Unknown & ~AlwaysFalsy` to key of type `Unknown & ~Literal["environment"]` on TypedDict `ExecDict`
+ ops/pebble.py:1183:28: error[unresolved-attribute] Object of type `HttpDict` has no attribute `items`
+ ops/pebble.py:1191:27: error[invalid-assignment] Cannot assign value of type `Unknown & ~AlwaysFalsy` to key of type `Unknown & ~Literal["headers"]` on TypedDict `HttpDict`
+ ops/pebble.py:1201:28: error[unresolved-attribute] Object of type `TcpDict` has no attribute `items`
+ ops/pebble.py:1262:30: error[invalid-assignment] Object of type `(LogTargetDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to `LogTargetDict`
+ ops/pebble.py:1286:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:1510:49: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["change-id"]` and `_CheckInfoDict`
+ ops/pebble.py:1511:30: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["startup"]` and `_CheckInfoDict`
+ ops/pebble.py:1686:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["repeat-after"]` and `_NoticeDict`
+ ops/pebble.py:1689:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["expire-after"]` and `_NoticeDict`
+ ops/pebble.py:2096:56: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["local"]` and `IdentityDict`
+ ops/pebble.py:2097:56: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["basic"]` and `IdentityDict`
+ ops/pebble.py:2102:32: error[missing-typed-dict-key] Missing required key 'basic' in TypedDict `IdentityDict` constructor
+ ops/pebble.py:2102:32: error[missing-typed-dict-key] Missing required key 'local' in TypedDict `IdentityDict` constructor
+ ops/pebble.py:2103:23: error[invalid-argument-type] Invalid argument to key "access" with declared type `Literal["untrusted", "metrics", "read", "admin"]` on TypedDict `IdentityDict`: value of type `str`
+ ops/pebble.py:2556:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | LayerDict | None`, found `(LayerDict & Top[dict[Unknown, Unknown]]) | (Layer & Top[dict[Unknown, Unknown]])`
- Found 132 diagnostics
+ Found 158 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

pyproject-metadata (https://github.com/pypa/pyproject-metadata)
- pyproject_metadata/__init__.py:397:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyproject_metadata/pyproject.py:171:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyproject_metadata/__init__.py:398:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["project"]` and `PyProjectTable`
+ pyproject_metadata/project_table.py:142:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/project_table.py:144:17: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/project_table.py:146:24: error[missing-argument] No argument provided for required parameter `name`
+ pyproject_metadata/project_table.py:147:17: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/project_table.py:156:39: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/pyproject.py:230:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["readme"]` and `ProjectTable`
- Found 4 diagnostics
+ Found 9 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/cargo/interpreter.py:485:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["workspace"]` and `Manifest`
+ mesonbuild/cargo/interpreter.py:487:14: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["package"]` and `Manifest`
+ mesonbuild/cargo/manifest.py:310:22: error[no-matching-overload] No overload of function `_depv_to_dep` matches arguments
+ mesonbuild/cargo/manifest.py:568:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["package"]` and `Manifest`
- mesonbuild/cargo/manifest.py:304:16: error[missing-typed-dict-key] Missing required key 'workspace' in TypedDict `FromWorkspace` constructor
- mesonbuild/cargo/manifest.py:304:17: error[invalid-key] Unknown key "version" for TypedDict `FromWorkspace`: Unknown key "version"
- mesonbuild/cargo/manifest.py:516:40: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `LibTarget`, found `dict[Unknown, Unknown]`
+ mesonbuild/optinterpreter.py:202:38: error[unresolved-attribute] Object of type `FuncOptionArgs` has no attribute `items`
+ mesonbuild/scripts/depaccumulate.py:81:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["provides"]` and `Rule`
+ mesonbuild/scripts/depaccumulate.py:83:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["requires"]` and `Rule`
+ mesonbuild/scripts/depscan.py:186:25: error[unresolved-attribute] Object of type `Require` has no attribute `update`
- unittests/cargotests.py:377:68: error[invalid-key] Unknown key "optional" for TypedDict `FromWorkspace`: Unknown key "optional"
- unittests/cargotests.py:385:62: error[invalid-key] Unknown key "features" for TypedDict `FromWorkspace`: Unknown key "features"
+ unittests/cargotests.py:330:40: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:331:33: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | FromWorkspace]`
+ unittests/cargotests.py:331:45: error[missing-typed-dict-key] Missing required key 'version' in TypedDict `Package` constructor
+ unittests/cargotests.py:338:33: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ unittests/cargotests.py:338:45: error[missing-typed-dict-key] Missing required key 'version' in TypedDict `Package` constructor
+ unittests/cargotests.py:348:40: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:362:40: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:377:48: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `FromWorkspace | Dependency | str`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/cargotests.py:385:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `FromWorkspace | Dependency | str`, found `dict[Unknown | str, Unknown | bool | list[Unknown | str]]`
+ unittests/cargotests.py:398:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:415:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:439:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:455:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:495:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:502:38: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:513:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:547:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
+ unittests/cargotests.py:567:42: error[invalid-argument-type] Argument to bound method `from_raw` is incorrect: Expected `Manifest`, found `dict[str, object]`
- Found 2152 diagnostics
+ Found 2173 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/job.py:193:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["listing"]` and `DirectoryType`
+ cwltool/job.py:359:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["listing"]` and `DirectoryType`
- Found 252 diagnostics
+ Found 254 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/wrapper/_implementations.py:999:5: error[unsupported-operator] Operator `-` is not supported between objects of type `frozenset[Literal["__kw", "group", "name", "package", "provider", "to_config"]]` and `set[Unknown | str]`
+ src/hydra_zen/wrapper/_implementations.py:1438:41: error[unresolved-attribute] Object of type `_StoreCallSig` has no attribute `copy`
- src/hydra_zen/wrapper/_implementations.py:1572:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/hydra_zen/wrapper/_implementations.py:1565:28: error[unresolved-attribute] Object of type `_StoreCallSig` has no attribute `copy`
- Found 521 diagnostics
+ Found 523 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_vendor/pyproject_metadata/__init__.py:337:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/scikit_build_core/_vendor/pyproject_metadata/pyproject.py:169:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/scikit_build_core/_vendor/pyproject_metadata/__init__.py:338:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["project"]` and `PyProjectTable`
+ src/scikit_build_core/_vendor/pyproject_metadata/project_table.py:134:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ src/scikit_build_core/_vendor/pyproject_metadata/project_table.py:136:17: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ src/scikit_build_core/_vendor/pyproject_metadata/project_table.py:138:24: error[missing-argument] No argument provided for required parameter `name`
+ src/scikit_build_core/_vendor/pyproject_metadata/project_table.py:139:17: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ src/scikit_build_core/_vendor/pyproject_metadata/project_table.py:148:39: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ src/scikit_build_core/_vendor/pyproject_metadata/pyproject.py:228:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["readme"]` and `ProjectTable`
- Found 46 diagnostics
+ Found 51 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deployment.py:392:48: error[invalid-assignment] Invalid assignment to key "anchor_date" with declared type `str` on TypedDict `IntervalScheduleOptions`: value of type `datetime`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5410 diagnostics
+ Found 5406 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- Found 1821 diagnostics
+ Found 1823 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2026-01-15 20:36_

---

_Comment by @charliermarsh on 2026-01-15 20:48_

(I just wanted to see conformance, etc.)

---

_Comment by @charliermarsh on 2026-01-16 01:30_

At present, I believe the conformance changes are correct.

- `typeddicts_alt_syntax.py:35:72` is now caught.
- `typeddicts_alt_syntax.py:45:43` is now caught.
- `typeddicts_extra_items.py:22:47` is now caught.
- `typeddicts_extra_items.py:21:47` is a false positive because we don't support `extra_items` (but matches our class-based `TypedDict` behavior).
- `typeddicts_type_consistency.py:101:14` is flagged as a true positive removed, but the test suite says either option is acceptable.

---

_Comment by @charliermarsh on 2026-01-16 01:46_

I believe the Alerta diagnostics are "correct" in that they're also emitted by other type checkers: `"()"` is not accepted to `_FormatterConfigurationTypedDict` (https://mypy-play.net/?mypy=latest&python=3.12&gist=efa01b423c6f56db2042f3dbce0d01df).

---
