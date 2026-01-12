```yaml
number: 21320
title: "[ty] Promote literals in invariant return position"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ty
assignees: []
base: main
head: ibraheem/generic-promotion
created_at: 2025-11-07T16:59:20Z
updated_at: 2025-11-13T21:36:47Z
url: https://github.com/astral-sh/ruff/pull/21320
synced_at: 2026-01-12T15:57:21Z
```

# [ty] Promote literals in invariant return position

---

_@ibraheemdev_

## Summary

We currently unconditionally promote literals in generic class constructors. We should do this for any literal in an invariant position in the return type. Resolves https://github.com/astral-sh/ty/issues/1357.

This PR is stacked on https://github.com/astral-sh/ruff/pull/20933.

---

_Review requested from @carljm by @ibraheemdev on 2025-11-07 16:59_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-07 16:59_

---

_Label `ty` added by @ibraheemdev on 2025-11-07 16:59_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-07 16:59_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-07 16:59_

---

_Comment by @github-actions[bot] on 2025-11-07 17:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-10 23:05:16.349873661 +0000
+++ new-output.txt	2025-11-10 23:05:19.738890404 +0000
@@ -688,6 +688,8 @@
 literals_interactions.py:60:49: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[A@Matrix, B@Matrix]`
 literals_interactions.py:63:52: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[A@Matrix, C@__matmul__]`
 literals_interactions.py:66:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[B@Matrix, A@Matrix]`
+literals_interactions.py:71:9: error[unsupported-operator] Operator `@` is unsupported between objects of type `Matrix[Literal[2], Literal[3]]` and `Matrix[Literal[3], Literal[7]]`
+literals_interactions.py:72:5: error[type-assertion-failure] Argument does not have asserted type `Matrix[Literal[2], Literal[7]]`
 literals_interactions.py:106:35: error[invalid-argument-type] Argument to function `expects_bad_status` is incorrect: Expected `Literal["MALFORMED", "ABORTED"]`, found `str`
 literals_interactions.py:109:32: error[invalid-argument-type] Argument to function `expects_pending_status` is incorrect: Expected `Literal["PENDING"]`, found `str`
 literals_literalstring.py:23:51: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `LiteralString`
@@ -1007,5 +1009,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1009 diagnostics
+Found 1011 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @github-actions[bot] on 2025-11-07 17:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
nox (https://github.com/wntrblm/nox)
- nox/_resolver.py:166:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Node@lazy_stable_topo_sort, Literal[False]].__setitem__(key: Node@lazy_stable_topo_sort, value: Literal[False], /) -> None` cannot be called with a key of type `Node@lazy_stable_topo_sort` and a value of type `Literal[True]` on object of type `dict[Node@lazy_stable_topo_sort, Literal[False]]`
- Found 28 diagnostics
+ Found 27 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- dedupe/predicates.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
- dedupe/predicates.py:118:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
- Found 59 diagnostics
+ Found 57 diagnostics

