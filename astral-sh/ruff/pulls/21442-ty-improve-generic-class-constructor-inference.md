```yaml
number: 21442
title: "[ty] Improve generic class constructor inference"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/generic-constructor-inference
created_at: 2025-11-14T02:04:07Z
updated_at: 2025-11-14T20:25:48Z
url: https://github.com/astral-sh/ruff/pull/21442
synced_at: 2026-01-12T15:57:24Z
```

# [ty] Improve generic class constructor inference

---

_@ibraheemdev_

## Summary

We currently fail to account for the type context when inferring generic classes constructed with `__new__`, or synthesized `__init__` for dataclasses.

---

_Review requested from @carljm by @ibraheemdev on 2025-11-14 02:04_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-14 02:04_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-14 02:04_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-14 02:04_

---

_Label `ty` added by @ibraheemdev on 2025-11-14 02:04_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 02:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 02:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_base.py:200:9: error[invalid-assignment] Object of type `ReferenceType[Self@inverse]` is not assignable to attribute `_invweak` of type `ReferenceType[BidictBase[KT@BidictBase, VT@BidictBase]] | None`
- Found 15 diagnostics
+ Found 14 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/stack_sampler.py:49:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[object]`
- Found 42 diagnostics
+ Found 41 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- dedupe/predicates.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
- dedupe/predicates.py:118:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
- Found 52 diagnostics
+ Found 50 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/specs/openapi/schemas.py:430:9: error[invalid-assignment] Object of type `APIOperation[Unknown, OpenApiResponses, OpenApiSecurityParameters]` is not assignable to `APIOperation[OperationParameter, ResponsesContainer[Unknown], OpenApiSecurityParameters]`
- Found 268 diagnostics
+ Found 267 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_catalog.py:214:65: error[invalid-argument-type] Argument is incorrect: Expected `ReferenceType[CatalogImpl]`, found `ReferenceType[Self@__init__]`
- Found 315 diagnostics
+ Found 314 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/persistence/models.py:29:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[str | None]`
- Found 615 diagnostics
+ Found 614 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/_csot.py:30:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[int | float | None]`
- pymongo/_csot.py:31:1: error[invalid-assignment] Object of type `ContextVar[float]` is not assignable to `ContextVar[int | float]`
- pymongo/_csot.py:32:1: error[invalid-assignment] Object of type `ContextVar[float]` is not assignable to `ContextVar[int | float]`
- Found 475 diagnostics
+ Found 472 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_run.py:1452:21: error[invalid-assignment] Object of type `_TaskStatus[None]` is not assignable to `_TaskStatus[object]`
- Found 524 diagnostics
+ Found 523 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cmake/executor.py:29:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to `PerMachine[ExternalProgram | None]`
- mesonbuild/cmake/executor.py:30:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to `PerMachine[str | None]`
+ mesonbuild/cmake/interpreter.py:1004:70: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `str | int | Path | BaseNode`
- mesonbuild/dependencies/cmake.py:35:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to `PerMachine[CMakeInfo | None]`
- mesonbuild/dependencies/pkgconfig.py:36:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to `PerMachine[ExternalProgram | None]`
- mesonbuild/modules/rust.py:90:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to `PerMachine[ExternalProgram | None]`
+ mesonbuild/mparser.py:702:65: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `None`
- mesonbuild/utils/universal.py:589:16: error[invalid-return-type] Return type does not match returned value: expected `PerMachine[_T@PerMachineDefaultable]`, found `PerMachine[_T@PerMachineDefaultable & ~None]`
- mesonbuild/utils/universal.py:623:16: error[invalid-return-type] Return type does not match returned value: expected `PerThreeMachine[_T@PerThreeMachineDefaultable]`, found `PerThreeMachine[_T@PerThreeMachineDefaultable & ~None]`
- run_project_tests.py:557:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to attribute `class_cmakeinfo` of type `PerMachine[CMakeInfo | None]`
- run_project_tests.py:560:5: error[invalid-assignment] Object of type `PerMachine[None]` is not assignable to attribute `pkg_bin_per_machine` of type `PerMachine[ExternalProgram | None]`
- Found 1734 diagnostics
+ Found 1727 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/context.py:762:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[UUID | None]`
- src/prefect/context.py:763:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[dict[str, Any] | None]`
- src/prefect/server/database/configurations.py:40:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[str | None]`
- Found 3237 diagnostics
+ Found 3234 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/typing_extensions.py:3546:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `object`
+ setuptools/_vendor/typing_extensions.py:3546:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `object`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/upstream/mybooks.py:468:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `SupportsKeysAndGetItem[Literal["want-to-read", "already-read", "currently-reading"], Literal["Want to Read", "Already Read", "Currently Reading"]]`, found `dict[Unknown | str, Unknown | str]`
- Found 942 diagnostics
+ Found 943 diagnostics

