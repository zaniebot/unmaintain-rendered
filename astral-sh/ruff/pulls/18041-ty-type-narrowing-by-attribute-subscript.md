```yaml
number: 18041
title: "[ty] type narrowing by attribute/subscript assignments"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: narrow_by_assignment
created_at: 2025-05-12T12:31:25Z
updated_at: 2025-06-06T03:43:14Z
url: https://github.com/astral-sh/ruff/pull/18041
synced_at: 2026-01-12T15:56:11Z
```

# [ty] type narrowing by attribute/subscript assignments

---

_@mtshiba_

## Summary

This PR partially solves https://github.com/astral-sh/ty/issues/164 (derived from #17643).

Currently, the definitions we manage are limited to those for simple name (symbol) targets, but we expand this to track definitions for attribute and subscript targets as well.

This was originally planned as part of the work in #17643, but the changes are significant, so I made it a separate PR.
After merging this PR, I will reflect this changes in #17643.

There is still some incomplete work remaining, but the basic features have been implemented, so I am publishing it as a draft PR.
Here is the TODO list (there may be more to come):
* [x] Complete rewrite and refactoring of documentation (removing `Symbol` and replacing it with `Place`)
* [x] More thorough testing
* [x] Consolidation of duplicated code (maybe we can consolidate the handling related to name, attribute, and subscript)

This PR replaces the current `Symbol` API with the `Place` API, which is a concept that includes attributes and subscripts (the term is borrowed from Rust).

## Test Plan

`mdtest/narrow/assignment.md` is added.


---

_Label `ty` added by @AlexWaygood on 2025-05-12 14:11_

---

_Comment by @github-actions[bot] on 2025-05-18 13:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:584:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
- warning[possibly-unbound-attribute] beartype/_check/forward/fwdresolve.py:585:5: Attribute `update` on type `BeartypeForwardScope | None` is possibly unbound
- Found 574 diagnostics
+ Found 572 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[unresolved-reference] src/attr/_make.py:1472:13: Name `hash` used when not defined
- warning[possibly-unresolved-reference] src/attr/_make.py:1478:12: Name `hash` used when possibly not defined
- warning[possibly-unresolved-reference] src/attr/_make.py:1478:33: Name `hash` used when possibly not defined
- warning[possibly-unresolved-reference] src/attr/_make.py:1478:55: Name `hash` used when possibly not defined
- warning[possibly-unresolved-reference] src/attr/_make.py:1483:12: Name `hash` used when possibly not defined
- warning[possibly-unresolved-reference] src/attr/_make.py:1483:30: Name `hash` used when possibly not defined
- warning[possibly-unresolved-reference] src/attr/_make.py:1489:14: Name `hash` used when possibly not defined
- warning[possibly-unresolved-reference] src/attr/_make.py:1490:13: Name `hash` used when possibly not defined
- info[revealed-type] tests/dataclass_transform_example.py:34:13: Revealed type: `str`
+ info[revealed-type] tests/dataclass_transform_example.py:34:13: Revealed type: `Literal["new"]`
- info[revealed-type] tests/dataclass_transform_example.py:45:13: Revealed type: `str`
+ info[revealed-type] tests/dataclass_transform_example.py:45:13: Revealed type: `Literal["new"]`
- error[unresolved-attribute] tests/test_slots.py:149:22: Type `C2Slots` has no attribute `t`
- Found 617 diagnostics
+ Found 608 diagnostics

kornia (https://github.com/kornia/kornia)
+ error[unresolved-attribute] kornia/nerf/samplers.py:230:13: Unresolved attribute `_x` on type `Points2D_FlatTensors`.
+ error[unresolved-attribute] kornia/nerf/samplers.py:231:13: Unresolved attribute `_y` on type `Points2D_FlatTensors`.
+ error[unresolved-attribute] kornia/nerf/samplers.py:233:13: Unresolved attribute `_x` on type `Points2D_FlatTensors`.
+ error[unresolved-attribute] kornia/nerf/samplers.py:233:57: Type `Points2D_FlatTensors` has no attribute `_x`
+ error[unresolved-attribute] kornia/nerf/samplers.py:234:13: Unresolved attribute `_y` on type `Points2D_FlatTensors`.
+ error[unresolved-attribute] kornia/nerf/samplers.py:234:57: Type `Points2D_FlatTensors` has no attribute `_y`
+ error[unresolved-attribute] kornia/nerf/samplers.py:259:30: Type `Points2D_FlatTensors` has no attribute `_x`
+ error[unresolved-attribute] kornia/nerf/samplers.py:259:58: Type `Points2D_FlatTensors` has no attribute `_y`
- Found 941 diagnostics
+ Found 949 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- warning[possibly-unbound-attribute] src/aiortc/sdp.py:472:25: Attribute `fingerprints` on type `RTCDtlsParameters | None` is possibly unbound
- error[invalid-assignment] src/aiortc/sdp.py:478:25: Object of type `str | None` is not assignable to attribute `password` on type `RTCIceParameters | None`
- error[invalid-assignment] src/aiortc/sdp.py:480:25: Object of type `str | None` is not assignable to attribute `usernameFragment` on type `RTCIceParameters | None`
- error[invalid-assignment] src/aiortc/sdp.py:496:25: Object of type `Unknown` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
- warning[possibly-unbound-attribute] src/aiortc/sdp.py:538:16: Attribute `role` on type `RTCDtlsParameters | None` is possibly unbound
- Found 133 diagnostics
+ Found 128 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ warning[unused-ignore-comment] tests/language/test_ast.py:233:33: Unused blanket `type: ignore` directive
- error[invalid-assignment] tests/execution/test_union_interface.py:137:1: Object of type `list[Unknown]` is not assignable to attribute `progeny` on type `Cat | None`
- error[invalid-assignment] tests/execution/test_union_interface.py:141:1: Object of type `list[Unknown]` is not assignable to attribute `progeny` on type `Dog | None`
+ warning[unused-ignore-comment] tests/language/test_source.py:80:38: Unused blanket `type: ignore` directive

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-argument-type] strawberry/dataloader.py:217:22: Argument to function `dispatch` is incorrect: Expected `Batch[Unknown, Unknown]`, found `Batch[Unknown, Unknown] | None`
- error[invalid-return-type] strawberry/dataloader.py:219:12: Return type does not match returned value: expected `Batch[Unknown, Unknown]`, found `Batch[Unknown, Unknown] | None`
+ warning[unused-ignore-comment] strawberry/types/object_type.py:170:41: Unused blanket `type: ignore` directive
- Found 432 diagnostics
+ Found 431 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
- error[unresolved-attribute] httpx_caching/_async/_transport.py:127:9: Unresolved attribute `callback` on type `ByteStream`.
- error[unresolved-attribute] httpx_caching/_sync/_transport.py:125:9: Unresolved attribute `callback` on type `ByteStream`.
- Found 30 diagnostics
+ Found 28 diagnostics

trio (https://github.com/python-trio/trio)
+ warning[unused-ignore-comment] src/trio/_core/_tests/test_asyncgen.py:260:52: Unused blanket `type: ignore` directive
- error[invalid-argument-type] src/trio/_repl.py:51:32: Argument is incorrect: Expected `type[BaseException]`, found `type[BaseException] | None`
- error[invalid-argument-type] src/trio/_repl.py:51:47: Argument is incorrect: Expected `BaseException`, found `BaseException | None`
+ error[unresolved-attribute] src/trio/_tests/test_ssl.py:106:8: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] src/trio/_tests/test_ssl.py:108:10: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] src/trio/_tests/test_threads.py:496:58: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:502:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:503:17: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:539:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:545:16: Attribute `high_water` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:554:16: Attribute `ran` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[unresolved-attribute] src/trio/_tests/test_threads.py:555:16: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
+ warning[unused-ignore-comment] src/trio/_tests/test_util.py:267:55: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/test_util.py:268:66: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] src/trio/_tests/test_util.py:269:69: Unused blanket `type: ignore` directive
- error[unresolved-attribute] src/trio/_tests/test_util.py:213:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:214:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:218:12: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:219:12: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:224:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:225:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:229:5: Type `ModuleType` has no attribute `not_funclike`
- error[unresolved-attribute] src/trio/_tests/test_util.py:233:5: Type `ModuleType` has no attribute `only_has_name`
- error[unresolved-attribute] src/trio/_tests/test_util.py:234:5: Type `ModuleType` has no attribute `only_has_name`
- error[unresolved-attribute] src/trio/_tests/test_util.py:238:5: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:239:5: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:239:29: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:254:12: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:255:12: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:256:12: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:258:12: Type `ModuleType` has no attribute `not_funclike`
- error[unresolved-attribute] src/trio/_tests/test_util.py:259:12: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:260:12: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:261:12: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:263:12: Type `ModuleType` has no attribute `only_has_name`
- error[unresolved-attribute] src/trio/_tests/test_util.py:264:12: Type `ModuleType` has no attribute `only_has_name`
- error[unresolved-attribute] src/trio/_tests/test_util.py:265:24: Type `ModuleType` has no attribute `only_has_name`
- error[unresolved-attribute] src/trio/_tests/test_util.py:271:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:272:5: Type `ModuleType` has no attribute `some_func`
- error[unresolved-attribute] src/trio/_tests/test_util.py:273:5: Type `ModuleType` has no attribute `_private`
- error[unresolved-attribute] src/trio/_tests/test_util.py:274:5: Type `ModuleType` has no attribute `SomeClass`
- Found 1093 diagnostics
+ Found 1064 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- error[unresolved-attribute] test/unit/test_inference.py:72:32: Type `Translator` has no attribute `inf_array`
- Found 357 diagnostics
+ Found 356 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[invalid-assignment] tests/ignite/handlers/test_base_logger.py:149:5: Object of type `float` is not assignable to attribute `alpha` on type `Unknown | State`
+ error[unresolved-attribute] tests/ignite/handlers/test_base_logger.py:149:5: Unresolved attribute `alpha` on type `State`.
- error[invalid-assignment] tests/ignite/handlers/test_base_logger.py:150:5: Object of type `Unknown` is not assignable to attribute `beta` on type `Unknown | State`
+ error[unresolved-attribute] tests/ignite/handlers/test_base_logger.py:150:5: Unresolved attribute `beta` on type `State`.
- error[invalid-assignment] tests/ignite/handlers/test_base_logger.py:151:5: Object of type `Unknown` is not assignable to attribute `gamma` on type `Unknown | State`
+ error[unresolved-attribute] tests/ignite/handlers/test_base_logger.py:151:5: Unresolved attribute `gamma` on type `State`.
- error[invalid-assignment] tests/ignite/handlers/test_checkpoint.py:285:5: Object of type `float` is not assignable to attribute `score` on type `Unknown | State`
+ error[unresolved-attribute] tests/ignite/handlers/test_checkpoint.py:285:5: Unresolved attribute `score` on type `State`.
- error[invalid-assignment] tests/ignite/handlers/test_checkpoint.py:362:5: Object of type `int | float` is not assignable to attribute `score` on type `Unknown | State`
+ error[unresolved-attribute] tests/ignite/handlers/test_checkpoint.py:362:5: Unresolved attribute `score` on type `State`.
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:461:17: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:462:24: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:462:48: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:467:21: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:468:28: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:468:54: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:471:24: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:471:24: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:471:68: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:471:68: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:471:89: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:471:89: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:494:17: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:495:24: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:495:48: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:500:21: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:501:28: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:501:54: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:504:24: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:504:24: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:504:68: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:504:68: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:504:89: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_precision.py:504:89: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:463:17: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:464:24: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:464:48: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:469:21: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:470:28: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:470:54: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:473:24: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:473:24: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:473:68: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:473:68: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:473:89: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:473:89: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:497:17: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:498:24: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:498:48: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:503:21: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:504:28: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:504:54: Attribute `device` on type `int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:507:24: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:507:24: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:507:68: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:507:68: Attribute `device` on type `Unknown | int` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:507:89: Attribute `device` on type `int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/metrics/test_recall.py:507:89: Attribute `device` on type `Unknown | int` is possibly unbound
- Found 2253 diagnostics
+ Found 2229 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- warning[possibly-unbound-attribute] tests/test_runserver_watch.py:123:11: Attribute `close` on type `ClientSession | None` is possibly unbound
- error[invalid-assignment] tests/test_runserver_watch.py:158:5: Object of type `MagicMock` is not assignable to attribute `is_alive` on type `Unknown | Process`
- error[invalid-assignment] tests/test_runserver_watch.py:159:5: Object of type `Literal[123]` is not assignable to attribute `exitcode` on type `Unknown | Process`
- error[invalid-assignment] tests/test_runserver_watch.py:171:5: Object of type `MagicMock` is not assignable to attribute `is_alive` on type `Unknown | Process`
- error[invalid-assignment] tests/test_runserver_watch.py:172:5: Object of type `Literal[321]` is not assignable to attribute `pid` on type `Unknown | Process`
- error[invalid-assignment] tests/test_runserver_watch.py:173:5: Object of type `Literal[123]` is not assignable to attribute `exitcode` on type `Unknown | Process`
- Found 65 diagnostics
+ Found 59 diagnostics

stone (https://github.com/dropbox/stone)
- error[unresolved-attribute] test/test_python_gen.py:288:22: Type `S` has no attribute `f`
- error[unresolved-attribute] test/test_python_gen.py:375:22: Type `S` has no attribute `f`
- error[unresolved-attribute] test/test_python_gen.py:376:9: Type `S` has no attribute `f`
+ error[unresolved-attribute] test/test_python_gen.py:376:9: Unresolved attribute `i` on type `S2`.
- error[unresolved-attribute] test/test_python_gen.py:377:9: Type `S` has no attribute `f`
- error[unresolved-attribute] test/test_python_gen.py:377:24: Type `S` has no attribute `f`
- Found 174 diagnostics
+ Found 170 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ error[unresolved-attribute] tests/console/commands/test_init.py:316:31: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] tests/installation/test_installer.py:117:48: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] tests/installation/test_installer.py:118:11: Type `FixtureRequest` has no attribute `param`
- Found 1023 diagnostics
+ Found 1026 diagnostics

