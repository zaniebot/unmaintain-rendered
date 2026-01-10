```yaml
number: 18750
title: "[ty] Infer nonlocal types as unions of all reachable bindings"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/public-types
created_at: 2025-06-18T13:17:42Z
updated_at: 2025-06-26T10:24:42Z
url: https://github.com/astral-sh/ruff/pull/18750
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Infer nonlocal types as unions of all reachable bindings

---

_Pull request opened by @sharkdp on 2025-06-18 13:17_

## Summary

This PR includes a behavioral change to how we infer types for nonlocal uses of symbols within a module. Where we would previously use the type that a use at the end of the scope would see, we now consider all reachable bindings and union the results:

```py
x = None

def f():
    reveal_type(x)  # previously `Unknown | Literal[1]`, now `Unknown | None | Literal[1]`

f()

x = 1

f()
```

This helps especially in cases where the the end of the scope is not reachable:

```py
def outer(x: int):
    def inner():
        reveal_type(x)  # previously `Unknown`, now `int`

    raise ValueError
```

This PR also proposes to skip the boundness analysis of nonlocal uses. This is consistent with the "all reachable bindings" strategy, because the implicit `x = <unbound>` binding is also always reachable, and we would have to emit "possibly-unresolved" diagnostics for every nonlocal use otherwise. Changing this behavior allows common use-cases like the following to type check without any errors:

```py
def outer(flag: bool):
    if flag:
        x = 1

        def inner():
            print(x)  # previously: possibly-unresolved-reference, now: no error
