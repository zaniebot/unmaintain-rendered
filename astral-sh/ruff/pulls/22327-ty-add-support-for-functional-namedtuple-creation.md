```yaml
number: 22327
title: "[ty] Add support for functional `namedtuple` creation"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: charlie/dyn-expression
head: charlie/functional-namedtuple
created_at: 2026-01-01T13:23:44Z
updated_at: 2026-01-13T23:34:22Z
url: https://github.com/astral-sh/ruff/pull/22327
synced_at: 2026-01-14T00:34:17Z
```

# [ty] Add support for functional `namedtuple` creation

---

_@charliermarsh_

## Summary

This PR is intended to demonstrate how the pattern established in https://github.com/astral-sh/ruff/pull/22291 generalizes to other class "kinds".

Closes https://github.com/astral-sh/ty/issues/1049.


---

_Label `ty` added by @charliermarsh on 2026-01-01 13:23_

---

_Comment by @astral-sh-bot[bot] on 2026-01-01 13:25_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-13 23:30:20.789529880 +0000
+++ new-output.txt	2026-01-13 23:30:21.116530273 +0000
@@ -753,6 +753,16 @@
 namedtuples_define_class.py:86:5: error[invalid-named-tuple] NamedTuple field without default value cannot follow field(s) with default value(s): Field `latitude` defined here without a default value
 namedtuples_define_class.py:125:19: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `float`
 namedtuples_define_class.py:132:24: error[invalid-named-tuple] NamedTuple class `Unit` cannot use multiple inheritance except with `Generic[]`