Expression (https://github.com/cognitedata/Expression)
- tests/test_map.py:142:34: error[invalid-argument-type] Argument to bound method `try_get_value` is incorrect: Expected `list[Literal[1, 2]]`, found `list[int]`
- tests/test_map.py:143:34: error[invalid-argument-type] Argument to bound method `try_get_value` is incorrect: Expected `list[Literal[1, 2]]`, found `list[int]`
- tests/test_map.py:144:34: error[invalid-argument-type] Argument to bound method `try_get_value` is incorrect: Expected `list[Literal[1, 2]]`, found `list[int]`
- Found 198 diagnostics
+ Found 195 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_adapters_map.py:147:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown, Literal[False]].__setitem__(key: Unknown, value: Literal[False], /) -> None) | (bound method dict[Unknown, Literal[True]].__setitem__(key: Unknown, value: Literal[True], /) -> None)` cannot be called with a key of type `PyFormat` and a value of type `Literal[True]` on object of type `Unknown | dict[Unknown, Literal[False]] | dict[Unknown, Literal[True]]`
- Found 666 diagnostics
+ Found 665 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/computation/rolling.py:112:45: error[invalid-argument-type] Argument to bound method `_mapping_to_list` is incorrect: Expected `Literal[False] | Mapping[Any, Literal[False]]`, found `bool | Mapping[Any, bool]`
- xarray/computation/rolling.py:453:41: error[invalid-argument-type] Argument to bound method `_mapping_to_list` is incorrect: Expected `Literal[1] | Mapping[Any, Literal[1]]`, found `int | Mapping[Any, int]`
- xarray/computation/rolling.py:989:41: error[invalid-argument-type] Argument to bound method `_mapping_to_list` is incorrect: Expected `Literal[1] | Mapping[Any, Literal[1]]`, found `int | Mapping[Any, int]`
- Found 1755 diagnostics
+ Found 1752 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/build.py:1293:9: error[invalid-assignment] Object of type `list[Sequence[str | Literal[False]] | Literal[False]]` is not assignable to attribute `install_dir` of type `list[str | Literal[False]]`
+ mesonbuild/build.py:1293:9: error[invalid-assignment] Object of type `list[Sequence[str | bool] | Literal[False]]` is not assignable to attribute `install_dir` of type `list[str | Literal[False]]`

manticore (https://github.com/trailofbits/manticore)
- manticore/core/manticore.py:1078:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `value` on type `Unknown | c_bool | ValueProxy[Literal[False]]`
- manticore/core/manticore.py:1156:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `value` on type `Unknown | c_bool | ValueProxy[Literal[False]]`
- Found 11081 diagnostics
+ Found 11079 diagnostics

altair (https://github.com/vega/altair)
- altair/utils/core.py:219:1: error[invalid-assignment] Object of type `frozenset[str]` is not assignable to `frozenset[Literal["field", "aggregate", "type", "timeUnit"]]`
- Found 1021 diagnostics
+ Found 1020 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/bigquery/tests/unit/test_compiler.py:273:26: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, Literal["string"]]`
+ ibis/backends/bigquery/tests/unit/test_compiler.py:273:26: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[str | Unknown, str]`
- ibis/backends/tests/test_udf.py:82:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, int]`, found `dict[Unknown, Literal[0]]`
- ibis/backends/tests/test_udf.py:110:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, int]`, found `dict[Unknown, Literal[0]]`
- ibis/backends/tests/test_udf.py:124:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, int]`, found `dict[Unknown, Literal[0]]`
- Found 4240 diagnostics
+ Found 4237 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/_exploit_prevention/stack_traces.py:50:44: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[Literal[2]]`, in comparing `int` with `(Unknown & ~AlwaysFalsy) | (EnvVariable[Literal[2]] & ~AlwaysFalsy)`
+ ddtrace/appsec/_exploit_prevention/stack_traces.py:50:44: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `(Unknown & ~AlwaysFalsy) | (EnvVariable[int] & ~AlwaysFalsy)`
- ddtrace/appsec/_exploit_prevention/stack_traces.py:65:8: error[unsupported-operator] Operator `>` is not supported for types `int` and `EnvVariable[Literal[32]]`, in comparing `int` with `Unknown | EnvVariable[Literal[32]]`
+ ddtrace/appsec/_exploit_prevention/stack_traces.py:65:8: error[unsupported-operator] Operator `>` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `Unknown | EnvVariable[int]`
- ddtrace/appsec/_exploit_prevention/stack_traces.py:65:21: error[unsupported-operator] Operator `>` is not supported for types `EnvVariable[Literal[32]]` and `int`, in comparing `Unknown | EnvVariable[Literal[32]]` with `Literal[0]`
+ ddtrace/appsec/_exploit_prevention/stack_traces.py:65:21: error[unsupported-operator] Operator `>` is not supported for types `EnvVariable[int]` and `int`, in comparing `Unknown | EnvVariable[int]` with `Literal[0]`
- ddtrace/appsec/_exploit_prevention/stack_traces.py:66:25: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | EnvVariable[Literal[32]]` and `Unknown | EnvVariable[float]`
+ ddtrace/appsec/_exploit_prevention/stack_traces.py:66:25: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | EnvVariable[int]` and `Unknown | EnvVariable[float]`
- ddtrace/appsec/_exploit_prevention/stack_traces.py:67:24: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | EnvVariable[Literal[32]]` and `int`
+ ddtrace/appsec/_exploit_prevention/stack_traces.py:67:24: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | EnvVariable[int]` and `int`
- ddtrace/appsec/_iast/_metrics.py:25:23: warning[possibly-missing-attribute] Attribute `upper` may be missing on object of type `Unknown | EnvVariable[Literal["INFORMATION"]]`
+ ddtrace/appsec/_iast/_metrics.py:25:23: warning[possibly-missing-attribute] Attribute `upper` may be missing on object of type `Unknown | EnvVariable[str]`
- ddtrace/appsec/_iast/sampling/vulnerability_detection.py:40:8: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[Literal[2]]`, in comparing `int` with `Unknown | EnvVariable[Literal[2]]`
+ ddtrace/appsec/_iast/sampling/vulnerability_detection.py:40:8: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `Unknown | EnvVariable[int]`
- ddtrace/appsec/_iast/sampling/vulnerability_detection.py:88:8: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[Literal[2]]`, in comparing `Unknown | int` with `Unknown | EnvVariable[Literal[2]]`
+ ddtrace/appsec/_iast/sampling/vulnerability_detection.py:88:8: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[int]`, in comparing `Unknown | int` with `Unknown | EnvVariable[int]`
- ddtrace/appsec/_iast/secure_marks/configuration.py:199:51: error[invalid-argument-type] Argument to function `parse_security_controls_config` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (EnvVariable[Literal[""]] & ~AlwaysFalsy)`
+ ddtrace/appsec/_iast/secure_marks/configuration.py:199:51: error[invalid-argument-type] Argument to function `parse_security_controls_config` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (EnvVariable[str] & ~AlwaysFalsy)`
- ddtrace/appsec/_iast/taint_sinks/_base.py:69:20: error[unsupported-operator] Operator `<` is not supported for types `int` and `EnvVariable[Literal[2]]`, in comparing `int` with `Unknown | EnvVariable[Literal[2]]`
+ ddtrace/appsec/_iast/taint_sinks/_base.py:69:20: error[unsupported-operator] Operator `<` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `Unknown | EnvVariable[int]`
- ddtrace/appsec/_processor.py:106:49: warning[possibly-missing-attribute] Attribute `encode` may be missing on object of type `Unknown | EnvVariable[Unknown | Literal["(?i)pass|pw(?:or)?d|secret|(?:api|private|public|access)[_-]?key|token|consumer[_-]?(?:id|key|secret)|sign(?:ed|ature)|bearer|authorization|jsessionid|phpsessid|asp\\.net[_-]sessionid|sid|jwt"]]`
- ddtrace/appsec/_processor.py:107:51: warning[possibly-missing-attribute] Attribute `encode` may be missing on object of type `Unknown | EnvVariable[Unknown | Literal["(?i)(?:p(?:ass)?w(?:or)?d|pass(?:[_-]?phrase)?|secret(?:[_-]?key)?|(?:(?:api|private|public|access)[_-]?)key(?:[_-]?id)?|(?:(?:auth|access|id|refresh)[_-]?)?token|consumer[_-]?(?:id|key|secret)|sign(?:ed|ature)?|auth(?:entication|orization)?|jsessionid|phpsessid|asp\\.net(?:[_-]|-)sessionid|sid|jwt)(?:\\s*=([^;&]+)|\"\\s*:\\s*(\"[^\"]+\"|\\d+))|bearer\\s+([a-z0-9\\._\\-]+)|token\\s*:\\s*([a-z0-9]{13})|gh[opsu]_([0-9a-zA-Z]{36})|ey[I-L][\\w=-]+\\.(ey[I-L][\\w=-]+(?:\\.[\\w.+\\/=-]+)?)|[\\-]{5}BEGIN[a-z\\s]+PRIVATE\\sKEY[\\-]{5}([^\\-]+)[\\-]{5}END[a-z\\s]+PRIVATE\\sKEY|ssh-rsa\\s*([a-z0-9\\/\\.+]{100,})"]]`
+ ddtrace/appsec/_processor.py:106:49: warning[possibly-missing-attribute] Attribute `encode` may be missing on object of type `Unknown | EnvVariable[Unknown | str]`
+ ddtrace/appsec/_processor.py:107:51: warning[possibly-missing-attribute] Attribute `encode` may be missing on object of type `Unknown | EnvVariable[Unknown | str]`
- ddtrace/appsec/ai_guard/_api_client.py:106:25: error[unsupported-operator] Operator `//` is unsupported between objects of type `Unknown | EnvVariable[Literal[10000]]` and `Literal[1000]`
+ ddtrace/appsec/ai_guard/_api_client.py:106:25: error[unsupported-operator] Operator `//` is unsupported between objects of type `Unknown | EnvVariable[int]` and `Literal[1000]`
- ddtrace/appsec/ai_guard/_api_client.py:115:12: error[unsupported-operator] Operator `>` is not supported for types `int` and `EnvVariable[Literal[16]]`, in comparing `int` with `Unknown | EnvVariable[Literal[16]]`
+ ddtrace/appsec/ai_guard/_api_client.py:115:12: error[unsupported-operator] Operator `>` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `Unknown | EnvVariable[int]`
- ddtrace/appsec/ai_guard/_api_client.py:119:29: error[unsupported-operator] Unary operator `-` is unsupported for type `Unknown | EnvVariable[Literal[16]]`
+ ddtrace/appsec/ai_guard/_api_client.py:119:29: error[unsupported-operator] Unary operator `-` is unsupported for type `Unknown | EnvVariable[int]`
- ddtrace/appsec/ai_guard/_api_client.py:127:16: error[unsupported-operator] Operator `>` is not supported for types `int` and `EnvVariable[Literal[524288]]`, in comparing `int` with `Unknown | EnvVariable[Literal[524288]]`
+ ddtrace/appsec/ai_guard/_api_client.py:127:16: error[unsupported-operator] Operator `>` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `Unknown | EnvVariable[int]`
- ddtrace/contrib/internal/django/user.py:50:53: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `Unknown | (EnvVariable[Literal[""]] & ~AlwaysFalsy) | str`
+ ddtrace/contrib/internal/django/user.py:50:53: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `Unknown | (EnvVariable[str] & ~AlwaysFalsy) | str`
- ddtrace/debugging/_exception/replay.py:332:35: error[unsupported-operator] Operator `<` is not supported for types `int` and `EnvVariable[Literal[8]]`, in comparing `int` with `Unknown | EnvVariable[Literal[8]]`
+ ddtrace/debugging/_exception/replay.py:332:35: error[unsupported-operator] Operator `<` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `Unknown | EnvVariable[int]`
- ddtrace/debugging/_exception/replay.py:343:33: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[Literal[8]]`, in comparing `int` with `Unknown | EnvVariable[Literal[8]]`
+ ddtrace/debugging/_exception/replay.py:343:33: error[unsupported-operator] Operator `>=` is not supported for types `int` and `EnvVariable[int]`, in comparing `int` with `Unknown | EnvVariable[int]`
- ddtrace/debugging/_probe/remoteconfig.py:302:13: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | float` and `Unknown | EnvVariable[Literal[3600]]`
+ ddtrace/debugging/_probe/remoteconfig.py:302:13: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | float` and `Unknown | EnvVariable[int]`
- ddtrace/internal/core/crashtracking.py:111:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | EnvVariable[Literal[True]]`
+ ddtrace/internal/core/crashtracking.py:111:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | EnvVariable[bool]`
- ddtrace/internal/core/crashtracking.py:112:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | EnvVariable[Literal[True]]`
+ ddtrace/internal/core/crashtracking.py:112:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | EnvVariable[bool]`
- ddtrace/internal/gitmetadata.py:52:35: error[invalid-argument-type] Argument to function `parse_tags_str` is incorrect: Expected `str | None`, found `Unknown | EnvVariable[Literal[""]]`
+ ddtrace/internal/gitmetadata.py:52:35: error[invalid-argument-type] Argument to function `parse_tags_str` is incorrect: Expected `str | None`, found `Unknown | EnvVariable[str]`
- ddtrace/internal/openfeature/writer.py:99:13: error[invalid-assignment] Object of type `Unknown | EnvVariable[Literal[True]]` is not assignable to `bool | None`
+ ddtrace/internal/openfeature/writer.py:99:13: error[invalid-assignment] Object of type `Unknown | EnvVariable[bool]` is not assignable to `bool | None`
- ddtrace/internal/telemetry/writer.py:68:45: error[invalid-argument-type] Argument to bound method `get_host` is incorrect: Expected `str`, found `Unknown | EnvVariable[Literal["datadoghq.com"]]`
+ ddtrace/internal/telemetry/writer.py:68:45: error[invalid-argument-type] Argument to bound method `get_host` is incorrect: Expected `str`, found `Unknown | EnvVariable[str]`
- ddtrace/internal/telemetry/writer.py:181:13: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | (EnvVariable[Literal[False]] & ~AlwaysFalsy) | bool` is not assignable to `bool | None`
+ ddtrace/internal/telemetry/writer.py:181:13: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | (EnvVariable[bool] & ~AlwaysFalsy) | bool` is not assignable to `bool | None`
- ddtrace/internal/telemetry/writer.py:350:46: error[invalid-argument-type] Argument to bound method `flush` is incorrect: Expected `int`, found `Unknown | EnvVariable[Unknown | Literal[300]]`
+ ddtrace/internal/telemetry/writer.py:350:46: error[invalid-argument-type] Argument to bound method `flush` is incorrect: Expected `int`, found `Unknown | EnvVariable[Unknown | int]`
- ddtrace/profiling/collector/_lock.py:243:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[Literal[64]]` is not assignable to annotated parameter type `int`
+ ddtrace/profiling/collector/_lock.py:243:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[int]` is not assignable to annotated parameter type `int`
- ddtrace/profiling/collector/_lock.py:244:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[Literal[True]]` is not assignable to annotated parameter type `bool`
+ ddtrace/profiling/collector/_lock.py:244:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[bool]` is not assignable to annotated parameter type `bool`
- ddtrace/profiling/profiler.py:130:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[Literal[True]]` is not assignable to annotated parameter type `bool`
+ ddtrace/profiling/profiler.py:130:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[bool]` is not assignable to annotated parameter type `bool`
- ddtrace/profiling/profiler.py:131:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[Literal[True]]` is not assignable to annotated parameter type `bool`
+ ddtrace/profiling/profiler.py:131:9: error[invalid-parameter-default] Default value of type `Unknown | EnvVariable[bool]` is not assignable to annotated parameter type `bool`
- ddtrace/profiling/profiler.py:182:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `int | None`, found `Unknown | EnvVariable[Literal[64]]`
+ ddtrace/profiling/profiler.py:182:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `int | None`, found `Unknown | EnvVariable[int]`
- ddtrace/profiling/profiler.py:183:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `bool | None`, found `Unknown | EnvVariable[Literal[True]]`
+ ddtrace/profiling/profiler.py:183:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `bool | None`, found `Unknown | EnvVariable[bool]`
- ddtrace/profiling/profiler.py:185:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `int | None`, found `Unknown | EnvVariable[Literal[4]]`
+ ddtrace/profiling/profiler.py:185:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `int | None`, found `Unknown | EnvVariable[int]`
- ddtrace/profiling/profiler.py:186:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `int | None`, found `Unknown | EnvVariable[Literal[10000]]`
+ ddtrace/profiling/profiler.py:186:13: error[invalid-argument-type] Argument to function `config` is incorrect: Expected `int | None`, found `Unknown | EnvVariable[int]`
- ddtrace/settings/_opentelemetry.py:10:61: error[invalid-argument-type] Argument to function `_get_default_endpoint` is incorrect: Expected `str`, found `Unknown | EnvVariable[Literal["grpc"]]`
+ ddtrace/settings/_opentelemetry.py:10:61: error[invalid-argument-type] Argument to function `_get_default_endpoint` is incorrect: Expected `str`, found `Unknown | EnvVariable[str]`
- ddtrace/settings/asm.py:195:5: error[invalid-assignment] Object of type `EnvVariable[Literal[1]]` is not assignable to `int`
+ ddtrace/settings/asm.py:195:5: error[invalid-assignment] Object of type `EnvVariable[int]` is not assignable to `int`
- ddtrace/settings/asm.py:318:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | Literal[False] | (EnvVariable[Literal[False]] & ~AlwaysTruthy) | EnvVariable[Literal[True]]`
+ ddtrace/settings/asm.py:318:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | Literal[False] | EnvVariable[bool]`
- ddtrace/settings/asm.py:322:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `(Unknown & ~AlwaysFalsy & ~AlwaysTruthy) | (EnvVariable[Literal[False]] & ~AlwaysFalsy & ~AlwaysTruthy) | bool`
+ ddtrace/settings/asm.py:322:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `(Unknown & ~AlwaysFalsy & ~AlwaysTruthy) | (EnvVariable[bool] & ~AlwaysFalsy & ~AlwaysTruthy) | bool`
- ddtrace/settings/asm.py:331:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | EnvVariable[Literal["identification"]] | Literal["disabled"]`
+ ddtrace/settings/asm.py:331:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Unknown | EnvVariable[str] | Literal["disabled"]`
- ddtrace/settings/profiling.py:170:9: error[invalid-argument-type] Argument to bound method `v` is incorrect: Expected `((str, /) -> Literal[False]) | None`, found `def _parse_profiling_enabled(raw: str) -> bool`
- ddtrace/settings/profiling.py:254:9: error[invalid-argument-type] Argument to bound method `v` is incorrect: Expected `((str, /) -> Literal[10000]) | None`, found `def _parse_api_timeout_ms(raw: str) -> int`
- ddtrace/settings/profiling.py:311:9: error[invalid-argument-type] Argument to bound method `v` is incorrect: Expected `((str, /) -> Literal[True]) | None`, found `def _parse_v2_enabled(raw: str) -> bool`
- tests/appsec/ai_guard/api/test_api_client.py:195:20: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | EnvVariable[Literal[16]]` and `Literal[1]`
+ tests/appsec/ai_guard/api/test_api_client.py:195:20: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | EnvVariable[int]` and `Literal[1]`
- tests/appsec/ai_guard/api/test_api_client.py:210:35: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | EnvVariable[Literal[524288]]` and `Literal[1]`
+ tests/appsec/ai_guard/api/test_api_client.py:210:35: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | EnvVariable[int]` and `Literal[1]`
- tests/appsec/iast/test_overhead_control_engine.py:73:48: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | EnvVariable[Literal[2]]` and `Literal[1]`
+ tests/appsec/iast/test_overhead_control_engine.py:73:48: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | EnvVariable[int]` and `Literal[1]`
- tests/debugging/exception/test_replay.py:359:17: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | EnvVariable[Literal[8]]` and `Literal[2]`
+ tests/debugging/exception/test_replay.py:359:17: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | EnvVariable[int]` and `Literal[2]`
- Found 8211 diagnostics
+ Found 8208 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/scalars/test_scalars.py:160:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/scalars/test_scalars.py:176:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/scalars/test_scalars.py:208:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/scalars/test_scalars.py:225:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/scalars/test_scalars.py:250:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/scalars/test_scalars.py:251:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/test_interval.py:41:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/test_interval.py:42:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/test_interval.py:43:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/test_interval.py:45:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/test_interval.py:101:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- tests/test_interval.py:103:11: error[type-assertion-failure] Argument does not have asserted type `Interval[int]`
- Found 6007 diagnostics
+ Found 5995 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/actions/custom_profile_fields.py:126:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `editable_by_user` on type `CustomProfileField` with custom `__set__` method
+ zerver/actions/user_groups.py:587:5: error[invalid-assignment] Invalid assignment to data descriptor attribute `deactivated` on type `NamedUserGroup` with custom `__set__` method
+ zerver/tests/test_message_send.py:2455:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `is_recently_active` on type `Stream` with custom `__set__` method
- zerver/tests/test_users.py:256:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_owner"]` on object of type `dict[Literal["email"], Unknown]`
- zerver/tests/test_users.py:258:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_owner"]` on object of type `dict[Literal["email"], Unknown]`
- zerver/tests/test_users.py:315:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_admin"]` on object of type `dict[Literal["email"], Unknown]`
- zerver/tests/test_users.py:317:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_admin"]` on object of type `dict[Literal["email"], Unknown]`
- zerver/tests/test_users.py:358:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["email"]` on object of type `dict[Literal["user_id"], Unknown]`
- zerver/tests/test_users.py:359:27: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["avatar_url"]` on object of type `dict[Literal["user_id"], Unknown]`
- zerver/tests/test_users.py:360:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["delivery_email"]` on object of type `dict[Literal["user_id"], Unknown]`
- zerver/tests/test_users.py:384:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["email"]` on object of type `dict[Literal["user_id"], Unknown]`
- zerver/tests/test_users.py:386:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["avatar_url"]` on object of type `dict[Literal["user_id"], Unknown]`
- zerver/tests/test_users.py:399:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["email"]` on object of type `dict[Literal["user_id"], Unknown]`
- zerver/tests/test_users.py:401:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["avatar_url"]` on object of type `dict[Literal["user_id"], Unknown]`
- zerver/tests/test_users.py:403:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["delivery_email"]` on object of type `dict[Literal["user_id"], Unknown]`
- zproject/backends.py:176:21: error[invalid-argument-type] Argument to function `pad_method_dict` is incorrect: Expected `dict[str, bool]`, found `dict[str, bool] | dict[str, Literal[True]]`
- Found 3318 diagnostics
+ Found 3308 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/ethereum/modules/yearn/decoder.py:404:13: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[ChecksumAddress, Literal["yearn-v1", "yearn-v2"]]`
- rotkehlchen/db/dbhandler.py:2576:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 2102 diagnostics
+ Found 2100 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/ukraine_alarm/coordinator.py:57:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Unknown | str, Literal[False]].__setitem__(key: Unknown | str, value: Literal[False], /) -> None` cannot be called with a key of type `Unknown` and a value of type `Literal[True]` on object of type `dict[Unknown | str, Literal[False]]`
- Found 14476 diagnostics
+ Found 14475 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-11-10 20:46_