```

closes https://github.com/astral-sh/ty/issues/210
closes https://github.com/astral-sh/ty/issues/607
closes https://github.com/astral-sh/ty/issues/699

## Follow up

It is now possible to resolve the following TODO, but I would like to do that as a follow-up, because it requires some changes to how we treat implicit attribute assignments, which could result in ecosystem changes that I'd like to see separately.

https://github.com/astral-sh/ruff/blob/315fb0f3da4e5f2097a294c344025dec28ebf2a5/crates/ty_python_semantic/src/semantic_index/builder.rs#L1095-L1117

## Ecosystem analysis

[**Full report**](https://shark.fish/diff-public-types.html)

* This change obviously removes a lot of `possibly-unresolved-reference` diagnostics (7818) because we do not analyze boundness for nonlocal uses of symbols inside modules anymore.
* As the primary goal here, this change also removes a lot of false-positive `unresolved-reference` diagnostics (231) in scenarios like this:
    ```py
    def _(flag: bool):
        if flag:
            x = 1
    
            def inner():
                x
    
            raise
    ```
* This change also introduces some new false positives for cases like:
    ```py
    def _():
        x = None
    
        x = "test"
    
        def inner():
            x.upper()  # Attribute `upper` on type `Unknown | None | Literal["test"]` is possibly unbound
    ```
    We have test cases for these situations and it's plausible that we can improve this in a follow-up.


## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-06-18 13:17_

---

_Renamed from "David/public types" to "[ty] Infer public types as unions of all reachable bindings" by @sharkdp on 2025-06-18 13:18_

---

_Comment by @github-actions[bot] on 2025-06-18 13:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ warning[unused-ignore-comment] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:673:33: Unused `ty: ignore` directive: 'possibly-unresolved-reference'
+ warning[unused-ignore-comment] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:690:40: Unused `ty: ignore` directive: 'possibly-unresolved-reference'
- Found 183 diagnostics
+ Found 185 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[unresolved-reference] mypy_primer/main.py:175:67: Name `shard_costs` used when not defined
- Found 10 diagnostics
+ Found 9 diagnostics

git-revise (https://github.com/mystor/git-revise)
- warning[possibly-unresolved-reference] gitrevise/utils.py:102:34: Name `pat_is_comment_line` used when possibly not defined
- Found 2 diagnostics
+ Found 1 diagnostic

parso (https://github.com/davidhalter/parso)
- warning[possibly-unresolved-reference] parso/python/errors.py:323:36: Name `base_name` used when possibly not defined
- warning[possibly-unresolved-reference] parso/python/errors.py:323:58: Name `base_name` used when possibly not defined
- warning[possibly-unresolved-reference] parso/python/tokenize.py:385:43: Name `spos` used when possibly not defined
- warning[possibly-unresolved-reference] parso/python/tree.py:422:31: Name `dct` used when possibly not defined
- warning[possibly-unresolved-reference] parso/python/tree.py:426:25: Name `recurse` used when possibly not defined
- Found 77 diagnostics
+ Found 72 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- error[unresolved-reference] pyp.py:652:24: Name `line_to_node` used when not defined
- Found 9 diagnostics
+ Found 8 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- warning[possibly-unresolved-reference] chess/engine.py:962:25: Name `cmd` used when possibly not defined
- Found 20 diagnostics
+ Found 19 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- warning[possibly-unresolved-reference] paroxython/__init__.py:36:33: Name `ParoxythonMagics` used when possibly not defined
- warning[possibly-unresolved-reference] paroxython/__init__.py:55:26: Name `main` used when possibly not defined
- warning[possibly-unresolved-reference] paroxython/__init__.py:62:21: Name `display` used when possibly not defined
- warning[possibly-unresolved-reference] paroxython/__init__.py:62:29: Name `Markdown` used when possibly not defined
- Found 15 diagnostics
+ Found 11 diagnostics

beartype (https://github.com/beartype/beartype)
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep484604.py:47:33: Name `HintPep604ItemTypes` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep484604.py:112:33: Name `HintPep604Type` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:223:9: Name `_MODULE_NAME_TO_ANNOTATIONS` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:224:9: Name `_MODULE_NAME_TO_HINTABLE_BASENAME_TO_ANNOTATIONS` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:243:33: Name `CallableOrClassTypes` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:247:27: Name `get_object_module_name_or_none` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:256:21: Name `_MODULE_NAME_TO_HINTABLE_BASENAME_TO_ANNOTATIONS` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:257:38: Name `SENTINEL` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:260:56: Name `SENTINEL` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:264:25: Name `_MODULE_NAME_TO_HINTABLE_BASENAME_TO_ANNOTATIONS` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:272:37: Name `get_object_basename_scoped` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:279:40: Name `SENTINEL` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:282:48: Name `SENTINEL` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:288:21: Name `_get_pep649_hintable_annotations_or_none_uncached` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:303:35: Name `ModuleType` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:305:27: Name `get_module_name` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:311:34: Name `_MODULE_NAME_TO_ANNOTATIONS` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:312:30: Name `SENTINEL` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:315:42: Name `SENTINEL` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:321:17: Name `_get_pep649_hintable_annotations_or_none_uncached` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:324:13: Name `_MODULE_NAME_TO_ANNOTATIONS` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:335:16: Name `_get_pep649_hintable_annotations_or_none_uncached` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:375:36: Name `Format` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:487:35: Name `_ANNOTATE_FORMATS_VALUELIKE` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:495:36: Name `Format` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:860:20: Name `get_annotations` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/_util/hint/pep/proposal/pep649.py:860:53: Name `Format` used when possibly not defined
- warning[possibly-unresolved-reference] beartype/typing/_typingpep544.py:690:16: Name `_generic_class_getitem_old` used when possibly not defined
- Found 581 diagnostics
+ Found 553 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[unresolved-attribute] pyinstrument/context_manager.py:56:54: Type `((...) -> Unknown) | None` has no attribute `__qualname__`
+ error[unresolved-attribute] pyinstrument/context_manager.py:56:77: Type `((...) -> Unknown) | None` has no attribute `__code__`
+ error[unresolved-attribute] pyinstrument/context_manager.py:56:105: Type `((...) -> Unknown) | None` has no attribute `__code__`
+ error[call-non-callable] pyinstrument/context_manager.py:59:28: Object of type `None` is not callable
- error[unresolved-reference] pyinstrument/magic/magic.py:258:31: Name `old_custom_tb` used when not defined
- error[unresolved-reference] pyinstrument/magic/magic.py:259:40: Name `old_custom_exceptions` used when not defined
- error[unresolved-reference] pyinstrument/vendor/appdirs.py:99:33: Name `_get_win_folder` used when not defined
- error[unresolved-reference] pyinstrument/vendor/appdirs.py:152:33: Name `_get_win_folder` used when not defined
- error[unresolved-reference] pyinstrument/vendor/appdirs.py:319:33: Name `_get_win_folder` used when not defined
- warning[possibly-unresolved-reference] pyinstrument/vendor/decorator.py:57:16: Name `FullArgSpec` used when possibly not defined
- Found 52 diagnostics
+ Found 50 diagnostics

anyio (https://github.com/agronholm/anyio)
- warning[possibly-unresolved-reference] src/anyio/abc/_sockets.py:77:67: Name `remote_port` used when possibly not defined
- warning[possibly-unbound-attribute] src/anyio/from_thread.py:211:27: Attribute `cancel` on type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/anyio/from_thread.py:211:27: Attribute `cancel` on type `Unknown | CancelScope | None` is possibly unbound
- error[unresolved-reference] src/anyio/pytest_plugin.py:90:12: Name `has_backend_arg` used when not defined
- error[unresolved-reference] src/anyio/pytest_plugin.py:93:12: Name `has_request_arg` used when not defined
- warning[possibly-unresolved-reference] src/anyio/pytest_plugin.py:139:25: Name `backend_name` used when possibly not defined
- warning[possibly-unresolved-reference] src/anyio/pytest_plugin.py:139:39: Name `backend_options` used when possibly not defined
- error[unresolved-reference] src/anyio/pytest_plugin.py:140:29: Name `original_func` used when not defined
- Found 100 diagnostics
+ Found 94 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[unresolved-reference] expression/core/result.py:121:56: Name `value` used when not defined
- Found 235 diagnostics
+ Found 234 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- warning[possibly-unresolved-reference] aioredis/connection.py:455:32: Name `hiredis` used when possibly not defined
- warning[possibly-unresolved-reference] aioredis/connection.py:468:24: Name `hiredis` used when possibly not defined
- Found 22 diagnostics
+ Found 20 diagnostics

attrs (https://github.com/python-attrs/attrs)
- warning[possibly-unresolved-reference] tests/test_slots.py:94:16: Name `asizeof` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_slots.py:94:41: Name `asizeof` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_slots.py:257:16: Name `asizeof` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_slots.py:257:30: Name `asizeof` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_slots.py:401:16: Name `asizeof` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_slots.py:401:30: Name `asizeof` used when possibly not defined
- Found 608 diagnostics
+ Found 602 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- warning[possibly-unresolved-reference] src/aiortc/contrib/signaling.py:126:17: Name `connected` used when possibly not defined
- warning[possibly-unresolved-reference] src/aiortc/contrib/signaling.py:183:17: Name `connected` used when possibly not defined
- warning[possibly-unresolved-reference] src/aiortc/rtcpeerconnection.py:105:43: Name `c` used when possibly not defined
- warning[possibly-unresolved-reference] src/aiortc/rtcsctptransport.py:743:68: Name `prefix` used when possibly not defined
- Found 102 diagnostics
+ Found 98 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- warning[possibly-unresolved-reference] backend/models/core_models.py:242:18: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:243:13: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:243:35: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:254:24: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:266:13: Name `TimeSlot` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:289:17: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:290:17: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:292:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:293:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:319:33: Name `TimeSlot` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:341:17: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:343:29: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:344:29: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:347:17: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:348:17: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:350:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:351:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:363:34: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:365:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:366:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:377:29: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:377:86: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:378:25: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:378:73: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:380:27: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:380:42: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:396:18: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:397:13: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:397:40: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:401:30: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:415:17: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:416:17: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:418:25: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:419:25: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:432:40: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:434:25: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:435:25: Name `ScheduleGroup` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:446:17: Name `Registration` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:447:17: Name `Registration` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:449:25: Name `Registration` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:453:13: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:455:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:456:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:473:35: Name `Participant` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:489:17: Name `Registration` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:490:17: Name `Registration` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:492:25: Name `Registration` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:496:13: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:498:25: Name `Schedule` used when possibly not defined
- warning[possibly-unresolved-reference] backend/models/core_models.py:499:25: Name `Schedule` used when possibly not defined
- Found 116 diagnostics
+ Found 66 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- warning[possibly-unresolved-reference] src/graphql/execution/execute.py:1062:38: Name `completed_item` used when possibly not defined
+ warning[unused-ignore-comment] src/graphql/execution/execute.py:1249:47: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] src/graphql/execution/execute.py:1449:40: Name `incremental_result` used when possibly not defined
+ warning[possibly-unbound-attribute] src/graphql/execution/execute.py:2200:20: Attribute `map_source_to_response` on type `Unknown | list[GraphQLError] | ExecutionContext` is possibly unbound
- error[unresolved-reference] src/graphql/execution/execute.py:2283:30: Name `awaitable_event_stream` used when not defined
- error[unresolved-reference] src/graphql/language/visitor.py:341:44: Name `enter_list` used when not defined
- error[unresolved-reference] src/graphql/language/visitor.py:354:44: Name `leave_list` used when not defined
+ error[unresolved-attribute] src/graphql/utilities/lexicographic_sort_schema.py:124:51: Type `GraphQLNamedType` has no attribute `interfaces`
+ error[unresolved-attribute] src/graphql/utilities/lexicographic_sort_schema.py:125:48: Type `GraphQLNamedType` has no attribute `fields`
+ error[unresolved-attribute] src/graphql/utilities/lexicographic_sort_schema.py:132:51: Type `GraphQLNamedType` has no attribute `interfaces`
+ error[unresolved-attribute] src/graphql/utilities/lexicographic_sort_schema.py:133:48: Type `GraphQLNamedType` has no attribute `fields`
+ error[unresolved-attribute] src/graphql/utilities/lexicographic_sort_schema.py:138:76: Type `GraphQLNamedType` has no attribute `types`
+ error[unresolved-attribute] src/graphql/utilities/lexicographic_sort_schema.py:159:54: Type `GraphQLNamedType` has no attribute `fields`
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:111:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:117:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:123:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:129:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:137:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:144:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:151:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:158:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:165:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:171:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:177:17: Name `visited` used when possibly not defined
- warning[possibly-unresolved-reference] tests/language/test_visitor.py:183:17: Name `visited` used when possibly not defined
- error[unresolved-reference] tests/utils/assert_equal_awaitables_or_values.py:21:67: Name `awaitable_items` used when not defined
- Found 381 diagnostics
+ Found 371 diagnostics

starlette (https://github.com/encode/starlette)
- warning[possibly-unresolved-reference] starlette/_utils.py:80:35: Name `BaseExceptionGroup` used when possibly not defined
- warning[possibly-unresolved-reference] starlette/responses.py:275:25: Name `task_group` used when possibly not defined
- warning[possibly-unresolved-reference] starlette/templating.py:119:10: Name `pass_context` used when possibly not defined
- Found 173 diagnostics
+ Found 170 diagnostics

rich (https://github.com/Textualize/rich)
- warning[possibly-unresolved-reference] examples/listdir.py:23:45: Name `root_path` used when possibly not defined
- warning[possibly-unresolved-reference] examples/suppress.py:19:9: Name `click` used when possibly not defined
- warning[possibly-unresolved-reference] rich/ansi.py:225:16: Name `os` used when possibly not defined
- warning[possibly-unresolved-reference] rich/ansi.py:226:9: Name `stdout` used when possibly not defined
- error[invalid-return-type] rich/console.py:575:16: Return type does not match returned value: expected `WindowsConsoleFeatures`, found `WindowsConsoleFeatures | None`
+ error[invalid-return-type] rich/console.py:575:16: Return type does not match returned value: expected `WindowsConsoleFeatures`, found `None | WindowsConsoleFeatures`
- warning[possibly-unresolved-reference] rich/logging.py:287:9: Name `log` used when possibly not defined
- warning[possibly-unresolved-reference] rich/logging.py:291:13: Name `log` used when possibly not defined
- warning[possibly-unresolved-reference] rich/palette.py:92:34: Name `colorsys` used when possibly not defined
- warning[possibly-unresolved-reference] rich/palette.py:93:34: Name `colorsys` used when possibly not defined
- warning[possibly-unresolved-reference] rich/palette.py:94:31: Name `Color` used when possibly not defined
- warning[possibly-unresolved-reference] rich/palette.py:95:29: Name `Color` used when possibly not defined
- warning[possibly-unresolved-reference] rich/palette.py:96:27: Name `Segment` used when possibly not defined
- warning[possibly-unresolved-reference] rich/palette.py:96:40: Name `Style` used when possibly not defined
- warning[possibly-unresolved-reference] rich/palette.py:97:23: Name `Segment` used when possibly not defined
- warning[possibly-unresolved-reference] rich/pretty.py:62:27: Name `_attr_module` used when possibly not defined
- warning[possibly-unresolved-reference] rich/pretty.py:67:12: Name `_attr_module` used when possibly not defined
- warning[possibly-unresolved-reference] rich/pretty.py:735:37: Name `attr_fields` used when possibly not defined
- warning[possibly-unresolved-reference] rich/syntax.py:516:35: Name `line_tokenize` used when possibly not defined
- warning[possibly-unresolved-reference] rich/syntax.py:518:35: Name `line_start` used when possibly not defined
- warning[possibly-unresolved-reference] rich/syntax.py:518:53: Name `line_start` used when possibly not defined
- warning[possibly-unresolved-reference] rich/syntax.py:534:32: Name `line_end` used when possibly not defined
- warning[possibly-unresolved-reference] rich/syntax.py:534:56: Name `line_end` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:845:20: Name `header_row` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:847:22: Name `footer_row` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:851:60: Name `row_height` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:853:63: Name `row_height` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:854:59: Name `row_height` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:968:13: Name `console` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:969:13: Name `console` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:969:26: Name `highlight` used when possibly not defined
- warning[possibly-unresolved-reference] rich/table.py:970:13: Name `console` used when possibly not defined
- warning[possibly-unresolved-reference] rich/traceback.py:894:9: Name `bar` used when possibly not defined
- warning[possibly-unresolved-reference] rich/traceback.py:897:9: Name `foo` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:20:28: Name `LegacyWindowsTerm` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:26:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:33:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:46:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:54:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:57:9: Name `WindowsCoordinates` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:64:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:72:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:74:61: Name `WindowsCoordinates` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:91:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:107:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:115:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:123:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:131:5: Name `legacy_windows_render` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_windows_renderer.py:139:5: Name `legacy_windows_render` used when possibly not defined
- Found 374 diagnostics
+ Found 327 diagnostics

kornia (https://github.com/kornia/kornia)
- warning[possibly-unresolved-reference] kornia/feature/dedode/transformer/layers/attention.py:89:19: Name `unbind` used when possibly not defined
- warning[possibly-unresolved-reference] kornia/feature/dedode/transformer/layers/attention.py:91:13: Name `memory_efficient_attention` used when possibly not defined
- warning[possibly-unresolved-reference] kornia/feature/dedode/transformer/layers/block.py:168:27: Name `scaled_index_add` used when possibly not defined
- warning[possibly-unresolved-reference] kornia/feature/dedode/transformer/layers/block.py:186:21: Name `fmha` used when possibly not defined
- warning[possibly-unresolved-reference] kornia/feature/dedode/transformer/layers/block.py:191:23: Name `index_select_cat` used when possibly not defined
- Found 804 diagnostics
+ Found 799 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- warning[possibly-unresolved-reference] dedupe/training.py:306:21: Name `covered_sample_matches` used when possibly not defined
- warning[possibly-unresolved-reference] dedupe/training.py:306:46: Name `sample_match_cover` used when possibly not defined
- warning[possibly-unresolved-reference] dedupe/training.py:307:25: Name `covered_comparisons` used when possibly not defined
- Found 62 diagnostics
+ Found 59 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-type-form] src/trio/_core/_io_kqueue.py:150:30: List literals are not allowed in this context in a type expression
+ error[invalid-argument-type] src/trio/_core/_io_kqueue.py:287:58: Argument is incorrect: Expected `bool`, found `ClosedResourceError`
- error[invalid-type-form] src/trio/_core/_io_windows.py:931:29: List literals are not allowed in this context in a type expression
- warning[possibly-unresolved-reference] src/trio/_core/_run.py:140:25: Name `get_pretty_function_description` used when possibly not defined
- error[unresolved-reference] src/trio/_core/_tests/test_guest_mode.py:70:13: Name `todo` used when not defined
- error[unresolved-reference] src/trio/_core/_tests/test_guest_mode.py:71:9: Name `todo` used when not defined
- error[unresolved-reference] src/trio/_core/_tests/test_guest_mode.py:80:13: Name `todo` used when not defined
- error[unresolved-reference] src/trio/_core/_tests/test_guest_mode.py:81:9: Name `todo` used when not defined
- error[unresolved-reference] src/trio/_core/_tests/test_guest_mode.py:85:9: Name `todo` used when not defined
- warning[possibly-unresolved-reference] src/trio/_deprecate.py:93:17: Name `thing` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_highlevel_open_unix_stream.py:61:19: Name `AF_UNIX` used when possibly not defined
- error[unresolved-reference] src/trio/_subprocess.py:763:26: Name `killer_cscope` used when not defined
+ error[call-non-callable] src/trio/_subprocess.py:764:31: Object of type `None` is not callable
- warning[possibly-unresolved-reference] src/trio/_subprocess_platform/waitid.py:17:9: Name `waitid` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_subprocess_platform/waitid.py:57:18: Name `waitid_ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_subprocess_platform/waitid.py:58:15: Name `waitid_cffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_subprocess_platform/waitid.py:59:25: Name `waitid_ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_sync.py:99:36: Name `task` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_dtls.py:36:11: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_dtls.py:36:23: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_dtls.py:42:11: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_dtls.py:42:23: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:306:28: Name `cache` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_highlevel_serve_listeners.py:120:19: Name `error` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_highlevel_serve_listeners.py:123:25: Name `error` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_socket.py:667:27: Name `local` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:208:15: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:208:27: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:225:28: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:227:22: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:277:24: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:281:24: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:301:28: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:316:36: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:325:40: Name `SSL` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:29:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:29:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:29:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:30:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:31:5: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:32:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:36:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:36:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:36:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:37:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:37:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:37:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:38:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:39:5: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:40:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:41:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:45:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:45:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:45:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:46:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:46:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:46:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:47:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:48:5: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:49:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:50:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:54:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:54:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:54:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:55:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:55:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:55:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:56:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:58:9: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:59:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:63:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:63:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:63:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:64:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:64:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:64:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:65:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:67:9: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:68:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:80:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:80:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:80:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:85:13: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:91:9: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:94:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:98:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:98:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:98:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:99:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:99:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:99:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:104:13: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:109:9: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:112:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:113:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:117:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:117:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:117:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:118:15: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:118:37: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:118:60: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:123:13: Name `WaitForMultipleObjects_sync` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:128:9: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:131:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:132:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:141:14: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:141:36: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:141:59: Name `ffi` used when possibly not defined
+ warning[unused-ignore-comment] src/trio/_tests/test_wait_for_object.py:164:52: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:142:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:143:11: Name `WaitForSingleObject` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:144:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:148:14: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:148:36: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:148:59: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:149:22: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:150:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:151:11: Name `WaitForSingleObject` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:152:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:156:14: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:156:36: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:156:59: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:157:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:159:15: Name `WaitForSingleObject` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:179:41: Name `Handle` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:181:9: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:185:14: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:185:36: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:185:59: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:189:28: Name `WaitForSingleObject` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:192:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:199:14: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:199:36: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:199:59: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:200:22: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:204:28: Name `WaitForSingleObject` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:207:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:216:14: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:216:36: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:216:59: Name `ffi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:220:15: Name `WaitForSingleObject` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_wait_for_object.py:222:5: Name `kernel32` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_util.py:226:29: Name `objname` used when possibly not defined
- Found 1008 diagnostics
+ Found 871 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ warning[possibly-unbound-attribute] dulwich/client.py:1687:13: Attribute `close` on type `Unknown | None | socket` is possibly unbound
- warning[possibly-unresolved-reference] dulwich/merge.py:125:20: Name `_merge3_to_bytes` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/merge.py:147:24: Name `_merge3_to_bytes` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/merge.py:159:24: Name `_merge3_to_bytes` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/merge.py:177:22: Name `_merge3_to_bytes` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/notes.py:242:43: Name `update_tree` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/notes.py:252:39: Name `update_tree` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/pack.py:391:28: Name `mmap` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/pack.py:391:55: Name `mmap` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/porcelain.py:1127:38: Name `entry` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/porcelain.py:1162:42: Name `o` used when possibly not defined
+ error[invalid-argument-type] dulwich/porcelain.py:1649:64: Argument to function `parse_reftuples` is incorrect: Expected `bytes | list[bytes]`, found `Unknown | None | list[Unknown]`
+ error[invalid-argument-type] dulwich/porcelain.py:1745:54: Argument to function `parse_reftuples` is incorrect: Expected `bytes | list[bytes]`, found `Unknown | None | list[Unknown]`
+ warning[possibly-unbound-attribute] dulwich/porcelain.py:2094:9: Attribute `write` on type `Unknown | None | TextIO` is possibly unbound
+ warning[possibly-unbound-attribute] dulwich/porcelain.py:2095:9: Attribute `flush` on type `Unknown | None | TextIO` is possibly unbound
+ warning[possibly-unbound-attribute] dulwich/porcelain.py:2120:9: Attribute `write` on type `Unknown | None | TextIO` is possibly unbound
+ warning[possibly-unbound-attribute] dulwich/porcelain.py:2121:9: Attribute `flush` on type `Unknown | None | TextIO` is possibly unbound
- warning[possibly-unresolved-reference] dulwich/server.py:998:17: Name `writer` used when possibly not defined
- warning[possibly-unresolved-reference] dulwich/tests/utils.py:109:31: Name `sha` used when possibly not defined
- Found 153 diagnostics
+ Found 148 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:396:25: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:405:25: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:414:37: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:418:25: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:426:25: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:437:25: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:443:37: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] test/generated/testproto/test_pb2.pyi:447:25: Attribute `service` on type `<module 'google.protobuf'> | Unknown` is possibly unbound
- Found 46 diagnostics
+ Found 54 diagnostics

comtypes (https://github.com/enthought/comtypes)
- warning[possibly-unresolved-reference] comtypes/_comobject.py:74:9: Name `_acquire` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/_comobject.py:77:9: Name `_release` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/_comobject.py:81:9: Name `_acquire` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/_comobject.py:84:9: Name `_release` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/_memberspec.py:385:32: Name `putref` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/_memberspec.py:386:28: Name `put` used when possibly not defined
- error[unresolved-reference] comtypes/_memberspec.py:588:46: Name `interface` used when not defined
- warning[possibly-unresolved-reference] comtypes/test/test_comserver.py:141:16: Name `Dispatch` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_comserver.py:162:16: Name `Dispatch` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_records.py:24:35: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_records.py:29:23: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_records.py:37:23: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_records.py:51:23: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_records.py:68:25: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_records.py:75:14: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_safearrays.py:27:35: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispifc_safearrays.py:32:23: Name `ComtypesCppTestSrvLib` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispinterface.py:40:13: Name `EnsureDispatch` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_dispinterface.py:67:13: Name `Dispatch` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_excel.py:71:37: Name `xlRangeValueDefault` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:46:25: Name `IDispSafearrayParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:50:23: Name `IDispSafearrayParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:52:61: Name `IDispSafearrayParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:59:57: Name `IDispSafearrayParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:64:14: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:66:18: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:70:53: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_midl_safearray_create.py:77:49: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:19:45: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:25:14: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:35:19: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:43:29: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:49:13: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:55:20: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:60:29: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_recordinfo.py:72:19: Name `StructRecordParamTest` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:67:5: Name `_pack` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:76:26: Name `unpack` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:82:26: Name `unpack` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:88:26: Name `unpack` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:97:26: Name `unpack` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:115:12: Name `_PyCom_PyObjectFromIUnknown` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:137:16: Name `win32com` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_win32com_interop.py:153:16: Name `win32com` used when possibly not defined
- warning[possibly-unresolved-reference] comtypes/test/test_word.py:69:31: Name `Word` used when possibly not defined
- Found 530 diagnostics
+ Found 485 diagnostics

alerta (https://github.com/alerta/alerta)
- warning[possibly-unresolved-reference] alerta/app.py:107:14: Name `Celery` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/auth/utils.py:24:16: Name `bcrypt` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/auth/utils.py:24:40: Name `bcrypt` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/auth/utils.py:27:16: Name `bcrypt` used when possibly not defined
+ warning[possibly-unbound-attribute] alerta/database/backends/mongodb/base.py:805:28: Attribute `where` on type `Unknown | None | (Unknown & ~AlwaysFalsy)` is possibly unbound
+ warning[possibly-unbound-attribute] alerta/database/backends/mongodb/base.py:843:28: Attribute `where` on type `Unknown | None | (Unknown & ~AlwaysFalsy)` is possibly unbound
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:38:15: Name `MIMEMultipart` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:44:20: Name `MIMEText` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:49:19: Name `ssl` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:55:22: Name `smtplib` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:58:22: Name `smtplib` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:72:16: Name `smtplib` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:74:26: Name `socket` used when possibly not defined
- warning[possibly-unresolved-reference] alerta/utils/mailer.py:74:41: Name `socket` used when possibly not defined
- error[unresolved-reference] alerta/webhooks/custom.py:51:92: Name `alert` used when not defined
+ warning[unused-ignore-comment] alerta/webhooks/custom.py:50:123: Unused blanket `type: ignore` directive
- Found 458 diagnostics
+ Found 448 diagnostics

pyjwt (https://github.com/jpadilla/pyjwt)
- warning[possibly-unresolved-reference] jwt/algorithms.py:157:26: Name `RSAAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:157:39: Name `RSAAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:158:26: Name `RSAAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:158:39: Name `RSAAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:159:26: Name `RSAAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:159:39: Name `RSAAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:160:26: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:160:38: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:161:27: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:161:39: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:162:26: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:162:38: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:163:26: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:163:38: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:164:26: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:165:21: Name `ECAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:167:26: Name `RSAPSSAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:167:42: Name `RSAPSSAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:168:26: Name `RSAPSSAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:168:42: Name `RSAPSSAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:169:26: Name `RSAPSSAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:169:42: Name `RSAPSSAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:170:26: Name `OKPAlgorithm` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:183:35: Name `AllowedKeys` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:199:38: Name `hashes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:201:22: Name `hashes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:201:54: Name `default_backend` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:207:42: Name `PublicKeyTypes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:207:59: Name `PrivateKeyTypes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:386:31: Name `hashes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:387:31: Name `hashes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:388:31: Name `hashes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:392:43: Name `hashes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:395:36: Name `AllowedRSAKeys` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:395:69: Name `AllowedRSAKeys` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:406:33: Name `PublicKeyTypes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:406:50: Name `load_ssh_public_key` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:408:33: Name `RSAPublicKey` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:410:34: Name `PrivateKeyTypes` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:410:52: Name `load_pem_private_key` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algorithms.py:414:33: Name `RSAPrivateKey` used when possibly not defined
- warning[possibly-unresolved-reference] jwt/algori...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-20 13:23_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-06-23 08:56_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-23 08:56_

---

_Comment by @codspeed-hq[bot] on 2025-06-23 13:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fpublic-types?runnerMode=WallTime)

### Merging #18750 will **not alter performance**

<sub>Comparing <code>david/public-types</code> (4226670) with <code>main</code> (2362263)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-06-24 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@sharkdp reviewed on 2025-06-24 14:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/scopes/builtin.md`:35 on 2025-06-24 14:50_

