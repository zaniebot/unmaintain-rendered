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
draft: true
base: charlie/dyn-expression
head: charlie/functional-namedtuple
created_at: 2026-01-01T13:23:44Z
updated_at: 2026-01-12T21:39:25Z
url: https://github.com/astral-sh/ruff/pull/22327
synced_at: 2026-01-12T22:24:50Z
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
--- old-output.txt	2026-01-12 13:48:03.929800365 +0000
+++ new-output.txt	2026-01-12 13:48:04.282801771 +0000
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
- Found 1102 diagnostics
+ Found 1103 diagnostics

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

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/test_messagequeue.py:792:32: error[invalid-argument-type] Argument to bound method `_consume_one` is incorrect: Expected `Item`, found `tuple[Literal[""], None, Literal[""], Literal["{}"], Literal[""], Literal[""]]`
+ tests/test_messagequeue.py:811:32: error[invalid-argument-type] Argument to bound method `_consume_one` is incorrect: Expected `Item`, found `tuple[Literal[""], None, Literal[""], Literal["{}"], Literal[""], Literal[""]]`
- Found 240 diagnostics
+ Found 242 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

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
+ discord/enums.py:100:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__le__` of type `def __le__(self, value: tuple[Any, ...], /) -> bool`
+ discord/enums.py:101:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__ge__` of type `def __ge__(self, value: tuple[Any, ...], /) -> bool`
+ discord/enums.py:102:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__lt__` of type `def __lt__(self, value: tuple[Any, ...], /) -> bool`
+ discord/enums.py:103:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__gt__` of type `def __gt__(self, value: tuple[Any, ...], /) -> bool`
- Found 548 diagnostics
+ Found 554 diagnostics

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
- Found 1179 diagnostics
+ Found 1188 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
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
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5362 diagnostics
+ Found 5367 diagnostics

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
- Found 8473 diagnostics
+ Found 8477 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/interpreters/ad.py:430:7: error[call-non-callable] Object of type `aval_method` is not callable
- jax/_src/pallas/mosaic/sc_primitives.py:108:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:173:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:198:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:215:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ jax/_src/pallas/mosaic_gpu/pipeline.py:123:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> Unknown, (index: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` cannot be called with key of type `tuple[Slice | Array, ...]` on object of type `aval_property`
+ jax/_src/pallas/mosaic_gpu/pipeline.py:136:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> Unknown, (index: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` cannot be called with key of type `tuple[Slice | Array, ...]` on object of type `aval_property`
- Found 2802 diagnostics
+ Found 2801 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/profile/__main__.py:2135:9: error[invalid-assignment] Object of type `dict[str, FunctionMetaData | None]` is not assignable to attribute `meta` of type `dict[str, FunctionMetaData] | None`
- Found 1827 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2104 diagnostics
+ Found 2103 diagnostics

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
- Found 8081 diagnostics
+ Found 8127 diagnostics


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
| `invalid-argument-type` | 28 | 2 | 5 |
| `invalid-assignment` | 14 | 1 | 7 |
| `possibly-missing-attribute` | 1 | 0 | 9 |
| `call-non-callable` | 7 | 0 | 0 |
| `unused-ignore-comment` | 2 | 5 | 0 |
| `invalid-return-type` | 0 | 0 | 5 |
| `not-subscriptable` | 3 | 0 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| `unresolved-attribute` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **166** | **8** | **26** |


**[Full report with detailed diff](https://eaefd911.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://eaefd911.ty-ecosystem-ext.pages.dev/timing))



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
