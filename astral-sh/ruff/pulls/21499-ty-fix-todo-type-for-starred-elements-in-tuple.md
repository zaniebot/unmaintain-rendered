```yaml
number: 21499
title: "[ty] Fix `Todo` type for starred elements in tuple/list/set expressions"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/starred-tuples
created_at: 2025-11-17T12:03:04Z
updated_at: 2026-01-11T16:36:14Z
url: https://github.com/astral-sh/ruff/pull/21499
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Fix `Todo` type for starred elements in tuple/list/set expressions

---

_@AlexWaygood_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ty/issues/2071

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-11-17 12:03_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 12:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-27 18:05:24.128481351 +0000
+++ new-output.txt	2025-11-27 18:05:27.473482077 +0000
@@ -456,13 +456,10 @@
 generics_defaults.py:80:53: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
 generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
 generics_defaults.py:81:68: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
-generics_defaults.py:91:26: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_defaults.py:94:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `<class 'Class_TypeVarTuple'>`
 generics_defaults.py:95:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `Class_TypeVarTuple`
 generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
 generics_defaults.py:131:1: error[type-assertion-failure] Type `Any` does not match asserted type `int`
-generics_defaults.py:142:12: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_defaults.py:152:12: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_defaults.py:155:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
 generics_defaults.py:156:58: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[bytes]`?
 generics_defaults.py:170:1: error[type-assertion-failure] Type `(Foo7[int], /) -> Foo7[int]` does not match asserted type `def meth(self, /) -> Self@meth`