I believe this could be a follow-up task. I haven't seen any ecosystem problems related to this.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:1 on 2025-06-24 14:52_

I think I'll need some help deciding if these changes here are okay or not.

---

_@sharkdp reviewed on 2025-06-24 14:52_

---

_@sharkdp reviewed on 2025-06-25 11:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1646 on 2025-06-25 11:09_

Can't easily get this to work with `TypeArrayDisplay`, so just join it manually.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-06-25 11:27_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-25 11:27_

---

_@AlexWaygood reviewed on 2025-06-25 11:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1646 on 2025-06-25 11:51_

I added this function yesterday, which could possibly be generalised: https://github.com/astral-sh/ruff/blob/c1fed55d51b34e1235f72c610fe4b1700ed76451/crates/ty_python_semantic/src/types/diagnostic.rs#L2039-L2064

its behaviour is that if you have a list of two types `A` and `B`, it creates the string ``"`A` and `B`"``, if you have a list of three types `A`, `B` and `C`, it creates the string ``"`A`, `B` and `C`"``, for four types it creates the string ``"`A`, `B`, `C` and `D`"``, etc.

---

_@AlexWaygood reviewed on 2025-06-25 11:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1646 on 2025-06-25 11:55_

I guess a generalised version would be something like this (untested):

```rs
fn describe_types_list<'db, I, IT, D>(db: &'db dyn Db, types: I) -> String
where
    I: IntoIterator<IntoIter = IT>,
    IT: ExactSizeIterator<Item = D> + DoubleEndedIterator,
    D: std::fmt::Display,
{
    let types = types.into_iter();
    debug_assert!(types.len() >= 2);

    let final_type = types.next_back().unwrap();
    let penultimate_type = types.next_back().unwrap();

    let mut buffer = String::new();

    for type in types {
        buffer.push('`');
        buffer.push_str(type);
        buffer.push_str("`, ");
    }

    buffer.push('`');
    buffer.push_str(penultimate_type);
    buffer.push_str("` and `");
    buffer.push_str(final_type);
    buffer.push('`');

    buffer
}