+namedtuples_define_functional.py:16:8: error[missing-argument] No argument provided for required parameter `y`
+namedtuples_define_functional.py:21:8: error[missing-argument] No arguments provided for required parameters `x`, `y`
+namedtuples_define_functional.py:26:21: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
+namedtuples_define_functional.py:31:8: error[missing-argument] No argument provided for required parameter `y`
+namedtuples_define_functional.py:31:18: error[unknown-argument] Argument `z` does not match any known parameter
+namedtuples_define_functional.py:36:18: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["1"]`
+namedtuples_define_functional.py:37:21: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
+namedtuples_define_functional.py:42:18: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["1"]`
+namedtuples_define_functional.py:43:15: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
+namedtuples_define_functional.py:69:1: error[missing-argument] No argument provided for required parameter `a`
 namedtuples_type_compat.py:22:23: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, int]`
 namedtuples_type_compat.py:23:28: error[invalid-assignment] Object of type `Point` is not assignable to `tuple[int, str, str]`
 namedtuples_usage.py:34:7: error[index-out-of-bounds] Index 3 is out of bounds for tuple `Point` with length 3
@@ -879,6 +889,10 @@
 qualifiers_final_annotation.py:81:1: error[invalid-assignment] Cannot assign to final attribute `DEFAULT_ID` on type `<class 'ClassB'>`
 qualifiers_final_annotation.py:118:9: error[invalid-type-form] Type qualifier `typing.Final` is not allowed in type expressions (only in annotation expressions)
 qualifiers_final_annotation.py:121:11: error[invalid-type-form] `Final` is not allowed in function parameter annotations
+qualifiers_final_annotation.py:134:1: error[missing-argument] No arguments provided for required parameters `x`, `y`
+qualifiers_final_annotation.py:134:3: error[unknown-argument] Argument `a` does not match any known parameter
+qualifiers_final_annotation.py:135:3: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal[""]`
+qualifiers_final_annotation.py:135:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal[""]`
 qualifiers_final_annotation.py:141:5: error[invalid-assignment] Reassignment of `Final` symbol `ID1` is not allowed: Reassignment of `Final` symbol
 qualifiers_final_annotation.py:145:5: error[invalid-assignment] Reassignment of `Final` symbol `x` is not allowed: Symbol later reassigned here
 qualifiers_final_annotation.py:147:10: error[invalid-assignment] Reassignment of `Final` symbol `x` is not allowed: Symbol later reassigned here
@@ -1029,4 +1043,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1031 diagnostics
+Found 1045 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-01 13:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
+ lib/spack/spack/test/directives.py:118:5: error[invalid-assignment] Object of type `dict[Unknown | Spec, Unknown | str]` is not assignable to attribute `licenses` of type `property`
+ lib/spack/spack/test/directives.py:119:5: error[invalid-assignment] Object of type `Literal["test_package"]` is not assignable to attribute `name` of type `property`
+ lib/spack/spack/test/directives.py:127:43: error[invalid-argument-type] Argument to function `_execute_license` is incorrect: Expected `type[PackageBase]`, found `<class 'package'>`
+ lib/spack/spack/test/directives.py:132:5: error[invalid-assignment] Object of type `dict[Unknown | Spec, Unknown | str]` is not assignable to attribute `licenses` of type `property`
+ lib/spack/spack/test/directives.py:133:5: error[invalid-assignment] Object of type `Literal["test_package"]` is not assignable to attribute `name` of type `property`
+ lib/spack/spack/test/directives.py:141:43: error[invalid-argument-type] Argument to function `_execute_license` is incorrect: Expected `type[PackageBase]`, found `<class 'package'>`
+ lib/spack/spack/test/directives.py:154:43: error[invalid-argument-type] Argument to function `_execute_version` is incorrect: Expected `type[PackageBase]`, found `package`
+ lib/spack/spack/test/directives.py:158:43: error[invalid-argument-type] Argument to function `_execute_version` is incorrect: Expected `type[PackageBase]`, found `package`
+ lib/spack/spack/test/test_suite.py:264:44: error[invalid-argument-type] Argument to function `copy_test_files` is incorrect: Expected `PackageBase`, found `MyPackage`
+ lib/spack/spack/test/test_suite.py:275:40: error[invalid-argument-type] Argument to function `copy_test_files` is incorrect: Expected `PackageBase`, found `MyPackage`
+ lib/spack/spack/test/test_suite.py:293:47: error[invalid-argument-type] Argument to function `process_test_parts` is incorrect: Expected `PackageBase`, found `MyPackage`
+ lib/spack/spack/test/test_suite.py:532:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `PackageBase`, found `MyPackage`
- Found 4319 diagnostics
+ Found 4331 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/iptables.py:38:9: error[unresolved-attribute] Object of type `_RuleBase` has no attribute `validate`
- Found 1101 diagnostics
+ Found 1102 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/database/backends/mongodb/base.py:455:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:461:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:467:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:481:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:507:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:604:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:664:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:668:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:684:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:688:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:692:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:727:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:764:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:801:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:838:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:882:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:905:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:967:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:971:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1103:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1108:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1136:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1172:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1179:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1233:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1237:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1320:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1324:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1393:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1397:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1444:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1448:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/mongodb/base.py:1503:26: error[missing-argument] No arguments provided for required parameters `where`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:360:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:370:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:389:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:409:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:476:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:518:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:526:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:537:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:546:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:555:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:581:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:607:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:636:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:669:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:703:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:719:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:751:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:761:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:919:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:932:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:966:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1000:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1016:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1079:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1088:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1189:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1198:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1280:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1289:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1354:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1363:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
+ alerta/database/backends/postgres/base.py:1433:26: error[missing-argument] No arguments provided for required parameters `where`, `vars`, `sort`, `group`
- Found 555 diagnostics
+ Found 620 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/connectionpool.py:1132:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/urllib3/connectionpool.py:1134:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 305 diagnostics
+ Found 303 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/test_messagequeue.py:792:32: error[invalid-argument-type] Argument to bound method `_consume_one` is incorrect: Expected `Item`, found `tuple[Literal[""], None, Literal[""], Literal["{}"], Literal[""], Literal[""]]`
+ tests/test_messagequeue.py:811:32: error[invalid-argument-type] Argument to bound method `_consume_one` is incorrect: Expected `Item`, found `tuple[Literal[""], None, Literal[""], Literal["{}"], Literal[""], Literal[""]]`
- Found 240 diagnostics
+ Found 242 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
+ pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Any | None | bytes`
- pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[str, ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[CoroutineType[Any, Any, None]]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
+ pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[str, ((credentials: MongoCredential, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: MongoCredential, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[CoroutineType[Any, Any, None]]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
- pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
+ pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Any | None | bytes`
- pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[str, ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[None]]` is not assignable to `Mapping[str, (...) -> None]`
+ pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[str, ((credentials: MongoCredential, conn: Connection) -> None) | ((credentials: MongoCredential, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[None]]` is not assignable to `Mapping[str, (...) -> None]`

discord.py (https://github.com/Rapptz/discord.py)
+ discord/enums.py:97:5: error[invalid-assignment] Object of type `(self) -> Unknown` is not assignable to attribute `__repr__` of type `def __repr__(self) -> str`
+ discord/enums.py:98:5: error[invalid-assignment] Object of type `(self) -> Unknown` is not assignable to attribute `__str__` of type `def __str__(self) -> str`
+ discord/enums.py:100:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__le__` of type `def __le__(self, value: tuple[Unknown, ...], /) -> bool`
+ discord/enums.py:101:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__ge__` of type `def __ge__(self, value: tuple[Unknown, ...], /) -> bool`
+ discord/enums.py:102:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__lt__` of type `def __lt__(self, value: tuple[Unknown, ...], /) -> bool`
+ discord/enums.py:103:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__gt__` of type `def __gt__(self, value: tuple[Unknown, ...], /) -> bool`
- Found 548 diagnostics
+ Found 554 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ tests/unittests/sources/test_smartos.py:806:5: error[invalid-assignment] Object of type `Literal[2882400018]` is not assignable to attribute `request_id` of type `property`
+ tests/unittests/sources/test_smartos.py:807:5: error[invalid-assignment] Object of type `Literal["value"]` is not assignable to attribute `metadata_value` of type `property`
+ tests/unittests/sources/test_smartos.py:808:5: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | int]` is not assignable to attribute `response_parts` of type `property`
+ tests/unittests/sources/test_smartos.py:818:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["payload"]` and `property`
+ tests/unittests/sources/test_smartos.py:820:17: error[not-subscriptable] Cannot subscript object of type `property` with no `__getitem__` method
+ tests/unittests/sources/test_smartos.py:823:13: error[no-matching-overload] No overload of bound method `format` matches arguments
+ tests/unittests/sources/test_smartos.py:830:5: error[invalid-assignment] Object of type `None` is not assignable to attribute `metasource_data` of type `property`
+ tests/unittests/sources/test_smartos.py:836:16: error[not-subscriptable] Cannot subscript object of type `property` with no `__getitem__` method
+ tests/unittests/sources/test_smartos.py:837:31: error[not-subscriptable] Cannot subscript object of type `property` with no `__getitem__` method
- Found 1166 diagnostics
+ Found 1175 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/platforms/evm.py:3016:16: warning[possibly-missing-attribute] Attribute `coinbase` may be missing on object of type `Unknown | None`
+ manticore/platforms/evm.py:3016:16: warning[possibly-missing-attribute] Attribute `coinbase` may be missing on object of type `Unknown | None | BlockHeader`
- manticore/platforms/evm.py:3019:16: warning[possibly-missing-attribute] Attribute `timestamp` may be missing on object of type `Unknown | None`
+ manticore/platforms/evm.py:3019:16: warning[possibly-missing-attribute] Attribute `timestamp` may be missing on object of type `Unknown | None | BlockHeader`
- manticore/platforms/evm.py:3022:16: warning[possibly-missing-attribute] Attribute `blocknumber` may be missing on object of type `Unknown | None`
+ manticore/platforms/evm.py:3022:16: warning[possibly-missing-attribute] Attribute `blocknumber` may be missing on object of type `Unknown | None | BlockHeader`
- manticore/platforms/evm.py:3025:16: warning[possibly-missing-attribute] Attribute `difficulty` may be missing on object of type `Unknown | None`
+ manticore/platforms/evm.py:3025:16: warning[possibly-missing-attribute] Attribute `difficulty` may be missing on object of type `Unknown | None | BlockHeader`
- manticore/platforms/evm.py:3028:16: warning[possibly-missing-attribute] Attribute `gaslimit` may be missing on object of type `Unknown | None`
+ manticore/platforms/evm.py:3028:16: warning[possibly-missing-attribute] Attribute `gaslimit` may be missing on object of type `Unknown | None | BlockHeader`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5364 diagnostics
+ Found 5369 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:917:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:984:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1053:69: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1124:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1195:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1243:44: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1244:29: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1282:44: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1283:29: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1320:44: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1321:29: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1358:44: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1358:62: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1395:44: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
+ lib/Crypto/SelfTest/PublicKey/test_ECC_NIST.py:1395:62: error[invalid-argument-type] Argument to function `construct` is incorrect: Expected `str | int`, found `IntegerBase`
- Found 1321 diagnostics
+ Found 1336 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/vendor/psutil/__init__.py:1769:12: error[call-non-callable] Object of type `scputimes` is not callable
+ ddtrace/vendor/psutil/__init__.py:1888:16: error[call-non-callable] Object of type `scputimes` is not callable
+ ddtrace/vendor/psutil/_pslinux.py:547:12: error[call-non-callable] Object of type `scputimes` is not callable
+ ddtrace/vendor/psutil/_pslinux.py:565:25: error[call-non-callable] Object of type `scputimes` is not callable
- Found 8393 diagnostics
+ Found 8397 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/interpreters/ad.py:430:7: error[call-non-callable] Object of type `aval_method` is not callable
- jax/_src/pallas/mosaic/sc_primitives.py:108:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:173:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:198:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:215:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ jax/_src/pallas/mosaic_gpu/pipeline.py:123:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> Unknown, (index: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` cannot be called with key of type `tuple[Slice | Array, ...]` on object of type `aval_property`
+ jax/_src/pallas/mosaic_gpu/pipeline.py:136:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> Unknown, (index: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` cannot be called with key of type `tuple[Slice | Array, ...]` on object of type `aval_property`
- Found 2853 diagnostics
+ Found 2852 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/profile/__main__.py:2135:9: error[invalid-assignment] Object of type `dict[str, FunctionMetaData | None]` is not assignable to attribute `meta` of type `dict[str, FunctionMetaData] | None`

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14503 diagnostics
+ Found 14502 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/optimize/_differentiable_functions.py:382:26: error[call-non-callable] Object of type `_FakeCounter` is not callable
+ scipy/optimize/_differentiable_functions.py:387:26: error[call-non-callable] Object of type `_FakeCounter` is not callable
+ scipy/optimize/_linprog_util.py:643:17: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:659:21: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:676:21: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:700:21: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:723:25: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:759:25: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:779:21: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:869:17: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_linprog_util.py:915:13: error[missing-argument] No argument provided for required parameter `integrality`
+ scipy/optimize/_shgo.py:1521:13: warning[possibly-missing-attribute] Attribute `add_points` may be missing on object of type `Unknown | Tri | Delaunay`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:23:10: error[missing-argument] No arguments provided for required parameters `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:47:10: error[missing-argument] No arguments provided for required parameters `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:75:45: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:76:46: error[missing-argument] No arguments provided for required parameters `b_ub`, `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:77:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:78:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:79:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:80:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:81:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:82:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:83:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:92:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:93:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:94:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:95:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:96:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:103:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:104:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:118:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:119:46: error[missing-argument] No arguments provided for required parameters `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:120:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:121:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:122:46: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `b_eq`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:127:23: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `b_eq`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:131:10: error[missing-argument] No arguments provided for required parameters `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:159:10: error[missing-argument] No arguments provided for required parameters `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:179:10: error[missing-argument] No arguments provided for required parameters `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:205:10: error[missing-argument] No arguments provided for required parameters `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:232:10: error[missing-argument] No arguments provided for required parameters `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:254:10: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:266:10: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:273:10: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
+ scipy/optimize/tests/test__linprog_clean_inputs.py:299:10: error[missing-argument] No arguments provided for required parameters `A_ub`, `b_ub`, `A_eq`, `b_eq`, `bounds`, `x0`, `integrality`
- scipy/stats/_hypotests.py:1859:26: warning[possibly-missing-attribute] Attribute `low` may be missing on object of type `Unknown | None`
+ scipy/stats/_hypotests.py:1859:26: warning[possibly-missing-attribute] Attribute `low` may be missing on object of type `Unknown | None | ConfidenceInterval`
- scipy/stats/_hypotests.py:1860:26: warning[possibly-missing-attribute] Attribute `high` may be missing on object of type `Unknown | None`
+ scipy/stats/_hypotests.py:1860:26: warning[possibly-missing-attribute] Attribute `high` may be missing on object of type `Unknown | None | ConfidenceInterval`
- scipy/stats/_multicomp.py:69:22: warning[possibly-missing-attribute] Attribute `low` may be missing on object of type `@Todo | None`
+ scipy/stats/_multicomp.py:69:22: warning[possibly-missing-attribute] Attribute `low` may be missing on object of type `ConfidenceInterval | None`
- scipy/stats/_multicomp.py:70:22: warning[possibly-missing-attribute] Attribute `high` may be missing on object of type `@Todo | None`
+ scipy/stats/_multicomp.py:70:22: warning[possibly-missing-attribute] Attribute `high` may be missing on object of type `ConfidenceInterval | None`
+ scipy/stats/tests/test_generation/studentized_range_mpmath_ref.py:67:13: error[too-many-positional-arguments] Too many positional arguments: expected 6, got 8
- Found 8082 diagnostics
+ Found 8128 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-02 16:46_

(The conformance changes are good, but I'm still working through the ecosystem changes.)

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-02 16:49_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 16:55_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `missing-argument` | 107 | 0 | 0 |
| `invalid-argument-type` | 27 | 0 | 2 |
| `invalid-assignment` | 14 | 1 | 2 |
| `possibly-missing-attribute` | 1 | 0 | 9 |
| `call-non-callable` | 7 | 0 | 0 |
| `invalid-await` | 0 | 0 | 6 |
| `unused-ignore-comment` | 0 | 6 | 0 |
| `invalid-return-type` | 1 | 1 | 1 |
| `not-subscriptable` | 3 | 0 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| `unresolved-attribute` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **164** | **8** | **20** |


**[Full report with detailed diff](https://5a63e675.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://5a63e675.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @charliermarsh on 2026-01-02 18:05_

I believe the `ddtrace` issues are real. They have a fallback assignment:

https://github.com/DataDog/dd-trace-py/blob/6602420478b59b2d3df24ed27218ce12365b0cb7/ddtrace/vendor/psutil/_pslinux.py#L269-L274

So they create `scputimes = namedtuple('scputimes', 'user system idle')(0.0, 0.0, 0.0)`, then later treat that as if it's a class rather than instance:

https://github.com/DataDog/dd-trace-py/blob/6602420478b59b2d3df24ed27218ce12365b0cb7/ddtrace/vendor/psutil/_pslinux.py#L547

---

_Comment by @charliermarsh on 2026-01-02 18:08_

The `alerta` issues are false positives, but I believe mypy at least also suffers from them.

Specifically, they set deferred defaults [here](https://github.com/alerta/alerta/blob/b7111d23ae9aefb5064ef107976f62e3836217b6/alerta/database/backends/mongodb/utils.py#L18-L19):

```python
Query = namedtuple('Query', ['where', 'sort', 'group'])
Query.__new__.__defaults__ = ({}, [('_id', 1)], 'status')  # type: ignore
```


---

_Comment by @charliermarsh on 2026-01-02 18:19_

SciPy is similar -- deferred `__defaults__`: https://github.com/scipy/scipy/blob/b9ae1430d33a36c2e95b51932963278c255c100d/scipy/optimize/_linprog_util.py#L15-L17.

---

_Comment by @charliermarsh on 2026-01-02 18:20_

I think the Spack errors are true positives though? They're essentially creating a mock: https://github.com/spack/spack/blob/59b4e440391d5f58a665041ca365d9feb5caf472/lib/spack/spack/test/directives.py#L117-L119

---

_Marked ready for review by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @carljm by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-08 00:49_

---

_Converted to draft by @charliermarsh on 2026-01-10 18:53_

---

_Comment by @charliermarsh on 2026-01-12 02:22_

(I think this change got ruined in the rebase -- we lost most of the import pieces. I'm reviving from old commits.)

---

_Comment by @charliermarsh on 2026-01-12 04:32_

I _think_ the `pycryptodome` errors are correct: we now understand that the arguments need to be `Int`, but it's passing `IntegerBase`.

---

_Marked ready for review by @charliermarsh on 2026-01-13 02:13_

---

_Converted to draft by @charliermarsh on 2026-01-13 02:21_

---

_Marked ready for review by @charliermarsh on 2026-01-13 02:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:236 on 2026-01-13 12:47_

Calling `reveal_mro` on this class indicates that it's inferred from inheriting from the wrong kind of tuple on your branch currently:

```py
# Revealed MRO: (<class 'Url'>, <class 'Url'>, <class 'tuple[Unknown, ...]'>, <class 'object'>)
reveal_mro(Url)
```

It should be inferred as having `tuple[str, str]` in its MRO, not `tuple[Unknown, ...]`. This means that unpacking `Url` instances doesn't work correctly:

```py
a, b = Url("foo", "bar")
reveal_type(a)  # Unknown, should be str
reveal_type(b)  # Unknown, should be str
```

neither does indexing:

```py
reveal_type(Url("a", "b")[0])  # revealed: Unknown, should be str
```

and we have false negatives on these two lines, both of which should cause us to emit diagnostics:

```py
a, b, c = Url("a", "b")
Url("a", "b")[52]
```

all of this would be fixed if we had `tuple[str, str]` in the MRO instead of `tuple[Unknown, ...]`.

---

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:220 on 2026-01-13 12:50_

interesting, did this come up in the ecosystem?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:211 on 2026-01-13 13:02_

And this is because for `class Url(namedtuple("Url", "host port")): ...`, the inner "inline" `Url` class is a namedtuple class but the outer one is not. So it's really equivalent to doing

```py
class Url(NamedTuple):
    host: str
    port: int

class Url(Url): ...
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:379 on 2026-01-13 13:02_

And again, this is because they aren't actually namedtuple classes, they just inherit from namedtuple classes

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:230 on 2026-01-13 13:04_

I'm confused, I don't see a `__getattr__` method here?

https://github.com/astral-sh/ruff/blob/990d0a899972a59cccdf69d57c56358773d2da5c/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/_type_checker_internals.pyi#L54-L75

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:602 on 2026-01-13 13:04_

same comment as above

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4033 on 2026-01-13 13:15_

`.to_class_literal(db)` returns a type representing the class object `NamedTupleFallback` itself. But that's not what calling `namedtuple()` returns. It returns a subclass of `NamedTupleFallback`.

Also, why are some of the annotated types so imprecise here? 😄 Typeshed gives much more precise types for most of these

```suggestion
                // collections.namedtuple(typename, field_names, ...)
                Some(KnownFunction::NamedTuple) => {
                    let str_type = KnownClass::Str.to_instance(db);
                    let none_type = Type::none(db);

                    let parameters = [
                        Parameter::positional_or_keyword(Name::new_static("typename"))
                            .with_annotated_type(str_type),
                        Parameter::positional_or_keyword(Name::new_static("field_names"))
                            .with_annotated_type(
                                KnownClass::Iterable.to_specialized_instance(db, &[str_type]),
                            ),
                        // Additional optional parameters have defaults.
                        Parameter::keyword_only(Name::new_static("rename"))
                            .with_annotated_type(KnownClass::Bool.to_instance(db))
                            .with_default_type(Type::BooleanLiteral(false)),
                        Parameter::keyword_only(Name::new_static("defaults"))
                            .with_annotated_type(UnionType::from_elements(
                                db,
                                [
                                    KnownClass::Iterable
                                        .to_specialized_instance(db, &[Type::any()]),
                                    none_type,
                                ],
                            ))
                            .with_default_type(none_type),
                        Parameter::keyword_only(Name::new_static("module"))
                            .with_annotated_type(UnionType::from_elements(
                                db,
                                [str_type, none_type],
                            ))
                            .with_default_type(none_type),
                    ];

                    let signature = Signature::new(
                        Parameters::new(db, parameters),
                        KnownClass::NamedTupleFallback.to_class_literal(db),
                    );

                    Binding::single(self, signature).into()
                }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4475 on 2026-01-13 13:17_

should have an annotated type of `Iterable[tuple[str, Any]]`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4478 on 2026-01-13 13:17_

```suggestion
                        KnownClass::NamedTupleFallback.to_subclass_of(db),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:241 on 2026-01-13 13:39_

I think you've got a bit confused here, and it's caused a lot of complexity for you elsewhere.

For `class Url(namedtuple("Url", "host port")): ...`, the inner "inline" class is a namedtuple but the outer one is not -- the outer class is just a class that _inherits_ from a namedtuple class. It's equivalent to doing this:

```py
class Url(NamedTuple):
    host: str
    port: int

class Url(Url): ...
```

`CodeGeneratorKind::NamedTuple.matches(db, cls, None)` should return `true` for the inner class and `false` for the outer class.

You should get rid of this; doing so also allows you to get rid of the `directly_inherits_from_named_tuple_special_form` method you added elsewhere, too, because it's no longer necessary to distinguish between "different kinds of namedtuple classes". This diff causes some tests to fail regarding `__init__`, but I don't think they'll be too hard for you to fix:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index e547b0220b..e286141a76 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -231,14 +231,6 @@ impl<'db> CodeGeneratorKind<'db> {
                 .contains(&Type::SpecialForm(SpecialFormType::NamedTuple))
             {
                 Some(CodeGeneratorKind::NamedTuple)
-            } else if class
-                .explicit_bases(db)
-                .iter()
-                .any(|base| matches!(base, Type::ClassLiteral(ClassLiteral::DynamicNamedTuple(_))))
-            {
-                // Class inherits from a functional namedtuple like:
-                // class Url(NamedTuple("Url", [("host", str)])): ...
-                Some(CodeGeneratorKind::NamedTuple)
             } else if class.is_typed_dict(db) {
                 Some(CodeGeneratorKind::TypedDict)
             } else {
@@ -2109,20 +2101,6 @@ impl<'db> StaticClassLiteral<'db> {
         self.is_known(db, KnownClass::Tuple)
     }
 
-    /// Returns `true` if this class directly inherits from the `NamedTuple` special form
-    /// using class syntax (e.g., `class Foo(NamedTuple): ...`).
-    ///
-    /// This is distinct from inheriting from a functional namedtuple like
-    /// `class Foo(namedtuple("Foo", ...)): ...`, which creates a regular class.
-    ///
-    /// The distinction matters because:
-    /// - Classes using class syntax cannot use `super()` or override `__new__`
-    /// - Classes inheriting from functional namedtuples can do both
-    pub(crate) fn directly_inherits_from_named_tuple_special_form(self, db: &'db dyn Db) -> bool {
-        self.explicit_bases(db)
-            .contains(&Type::SpecialForm(SpecialFormType::NamedTuple))
-    }
-
     /// Returns `true` if this class inherits from a functional namedtuple
     /// (`DynamicNamedTupleLiteral`) that has unknown fields.
     ///
@@ -7386,7 +7364,7 @@ impl KnownClass {
                         // Check if the enclosing class directly inherits from NamedTuple special form,
                         // which forbids the use of `super()`. Classes inheriting from functional
                         // namedtuples (e.g., `class Foo(namedtuple(...)):`) can use `super()` normally.
-                        if enclosing_class.directly_inherits_from_named_tuple_special_form(db) {
+                        if CodeGeneratorKind::NamedTuple.matches(db, enclosing_class.into(), None) {
                             if let Some(builder) = context
                                 .report_lint(&SUPER_CALL_IN_NAMED_TUPLE_METHOD, call_expression)
                             {
@@ -7442,7 +7420,11 @@ impl KnownClass {
                         // which forbids the use of `super()`. Classes inheriting from functional
                         // namedtuples (e.g., `class Foo(namedtuple(...)):`) can use `super()` normally.
                         if let Some(enclosing_class) = nearest_enclosing_class(db, index, scope) {
-                            if enclosing_class.directly_inherits_from_named_tuple_special_form(db) {
+                            if CodeGeneratorKind::NamedTuple.matches(
+                                db,
+                                enclosing_class.into(),
+                                None,
+                            ) {
                                 if let Some(builder) = context
                                     .report_lint(&SUPER_CALL_IN_NAMED_TUPLE_METHOD, call_expression)
                                 {
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index cf1d72aee8..2c5e0c23c0 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -688,7 +688,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             //     - If the class is a protocol class: check for inheritance from a non-protocol class
             //     - If the class is a NamedTuple class: check for multiple inheritance that isn't `Generic[]`
             for (i, base_class) in class.explicit_bases(self.db()).iter().enumerate() {
-                if class.directly_inherits_from_named_tuple_special_form(self.db())
+                if is_named_tuple
                     && !matches!(
                         base_class,
                         Type::SpecialForm(SpecialFormType::NamedTuple)
diff --git a/crates/ty_python_semantic/src/types/overrides.rs b/crates/ty_python_semantic/src/types/overrides.rs
index 0e6086b924..15094518e8 100644
--- a/crates/ty_python_semantic/src/types/overrides.rs
+++ b/crates/ty_python_semantic/src/types/overrides.rs
@@ -131,10 +131,7 @@ fn check_class_declaration<'db>(
     // `NamedTuple` classes have certain synthesized attributes (like `_asdict`, `_make`, etc.)
     // that cannot be overwritten. Attempting to assign to these attributes (without type
     // annotations) or define methods with these names will raise an `AttributeError` at runtime.
-    //
-    // This only applies to classes that directly inherit from the `NamedTuple` special form,
-    // not to classes that inherit from functional namedtuples (which create regular classes).
-    if literal.directly_inherits_from_named_tuple_special_form(db)
+    if class_kind == Some(CodeGeneratorKind::NamedTuple)
         && configuration.check_prohibited_named_tuple_attrs()
         && PROHIBITED_NAMEDTUPLE_ATTRS.contains(&member.name.as_str())
         && let Some(symbol_id) = place_table(db, class_scope).symbol_id(&member.name)
~/dev/ruff (charlie/functional-namedtuple)⚡ % gd
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index e547b0220b..e286141a76 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -231,14 +231,6 @@ impl<'db> CodeGeneratorKind<'db> {
                 .contains(&Type::SpecialForm(SpecialFormType::NamedTuple))
             {
                 Some(CodeGeneratorKind::NamedTuple)
-            } else if class
-                .explicit_bases(db)
-                .iter()
-                .any(|base| matches!(base, Type::ClassLiteral(ClassLiteral::DynamicNamedTuple(_))))
-            {
-                // Class inherits from a functional namedtuple like:
-                // class Url(NamedTuple("Url", [("host", str)])): ...
-                Some(CodeGeneratorKind::NamedTuple)
             } else if class.is_typed_dict(db) {
                 Some(CodeGeneratorKind::TypedDict)
             } else {
@@ -2109,20 +2101,6 @@ impl<'db> StaticClassLiteral<'db> {
         self.is_known(db, KnownClass::Tuple)
     }
 
-    /// Returns `true` if this class directly inherits from the `NamedTuple` special form
-    /// using class syntax (e.g., `class Foo(NamedTuple): ...`).
-    ///
-    /// This is distinct from inheriting from a functional namedtuple like
-    /// `class Foo(namedtuple("Foo", ...)): ...`, which creates a regular class.
-    ///
-    /// The distinction matters because:
-    /// - Classes using class syntax cannot use `super()` or override `__new__`
-    /// - Classes inheriting from functional namedtuples can do both
-    pub(crate) fn directly_inherits_from_named_tuple_special_form(self, db: &'db dyn Db) -> bool {
-        self.explicit_bases(db)
-            .contains(&Type::SpecialForm(SpecialFormType::NamedTuple))
-    }
-
     /// Returns `true` if this class inherits from a functional namedtuple
     /// (`DynamicNamedTupleLiteral`) that has unknown fields.
     ///
@@ -7386,7 +7364,7 @@ impl KnownClass {
                         // Check if the enclosing class directly inherits from NamedTuple special form,
                         // which forbids the use of `super()`. Classes inheriting from functional
                         // namedtuples (e.g., `class Foo(namedtuple(...)):`) can use `super()` normally.
-                        if enclosing_class.directly_inherits_from_named_tuple_special_form(db) {
+                        if CodeGeneratorKind::NamedTuple.matches(db, enclosing_class.into(), None) {
                             if let Some(builder) = context
                                 .report_lint(&SUPER_CALL_IN_NAMED_TUPLE_METHOD, call_expression)
                             {
@@ -7442,7 +7420,11 @@ impl KnownClass {
                         // which forbids the use of `super()`. Classes inheriting from functional
                         // namedtuples (e.g., `class Foo(namedtuple(...)):`) can use `super()` normally.
                         if let Some(enclosing_class) = nearest_enclosing_class(db, index, scope) {
-                            if enclosing_class.directly_inherits_from_named_tuple_special_form(db) {
+                            if CodeGeneratorKind::NamedTuple.matches(
+                                db,
+                                enclosing_class.into(),
+                                None,
+                            ) {
                                 if let Some(builder) = context
                                     .report_lint(&SUPER_CALL_IN_NAMED_TUPLE_METHOD, call_expression)
                                 {
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index cf1d72aee8..2c5e0c23c0 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -688,7 +688,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             //     - If the class is a protocol class: check for inheritance from a non-protocol class
             //     - If the class is a NamedTuple class: check for multiple inheritance that isn't `Generic[]`
             for (i, base_class) in class.explicit_bases(self.db()).iter().enumerate() {
-                if class.directly_inherits_from_named_tuple_special_form(self.db())
+                if is_named_tuple
                     && !matches!(
                         base_class,
                         Type::SpecialForm(SpecialFormType::NamedTuple)
diff --git a/crates/ty_python_semantic/src/types/overrides.rs b/crates/ty_python_semantic/src/types/overrides.rs
index 0e6086b924..15094518e8 100644
--- a/crates/ty_python_semantic/src/types/overrides.rs
+++ b/crates/ty_python_semantic/src/types/overrides.rs
@@ -131,10 +131,7 @@ fn check_class_declaration<'db>(
     // `NamedTuple` classes have certain synthesized attributes (like `_asdict`, `_make`, etc.)
     // that cannot be overwritten. Attempting to assign to these attributes (without type
     // annotations) or define methods with these names will raise an `AttributeError` at runtime.
-    //
-    // This only applies to classes that directly inherit from the `NamedTuple` special form,
-    // not to classes that inherit from functional namedtuples (which create regular classes).
-    if literal.directly_inherits_from_named_tuple_special_form(db)
+    if class_kind == Some(CodeGeneratorKind::NamedTuple)
         && configuration.check_prohibited_named_tuple_attrs()
         && PROHIBITED_NAMEDTUPLE_ATTRS.contains(&member.name.as_str())
         && let Some(symbol_id) = place_table(db, class_scope).symbol_id(&member.name)
```

---

_@AlexWaygood requested changes on 2026-01-13 13:40_

Not a full review yet, but this seems like enough comments to get you started ;)

A lot of this looks good, but I think there's a few places where you've introduced unnecessary complexity

---

_Converted to draft by @charliermarsh on 2026-01-13 14:39_

---

_@charliermarsh reviewed on 2026-01-13 17:24_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:241 on 2026-01-13 17:24_

Thank you, this is very helpful.

---

_@charliermarsh reviewed on 2026-01-13 17:40_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:230 on 2026-01-13 17:40_

(Sorry, this is a vestige of a previous solution where I had to change the typeshed stub.)

---

_Marked ready for review by @charliermarsh on 2026-01-13 18:05_

---

_@AlexWaygood reviewed on 2026-01-13 18:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:746 on 2026-01-13 18:05_

actually, all dynamic namedtuples have an empty tuple generated for their `__slots__`:

```pycon
>>> from collections import namedtuple
>>> n = namedtuple("n", "a b")
>>> n.__slots__
()
```

But `__slots__` must be non-empty for a class to be a disjoint base, so your code is correct here -- it's just the comment that's slightly incorrect

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:137 on 2026-01-13 18:12_

this change is unnecessary: no tests fail if I revert this change. `CodeGeneratorKind::NamedTuple` is only understood as the code-generator kind for a class if the class has been created directly via `namedtuple()`. `class_kind` will be `None` for a class that inherits from a namedtuple class.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:7571 on 2026-01-13 18:14_

this change doesn't look necessary; no tests fail if I revert it

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:7627 on 2026-01-13 18:15_

This change doesn't look necessary to me, and no tests fail if I revert it

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:690 on 2026-01-13 18:16_

this change doesn't look necessary to me, and no tests fail if I revert it

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:602 on 2026-01-13 18:16_

this conversation was marked as resolved but the confusing comment about `NamedTupleFallback.__getattr__` (which doesn't exist) is still here

---

_@AlexWaygood reviewed on 2026-01-13 18:18_

I still don't think we need the new `directly_inherits_from_named_tuple_special_form()` method you've added; I can revert all the new uses of it and no tests pass. I think you only needed it in an earlier version because you had `CodeGeneratorKind::NamedTuple.matches(db, class)` returning `true` for all _subclasses_ of namedtuple classes as well as all namedtuple classes

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:236 on 2026-01-13 18:22_

```suggestion
from typing import NamedTuple
from ty_extensions import reveal_mro

class Url(NamedTuple("Url", [("host", str), ("path", str)])):
    pass

reveal_type(Url)  # revealed: <class 'Url'>
reveal_mro(Url)  # revealed: (<class 'main.Url @ main.py:5:7'>, <class 'main.Url @ main.py:5:11'>, <class 'tuple[str, str]'>, <class 'object'>)
```

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:602 on 2026-01-13 18:26_

Ugh sorry, I fixed the above use but not this one.

---

_@charliermarsh reviewed on 2026-01-13 18:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:310 on 2026-01-13 18:27_

```suggestion
import collections
from ty_extensions import reveal_mro

# String field names (space-separated)
Point1 = collections.namedtuple("Point", "x y")
reveal_type(Point1)  # revealed: <class 'Point'>
reveal_mro(Point1)  # revealed: (<class 'Point'>, tuple[Any, Any], <class 'object'>)

# String field names (comma-separated also works at runtime)
Point2 = collections.namedtuple("Point", "x, y")
reveal_type(Point2)  # revealed: <class 'Point'>
reveal_mro(Point2)  # revealed: (<class 'Point'>, tuple[Any, Any], <class 'object'>)

# List of strings
Point3 = collections.namedtuple("Point", ["x", "y"])
reveal_type(Point3)  # revealed: <class 'Point'>
reveal_mro(Point3)  # revealed: (<class 'Point'>, tuple[Any, Any], <class 'object'>)

# Tuple of strings
Point4 = collections.namedtuple("Point", ("x", "y"))
reveal_type(Point4)  # revealed: <class 'Point'>
reveal_mro(Point4)  # revealed: (<class 'Point'>, tuple[Any, Any], <class 'object'>)

# Invalid: integer is not a valid typename
# error: [invalid-argument-type]
reveal_type(collections.namedtuple(123, ["x", "y"]))  # revealed: type[NamedTupleFallback]
```

---

_@AlexWaygood reviewed on 2026-01-13 18:28_

---

_Converted to draft by @charliermarsh on 2026-01-13 18:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6282 on 2026-01-13 18:30_

```suggestion
    #[expect(clippy::type_complexity)]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6502 on 2026-01-13 18:30_

```suggestion
    #[expect(clippy::type_complexity)]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6542 on 2026-01-13 18:30_

```suggestion
    #[expect(clippy::type_complexity)]
```

---

_@AlexWaygood reviewed on 2026-01-13 18:30_

---

_Marked ready for review by @charliermarsh on 2026-01-13 18:58_

---

_@charliermarsh reviewed on 2026-01-13 19:01_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:3868 on 2026-01-13 19:01_

(I think this needs to be removed.)

---

_Comment by @carljm on 2026-01-13 19:26_

Going to let @AlexWaygood handle review here -- lmk if there's anything you particularly want me to look at.

---

_Review request for @carljm removed by @carljm on 2026-01-13 19:26_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-13 19:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1370 on 2026-01-13 20:23_

what's the reason that `DynamicNamedTupleLiteral::own_class_member`` returns `Option<PlaceAndQualifiers` whereas `DynamicClassLiteral::own_class_member` returns `Member`? It seems like they could both return `Member`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2128 on 2026-01-13 20:26_

but it seems like we still have false positives in other respects for tuples with unknown fields on this branch:

```py
from ty_extensions import reveal_mro
from collections import namedtuple

def f(x: str):
    Point = namedtuple("Point", x)
    p = Point(1, 2, 3)  # fine
    reveal_type(p.whatever)  # revealed: Unknown, fine

    p[0]  # error: [index-out-of-bounds], not fine
    reveal_mro(Point)  # revealed: (<class 'Point'>, <class 'tuple[()]'>, <class 'object'>)
```

I think namedtuple classes with unknown fields need to be inferred as inheriting from `tuple[Unknown, ...]` in order for these false positives to be removed

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2212 on 2026-01-13 20:27_

Can you add a test that demonstrates that we correctly infer them as inheriting the ordering methods from their tuple base classes? We get the behaviour correct on this branch, but it's untested -- we correctly do not emit a diagnostic on this:

```py
from collections import namedtuple
from functools import total_ordering

@total_ordering
class Point(namedtuple("Point", "x y")): ...
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3257 on 2026-01-13 20:28_

nice catch!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5173 on 2026-01-13 20:29_

Avoiding duplicating the synthesis logic seems like a good idea, but I only see this function called from the dynamic namedtuple code path -- I don't think this comment is accurate right now

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5411 on 2026-01-13 20:30_

`tuple_base_type` implies to me that this is going to return a `Type`; maybe this could be renamed to

```suggestion
    pub(crate) fn tuple_base_class(self, db: &'db dyn Db) -> ClassType<'db> {
```

?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6439 on 2026-01-13 20:35_

shall we just import `ast::name::Name` at the top of the file, so that the type annotation doesn't have to be split over four lines? 😛

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6317 on 2026-01-13 20:37_

should we also abort here if any of the positional arguments are starred expressions, or any of the keyword arguments are `**` arguments?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6360 on 2026-01-13 20:38_

I'm not sure you've added any tests for namedtuples that use `rename=True` or provide a value for the `defaults=` keyword parameter; can you add some?

Also, I'm not sure we emit any errors here if the user provides a nonexistent keyword argument like `foobarbaz=42`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6433 on 2026-01-13 20:41_

We can't return `None` here because we've already inferred (and stored) some types, but returning `None` will cause us to infer all types for the expression again, which could cause us to panic. As soon as we've stored at least one type for the call expression, we have to do the signature checking for the whole call expression "manually".

I also don't think it's _desirable_ to return `None` here, though. Can't we just use `<unknown>` for the name of the `collections.namedtuple` class, like we do for `type()` classes where a non-literal string is provided?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6478 on 2026-01-13 20:42_

can you add a test with a `Point = collections.namedtuple("Point", "x       y")`, and also one that uses a string with tabs in it? (Making sure that we infer the correct MRO in both cases)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6417 on 2026-01-13 20:44_

why are you using `exact_tuple_instance_spec()` here? I think `tuple_instance_spec()` should be fine?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6510 on 2026-01-13 20:45_

same here, I think `tuple_instance_spec` should be fine here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6532 on 2026-01-13 20:46_

nah, we should use `Type::in_type_expression()` here rather than reimplementing that method :P

---

_@AlexWaygood reviewed on 2026-01-13 20:46_

---

_@charliermarsh reviewed on 2026-01-13 21:01_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6433 on 2026-01-13 21:01_

Yes good call.

---

_@charliermarsh reviewed on 2026-01-13 21:14_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/infer/builder.rs`:6478 on 2026-01-13 21:14_

(Tab is causing me major problems because `mdformat` keeps removing it.)

---

_@AlexWaygood reviewed on 2026-01-13 21:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6478 on 2026-01-13 21:16_

hahaha is there no suppression comment to tell it to away??

---

_@AlexWaygood reviewed on 2026-01-13 21:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6437 on 2026-01-13 21:24_

can we move these imports to the top of the file?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:339 on 2026-01-13 21:46_

and add a `reveal_mro` call too?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:432 on 2026-01-13 21:47_

maybe also add `reveal_type(Point.__new__)` and `reveal_mro(Point)` calls?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:453 on 2026-01-13 21:48_

maybe also add a `reveal_type(Person.__new__)` call?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:487 on 2026-01-13 21:49_

worth adding `reveal_type` and `reveal_mro` calls for these?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1 on 2026-01-13 21:58_

Could you add a test for generic namedtuples defined using the functional syntax? I don't think it's a priority (at all) for us to support them, but it would be nice to at some point since [mypy does](https://mypy-play.net/?mypy=latest&python=3.12&gist=4bacc23ed34bd4552e65cc54eaf7a90d)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6285 on 2026-01-13 22:00_

we also need to emit a diagnostic in this method if too many arguments are provided, e.g.

```py
from ty_extensions import reveal_mro
from collections import namedtuple

NT = namedtuple("NT", "x", "y", "z", "aa", "bb")
```

and we need to make sure we call `self.infer_expression()` for every argument passed in, not just the first two

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6463 on 2026-01-13 22:04_

Uhh I think we had a conversation in https://github.com/astral-sh/ruff/pull/22291 about how calling `.fixed_elements()` is the wrong thing to do here -- you want to check whether it's _actually_ a _fixed-length_ tuple (by calling `Tuple::as_fixed_length`) before you iterate over it. If it's a tuple like `tuple[int, str, *tuple[bool, ...]]`, iterating over `.fixed_elements()` is going to give you the first two elements (the fixed ones) and then stop.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6548 on 2026-01-13 22:04_

same here, iterating over `.fixed_elements()` seems wrong -- you want to call `.as_fixed_length()` to check that it's _actually_ a fixed-length tuple

---

_@AlexWaygood reviewed on 2026-01-13 22:06_

Looks close! 🚀

---

_@AlexWaygood reviewed on 2026-01-13 22:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6562 on 2026-01-13 22:19_

rather than calling `.unwrap_or()`, we should report a diagnostic here if the user tried to pass an invalid type expression into the call

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6562 on 2026-01-13 22:20_

you can grep for other uses of `in_type_expression` to see how we convert errors into diagnostics -- it's fairly straightforward

---

_@AlexWaygood reviewed on 2026-01-13 22:20_

---