The primer report looks excellent but it looks like there's a new type-assertion failure in the conformance test suite -- have you checked out if that's expected?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/classes.md`:319 on 2025-11-10 20:49_

this doesn't seem to be used?

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/pep695/classes.md`:334 on 2025-11-10 20:49_

same here

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:518 on 2025-11-10 20:50_

wonderful!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:2774 on 2025-11-10 20:52_

What does `is_inherited` mean here? Why does it matter whether a typevar is inherited or not?

(It would be great if `is_inherited` could have a doc-comment explaining what it does 😃)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:2774 on 2025-11-10 20:55_

Ah, I think I see -- you're using the term to mean that the typevar is associated with a class's generic context rather than the generic context of a function or method? So it doesn't have any thing to do with the generic context of a class's base classes etc.?

---

_Comment by @sharkdp on 2025-11-10 20:55_

> The primer report looks excellent but it looks like there's a new type-assertion failure in the conformance test suite -- have you checked out if that's expected?

Note that both the primer and the conformance checks here include changes from the previous PRs on which this is stacked. We compare against main.

---

_@AlexWaygood reviewed on 2025-11-10 20:55_

---

_@ibraheemdev reviewed on 2025-11-10 22:54_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/call/bind.rs`:2774 on 2025-11-10 22:54_

That's right, [though I didn't invent that terminology](https://github.com/astral-sh/ruff/blob/47cdf494d5b6317a5d111ba1fdf3d3b820619f22/crates/ty_python_semantic/src/types/class.rs#L1476).

Ideally we wouldn't need to special case inherited type variables, but we don't currently have a robust way to get access to the bound type of a constructor method (the [`synthesized_constructor_return_ty`](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types.rs#L1115C19-L1115C52)).

---

_Comment by @ibraheemdev on 2025-11-10 23:32_

Hmm this is more tricky than I realized. We can only perform the literal promotion of a given type variable if it does not lead to argument assignability errors. We may need to resort to a simple heuristic of "promote every type variable in invariant position", and fallback to not performing any promotion if type-checking fails, as I suspect attempting partial promotion would be too expensive.

---

_Closed by @ibraheemdev on 2025-11-13 21:14_

---

_Comment by @ibraheemdev on 2025-11-13 21:36_

Superseded by https://github.com/astral-sh/ruff/pull/21439.

---