mkosi (https://github.com/systemd/mkosi)
- warning[possibly-unresolved-reference] mkosi/config.py:1564:27: Name `type` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1594:35: Name `type` used when possibly not defined
+ error[invalid-argument-type] mkosi/config.py:1564:22: Argument is incorrect: Expected `KeySourceType`, found `KeySourceType | <class 'type'>`
+ error[invalid-argument-type] mkosi/config.py:1594:30: Argument is incorrect: Expected `CertificateSourceType`, found `CertificateSourceType | <class 'type'>`

websockets (https://github.com/aaugustin/websockets)
+ error[unresolved-attribute] src/websockets/asyncio/router.py:89:13: Unresolved attribute `handler` on type `ServerConnection`.
+ error[unresolved-attribute] src/websockets/asyncio/router.py:89:33: Unresolved attribute `handler_kwargs` on type `ServerConnection`.
+ error[unresolved-attribute] src/websockets/asyncio/router.py:94:26: Type `ServerConnection` has no attribute `handler`
+ error[unresolved-attribute] src/websockets/asyncio/router.py:94:59: Type `ServerConnection` has no attribute `handler_kwargs`
+ error[unresolved-attribute] src/websockets/asyncio/server.py:980:9: Unresolved attribute `username` on type `ServerConnection`.
+ error[unresolved-attribute] src/websockets/sync/router.py:89:13: Unresolved attribute `handler` on type `ServerConnection`.
+ error[unresolved-attribute] src/websockets/sync/router.py:89:33: Unresolved attribute `handler_kwargs` on type `ServerConnection`.
+ error[unresolved-attribute] src/websockets/sync/router.py:94:20: Type `ServerConnection` has no attribute `handler`
+ error[unresolved-attribute] src/websockets/sync/router.py:94:53: Type `ServerConnection` has no attribute `handler_kwargs`
+ error[unresolved-attribute] src/websockets/sync/server.py:762:9: Unresolved attribute `username` on type `ServerConnection`.
- Found 110 diagnostics
+ Found 120 diagnostics

yarl (https://github.com/aio-libs/yarl)
+ error[unresolved-attribute] tests/test_quoting.py:18:16: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] tests/test_quoting.py:22:16: Type `FixtureRequest` has no attribute `param`
- warning[unused-ignore-comment] tests/test_quoting.py:32:31: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/test_quoting.py:36:31: Unused blanket `type: ignore` directive

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[unresolved-attribute] tests/test_messagequeue.py:243:25: Type `(...) -> _GeneratorContextManager[Unknown, None, None]` has no attribute `called`
- error[unresolved-attribute] tests/test_messagequeue.py:247:25: Type `(...) -> _GeneratorContextManager[Unknown, None, None]` has no attribute `called`
- warning[possibly-unbound-attribute] tests/test_messagequeue.py:271:25: Attribute `called` on type `Unknown | (bound method ScheduledTask.start() -> Unknown)` is possibly unbound
- error[invalid-assignment] tests/test_psql.py:40:9: Object of type `Literal[0]` is not assignable to attribute `closed` on type `Unknown | None`
- error[invalid-assignment] tests/test_psql.py:50:9: Object of type `Literal[2]` is not assignable to attribute `closed` on type `Unknown | None`
- error[invalid-assignment] tests/test_psql.py:83:9: Object of type `Literal[0]` is not assignable to attribute `closed` on type `Unknown | None`
- Found 169 diagnostics
+ Found 163 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ error[unresolved-attribute] src/urllib3/connectionpool.py:553:13: Type `BaseHTTPResponse` has no attribute `length_remaining`
+ error[unresolved-attribute] test/conftest.py:289:28: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] test/conftest.py:373:8: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] test/conftest.py:383:15: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] test/conftest.py:385:12: Type `FixtureRequest` has no attribute `param`
- Found 506 diagnostics
+ Found 511 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- warning[possibly-unbound-attribute] tornado/test/options_test.py:79:21: Attribute `getvalue` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- Found 327 diagnostics
+ Found 326 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-assignment] examples/contrib/custom_next_layer.py:42:9: Object of type `TCPLayer` is not assignable to attribute `child_layer` on type `Layer | None`
- error[unresolved-attribute] examples/contrib/upstream_pac.py:59:20: Attribute `pac_file` can only be accessed on instances, not on the class object `<class 'UpstreamPac'>` itself.
- warning[possibly-unbound-attribute] mitmproxy/addons/asgiapp.py:126:13: Attribute `decode` on type `Response | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/addons/tlsconfig.py:252:9: Attribute `use_certificate` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/addons/tlsconfig.py:253:9: Attribute `use_privatekey` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/addons/tlsconfig.py:265:9: Attribute `set_app_data` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/addons/tlsconfig.py:272:9: Attribute `set_accept_state` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/addons/tlsconfig.py:362:17: Attribute `set_tlsext_host_name` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/addons/tlsconfig.py:376:13: Attribute `set_alpn_protos` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/addons/tlsconfig.py:378:9: Attribute `set_connect_state` on type `Connection | None` is possibly unbound
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:404:13: Object of type `@Todo(list comprehension type)` is not assignable to attribute `cipher_suites` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:408:9: Object of type `@Todo(list comprehension type)` is not assignable to attribute `alpn_protocols` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:415:9: Object of type `Unknown` is not assignable to attribute `certificate` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:416:9: Object of type `Unknown` is not assignable to attribute `certificate_private_key` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:417:9: Object of type `@Todo(list comprehension type)` is not assignable to attribute `certificate_chain` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:435:13: Object of type `VerifyMode` is not assignable to attribute `verify_mode` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:437:13: Object of type `VerifyMode` is not assignable to attribute `verify_mode` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:454:13: Object of type `@Todo(list comprehension type)` is not assignable to attribute `cipher_suites` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:458:13: Object of type `@Todo(list comprehension type)` is not assignable to attribute `alpn_protocols` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:464:9: Object of type `Unknown` is not assignable to attribute `ca_path` on type `QuicTlsSettings | None`
- error[invalid-assignment] mitmproxy/addons/tlsconfig.py:465:9: Object of type `Unknown` is not assignable to attribute `ca_file` on type `QuicTlsSettings | None`
- warning[possibly-unbound-attribute] mitmproxy/eventsequence.py:28:13: Attribute `messages` on type `WebSocketData | None` is possibly unbound
- error[invalid-assignment] mitmproxy/io/har.py:147:13: Object of type `Literal["HTTP/2"]` is not assignable to attribute `http_version` on type `Response | None`
- error[invalid-assignment] mitmproxy/io/har.py:149:13: Object of type `Literal["HTTP/2"]` is not assignable to attribute `http_version` on type `Response | None`
- error[invalid-assignment] mitmproxy/io/har.py:151:13: Object of type `Literal["HTTP/3"]` is not assignable to attribute `http_version` on type `Response | None`
- error[invalid-assignment] mitmproxy/io/har.py:153:13: Object of type `Literal["HTTP/1.1"]` is not assignable to attribute `http_version` on type `Response | None`
- error[invalid-argument-type] mitmproxy/proxy/layers/dns.py:100:35: Argument to function `pack_message` is incorrect: Expected `DNSMessage`, found `DNSMessage | None`
- warning[possibly-unbound-implicit-call] mitmproxy/proxy/server.py:251:53: Method `__getitem__` of type `tuple[@Todo(Generic tuple specializations), ...] | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/proxy/server.py:572:13: Attribute `set_accept_state` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/proxy/server.py:578:13: Attribute `set_connect_state` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] mitmproxy/proxy/server.py:580:17: Attribute `set_tlsext_host_name` on type `Connection | None` is possibly unbound
- error[invalid-assignment] mitmproxy/test/tflow.py:107:5: Object of type `Unknown | Literal[""]` is not assignable to attribute `close_reason` on type `WebSocketData | None`
- error[invalid-assignment] mitmproxy/test/tflow.py:110:9: Object of type `Unknown & ~None` is not assignable to attribute `close_code` on type `WebSocketData | None`
- error[invalid-assignment] mitmproxy/test/tflow.py:114:13: Object of type `Literal[1006]` is not assignable to attribute `close_code` on type `WebSocketData | None`
- error[invalid-assignment] mitmproxy/test/tflow.py:117:13: Object of type `Literal[1000]` is not assignable to attribute `close_code` on type `WebSocketData | None`
- error[invalid-assignment] test/mitmproxy/addons/test_dumper.py:77:9: Object of type `Literal[300]` is not assignable to attribute `status_code` on type `Response | None`
- warning[possibly-unbound-attribute] test/mitmproxy/addons/test_serverplayback.py:359:36: Attribute `data` on type `Response | None` is possibly unbound
- error[unresolved-attribute] test/mitmproxy/proxy/layers/quic/test__raw_layers.py:35:16: Type `Layer` has no attribute `flow`
- error[unresolved-attribute] test/mitmproxy/proxy/layers/quic/test__raw_layers.py:36:16: Type `Layer` has no attribute `flow`
- error[unresolved-attribute] test/mitmproxy/proxy/layers/quic/test__raw_layers.py:37:16: Type `Layer` has no attribute `flow`
- error[unresolved-attribute] test/mitmproxy/proxy/layers/quic/test__raw_layers.py:38:16: Type `Layer` has no attribute `flow`
- error[invalid-assignment] test/mitmproxy/proxy/layers/quic/test__stream_layers.py:507:13: Object of type `list[Unknown]` is not assignable to attribute `alpn_protocols` on type `QuicTlsSettings | None`
- error[invalid-assignment] test/mitmproxy/proxy/layers/quic/test__stream_layers.py:523:13: Object of type `list[Unknown]` is not assignable to attribute `alpn_protocols` on type `QuicTlsSettings | None`
- warning[possibly-unbound-attribute] test/mitmproxy/proxy/layers/test_tls.py:214:9: Attribute `set_accept_state` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] test/mitmproxy/proxy/layers/test_tls.py:250:9: Attribute `set_connect_state` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] test/mitmproxy/proxy/layers/test_tls.py:252:9: Attribute `set_tlsext_host_name` on type `Connection | None` is possibly unbound
- warning[possibly-unbound-attribute] test/mitmproxy/proxy/layers/test_tls.py:258:41: Attribute `_ssl` on type `Connection | None` is possibly unbound
- error[unresolved-attribute] test/mitmproxy/proxy/test_layer.py:146:31: Type `Layer | None` has no attribute `event_log`
- warning[possibly-unbound-attribute] test/mitmproxy/proxy/test_layer.py:178:16: Attribute `stack_pos` on type `Layer | None` is possibly unbound
- error[unresolved-attribute] test/mitmproxy/proxy/test_tutils.py:149:12: Type `TCommand` has no attribute `foo`
- error[unresolved-attribute] test/mitmproxy/proxy/test_tutils.py:149:21: Type `TCommand` has no attribute `foo`
- error[unresolved-attribute] test/mitmproxy/proxy/test_tutils.py:150:12: Type `TCommand` has no attribute `bar`
- error[unresolved-attribute] test/mitmproxy/proxy/test_tutils.py:150:23: Type `TCommand` has no attribute `bar`
- error[unresolved-attribute] test/mitmproxy/proxy/test_tutils.py:152:5: Type `TCommand` has no attribute `foo`
- warning[possibly-unbound-attribute] test/mitmproxy/tools/console/test_keymap.py:29:20: Attribute `called` on type `Unknown | CommandExecutor` is possibly unbound
- warning[possibly-unbound-attribute] test/mitmproxy/tools/console/test_keymap.py:32:16: Attribute `called` on type `Unknown | CommandExecutor` is possibly unbound
- warning[possibly-unbound-attribute] test/mitmproxy/tools/console/test_keymap.py:37:16: Attribute `called` on type `Unknown | CommandExecutor` is possibly unbound
- Found 2107 diagnostics
+ Found 2050 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- warning[unused-ignore-comment] tests/test___all__.py:26:27: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] tests/conftest.py:283:12: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] tests/test__cli.py:204:20: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] tests/test_gather.py:174:24: Type `FixtureRequest` has no attribute `param`
- Found 29 diagnostics
+ Found 31 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ error[unresolved-attribute] src/_pytest/python.py:1109:12: Type `FixtureRequest` has no attribute `param`
+ error[unresolved-attribute] testing/test_pathlib.py:120:17: Type `FixtureRequest` has no attribute `param`
- Found 776 diagnostics
+ Found 778 diagnostics