@@ -597,16 +594,13 @@
 generics_typevartuple_args.py:27:77: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_args.py:31:1: error[type-assertion-failure] Type `tuple[()]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_args.py:32:1: error[type-assertion-failure] Type `tuple[int, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo(PEP 646), ...]`, found `Height`
 generics_typevartuple_basic.py:65:27: error[unknown-argument] Argument `covariant` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:75:50: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:84:1: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:106:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_callable.py:29:57: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:33:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[int | float | complex, str, int]`
 generics_typevartuple_callable.py:37:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[str]`
@@ -614,11 +608,8 @@
 generics_typevartuple_callable.py:42:1: error[type-assertion-failure] Type `tuple[str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:45:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Type `tuple[int | float, str, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_concat.py:22:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:50:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
@@ -626,7 +617,6 @@
 generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
@@ -640,7 +630,6 @@
 generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `@Todo(Support for `typing.GenericAlias` instances in type expressions)`
 generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_unpack.py:17:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
 generics_upper_bound.py:43:1: error[type-assertion-failure] Type `list[int] | set[int]` does not match asserted type `list[Unknown | int] | set[Unknown | int]`
@@ -1029,4 +1018,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1031 diagnostics
+Found 1020 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-17 12:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_sockets.py:385:56: error[invalid-argument-type] Argument to function `setup_raw_socket` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `tuple[str | Unknown, int, @Todo]`
- Found 88 diagnostics
+ Found 87 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/utils/draw.py:376:5: error[invalid-assignment] Not enough values to unpack: Expected 5
- kornia/utils/draw.py:379:5: error[invalid-assignment] Not enough values to unpack: Expected 5
- kornia/utils/draw.py:382:5: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 763 diagnostics
+ Found 760 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/_internal/robot/utils.py:231:25: warning[redundant-cast] Value is already of type `list[object] | None`
- Found 167 diagnostics
+ Found 168 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/llnl/util/link_tree.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo]`
- lib/spack/spack/llnl/util/link_tree.py:151:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str, @Todo]`
- lib/spack/spack/spec.py:5691:69: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 7960 diagnostics
+ Found 7957 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/reports.py:302:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, int, str]`, found `tuple[@Todo, str]`
- src/_pytest/terminal.py:1582:23: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, str, int | None, str]`, found `tuple[int, @Todo]`
- Found 457 diagnostics
+ Found 455 diagnostics

black (https://github.com/psf/black)
- src/black/cache.py:144:59: error[invalid-assignment] Object of type `dict[str | Unknown, tuple[@Todo] | Unknown]` is not assignable to `dict[str, tuple[int | float, int, str]]`
- Found 63 diagnostics
+ Found 62 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/utils/env/mock_env.py:34:28: error[invalid-assignment] Object of type `tuple[@Todo, Literal["final"], Literal[0]]` is not assignable to `tuple[int, int, int] | tuple[int, int, int, str, int]`
+ src/poetry/utils/env/mock_env.py:34:28: error[invalid-assignment] Object of type `tuple[*tuple[int | str, ...], Literal["final"], Literal[0]]` is not assignable to `tuple[int, int, int] | tuple[int, int, int, str, int]`
+ tests/puzzle/test_solver.py:614:29: error[invalid-assignment] Object of type `list[str]` is not assignable to `list[Literal["a", "b", "download-package", "install-package"]]`
- Found 970 diagnostics
+ Found 971 diagnostics

vision (https://github.com/pytorch/vision)
+ test/test_datasets.py:698:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["trainval"]` and value of type `tuple[int, ...]` on object of type `dict[str, tuple[int, int, int] | tuple[int, int] | tuple[int]]`
- torchvision/datasets/utils.py:268:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str | None, str | None]`, found `tuple[@Todo, @Todo]`

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_dtypes.py:169:43: error[invalid-assignment] Object of type `list[tuple[dict[Unknown, Unknown], list[Unknown]] | tuple[dict[Unknown | <class 'datetime'> | <class 'datetime64'> | ... omitted 4 union elements, Unknown | str], Series[pandas._libs.tslibs.timestamps.Timestamp]] | tuple[dict[Unknown | PeriodDtype, Unknown | str], Series[Any]] | tuple[dict[Unknown | <class 'SparseDtype'> | SparseDtype, Unknown | SparseDtype], Series[Any]] | tuple[dict[Unknown | IntervalDtype, Unknown | str], Series[Any]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`
+ tests/pandas/test_dtypes.py:169:43: error[invalid-assignment] Object of type `list[tuple[dict[Unknown, Unknown], list[Unknown]] | Unknown | tuple[dict[Unknown | <class 'FLOAT32'> | <class 'FLOAT64'>, Unknown | str], list[Unknown | float | None]] | ... omitted 4 union elements]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/commands/data_commands.py:172:21: error[invalid-assignment] Not enough values to unpack: Expected 6
- freqtrade/commands/data_commands.py:226:21: error[invalid-assignment] Not enough values to unpack: Expected 4
- Found 689 diagnostics
+ Found 687 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/transformers.py:584:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- discord/ext/commands/converter.py:1241:13: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 550 diagnostics
+ Found 548 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/schema.py:80:17: error[invalid-assignment] Object of type `list[Unknown | NewType]` is not assignable to `Iterable[type]`
+ strawberry/federation/schema.py:80:17: error[invalid-assignment] Object of type `list[Unknown | type | NewType]` is not assignable to `Iterable[type]`

apprise (https://github.com/caronc/apprise)
- tests/test_plugin_email.py:634:5: error[invalid-assignment] Object of type `tuple[@Todo, @Todo]` is not assignable to attribute `EMAIL_TEMPLATES` of type `tuple[tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]`
+ tests/test_plugin_email.py:634:5: error[invalid-assignment] Object of type `tuple[tuple[Literal["Testing Lookup"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]` is not assignable to attribute `EMAIL_TEMPLATES` of type `tuple[tuple[Literal["Google Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Yandex"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Hotmail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Outlook"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Microsoft Office 365"], Pattern[str], dict[Unknown | str, Unknown | int | str]], tuple[Literal["Yahoo Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Fast Mail Extended Addresses"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Zoho Mail"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["SendGrid"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["163.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Foxmail.com"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Comcast.net"], Pattern[str], dict[Unknown | str, Unknown | int | str | tuple[Unknown | str]]], tuple[Literal["Local Mail Server"], Pattern[str], dict[Unknown | str, Unknown | str | None]], tuple[Literal["Custom"], Pattern[str], dict[Unknown | str, Unknown | None]]]`

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/blocks/core.py:86:33: error[invalid-assignment] Object of type `tuple[<class 'list'>, <class 'dict'>, <class 'tuple'>, *tuple[typing.Union | <class 'UnionType'>, ...]]` is not assignable to `tuple[type, ...]`
- Found 3300 diagnostics
+ Found 3301 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/i18n/__init__.py:209:13: error[invalid-assignment] Not enough values to unpack: Expected 6
- openlibrary/plugins/worksearch/code.py:304:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int] | tuple[str, str]]`
+ openlibrary/plugins/worksearch/code.py:304:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, str] | tuple[str, Unknown | int]]`
- openlibrary/utils/isbn.py:112:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | None, str | None, str | None]`, found `tuple[str | None, @Todo]`
- Found 1120 diagnostics
+ Found 1118 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 44 diagnostics

altair (https://github.com/vega/altair)
- tests/vegalite/v6/schema/test_channels.py:56:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1026 diagnostics
+ Found 1025 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/coverage/instrumentation_py3_11.py:338:46: error[invalid-argument-type] Argument to bound method `from_bytes` is incorrect: Expected `Iterable[SupportsIndex] | SupportsBytes | Buffer`, found `list[Unknown | bytes]`
+ ddtrace/internal/coverage/instrumentation_py3_12.py:216:46: error[invalid-argument-type] Argument to bound method `from_bytes` is incorrect: Expected `Iterable[SupportsIndex] | SupportsBytes | Buffer`, found `list[Unknown | bytes]`
- Found 8270 diagnostics
+ Found 8272 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/actions/message_send.py:1136:35: error[invalid-assignment] Object of type `dict[str, Unknown | None]` is not assignable to `UserData`
+ zerver/actions/message_send.py:1136:35: error[invalid-assignment] Object of type `dict[str, Unknown | int | None]` is not assignable to `UserData`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/models/__init__.py:108:11: error[invalid-assignment] Object of type `tuple[Literal["Model"], @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo]` is not assignable to `tuple[@Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo]`
- src/bokeh/models/annotations/__init__.py:44:11: error[invalid-assignment] Object of type `tuple[@Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo]` is not assignable to `tuple[@Todo, @Todo, @Todo]`
+ src/bokeh/server/tornado.py:448:41: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, type[RequestHandler]] | tuple[str, type[RequestHandler], dict[str, Any]]`, found `tuple[str | Unknown, *tuple[type[RequestHandler] | dict[str, Any] | Unknown, ...], dict[Unknown | str, Unknown | dict[Unknown, Unknown] | str | None | bool]]`
+ src/bokeh/server/tornado.py:451:37: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, type[RequestHandler]] | tuple[str, type[RequestHandler], dict[str, Any]]`, found `tuple[str | Unknown, *tuple[type[RequestHandler] | dict[str, Any] | Unknown, ...]]`

jax (https://github.com/google/jax)
- jax/_src/pallas/fuser/block_spec.py:854:10: error[invalid-return-type] Return type does not match returned value: expected `Sequence[BlockSpec]`, found `list[Unknown | NoBlockSpec]`
+ jax/_src/pallas/fuser/block_spec.py:854:10: error[invalid-return-type] Return type does not match returned value: expected `Sequence[BlockSpec]`, found `list[Unknown | NoBlockSpec | BlockSpec]`
- jax/_src/pallas/hlo_interpreter.py:399:33: error[invalid-argument-type] Argument to function `pad` is incorrect: Expected `Sequence[tuple[int, int, int]]`, found `list[tuple[@Todo, int] | Unknown]`
+ jax/_src/pallas/hlo_interpreter.py:399:33: error[invalid-argument-type] Argument to function `pad` is incorrect: Expected `Sequence[tuple[int, int, int]]`, found `list[tuple[*tuple[int | Unknown, ...], int] | Unknown]`
- jax/experimental/mosaic/gpu/utils.py:1977:10: error[invalid-return-type] Return type does not match returned value: expected `tuple[Unknown, Unknown, Unknown, Unknown]`, found `tuple[@Todo, Unknown]`
- jax/experimental/pallas/ops/gpu/all_gather_mgpu.py:91:6: error[unsupported-operator] Operator `%` is unsupported between objects of type `@Todo | int` and `int | None`
+ jax/experimental/pallas/ops/gpu/all_gather_mgpu.py:91:6: error[unsupported-operator] Operator `%` is unsupported between objects of type `int` and `int | None`
- jax/experimental/pallas/ops/gpu/reduce_scatter_mgpu.py:106:6: error[unsupported-operator] Operator `%` is unsupported between objects of type `Unknown | Literal[1]` and `int | None`
+ jax/experimental/pallas/ops/gpu/reduce_scatter_mgpu.py:106:6: error[unsupported-operator] Operator `%` is unsupported between objects of type `int` and `int | None`
- jax/experimental/slab/slab.py:248:3: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 2732 diagnostics
+ Found 2730 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/parsing/latex/__init__.py:117:12: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:118:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:120:59: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:125:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:126:30: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:128:53: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:133:43: error[index-out-of-bounds] Index 3 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:135:30: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:136:12: error[index-out-of-bounds] Index 7 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:139:37: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:140:30: error[index-out-of-bounds] Index 6 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:140:47: error[index-out-of-bounds] Index 4 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- sympy/parsing/latex/__init__.py:141:65: error[index-out-of-bounds] Index 5 is out of bounds for tuple `tuple[@Todo, @Todo]` with length 2
- Found 15187 diagnostics
+ Found 15174 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:1039:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/decoding/thegraph/balances.py:139:43: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[ChecksumAddress, ChecksumAddress, ChecksumAddress, int]`, found `tuple[@Todo, Unknown]`
- Found 2043 diagnostics
+ Found 2040 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/esphome/light.py:337:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, Unknown]`
- homeassistant/components/esphome/light.py:358:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, Unknown, Unknown]`
- homeassistant/components/esphome/light.py:363:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, Unknown, Unknown]`
- homeassistant/components/flux_led/util.py:98:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, int]`
- homeassistant/components/flux_led/util.py:99:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, int]`
- homeassistant/components/flux_led/util.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, int, int]`
- homeassistant/components/flux_led/util.py:115:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int, int]`, found `tuple[@Todo, int, int]`
- homeassistant/components/knx/light.py:375:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int] | None`, found `tuple[@Todo, Unknown & ~None]`
- homeassistant/components/rfxtrx/__init__.py:212:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/rfxtrx/entity.py:24:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/shelly/light.py:229:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, int]`
- homeassistant/components/shelly/light.py:404:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, int, int, int]`, found `tuple[@Todo, Unknown]`
+ homeassistant/components/zwave_js/discovery.py:1389:17: error[invalid-argument-type] Argument is incorrect: Expected `type[ZWaveBaseEntity]`, found `type`
- Found 14532 diagnostics
+ Found 14521 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/builtins.py:1073:31: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- python/egglog/thunk.py:36:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- python/egglog/thunk.py:83:18: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
- Found 1492 diagnostics
+ Found 1489 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/interpolate/tests/test_bsplines.py:1802:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1814:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1827:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1837:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1851:9: error[invalid-assignment] Not enough values to unpack: Expected 4
- scipy/interpolate/tests/test_bsplines.py:1862:9: error[invalid-assignment] Not enough values to unpack: Expected 3
- scipy/interpolate/tests/test_bsplines.py:1874:9: error[invalid-assignment] Not enough values to unpack: Expected 3
- Found 7845 diagnostics
+ Found 7838 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-11-17 12:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fstarred-tuples?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21499 will **not alter performance**