---

_Marked ready for review by @sharkdp on 2025-06-25 12:25_

---

_Review requested from @carljm by @sharkdp on 2025-06-25 12:25_

---

_Review requested from @dcreager by @sharkdp on 2025-06-25 12:25_

---

_@dcreager reviewed on 2025-06-25 12:41_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:1646 on 2025-06-25 12:41_

> if you have a list of three types `A`, `B` and `C`, it creates the string `` "`A`, `B` and `C`" ``, for four types it creates the string `` "`A`, `B`, `C` and `D`" ``, etc.

I see we are Oxford comma nemeses... :ox: 

---

_Converted to draft by @sharkdp on 2025-06-25 13:34_

---

_Comment by @sharkdp on 2025-06-25 13:35_

Converting back to draft because I think I have a solution for the declaration-shadows-bindings problem.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-06-25 14:07_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-25 14:07_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-06-25 14:08_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-25 14:08_

---

_@sharkdp reviewed on 2025-06-25 14:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:421 on 2025-06-25 14:10_

We currently don't filter out overloads from *bindings*, only from declarations. This is easy to change, but we would still infer the wrong type here (only `Overload[(x: int) -> int, (x: bytes) -> bytes]`). It's not clear to me if this is an important case that we need to handle.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-06-25 14:13_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-25 14:13_