paasta (https://github.com/yelp/paasta)
- warning[possibly-unbound-attribute] paasta_tools/api/api.py:254:28: Attribute `get_cluster` on type `SystemPaastaConfig | None` is possibly unbound
- error[unresolved-attribute] paasta_tools/api/api.py:331:16: Type `<module 'sys'>` has no attribute `_argv`
- Found 951 diagnostics
+ Found 949 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- warning[possibly-unbound-attribute] dragonchain/transaction_processor/level_5_actions_utest.py:244:9: Attribute `assert_called_once_with` on type `Unknown | (bound method InterchainModel.publish_l5_hash_to_public_network(l5_block_hash: str) -> str)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/transaction_processor/level_5_actions_utest.py:470:9: Attribute `assert_called_once` on type `Unknown | (bound method InterchainModel.check_balance() -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/transaction_processor/level_5_actions_utest.py:485:9: Attribute `assert_called_once` on type `Unknown | (bound method InterchainModel.check_balance() -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/transaction_processor/level_5_actions_utest.py:496:9: Attribute `assert_called_once` on type `Unknown | (bound method InterchainModel.get_transaction_fee_estimate() -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/transaction_processor/level_5_actions_utest.py:510:9: Attribute `assert_called_once` on type `Unknown | (bound method InterchainModel.get_transaction_fee_estimate() -> int)` is possibly unbound
- warning[possibly-unbound-attribute] dragonchain/transaction_processor/level_5_actions_utest.py:522:9: Attribute `assert_called_once` on type `Unknown | (bound method InterchainModel.get_transaction_fee_estimate() -> int)` is possibly unbound
- Found 326 diagnostics
+ Found 320 diagnostics

jinja (https://github.com/pallets/jinja)
- error[unresolved-attribute] tests/test_ext.py:331:13: Type `def get_user_count() -> Unknown` has no attribute `called`
- error[unresolved-attribute] tests/test_ext.py:336:16: Type `def get_user_count() -> Unknown` has no attribute `called`
- Found 228 diagnostics
+ Found 226 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[unsupported-operator] src/hydra_zen/structured_configs/_implementations.py:2966:17: Operator `+=` is unsupported between objects of type `None` and `str`
- Found 614 diagnostics
+ Found 613 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[unresolved-attribute] tests/test_command_check.py:198:9: Type `bound method CrawlerProcessBase.crawl(crawler_or_spidercls: type[Spider] | str | Crawler, *args: Any, **kwargs: Any) -> Awaitable[None]` has no attribute `assert_not_called`
- warning[possibly-unbound-attribute] tests/test_command_check.py:198:9: Attribute `crawl` on type `CrawlerProcessBase | None` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_contracts.py:377:16: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_contracts.py:387:16: Method `__getitem__` of type `Unknown | None` is possibly unbound
- error[unresolved-attribute] tests/test_crawler.py:192:69: Type `<class 'MySpider'>` has no attribute `cls`
- error[unresolved-attribute] tests/test_crawler.py:272:57: Type `<class 'MySpider'>` has no attribute `cls`
- error[unresolved-attribute] tests/test_crawler.py:352:61: Type `<class 'MySpider'>` has no attribute `cls`
- error[unresolved-attribute] tests/test_crawler.py:432:65: Type `<class 'MySpider'>` has no attribute `cls`
+ warning[unused-ignore-comment] tests/test_downloadermiddleware_robotstxt.py:201:51: Unused blanket `type: ignore` directive
- error[unresolved-attribute] tests/test_feedexport.py:1689:24: Type `<class 'Storage'>` has no attribute `open_file`
- error[unresolved-attribute] tests/test_feedexport.py:1711:24: Type `<class 'Storage'>` has no attribute `open_file`
- error[unresolved-attribute] tests/test_utils_python.py:183:9: Type `Obj` has no attribute `meta`
- error[unresolved-attribute] tests/test_utils_python.py:184:9: Type `Obj` has no attribute `meta`
- error[unresolved-attribute] tests/test_utils_python.py:194:9: Type `Obj` has no attribute `meta`
- Found 1329 diagnostics
+ Found 1317 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ error[unresolved-attribute] psycopg_pool/psycopg_pool/base.py:201:9: Unresolved attribute `_expire_at` on type `BaseConnection[Any]`.
- Found 1041 diagnostics
+ Found 1042 diagnostics

apprise (https://github.com/caronc/apprise)
- error[unresolved-attribute] apprise/logger.py:44:22: Type `<module 'logging'>` has no attribute `DEPRECATE`
- error[unresolved-attribute] apprise/logger.py:45:22: Type `<module 'logging'>` has no attribute `TRACE`
+ error[unsupported-operator] apprise/utils/parse.py:781:9: Operator `+=` is unsupported between objects of type `str` and `str | @Todo(Type::Intersection.call()) | None`
- error[unresolved-attribute] test/test_api.py:2045:12: Type `ConfigBase` has no attribute `asset`
- warning[possibly-unbound-attribute] test/test_config_http.py:277:16: Attribute `default_config_format` on type `ConfigHTTP` is possibly unbound
- warning[possibly-unbound-attribute] test/test_config_http.py:287:16: Attribute `default_config_format` on type `ConfigHTTP` is possibly unbound
- warning[possibly-unbound-attribute] test/test_config_http.py:297:16: Attribute `default_config_format` on type `ConfigHTTP` is possibly unbound
- warning[possibly-unbound-attribute] test/test_config_http.py:304:12: Attribute `default_config_format` on type `ConfigHTTP` is possibly unbound
- error[unresolved-attribute] test/test_config_http.py:310:23: Type `ConfigHTTP` has no attribute `max_buffer_size`
- error[unresolved-attribute] test/test_config_http.py:329:33: Type `ConfigHTTP` has no attribute `max_buffer_size`
- error[unresolved-attribute] test/test_config_http.py:334:34: Type `ConfigHTTP` has no attribute `max_buffer_size`
- error[unresolved-attribute] test/test_plugin_dbus.py:91:5: Type `ModuleType` has no attribute `repository`
+ error[unresolved-attribute] test/test_plugin_dbus.py:91:5: Unresolved attribute `GdkPixbuf` on type `ModuleType`.
- error[unresolved-attribute] test/test_plugin_dbus.py:93:5: Type `ModuleType` has no attribute `repository`
+ error[unresolved-attribute] test/test_plugin_dbus.py:93:5: Unresolved attribute `Pixbuf` on type `ModuleType`.
- error[unresolved-attribute] test/test_plugin_dbus.py:101:44: Type `ModuleType` has no attribute `repository`
- error[unresolved-attribute] test/test_plugin_gnome.py:73:5: Type `ModuleType` has no attribute `repository`
+ error[unresolved-attribute] test/test_plugin_gnome.py:73:5: Unresolved attribute `GdkPixbuf` on type `ModuleType`.
- error[unresolved-attribute] test/test_plugin_gnome.py:75:5: Type `ModuleType` has no attribute `repository`
+ error[unresolved-attribute] test/test_plugin_gnome.py:75:5: Unresolved attribute `Pixbuf` on type `ModuleType`.
- error[unresolved-attribute] test/test_plugin_gnome.py:76:5: Type `ModuleType` has no attribute `repository`
+ error[unresolved-attribute] test/test_plugin_gnome.py:76:5: Unresolved attribute `Notify` on type `ModuleType`.
- error[unresolved-attribute] test/test_plugin_gnome.py:77:5: Type `ModuleType` has no attribute `repository`
- error[unresolved-attribute] test/test_plugin_gnome.py:78:5: Type `ModuleType` has no attribute `repository`
- error[unresolved-attribute] test/test_plugin_gnome.py:86:44: Type `ModuleType` has no attribute `repository`
- error[unresolved-attribute] test/test_plugin_gnome.py:87:51: Type `ModuleType` has no attribute `repository`
- error[unresolved-attribute] test/test_plugin_windows.py:173:5: Type `ModuleType` has no attribute `LoadImage`
- error[unresolved-attribute] test/test_plugin_windows.py:178:5: Type `ModuleType` has no attribute `LoadImage`
- error[unresolved-attribute] test/test_plugin_windows.py:181:5: Type `ModuleType` has no attribute `UpdateWindow`
- error[unresolved-attribute] test/test_plugin_windows.py:186:5: Type `ModuleType` has no attribute `UpdateWindow`
- Found 3695 diagnostics
+ Found 3677 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-assignment] mypy/checker.py:5437:17: Object of type `Var` is not assignable to attribute `node` on type `NameExpr | None`
- warning[possibly-unbound-attribute] mypy/nodes.py:613:30: Attribute `line` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- error[invalid-argument-type] mypy/semanal.py:998:51: Argument to function `set_callable_name` is incorrect: Expected `Type`, found `Unknown | FunctionLike | None`
- error[invalid-assignment] mypy/semanal.py:1283:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `ProperType | None`
- warning[possibly-unbound-attribute] mypy/semanal.py:2805:41: Attribute `type` on type `Instance | None` is possibly unbound
+ warning[possibly-unbound-attribute] mypy/semanal.py:2805:41: Attribute `type` on type `Instance | None | (Unknown & ~None)` is possibly unbound
- error[invalid-assignment] mypy/semanal.py:5728:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `Unknown | Expression | None`
- error[invalid-assignment] mypy/semanal.py:5729:13: Object of type `Unknown | int` is not assignable to attribute `column` on type `Unknown | Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal.py:5730:13: Attribute `accept` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal.py:5741:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `Unknown | Expression | None`
- error[invalid-assignment] mypy/semanal.py:5742:13: Object of type `Unknown | int` is not assignable to attribute `column` on type `Unknown | Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal.py:5743:13: Attribute `accept` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal.py:5758:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `Unknown | Expression | None`
- error[invalid-assignment] mypy/semanal.py:5759:13: Object of type `Unknown | int` is not assignable to attribute `column` on type `Unknown | Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal.py:5760:13: Attribute `accept` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal.py:5791:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `Unknown | Expression | None`
- error[invalid-assignment] mypy/semanal.py:5792:13: Object of type `Unknown | int` is not assignable to attribute `column` on type `Unknown | Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal.py:5793:13: Attribute `accept` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal.py:5808:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `Unknown | Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal.py:5809:13: Attribute `accept` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal.py:5816:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `Unknown | Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal.py:5817:13: Attribute `accept` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal.py:5982:9: Object of type `Unknown | int` is not assignable to attribute `line` on type `TypeApplication | TypeAliasExpr | None`
- error[invalid-assignment] mypy/semanal.py:5983:9: Object of type `Unknown | int` is not assignable to attribute `column` on type `TypeApplication | TypeAliasExpr | None`
- warning[possibly-unbound-attribute] mypy/semanal_enum.py:131:9: Attribute `set_line` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal_namedtuple.py:133:21: Object of type `Unknown | int` is not assignable to attribute `line` on type `Expression | None`
- error[invalid-assignment] mypy/semanal_namedtuple.py:134:21: Object of type `Unknown | int` is not assignable to attribute `column` on type `Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal_namedtuple.py:313:13: Attribute `set_line` on type `Unknown | Expression | None` is possibly unbound
- warning[possibly-unbound-attribute] mypy/semanal_namedtuple.py:336:9: Attribute `set_line` on type `Unknown | Expression | None` is possibly unbound
- error[invalid-assignment] mypy/semanal_typeddict.py:122:13: Object of type `Unknown | int` is not assignable to attribute `line` on type `Expression | None`
- error[invalid-assignment] mypy/semanal_typeddict.py:123:13: Object of type `Unknown | int` is not assignable to attribute `column` on type `Expression | None`
- error[invalid-assignment] mypy/semanal_typeddict.py:177:9: Object of type `Unknown | int` is not assignable to attribute `line` on type `Expression | None`
- error[invalid-assignment] mypy/semanal_typeddict.py:178:9: Object of type `Unknown | int` is not assignable to attribute `column` on type `Expression | None`
- warning[possibly-unbound-attribute] mypy/semanal_typeddict.py:499:9: Attribute `set_line` on type `Unknown | Expression | None` is possibly unbound
- warning[possibly-unbound-attribute] mypy/server/astmerge.py:283:17: Attribute `accept` on type `SymbolNode | None` is possibly unbound
- error[invalid-assignment] mypy/stubgen.py:1746:5: Object of type `str` is not assignable to attribute `_fullname` on type `MypyFile | None`
- warning[possibly-unresolved-reference] mypy/test/testtypegen.py:71:64: Name `map` used when possibly not defined
+ warning[possibly-unbound-implicit-call] mypy/test/testtypegen.py:71:64: Method `__getitem__` of type `Unknown | dict[Expression, Type] | <class 'map'>` is possibly unbound
- warning[possibly-unbound-attribute] mypy/treetransform.py:592:13: Attribute `set_line` on type `TypeApplication | TypeAliasExpr | None` is possibly unbound
- error[invalid-argument-type] mypy/treetransform.py:592:35: Argument to bound method `set_line` is incorrect: Expected `Context | int`, found `TypeApplication | TypeAliasExpr | None`
- error[invalid-argument-type] mypy/treetransform.py:592:35: Argument to bound method `set_line` is incorrect: Expected `Context | int`, found `TypeApplication | TypeAliasExpr | None`
- warning[possibly-unbound-attribute] mypyc/irbuild/vtable.py:32:9: Attribute `update` on type `dict[str, int] | None` is possibly unbound
- warning[possibly-unbound-implicit-call] mypyc/irbuild/vtable.py:44:17: Method `__getitem__` of type `dict[str, int] | None` is possibly unbound
- Found 3225 diagnostics
+ Found 3186 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- warning[possibly-unbound-attribute] tests/test_debug.py:198:17: Attribute `reset` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_debug.py:200:17: Attribute `reset` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_debug.py:215:17: Attribute `reset` on type `TextIO | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- error[invalid-assignment] tests/test_http.py:141:9: Object of type `float` is not assignable to attribute `max_age` of type `int | None`
- error[invalid-assignment] tests/test_http.py:144:9: Object of type `float` is not assignable to attribute `s_maxage` of type `int | None`
- Found 472 diagnostics
+ Found 467 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
- warning[possibly-unbound-attribute] src/menu_bar.py:159:5: Attribute `start` on type `QThread | None | @Todo(instance attribute on class with dynamic base)` is possibly unbound
- Found 52 diagnostics
+ Found 51 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[unsupported-operator] pwndbg/aglib/disasm/arch.py:315:20: Operator `>=` is not supported for types `None` and `int`, in comparing `int | None` with `Literal[0]`
- error[invalid-argument-type] pwndbg/aglib/disasm/arch.py:316:70: Argument to function `attempt_colorized_symbol` is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] pwndbg/aglib/disasm/arch.py:329:29: Argument to function `hex` is incorrect: Expected `SupportsIndex`, found `int | None`
- error[invalid-argument-type] pwndbg/aglib/disasm/arch.py:961:58: Argument to function `register_assign` is incorrect: Expected `str`, found `str | None`
- error[unsupported-operator] pwndbg/aglib/disasm/arch.py:1058:17: Operator `+=` is unsupported between objects of type `None` and `str`
- error[unsupported-operator] pwndbg/aglib/disasm/arch.py:1078:20: Operator `>=` is not supported for types `None` and `int`, in comparing `int | None` with `Literal[0]`
- error[invalid-argument-type] pwndbg/aglib/disasm/arch.py:1079:70: Argument to function `attempt_colorized_symbol` is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] pwndbg/aglib/disasm/arch.py:1082:65: Argument to function `hex` is incorrect: Expected `SupportsIndex`, found `int | None`
- error[unresolved-attribute] pwndbg/commands/telescope.py:237:54: Type `CommandObj` has no attribute `offset`
- error[unresolved-attribute] pwndbg/commands/telescope.py:238:73: Type `CommandObj` has no attribute `offset`
- error[unresolved-attribute] pwndbg/commands/telescope.py:240:43: Type `CommandObj` has no attribute `offset`
- error[unresolved-attribute] pwndbg/commands/telescope.py:241:30: Type `CommandObj` has no attribute `offset`
- error[unresolved-attribute] pwndbg/commands/telescope.py:273:5: Type `CommandObj` has no attribute `offset`
- error[unresolved-attribute] pwndbg/gdblib/events.py:253:5: Type `<module 'gdb.events'>` has no attribute `start`
- Found 2303 diagnostics
+ Found 2289 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ error[invalid-argument-type] static_frame/core/container_util.py:324:38: Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `def format(value: object, format_spec: str = Literal[""], /) -> str`
- error[unresolved-reference] static_frame/core/container_util.py:323:38: Name `format` used when not defined
- error[unresolved-reference] static_frame/core/container_util.py:324:38: Name `format` used when not defined
- Found 1932 diagnostics
+ Found 1931 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- warning[possibly-unbound-attribute] discord/ext/commands/core.py:1650:19: Attribute `invoke` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[possibly-unbound-attribute] discord/ext/commands/core.py:1650:19: Attribute `invoke` on type `None | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] discord/ext/commands/core.py:1690:19: Attribute `reinvoke` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[possibly-unbound-attribute] discord/ext/commands/core.py:1690:19: Attribute `reinvoke` on type `None | Unknown` is possibly unbound

colour (https://github.com/colour-science/colour)
- error[unresolved-attribute] colour/__init__.py:942:25: Type `<class 'colour'>` has no attribute `__disable_lazy_load__`
- Found 543 diagnostics
+ Found 542 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- error[type-assertion-failure] tests/test_config.py:91:11: Argument does not have asserted type `Literal["left"]`
- error[type-assertion-failure] tests/test_config.py:93:11: Argument does not have asserted type `Literal["right"]`
- error[type-assertion-failure] tests/test_config.py:98:11: Argument does not have asserted type `Literal["truncate"]`
- error[type-assertion-failure] tests/test_config.py:100:11: Argument does not have asserted type `Literal["info"]`
- error[type-assertion-failure] tests/test_config.py:105:11: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/test_config.py:107:11: Argument does not have asserted type `Literal[False]`
- error[type-assertion-failure] tests/test_config.py:109:11: Argument does not have asserted type `Literal["deep"]`
- error[type-assertion-failure] tests/test_config.py:111:11: Argument does not have asserted type `None`
- error[type-assertion-failure] tests/test_config.py:116:11: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/test_config.py:118:11: Argument does not have asserted type `Literal[False]`
- error[type-assertion-failure] tests/test_config.py:120:11: Argument does not have asserted type `Literal["truncate"]`
- Found 2943 diagnostics
+ Found 2932 diagnostics

meson (https://github.com/mesonbuild/meson)
- warning[possibly-unbound-attribute] docs/refman/loaderbase.py:165:17: Attribute `extended_by` on type `Object | None` is possibly unbound
- error[call-non-callable] mesonbuild/coredata.py:718:9: Method `__getitem__` of type `bound method dict[OptionKey, str].__getitem__(key: OptionKey, /) -> str` is not callable on object of type `dict[OptionKey, str]`
- error[invalid-assignment] mesonbuild/interpreter/interpreter.py:1453:17: Object of type `Any | None` is not assignable to attribute `colorize_console` on type `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[unresolved-attribute] mesonbuild/interpreter/interpreter.py:1453:17: Unresolved attribute `colorize_console` on type `StringIO`.
- warning[possibly-unresolved-reference] unittests/internaltests.py:1061:32: Name `compile` used when possibly not defined
+ error[no-matching-overload] unittests/internaltests.py:1061:32: No overload of function `compile` matches arguments
- Found 1333 diagnostics
+ Found 1331 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- error[invalid-assignment] cwltool/pack.py:126:5: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `idx` on type `Loader | None`
- error[not-iterable] cwltool/workflow.py:206:29: Object of type `list[Unknown | MutableMapping[str, @Todo(Inference of subscript on special form) | None]] | None` may not be iterable
- warning[possibly-unbound-attribute] cwltool/workflow.py:211:17: Attribute `append` on type `list[Unknown | MutableMapping[str, @Todo(Inference of subscript on special form) | None]] | None` is possibly unbound
- warning[possibly-unbound-attribute] cwltool/workflow.py:212:9: Attribute `extend` on type `list[Unknown | MutableMapping[str, @Todo(Inference of subscript on special form) | None]] | None` is possibly unbound
- Found 302 diagnostics
+ Found 298 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- warning[possibly-unbound-attribute] tests/unittests/net/test_dhcp.py:73:13: Attribute `write_text` on type `Unknown | Literal["/run/dhclient.lease"]` is possibly unbound
- Found 749 diagnostics
+ Found 748 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[unresolved-attribute] src/bokeh/plotting/graph.py:124:25: Type `DataSource` has no attribute `data`
- error[not-iterable] src/bokeh/server/session.py:105:26: Object of type `list[Awaitable[None]] | None` may not be iterable
+ warning[unused-ignore-comment] src/bokeh/util/tornado.py:236:34: Unused blanket `type: ignore` directive
- Found 940 diagnostics
+ Found 939 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:58:10: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:64:52: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:65:61: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:70:12: Type `ElGamalKey` has no attribute `g`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:75:13: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:75:26: Type `ElGamalKey` has no attribute `g`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:81:16: Type `ElGamalKey` has no attribute `g`
+ error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:81:16: Type `int` has no attribute `inverse`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:81:30: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:82:13: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:90:48: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:93:17: Type `ElGamalKey` has no attribute `g`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:93:24: Type `ElGamalKey` has no attribute `x`
- error[unresolved-attribute] lib/Crypto/PublicKey/ElGamal.py:93:31: Type `ElGamalKey` has no attribute `p`
- error[unresolved-attribute] lib/Crypto/SelfTest/Protocol/test_KDF.py:421:32: Type `TestVector` has no attribute `output`
- error[unresolved-attribute] lib/Crypto/SelfTest/loader.py:235:102: Type `TestVector` has no attribute `id`
- Found 1544 diagnostics
+ Found 1530 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- warning[possibly-unbound-attribute] freqtrade/freqai/base_models/BaseClassifierModel.py:129:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- warning[possibly-unbound-attribute] freqtrade/freqai/base_models/BasePyTorchClassifier.py:99:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- warning[possibly-unbound-attribute] freqtrade/freqai/base_models/BasePyTorchRegressor.py:60:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- warning[possibly-unbound-attribute] freqtrade/freqai/base_models/BaseRegressionModel.py:123:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- warning[possibly-unbound-attribute] freqtrade/freqai/prediction_models/PyTorchTransformerRegressor.py:155:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- warning[possibly-unbound-attribute] freqtrade/freqai/prediction_models/SKLearnRandomForestClassifier.py:84:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- warning[possibly-unbound-attribute] freqtrade/freqai/prediction_models/XGBoostClassifier.py:88:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- warning[possibly-unbound-attribute] freqtrade/freqai/prediction_models/XGBoostRFClassifier.py:88:26: Attribute `do_predict` on type `FreqaiDataKitchen` is possibly unbound
- Found 484 diagnostics
+ Found 476 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ warning[unused-ignore-comment] lib/streamlit/logger.py:80:63: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] lib/streamlit/logger.py:83:58: Unused blanket `type: ignore` directive
- Found 3273 diagnostics
+ Found 3275 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[unresolved-attribute] openlibrary/coverstore/tests/test_webapp.py:26:25: Type `<module 'openlibrary.coverstore.config'>` has no attribute `db_parameters`
- Found 727 diagnostics
+ Found 726 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[unresolved-attribute] ddtrace/contrib/internal/unittest/patch.py:863:12: Type `<class 'CIVisibility'>` has no attribute `_datadog_finished_sessions`
- error[unresolved-attribute] ddtrace/contrib/internal/unittest/patch.py:863:56: Type `<class 'CIVisibility'>` has no attribute `_datadog_expected_sessions`
- error[unresolved-attribute] ddtrace/contrib/internal/unittest/patch.py:868:8: Type `<class 'CIVisibility'>` has no attribute `_datadog_finished_sessions`
- error[unresolved-attribute] ddtrace/contrib/internal/unittest/patch.py:868:52: Type `<class 'CIVisibility'>` has no attribute `_datadog_expected_sessions`
- error[unresolved-attribute] ddtrace/internal/datadog/profiling/ddup/test/interface.py:10:1: Unresolved attribute `__version__` on type `ModuleType`.
- error[unresolved-attribute] ddtrace/internal/datadog/profiling/ddup/test/interface.py:42:1: Unresolved attribute `Span` on type `ModuleType`.
- error[unresolved-attribute] ddtrace/profiling/bootstrap/sitecustomize.py:12:5: Type `<module 'ddtrace.profiling.bootstrap'>` has no attribute `profiler`
- error[unresolved-attribute] ddtrace/vendor/ply/lex.py:335:24: Type `LexToken` has no attribute `type`
- error[unresolved-attribute] ddtrace/vendor/ply/lex.py:372:32: Type `LexToken` has no attribute `value`
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/lex.py:931:9: Attribute `add` on type `Unknown | None` is possibly unbound
- error[unsupported-operator] ddtrace/vendor/ply/lex.py:939:28: Operator `|` is unsupported between objects of type `Unknown | None` and `set[Unknown]`
- error[unresolved-attribute] ddtrace/vendor/psutil/__init__.py:2184:1: Type `def disk_io_counters(perdisk=Literal[False], nowrap=Literal[True]) -> Unknown` has no attribute `cache_clear`
- error[unresolved-attribute] ddtrace/vendor/psutil/__init__.py:2233:1: Type `def net_io_counters(pernic=Literal[False], nowrap=Literal[True]) -> Unknown` has no attribute `cache_clear`
- error[unresolved-attribute] tests/appsec/iast/aspects/test_side_effects.py:75:14: Type `MagicMethodsException`...*[Comment body truncated]*

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/place.rs`:54 on 2025-05-18 15:00_

The most important change in this PR is replacing the `Symbol` API with the `Place` API.

With this PR, assignments such as `c.x = ...` and `c[0] = ...` are now tracked by `UseDefMap`. Therefore, it is no longer accurate to call the target of the assignment a symbol.
There seems to be no specific name in Python's vocabulary for the expression that is the target of the assignment, which includes names, attributes and subscripts (but does not include lists or tuples), so I have chosen to name it *place*, following [the Rust's terminology](https://doc.rust-lang.org/reference/expressions.html#r-expr.place-value.place-memory-location).
This is almost the same as *lvalue* ​​in C, but I personally prefer the Rust naming (even though they're not strictly equivalent), as lvalue ​​feel like something that only appears on the left hand side of an assignment.

I've also added a comment to the top of `use_def.rs` explaining this.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/primer/bad.txt`:4 on 2025-05-18 15:19_

I'm currently investigating the direct cause of these package checks failing, but as mentioned in the comments on `narrow.rs`, I think it is due to the formation of a previously unrealized call graph (potentially causing a panic / hang).

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/narrow.rs`:626 on 2025-05-18 16:40_

I made this change in response to [the mypy_primer failure](https://github.com/astral-sh/ruff/actions/runs/15075142424/job/42380715833?pr=18041).
Before the fix, the minimal code that reproduces the panic is as follows:

```python
class C:
    def f(self, other: "C"):
        if self.a > other.b or self.b:
            return False
        if self:
            return True

C().a
```

stacktrace:
<details>

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[panic]: Panicked at crates\ty_python_semantic\src\types\narrow.rs:838:22 when checking `C:\Users\sbym8\Desktop\typepy\panic.py`: `expression should belong to this TypeInference region and TypeInferenceBuilder should have inferred a type for it`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: windows x86_64
info: Args: ["C:\\users\\sbym8\\GitHub\\ruff\\target\\debug\\ty.exe", "check", "panic.py"]
info: Backtrace:
   0: std::backtrace_rs::backtrace::win64::trace
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\..\..\backtrace\src\backtrace\win64.rs:85
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\..\..\backtrace\src\backtrace\mod.rs:66
   2: std::backtrace::Backtrace::create
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\backtrace.rs:331
   3: std::backtrace::Backtrace::capture
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\backtrace.rs:296
   4: ruff_db::panic::install_hook::closure$0::closure$0
             at C:\Users\sbym8\GitHub\ruff\crates\ruff_db\src\panic.rs:81
   5: std::panicking::rust_panic_with_hook
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\panicking.rs:841
   6: std::panicking::begin_panic_handler::closure$0
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\panicking.rs:706
   7: std::sys::backtrace::__rust_end_short_backtrace<std::panicking::begin_panic_handler::closure_env$0,never$>
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\sys\backtrace.rs:168
   8: std::panicking::begin_panic_handler
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\panicking.rs:697
   9: core::panicking::panic_fmt
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\core\src\panicking.rs:75
  10: core::panicking::panic_display
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\core\src\panicking.rs:261
  11: core::option::expect_failed
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\core\src\option.rs:2024
  12: enum2$<core::option::Option<enum2$<ty_python_semantic::types::Type> > >::expect<enum2$<ty_python_semantic::types::Type> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:933
  13: ty_python_semantic::types::infer::TypeInference::expression_type
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:444
  14: ty_python_semantic::types::narrow::impl$1::evaluate_bool_op::closure$0
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\narrow.rs:837
  15: core::ops::function::impls::impl$3::call_mut<tuple$<ref$<ref$<enum2$<ruff_python_ast::generated::Expr> > > >,ty_python_semantic::types::narrow::impl$1::evaluate_bool_op::closure_env$0>
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:294
  16: core::slice::iter::impl$182::find<enum2$<ruff_python_ast::generated::Expr>,ref_mut$<ty_python_semantic::types::narrow::impl$1::evaluate_bool_op::closure_env$0> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\slice\iter\macros.rs:325
  17: core::iter::adapters::filter::impl$3::next<core::slice::iter::Iter<enum2$<ruff_python_ast::generated::Expr> >,ty_python_semantic::types::narrow::impl$1::evaluate_bool_op::closure_env$0>
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\filter.rs:98
  18: core::iter::adapters::map::impl$2::next<enum2$<core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHasher> > >,core::iter::adapters::f
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\map.rs:107
  19: alloc::vec::spec_from_iter_nested::impl$0::from_iter<enum2$<core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHasher> > >,core::iter
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\spec_from_iter_nested.rs:25
  20: alloc::vec::spec_from_iter::impl$0::from_iter<enum2$<core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHasher> > >,core::iter::adapt
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\spec_from_iter.rs:34
  21: alloc::vec::impl$15::from_iter<enum2$<core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHasher> > >,core::iter::adapters::map::Map<c
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\mod.rs:3438
  22: core::iter::traits::iterator::Iterator::collect<core::iter::adapters::map::Map<core::iter::adapters::filter::Filter<core::slice::iter::Iter<enum2$<ruff_python_ast::generated::Expr> >,ty_python_semantic::types::narrow::impl$1::evaluate_bool_op::closure_env$
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:1985
  23: ty_python_semantic::types::narrow::NarrowingConstraintsBuilder::evaluate_bool_op
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\narrow.rs:832
  24: ty_python_semantic::types::narrow::NarrowingConstraintsBuilder::evaluate_expression_node_predicate
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\narrow.rs:312
  25: ty_python_semantic::types::narrow::NarrowingConstraintsBuilder::evaluate_expression_predicate
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\narrow.rs:292
  26: ty_python_semantic::types::narrow::NarrowingConstraintsBuilder::finish
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\narrow.rs:271
  27: ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::impl$1::execute::inner_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\narrow.rs:99
  28: ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
  29: salsa::function::IngredientImpl<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>::execute_query<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
  30: salsa::function::IngredientImpl<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
  31: salsa::function::IngredientImpl<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>::execute<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
  32: salsa::function::IngredientImpl<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>::fetch_cold<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
  33: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
  34: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<enum2$<core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHasher> > > > 
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
  35: salsa::function::IngredientImpl<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
  36: salsa::function::IngredientImpl<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>::fetch<ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
  37: ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:365
  38: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,enum2$<core::option::Option<ref$<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHashe
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
  39: salsa::attach::attach::closure$0<enum2$<core::option::Option<ref$<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHasher> > > >,dyn$<ty_python_semantic
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
  40: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<core::option::Option<ref$<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
  41: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<core::option::Option<ref$<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<t
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
  42: salsa::attach::attach<enum2$<core::option::Option<ref$<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId,enum2$<ty_python_semantic::types::Type>,rustc_hash::FxBuildHasher> > > >,dyn$<ty_python_semantic::db::Db>,t
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
  43: ty_python_semantic::types::narrow::all_negative_narrowing_constraints_for_expression
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
  44: ty_python_semantic::types::narrow::infer_narrowing_constraint
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\narrow.rs:51
  45: ty_python_semantic::semantic_index::use_def::impl$5::narrow::closure$0
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\use_def.rs:597
  46: core::ops::function::impls::impl$3::call_mut<tuple$<ty_python_semantic::semantic_index::predicate::Predicate>,ty_python_semantic::semantic_index::use_def::impl$5::narrow::closure_env$0>
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:294
  47: core::iter::traits::iterator::Iterator::find_map::check::closure$0<ty_python_semantic::semantic_index::predicate::Predicate,enum2$<ty_python_semantic::types::Type>,ref_mut$<ty_python_semantic::semantic_index::use_def::impl$5::narrow::closure_env$0> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:2873
  48: core::iter::traits::iterator::Iterator::try_fold<ty_python_semantic::semantic_index::use_def::ConstraintsIterator,tuple$<>,core::iter::traits::iterator::Iterator::find_map::check::closure_env$0<ty_python_semantic::semantic_index::predicate::Predicate,enum2
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:2384
  49: core::iter::traits::iterator::Iterator::find_map<ty_python_semantic::semantic_index::use_def::ConstraintsIterator,enum2$<ty_python_semantic::types::Type>,ref_mut$<ty_python_semantic::semantic_index::use_def::impl$5::narrow::closure_env$0> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:2879
  50: core::iter::adapters::filter_map::impl$2::next<enum2$<ty_python_semantic::types::Type>,ty_python_semantic::semantic_index::use_def::ConstraintsIterator,ty_python_semantic::semantic_index::use_def::impl$5::narrow::closure_env$0>
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\filter_map.rs:64
  51: alloc::vec::spec_from_iter_nested::impl$0::from_iter<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::filter_map::FilterMap<ty_python_semantic::semantic_index::use_def::ConstraintsIterator,ty_python_semantic::semantic_index::use_def::impl$5::n
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\spec_from_iter_nested.rs:25
  52: alloc::vec::spec_from_iter::impl$0::from_iter<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::filter_map::FilterMap<ty_python_semantic::semantic_index::use_def::ConstraintsIterator,ty_python_semantic::semantic_index::use_def::impl$5::narrow::
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\spec_from_iter.rs:34
  53: alloc::vec::impl$15::from_iter<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::filter_map::FilterMap<ty_python_semantic::semantic_index::use_def::ConstraintsIterator,ty_python_semantic::semantic_index::use_def::impl$5::narrow::closure_env$0> 
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\vec\mod.rs:3438
  54: core::iter::traits::iterator::Iterator::collect<core::iter::adapters::filter_map::FilterMap<ty_python_semantic::semantic_index::use_def::ConstraintsIterator,ty_python_semantic::semantic_index::use_def::impl$5::narrow::closure_env$0>,alloc::vec::Vec<enum2$<
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:1985
  55: ty_python_semantic::semantic_index::use_def::ConstraintsIterator::narrow
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\use_def.rs:596
  56: ty_python_semantic::place::place_from_bindings_impl::closure$2
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\place.rs:912
  57: core::ops::function::impls::impl$3::call_mut<tuple$<ty_python_semantic::semantic_index::use_def::BindingWithConstraints>,ty_python_semantic::place::place_from_bindings_impl::closure_env$2>
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:294
  58: core::iter::traits::iterator::Iterator::find_map::check::closure$0<ty_python_semantic::semantic_index::use_def::BindingWithConstraints,enum2$<ty_python_semantic::types::Type>,ref_mut$<ty_python_semantic::place::place_from_bindings_impl::closure_env$2> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:2873
  59: core::iter::adapters::peekable::impl$1::try_fold<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator,tuple$<>,core::iter::traits::iterator::Iterator::find_map::check::closure_env$0<ty_python_semantic::semantic_index::use_def::Bindin
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\peekable.rs:100
  60: core::iter::traits::iterator::Iterator::find_map<core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>,enum2$<ty_python_semantic::types::Type>,ref_mut$<ty_python_semantic::place::place_from_bi
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\traits\iterator.rs:2879
  61: core::iter::adapters::filter_map::impl$2::next<enum2$<ty_python_semantic::types::Type>,core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>,ty_python_semantic::place::place_from_bindings_impl
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\filter_map.rs:64
  62: ty_python_semantic::place::place_from_bindings_impl
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\place.rs:916
  63: ty_python_semantic::place::place_from_bindings
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\place.rs:468
  64: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_place_load
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:5546
  65: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:5468
  66: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_name_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:5718
  67: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:4260
  68: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:1380
  69: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:738
  70: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:7622
  71: ty_python_semantic::types::infer::infer_expression_types::impl$1::execute::inner_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:239
  72: ty_python_semantic::types::infer::infer_expression_types::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
  73: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::execute_query<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
  74: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
  75: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::execute<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
  76: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
  77: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
  78: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference> >,salsa::function::fetch::impl$0::refresh_memo::c
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
  79: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
  80: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::fetch<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
  81: ty_python_semantic::types::infer::infer_expression_types::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:365
  82: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::infer::TypeInference>,ty_python_semantic::types::infer::infer_expression_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
  83: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
  84: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expr
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
  85: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expressi
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
  86: salsa::attach::attach<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
  87: ty_python_semantic::types::infer::infer_expression_types
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
  88: ty_python_semantic::types::infer::infer_same_file_expression_type
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:267
  89: ty_python_semantic::types::infer::infer_expression_type::impl$1::execute::inner_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:285
  90: ty_python_semantic::types::infer::infer_expression_type::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
  91: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::execute_query<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
  92: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
  93: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::execute<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
  94: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
  95: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
  96: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<enum2$<ty_python_semantic::types::Type> > > > >::or_else<ref$<salsa::function::memo::Memo<enum2$<ty_python_semantic::types::Type> > >,salsa::function::fetch::impl$0::refresh_memo::closure_env$0<t
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
  97: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
  98: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::fetch<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
  99: ty_python_semantic::types::infer::infer_expression_type::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:365
 100: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::infer::infer_expression_type::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
 101: salsa::attach::attach::closure$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
 102: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::c
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
 103: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::closu
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
 104: salsa::attach::attach<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
 105: ty_python_semantic::types::infer::infer_expression_type
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
 106: ty_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::analyze_single
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\visibility_constraints.rs:652
 107: ty_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::evaluate
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\visibility_constraints.rs:552
 108: ty_python_semantic::semantic_index::use_def::UseDefMap::is_binding_visible
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\use_def.rs:480
 109: ty_python_semantic::types::class::impl$43::implicit_instance_attribute::closure$1
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1513
 110: enum2$<core::option::Option<ref$<ty_python_semantic::semantic_index::use_def::BindingWithConstraints> > >::map<ref$<ty_python_semantic::semantic_index::use_def::BindingWithConstraints>,ty_python_semantic::types::Truthiness,ty_python_semantic::types::class:
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1119
 111: ty_python_semantic::types::class::ClassLiteral::implicit_instance_attribute
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1509
 112: ty_python_semantic::types::class::ClassLiteral::own_instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1810
 113: enum2$<ty_python_semantic::types::class::ClassType>::own_instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:479
 114: ty_python_semantic::types::class::ClassLiteral::instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1431
 115: enum2$<ty_python_semantic::types::class::ClassType>::instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:470
 116: enum2$<ty_python_semantic::types::Type>::instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:2551
 117: ty_python_semantic::types::impl$100::member_lookup_with_policy::impl$0::member_lookup_with_policy_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:3049
 118: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::impl$1::execute::inner_
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_method_body.rs:34
 119: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
 120: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::execute_query<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configura
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
 121: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::C
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
 122: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::execute<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
 123: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::fetch_cold<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuratio
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
 124: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
 125: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::place::PlaceAndQualifiers> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::place::PlaceAndQualifiers> >,salsa::function::fetch::impl$0::refresh_memo::closu
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
 126: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
 127: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::fetch<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
 128: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:362
 129: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ty_python_semantic::place::PlaceAndQualifiers,ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
 130: salsa::attach::attach::closure$0<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
 131: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_w
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
 132: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_with_
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
 133: salsa::attach::attach<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
 134: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
 135: enum2$<ty_python_semantic::types::Type>::member_lookup_with_policy
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:534
 136: enum2$<ty_python_semantic::types::Type>::member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:2882
 137: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_attribute_load
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:5770
 138: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_attribute_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:5843
 139: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:4261
 140: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:4226
 141: ty_python_semantic::types::infer::impl$2::infer_compare_expression::closure$0
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:6426
 142: ty_python_semantic::types::infer::impl$2::infer_chained_boolean_types::closure$0<core::iter::adapters::zip::Zip<itertools::tuple_impl::TupleWindows<core::iter::adapters::chain::Chain<core::iter::sources::once::Once<ref$<enum2$<ruff_python_ast::generated::E
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:6358
 143: core::iter::adapters::map::map_fold::closure$0<tuple$<itertools::with_position::Position,tuple$<tuple$<ref$<enum2$<ruff_python_ast::generated::Expr> >,ref$<enum2$<ruff_python_ast::generated::Expr> > >,ref$<ruff_python_ast::nodes::CmpOp> > >,enum2$<ty_pytho
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\map.rs:88
 144: itertools::with_position::impl$2::fold<core::iter::adapters::zip::Zip<itertools::tuple_impl::TupleWindows<core::iter::adapters::chain::Chain<core::iter::sources::once::Once<ref$<enum2$<ruff_python_ast::generated::Expr> > >,core::slice::iter::Iter<enum2$<ru
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\itertools-0.14.0\src\with_position.rs:107
 145: core::iter::adapters::map::impl$2::fold<enum2$<ty_python_semantic::types::Type>,itertools::with_position::WithPosition<core::iter::adapters::zip::Zip<itertools::tuple_impl::TupleWindows<core::iter::adapters::chain::Chain<core::iter::sources::once::Once<ref
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\iter\adapters\map.rs:128
 146: ty_python_semantic::types::UnionType::from_elements<core::iter::adapters::map::Map<itertools::with_position::WithPosition<core::iter::adapters::zip::Zip<itertools::tuple_impl::TupleWindows<core::iter::adapters::chain::Chain<core::iter::sources::once::Once<
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:7513
 147: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_chained_boolean_types<core::iter::adapters::zip::Zip<itertools::tuple_impl::TupleWindows<core::iter::adapters::chain::Chain<core::iter::sources::once::Once<ref$<enum2$<ruff_python_ast::generated
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:6398
 148: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_compare_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:6418
 149: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:4265
 150: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:1380
 151: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:738
 152: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:7622
 153: ty_python_semantic::types::infer::infer_expression_types::impl$1::execute::inner_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:239
 154: ty_python_semantic::types::infer::infer_expression_types::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
 155: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::execute_query<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
 156: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
 157: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::execute<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
 158: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
 159: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
 160: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference> >,salsa::function::fetch::impl$0::refresh_memo::c
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
 161: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
 162: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_types::Configuration_>::fetch<ty_python_semantic::types::infer::infer_expression_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
 163: ty_python_semantic::types::infer::infer_expression_types::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:365
 164: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::infer::TypeInference>,ty_python_semantic::types::infer::infer_expression_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
 165: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
 166: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expr
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
 167: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expressi
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
 168: salsa::attach::attach<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
 169: ty_python_semantic::types::infer::infer_expression_types
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
 170: ty_python_semantic::types::infer::infer_same_file_expression_type
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:267
 171: ty_python_semantic::types::infer::infer_expression_type::impl$1::execute::inner_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:285
 172: ty_python_semantic::types::infer::infer_expression_type::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
 173: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::execute_query<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
 174: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
 175: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::execute<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
 176: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
 177: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
 178: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<enum2$<ty_python_semantic::types::Type> > > > >::or_else<ref$<salsa::function::memo::Memo<enum2$<ty_python_semantic::types::Type> > >,salsa::function::fetch::impl$0::refresh_memo::closure_env$0<t
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
 179: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
 180: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_expression_type::Configuration_>::fetch<ty_python_semantic::types::infer::infer_expression_type::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
 181: ty_python_semantic::types::infer::infer_expression_type::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:365
 182: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,enum2$<ty_python_semantic::types::Type>,ty_python_semantic::types::infer::infer_expression_type::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
 183: salsa::attach::attach::closure$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
 184: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::c
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
 185: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::closu
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
 186: salsa::attach::attach<enum2$<ty_python_semantic::types::Type>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_expression_type::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
 187: ty_python_semantic::types::infer::infer_expression_type
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
 188: ty_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::analyze_single
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\visibility_constraints.rs:652
 189: ty_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints::evaluate
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\visibility_constraints.rs:552
 190: ty_python_semantic::semantic_index::use_def::UseDefMap::is_binding_visible
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\semantic_index\use_def.rs:480
 191: ty_python_semantic::types::class::impl$43::implicit_instance_attribute::closure$1
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1513
 192: enum2$<core::option::Option<ref$<ty_python_semantic::semantic_index::use_def::BindingWithConstraints> > >::map<ref$<ty_python_semantic::semantic_index::use_def::BindingWithConstraints>,ty_python_semantic::types::Truthiness,ty_python_semantic::types::class:
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1119
 193: ty_python_semantic::types::class::ClassLiteral::implicit_instance_attribute
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1509
 194: ty_python_semantic::types::class::ClassLiteral::own_instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1810
 195: enum2$<ty_python_semantic::types::class::ClassType>::own_instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:479
 196: ty_python_semantic::types::class::ClassLiteral::instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:1431
 197: enum2$<ty_python_semantic::types::class::ClassType>::instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\class.rs:470
 198: enum2$<ty_python_semantic::types::Type>::instance_member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:2551
 199: ty_python_semantic::types::impl$100::member_lookup_with_policy::impl$0::member_lookup_with_policy_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:3049
 200: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::impl$1::execute::inner_
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_method_body.rs:34
 201: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
 202: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::execute_query<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configura
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
 203: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::C
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
 204: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::execute<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
 205: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::fetch_cold<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuratio
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
 206: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
 207: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::place::PlaceAndQualifiers> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::place::PlaceAndQualifiers> >,salsa::function::fetch::impl$0::refresh_memo::closu
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
 208: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
 209: salsa::function::IngredientImpl<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>::fetch<ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
 210: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:362
 211: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ty_python_semantic::place::PlaceAndQualifiers,ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
 212: salsa::attach::attach::closure$0<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
 213: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_w
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
 214: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_with_
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
 215: salsa::attach::attach<ty_python_semantic::place::PlaceAndQualifiers,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
 216: ty_python_semantic::types::impl$100::member_lookup_with_policy::member_lookup_with_policy_
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
 217: enum2$<ty_python_semantic::types::Type>::member_lookup_with_policy
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:534
 218: enum2$<ty_python_semantic::types::Type>::member
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:2882
 219: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_attribute_load
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:5770
 220: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_attribute_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:5843
 221: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:4261
 222: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:4226
 223: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:1885
 224: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_body
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:1876
 225: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_module
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:1655
 226: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:747
 227: ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:735
 228: ty_python_semantic::types::infer::TypeInferenceBuilder::finish
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:7622
 229: ty_python_semantic::types::infer::infer_scope_types::impl$1::execute::inner_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:131
 230: ty_python_semantic::types::infer::infer_scope_types::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
 231: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::Configuration_>::execute_query<ty_python_semantic::types::infer::infer_scope_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
 232: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_scope_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:136
 233: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::Configuration_>::execute<ty_python_semantic::types::infer::infer_scope_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:87
 234: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_scope_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
 235: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_scope_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
 236: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference> >,salsa::function::fetch::impl$0::refresh_memo::c
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
 237: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
 238: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::Configuration_>::fetch<ty_python_semantic::types::infer::infer_scope_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
 239: ty_python_semantic::types::infer::infer_scope_types::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:365
 240: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::infer::TypeInference>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
 241: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
 242: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scop
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
 243: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_ty
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
 244: salsa::attach::attach<ref$<ty_python_semantic::types::infer::TypeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
 245: ty_python_semantic::types::infer::infer_scope_types
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
 246: ty_python_semantic::types::check_types::impl$1::execute::inner_
             at C:\Users\sbym8\GitHub\ruff\crates\ty_python_semantic\src\types.rs:92
 247: ty_python_semantic::types::check_types::impl$1::execute
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:207
 248: salsa::function::IngredientImpl<ty_python_semantic::types::check_types::Configuration_>::execute_query<ty_python_semantic::types::check_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:271
 249: salsa::function::IngredientImpl<ty_python_semantic::types::check_types::Configuration_>::execute<ty_python_semantic::types::check_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\execute.rs:44
 250: salsa::function::IngredientImpl<ty_python_semantic::types::check_types::Configuration_>::fetch_cold<ty_python_semantic::types::check_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:196
 251: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::check_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:47
 252: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >,salsa::function::fetch:
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1552
 253: salsa::function::IngredientImpl<ty_python_semantic::types::check_types::Configuration_>::refresh_memo
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:45
 254: salsa::function::IngredientImpl<ty_python_semantic::types::check_types::Configuration_>::fetch<ty_python_semantic::types::check_types::Configuration_>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\function\fetch.rs:18
 255: ty_python_semantic::types::check_types::closure$0
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:365
 256: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics>,ty_python_semantic::types::check_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:89
 257: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::check_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:113
 258: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::check
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:311
 259: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::check_typ
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:275
 260: salsa::attach::attach<ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::check_types::closure_env$0>
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\attach.rs:111
 261: ty_python_semantic::types::check_types
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\components\salsa-macro-rules\src\setup_tracked_fn.rs:357
 262: ty_project::check_file_impl::closure$2
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\lib.rs:472
 263: std::panicking::try::do_call<ty_project::check_file_impl::closure_env$2,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:589
 264: salsa::cancelled::Cancelled::catch<ty_project::db::impl$0::with_db::closure_env$0<ty_project::db::impl$0::check_file::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,allo
 265: std::panicking::try
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:552
 266: std::panic::catch_unwind
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 267: salsa::cancelled::Cancelled::catch<ty_project::check_file_impl::closure_env$2,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\cancelled.rs:34
 268: ty_project::catch::closure$0<ty_project::check_file_impl::closure_env$2,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\lib.rs:583
 269: std::panicking::try::do_call<ty_project::catch::closure_env$0<ty_project::check_file_impl::closure_env$2,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >,enum2$<core::option::Option<ref$<ty_python_semantic::types::diagnostic::TypeCheckDi
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:589
 270: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<rayon_core::job::impl$9::call::closure_env$0<tuple$<>,rayon_core::registry::impl$6::in_worker_cross::closure_env$0<rayon_core::scope::scope::closure_env$0<ty_project::impl$14::check::closu
 271: std::panicking::try
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:552
 272: std::panic::catch_unwind<ty_project::catch::closure_env$0<ty_project::check_file_impl::closure_env$2,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >,enum2$<core::option::Option<ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagno
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 273: ruff_db::panic::catch_unwind<ty_project::catch::closure_env$0<ty_project::check_file_impl::closure_env$2,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >,enum2$<core::option::Option<ref$<ty_python_semantic::types::diagnostic::TypeCheckDi
             at C:\Users\sbym8\GitHub\ruff\crates\ruff_db\src\panic.rs:112
 274: ty_project::catch<ty_project::check_file_impl::closure_env$2,ref$<ty_python_semantic::types::diagnostic::TypeCheckDiagnostics> >
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\lib.rs:581
 275: ty_project::check_file_impl
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\lib.rs:472
 276: ty_project::impl$14::check::closure$0::closure$0
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\lib.rs:232
 277: rayon_core::scope::impl$0::spawn::closure$0::closure$0<ty_project::impl$14::check::closure$0::closure_env$0>
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\scope\mod.rs:526
 278: core::panic::unwind_safe::impl$25::call_once<tuple$<>,rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$14::check::closure$0::closure_env$0> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panic\unwind_safe.rs:272
 279: std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$14::check::closure$0::closure_env$0> >,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:589
 280: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<rayon_core::job::impl$9::call::closure_env$0<tuple$<>,rayon_core::registry::impl$6::in_worker_cross::closure_env$0<rayon_core::scope::scope::closure_env$0<ty_project::impl$14::check::closu
 281: std::panicking::try
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:552
 282: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$14::check::closure$0::closure_env$0> >,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 283: rayon_core::unwind::halt_unwinding<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$14::check::closure$0::closure_env$0>,tuple$<> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\unwind.rs:17
 284: rayon_core::scope::ScopeBase::execute_job_closure<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$14::check::closure$0::closure_env$0>,tuple$<> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\scope\mod.rs:689
 285: rayon_core::scope::ScopeBase::execute_job<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$14::check::closure$0::closure_env$0> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\scope\mod.rs:679
 286: rayon_core::scope::impl$0::spawn::closure$0<ty_project::impl$14::check::closure$0::closure_env$0>
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\scope\mod.rs:526
 287: rayon_core::job::impl$6::execute<rayon_core::scope::impl$0::spawn::closure_env$0<ty_project::impl$14::check::closure$0::closure_env$0> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\job.rs:169
 288: rayon_core::job::JobRef::execute
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\job.rs:64
 289: rayon_core::registry::WorkerThread::execute
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:860
 290: rayon_core::registry::WorkerThread::wait_until_cold
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:786
 291: rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::CoreLatch>
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:769
 292: rayon_core::latch::CountLatch::wait
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\latch.rs:400
 293: rayon_core::scope::ScopeBase::complete<rayon_core::scope::scope::closure$0::closure_env$0<ty_project::impl$14::check::closure_env$0,tuple$<> >,tuple$<> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\scope\mod.rs:668
 294: rayon_core::scope::scope::closure$0<ty_project::impl$14::check::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\scope\mod.rs:291
 295: rayon_core::registry::in_worker<rayon_core::scope::scope::closure_env$0<ty_project::impl$14::check::closure_env$0,tuple$<> >,tuple$<> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:951
 296: rayon_core::scope::scope<ty_project::impl$14::check::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\scope\mod.rs:289
 297: ty_project::Project::check
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\lib.rs:224
 298: ty_project::db::impl$0::check_with_reporter::closure$0
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\db.rs:83
 299: ty_project::db::impl$0::with_db::closure$0<ty_project::db::impl$0::check_with_reporter::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\db.rs:106
 300: std::panicking::try::do_call<ty_project::db::impl$0::with_db::closure_env$0<ty_project::db::impl$0::check_with_reporter::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,a
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:589
 301: salsa::cancelled::Cancelled::catch<ty_project::db::impl$0::with_db::closure_env$0<ty_project::db::impl$0::check_file::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,allo
 302: std::panicking::try
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:552
 303: std::panic::catch_unwind
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 304: salsa::cancelled::Cancelled::catch<ty_project::db::impl$0::with_db::closure_env$0<ty_project::db::impl$0::check_with_reporter::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,alloc::vec::Vec<ruff_db::diagnostic::Diagno
             at C:\Users\sbym8\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\7edce6e\src\cancelled.rs:34
 305: ty_project::db::ProjectDatabase::with_db<ty_project::db::impl$0::check_with_reporter::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\db.rs:106
 306: ty_project::db::ProjectDatabase::check_with_reporter
             at C:\Users\sbym8\GitHub\ruff\crates\ty_project\src\db.rs:83
 307: ty::impl$1::main_loop::closure$0<ty::IndicatifReporter>
             at C:\Users\sbym8\GitHub\ruff\crates\ty\src\lib.rs:251
 308: core::panic::unwind_safe::impl$25::call_once<tuple$<>,ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panic\unwind_safe.rs:272
 309: std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter> >,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:589
 310: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter> >,tuple$<> >
 311: std::panicking::try
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:552
 312: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter> >,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 313: rayon_core::unwind::halt_unwinding<ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter>,tuple$<> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\unwind.rs:17
 314: rayon_core::registry::Registry::catch_unwind<ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:367
 315: rayon_core::spawn::spawn_job::closure$0<ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter> >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\spawn\mod.rs:97
 316: rayon_core::job::impl$6::execute<rayon_core::spawn::spawn_job::closure_env$0<ty::impl$1::main_loop::closure_env$0<ty::IndicatifReporter> > >
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\job.rs:169
 317: rayon_core::job::JobRef::execute
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\job.rs:64
 318: rayon_core::registry::WorkerThread::execute
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:860
 319: rayon_core::registry::WorkerThread::wait_until_cold
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:794
 320: rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::OnceLatch>
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:769
 321: rayon_core::registry::WorkerThread::wait_until_out_of_work
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:818
 322: rayon_core::registry::main_loop
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:923
 323: rayon_core::registry::ThreadBuilder::run
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:53
 324: rayon_core::registry::impl$2::spawn::closure$0
             at C:\Users\sbym8\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.12.1\src\registry.rs:98
 325: core::hint::black_box
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\hint.rs:477
 326: std::sys::backtrace::__rust_begin_short_backtrace<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\sys\backtrace.rs:152
 327: std::thread::impl$0::spawn_unchecked_::closure$1::closure$0<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\mod.rs:559
 328: core::panic::unwind_safe::impl$25::call_once<tuple$<>,std::thread::impl$0::spawn_unchecked_::closure$1::closure_env$0<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> > >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panic\unwind_safe.rs:272
 329: std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::impl$0::spawn_unchecked_::closure$1::closure_env$0<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> > >,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:589
 330: crossbeam_deque::deque::impl$17::default<rayon_core::job::JobRef>
 331: std::panicking::try
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:552
 332: std::panic::catch_unwind
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 333: std::thread::impl$0::spawn_unchecked_::closure$1<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\mod.rs:557
 334: core::ops::function::FnOnce::call_once<std::thread::impl$0::spawn_unchecked_::closure_env$1<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >,tuple$<> >
             at C:\Users\sbym8\.rustup\toolchains\1.87-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:250
 335: alloc::boxed::impl$28::call_once
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\alloc\src\boxed.rs:1966
 336: alloc::boxed::impl$28::call_once
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\alloc\src\boxed.rs:1966
 337: std::sys::pal::windows::thread::impl$0::new::thread_start
             at /rustc/17067e9ac6d7ecb70e50f92c1944e545188d2359/library\std\src\sys\pal\windows\thread.rs:56
 338: BaseThreadInitThunk
 339: RtlUserThreadStart

info: query stacktrace:
   0: all_negative_narrowing_constraints_for_expression(Id(1801))
             at crates\ty_python_semantic\src\types\narrow.rs:90
             cycle heads: infer_expression_types(Id(1800)) -> 0
   1: infer_expression_types(Id(1802))
             at crates\ty_python_semantic\src\types\infer.rs:223
             cycle heads: infer_expression_type(Id(1800)) -> 0, infer_expression_types(Id(1800)) -> 0
   2: infer_expression_type(Id(1802))
             at crates\ty_python_semantic\src\types\infer.rs:279
   3: member_lookup_with_policy_(Id(4009))
             at crates\ty_python_semantic\src\types.rs:534
             cycle heads: infer_expression_type(Id(1800)) -> 0, infer_expression_types(Id(1800)) -> 0
   4: infer_expression_types(Id(1800))
             at crates\ty_python_semantic\src\types\infer.rs:223
   5: infer_expression_type(Id(1800))
             at crates\ty_python_semantic\src\types\infer.rs:279
   6: member_lookup_with_policy_(Id(4007))
             at crates\ty_python_semantic\src\types.rs:534
   7: infer_scope_types(Id(1000))
             at crates\ty_python_semantic\src\types\infer.rs:122
   8: check_types(Id(c00))
             at crates\ty_python_semantic\src\types.rs:82


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

</details>

`infer_expression_types` called in `evaluate_bool_op` returns an incomplete value. The inference results of sub-expressions are not stored. I think this is because a query cycle is formed. If I understand correctly, a function (caller) that calls a tracked function (callee) must take into account that the tracked function may return an incomplete value that has not reached a fixed point if the caller is included in the call graph of the tracked function.


---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:226 on 2025-05-18 16:46_

See the review comment of `narrow.rs`.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:5524 on 2025-05-18 16:53_

Handling of `infer_name_load` has been generalized to `infer_place_load`, and type narrowing can now be performed on `infer_attribute_load` and `infer_subscript_load` as well.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1940 on 2025-05-18 17:02_

The handling of adding definitions, which was previously done separately for name and instance attributes, has been integrated into this section.
In addition, definitions for attributes and subscripts are now also recorded.

---

_Comment by @mtshiba on 2025-05-18 17:14_

For now, the planned work has been completed, so I will mark the PR as ready for review.
There are many changed files, but most of them are the result of renaming, and I have noted the particularly important changes in the review comments.

---

_@mtshiba reviewed on 2025-05-18 17:14_

---

_Marked ready for review by @mtshiba on 2025-05-18 17:14_

---

_Review requested from @carljm by @mtshiba on 2025-05-18 17:14_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-05-18 17:14_

---

_Review requested from @sharkdp by @mtshiba on 2025-05-18 17:14_

---

_Review requested from @dcreager by @mtshiba on 2025-05-18 17:14_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1931 on 2025-05-18 18:16_

See the new test case of https://github.com/mtshiba/ruff/blob/narrow_by_assignment/crates/ty_python_semantic/resources/mdtest/attributes.md#attributes-defined-in-comprehensions.

This is a minor follow-up to #17396. It requires a more involved mechanism to fully solve the issue, but since it only matters for unusual code, I don't think it's a high priority to implement.

---

_@mtshiba reviewed on 2025-05-18 18:29_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:5823 on 2025-05-20 04:19_

See https://github.com/mtshiba/ruff/blob/narrow_by_assignment/crates/ty_python_semantic/resources/mdtest/narrow/assignment.md#attribute for why narrowing should not be performed in these cases.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:7229 on 2025-05-20 04:24_

See https://github.com/mtshiba/ruff/blob/narrow_by_assignment/crates/ty_python_semantic/resources/mdtest/narrow/assignment.md#subscript for why narrowing should not be performed in these cases.

Perhaps other classes can be considered safe, but we cannot expect that all standard library types will be soundly implemented (e.g. `email.message.EmailMessage`).

---

_@mtshiba reviewed on 2025-05-20 04:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:69 on 2025-05-20 09:59_

```suggestion
# Strictly speaking, inferring this as `Literal[False]` rather than `bool` is unsound in general
# (we don't know what else happened to `c_instance` between the assignment and the use here),
# but mypy and pyright support this.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:262 on 2025-05-20 10:02_

This seems like a regression, do you have an idea why is this happening? Should we keep the original TODO comment?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:23 on 2025-05-20 10:07_

Maybe:
```suggestion

# Make sure that we infer the narrowed type for eager
# scopes (class, comprehension) and the non-narrowed
# public type for lazy scopes (function)
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:71 on 2025-05-20 10:12_

Maybe something like this, to make it a bit more clear?

```suggestion
arbitrary `__getitem__`/`__setitem__` methods on a class do not necessarily guarantee that the passed-in value
for `__setitem__` is stored and can be retrieved unmodified via `__getitem__`. Therefore, we currently only perform assignment-based narrowing on a few
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:67 on 2025-05-20 10:14_

Maybe

```suggestion
### Specialization for builtin types
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:132 on 2025-05-20 10:15_

Maybe
```suggestion
### No narrowing for custom classes with arbitrary `__getitem__` / `__setitem__`
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:144 on 2025-05-20 10:23_

Not that it matters too much, but maybe
```suggestion
        if len(self.l) == index:
            self.l.append(str(value))
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:54 on 2025-05-20 10:30_

I don't feel strongly about the particular name we use here, but a note for the future: it would have been easier for reviewers if this naming change would be a follow-up PR. It's hard to see the actual changes through all the diff noise. And it would have been potentially easier for you to discuss this first on Discord (in case we decide this is not the best name).

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:195 on 2025-05-20 10:34_

*If* we decide to rename `Symbol` -> `Place`, we should also go through doc comments (maybe search for the regex `///.*symbol`) and rename accordingly. This is just one instance where the text still includes references to "symbol" instead of "place".

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:674 on 2025-05-20 10:38_

Maybe consider adding a `is_name_and(|name| …)` method (similar to `Option::is_some_and`) that could be used here and in at least one other place?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:774 on 2025-05-20 10:39_

Does this mean that we fall back to `Place::Unbound` if one of the segments is possibly unbound?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:786 on 2025-05-20 10:47_

What exactly does it mean for those to be TODOs?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/primer/bad.txt`:4 on 2025-05-20 11:34_

Maybe run with `TY_MAX_PARALLELISM` and see if you can get a deterministic panic message to add here? Similar for bokeh and sympy.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/place.rs`:39 on 2025-05-20 11:41_

Maybe "target", because the assignment target not always on the left (e.g. in a `with` statement)?
```suggestion
/// An expression that can be the target of a `Definition`.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/place.rs`:27 on 2025-05-20 11:43_

This is maybe a bit pedantic, but would this be slightly clearer (if it's correct)?
```suggestion
    /// A member access, e.g. `.y` in `x.y`
    Member(ast::name::Name),
    /// An integer-based index access, e.g. `[1]` in `x[1]`
    IntSubscript(ast::Int),
    /// A string-based index access, e.g. `["foo"]` in `x["foo"]`
    StringSubscript(String),
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/place.rs`:111 on 2025-05-20 11:48_

Maybe
```suggestion
            ast::Expr::NumberLiteral(ast::ExprNumberLiteral {
                value: ast::Number::Int(index),
                ..
            }) => {
                place
                    .sub_segments
                    .push(PlaceExprSubSegment::IntSubscript(index.clone()));
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/place.rs`:198 on 2025-05-20 11:51_

Why is the `.unwrap()` safe here?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/place.rs`:191 on 2025-05-20 11:52_

```suggestion
        self.is_instance_attribute()
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/place.rs`:256 on 2025-05-20 11:54_

Maybe add a comment here that this will only ever be the first segment?

---

_@sharkdp reviewed on 2025-05-20 11:57_

Thank you very much, this looks great!!

I am submitting a first batch of review comments, as it will probably take me some more time to get through the rest, and it's already getting late where you live :-)

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/definition.rs`:687 on 2025-05-20 12:11_

Can you comment on why this change was necessary? It's true that we consider declared-but-not-bound attributes on class bodies as *definitely bound* instance attributes:
```py
class C:
    x: int

C().x  # definitely bound
```

But I'm not sure why it's necessary to consider `x: int` both a declaration and a binding when it seems apparent that it is just a declaration.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/definition.rs`:772 on 2025-05-20 12:12_

```suggestion
/// it is in the scope in which the root variable is bound.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:17 on 2025-05-20 12:13_

Similar here, maybe:
```suggestion
//!   and syntactically, an expression that can be the target of an assignment,
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:951 on 2025-05-20 12:14_

This is just one tiny example, but I really like that your PR leads to *simplifications* in a lot of places — very nice!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/narrow.rs`:626 on 2025-05-20 12:20_

Thank you for explaining this in detail. I do not think that (correct) fixpoint iteration should lead us to a state where some intermediate expressions do not have an attached type. This is most probably not related to your PR, but I think it would be good to see if adding that snippet to the corpus (`crates/ty_project/resources/test/corpus/`) causes any problems.

The changes here and below should ideally be reverted, I think (eventually)?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:226 on 2025-05-20 12:21_

See my other comment, I think we should get to the bottom of this. And then we can hopefully remove this again.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1492 on 2025-05-20 12:22_

Similar to my other comment: I would like to understand in what kind of situations this code kicks in. Can you give an example?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:3661 on 2025-05-20 12:25_

I assume you removed this because `target` can now be a non-name at this point? If so, that's problematic. It means that we can't simply use `self.store_expression_type(target, …)` below, as we might fail to store types on intermediate expressions. Maybe this is related to the panic that you saw … the changes in `narrow.rs`?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:4627 on 2025-05-20 12:26_

Should we use a positive test for names here? (in case the list of possibilities will ever be extended? or is that not possble?)

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:5666 on 2025-05-20 12:28_

Should this still be the doc comment for `infer_name_load`?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:5823 on 2025-05-20 12:53_

Checking for descriptor attributes here definitely makes sense, thank you!

The performed check here seems a bit "ad hoc", especially that we need to check `property` separately (those are also descriptor attributes, so ideally, checking for descriptors should be enough).

I think it also runs into problems if `member` is a union type and just one of the elements is a data descriptor. The reason for this is that `try_call_dunder_get_on_attribute` uses an `.all`  (instead of `.any`) logic for checking whether a union is a data descriptor. But here, we should probably use the `.any` logic: if *any* of the union elements is a descriptor, do not perform the narrowing.

I think we may want to create a new function like `Type::is_data_descriptor` or similar that has the right behavior for unions and intersections? If the check above works well enough, I'm inclined to say this could also be a follow-up task.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:7214 on 2025-05-20 12:53_

Same suggestion here in terms of wording.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:7228 on 2025-05-20 12:55_

Is `is_assignable_to` enough here? What if I subclass any of these types and add special `__getitem__` or `__setitem__` methods? Or would that be a LSP violation?

---

_@sharkdp reviewed on 2025-05-20 12:57_

… and here is the second batch of review comments.

Again — thank you very much! I really like how this simplifies / unifies the code in various places. And I'm obviously looking forward to the follow-up that adds full narrowing on attribute/subscript assignments.

---

_@mtshiba reviewed on 2025-05-20 17:06_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/place.rs`:54 on 2025-05-20 17:06_

Yeah, this was my carelessness. I'll be more careful from the next PR onwards.

As a temporary measure, I created a dummy PR that doesn't rename `Symbol`, so if you want to see the diff that doesn't include refactoring, please refer to it.

https://github.com/mtshiba/ruff/pull/1

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:262 on 2025-05-20 20:04_

Also curious why this happens, but the TODO comment as edited still seems to capture the desirable behavior here. (Also I'm not sure that this is a regression; `Weird` is not a possible type for the `w` attribute, so I would say inferring `Unknown` is strictly better than inferring `Unknown | Weird`, though `Unknown | str` would be even better.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:1 on 2025-05-20 20:20_

Here is a test case we should include, in some form:

```py
class C:
    attr: str | None

c = C()
reveal_type(c.attr)  # str | None
c.attr = "foo"
reveal_type(c.attr)  # Literal["foo"]
c = C()
reveal_type(c.attr)  # should be str | None, not Literal["foo"]
```

Currently in this PR the last line reveals `Literal["foo"]`, but it should reveal `str | None`. Re-definitions of a name/symbol need to somehow "reset" definitions of attributes/subscripts of that name. (And this must also apply recursively to expressions; e.g. a re-assignment of `c.x` must "clear" any narrowing of `c.x[0]`, etc.)

I haven't reviewed enough of this PR yet to have a sense of how difficult that will be to add. If it's very easy, maybe we should do it in this PR. If it's doable but not a small change, maybe the test should be added with a TODO, and then addressed in a follow-up PR. If it's not clear how it can be integrated into this approach (that would surprise me), then we may need to reconsider the approach?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:105 on 2025-05-20 20:31_

I know it feels repetitive, but I think we should aim to make each TODO comment clear in isolation; makes the "fix" PR easier to review.
```suggestion
    # TODO: should be Literal[0]
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:112 on 2025-05-20 20:35_

```suggestion
# TODO: should be Literal[0]
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:54 on 2025-05-20 21:16_

I think this rename is fine and I don't mind the name `Place`.

I think it is important that we have a clear understanding of the semantics of `Boundness` on a `Place`. If we have a `Place` that says `x.a` is `Unbound`, does that mean we have actually checked `x` and done member lookup on it, or just that we have no binding for `x.a` directly? (Not clear to me yet.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:290 on 2025-05-20 21:54_

Why does this newly require this annotation in this PR? I did a textual search and wasn't quickly able to find a non-test usage of it that was removed.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:771 on 2025-05-20 22:17_

It's not clear to me how this `fallback_place` integrates correctly with `TypeInferenceBuilder`. It seems that wherever we use this we might either repeat work (e.g. call `Type::member` multiple times for the same attribute expression, once in `fallback_place` and once in `TypeInferenceBuilder`), or fail to store a type for sub-expressions in `TypeInferenceBuilder` (if we only infer those types here.) I'm not sure which of those is currently happening (or neither because I've misunderstood another option). If the latter were happening I'd expect that to cause "missing type for expression" corpus panics.

---

_@carljm reviewed on 2025-05-21 02:40_

I haven't finished review yet. The core idea of this PR looks quite good to me. Still some questions on some details. Will finish review in the morning.

---

_@mtshiba reviewed on 2025-05-21 18:03_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:1 on 2025-05-21 18:03_

I thought that when the name `c` is shadowed, we could assume that the definitions for `c.attr`, etc. are also shadowed together, but handling in different scopes is more complicated.

```python
c = C()
c.attr = "foo"

class D:
    if False:
        c = C()
    reveal_type(c.attr)  # revealed: Literal["foo"]

    if True: # or ambiguous
        c = C()
    reveal_type(c.attr)  # revealed: str | None
```

Since there is no definition of `c.attr` in scope `D`, we look at the definition in the outer scope. However, whether the definition of `c.attr` (of the global scope) is visible actually depends on whether the definition of `c` in scope `D` is visible. Should `bindings_by_use` additionally hold this information?

---

_@carljm reviewed on 2025-05-21 18:16_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:1 on 2025-05-21 18:16_

Do we need additional information in `bindings_by_use` to handle this? Or do we instead need to adjust the order of lookups, so that if there is no visible binding for `c.attr` in the local scope, we look for a binding of `c` in the local scope, and only if that is possibly unbound do we climb up to the enclosing scope to find a binding of `c.attr` or `c`. That seems like the lookup order that better reflects the correct semantics. If `c` is bound locally we don't care about the type of `c.attr` in an enclosing scope.

---

_@mtshiba reviewed on 2025-05-23 00:56_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/place.rs`:54 on 2025-05-23 00:56_

`SemanticIndexBuilder` simply records all assignments to place expressions in the scope (considering the existence of unbound binding, even references without assignments will be recorded), and whether or not they are valid assignments is a problem that is resolved during type inference (Added this explanation to the `use_def.rs` documentation).
For example, if there is an assignment to place expression `x.y.z`, even if `x` or `x.y` are unresolved, the information that there was an assignment is recorded in `SemanticIndex`. Also, even if there is no definition of `x.y.z` within this scope, the instance may already have that attribute. In this case, it falls back to the handling of the member lookup using the `x.y` type.
Currently, our purpose in recording these is limited to type narrowing, so if `x` is clearly not in the scope, the assignment to `x.y.z` is invalid and does not need to be recorded. However, to achieve this kind of omitting, we need to reimplement the place lookup handling (done by `TypeInferenceBuilder`) in `SemanticIndexBuilder`. 

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/primer/bad.txt`:4 on 2025-05-23 01:10_

Checking sympy works fine in a single thread, but setting `TY_MAX_PARALLELISM=1` still did not solve the problem of arviz ~~and bokeh~~ inspections not stopping immediately, so it seems that the problem is not related to multithreading. On the other hand, when I removed pandas from the arviz and bokeh dependencies and ran it locally, the errors occurred immediately, so I think that pandas is the cause.

edit: I ran it again and bokeh was checked successfully (I realized this was because mypy_primer had not installed pandas as a dependency when checking bokeh. Strictly speaking this is not good, but the failure of the pandas check is a known issue, so this is OK?), but arviz still did not work.

---

_@mtshiba reviewed on 2025-05-23 01:10_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/attributes.md`:262 on 2025-05-23 07:57_

Yes, I have the same opinion as Carl. I've added a comment.

---

_@mtshiba reviewed on 2025-05-23 07:57_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/definition.rs`:687 on 2025-05-23 08:43_

What I had in mind is code like below, which doesn't give an error in mypy or pyright:

```python
class C:
    def __init__(self):
        self.x: int

c = C()
c.x = 1  # OK
reveal_type(c.x)  # Literal[1]
```

However, without this part, ty will give the following error:

```python
error[unresolved-attribute]: Unresolved attribute `x` on type `C`.
 --> attr.py:8:1
  |
7 | c = C()
8 | c.x = 1
  | ^^^
9 | reveal_type(c.x)
  |
info: rule `unresolved-attribute` is enabled by default

info[revealed-type]: Revealed type
 --> attr.py:9:13
  |
7 | c = C()
8 | c.x = 1
9 | reveal_type(c.x)
  |             ^^^ `Literal[1]`
  |

Found 2 diagnostics
```

However, I began to think that it might be better to handle it at the type inference phase, rather than marking it as definitely bound at the definition phase, and to make it OK for an assignment like `c.x = 1` if there is a type declaration for `x` (and then treat it as bound).

---

_@mtshiba reviewed on 2025-05-23 08:43_

---

_@mtshiba reviewed on 2025-05-24 12:03_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/narrow.rs`:626 on 2025-05-24 12:03_

I investigated the cause of this again and it seems that the bug is no longer manifesting.

I did a bisection checking to find out the part that removed the problem, and it seems that merge commit 9c8ed0407ce719a38947f978331e5fd5ac68bb73 was the cause.

This reflects the changes in PR https://github.com/astral-sh/ruff/pull/18150, which reverts a `SemanticIndexBuilder` regression.
In the end, the cause of the bug is still unclear, but for now I add a regression test and consider this issue resolved.

---

_Review requested from @MichaReiser by @mtshiba on 2025-05-24 12:10_

---

_@mtshiba reviewed on 2025-05-24 12:38_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/place.rs`:290 on 2025-05-24 12:38_

`symbol` is used in https://github.com/mtshiba/ruff/blob/ec5166840db35085852feb3e8e9fe3604ec48073/crates/ty_python_semantic/src/types/infer.rs#L9337.
Currently it is only used in tests, so would it be better to move it into the test module?

---

_@mtshiba reviewed on 2025-05-24 16:42_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:7228 on 2025-05-24 16:42_

Yes, it is true that a subclass that does not hold the properties of its superclass - in this case, the property that assignment-based narrowing can be performed - violates LSP, but I realized that this might not be caught by a type system.

```python
class BadList(list[int]):
    def __setitem__(self, index: int, value: int) -> None:
        super().__setitem__(index, -value)  # LSP violation!

    def __getitem__(self, index: int) -> int:
        return super().__getitem__(index)

l = BadList([0])
l[0] = 1
x = l[0]
reveal_type(x)  # Literal[1]
print(x)  # -1
```

ty does not yet implement type checking for method overriding, but even if it were implemented, it would not be able to detect this kind of inconsistency. `BadList` overrides the `list` methods without any problem in terms of signature types (although the actual signature is a bit more complicated), but the behavior expected by our type system is different from the behavior at runtime.

If we want to perform narrowing in subclasses properly, we will need to impose an additional condition, such as not overriding `__getitem__/__setitem__` (I think it's probably okay if we impose this condition, but I'm not yet sure if it's completely sound). However, I think that this is a relatively low-priority task.
For now, I will change it so that subclasses are not considered safe.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:3661 on 2025-05-25 16:19_

Yes, the reason I removed this assertion is as you explain, but I don't think this part is problematic.

In the handling of `infer_annotated_assignment_statement`, the target is inferred as a normal expression unless target is a name (that has no sub-expressions). Therefore, `infer_annotated_assignment_definition` does not need to infer the sub-expressions of the target. (By the way, there seems to be a problem with the current implementation of `infer_annotated_assignment_statement`. I reported it in astral-sh/ty#509.)

Also, as I already mentioned in [my reply](https://github.com/astral-sh/ruff/pull/18041#discussion_r2105798791), the strange bug of `narrow.rs` has not been confirmed in the latest commit.

---

_@mtshiba reviewed on 2025-05-25 16:19_

---

_Comment by @codspeed-hq[bot] on 2025-05-26 16:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Anarrow_by_assignment)

### Merging #18041 will **degrade performances by 5.31%**

<sub>Comparing <code>mtshiba:narrow_by_assignment</code> (bc72edb) with <code>main</code> (ce8b744)</sub>



### Summary

`❌ 2` regressions  
`✅ 32` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Anarrow_by_assignment)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_check_file[cold] `` | 109.8 ms | 115.9 ms | -5.31% |
| ❌ | `` ty_micro[many_string_assignments] `` | 62.7 ms | 66.1 ms | -5.14% |


---

_@mtshiba reviewed on 2025-05-30 14:32_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:1492 on 2025-05-30 14:32_

This part is needed to prevent the overwriting of attribute/subscript types by invalid assignments:

```python
class C:
    x: int

def f(c: C, s: str):
    c.x = s # error: [invalid-assignment]
    reveal_type(c.x) # revealed: int
```

`TypeInferenceBuilder::add_binding` detects this kind of type inconsistency when registering a binding and corrects the type of the binding to the declared type. However, without `fallback_place`, the type of `c.x` cannot be corrected.
We cannot get the type of `c.x` using `place_from_declarations` or `place`. This is because there is no (direct) type declaration for `c.x`, and if we try to get the type of the target `c.x` when registering the binding, a cycle will occur and the resulting type will be wrong (previously, `add_binding` did not handle bindings like `c.x = s`, so this problem did not arise).

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/primer/good.txt`:23 on 2025-05-31 00:43_

nit: these are alphabetized, it should go before `boostedblob` as it is before this PR

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/definition.rs`:687 on 2025-05-31 13:48_

Yes, I'd like to revert this change, add some TODOs in the tests, and consider this case in a separate PR. Handling it in type inference sounds better to me than mis-categorizing the declaration.

(I've already made that change locally to verify it works, will push it shortly.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:290 on 2025-05-31 13:52_

I think marking it as allow-unused for now is fine, we can consider its future as a separate follow-up.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:771 on 2025-05-31 15:08_

Ok, on looking at this again I see that there's actually only one use of `fallback_place`, and it's where we look up the declared type when adding a binding. So I think this comment is not relevant.

I still have some questions about the naming and use of `fallback_place_impl`, and generally how we handle looking up declarations. I don't think we should allow declarations directly on an attribute or subscript expression (other than direct attributes of `self` or maybe `cls`), so I'm not sure we should ever be looking up declarations on a non-Name Place? As in, it seems like maybe we should always use "fallback" for the declarations case?

But I would like to get this merged, and given that the state of the tests and mypy-primer looks pretty good, I'm happy to defer the questions around this method to a follow-up.

---

_@carljm reviewed on 2025-05-31 15:20_

---

_Comment by @carljm on 2025-05-31 15:22_

I still want to look more closely to understand the new `DefinitionState` stuff that was added...

---

_Comment by @mtshiba on 2025-05-31 16:06_

> I still want to look more closely to understand the new `DefinitionState` stuff that was added...

In order to clear the "associated definitions" such as `c.x = ...` when a variable is redefined, I've added the state `DefinitionState::Deleted`.
The following explanation is for the single scope.

```python
c = C()
c.x = ...
c.x.y = ...

if cond:
    c = C() # del c.x, c.x.y

reveal_type(c.x) # c.x = ... is possibly deleted, don't narrow
```

When `c` is redefined, the previous definition is shadowed. We would like `c.x = ...` and others to be shadowed at the same time, but there is a problem that the new definition does not exist. Therefore, when `c` is redefined, the definitions associated with the previous definition such as `c.x = ...` must be in "deleted" states. This is expressed by `DefinitionState::Deleted`.
When a definition in this state is visible, the place is deleted. Narrowing is not performed, and it falls back to normal attribute resolution / subscript type inference. `DefinitionState::Undefined` is the old equivalent of `Option<Definition>::None`. It always points to the "unbound binding", while `DefinitionState::Deleted` points to an intermediate deleted definition.

Perhaps `DefinitionState::Deleted` could be used to [support del statement](https://github.com/astral-sh/ty/issues/238), which is not implemented yet. That is obviously a future work, though.

---

_@mtshiba reviewed on 2025-05-31 17:14_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/place.rs`:771 on 2025-05-31 17:14_

Yes, as [commented here](https://github.com/astral-sh/ruff/pull/18041#discussion_r2116044304), `fallback_place` is only used in this part and it doesn't seem like a very elegant way.
I thought the problem was that there was no way to get the target expression directly here and get its type, but then I realized that `node` is exactly the target expression. So, `fallback_place` is no longer necessary.

---

_@mtshiba reviewed on 2025-05-31 17:42_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/place.rs`:198 on 2025-05-31 17:42_

If it is marked with `IS_INSTANCE_ATTRIBUTE`, it is an instance attribute and passes the `place_expr.is_member()` check.

https://github.com/mtshiba/ruff/blob/narrow_by_assignment/crates/ty_python_semantic/src/semantic_index/builder.rs#L1964-L1966

---

_@mtshiba reviewed on 2025-06-03 14:26_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/primer/bad.txt`:17 on 2025-06-03 14:26_

The `schemathesis` check seems to panic due to the changes in this PR.

https://github.com/astral-sh/ruff/actions/runs/15408356886/job/43355304458?pr=18041

It still panics even in a single thread, and for some reason doesn't print any debug info like backtrace. In what cases can this happen?

---

_Review requested from @carljm by @mtshiba on 2025-06-03 14:30_

---

_Review requested from @sharkdp by @mtshiba on 2025-06-03 14:30_

---

_@carljm reviewed on 2025-06-03 17:29_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/primer/bad.txt`:17 on 2025-06-03 17:29_

Looking at this

---

_@carljm reviewed on 2025-06-03 22:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/primer/bad.txt`:17 on 2025-06-03 22:32_

It's a Salsa query cycle panic. There was a regression in our debug logging on those panics. I'm resolving that regression; likely we need cycle handling on an additional query.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/place.rs`:672 on 2025-06-04 02:18_

Is it a significant performance win to keep `add_symbol` and `PlaceTable::hash_name` separate from `add_place` and `PlaceTable::hash_place_expr`? Or is there another motivation? Because it seems like in principle we could always use `add_place`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:11 on 2025-06-04 05:17_

What do you mean by an "invalid place" here? Because from what I see in the code, we only record assignments to valid places (that is, those which can be parsed into a `PlaceExpr`).

---

_Comment by @carljm on 2025-06-04 05:26_

Locally I'm seeing a significant speedup (about 10%) on the cold benchmark from switching to a SmallVec for `PlaceExpr` sub-expressions. That doesn't seem to reproduce on CodSpeed, but I'm inclined to trust the local results; I don't think CodSpeed always reflects the impact of extra allocations accurately.

---

_@carljm reviewed on 2025-06-04 05:50_

Sorry that review here has taken so long! I know that resolving merge conflicts here has been a pain.

I think this is getting quite close. I want to take a closer look at the new `infer_*_load` logic in `TypeInferenceBuilder` with a clear head in the morning, and then I think we should merge.

This is really excellent work. Resolves a major issue, and does it with relatively small performance regression, and overall code simplification. Thank you!!

---

_Comment by @carljm on 2025-06-05 00:10_

I removed the added code in `SemanticIndexBuilder` to record some expressions in comprehensions in a different scope. I think this is not the right way to address this problem, and it can easily cause panics because we miss some expressions (the root Name as a use) being recorded in the right scope. I think it is fine to not support definition of instance attributes within a comprehension for now (this is a very strange pattern); when we add support we can give the problem more careful consideration as a separate PR.

---

_Comment by @carljm on 2025-06-05 00:24_

There are some more things I still want to explore and understand better here, but I don't think it's a net gain to keep this un-merged any longer; better to merge and iterate on it. Thank you for your work on it!

---

_Merged by @carljm on 2025-06-05 00:24_

---

_Closed by @carljm on 2025-06-05 00:24_

---

_Branch deleted on 2025-06-06 03:43_

---
