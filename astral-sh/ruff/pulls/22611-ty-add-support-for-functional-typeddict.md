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
updated_at: 2026-01-15T20:48:37Z
url: https://github.com/astral-sh/ruff/pull/22611
synced_at: 2026-01-15T21:12:57Z
```

# [ty] Add support for functional `TypedDict`

---

_@charliermarsh_

## Summary

(Not ready for review.)

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 20:33_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Typing Conformance

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 675 | 678 | +3 | ⏫ (✅) |
| False Positives | 296 | 305 | +9 | ⏫ (❌) |
| False Negatives | 480 | 477 | -3 | ⏬ (✅) |
| Total Diagnostics | 971 | 983 | 12 | ⏫ |
| Precision | 69.52% | 68.97% | -0.54% | ⏬ (❌) |
| Recall | 58.44% | 58.70% | +0.26% | ⏫ (✅) |


The percentage of diagnostics emitted that were expected errors decreased from 69.52% to 68.97%, and the percentage of expected errors that received a diagnostic increased from 58.44% to 58.70%.

## True positives added 🎉

<details>

| Location | Name | Message |
|----------|------|---------|
| [typeddicts_alt_syntax.py:35:72](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_alt_syntax.py#L35) | unknown-argument | unknown-argument: Argument `other` does not match any known parameter of `TypedDict` |
| [typeddicts_extra_items.py:22:47](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_extra_items.py#L22) | invalid-key | invalid-key: Unknown key "year" for TypedDict `MovieFunctional`: Unknown key "year" |
| [typeddicts_type_consistency.py:107:13](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_type_consistency.py#L107) | no-matching-overload | no-matching-overload: No overload of bound method `get` matches arguments |


</details>

## False positives added 🫤

<details>

| Location | Name | Message |
|----------|------|---------|
| [typeddicts_alt_syntax.py:43:9](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_alt_syntax.py#L43) | invalid-type-form | invalid-type-form: Variable of type `type[TypedDictFallback]` is not allowed in a type expression |
| [typeddicts_extra_items.py:19:63](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_extra_items.py#L19) | unknown-argument | unknown-argument: Argument `extra_items` does not match any known parameter of `TypedDict` |
| [typeddicts_extra_items.py:21:47](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_extra_items.py#L21) | invalid-key | invalid-key: Unknown key "novel_adaptation" for TypedDict `MovieFunctional`: Unknown key "novel_adaptation" |
| [typeddicts_required.py:71:63](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_required.py#L71) | invalid-type-form | invalid-type-form: Invalid type `Literal["RecursiveMovie"]` in `TypedDict` field type |
| [typeddicts_type_consistency.py:103:13](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_type_consistency.py#L103) | no-matching-overload | no-matching-overload: No overload of bound method `get` matches arguments |
| [typeddicts_type_consistency.py:105:19](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_type_consistency.py#L105) | no-matching-overload | no-matching-overload: No overload of bound method `get` matches arguments |
| [typeddicts_type_consistency.py:91:14](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_type_consistency.py#L91) | no-matching-overload | no-matching-overload: No overload of bound method `get` matches arguments |
| [typeddicts_type_consistency.py:92:13](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_type_consistency.py#L92) | no-matching-overload | no-matching-overload: No overload of bound method `get` matches arguments |
| [typeddicts_type_consistency.py:97:21](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/typeddicts_type_consistency.py#L97) | invalid-argument-type | invalid-argument-type: Argument to bound method `get` is incorrect: Argument type `UserType2` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self` |


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

