```yaml
number: 18214
title: "[ty] Experiment: half-baked typing.TypeAlias support"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/type-alias
created_at: 2025-05-20T07:44:30Z
updated_at: 2025-06-06T12:14:05Z
url: https://github.com/astral-sh/ruff/pull/18214
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Experiment: half-baked typing.TypeAlias support

---

_Pull request opened by @sharkdp on 2025-05-20 07:44_

## Summary

This (ab)uses `KnownInstanceType::TypeAliasType` to support type aliases that are declared with `typing.TypeAlias`.

would close:

https://github.com/astral-sh/ty/issues/405
https://github.com/astral-sh/ty/issues/433
https://github.com/astral-sh/ty/issues/368

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-05-20 07:44_

---

_Comment by @github-actions[bot] on 2025-05-20 09:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
- warning[unused-ignore-comment] dacite/types.py:180:54: Unused blanket `type: ignore` directive
- Found 24 diagnostics
+ Found 23 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[unsupported-operator] mypy_primer/main.py:135:42: Operator `<` is not supported for types `_version_info` and `None`, in comparing `_version_info` with `tuple[int, int] | None`
- Found 17 diagnostics
+ Found 16 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ error[no-matching-overload] aioredis/connection.py:1222:30: No overload of function `__new__` matches arguments
- Found 28 diagnostics
+ Found 29 diagnostics

beartype (https://github.com/beartype/beartype)
- warning[unused-ignore-comment] beartype/_util/cls/pep/clspep3119.py:766:32: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] beartype/_util/hint/pep/proposal/pep544.py:133:48: Unused blanket `type: ignore` directive
- Found 551 diagnostics
+ Found 549 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- warning[unused-ignore-comment] chess/__init__.py:4066:63: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] chess/__init__.py:4232:56: Unused blanket `type: ignore` directive
+ error[no-matching-overload] chess/engine.py:1521:50: No overload of function `__new__` matches arguments
+ error[no-matching-overload] chess/engine.py:1523:32: No overload of function `__new__` matches arguments
+ error[no-matching-overload] chess/engine.py:1526:39: No overload of function `__new__` matches arguments
+ error[no-matching-overload] chess/engine.py:1529:39: No overload of function `__new__` matches arguments
+ error[no-matching-overload] chess/engine.py:1532:39: No overload of function `__new__` matches arguments
+ error[no-matching-overload] chess/engine.py:2203:56: No overload of function `__new__` matches arguments
+ error[no-matching-overload] chess/engine.py:2205:56: No overload of function `__new__` matches arguments
+ error[no-matching-overload] chess/engine.py:2264:49: No overload of function `max` matches arguments
- error[invalid-return-type] chess/gaviota.py:1670:16: Return type does not match returned value: expected `BinaryIO`, found `(Unknown & ~None) | TextIOWrapper[_WrappedBuffer]`
- error[invalid-argument-type] chess/gaviota.py:1892:35: Argument to bound method `__init__` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None | Literal[64]`
+ error[invalid-argument-type] chess/gaviota.py:1892:35: Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] chess/polyglot.py:262:59: Argument to function `square_file` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None`
+ error[invalid-argument-type] chess/polyglot.py:262:59: Argument to function `square_file` is incorrect: Expected `int`, found `int | None`
- warning[unused-ignore-comment] chess/polyglot.py:404:31: Unused blanket `type: ignore` directive
- Found 37 diagnostics
+ Found 41 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[no-matching-overload] pyinstrument/__main__.py:301:13: No overload of function `open` matches arguments
- error[invalid-return-type] pyinstrument/session.py:215:16: Return type does not match returned value: expected `str`, found `str | bytes`
+ error[no-matching-overload] pyinstrument/vendor/appdirs.py:102:24: No overload of function `join` matches arguments
+ error[no-matching-overload] pyinstrument/vendor/appdirs.py:155:24: No overload of function `join` matches arguments
+ error[no-matching-overload] pyinstrument/vendor/appdirs.py:322:24: No overload of function `join` matches arguments
+ error[no-matching-overload] pyinstrument/vendor/appdirs.py:415:16: No overload of function `join` matches arguments
- Found 88 diagnostics
+ Found 92 diagnostics

python-sop (https://gitlab.com/dkg/python-sop)
+ All checks passed!
- error[invalid-assignment] sop/__init__.py:380:13: Object of type `TextIOWrapper[_WrappedBuffer]` is not assignable to `BinaryIO`
- error[invalid-assignment] sop/__init__.py:382:13: Object of type `TextIOWrapper[_WrappedBuffer]` is not assignable to `BinaryIO`
- Found 2 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[invalid-argument-type] tests/test_validators.py:185:68: Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
+ error[invalid-argument-type] tests/test_validators.py:185:68: Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
- error[invalid-argument-type] tests/test_validators.py:219:56: Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
+ error[invalid-argument-type] tests/test_validators.py:219:56: Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`
- error[invalid-argument-type] tests/typing_example.py:221:64: Argument to function `matches_re` is incorrect: Expected `((Literal["foo"], Literal["foo"], int, /) -> Match[Literal["foo"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
+ error[invalid-argument-type] tests/typing_example.py:221:64: Argument to function `matches_re` is incorrect: Expected `((Literal["foo"], Literal["foo"], int, /) -> Match[Literal["foo"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = Literal[0]) -> Match[bytes] | None]`

anyio (https://github.com/agronholm/anyio)
- error[invalid-return-type] src/anyio/_backends/_trio.py:553:32: Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, @Todo(Support for `typing.TypeAlias`)]`
+ error[invalid-return-type] src/anyio/_backends/_trio.py:553:32: Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, tuple[str, int]]`
+ error[invalid-return-type] src/anyio/_backends/_trio.py:596:32: Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, str]`
+ error[no-matching-overload] src/anyio/_backends/_trio.py:1123:28: No overload of function `fspath` matches arguments
- Found 118 diagnostics
+ Found 120 diagnostics