---

_Marked ready for review by @sharkdp on 2025-06-25 14:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/declaration/error.md`:50 on 2025-06-25 14:18_

nit: it would be nice to at least have backticks around each type, e.g.

```suggestion
    x = "a"  # error: [conflicting-declarations] "Conflicting declared types for `x`: `str`, `int`, `bytes`"
```

---

_Review comment by @AlexWaygood on `python/py-fuzzer/fuzz.py`:175 on 2025-06-25 14:26_

I don't think this should be necessary because the fuzzer CI job currently only complains if you introduce _new_ panics relative to the `main` branch. So once this PR is merged, the new failures will be naturally incorporated into the baseline for future PRs, and the fuzzer CI job won't fail on those future PRs.

The reason for the existing `skip_check` set above isn't because we panic on those seeds -- it's because we take an excruciatingly long time attempting to resolve cycles when we analyze those seeds (or at least, that was the case last time we checked), and it sucks if the fuzzer job takes 15 minutes in CI 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:5 on 2025-06-25 14:27_

```suggestion
The "public type" of a symbol refers to the type that is inferred for a symbol from an external (either enclosing or nested)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:7 on 2025-06-25 14:27_

```suggestion
currently make the simplifying assumption that an inner scope (such as the `inner` function below)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:9 on 2025-06-25 14:28_

```suggestion
could be executed at any position in its enclosing scope. The public type of a symbol in the outer scope
from the perspective of the inner scope should therefore be the union of all possible types that the
symbol could have.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:91 on 2025-06-25 14:30_