porcupine (https://github.com/Akuli/porcupine)
+ porcupine/plugins/aboutdialog.py:55:31: error[invalid-argument-type] Method `__getitem__` of type `(key: Literal["in"], /) -> Misc` cannot be called with key of type `Literal["x"]` on object of type `_PlaceInfo`
+ porcupine/plugins/aboutdialog.py:55:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Misc`
+ porcupine/plugins/aboutdialog.py:55:55: error[invalid-argument-type] Method `__getitem__` of type `(key: Literal["in"], /) -> Misc` cannot be called with key of type `Literal["y"]` on object of type `_PlaceInfo`
+ porcupine/plugins/aboutdialog.py:55:55: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Misc`
+ porcupine/plugins/aboutdialog.py:68:20: error[invalid-argument-type] Method `__getitem__` of type `(key: Literal["in"], /) -> Misc` cannot be called with key of type `Literal["y"]` on object of type `_PlaceInfo`
+ porcupine/plugins/aboutdialog.py:68:20: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Misc`
+ porcupine/plugins/aboutdialog.py:99:21: error[invalid-argument-type] Method `__getitem__` of type `(key: Literal["in"], /) -> Misc` cannot be called with key of type `Literal["x"]` on object of type `_PlaceInfo`
+ porcupine/plugins/aboutdialog.py:99:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Misc`
+ porcupine/plugins/aboutdialog.py:100:21: error[invalid-argument-type] Method `__getitem__` of type `(key: Literal["in"], /) -> Misc` cannot be called with key of type `Literal["y"]` on object of type `_PlaceInfo`
+ porcupine/plugins/aboutdialog.py:100:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Misc`
+ porcupine/settings.py:641:18: error[invalid-argument-type] Argument to bound method `grid_configure` is incorrect: Expected `int`, found `Misc`
+ porcupine/settings.py:641:22: error[invalid-argument-type] Method `__getitem__` of type `(key: Literal["in"], /) -> Misc` cannot be called with key of type `Literal["row"]` on object of type `_GridInfo`
+ porcupine/settings.py:643:23: error[invalid-argument-type] Argument to bound method `grid_configure` is incorrect: Expected `int`, found `Misc`
+ porcupine/settings.py:643:27: error[invalid-argument-type] Method `__getitem__` of type `(key: Literal["in"], /) -> Misc` cannot be called with key of type `Literal["row"]` on object of type `_GridInfo`
- Found 18 diagnostics
+ Found 32 diagnostics

ppb-vector (https://github.com/ppb/ppb-vector)
- ppb_vector/__init__.py:567:16: error[invalid-return-type] Return type does not match returned value: expected `Vector`, found `int | float`
- ppb_vector/__init__.py:588:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Vector, Vector]`, found `tuple[int | float, Vector]`
+ ppb_vector/__init__.py:188:45: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["x"]` and `(Sequence[SupportsFloat] & Top[Mapping[Unknown, object]] & ~Vector) | VectorLikeDict`
+ ppb_vector/__init__.py:188:62: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["y"]` and `(Sequence[SupportsFloat] & Top[Mapping[Unknown, object]] & ~Vector) | VectorLikeDict`
- tests/test_length.py:41:20: error[unresolved-attribute] Object of type `int | float` has no attribute `length`
- tests/test_project.py:50:8: error[unresolved-attribute] Object of type `int | float` has no attribute `isclose`
- tests/test_scalar_multiplication.py:10:28: error[unresolved-attribute] Object of type `int | float` has no attribute `x`
- tests/test_scalar_multiplication.py:11:28: error[unresolved-attribute] Object of type `int | float` has no attribute `y`
- tests/test_scalar_multiplication.py:19:12: error[unresolved-attribute] Object of type `int | float` has no attribute `isclose`
- tests/test_scalar_multiplication.py:24:12: error[unresolved-attribute] Object of type `int | float` has no attribute `isclose`
- tests/test_scalar_multiplication.py:32:20: error[unresolved-attribute] Object of type `int | float` has no attribute `length`
- Found 21 diagnostics
+ Found 14 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

dragonchain (https://github.com/dragonchain/dragonchain)
+ dragonchain/lib/database/redisearch.py:122:15: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `custom_index` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ dragonchain/lib/database/redisearch_utest.py:71:55: error[invalid-argument-type] Argument to function `create_transaction_index` is incorrect: Expected `Iterable[custom_index] | None`, found `list[dict[Unknown | str, Unknown | str]]`
+ dragonchain/lib/database/redisearch_utest.py:71:56: error[missing-typed-dict-key] Missing required key 'options' in TypedDict `custom_index` constructor
- dragonchain/lib/dto/api_key_model_utest.py:265:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["transactions"]` on object of type `str`
- dragonchain/lib/dto/api_key_model_utest.py:265:9: error[not-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
+ dragonchain/lib/dto/api_key_model.py:175:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `permissions_doc`, found `permissions_doc | dict[Unknown | str, Unknown | str | bool | dict[Unknown, Unknown]]`
+ dragonchain/lib/dto/api_key_model.py:209:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `permissions_doc`, found `permissions_doc | dict[Unknown | str, Unknown | str | bool | dict[Unknown, Unknown]]`
+ dragonchain/lib/dto/api_key_model.py:250:12: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `permissions_doc` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ dragonchain/lib/dto/api_key_model.py:254:73: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `permissions_doc` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ dragonchain/transaction_processor/level_3_actions_utest.py:247:86: error[invalid-argument-type] Argument to function `verify_blocks` is incorrect: Expected `L1Headers`, found `dict[Unknown | str, Unknown | str]`
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:63: error[invalid-argument-type] Argument to function `verify_blocks` is incorrect: Expected `L1Headers`, found `dict[Unknown | str, Unknown | int | str]`
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:73: error[invalid-argument-type] Invalid argument to key "dc_id" with declared type `str` on TypedDict `L1Headers`: value of type `Literal[123]`
+ dragonchain/transaction_processor/level_4_actions_utest.py:73:90: error[invalid-argument-type] Invalid argument to key "block_id" with declared type `str` on TypedDict `L1Headers`: value of type `Literal[124]`
+ dragonchain/webserver/helpers.py:208:22: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `custom_index` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ dragonchain/webserver/helpers.py:210:49: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `custom_index` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ dragonchain/webserver/helpers.py:212:48: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `custom_index` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ dragonchain/webserver/helpers.py:214:51: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `custom_index` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
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
+ Found 465 diagnostics

operator (https://github.com/canonical/operator)
+ ops/_private/harness.py:980:9: error[invalid-assignment] Invalid subscript assignment with key of type `Unknown` and value of type `dict[Unknown | str, Unknown | str | list[Unknown]]` on object of type `dict[int, _RelationEntities]`
+ ops/_private/harness.py:1640:9: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[str | None, int | None]` and value of type `dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str]]]] | list[str]]` on object of type `dict[tuple[str | None, int | None], _NetworkDict]`
+ ops/_private/harness.py:2275:41: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/_private/harness.py:2282:18: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/_private/harness.py:3351:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | LayerDict | None`, found `str | (LayerDict & Top[dict[Unknown, Unknown]]) | (Layer & Top[dict[Unknown, Unknown]])`
+ ops/charm.py:2040:30: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2040:30: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2044:22: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_RelationMetaDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/charm.py:2052:25: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2088:28: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2089:23: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2090:26: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2091:29: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_StorageMetaDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/charm.py:2092:25: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_StorageMetaDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/charm.py:2094:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["multiple"]` and `_StorageMetaDict`
+ ops/charm.py:2103:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2127:25: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2128:28: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2328:23: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/charm.py:2329:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/hookcmds/_types.py:86:59: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `BindAddressDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/hookcmds/_types.py:88:25: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/hookcmds/_types.py:89:28: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/hookcmds/_types.py:363:64: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/model.py:1126:31: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/model.py:1134:24: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/model.py:1138:23: error[no-matching-overload] No overload of bound method `get` matches arguments
- ops/model.py:3730:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ ops/model.py:3729:16: error[invalid-return-type] Return type does not match returned value: expected `_StatusDict`, found `dict[Unknown | str, Unknown | str]`
+ ops/model.py:3843:16: error[invalid-return-type] Return type does not match returned value: expected `_NetworkDict`, found `dict[Unknown | str, Unknown | list[dict[Unknown | str, Unknown | str | list[dict[Unknown | str, Unknown | str] | Unknown]] | Unknown] | list[str]]`
+ ops/model.py:3844:31: error[invalid-argument-type] Invalid argument to key "bind-addresses" with declared type `list[BindAddressDict]` on TypedDict `_NetworkDict`: value of type `list[dict[Unknown | str, Unknown | str | list[dict[Unknown | str, Unknown | str] | Unknown]] | Unknown]`
+ ops/pebble.py:130:18: error[invalid-type-form] Invalid type `Literal["CheckLevel | str"]` in `TypedDict` field type
+ ops/pebble.py:184:18: error[invalid-type-form] Invalid type `Literal["NotRequired[LocalIdentityDict]"]` in `TypedDict` field type
+ ops/pebble.py:185:18: error[invalid-type-form] Invalid type `Literal["NotRequired[BasicIdentityDict]"]` in `TypedDict` field type
- ops/pebble.py:630:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ops/pebble.py:720:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ops/pebble.py:789:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ ops/pebble.py:631:20: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_WarningDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:716:17: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_TaskDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:721:20: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_TaskDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:724:18: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_TaskDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:784:47: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_ChangeDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:786:17: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_ChangeDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:790:20: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_ChangeDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:793:18: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_ChangeDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:829:63: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:832:57: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:835:63: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:884:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:917:24: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:918:28: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:920:63: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:922:68: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:924:63: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:946:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:963:28: error[invalid-assignment] Object of type `(ServiceDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to `ServiceDict`
+ ops/pebble.py:964:24: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:965:28: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:966:24: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:967:25: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:968:24: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:969:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:970:28: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:971:30: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:972:33: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:973:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:974:24: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ServiceDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:975:22: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:976:25: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ServiceDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:977:28: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:978:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:979:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:980:38: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:981:30: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:982:31: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ServiceDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:983:30: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:984:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1033:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:1101:26: error[invalid-assignment] Object of type `(CheckDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to `CheckDict`
+ ops/pebble.py:1102:30: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1104:50: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1106:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1108:37: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1109:35: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1110:36: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1111:38: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `CheckDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1113:16: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `CheckDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1118:15: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `CheckDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1123:17: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `CheckDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1148:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:1165:28: error[invalid-argument-type] Argument to bound method `items` is incorrect: Argument type `ExecDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1169:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1173:27: error[invalid-assignment] Cannot assign value of type `Unknown & ~AlwaysFalsy` to key of type `Unknown & ~Literal["environment"]` on TypedDict `ExecDict`
+ ops/pebble.py:1183:28: error[invalid-argument-type] Argument to bound method `items` is incorrect: Argument type `HttpDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1187:27: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1191:27: error[invalid-assignment] Cannot assign value of type `Unknown & ~AlwaysFalsy` to key of type `Unknown & ~Literal["headers"]` on TypedDict `HttpDict`
+ ops/pebble.py:1201:28: error[invalid-argument-type] Argument to bound method `items` is incorrect: Argument type `TcpDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1262:30: error[invalid-assignment] Object of type `(LogTargetDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to `LogTargetDict`
+ ops/pebble.py:1263:30: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1264:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1265:25: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1266:41: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1267:18: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `LogTargetDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1286:9: error[invalid-method-override] Invalid override of method `__eq__`: Definition is incompatible with `object.__eq__`
+ ops/pebble.py:1391:18: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_FileInfoDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1394:21: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_FileInfoDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1395:18: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_FileInfoDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1396:22: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_FileInfoDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1397:19: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_FileInfoDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1503:32: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1505:21: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_CheckInfoDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1510:49: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["change-id"]` and `_CheckInfoDict`
+ ops/pebble.py:1511:30: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["startup"]` and `_CheckInfoDict`
+ ops/pebble.py:1518:34: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1520:23: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_CheckInfoDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1521:22: error[no-matching-overload] No overload of bound method `get` matches arguments
+ ops/pebble.py:1677:21: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_NoticeDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1684:23: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_NoticeDict` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ ops/pebble.py:1686:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["repeat-after"]` and `_NoticeDict`
+ ops/pebble.py:1689:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["expire-after"]` and `_NoticeDict`
+ ops/pebble.py:2096:56: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["local"]` and `IdentityDict`
+ ops/pebble.py:2097:56: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["basic"]` and `IdentityDict`
+ ops/pebble.py:2102:32: error[missing-typed-dict-key] Missing required key 'basic' in TypedDict `IdentityDict` constructor
+ ops/pebble.py:2102:32: error[missing-typed-dict-key] Missing required key 'local' in TypedDict `IdentityDict` constructor
+ ops/pebble.py:2103:23: error[invalid-argument-type] Invalid argument to key "access" with declared type `Literal["untrusted", "metrics", "read", "admin"]` on TypedDict `IdentityDict`: value of type `str`
+ ops/pebble.py:2556:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | LayerDict | None`, found `(LayerDict & Top[dict[Unknown, Unknown]]) | (Layer & Top[dict[Unknown, Unknown]])`
+ ops/pebble.py:2666:17: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `_Item` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
- Found 132 diagnostics
+ Found 251 diagnostics

pyproject-metadata (https://github.com/pypa/pyproject-metadata)
- pyproject_metadata/__init__.py:397:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyproject_metadata/__init__.py:398:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["project"]` and `PyProjectTable`
+ pyproject_metadata/__init__.py:427:20: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/__init__.py:438:23: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/__init__.py:462:35: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/__init__.py:469:31: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/__init__.py:500:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:503:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:505:44: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:508:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:512:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:516:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:520:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:524:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/__init__.py:527:21: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/project_table.py:142:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/project_table.py:144:17: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/project_table.py:146:24: error[missing-argument] No argument provided for required parameter `name`
+ pyproject_metadata/project_table.py:147:17: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/project_table.py:156:39: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ pyproject_metadata/pyproject.py:164:15: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/pyproject.py:215:25: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/pyproject.py:230:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["readme"]` and `ProjectTable`
+ pyproject_metadata/pyproject.py:305:35: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/pyproject.py:328:15: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `ProjectTable` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ pyproject_metadata/pyproject.py:379:15: error[no-matching-overload] No overload of bound method `get` matches arguments
+ pyproject_metadata/pyproject.py:422:19: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 4 diagnostics
+ Found 29 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/cargo/interpreter.py:485:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["workspace"]` and `Manifest`
+ mesonbuild/cargo/interpreter.py:487:14: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["package"]` and `Manifest`
- mesonbuild/cargo/manifest.py:304:16: error[missing-typed-dict-key] Missing required key 'workspace' in TypedDict `FromWorkspace` constructor
- mesonbuild/cargo/manifest.py:304:17: error[invalid-key] Unknown key "version" for TypedDict `FromWorkspace`: Unknown key "version"
+ mesonbuild/cargo/manifest.py:310:22: error[no-matching-overload] No overload of function `_depv_to_dep` matches arguments
+ mesonbuild/cargo/manifest.py:354:34: error[invalid-argument-type] Argument to function `_raw_to_dataclass` is incorrect: Expected `Mapping[str, object]`, found `LibTarget`
+ mesonbuild/cargo/manifest.py:370:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["path"], /) -> str, (key: Literal["test"], /) -> bool, (key: Literal["doctest"], /) -> bool, (key: Literal["bench"], /) -> bool, (key: Literal["doc"], /) -> bool, (key: Literal["plugin"], /) -> bool, (key: Literal["proc-macro"], /) -> bool, (key: Literal["harness"], /) -> bool, (key: Literal["edition"], /) -> Literal["2015", "2018", "2021"], (key: Literal["crate-type"], /) -> list[Literal["bin", "lib", "dylib", "staticlib", "cdylib", "rlib", "proc-macro"]], (key: Literal["required-features"], /) -> list[str]]` cannot be called with key of type `Literal["name"]` on object of type `BuildTarget`
+ mesonbuild/cargo/manifest.py:371:34: error[invalid-argument-type] Argument to function `_raw_to_dataclass` is incorrect: Expected `Mapping[str, object]`, found `BuildTarget`
+ mesonbuild/cargo/manifest.py:383:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["path"], /) -> str, (key: Literal["test"], /) -> bool, (key: Literal["doctest"], /) -> bool, (key: Literal["bench"], /) -> bool, (key: Literal["doc"], /) -> bool, (key: Literal["plugin"], /) -> bool, (key: Literal["proc-macro"], /) -> bool, (key: Literal["harness"], /) -> bool, (key: Literal["edition"], /) -> Literal["2015", "2018", "2021"], (key: Literal["crate-type"], /) -> list[Literal["bin", "lib", "dylib", "staticlib", "cdylib", "rlib", "proc-macro"]], (key: Literal["required-features"], /) -> list[str]]` cannot be called with key of type `Literal["name"]` on object of type `BuildTarget`
+ mesonbuild/cargo/manifest.py:384:34: error[invalid-argument-type] Argument to function `_raw_to_dataclass` is incorrect: Expected `Mapping[str, object]`, found `BuildTarget`
+ mesonbuild/cargo/manifest.py:398:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["path"], /) -> str, (key: Literal["test"], /) -> bool, (key: Literal["doctest"], /) -> bool, (key: Literal["bench"], /) -> bool, (key: Literal["doc"], /) -> bool, (key: Literal["plugin"], /) -> bool, (key: Literal["proc-macro"], /) -> bool, (key: Literal["harness"], /) -> bool, (key: Literal["edition"], /) -> Literal["2015", "2018", "2021"], (key: Literal["crate-type"], /) -> list[Literal["bin", "lib", "dylib", "staticlib", "cdylib", "rlib", "proc-macro"]], (key: Literal["required-features"], /) -> list[str]]` cannot be called with key of type `Literal["name"]` on object of type `BuildTarget`
+ mesonbuild/cargo/manifest.py:399:34: error[invalid-argument-type] Argument to function `_raw_to_dataclass` is incorrect: Expected `Mapping[str, object]`, found `BuildTarget`
+ mesonbuild/cargo/manifest.py:413:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["path"], /) -> str, (key: Literal["test"], /) -> bool, (key: Literal["doctest"], /) -> bool, (key: Literal["bench"], /) -> bool, (key: Literal["doc"], /) -> bool, (key: Literal["plugin"], /) -> bool, (key: Literal["proc-macro"], /) -> bool, (key: Literal["harness"], /) -> bool, (key: Literal["edition"], /) -> Literal["2015", "2018", "2021"], (key: Literal["crate-type"], /) -> list[Literal["bin", "lib", "dylib", "staticlib", "cdylib", "rlib", "proc-macro"]], (key: Literal["required-features"], /) -> list[str]]` cannot be called with key of type `Literal["name"]` on object of type `BuildTarget`
+ mesonbuild/cargo/manifest.py:414:34: error[invalid-argument-type] Argument to function `_raw_to_dataclass` is incorrect: Expected `Mapping[str, object]`, found `BuildTarget`
+ mesonbuild/cargo/manifest.py:445:33: error[no-matching-overload] No overload of bound method `get` matches arguments
+ mesonbuild/cargo/manifest.py:448:45: error[no-matching-overload] No overload of bound method `get` matches arguments
+ mesonbuild/cargo/manifest.py:568:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["package"]` and `Manifest`
+ mesonbuild/optinterpreter.py:202:38: error[invalid-argument-type] Argument to bound method `items` is incorrect: Argument type `FuncOptionArgs` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ mesonbuild/scripts/depaccumulate.py:61:18: error[no-matching-overload] No overload of bound method `get` matches arguments
+ mesonbuild/scripts/depaccumulate.py:81:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["provides"]` and `Rule`
+ mesonbuild/scripts/depaccumulate.py:83:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["requires"]` and `Rule`
+ mesonbuild/scripts/depaccumulate.py:85:27: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `Require` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
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
+ Found 2187 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/job.py:193:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["listing"]` and `DirectoryType`
+ cwltool/job.py:359:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["listing"]` and `DirectoryType`
- Found 254 diagnostics
+ Found 256 diagnostics

archinstall (https://github.com/archlinux/archinstall)
+ archinstall/lib/models/users.py:192:15: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `UserSerialization` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ archinstall/lib/models/users.py:194:13: error[no-matching-overload] No overload of bound method `get` matches arguments
+ archinstall/lib/models/users.py:195:16: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `UserSerialization` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ archinstall/lib/models/users.py:196:19: error[invalid-argument-type] Argument to bound method `get` is incorrect: Argument type `UserSerialization` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ archinstall/lib/models/users.py:210:10: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 67 diagnostics
+ Found 72 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/wrapper/_implementations.py:999:5: error[unsupported-operator] Operator `-` is not supported between objects of type `frozenset[Literal["name", "group", "package", "provider", "__kw", "to_config"]]` and `set[Unknown | str]`
+ src/hydra_zen/wrapper/_implementations.py:1438:41: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Argument type `_StoreCallSig` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
- src/hydra_zen/wrapper/_implementations.py:1572:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/hydra_zen/wrapper/_implementations.py:1565:13: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `_defaults` of type `_StoreCallSig`
+ src/hydra_zen/wrapper/_implementations.py:1565:28: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Argument type `_StoreCallSig` does not satisfy upper bound `dict[_KT@dict, _VT@dict]` of type variable `Self`
+ src/hydra_zen/wrapper/_implementations.py:1578:13: error[unresolved-attribute] Object of type `_StoreCallSig` has no attribute `update`
- Found 521 diagnostics
+ Found 525 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await]

... (truncated 157 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2026-01-15 20:36_

---

_Comment by @charliermarsh on 2026-01-15 20:48_

(I just wanted to see conformance, etc.)

---
