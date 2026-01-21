```yaml
number: 22317
title: "[ty] Respect `__new__` and metaclass `__call__` return types"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: charlie/metaclass
created_at: 2025-12-31T16:45:36Z
updated_at: 2026-01-21T13:49:34Z
url: https://github.com/astral-sh/ruff/pull/22317
synced_at: 2026-01-21T14:07:21Z
```

# [ty] Respect `__new__` and metaclass `__call__` return types

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/281.
Closes https://github.com/astral-sh/ty/issues/2288.


---

_Label `ty` added by @charliermarsh on 2025-12-31 16:45_

---

_Comment by @astral-sh-bot[bot] on 2025-12-31 16:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.47% to 77.79%. The percentage of expected errors that received a diagnostic increased from 60.67% to 60.85%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 674 | 676 | +2 | ⏫ (✅) |
| False Positives | 196 | 193 | -3 | ⏬ (✅) |
| False Negatives | 437 | 435 | -2 | ⏬ (✅) |
| Total Diagnostics | 870 | 869 | -1 | ⏬ |
| Precision | 77.47% | 77.79% | +0.32% | ⏫ (✅) |
| Recall | 60.67% | 60.85% | +0.18% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [constructors_call_type.py:30:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/constructors_call_type.py#L30) | missing-argument | No arguments provided for required parameters `x`, `y` of bound method `__call__` |
| [constructors_call_type.py:72:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/constructors_call_type.py#L72) | missing-argument | No arguments provided for required parameters `x`, `y` of bound method `__call__` |


</details>

### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [constructors_call_metaclass.py:39:1](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/constructors_call_metaclass.py#L39) | type-assertion-failure | Type `int \| Meta2` does not match asserted type `Class2` |
| [constructors_call_new.py:117:1](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/constructors_call_new.py#L117) | type-assertion-failure | Type `Class8[list[int]]` does not match asserted type `Class8[int]` |
| [constructors_call_new.py:118:1](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/constructors_call_new.py#L118) | type-assertion-failure | Type `Class8[list[str]]` does not match asserted type `Class8[str]` |


</details>



---

_Comment by @astral-sh-bot[bot] on 2025-12-31 16:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bandersnatch (https://github.com/pypa/bandersnatch)
+ src/bandersnatch/filter.py:41:13: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["plugins"]` and `type`
+ src/bandersnatch/filter.py:42:33: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/filter.py:46:25: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/filter.py:103:16: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/filter.py:107:16: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/filter.py:170:30: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/main.py:222:23: error[unresolved-attribute] Object of type `type` has no attribute `get`
+ src/bandersnatch/main.py:228:41: error[invalid-argument-type] Argument to function `async_main` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/storage.py:70:31: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/storage.py:79:17: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `ConfigParser | type`
+ src/bandersnatch/storage.py:88:25: warning[possibly-missing-attribute] Attribute `getint` may be missing on object of type `ConfigParser | type`
+ src/bandersnatch/storage.py:110:20: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | ConfigParser | type`
+ src/bandersnatch/storage.py:343:18: error[invalid-assignment] Object of type `type` is not assignable to `ConfigParser | None`
+ src/bandersnatch/storage.py:346:30: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ src/bandersnatch/tests/mock_config.py:15:5: error[unresolved-attribute] Object of type `type` has no attribute `clear`
+ src/bandersnatch/tests/mock_config.py:17:5: error[unresolved-attribute] Object of type `type` has no attribute `_read_defaults_file`
+ src/bandersnatch/tests/mock_config.py:19:5: error[unresolved-attribute] Object of type `type` has no attribute `read_string`
+ src/bandersnatch/tests/mock_config.py:20:12: error[invalid-return-type] Return type does not match returned value: expected `BandersnatchConfig`, found `type`
+ src/bandersnatch/tests/test_configuration.py:54:36: error[unresolved-attribute] Object of type `type` has no attribute `sections`
+ src/bandersnatch/tests/test_configuration.py:58:41: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:108:29: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:113:9: error[unresolved-attribute] Object of type `type` has no attribute `read_string`
+ src/bandersnatch/tests/test_configuration.py:115:26: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:119:9: error[unresolved-attribute] Object of type `type` has no attribute `read_string`
+ src/bandersnatch/tests/test_configuration.py:121:30: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:125:9: error[unresolved-attribute] Object of type `type` has no attribute `read_string`
+ src/bandersnatch/tests/test_configuration.py:127:26: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:131:9: error[unresolved-attribute] Object of type `type` has no attribute `read_string`
+ src/bandersnatch/tests/test_configuration.py:135:26: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:155:52: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_configuration.py:175:9: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:177:52: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_configuration.py:199:9: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:205:52: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_configuration.py:226:9: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:227:68: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_configuration.py:247:9: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:248:68: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_configuration.py:253:9: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch/tests/test_configuration.py:255:36: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_configuration.py:263:12: error[unresolved-attribute] Object of type `type` has no attribute `has_option`
+ src/bandersnatch/tests/test_configuration.py:264:13: error[unresolved-attribute] Object of type `type` has no attribute `remove_option`
+ src/bandersnatch/tests/test_configuration.py:265:41: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_configuration.py:320:17: error[unresolved-attribute] Object of type `type` has no attribute `read_dict`
+ src/bandersnatch/tests/test_configuration.py:321:56: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_delete.py:82:5: error[unresolved-attribute] Object of type `type` has no attribute `read_dict`
+ src/bandersnatch/tests/test_filter.py:122:9: error[unresolved-attribute] Object of type `type` has no attribute `read_string`
+ src/bandersnatch/tests/test_mirror.py:1251:5: error[unresolved-attribute] Object of type `type` has no attribute `read_dict`
+ src/bandersnatch/tests/test_mirror.py:1287:22: error[invalid-argument-type] Argument to function `mirror` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_mirror.py:1334:5: error[unresolved-attribute] Object of type `type` has no attribute `read_dict`
+ src/bandersnatch/tests/test_mirror.py:1372:22: error[invalid-argument-type] Argument to function `mirror` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch/tests/test_simple.py:50:37: error[invalid-argument-type] Argument to function `validate_config_values` is incorrect: Expected `ConfigParser`, found `type`
+ src/bandersnatch_filter_plugins/latest_name.py:30:29: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/latest_name.py:38:23: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/metadata_filter.py:36:36: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/metadata_filter.py:36:36: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/metadata_filter.py:198:38: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/metadata_filter.py:271:36: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/prerelease_name.py:40:25: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/regex_name.py:26:22: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_filter_plugins/regex_name.py:60:22: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_storage_plugins/s3.py:139:21: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_storage_plugins/s3.py:159:25: error[not-subscriptable] Cannot subscript object of type `type` with no `__class_getitem__` method
+ src/bandersnatch_storage_plugins/s3.py:163:43: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `Unknown | ConfigParser | type`
- Found 78 diagnostics
+ Found 142 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ testing/test_nodes.py:33:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 407 diagnostics
+ Found 408 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/type/definition.py:438:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLScalarType`, found `GraphQLNamedType`
+ src/graphql/type/definition.py:768:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLObjectType`, found `GraphQLNamedType`
+ src/graphql/type/definition.py:872:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInterfaceType`, found `GraphQLNamedType`
+ src/graphql/type/definition.py:975:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLUnionType`, found `GraphQLNamedType`
+ src/graphql/type/definition.py:1110:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLEnumType`, found `GraphQLNamedType`
+ src/graphql/type/definition.py:1342:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInputObjectType`, found `GraphQLNamedType`
+ src/graphql/type/directives.py:159:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/directives.py:176:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/directives.py:191:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/directives.py:195:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/directives.py:207:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/directives.py:211:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/directives.py:213:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/directives.py:234:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/directives.py:252:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:48:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:103:30: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLObjectType`
+ src/graphql/type/introspection.py:118:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:122:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:126:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:137:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:170:33: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLObjectType`
+ src/graphql/type/introspection.py:183:39: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLEnumType`
+ src/graphql/type/introspection.py:272:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:273:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:274:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:279:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:295:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:304:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:310:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:405:28: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLObjectType`
+ src/graphql/type/introspection.py:424:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:425:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:430:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:437:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:441:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:475:29: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLObjectType`
+ src/graphql/type/introspection.py:487:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:489:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:493:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:499:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:503:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:536:34: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLObjectType`
+ src/graphql/type/introspection.py:549:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:552:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:555:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:559:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ src/graphql/type/introspection.py:580:33: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLObjectType`
+ src/graphql/type/introspection.py:603:30: error[invalid-assignment] Object of type `GraphQLNamedType` is not assignable to `GraphQLEnumType`
+ src/graphql/type/introspection.py:672:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/introspection.py:678:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ src/graphql/type/scalars.py:307:59: error[invalid-assignment] Object of type `dict[str, GraphQLNamedType]` is not assignable to `Mapping[str, GraphQLScalarType]`
+ src/graphql/utilities/build_client_schema.py:155:20: error[invalid-return-type] Return type does not match returned value: expected `GraphQLScalarType`, found `GraphQLNamedType`
+ src/graphql/utilities/build_client_schema.py:186:20: error[invalid-return-type] Return type does not match returned value: expected `GraphQLObjectType`, found `GraphQLNamedType`
+ src/graphql/utilities/build_client_schema.py:196:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInterfaceType`, found `GraphQLNamedType`
+ src/graphql/utilities/build_client_schema.py:216:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLUnionType`, found `GraphQLNamedType`
+ src/graphql/utilities/build_client_schema.py:233:20: error[invalid-return-type] Return type does not match returned value: expected `GraphQLEnumType`, found `GraphQLNamedType`
+ src/graphql/utilities/build_client_schema.py:255:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInputObjectType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:352:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInputObjectType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:367:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLEnumType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:384:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLScalarType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLObjectType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:457:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInterfaceType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:482:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLUnionType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:683:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLObjectType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:702:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInterfaceType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:718:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLEnumType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:733:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLUnionType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:746:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLScalarType`, found `GraphQLNamedType`
+ src/graphql/utilities/extend_schema.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `GraphQLInputObjectType`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_async.py:14:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_async.py:15:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_async.py:28:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_async.py:32:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_sync.py:12:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_sync.py:13:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_sync.py:26:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/benchmarks/test_execution_sync.py:30:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/benchmarks/test_repeated_fields.py:10:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/benchmarks/test_repeated_fields.py:14:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:96:70: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:101:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:102:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:104:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLInterfaceType]) | Collection[GraphQLInterfaceType] | None`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:111:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:112:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:114:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLInterfaceType]) | Collection[GraphQLInterfaceType] | None`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:119:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:160:70: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:165:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:166:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:168:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLInterfaceType]) | Collection[GraphQLInterfaceType] | None`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:175:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:176:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:178:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLInterfaceType]) | Collection[GraphQLInterfaceType] | None`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:183:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:231:70: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:236:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:237:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:239:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLInterfaceType]) | Collection[GraphQLInterfaceType] | None`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:244:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:283:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:284:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:292:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:293:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:298:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLObjectType]) | Collection[GraphQLObjectType]`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:301:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:344:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:351:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:352:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:354:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLInterfaceType]) | Collection[GraphQLInterfaceType] | None`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:360:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:361:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_abstract.py:363:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> Collection[GraphQLInterfaceType]) | Collection[GraphQLInterfaceType] | None`, found `list[Unknown | GraphQLNamedType]`
+ tests/execution/test_abstract.py:367:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:17:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:17:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:33:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:35:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:79:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:79:67: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:80:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:81:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:123:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:123:67: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:124:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_customize.py:128:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:42:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:43:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:44:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ tests/execution/test_defer.py:59:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:60:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:61:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:62:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:68:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:68:72: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:72:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:86:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:87:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ tests/execution/test_defer.py:94:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:101:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:102:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:109:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:110:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:117:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:124:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:125:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:126:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements` of type variable `GNT_co`
+ tests/execution/test_defer.py:128:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:129:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:135:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:135:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:135:79: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_defer.py:138:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_directives.py:6:5: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_directives.py:7:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_directives.py:7:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:42:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLObjectType | None`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:44:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:103:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:104:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:105:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:106:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:107:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:108:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:110:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 4 union elements`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:111:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLNamedType`
+ tests/execution/test_executor.py:115:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expec

