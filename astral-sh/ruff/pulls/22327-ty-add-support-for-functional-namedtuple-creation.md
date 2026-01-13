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
updated_at: 2026-01-13T14:21:42Z
url: https://github.com/astral-sh/ruff/pull/22327
synced_at: 2026-01-13T14:32:13Z
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
--- old-output.txt	2026-01-13 02:25:31.359335812 +0000
+++ new-output.txt	2026-01-13 02:25:31.664338938 +0000
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
pip (https://github.com/pypa/pip)
+ src/pip/_vendor/urllib3/connectionpool.py:500:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ src/pip/_vendor/urllib3/util/url.py:357:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ src/pip/_vendor/urllib3/util/url.py:419:12: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 602 diagnostics
+ Found 605 diagnostics

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

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/test_messagequeue.py:792:32: error[invalid-argument-type] Argument to bound method `_consume_one` is incorrect: Expected `Item`, found `tuple[Literal[""], None, Literal[""], Literal["{}"], Literal[""], Literal[""]]`
+ tests/test_messagequeue.py:811:32: error[invalid-argument-type] Argument to bound method `_consume_one` is incorrect: Expected `Item`, found `tuple[Literal[""], None, Literal[""], Literal["{}"], Literal[""], Literal[""]]`
- Found 240 diagnostics
+ Found 242 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/enums.py:97:5: error[invalid-assignment] Object of type `(self) -> Unknown` is not assignable to attribute `__repr__` of type `def __repr__(self) -> str`
+ discord/enums.py:98:5: error[invalid-assignment] Object of type `(self) -> Unknown` is not assignable to attribute `__str__` of type `def __str__(self) -> str`
+ discord/enums.py:100:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__le__` of type `def __le__(self, value: tuple[Any, ...], /) -> bool`
+ discord/enums.py:101:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__ge__` of type `def __ge__(self, value: tuple[Any, ...], /) -> bool`
+ discord/enums.py:102:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__lt__` of type `def __lt__(self, value: tuple[Any, ...], /) -> bool`
+ discord/enums.py:103:9: error[invalid-assignment] Object of type `(self, other) -> Unknown` is not assignable to attribute `__gt__` of type `def __gt__(self, value: tuple[Any, ...], /) -> bool`
- Found 548 diagnostics
+ Found 554 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
+ pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Any | None | bytes`
- pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[str, ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[CoroutineType[Any, Any, None]]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
+ pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[str, ((credentials: MongoCredential, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: MongoCredential, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[CoroutineType[Any, Any, None]]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
- pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
+ pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Any | None | bytes`
- pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[str, ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[None]]` is not assignable to `Mapping[str, (...) -> None]`
+ pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[str, ((credentials: MongoCredential, conn: Connection) -> None) | ((credentials: MongoCredential, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[None]]` is not assignable to `Mapping[str, (...) -> None]`

vision (https://github.com/pytorch/vision)
+ test/test_video_reader.py:39:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:40:5: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:41:5: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:42:5: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:43:5: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:44:5: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:48:44: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:49:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:50:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:51:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:52:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:53:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:55:60: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:56:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:57:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:58:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:59:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:60:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:62:47: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:63:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:64:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:65:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:66:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:67:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:69:37: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:70:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:71:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:72:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:73:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:74:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:76:37: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:77:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:78:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:79:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:80:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:81:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:83:24: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:84:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:85:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:86:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:88:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:89:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:91:24: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:92:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:93:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:94:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:96:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:97:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_video_reader.py:99:24: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_video_reader.py:100:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_video_reader.py:101:9: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_video_reader.py:102:9: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_video_reader.py:104:9: error[unknown-argument] Argument `check_aframes` does not match any known parameter
+ test/test_video_reader.py:105:9: error[unknown-argument] Argument `check_aframe_pts` does not match any known parameter
+ test/test_videoapi.py:52:44: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:52:56: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:52:70: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:52:86: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_videoapi.py:53:60: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:54:9: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:54:23: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:54:39: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_videoapi.py:56:47: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:56:59: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:56:73: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:56:89: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_videoapi.py:57:37: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:57:49: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:57:63: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:57:80: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_videoapi.py:58:37: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:58:49: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:58:63: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:58:80: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_videoapi.py:59:24: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:59:36: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:59:51: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:59:67: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_videoapi.py:60:24: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:60:36: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:60:51: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:60:68: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
+ test/test_videoapi.py:61:24: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ test/test_videoapi.py:61:36: error[unknown-argument] Argument `duration` does not match any known parameter
+ test/test_videoapi.py:61:51: error[unknown-argument] Argument `video_fps` does not match any known parameter
+ test/test_videoapi.py:61:68: error[unknown-argument] Argument `audio_sample_rate` does not match any known parameter
- Found 1409 diagnostics
+ Found 1495 diagnostics

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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

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
- Found 8399 diagnostics
+ Found 8403 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/interpreters/ad.py:430:7: error[call-non-callable] Object of type `aval_method` is not callable
- jax/_src/pallas/mosaic/sc_primitives.py:108:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:173:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:198:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_primitives.py:215:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ jax/_src/pallas/mosaic_gpu/pipeline.py:123:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> Unknown, (index: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` cannot be called with key of type `tuple[Slice | Array, ...]` on object of type `aval_property`
+ jax/_src/pallas/mosaic_gpu/pipeline.py:136:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> Unknown, (index: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` cannot be called with key of type `tuple[Slice | Array, ...]` on object of type `aval_property`
- Found 2803 diagnostics
+ Found 2802 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/profile/__main__.py:2135:9: error[invalid-assignment] Object of type `dict[str, FunctionMetaData | None]` is not assignable to attribute `meta` of type `dict[str, FunctionMetaData] | None`
- Found 1827 diagnostics
+ Found 1825 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/tests/frame/indexing/test_indexing.py:1409:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ pandas/tests/frame/indexing/test_indexing.py:1409:30: error[unknown-argument] Argument `first` does not match any known parameter
+ pandas/tests/frame/indexing/test_indexing.py:1409:41: error[unknown-argument] Argument `second` does not match any known parameter
+ pandas/tests/frame/test_constructors.py:1578:19: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ pandas/tests/frame/test_constructors.py:1578:31: error[invalid-argument-type] Argument is incorrect: Expected `Iterable[Unknown]`, found `Literal[1]`
+ pandas/tests/frame/test_constructors.py:1578:34: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
+ pandas/tests/frame/test_constructors.py:1578:38: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ pandas/tests/frame/test_constructors.py:1578:50: error[invalid-argument-type] Argument is incorrect: Expected `Iterable[Unknown]`, found `Literal[2]`
+ pandas/tests/frame/test_constructors.py:1578:53: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
- Found 3794 diagnostics
+ Found 3803 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/tests/utils/mock.py:73:16: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ rotkehlchen/tests/utils/mock.py:73:51: error[unknown-argument] Argument `version` does not match any known parameter
- Found 2055 diagnostics
+ Found 2057 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5170 diagnostics
+ Found 5169 diagnostics

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
| `unknown-argument` | 72 | 0 | 0 |
| `invalid-argument-type` | 30 | 0 | 5 |
| `invalid-assignment` | 14 | 1 | 7 |
| `possibly-missing-attribute` | 4 | 0 | 10 |
| `call-non-callable` | 7 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 5 |
| `unused-ignore-comment` | 0 | 5 | 0 |
| `not-subscriptable` | 3 | 0 | 0 |
| `too-many-positional-arguments` | 3 | 0 | 0 |
| `unresolved-attribute` | 1 | 0 | 2 |
| `invalid-await` | 2 | 0 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **246** | **6** | **29** |


**[Full report with detailed diff](https://80b240b7.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://80b240b7.ty-ecosystem-ext.pages.dev/timing))



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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:152 on 2026-01-13 12:47_

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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:186 on 2026-01-13 13:02_

And this is because for `class Url(namedtuple("Url", "host port")): ...`, the inner "inline" `Url` class is a namedtuple class but the outer one is not. So it's really equivalent to doing

```py
class Url(NamedTuple):
    host: str
    port: int

class Url(Url): ...
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:309 on 2026-01-13 13:02_

And again, this is because they aren't actually namedtuple classes, they just inherit from namedtuple classes

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:230 on 2026-01-13 13:04_

I'm confused, I don't see a `__getattr__` method here?

https://github.com/astral-sh/ruff/blob/990d0a899972a59cccdf69d57c56358773d2da5c/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/_type_checker_internals.pyi#L54-L75

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:522 on 2026-01-13 13:04_

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