starlette (https://github.com/encode/starlette)
+ error[no-matching-overload] starlette/staticfiles.py:79:50: No overload of function `join` matches arguments
- Found 176 diagnostics
+ Found 177 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ error[invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:257:9: Argument to function `merge_robot_options` is incorrect: Expected `Mapping[str, object]`, found `RobotOptions`
+ error[invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:277:9: Argument to function `merge_robot_options` is incorrect: Expected `Mapping[str, object]`, found `RobotOptions`
- Found 228 diagnostics
+ Found 230 diagnostics

kornia (https://github.com/kornia/kornia)
- warning[unused-ignore-comment] kornia/core/mixin/image_module.py:115:45: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] kornia/core/mixin/onnx.py:307:47: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] kornia/core/module.py:121:45: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] kornia/utils/draw.py:377:29: Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
- Found 969 diagnostics
+ Found 967 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ error[invalid-argument-type] src/graphql/execution/collect_fields.py:152:9: Argument is incorrect: Expected `dict[str, FieldGroup]`, found `list[DeferUsage]`
+ error[invalid-argument-type] src/graphql/execution/collect_fields.py:196:9: Argument is incorrect: Expected `dict[str, FieldGroup]`, found `list[DeferUsage]`
+ error[invalid-argument-type] src/graphql/execution/collect_fields.py:356:35: Argument to bound method `is_sub_type` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLNamedType | None`
+ warning[possibly-unbound-attribute] src/graphql/execution/execute.py:654:17: Attribute `of_type` on type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]` is possibly unbound
+ error[invalid-argument-type] src/graphql/execution/values.py:106:49: Argument to function `value_from_ast` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLType | None`
+ error[invalid-argument-type] src/graphql/execution/values.py:151:20: Argument to function `coerce_input_value` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLType | None`
+ error[invalid-argument-type] src/graphql/language/parser.py:266:29: Argument to bound method `__init__` is incorrect: Expected `Source`, found `Source | str`
- warning[possibly-unbound-attribute] src/graphql/type/definition.py:1667:17: Attribute `of_type` on type `@Todo(Inference of subscript on special form) | GraphQLNonNull[Unknown] | None` is possibly unbound
- warning[unused-ignore-comment] src/graphql/type/schema.py:399:52: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] src/graphql/type/definition.py:1667:17: Attribute `of_type` on type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[Unknown] | None` is possibly unbound
+ error[invalid-base] src/graphql/type/introspection.py:42:20: Invalid class base with type `typing.TypeAliasType` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] src/graphql/type/introspection.py:110:23: Invalid class base with type `typing.TypeAliasType` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] src/graphql/type/introspection.py:266:18: Invalid class base with type `typing.TypeAliasType` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] src/graphql/type/introspection.py:421:19: Invalid class base with type `typing.TypeAliasType` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] src/graphql/type/introspection.py:485:24: Invalid class base with type `typing.TypeAliasType` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[invalid-base] src/graphql/type/introspection.py:546:23: Invalid class base with type `typing.TypeAliasType` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ warning[possibly-unbound-attribute] src/graphql/type/schema.py:328:13: Attribute `types` on type `GraphQLInterfaceType | GraphQLUnionType` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/type/schema.py:354:30: Attribute `types` on type `GraphQLInterfaceType | GraphQLUnionType` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/ast_from_value.py:63:43: Attribute `of_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/ast_from_value.py:79:21: Attribute `of_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/ast_from_value.py:93:38: Attribute `fields` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/ast_from_value.py:106:22: Attribute `serialize` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ error[invalid-argument-type] src/graphql/utilities/build_client_schema.py:301:13: Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]`, found `GraphQLType`
+ error[invalid-argument-type] src/graphql/utilities/build_client_schema.py:331:75: Argument to function `value_from_ast` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLType`
+ error[invalid-argument-type] src/graphql/utilities/build_client_schema.py:334:13: Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLType`
+ error[invalid-argument-type] src/graphql/utilities/build_client_schema.py:368:75: Argument to function `value_from_ast` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLType`
+ error[invalid-argument-type] src/graphql/utilities/build_client_schema.py:371:13: Argument to bound method `__init__` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]`, found `GraphQLType`
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:57:52: Attribute `of_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:72:21: Attribute `of_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:91:48: Attribute `name` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:96:18: Attribute `fields` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:118:75: Attribute `name` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:129:73: Attribute `name` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:134:12: Attribute `is_one_of` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:142:45: Attribute `name` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/coerce_input_value.py:157:16: Attribute `out_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ error[invalid-argument-type] src/graphql/utilities/lexicographic_sort_schema.py:53:35: Argument to bound method `__init__` is incorrect: Argument type `GraphQLList[Unknown] | GraphQLNonNull[Unknown] | GraphQLNamedType` does not satisfy upper bound of type variable `GNT_co`
+ error[invalid-argument-type] src/graphql/utilities/type_comparators.py:79:32: Argument to bound method `is_sub_type` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLType`
+ error[invalid-argument-type] src/graphql/utilities/type_comparators.py:105:36: Argument to bound method `is_sub_type` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType`
+ error[invalid-argument-type] src/graphql/utilities/type_comparators.py:106:56: Argument to bound method `get_possible_types` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType`
+ error[invalid-argument-type] src/graphql/utilities/type_comparators.py:109:35: Argument to bound method `is_sub_type` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType`
+ error[invalid-argument-type] src/graphql/utilities/type_comparators.py:113:35: Argument to bound method `is_sub_type` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType`
+ warning[possibly-unbound-attribute] src/graphql/utilities/value_from_ast.py:73:43: Attribute `of_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/value_from_ast.py:79:21: Attribute `of_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/value_from_ast.py:105:18: Attribute `fields` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/value_from_ast.py:121:12: Attribute `is_one_of` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/value_from_ast.py:129:16: Attribute `out_type` on type `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | GraphQLList[Unknown]]` is possibly unbound
+ error[invalid-argument-type] src/graphql/validation/rules/fields_on_correct_type.py:83:52: Argument to bound method `get_possible_types` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]`
+ error[invalid-argument-type] src/graphql/validation/rules/fields_on_correct_type.py:109:61: Argument to bound method `is_sub_type` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLObjectType | GraphQLInterfaceType`
+ error[invalid-argument-type] src/graphql/validation/rules/fields_on_correct_type.py:111:61: Argument to bound method `is_sub_type` is incorrect: Expected `GraphQLInterfaceType | GraphQLUnionType`, found `GraphQLObjectType | GraphQLInterfaceType`
+ warning[possibly-unbound-attribute] src/graphql/validation/rules/fields_on_correct_type.py:132:37: Attribute `fields` on type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]` is possibly unbound
+ error[invalid-return-type] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:50:12: Return type does not match returned value: expected `str`, found `(str & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])`
+ error[no-matching-overload] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:586:13: No overload of function `get_named_type` matches arguments
+ error[no-matching-overload] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:588:13: No overload of function `get_named_type` matches arguments
+ warning[possibly-unbound-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:659:31: Attribute `of_type` on type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:659:46: Attribute `of_type` on type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:667:31: Attribute `of_type` on type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:667:46: Attribute `of_type` on type `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown] | GraphQLNonNull[GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | GraphQLEnumType | GraphQLList[Unknown]]` is possibly unbound
+ error[invalid-return-type] src/graphql/validation/rules/possible_fragment_spreads.py:67:24: Return type does not match returned value: expected `GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | None`, found `GraphQLNamedType | None`
- warning[unused-ignore-comment] tests/type/test_definition.py:947:55: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/type/test_definition.py:955:57: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] tests/type/test_validation.py:74:24: Argument to bound method `__init__` is incorrect: Argument type `GraphQLNamedType` does not satisfy upper bound of type variable `GNT_co`
- warning[unused-ignore-comment] tests/type/test_validation.py:1016:51: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/type/test_validation.py:1311:54: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/type/test_validation.py:1425:42: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/type/test_validation.py:1542:50: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] tests/utilities/test_build_ast_schema.py:75:22: Argument to function `print_ast` is incorrect: Expected `Node`, found `InputValueDefinitionNode | None | EnumValueDefinitionNode | FieldDefinitionNode | TypeDefinitionNode`
+ error[invalid-argument-type] tests/utilities/test_extend_schema.py:65:22: Argument to function `print_ast` is incorrect: Expected `Node`, found `InputValueDefinitionNode | None | EnumValueDefinitionNode | FieldDefinitionNode | TypeDefinitionNode | SchemaDefinitionNode`
+ error[no-matching-overload] tests/utilities/test_type_info.py:341:43: No overload of function `get_named_type` matches arguments
- Found 410 diagnostics
+ Found 463 diagnostics

kopf (https://github.com/nolar/kopf)
+ error[invalid-argument-type] kopf/_cogs/clients/auth.py:110:50: Argument to function `b64decode` is incorrect: Expected `str | Buffer`, found `bytes | None`
+ error[invalid-argument-type] kopf/_cogs/clients/auth.py:119:59: Argument to function `b64decode` is incorrect: Expected `str | Buffer`, found `bytes | None`
+ error[invalid-argument-type] kopf/_cogs/clients/auth.py:128:59: Argument to function `b64decode` is incorrect: Expected `str | Buffer`, found `bytes | None`
- Found 167 diagnostics
+ Found 170 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[no-matching-overload] src/check_jsonschema/cachedownloader.py:82:5: No overload of bound method `write` matches arguments
- Found 67 diagnostics
+ Found 66 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ error[unresolved-attribute] test/generated/testproto/nested/nested_pb2.pyi:80:13: Type `typing.TypeAliasType` has no attribute `NestedEnum`
+ error[unresolved-attribute] test/generated/testproto/nested/nested_pb2.pyi:81:14: Type `typing.TypeAliasType` has no attribute `NestedMessage`
+ error[unresolved-attribute] test/generated/testproto/nested/nested_pb2.pyi:87:17: Type `typing.TypeAliasType` has no attribute `NestedEnum`
+ error[unresolved-attribute] test/generated/testproto/nested/nested_pb2.pyi:88:18: Type `typing.TypeAliasType` has no attribute `NestedMessage`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:144:19: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:145:17: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:148:26: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:149:26: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:152:16: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:180:23: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:182:21: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:186:30: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:187:30: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test3_pb2.pyi:191:20: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:148:13: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:150:17: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:154:26: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:155:26: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:165:98: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:167:32: Type `typing.TypeAliasType` has no attribute `InnerMessage`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:169:104: Type `typing.TypeAliasType` has no attribute `InnerMessage`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:185:17: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:189:21: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:190:50: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:191:24: Type `typing.TypeAliasType` has no attribute `InnerMessage`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:192:53: Type `typing.TypeAliasType` has no attribute `InnerMessage`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:199:30: Type `typing.TypeAliasType` has no attribute `ValueType`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:200:30: Type `typing.TypeAliasType` has no attribute `InnerEnum`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:341:12: Type `typing.TypeAliasType` has no attribute `_r_finally`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:350:16: Type `typing.TypeAliasType` has no attribute `_r_finally`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:407:45: Type `typing.TypeAliasType` has no attribute `_r_lambda`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:408:36: Type `typing.TypeAliasType` has no attribute `_r_lambda`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:428:45: Type `typing.TypeAliasType` has no attribute `_r_lambda`
+ error[unresolved-attribute] test/generated/testproto/test_pb2.pyi:429:36: Type `typing.TypeAliasType` has no attribute `_r_lambda`
- Found 46 diagnostics
+ Found 80 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ error[no-matching-overload] porcupine/pluginmanager.py:34:18: No overload of function `open` matches arguments
- Found 81 diagnostics
+ Found 82 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ error[no-matching-overload] aiohttp_devtools/runserver/serve.py:140:28: No overload of bound method `create_server` matches arguments
- Found 65 diagnostics
+ Found 66 diagnostics

rich (https://github.com/Textualize/rich)
- error[invalid-argument-type] rich/progress.py:1373:26: Argument to bound method `__init__` is incorrect: Expected `BinaryIO`, found `TextIOWrapper[_WrappedBuffer]`
+ error[no-matching-overload] rich/progress.py:678:19: No overload of function `max` matches arguments
+ error[no-matching-overload] rich/progress.py:836:38: No overload of function `__new__` matches arguments
+ error[no-matching-overload] rich/progress.py:859:17: No overload of function `__new__` matches arguments
+ error[no-matching-overload] rich/progress.py:885:13: No overload of function `__new__` matches arguments
+ error[no-matching-overload] rich/progress.py:905:21: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] tests/test_traceback.py:29:28: Argument is incorrect: Expected `type[BaseException]`, found `type[BaseException] | None`
+ error[invalid-argument-type] tests/test_traceback.py:29:38: Argument is incorrect: Expected `BaseException`, found `BaseException | None`
+ error[invalid-argument-type] tests/test_traceback.py:325:42: Argument to bound method `from_exception` is incorrect: Expected `type[Any]`, found `type[BaseException] | None`
+ error[invalid-argument-type] tests/test_traceback.py:325:52: Argument to bound method `from_exception` is incorrect: Expected `BaseException`, found `BaseException | None`
- Found 391 diagnostics
+ Found 399 diagnostics

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
+ error[unresolved-attribute] pyi.py:654:15: Type `expr` has no attribute `elts`
+ error[unresolved-attribute] pyi.py:1483:70: Type `expr` has no attribute `value`
+ error[unresolved-attribute] pyi.py:1909:24: Type `expr` has no attribute `elts`
+ error[unresolved-attribute] pyi.py:1915:63: Type `expr` has no attribute `elts`
+ error[unresolved-attribute] pyi.py:1940:24: Type `expr` has no attribute `elts`
+ error[unresolved-attribute] pyi.py:1943:54: Type `expr` has no attribute `elts`
+ error[unresolved-attribute] pyi.py:2060:27: Type `expr` has no attribute `id`
- Found 27 diagnostics
+ Found 34 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-argument-type] src/jinja2/bccache.py:277:34: Argument to bound method `load_bytecode` is incorrect: Expected `BinaryIO`, found `TextIOWrapper[_WrappedBuffer]`
- error[invalid-argument-type] src/jinja2/bccache.py:302:39: Argument to bound method `write_bytecode` is incorrect: Expected `IO[bytes]`, found `_TemporaryFileWrapper[str]`
- error[invalid-assignment] src/jinja2/environment.py:1613:13: Object of type `TextIOWrapper[_WrappedBuffer]` is not assignable to `IO[bytes]`
- Found 234 diagnostics
+ Found 231 diagnostics

yarl (https://github.com/aio-libs/yarl)
+ error[no-matching-overload] yarl/_url.py:1598:36: No overload of function `max` matches arguments
- Found 159 diagnostics
+ Found 160 diagnostics

flake8 (https://github.com/pycqa/flake8)
+ error[no-matching-overload] tests/unit/plugins/pycodestyle_test.py:32:10: No overload of function `open` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-assignment] src/werkzeug/datastructures/file_storage.py:129:13: Object of type `TextIOWrapper[_WrappedBuffer]` is not assignable to `str | PathLike[str] | IO[bytes]`
- error[invalid-argument-type] src/werkzeug/datastructures/file_storage.py:133:38: Argument to function `copyfileobj` is incorrect: Expected `SupportsWrite[Unknown]`, found `IO[bytes] | (Unknown & ~str) | str | PathLike[str]`
- warning[possibly-unbound-attribute] src/werkzeug/datastructures/file_storage.py:136:17: Attribute `close` on type `IO[bytes] | (Unknown & ~str) | str | PathLike[str]` is possibly unbound
- error[invalid-assignment] src/werkzeug/datastructures/file_storage.py:196:13: Object of type `TextIOWrapper[_WrappedBuffer]` is not assignable to `IO[bytes]`
+ error[invalid-argument-type] src/werkzeug/http.py:1218:38: Argument to function `parse_cookie` is incorrect: Expected `str | None`, found `(Unknown & ~AlwaysTruthy) | None | (dict[str, Any] & ~dict[Unknown, Unknown] & ~AlwaysTruthy) | (str & ~dict[Unknown, Unknown] & ~AlwaysTruthy) | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[call-non-callable] src/werkzeug/serving.py:367:21: Object of type `object` is not callable
- error[invalid-return-type] src/werkzeug/test.py:442:16: Return type does not match returned value: expected `str`, found `bytes`
+ error[unsupported-operator] src/werkzeug/test.py:442:16: Operator `+` is unsupported between objects of type `bytes` and `Literal["/"]`
+ error[invalid-argument-type] src/werkzeug/test.py:442:71: Argument to bound method `rstrip` is incorrect: Expected `Buffer | None`, found `Literal["/"]`
- error[invalid-argument-type] src/werkzeug/utils.py:486:35: Argument to function `wrap_file` is incorrect: Expected `IO[bytes]`, found `IO[bytes] | None`
+ error[missing-argument] src/werkzeug/wrappers/request.py:196:24: No arguments provided for required parameters 1, 2
+ error[missing-argument] src/werkzeug/wsgi.py:28:38: No arguments provided for required parameters 1, 2
+ error[no-matching-overload] src/werkzeug/wsgi.py:78:28: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] tests/middleware/test_shared_data.py:10:32: Argument to bound method `__init__` is incorrect: Expected `(dict[str, Any], StartResponse, /) -> Iterable[bytes]`, found `None`
- warning[possibly-unbound-attribute] tests/test_debug.py:198:17: Attribute `reset` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_debug.py:198:17: Attribute `reset` on type `TextIO | Any` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_debug.py:200:17: Attribute `reset` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_debug.py:200:17: Attribute `reset` on type `TextIO | Any` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_debug.py:215:17: Attribute `reset` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_debug.py:215:17: Attribute `reset` on type `TextIO | Any` is possibly unbound
+ error[invalid-argument-type] tests/test_routing.py:473:13: Argument to bound method `force_type` is incorrect: Expected `Response`, found `Unknown | ((dict[str, Any], StartResponse, /) -> Iterable[bytes])`
+ error[invalid-argument-type] tests/test_test.py:725:46: Argument to function `run_wsgi_app` is incorrect: Expected `(dict[str, Any], StartResponse, /) -> Iterable[bytes]`, found `<class 'app'> | Unknown`
+ error[invalid-argument-type] tests/test_test.py:725:46: Argument to function `run_wsgi_app` is incorrect: Expected `(dict[str, Any], StartResponse, /) -> Iterable[bytes]`, found `<class 'app'> | Unknown`
+ error[invalid-argument-type] tests/test_test.py:725:46: Argument to function `run_wsgi_app` is incorrect: Expected `(dict[str, Any], StartResponse, /) -> Iterable[bytes]`, found `<class 'app'> | Unknown`
+ error[invalid-argument-type] tests/test_test.py:725:46: Argument to function `run_wsgi_app` is incorrect: Expected `(dict[str, Any], StartResponse, /) -> Iterable[bytes]`, found `<class 'app'> | Unknown`
- Found 454 diagnostics
+ Found 461 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ error[invalid-argument-type] mkosi/__init__.py:520:22: Argument to function `chmod` is incorrect: Expected `int | str | bytes | PathLike[str]`, found `Unknown | None`
+ error[invalid-argument-type] mkosi/__init__.py:526:26: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | None`
+ error[invalid-argument-type] mkosi/__init__.py:527:32: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | None`
+ error[no-matching-overload] mkosi/__init__.py:2400:9: No overload of function `copy2` matches arguments
+ error[invalid-argument-type] mkosi/__init__.py:2524:17: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | str | None`
+ error[no-matching-overload] mkosi/__init__.py:4248:13: No overload of function `copy2` matches arguments
+ error[invalid-argument-type] mkosi/mounts.py:35:21: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
+ error[no-matching-overload] mkosi/qemu.py:768:9: No overload of function `copy` matches arguments
+ error[invalid-argument-type] mkosi/run.py:84:14: Argument is incorrect: Expected `int`, found `int | str | None`
- Found 247 diagnostics
+ Found 256 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ error[invalid-argument-type] src/poetry/json/__init__.py:15:16: Argument to function `files` is incorrect: Expected `str | ModuleType`, found `str | None`
+ error[invalid-argument-type] src/poetry/utils/helpers.py:250:21: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `bytes`
- Found 1015 diagnostics
+ Found 1017 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
+ error[no-matching-overload] dedupe/clustering.py:346:38: No overload of function `__new__` matches arguments
+ error[no-matching-overload] dedupe/clustering.py:374:38: No overload of function `__new__` matches arguments
+ error[no-matching-overload] dedupe/clustering.py:380:38: No overload of function `__new__` matches arguments
- Found 63 diagnostics
+ Found 66 diagnostics

ignite (https://github.com/pytorch/ignite)
+ error[invalid-argument-type] examples/references/classification/imagenet/main.py:289:17: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[invalid-argument-type] examples/references/classification/imagenet/main.py:289:17: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[invalid-argument-type] examples/references/classification/imagenet/main.py:348:17: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[invalid-argument-type] examples/references/classification/imagenet/main.py:348:17: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[invalid-argument-type] examples/references/segmentation/pascal_voc2012/main.py:326:17: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[invalid-argument-type] examples/references/segmentation/pascal_voc2012/main.py:326:17: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[invalid-argument-type] examples/references/segmentation/pascal_voc2012/main.py:385:17: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[invalid-argument-type] examples/references/segmentation/pascal_voc2012/main.py:385:17: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `Unknown | int | float | str`
+ error[no-matching-overload] tests/ignite/conftest.py:326:37: No overload of function `__new__` matches arguments
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:1429:17: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | Path | None`
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:1429:17: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | Path | None`
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:1812:25: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | Path | None`
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:1812:25: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | Path | None`
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:1814:25: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | Path | None`
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:1814:25: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | Path | None`
+ error[invalid-argument-type] tests/ignite/metrics/test_precision_recall_curve.py:177:32: Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Tuple`
- Found 2229 diagnostics
+ Found 2245 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ error[invalid-argument-type] dummyserver/socketserver.py:167:25: Argument to bound method `load_cert_chain` is incorrect: Expected `str | bytes | PathLike[str]`, found `Unknown | None`
- warning[possibly-unbound-attribute] dummyserver/testcase.py:323:17: Attribute `sendall` on type `socket | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-attribute] dummyserver/testcase.py:323:17: Attribute `sendall` on type `socket | Any | None` is possibly unbound
+ error[invalid-argument-type] src/urllib3/contrib/emscripten/fetch.py:71:11: Argument to function `files` is incorrect: Expected `str | ModuleType`, found `str | None`
- warning[possibly-unbound-attribute] test/test_http2_connection.py:133:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:133:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:150:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:150:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:173:9: Attribute `assert_has_calls` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:173:9: Attribute `assert_has_calls` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:205:17: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:205:17: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:229:13: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:229:13: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:259:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:259:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:292:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:292:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:325:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:325:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[possibly-unbound-attribute] test/test_http2_connection.py:347:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_http2_connection.py:347:9: Attribute `assert_called_with` on type `(bound method socket.sendall(data: Buffer, flags: int = ellipsis, /) -> None) | Any | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- warning[call-possibly-unbound-method] test/with_dummyserver/test_socketlevel.py:2130:54: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ error[call-non-callable] test/with_dummyserver/test_socketlevel.py:2130:54: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> object, (key: slice[Any, Any, Any], /) -> tuple[object, ...]]) | (bound method Mapping[str, object].__getitem__(key: str, /) -> object)` is not callable on object of type `tuple[object, ...] | Mapping[str, object] | None`
- Found 449 diagnostics
+ Found 451 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ error[invalid-argument-type] cki_lib/inttests/rabbitmq.py:119:21: Argument to bound method `put` is incorrect: Expected `bool | str | None`, found `Path`
- error[invalid-argument-type] cki_lib/inttests/remote_responses.py:248:48: Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class '_MockRequestHandler'>`
+ error[invalid-argument-type] cki_lib/inttests/remote_responses.py:248:48: Argument to bound method `__init__` is incorrect: Expected `(Any, Any, Unknown, /) -> BaseRequestHandler`, found `<class '_MockRequestHandler'>`
+ warning[possibly-unbound-attribute] tests/test_session.py:127:31: Attribute `__name__` on type `((Response, /) -> Any) | Any` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_session.py:132:31: Attribute `__name__` on type `((Response, /) -> Any) | Any` is possibly unbound
- Found 169 diagnostics
+ Found 172 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ error[invalid-argument-type] src/bandersnatch/tests/test_main.py:69:29: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
+ error[invalid-argument-type] src/bandersnatch/tests/test_main.py:69:29: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
+ error[invalid-argument-type] src/bandersnatch/tests/test_main.py:138:25: Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
+ error[invalid-argument-type] src/bandersnatch/tests/test_main.py:138:25: Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 124 diagnostics
+ Found 128 diagnostics