To more clearly distinguish this case from the case that we _do_ want to complain about (which you detail in your following example):

```suggestion
If a symbol is only conditionally bound and the inner scope is bound in the same branch,
we do not raise any errors:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:115 on 2025-06-25 14:31_

```suggestion
type, which would have resulted in incorrect type inference here:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:147 on 2025-06-25 14:32_

```suggestion
Arbitrarily many levels of nesting are supported:
```

or

```suggestion
An arbitrary level of nesting is supported:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:192 on 2025-06-25 14:33_

```suggestion
When a declaration only appears in one branch, we also consider the types of the symbol's bindings in other branches:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:210 on 2025-06-25 14:34_

```suggestion
declaration and a binding, but we still add `None` to the public type union in a situation like this:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:308 on 2025-06-25 14:36_

```suggestion
        # TODO: this should ideally be `Unknown | None | Literal[1]`
        # (requires https://github.com/astral-sh/ty/issues/220)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:362 on 2025-06-25 14:38_

I'm struggling to parse this -- the example seems to show that we include both overloads in the revealed type of `f`, just like we do if there _is_ an overload implementation?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-25 14:39_

hmm, this one's a bit concerning. It seems pretty obviously incorrect, and (unlike the one with conditionally overridden builtins) I don't think it'll be easy to explain to users why it happens.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/builtin.md`:35 on 2025-06-25 14:39_

yeah I think this is definitely acceptable, at least for now

---

_@AlexWaygood reviewed on 2025-06-25 14:41_

Only reviewed the tests so far. Which are overall fantastic! These are mostly nitpicks:

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:1 on 2025-06-25 16:16_

FWIW I don't love this name, if anyone has a better idea -- originally it was both the type seen by nested scopes and by imports, but those have diverged already, and fully break with this PR. "Public" seems like a better fit for imports than for nested scopes. But I don't have a great alternative in mind for how to name "the type seen by nested scopes in the same file."

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:5 on 2025-06-25 16:20_

I think the current text is correct here (although it maybe could be clearer, I initially read "from an enclosing scope" as describing "where we are inferring from" rather than describing "a symbol").

I don't think we ever look at public types of symbols of symbols in nested scopes (name lookup travels up the scope tree, not down it), so I don't see a reason to mention both, as in the suggested change here.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:6 on 2025-06-25 16:22_

One idea for how to make this read slightly clearer (to me, anyway)

```suggestion
The "public type" of a symbol refers to the type that is inferred in a nested scope for a symbol
defined in an outer enclosing scope. Since it is not generally possible to analyze the full control
flow of a program, we
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:9 on 2025-06-25 16:25_

This edit seems to imply that a given symbol may have different public types, from different perspectives. I don't think that's true?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:91 on 2025-06-25 16:31_

I think this edit suggests we are already employing a heuristic, or a level of smarts, that we don't actually employ in this PR -- though we may want to in future.

I do agree that currently this example and the following one are redundant, except for the fact that in the second case we say we want to change the behavior in future, and in the first case we don't say that. So it might be good to be clearer about that. I would probably try to do that without implying that we are already doing more than we do. So something like this text introducing the second test:

"In the future, we may try to be smarter about which bindings must or must not be a visible to a given nested scope, depending where it is defined. In the above case, this shouldn't change the behavior -- `x` is defined before `inner` in the same branch, so should be considered definitely-bound for `inner`. But in other cases we may want to emit `possibly-unresolved-reference` in future:"

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:26 on 2025-06-25 16:35_

This can be a TODO for now and considered as a follow-up, but when the symbol resolves to an outer scope that is a function scope (as opposed to module global scope), and that name is never declared nonlocal in a nested scope that also assigns to it (this is detectable statically in semantic indexing), I think we can skip the union with `Unknown` here, even if the symbol is not declared. This is because in a function scope we can see all bindings. There is no possibility (within the realm of reasonable code that we should attempt to model) that some external actor could reach in and set the symbol to some other value than the ones we can see.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:231 on 2025-06-25 16:39_

I think in this test it would be worth adding a real named file and importing something existing from it, just to clarify the distinction between the imported type vs the `Unknown` that we sometimes union with undeclared public types.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:308 on 2025-06-25 16:41_

I am not sure that we should ever do this (actually resolve the types of all nonlocal bindings in nested scopes), but we probably could. I think it could lead to cycles. Does any other type checker do this?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:362 on 2025-06-25 16:44_

"Consider" here means "union into the final type". That is, the point of this test is that we infer the type `Overload[(x: int) -> int, (x: str) -> str]` for `f` (and yes, that does "consider" all overloads in a different sense), we don't infer the type `Overload[(x: int) -> int, (x: str) -> str] | ((x: int) -> int)` (where we wrongly "consider" the first overload as a standalone definition to be unioned.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-25 16:51_

I'm not sure why the same logic that handles the non-conditional overload cases doesn't handle this one as well (probably will be clearer after reading the code), but I don't think find it super concerning, mostly just because I think this pattern is vanishingly rare, and other type checkers don't handle it well either. (There are conditionally-defined overloads in typeshed, but always with statically known branches.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/scopes/eager.md`:392 on 2025-06-25 16:53_