altair (https://github.com/vega/altair)
- altair/utils/core.py:219:1: error[invalid-assignment] Object of type `frozenset[str]` is not assignable to `frozenset[Literal["field", "aggregate", "type", "timeUnit"]]`
- Found 991 diagnostics
+ Found 990 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/_trace/provider.py:17:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[Span | Context | None]`
- ddtrace/appsec/_iast/_iast_request_context_base.py:22:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[int | None]`
- ddtrace/internal/ci_visibility/context.py:12:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[Context | Span | None]`
- ddtrace/llmobs/_context.py:14:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[Context | Span | None]`
- Found 7978 diagnostics
+ Found 7974 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/plotting/contour.py:249:9: error[invalid-argument-type] Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `GlyphRenderer[MultiPolygons]`
- src/bokeh/plotting/contour.py:250:9: error[invalid-argument-type] Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `GlyphRenderer[MultiLine]`
- src/bokeh/server/contexts.py:233:13: error[invalid-assignment] Object of type `ReferenceType[BokehSessionContext]` is not assignable to attribute `_session_context` of type `ReferenceType[SessionContext] | None`
- Found 637 diagnostics
+ Found 634 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_natype.py:80:11: error[type-assertion-failure] Argument does not have asserted type `Index[int]`
- tests/test_natype.py:160:11: error[type-assertion-failure] Argument does not have asserted type `Index[int]`
- tests/test_pandas.py:1036:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_pandas.py:1037:11: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- tests/test_pandas.py:1040:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_pandas.py:1041:11: error[type-assertion-failure] Argument does not have asserted type `@Todo`
- Found 5968 diagnostics
+ Found 5966 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/lib/typed_endpoint.py:271:12: error[invalid-return-type] Return type does not match returned value: expected `FuncParam[T@parse_single_parameter]`, found `FuncParam[Any & ~<class '_empty'>]`
- Found 3283 diagnostics
+ Found 3282 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_generics.py:96:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[WeakValueDictionary[tuple[Any, Any, tuple[Any, ...]], type[BaseModel]] | None]`
- pydantic/_internal/_generics.py:395:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[set[str] | None]`
- Found 4816 diagnostics
+ Found 4814 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/conversation/chat_log.py:29:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[ChatLog | None]`
- homeassistant/components/conversation/trace.py:78:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[ConversationTrace | None]`
- homeassistant/config_entries.py:1344:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[ConfigEntry[Any] | None]`
- homeassistant/core.py:1683:9: error[invalid-assignment] Object of type `_OneTimeListener[Mapping[str, Any]]` is not assignable to `_OneTimeListener[_DataT@async_listen_once]`
- homeassistant/helpers/chat_session.py:33:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[ChatSession | None]`
- homeassistant/helpers/entity_platform.py:1248:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[EntityPlatform | None]`
- homeassistant/helpers/script.py:159:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[list[str] | None]`
- homeassistant/helpers/template/context.py:13:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[tuple[str, str] | None]`
- homeassistant/helpers/template/render_info.py:22:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[RenderInfo | None]`
- homeassistant/helpers/trace.py:102:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[dict[str, deque[TraceElement]] | None]`
- homeassistant/helpers/trace.py:106:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[list[TraceElement] | None]`
- homeassistant/helpers/trace.py:110:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[list[str] | None]`
- homeassistant/helpers/trace.py:116:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[tuple[str, str] | None]`
- homeassistant/helpers/trace.py:120:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[StopReason | None]`
- homeassistant/setup.py:38:1: error[invalid-assignment] Object of type `ContextVar[None]` is not assignable to `ContextVar[tuple[str, str | None] | None]`
- Found 14489 diagnostics
+ Found 14474 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-14 02:12_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 02:21_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 41 | 0 |
| `invalid-argument-type` | 3 | 3 | 1 |
| `type-assertion-failure` | 2 | 4 | 0 |
| `invalid-return-type` | 0 | 5 | 0 |
| **Total** | **5** | **53** | **1** |

**[Full report with detailed diff](https://ibraheem-generic-constructor-fox4.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-generic-constructor-fox4.ecosystem-663.pages.dev/timing))




---

_Renamed from "[ty] Improve generic constructor inference" to "[ty] Improve generic class constructor inference" by @ibraheemdev on 2025-11-14 02:30_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6329 on 2025-11-14 10:29_

Am I missing something or is this is the same as `init_ty` above?

---

_@sharkdp approved on 2025-11-14 10:30_

This is great, thank you!

---

_Merged by @ibraheemdev on 2025-11-14 20:25_

---

_Closed by @ibraheemdev on 2025-11-14 20:25_

---

_Branch deleted on 2025-11-14 20:25_

---
