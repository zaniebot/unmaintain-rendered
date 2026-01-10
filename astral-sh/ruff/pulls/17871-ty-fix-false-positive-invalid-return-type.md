```yaml
number: 17871
title: "[ty] Fix false-positive `[invalid-return-type]` diagnostics on generator functions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/generator-function-return
created_at: 2025-05-05T20:37:15Z
updated_at: 2025-05-05T21:45:01Z
url: https://github.com/astral-sh/ruff/pull/17871
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Fix false-positive `[invalid-return-type]` diagnostics on generator functions

---

_Pull request opened by @AlexWaygood on 2025-05-05 20:37_

## Summary

A generator function implicitly returns an instance of `types.GeneratorType` even if it does not contain any `return` statements. Our `[invalid-return-type]` lint does not understand this currently, leading to false positives on generator functions: https://types.ruff.rs/3b0fcf8b-d607-43a7-8f37-d6df4b62122d. This PR fixes those false positives.

## Test Plan

new mdtests and snapshots added


---

_Label `ty` added by @AlexWaygood on 2025-05-05 20:37_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-05 20:37_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-05 20:37_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-05 20:37_

---

_Comment by @github-actions[bot] on 2025-05-05 20:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[lint:invalid-return-type] pytest_robotframework/_internal/pytest/plugin.py:433:47: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] pytest_robotframework/_internal/pytest/plugin.py:581:41: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] pytest_robotframework/_internal/pytest/plugin.py:612:40: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] pytest_robotframework/_internal/pytest/plugin.py:621:44: Function can implicitly return `None`, which is not assignable to return type `Generator`
- Found 353 diagnostics
+ Found 349 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- error[lint:invalid-return-type] paroxython/label_programs.py:55:54: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- Found 21 diagnostics
+ Found 20 diagnostics

beartype (https://github.com/beartype/beartype)
- error[lint:invalid-return-type] beartype/_util/api/standard/utiltyping.py:389:6: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- Found 577 diagnostics
+ Found 576 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- error[lint:invalid-return-type] aioredis/client.py:2437:10: Function can implicitly return `None`, which is not assignable to return type `AsyncIterator`
- error[lint:invalid-return-type] aioredis/client.py:2484:10: Function can implicitly return `None`, which is not assignable to return type `AsyncIterator`
- error[lint:invalid-return-type] aioredis/client.py:2525:10: Function can implicitly return `None`, which is not assignable to return type `AsyncIterator`
- error[lint:invalid-return-type] aioredis/client.py:2574:10: Function can implicitly return `None`, which is not assignable to return type `AsyncIterator`
- error[lint:invalid-return-type] aioredis/client.py:4176:31: Function can implicitly return `None`, which is not assignable to return type `AsyncIterator`
- Found 34 diagnostics
+ Found 29 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[lint:invalid-return-type] strawberry/extensions/context.py:120:37: Function can implicitly return `None`, which is not assignable to return type `AsyncIterator`
- Found 556 diagnostics
+ Found 555 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- error[lint:invalid-return-type] sockeye/test_utils.py:156:59: Function can implicitly return `None`, which is not assignable to return type `dict[Unknown, Unknown]`
+ error[lint:invalid-return-type] sockeye/test_utils.py:156:59: Return type does not match returned value: Expected `dict[Unknown, Unknown]`, found `types.GeneratorType`
- error[lint:invalid-return-type] sockeye/utils.py:498:13: Return type does not match returned value: Expected `Iterable`, found `None`
- Found 395 diagnostics
+ Found 394 diagnostics

pip (https://github.com/pypa/pip)
- error[lint:invalid-return-type] src/pip/_internal/index/sources.py:129:29: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] src/pip/_internal/index/sources.py:161:29: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] src/pip/_internal/index/sources.py:163:13: Return type does not match returned value: Expected `Iterable`, found `None`
- error[lint:invalid-return-type] src/pip/_internal/index/sources.py:195:29: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] src/pip/_internal/req/req_file.py:498:45: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] src/pip/_internal/req/req_file.py:529:50: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] src/pip/_internal/req/req_file.py:540:55: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] src/pip/_vendor/rich/console.py:314:10: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] src/pip/_vendor/rich/repr.py:127:36: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- Found 1100 diagnostics
+ Found 1091 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[lint:invalid-return-type] pydantic/deprecated/copy_internals.py:38:6: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] pydantic/deprecated/copy_internals.py:55:9: Return type does not match returned value: Expected `Generator`, found `None`
- Found 910 diagnostics
+ Found 908 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[lint:invalid-return-type] ignite/engine/deterministic.py:78:27: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] ignite/engine/engine.py:1045:48: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] ignite/engine/engine.py:1350:39: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- Found 2569 diagnostics
+ Found 2566 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[lint:invalid-return-type] mitmproxy/eventsequence.py:16:40: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- error[lint:invalid-return-type] mitmproxy/eventsequence.py:35:37: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- error[lint:invalid-return-type] mitmproxy/eventsequence.py:48:37: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- error[lint:invalid-return-type] mitmproxy/eventsequence.py:61:37: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- error[lint:invalid-return-type] test/mitmproxy/tools/console/conftest.py:37:35: Function can implicitly return `None`, which is not assignable to return type `ConsoleTestMaster`
+ error[lint:invalid-return-type] test/mitmproxy/tools/console/conftest.py:37:35: Return type does not match returned value: Expected `ConsoleTestMaster`, found `types.AsyncGeneratorType`
- Found 2201 diagnostics
+ Found 2197 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[lint:invalid-return-type] mesonbuild/cargo/cfg.py:53:24: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- Found 1696 diagnostics
+ Found 1695 diagnostics

paasta (https://github.com/yelp/paasta)
- error[lint:invalid-return-type] paasta_tools/cleanup_kubernetes_jobs.py:80:67: Function can implicitly return `None`, which is not assignable to return type `Generator`
- error[lint:invalid-return-type] paasta_tools/setup_istio_mesh.py:135:75: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- error[lint:invalid-return-type] paasta_tools/setup_istio_mesh.py:211:6: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- error[lint:invalid-return-type] paasta_tools/setup_istio_mesh.py:260:6: Function can implicitly return `None`, which is not assignable to return type `Iterator`
- Found 1070 diagnostics
+ Found 1066 diagnostics

rich (https://github.com/Textualize/rich)
+ error[lint:invalid-return-type] examples/table_movie.py:63:30: Return type does not match returned value: Expected `None`, found `types.GeneratorType`
- error[lint:invalid-return-type] rich/console.py:314:10: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- error[lint:invalid-return-type] rich/repr.py:127:36: Function can implicitly return `None`, which is not assignable to return type `Iterable`
- Found 503 diagnostics
+ Found 502 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[lint:invalid-return-type] openlibrary/plugins/worksearch/code.py:101:6: Function can implicitly return `None`, which is not assignable to return type `tuple[str, str, int]`
- error[lint:invalid-return-type] openlibrary/plugins/worksearch/code.py:121:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, tuple[str, str, int]]`
+ error[lint:invalid-return-type] openlibrary/plugins/worksearch/code.py:101:6: Return type does not match returned value: Expected `tuple[str, str, int]`, found `types.GeneratorType`
+ error[lint:invalid-return-type] openlibrary/plugins/worksearch/code.py:121:6: Return type does not match returned value: Expected `dict[str, tuple[str, str, int]]`, found `types.GeneratorType`