The semantics of deferred annotations aren't clear -- in principle evaluation of a deferred annotation could be triggered at any time. So it's not clear to me that this should ideally be `Unknown | str`

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/terminal_statements.md`:587 on 2025-06-25 16:56_

I think this is the same thing I mentioned in a comment above, but I don't think it relies on escape analysis, as implied in this comment. If `g` escapes, that still doesn't allow some external code not visible to us can change its closure variables. (Well, technically they can by directly modifying `__closure__` on the function, but this is beyond what I think we should model.) I think we can eliminate `Unknown` simply based on "symbol resolves to a function-like scope, and there are no nested `nonlocal` declarations of that symbol that might modify it".

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1780 on 2025-06-25 17:01_

As mentioned in another test, it's not actually clear to me what's more correct here, so maybe we don't need a TODO for it (unless we see real-world motivating examples for a different behavior)

---

_@carljm reviewed on 2025-06-25 17:12_

Thank you, this is looking great! Submitting a partial review, will come back to the rest later.

---

_@AlexWaygood reviewed on 2025-06-25 17:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:5 on 2025-06-25 17:17_

> I don't think we ever look at public types of symbols of symbols in nested scopes

Don't we? This reveals `int`, not `Literal[1]`:

```py
class Foo:
    X: int = 1

reveal_type(Foo.X)
```

Or am I confusing the concept of "public types" with something else?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:91 on 2025-06-25 17:20_

sure

---

_@AlexWaygood reviewed on 2025-06-25 17:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:1 on 2025-06-25 17:21_

I wonder if "nonlocal type" would be a better name, if this concept really is limited to nested scopes.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:5 on 2025-06-25 17:22_

Resolving class variables uses end-of-scope types, even after this PR, and I don't think we are considering changing that (or should). So the concept of "public type" described in this PR is really just about nested scopes.

I think this is a further point in favor of "public type is not a good name for this concept."

You can see that even after this PR, we only consider the last definition of `x` here, not all definitions of `x`:

```py
class C:
    x = 1
    x = 2