... (truncated 2710 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-31 16:52_

(Needs work, intentionally in draft, not ready for review.)

---

_Comment by @charliermarsh on 2025-12-31 18:00_

Hmm, I think the bandersnatch diagnostics are technically correct? It uses a singleton:

```python
class Singleton(type):  # pragma: no cover
    _instances: dict["Singleton", type] = {}

    def __call__(cls, *args: Any, **kwargs: Any) -> type:
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]
```

But the return type is not `type`, it's an instance?

Mypy seems to ignore it, and Pyright seemingly synthesizes a return type `type[__class_type]`?

---

_Comment by @charliermarsh on 2025-12-31 18:12_

I think it's this? https://github.com/python/mypy/blob/c5c12fad9e69525fa5b12058b328d284c5feecc4/mypy/typeops.py#L1248

---

_Comment by @charliermarsh on 2025-12-31 19:39_

I think our behavior here is "correct" but it's stricter than mypy or pyright. Here are two examples (from bandersnatch and graphql-core respectively).

For bandersnatch:

```python
class Singleton(type):
    def __call__(cls, *args, **kwargs) -> type:
        ...

class Config(metaclass=Singleton): ...

Config().method()  # ty: error (type has no method)
```

Meanwhile mypy and pyright both allow this.

For graphql-core:

```python
class Base:
    def __new__(cls, name: str) -> "Base":  # should be -> Self
        return super().__new__(cls)

class Derived(Base):
    def __copy__(self) -> "Derived":
        return self.__class__("test")  # ty: error (returns Base)
```

Again, mypy and pyright both allow this.


---

_Marked ready for review by @charliermarsh on 2025-12-31 19:39_

---

_Review requested from @carljm by @charliermarsh on 2025-12-31 19:39_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-31 19:39_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-31 19:39_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-31 19:39_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-31 19:39_

---

_Label `ecosystem-analyzer` added by @charliermarsh on 2025-12-31 19:42_

---

_Comment by @astral-sh-bot[bot] on 2025-12-31 19:47_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 1,177 | 9 | 24 |
| `possibly-missing-attribute` | 735 | 13 | 26 |
| `unresolved-attribute` | 249 | 6 | 26 |
| `unsupported-operator` | 101 | 2 | 111 |
| `invalid-return-type` | 63 | 4 | 5 |
| `invalid-assignment` | 31 | 1 | 6 |
| `not-subscriptable` | 30 | 0 | 0 |
| `too-many-positional-arguments` | 16 | 0 | 0 |
| `unused-ignore-comment` | 7 | 9 | 0 |
| `no-matching-overload` | 11 | 0 | 0 |
| `invalid-key` | 10 | 0 | 0 |
| `missing-argument` | 3 | 7 | 0 |
| `invalid-parameter-default` | 1 | 0 | 7 |
| `redundant-cast` | 1 | 4 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `type-assertion-failure` | 0 | 2 | 0 |
| `unknown-argument` | 1 | 0 | 0 |
| **Total** | **2,438** | **57** | **205** |


**[Full report with detailed diff](https://80b83f30.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://80b83f30.ty-ecosystem-ext.pages.dev/timing))



---

_Converted to draft by @charliermarsh on 2026-01-21 04:25_

---