stone (https://github.com/dropbox/stone)
+ error[invalid-argument-type] stone/frontend/ir_generator.py:902:55: Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `TagRef | Unknown`
- Found 179 diagnostics
+ Found 180 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ error[no-matching-overload] tornado/simple_httpclient.py:172:17: No overload of function `min` matches arguments
- warning[possibly-unbound-attribute] tornado/test/options_test.py:79:21: Attribute `getvalue` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[possibly-unbound-attribute] tornado/test/options_test.py:79:21: Attribute `getvalue` on type `TextIO | Any` is possibly unbound
- warning[unused-ignore-comment] tornado/web.py:1938:45: Unused blanket `type: ignore` directive

dragonchain (https://github.com/dragonchain/dragonchain)
+ error[invalid-argument-type] dragonchain/lib/crypto.py:476:72: Argument to function `b64decode` is incorrect: Expected `str | Buffer`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/lib/crypto.py:478:84: Argument to function `b64decode` is incorrect: Expected `str | Buffer`, found `Unknown | None`
+ error[invalid-argument-type] dragonchain/lib/crypto.py:500:33: Argument to function `b64decode` is incorrect: Expected `str | Buffer`, found `Unknown | None`
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:102:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].set(name: @Todo(Support for `typing.TypeAlias`), value: @Todo(Support for `typing.TypeAlias`), ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = Literal[False], xx: bool = Literal[False], keepttl: bool = Literal[False], get: bool = Literal[False], exat: @Todo(Support for `typing.TypeAlias`) | None = None, pxat: @Todo(Support for `typing.TypeAlias`) | None = None) -> bool | None)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:106:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].set(name: @Todo(Support for `typing.TypeAlias`), value: @Todo(Support for `typing.TypeAlias`), ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = Literal[False], xx: bool = Literal[False], keepttl: bool = Literal[False], get: bool = Literal[False], exat: @Todo(Support for `typing.TypeAlias`) | None = None, pxat: @Todo(Support for `typing.TypeAlias`) | None = None) -> bool | None)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:102:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = Literal[False], xx: bool = Literal[False], keepttl: bool = Literal[False], get: bool = Literal[False], exat: Any | None = None, pxat: Any | None = None) -> bool | None)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:106:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].set(name: str | bytes, value: bytes | int | float | str, ex: None | int | float | timedelta = None, px: None | int | float | timedelta = None, nx: bool = Literal[False], xx: bool = Literal[False], keepttl: bool = Literal[False], get: bool = Literal[False], exat: Any | None = None, pxat: Any | None = None) -> bool | None)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:110:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].get(name: @Todo(Support for `typing.TypeAlias`)) -> Unknown | None)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:110:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].get(name: str | bytes) -> Unknown | None)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:114:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].delete(*names: @Todo(Support for `typing.TypeAlias`)) -> int)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:114:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].delete(*names: str | bytes) -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:122:9: Attribute `assert_called_once` on type `Unknown | (bound method Redis[Unknown].flushall(asynchronous: bool = Literal[False], **kwargs: @Todo(Support for `typing.TypeAlias`)) -> bool)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:122:9: Attribute `assert_called_once` on type `Unknown | (bound method Redis[Unknown].flushall(asynchronous: bool = Literal[False], **kwargs: Any) -> bool)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:126:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].hdel(name: @Todo(Support for `typing.TypeAlias`), *keys: @Todo(Support for `typing.TypeAlias`)) -> int)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:126:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].hdel(name: str | bytes, *keys: str | bytes) -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:130:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].lpush(name: @Todo(Support for `typing.TypeAlias`), *values: @Todo(Support for `typing.TypeAlias`)) -> int)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:130:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].lpush(name: bytes | int | float | str, *values: bytes | int | float | str) -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:134:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].rpush(name: @Todo(Support for `typing.TypeAlias`), *values: @Todo(Support for `typing.TypeAlias`)) -> int)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:134:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].rpush(name: bytes | int | float | str, *values: bytes | int | float | str) -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:138:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].delete(*names: @Todo(Support for `typing.TypeAlias`)) -> int)` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:138:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].delete(*names: str | bytes) -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:142:9: Attribute `assert_called_once_with` on type `Unknown | (Overload[(name: @Todo(Support for `typing.TypeAlias`), key: @Todo(Support for `typing.TypeAlias`), value: @Todo(Support for `typing.TypeAlias`), mapping: Mapping[@Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.TypeAlias`)] | None = None, items: @Todo(Support for `typing.TypeAlias`) | None = None) -> int, (name: @Todo(Support for `typing.TypeAlias`), key: None, value: None, mapping: Mapping[@Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.TypeAlias`)], items: @Todo(Support for `typing.TypeAlias`) | None = None) -> int, (name: @Todo(Support for `typing.TypeAlias`), *, mapping: Mapping[@Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.TypeAlias`)], items: @Todo(Support for `typing.TypeAlias`) | None = None) -> int])` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:146:9: Attribute `assert_called_once_with` on type `Unknown | (Overload[(keys: @Todo(Support for `typing.TypeAlias`) | Iterable[@Todo(Support for `typing.TypeAlias`)], timeout: Literal[0] | None = Literal[0]) -> tuple[Unknown, Unknown], (keys: @Todo(Support for `typing.TypeAlias`) | Iterable[@Todo(Support for `typing.TypeAlias`)], timeout: int | float) -> tuple[Unknown, Unknown] | None])` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:142:9: Attribute `assert_called_once_with` on type `Unknown | (Overload[(name: str | bytes, key: str | bytes, value: bytes | int | float | str, mapping: Mapping[str | bytes, bytes | int | float | str] | None = None, items: Any | None = None) -> int, (name: str | bytes, key: None, value: None, mapping: Mapping[str | bytes, bytes | int | float | str], items: Any | None = None) -> int, (name: str | bytes, *, mapping: Mapping[str | bytes, bytes | int | float | str], items: Any | None = None) -> int])` is possibly unbound
+ warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:146:9: Attribute `assert_called_once_with` on type `Unknown | (Overload[(keys: Iterable[bytes | int | float | str] | int | float, timeout: Literal[0] | None = Literal[0]) -> tuple[Unknown, Unknown], (keys: Iterable[bytes | int | float | str] | int | float, timeout: int | float) -> tuple[Unknown, Unknown] | None])` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/lib/database/redis_utest.py:154:9: Attribute `assert_called_once_with` on type `Unknown | (bound method Redis[Unknown].get(name: @Todo(Support for `typing.TypeAlias`)) -> Unknown | None)` is possibly unbound
+ warning[possibly-unbound-at...*[Comment body truncated]*

---

_Closed by @sharkdp on 2025-06-06 12:14_

---