<sub>Comparing <code>alex/starred-tuples</code> (2638777) with <code>main</code> (df66946)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fstarred-tuples?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Renamed from "[ty] Fix `Todo` type for starred elements in tuple expressions" to "[ty] Fix `Todo` type for starred elements in tuple/list/set expressions" by @AlexWaygood on 2025-11-17 12:41_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-17 12:46_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 12:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 3 | 20 | 5 |
| `invalid-return-type` | 0 | 16 | 1 |
| `index-out-of-bounds` | 0 | 13 | 0 |
| `invalid-argument-type` | 5 | 6 | 2 |
| `unused-ignore-comment` | 0 | 3 | 0 |
| `unsupported-base` | 0 | 2 | 0 |
| `unsupported-operator` | 0 | 0 | 2 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **9** | **60** | **10** |

**[Full report with detailed diff](https://alex-starred-tuples.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-starred-tuples.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-11-17 12:59_

> ```diff
> - src/bokeh/models/annotations/__init__.py:44:1: error[invalid-assignment] Object of type `tuple[@Todo, @Todo, @Todo, @Todo, @Todo, @Todo, @Todo]` is not assignable to `tuple[@Todo, @Todo, @Todo]`
> ```

Well I definitely won't mourn that diagnostic going away

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-17 15:03_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-17 15:03_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-27 18:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-27 18:06_

---

_Closed by @AlexWaygood on 2026-01-11 16:34_

---

_Branch deleted on 2026-01-11 16:34_

---

_Comment by @AlexWaygood on 2026-01-11 16:36_

This was hopelessly merge-conflicted, but I recreated it as https://github.com/astral-sh/ruff/pull/22503

---