bokeh (https://github.com/bokeh/bokeh)
- error[lint:invalid-return-type] src/bokeh/application/handlers/code.py:194:65: Function can implicitly return `None`, which is not assignable to return type `dict[str, Any]`
+ error[lint:invalid-return-type] src/bokeh/application/handlers/code.py:194:65: Return type does not match returned value: Expected `dict[str, Any]`, found `types.GeneratorType`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:invalid-return-type] benchmarks/bm/iast_fixtures/str_methods.py:928:20: Function can implicitly return `None`, which is not assignable to return type `Generator`
+ error[lint:invalid-return-type] ddtrace/contrib/internal/pytest/_plugin_v2.py:420:48: Return type does not match returned value: Expected `None`, found `types.GeneratorType`
+ error[lint:invalid-return-type] ddtrace/contrib/internal/pytest/_plugin_v2.py:560:76: Return type does not match returned value: Expected `None`, found `types.GeneratorType`
- Found 8773 diagnostics
+ Found 8774 diagnostics

jax (https://github.com/google/jax)
- error[lint:invalid-return-type] jax/_src/pallas/mosaic_gpu/lowering.py:328:8: Function can implicitly return `None`, which is not assignable to return type `BarrierRef | DialectBarrierRef | CollectiveBarrierRef`
+ error[lint:invalid-return-type] jax/_src/pallas/mosaic_gpu/lowering.py:328:8: Return type does not match returned value: Expected `BarrierRef | DialectBarrierRef | CollectiveBarrierRef`, found `types.GeneratorType`

materialize (https://github.com/MaterializeInc/materialize)
- error[lint:invalid-return-type] misc/python/materialize/mzexplore/sql.py:69:40: Function can implicitly return `None`, which is not assignable to return type `Generator`
- Found 4438 diagnostics
+ Found 4437 diagnostics

```
</details>


---

_Converted to draft by @AlexWaygood on 2025-05-05 20:50_

---

_Marked ready for review by @AlexWaygood on 2025-05-05 21:01_

---

_Closed by @AlexWaygood on 2025-05-05 21:13_

---

_Reopened by @AlexWaygood on 2025-05-05 21:13_

---

_Comment by @AlexWaygood on 2025-05-05 21:19_

The primer report is nearly all false positives going away, but there are a few new diagnostics being introduced here.

> ```diff
> + error[lint:invalid-return-type] sockeye/test_utils.py:156:59: Return type does not match returned value: Expected `dict[Unknown, Unknown]`, found `types.GeneratorType`
> ```

This is a true positive. It's annotated as returning `Dict[str, Any]`, but the correct annotation would be `Iterator[Dict[str, Any]]` or similar. https://github.com/awslabs/sockeye/blob/871d9863d6a657d87de1c90f44977c5b70c99c7e/sockeye/test_utils.py#L156

> ```diff
> + error[lint:invalid-return-type] test/mitmproxy/tools/console/conftest.py:37:35: Return type does not match returned value: Expected `ConsoleTestMaster`, found `types.AsyncGeneratorType`
> ```

Similarly here, it's [annotated as returning `ConsoleTestMaster`](https://github.com/mitmproxy/mitmproxy/blob/eea4addf002c44adb9a60302b58f4fe570129e4f/test/mitmproxy/tools/console/conftest.py#L37) but it should be annotated as returning `AsyncIterator[ConsoleTestMaster]` or similar.

The other new diagnostics also look similar!

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/return_type.md_-_Function_return_type_-_Generator_functions.snap`:79 on 2025-05-05 21:30_

nit: should be "an async generator function" -- I know this is a bit of a pain to implement :/

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:1653 on 2025-05-05 21:33_

Should we have a TODO here for inferring these as generic, specialized with the actual returned type (in or after your generic protocols PR)?

I think that will require a bit of restructuring of this check, since it means we need to use `self.return_types_and_ranges` and actually construct an inferred return type for each return statement in the function, wrapped with the right kind of generator type.

---

_@carljm approved on 2025-05-05 21:34_

Looks good!

---

_@AlexWaygood reviewed on 2025-05-05 21:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/return_type.md_-_Function_return_type_-_Generator_functions.snap`:79 on 2025-05-05 21:35_

lol

---

_@AlexWaygood reviewed on 2025-05-05 21:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1653 on 2025-05-05 21:36_

yeah, it's going to be icky. But I think it can be easily postponed until after the alpha -- my generic-protocol PR shouldn't break this, I don't think. And it's "just a missing check" rather than something that will cause false positives.

---

_Merged by @AlexWaygood on 2025-05-05 21:45_

---

_Closed by @AlexWaygood on 2025-05-05 21:45_

---

_Branch deleted on 2025-05-05 21:45_

---