reveal_type(C.x)  # revealed: Unknown | Literal[2]
```

---

_@carljm reviewed on 2025-06-25 17:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:5 on 2025-06-25 17:26_

I see. I thought the concept "public type" referred to "the type of the symbol when considered by any external scope", and thus it also covered the fact that we prefer the declared type over the inferred type when a class attribute was accessed from outside the class, or when an attribute on module `b` was accessed from an external module `a` that had imported `b`. Evidently I was mistaken :-)

---

_@AlexWaygood reviewed on 2025-06-25 17:26_

---

_@AlexWaygood reviewed on 2025-06-25 17:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:9 on 2025-06-25 17:27_

I think this is the same conversation we're having at https://github.com/astral-sh/ruff/pull/18750#discussion_r2166871990

---

_@AlexWaygood reviewed on 2025-06-25 17:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:26 on 2025-06-25 17:36_

> (as opposed to module global scope)

or a class scope inside the global scope, or a class scope inside a class scope inside the global scope, etc... If all enclosing scopes are either module or class scopes, variables inside any enclosing scopes could be externally mutated (and so we should union with `Unknown` when inferring their public types), but this is not the case if any enclosing scopes are function-like scopes.

---

_@AlexWaygood reviewed on 2025-06-25 17:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:308 on 2025-06-25 17:39_

hmm, yeah, I'm not sure either now that I look more closely. No idea what other type checkers do.

---

_@AlexWaygood reviewed on 2025-06-25 17:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:362 on 2025-06-25 17:41_

maybe this, to clarify that it's just showing that we have similar behaviour for overloaded functions without implementations as overloaded functions with implementations:

```suggestion
Similarly, if there is no implementation, we only consider the last overload definition.
```

---

_@AlexWaygood reviewed on 2025-06-25 17:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-25 17:42_

it still seems like it would be highly confusing if you came across it as a user, but I do agree that it should be rare.

---

_@sharkdp reviewed on 2025-06-25 17:58_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-25 17:58_

> I'm not sure why the same logic that handles the non-conditional overload cases doesn't handle this one as well

I described this in another comment higher above:

> We currently don't filter out overloads from bindings, only from declarations. This is easy to change, but we would still infer the wrong type here (only Overload[(x: int) -> int, (x: bytes) -> bytes]). It's not clear to me if this is an important case that we need to handle.

In summary, it is easy to change this to `Overload[(x: int) -> int, (x: bytes) -> bytes`, and I should probably just do that. It's harder to change this to the same union that we infer in the local scope.

---

_Review comment by @sharkdp on `python/py-fuzzer/fuzz.py`:175 on 2025-06-25 18:03_

Ah, I see! I was very confused why I was so unlucky to only be the second person to run into this. So if I understand correctly, I'll just revert this change and ignore the fuzzer failure on this PR (I made sure that all these seeds are related to cyclic / self-referential problems, as indicated in the comment).

---

_@sharkdp reviewed on 2025-06-25 18:03_

---

_@AlexWaygood reviewed on 2025-06-25 18:03_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/fuzz.py`:175 on 2025-06-25 18:03_

yup!

---

_@sharkdp reviewed on 2025-06-25 18:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:308 on 2025-06-25 18:07_

Ah, yes — I thought I had mentioned it here as well in the TODO. I added this test because I saw that both mypy and pyright support this. Both infer `int | None` here.

---

_@carljm reviewed on 2025-06-25 18:18_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:26 on 2025-06-25 18:18_

Yeah, I didn't mention class scopes because names in class scopes are not visible to nested scopes anyway

---

_@AlexWaygood reviewed on 2025-06-25 18:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/public_types.md`:26 on 2025-06-25 18:21_

good point

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:5 on 2025-06-25 18:22_

> also covered the fact that we prefer the declared type over the inferred type

This is indeed a characteristic that is shared by all of imported types, attribute types, and "nonlocal types" (what are currently called "public types" in this PR) -- but this PR is only about the latter.

The confusion here is really my fault -- I conflated all of these under the single name "public type" initially, and we've discovered over time the ways in which they need to be different, and now we need to sort out what to call each of them as well.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:973 on 2025-06-25 18:34_

This comment seems incomplete

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-25 18:50_

I don't understand the relevance of bindings vs declarations, considering that all `def` statements are both, so in this example `f` is always declared, so I don't see why we would ever even look at bindings for `f` when deciding its public type?

To me it seems this limitation is more related to the fact that the current handling of overloads in `PublicTypeBuilder` is simply based on the order in which we see the definitions, with the logic being that any function-literal declaration of a name "erases" an immediately-preceding overloaded function-literal declaration of that same name.

The handling which I intuitively expected would be a bit different, and perhaps more complex, but would handle this case correctly: it would be that for any `OverloadLiteral` declaration we find, we look at what function-literal type is its predecessor (not based on the order we are processing the declarations, but based on `OverloadLiteral::previous_overload` on the type, which respects control flow), and we'd eliminate that type from the union we are building.

That said, I don't think that this is an important case we need to handle, so I'm fine going with the current handling with TODO. (And it's entirely probably that I'm missing some reason why the handling I expected doesn't actually work.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:1035 on 2025-06-25 18:53_

Minor nit: we spell this type both here and as part of `PlaceFromDeclarationsResult` -- perhaps it should also be its own type alias?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:1152 on 2025-06-25 18:55_

A goal of the previous code was to avoid creating a `UnionBuilder` if there is only one type to consider (which is probably the common case?). It seems we could still avoid creating a `PublicTypeBuilder` in that scenario? I'm not sure if this makes a measurable difference to performance, though.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/place.rs`:1125 on 2025-06-25 18:58_

Would `AssumeBound` be a better name than `AlwaysBound` for this variant? This usage site makes it clear that it's not really "always bound", it's just that we generally aren't checking for boundness.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:686 on 2025-06-25 19:04_

It's not totally clear to me that reusing `Declarations` and `Bindings` structs, adding `PreviousDefinitions` flag when recording a definition, and leaving their constraint-tracking functionality unused in this case, is the clearest approach here? How much code would actually be duplicated if we used bespoke simpler vectors of bindings/declarations here?

Really fine with either option here, just curious if it's a tradeoff you considered.

---

_@carljm approved on 2025-06-25 19:05_

This looks really good, thank you! I think all of my comments fall into the "for your consider and discretion" category, I'm happy with this as-is.

---

_@carljm reviewed on 2025-06-25 19:26_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-25 19:26_

On further reflection I realized why bindings matter -- because in this scenario, given the control-flow-insensitive handling of the overloaded function declarations, we end up considering only the third declaration, and that declaration is only conditionally reachable, so then we fall back to bindings.

I think doing the same filtering on bindings that we do on declarations might be important, not for the scenario shown here (overloaded functions in which some overloads are conditional based on not-statically-known conditions should be rare), but for this simpler and more plausible scenario:

```py
from typing import overload

def returns_bool() -> bool:
    return True

if returns_bool():
    @overload
    def f(x: int) -> int: ...
    @overload
    def f(x: int) -> int: ...

    def _():
        reveal_type(f)  # reveals a bogus union of both definitions in the current PR
```

---

_@carljm reviewed on 2025-06-26 00:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-26 00:38_

It feels like (in general) we should be able to detect when the bindings we get back are the same Definitions as the declarations we already checked, and not uselessly redo the same work in these scenarios with Definitions that are both bindings and declarations. But that's probably out of scope for this PR (and maybe not that valuable anyway.)

---

_@sharkdp reviewed on 2025-06-26 07:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:231 on 2025-06-26 07:47_

This can't really be demonstrated without a gradual type like `Unknown`, I think? If `some_library` would exist, the import would be a declaration of `<module 'some_library'>`. We would then error on the `some_library = None` binding (`invalid-assignment`). And both the local and nonlocal type would be `<module 'some_library'>` here.

This code pattern will lead to type errors in either case (library exists or not), and I'm not sure if it should generally be recommended over alternatives, but it is so pervasive across the ecosystem that I wanted to capture it in a test.

I changed the name to `optional_dependency` to make this a bit more clear, but also happy to just remove this test.

---

_@sharkdp reviewed on 2025-06-26 08:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:1 on 2025-06-26 08:34_

I'll do the renaming (which involves renaming some files) in a follow-up to this.

---

_@sharkdp reviewed on 2025-06-26 08:40_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:1152 on 2025-06-26 08:40_

Will try that separately in a follow-up.

---

_@sharkdp reviewed on 2025-06-26 08:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-26 08:48_

> On further reflection I realized why bindings matter -- because in this scenario, given the control-flow-insensitive handling of the overloaded function declarations, we end up considering only the third declaration, and that declaration is only conditionally reachable, so then we fall back to bindings.

Yes, exactly. I should have described this better.

> It feels like (in general) we should be able to detect when the bindings we get back are the same Definitions as the declarations we already checked, and not uselessly redo the same work in these scenarios with Definitions that are both bindings and declarations. But that's probably out of scope for this PR

I thought about this as well. And I agree, this seems like a major change. In a scenario like the following, we currently do not keep track of whether or not these two definitions come from the same "possibly reachable" branch or not
```py
if flag:
    x: int
else:
    x = 1

use(x)
```

---

_@sharkdp reviewed on 2025-06-26 10:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/public_types.md`:419 on 2025-06-26 10:03_

> I think doing the same filtering on bindings that we do on declarations might be important, not for the scenario shown here (overloaded functions in which some overloads are conditional based on not-statically-known conditions should be rare), but for this simpler and more plausible scenario:

Done.

---

_@sharkdp reviewed on 2025-06-26 10:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:1646 on 2025-06-26 10:03_

Thank you, Alex. I'll keep this for a follow-up.

---

_@sharkdp reviewed on 2025-06-26 10:04_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/declaration/error.md`:50 on 2025-06-26 10:04_

Same here => follow-up.

---

_@sharkdp reviewed on 2025-06-26 10:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:686 on 2025-06-26 10:09_

I did indeed consider this, the path I chose seemed to be the easiest one, but I agree that the other option might be better. I can make an attempt to change this.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-06-26 10:14_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-26 10:14_

---

_Renamed from "[ty] Infer public types as unions of all reachable bindings" to "[ty] Infer nonlocal types as unions of all reachable bindings" by @sharkdp on 2025-06-26 10:24_

---

_Merged by @sharkdp on 2025-06-26 10:24_

---

_Closed by @sharkdp on 2025-06-26 10:24_

---

_Branch deleted on 2025-06-26 10:24_

---
