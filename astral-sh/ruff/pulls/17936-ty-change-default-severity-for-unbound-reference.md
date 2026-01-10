```yaml
number: 17936
title: "[ty] Change default severity for `unbound-reference` to `error`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/unbound-reference-error
created_at: 2025-05-08T07:00:42Z
updated_at: 2025-05-08T15:54:48Z
url: https://github.com/astral-sh/ruff/pull/17936
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Change default severity for `unbound-reference` to `error`

---

_Pull request opened by @MichaReiser on 2025-05-08 07:00_

## Summary

A unbound reference is always a runtime error and this is a high confidence rule. Therefore,
the default severity should be error.

## Test Plan

Updated tests


---

_Review requested from @carljm by @MichaReiser on 2025-05-08 07:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-08 07:00_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-08 07:00_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-08 07:00_

---

_Label `configuration` added by @MichaReiser on 2025-05-08 07:00_

---

_Label `ty` added by @MichaReiser on 2025-05-08 07:00_

---

_@sharkdp approved on 2025-05-08 07:04_

Thank you

---

_Comment by @github-actions[bot] on 2025-05-08 07:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- warning[lint:unresolved-reference] mypy_primer/main.py:175:67: Name `shard_costs` used when not defined
+ error[lint:unresolved-reference] mypy_primer/main.py:175:67: Name `shard_costs` used when not defined

parso (https://github.com/davidhalter/parso)
- warning[lint:unresolved-reference] parso/python/errors.py:208:23: Name `testlist_comp` used when not defined
+ error[lint:unresolved-reference] parso/python/errors.py:208:23: Name `testlist_comp` used when not defined

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- warning[lint:unresolved-reference] pytest_robotframework/_internal/utils.py:37:12: Name `method_name` used when not defined
+ error[lint:unresolved-reference] pytest_robotframework/_internal/utils.py:37:12: Name `method_name` used when not defined

nionutils (https://github.com/nion-software/nionutils)
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:218:13: Name `begin_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:218:13: Name `begin_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:222:13: Name `end_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:222:13: Name `end_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:245:13: Name `begin_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:245:13: Name `begin_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:249:13: Name `end_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:249:13: Name `end_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:368:13: Name `begin_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:368:13: Name `begin_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:372:13: Name `end_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:372:13: Name `end_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:396:13: Name `begin_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:396:13: Name `begin_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/ListModel_test.py:400:13: Name `end_changes_count` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/ListModel_test.py:400:13: Name `end_changes_count` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/Process_test.py:27:13: Name `a` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/Process_test.py:27:13: Name `a` used when not defined
- warning[lint:unresolved-reference] nion/utils/test/Process_test.py:31:13: Name `b` used when not defined
+ error[lint:unresolved-reference] nion/utils/test/Process_test.py:31:13: Name `b` used when not defined

beartype (https://github.com/beartype/beartype)
- warning[lint:unresolved-reference] beartype/_util/py/utilpyinterpreter.py:59:12: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] beartype/_util/py/utilpyinterpreter.py:59:12: Name `__debug__` used when not defined

more-itertools (https://github.com/more-itertools/more-itertools)
- warning[lint:unresolved-reference] more_itertools/more.py:3773:23: Name `remaining` used when not defined
+ error[lint:unresolved-reference] more_itertools/more.py:3773:23: Name `remaining` used when not defined
- warning[lint:unresolved-reference] more_itertools/more.py:3774:21: Name `remaining` used when not defined
+ error[lint:unresolved-reference] more_itertools/more.py:3774:21: Name `remaining` used when not defined

pyp (https://github.com/hauntsaninja/pyp)
- warning[lint:unresolved-reference] pyp.py:652:24: Name `line_to_node` used when not defined
+ error[lint:unresolved-reference] pyp.py:652:24: Name `line_to_node` used when not defined
- warning[lint:unresolved-reference] tests/test_pyp.py:187:16: Name `count` used when not defined
+ error[lint:unresolved-reference] tests/test_pyp.py:187:16: Name `count` used when not defined
- warning[lint:unresolved-reference] tests/test_pyp.py:189:17: Name `count` used when not defined
+ error[lint:unresolved-reference] tests/test_pyp.py:189:17: Name `count` used when not defined

pyinstrument (https://github.com/joerick/pyinstrument)
- warning[lint:unresolved-reference] pyinstrument/vendor/appdirs.py:99:33: Name `_get_win_folder` used when not defined
+ error[lint:unresolved-reference] pyinstrument/vendor/appdirs.py:99:33: Name `_get_win_folder` used when not defined
- warning[lint:unresolved-reference] pyinstrument/vendor/appdirs.py:152:33: Name `_get_win_folder` used when not defined
+ error[lint:unresolved-reference] pyinstrument/vendor/appdirs.py:152:33: Name `_get_win_folder` used when not defined
- warning[lint:unresolved-reference] pyinstrument/vendor/appdirs.py:319:33: Name `_get_win_folder` used when not defined
+ error[lint:unresolved-reference] pyinstrument/vendor/appdirs.py:319:33: Name `_get_win_folder` used when not defined

anyio (https://github.com/agronholm/anyio)
- warning[lint:unresolved-reference] src/anyio/_core/_sockets.py:180:16: Name `connected_stream` used when not defined
+ error[lint:unresolved-reference] src/anyio/_core/_sockets.py:180:16: Name `connected_stream` used when not defined
- warning[lint:unresolved-reference] src/anyio/pytest_plugin.py:90:12: Name `has_backend_arg` used when not defined
+ error[lint:unresolved-reference] src/anyio/pytest_plugin.py:90:12: Name `has_backend_arg` used when not defined
- warning[lint:unresolved-reference] src/anyio/pytest_plugin.py:93:12: Name `has_request_arg` used when not defined
+ error[lint:unresolved-reference] src/anyio/pytest_plugin.py:93:12: Name `has_request_arg` used when not defined
- warning[lint:unresolved-reference] src/anyio/pytest_plugin.py:140:29: Name `original_func` used when not defined
+ error[lint:unresolved-reference] src/anyio/pytest_plugin.py:140:29: Name `original_func` used when not defined

attrs (https://github.com/python-attrs/attrs)
- warning[lint:unresolved-reference] src/attr/_make.py:1472:13: Name `hash` used when not defined
+ error[lint:unresolved-reference] src/attr/_make.py:1472:13: Name `hash` used when not defined
- warning[lint:unresolved-reference] src/attr/_next_gen.py:379:26: Name `on_setattr` used when not defined
+ error[lint:unresolved-reference] src/attr/_next_gen.py:379:26: Name `on_setattr` used when not defined
- warning[lint:unresolved-reference] src/attr/_next_gen.py:382:32: Name `on_setattr` used when not defined
+ error[lint:unresolved-reference] src/attr/_next_gen.py:382:32: Name `on_setattr` used when not defined
- warning[lint:unresolved-reference] tests/test_annotations.py:434:23: Name `typing_extensions` used when not defined
+ error[lint:unresolved-reference] tests/test_annotations.py:434:23: Name `typing_extensions` used when not defined
- warning[lint:unresolved-reference] tests/test_make.py:1823:24: Name `__builtins__` used when not defined
+ error[lint:unresolved-reference] tests/test_make.py:1823:24: Name `__builtins__` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:47:16: Name `__class__` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:47:16: Name `__class__` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:71:16: Name `__class__` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:71:16: Name `__class__` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:442:24: Name `__class__` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:442:24: Name `__class__` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:447:24: Name `__class__` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:447:24: Name `__class__` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:474:24: Name `__class__` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:474:24: Name `__class__` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:482:24: Name `__class__` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:482:24: Name `__class__` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:1058:13: Name `call_count` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:1058:13: Name `call_count` used when not defined
- warning[lint:unresolved-reference] tests/test_slots.py:1078:13: Name `call_count` used when not defined
+ error[lint:unresolved-reference] tests/test_slots.py:1078:13: Name `call_count` used when not defined

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- warning[lint:unresolved-reference] backend/services/user_updater_helpers.py:224:12: Name `position` used when not defined
+ error[lint:unresolved-reference] backend/services/user_updater_helpers.py:224:12: Name `position` used when not defined
- warning[lint:unresolved-reference] backend/services/user_updater_helpers.py:225:13: Name `position` used when not defined
+ error[lint:unresolved-reference] backend/services/user_updater_helpers.py:225:13: Name `position` used when not defined
- warning[lint:unresolved-reference] backend/services/utils.py:184:30: Name `next_params` used when not defined
+ error[lint:unresolved-reference] backend/services/utils.py:184:30: Name `next_params` used when not defined
- warning[lint:unresolved-reference] backend/services/utils.py:188:25: Name `next_params` used when not defined
+ error[lint:unresolved-reference] backend/services/utils.py:188:25: Name `next_params` used when not defined

kopf (https://github.com/nolar/kopf)
- warning[lint:unresolved-reference] kopf/_core/engines/probing.py:51:12: Name `probing_timestamp` used when not defined
+ error[lint:unresolved-reference] kopf/_core/engines/probing.py:51:12: Name `probing_timestamp` used when not defined
- warning[lint:unresolved-reference] kopf/_core/engines/probing.py:51:47: Name `probing_timestamp` used when not defined
+ error[lint:unresolved-reference] kopf/_core/engines/probing.py:51:47: Name `probing_timestamp` used when not defined
- warning[lint:unresolved-reference] kopf/_core/engines/probing.py:54:20: Name `probing_timestamp` used when not defined
+ error[lint:unresolved-reference] kopf/_core/engines/probing.py:54:20: Name `probing_timestamp` used when not defined
- warning[lint:unresolved-reference] kopf/_core/engines/probing.py:54:55: Name `probing_timestamp` used when not defined
+ error[lint:unresolved-reference] kopf/_core/engines/probing.py:54:55: Name `probing_timestamp` used when not defined
- warning[lint:unresolved-reference] kopf/_core/reactor/queueing.py:154:12: Name `worker_error` used when not defined
+ error[lint:unresolved-reference] kopf/_core/reactor/queueing.py:154:12: Name `worker_error` used when not defined
- warning[lint:unresolved-reference] kopf/on.py:177:52: Name `operations` used when not defined
+ error[lint:unresolved-reference] kopf/on.py:177:52: Name `operations` used when not defined
- warning[lint:unresolved-reference] kopf/on.py:237:52: Name `operations` used when not defined
+ error[lint:unresolved-reference] kopf/on.py:237:52: Name `operations` used when not defined

starlette (https://github.com/encode/starlette)
- warning[lint:unresolved-reference] starlette/testclient.py:287:16: Name `request_complete` used when not defined
+ error[lint:unresolved-reference] starlette/testclient.py:287:16: Name `request_complete` used when not defined
- warning[lint:unresolved-reference] starlette/testclient.py:316:28: Name `response_started` used when not defined
+ error[lint:unresolved-reference] starlette/testclient.py:316:28: Name `response_started` used when not defined
- warning[lint:unresolved-reference] starlette/testclient.py:321:24: Name `response_started` used when not defined
+ error[lint:unresolved-reference] starlette/testclient.py:321:24: Name `response_started` used when not defined
- warning[lint:unresolved-reference] tests/test_background.py:52:9: Name `TASK_COUNTER` used when not defined
+ error[lint:unresolved-reference] tests/test_background.py:52:9: Name `TASK_COUNTER` used when not defined
- warning[lint:unresolved-reference] tests/test_background.py:75:9: Name `TASK_COUNTER` used when not defined
+ error[lint:unresolved-reference] tests/test_background.py:75:9: Name `TASK_COUNTER` used when not defined
- warning[lint:unresolved-reference] tests/test_responses.py:113:37: Name `filled_by_bg_task` used when not defined
+ error[lint:unresolved-reference] tests/test_responses.py:113:37: Name `filled_by_bg_task` used when not defined
- warning[lint:unresolved-reference] tests/test_responses.py:228:33: Name `filled_by_bg_task` used when not defined
+ error[lint:unresolved-reference] tests/test_responses.py:228:33: Name `filled_by_bg_task` used when not defined
- warning[lint:unresolved-reference] tests/test_responses.py:576:13: Name `streamed` used when not defined
+ error[lint:unresolved-reference] tests/test_responses.py:576:13: Name `streamed` used when not defined
- warning[lint:unresolved-reference] tests/test_responses.py:605:20: Name `streamed` used when not defined
+ error[lint:unresolved-reference] tests/test_responses.py:605:20: Name `streamed` used when not defined

alerta (https://github.com/alerta/alerta)
- warning[lint:unresolved-reference] alerta/webhooks/custom.py:51:92: Name `alert` used when not defined
+ error[lint:unresolved-reference] alerta/webhooks/custom.py:51:92: Name `alert` used when not defined

trio (https://github.com/python-trio/trio)
- warning[lint:unresolved-reference] src/trio/_core/_tests/test_asyncgen.py:243:31: Name `_attempt` used when not defined
+ error[lint:unresolved-reference] src/trio/_core/_tests/test_asyncgen.py:243:31: Name `_attempt` used when not defined
- warning[lint:unresolved-reference] src/trio/_core/_tests/test_run.py:1435:9: Name `cb_counter` used when not defined
+ error[lint:unresolved-reference] src/trio/_core/_tests/test_run.py:1435:9: Name `cb_counter` used when not defined
- warning[lint:unresolved-reference] src/trio/_deprecate.py:104:12: Name `thing` used when not defined
+ error[lint:unresolved-reference] src/trio/_deprecate.py:104:12: Name `thing` used when not defined
- warning[lint:unresolved-reference] src/trio/_subprocess.py:764:26: Name `killer_cscope` used when not defined
+ error[lint:unresolved-reference] src/trio/_subprocess.py:764:26: Name `killer_cscope` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_dtls.py:527:20: Name `first_time` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_dtls.py:527:20: Name `first_time` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_dtls.py:650:12: Name `blackholed` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_dtls.py:650:12: Name `blackholed` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_repl.py:47:29: Name `__builtins__` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_repl.py:47:29: Name `__builtins__` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_ssl.py:597:13: Name `sent` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_ssl.py:597:13: Name `sent` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_ssl.py:602:19: Name `received` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_ssl.py:602:19: Name `received` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_ssl.py:604:13: Name `received` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_ssl.py:604:13: Name `received` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_ssl.py:1038:9: Name `closed` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_ssl.py:1038:9: Name `closed` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_ssl.py:1149:9: Name `transport_close_count` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_ssl.py:1149:9: Name `transport_close_count` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_sync.py:531:17: Name `acquires` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_sync.py:531:17: Name `acquires` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_sync.py:532:28: Name `in_critical_section` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_sync.py:532:28: Name `in_critical_section` used when not defined
- warning[lint:unresolved-reference] src/trio/_tests/test_testing.py:360:13: Name `resource_busy_count` used when not defined
+ error[lint:unresolved-reference] src/trio/_tests/test_testing.py:360:13: Name `resource_busy_count` used when not defined

graphql-core (https://github.com/graphql-python/graphql-core)
- warning[lint:unresolved-reference] src/graphql/execution/execute.py:2267:30: Name `awaitable_event_stream` used when not defined
+ error[lint:unresolved-reference] src/graphql/execution/execute.py:2267:30: Name `awaitable_event_stream` used when not defined
- warning[lint:unresolved-reference] src/graphql/language/visitor.py:341:44: Name `enter_list` used when not defined
+ error[lint:unresolved-reference] src/graphql/language/visitor.py:341:44: Name `enter_list` used when not defined
- warning[lint:unresolved-reference] src/graphql/language/visitor.py:341:44: Name `enter_list` used when not defined
+ error[lint:unresolved-reference] src/graphql/language/visitor.py:341:44: Name `enter_list` used when not defined
- warning[lint:unresolved-reference] src/graphql/language/visitor.py:341:44: Name `enter_list` used when not defined
+ error[lint:unresolved-reference] src/graphql/language/visitor.py:341:44: Name `enter_list` used when not defined
- warning[lint:unresolved-reference] src/graphql/language/visitor.py:354:44: Name `leave_list` used when not defined
+ error[lint:unresolved-reference] src/graphql/language/visitor.py:354:44: Name `leave_list` used when not defined
- warning[lint:unresolved-reference] src/graphql/language/visitor.py:354:44: Name `leave_list` used when not defined
+ error[lint:unresolved-reference] src/graphql/language/visitor.py:354:44: Name `leave_list` used when not defined
- warning[lint:unresolved-reference] src/graphql/language/visitor.py:354:44: Name `leave_list` used when not defined
+ error[lint:unresolved-reference] src/graphql/language/visitor.py:354:44: Name `leave_list` used when not defined
- warning[lint:unresolved-reference] tests/type/test_definition.py:367:13: Name `calls` used when not defined
+ error[lint:unresolved-reference] tests/type/test_definition.py:367:13: Name `calls` used when not defined
- warning[lint:unresolved-reference] tests/type/test_definition.py:414:13: Name `calls` used when not defined
+ error[lint:unresolved-reference] tests/type/test_definition.py:414:13: Name `calls` used when not defined
- warning[lint:unresolved-reference] tests/type/test_definition.py:567:13: Name `calls` used when not defined
+ error[lint:unresolved-reference] tests/type/test_definition.py:567:13: Name `calls` used when not defined
- warning[lint:unresolved-reference] tests/type/test_definition.py:593:13: Name `calls` used when not defined
+ error[lint:unresolved-reference] tests/type/test_definition.py:593:13: Name `calls` used when not defined
- warning[lint:unresolved-reference] tests/utils/assert_equal_awaitables_or_values.py:21:67: Name `awaitable_items` used when not defined
+ error[lint:unresolved-reference] tests/utils/assert_equal_awaitables_or_values.py:21:67: Name `awaitable_items` used when not defined

kornia (https://github.com/kornia/kornia)
- warning[lint:unresolved-reference] kornia/feature/dedode/descriptor.py:41:32: Name `descriptions` used when not defined
+ error[lint:unresolved-reference] kornia/feature/dedode/descriptor.py:41:32: Name `descriptions` used when not defined
- warning[lint:unresolved-reference] kornia/onnx/utils.py:68:37: Name `file_path` used when not defined
+ error[lint:unresolved-reference] kornia/onnx/utils.py:68:37: Name `file_path` used when not defined
- warning[lint:unresolved-reference] kornia/utils/helpers.py:244:12: Name `out` used when not defined
+ error[lint:unresolved-reference] kornia/utils/helpers.py:244:12: Name `out` used when not defined
- warning[lint:unresolved-reference] kornia/utils/helpers.py:292:26: Name `info` used when not defined
+ error[lint:unresolved-reference] kornia/utils/helpers.py:292:26: Name `info` used when not defined
- warning[lint:unresolved-reference] kornia/utils/helpers.py:307:12: Name `X` used when not defined
+ error[lint:unresolved-reference] kornia/utils/helpers.py:307:12: Name `X` used when not defined
- warning[lint:unresolved-reference] kornia/utils/helpers.py:307:27: Name `A_LU` used when not defined
+ error[lint:unresolved-reference] kornia/utils/helpers.py:307:27: Name `A_LU` used when not defined

comtypes (https://github.com/enthought/comtypes)
- warning[lint:unresolved-reference] comtypes/_comobject.py:417:19: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/_comobject.py:417:19: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/_comobject.py:425:19: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/_comobject.py:425:19: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/_memberspec.py:588:46: Name `interface` used when not defined
+ error[lint:unresolved-reference] comtypes/_memberspec.py:588:46: Name `interface` used when not defined
- warning[lint:unresolved-reference] comtypes/_post_coinit/unknwn.py:31:8: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/_post_coinit/unknwn.py:31:8: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/_post_coinit/unknwn.py:36:16: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/_post_coinit/unknwn.py:36:16: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/_vtbl.py:59:26: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/_vtbl.py:59:26: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/_vtbl.py:96:27: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/_vtbl.py:96:27: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/_vtbl.py:204:16: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/_vtbl.py:204:16: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:577:23: Name `WinDLL` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:577:23: Name `WinDLL` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:583:13: Name `OleDLL` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:583:13: Name `OleDLL` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:587:30: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:587:30: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:591:25: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:591:25: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:595:24: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:595:24: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:599:27: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:599:27: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:685:9: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:685:9: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:691:19: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:691:19: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:692:19: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:692:19: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:694:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:694:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:781:23: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:781:23: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:784:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:784:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:794:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:794:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:799:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:799:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/automation.py:964:5: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/automation.py:964:5: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/client/_events.py:107:27: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/client/_events.py:107:27: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/client/_events.py:384:16: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/client/_events.py:384:16: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/client/dynamic.py:30:27: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/client/dynamic.py:30:27: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/errorinfo.py:129:16: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/errorinfo.py:129:16: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/server/inprocserver.py:95:12: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/server/inprocserver.py:95:12: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/server/register.py:108:12: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/server/register.py:108:12: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/server/register.py:145:16: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/server/register.py:145:16: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/server/register.py:169:20: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/server/register.py:169:20: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/server/register.py:234:20: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/server/register.py:234:20: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/server/register.py:375:30: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/server/register.py:375:30: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_GUID.py:33:32: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_GUID.py:33:32: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_GUID.py:41:32: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_GUID.py:41:32: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_GUID.py:49:32: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_GUID.py:49:32: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_agilent.py:15:8: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_agilent.py:15:8: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_client_dynamic.py:29:39: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_client_dynamic.py:29:39: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_comserver.py:25:12: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_comserver.py:25:12: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_createwrappers.py:54:16: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_createwrappers.py:54:16: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_dispinterface.py:19:12: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_dispinterface.py:19:12: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_getactiveobj.py:32:16: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_getactiveobj.py:32:16: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_getactiveobj.py:66:32: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_getactiveobj.py:66:32: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_msscript.py:14:8: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_msscript.py:14:8: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/test/test_typeinfo.py:20:32: Name `WindowsError` used when not defined
+ error[lint:unresolved-reference] comtypes/test/test_typeinfo.py:20:32: Name `WindowsError` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:32:21: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:32:21: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:710:12: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:710:12: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:725:12: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:725:12: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:734:12: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:734:12: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/helpers.py:124:12: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/helpers.py:124:12: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/helpers.py:228:12: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/helpers.py:228:12: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/helpers.py:287:12: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/helpers.py:287:12: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] comtypes/tools/codegenerator/packing.py:58:43: Name `details` used when not defined
+ error[lint:unresolved-reference] comtypes/tools/codegenerator/packing.py:58:43: Name `details` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:80:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:80:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:95:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:95:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:106:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:106:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:113:23: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:113:23: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:116:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:116:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:124:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:124:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:140:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:140:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:157:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:157:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:163:17: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:163:17: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:167:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:167:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:177:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:177:13: Name `HRESULT` used when not defined
- warning[lint:unresolved-reference] comtypes/viewobject.py:187:13: Name `HRESULT` used when not defined
+ error[lint:unresolved-reference] comtypes/viewobject.py:187:13: Name `HRESULT` used when not defined

mkosi (https://github.com/systemd/mkosi)
- warning[lint:unresolved-reference] mkosi/__init__.py:5152:80: Name `c` used when not defined
+ error[lint:unresolved-reference] mkosi/__init__.py:5152:80: Name `c` used when not defined
- warning[lint:unresolved-reference] mkosi/__init__.py:5157:79: Name `c` used when not defined
+ error[lint:unresolved-reference] mkosi/__init__.py:5157:79: Name `c` used when not defined
- warning[lint:unresolved-reference] mkosi/config.py:4474:41: Name `missing` used when not defined
+ error[lint:unresolved-reference] mkosi/config.py:4474:41: Name `missing` used when not defined

porcupine (https://github.com/Akuli/porcupine)
- warning[lint:unresolved-reference] porcupine/plugins/aboutdialog.py:65:26: Name `fall_speed` used when not defined
+ error[lint:unresolved-reference] porcupine/plugins/aboutdialog.py:65:26: Name `fall_speed` used when not defined
- warning[lint:unresolved-reference] porcupine/plugins/aboutdialog.py:66:13: Name `fall_speed` used when not defined
+ error[lint:unresolved-reference] porcupine/plugins/aboutdialog.py:66:13: Name `fall_speed` used when not defined
- warning[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:73:16: Name `timeout_scheduled` used when not defined
+ error[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:73:16: Name `timeout_scheduled` used when not defined
- warning[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:74:12: Name `running_requested` used when not defined
+ error[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:74:12: Name `running_requested` used when not defined
- warning[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:83:12: Name `timeout_scheduled` used when not defined
+ error[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:83:12: Name `timeout_scheduled` used when not defined
- warning[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:86:24: Name `running_requested` used when not defined
+ error[lint:unresolved-reference] porcupine/plugins/highlight/__init__.py:86:24: Name `running_requested` used when not defined
- warning[lint:unresolved-reference] porcupine/plugins/tab_order.py:81:13: Name `accumulator` used when not defined
+ error[lint:unresolved-reference] porcupine/plugins/tab_order.py:81:13: Name `accumulator` used when not defined
- warning[lint:unresolved-reference] porcupine/textutils.py:102:27: Name `old_cursor_pos` used when not defined
+ error[lint:unresolved-reference] porcupine/textutils.py:102:27: Name `old_cursor_pos` used when not defined
- warning[lint:unresolved-reference] porcupine/utils.py:794:37: Name `value` used when not defined
+ error[lint:unresolved-reference] porcupine/utils.py:794:37: Name `value` used when not defined

ignite (https://github.com/pytorch/ignite)
- warning[lint:unresolved-reference] examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:15:1: Name `display` used when not defined
+ error[lint:unresolved-reference] examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:15:1: Name `display` used when not defined
- warning[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:265:16: Name `ignore_idx` used when not defined
+ error[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:265:16: Name `ignore_idx` used when not defined
- warning[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:266:50: Name `ignore_idx` used when not defined
+ error[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:266:50: Name `ignore_idx` used when not defined
- warning[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:268:28: Name `ignore_idx` used when not defined
+ error[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:268:28: Name `ignore_idx` used when not defined
- warning[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:422:16: Name `ignore_idx` used when not defined
+ error[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:422:16: Name `ignore_idx` used when not defined
- warning[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:424:37: Name `ignore_idx` used when not defined
+ error[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:424:37: Name `ignore_idx` used when not defined
- warning[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:427:28: Name `ignore_idx` used when not defined
+ error[lint:unresolved-reference] ignite/metrics/confusion_matrix.py:427:28: Name `ignore_idx` used when not defined
- warning[lint:unresolved-reference] tests/ignite/conftest.py:106:9: Name `path` used when not defined
+ error[lint:unresolved-reference] tests/ignite/conftest.py:106:9: Name `path` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:389:9: Name `num_calls` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:389:9: Name `num_calls` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:416:9: Name `num_calls` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:416:9: Name `num_calls` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:436:9: Name `num_calls` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:436:9: Name `num_calls` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:459:9: Name `num_calls` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_custom_events.py:459:9: Name `num_calls` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:173:17: Name `expected_iter` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:173:17: Name `expected_iter` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:177:55: Name `expected_data_iter` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:177:55: Name `expected_data_iter` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:223:13: Name `num_calls_check_iter_epoch` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:223:13: Name `num_calls_check_iter_epoch` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:232:17: Name `expected_iter` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:232:17: Name `expected_iter` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:350:16: Name `call_count` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:350:16: Name `call_count` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:357:13: Name `call_count` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:357:13: Name `call_count` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:1437:9: Name `num_calls_check_iter_epoch` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:1437:9: Name `num_calls_check_iter_epoch` used when not defined
- warning[lint:unresolved-reference] tests/ignite/engine/test_engine.py:1484:9: Name `num_calls_check_iter_epoch` used when not defined
+ error[lint:unresolved-reference] tests/ignite/engine/test_engine.py:1484:9: Name `num_calls_check_iter_epoch` used when not defined

pydantic (https://github.com/pydantic/pydantic)
- warning[lint:unresolved-reference] pydantic/_internal/_decorators_v1.py:96:20: Name `val1` used when not defined
+ error[lint:unresolved-reference] pydantic/_internal/_decorators_v1.py:96:20: Name `val1` used when not defined
- warning[lint:unresolved-reference] pydantic/_internal/_model_construction.py:126:25: Name `original_model_post_init` used when not defined
+ error[lint:unresolved-reference] pydantic/_internal/_model_construction.py:126:25: Name `original_model_post_init` used when not defined
- warning[lint:unresolved-reference] pydantic/fields.py:1549:12: Name `description` used when not defined
+ error[lint:unresolved-reference] pydantic/fields.py:1549:12: Name `description` used when not defined
- warning[lint:unresolved-reference] pydantic/fields.py:1552:12: Name `deprecated` used when not defined
+ error[lint:unresolved-reference] pydantic/fields.py:1552:12: Name `deprecated` used when not defined
- warning[lint:unresolved-reference] pydantic/fields.py:1557:27: Name `alias_priority` used when not defined
+ error[lint:unresolved-reference] pydantic/fields.py:1557:27: Name `alias_priority` used when not defined

paasta (https://github.com/yelp/paasta)
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/deploy_queue.py:84:46: Name `DeployQueueServiceInstance` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/deploy_queue.py:84:46: Name `DeployQueueServiceInstance` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/deploy_queue.py:85:48: Name `DeployQueueServiceInstance` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/deploy_queue.py:85:48: Name `DeployQueueServiceInstance` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/envoy_location.py:84:27: Name `EnvoyBackend` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/envoy_location.py:84:27: Name `EnvoyBackend` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/envoy_status.py:85:28: Name `EnvoyLocation` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/envoy_status.py:85:28: Name `EnvoyLocation` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/flink_jobs.py:84:23: Name `FlinkJob` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/flink_jobs.py:84:23: Name `FlinkJob` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_mesh_status.py:88:28: Name `SmartstackStatus` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_mesh_status.py:88:28: Name `SmartstackStatus` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_mesh_status.py:89:23: Name `EnvoyStatus` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_mesh_status.py:89:23: Name `EnvoyStatus` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:96:23: Name `InstanceStatusAdhoc` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:96:23: Name `InstanceStatusAdhoc` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:97:23: Name `InstanceStatusFlink` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:97:23: Name `InstanceStatusFlink` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:98:26: Name `InstanceStatusFlink` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:98:26: Name `InstanceStatusFlink` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:102:34: Name `InstanceStatusCassandracluster` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:102:34: Name `InstanceStatusCassandracluster` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:103:30: Name `InstanceStatusKafkacluster` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:103:30: Name `InstanceStatusKafkacluster` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:104:28: Name `InstanceStatusKubernetes` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:104:28: Name `InstanceStatusKubernetes` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:105:31: Name `InstanceStatusKubernetesV2` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:105:31: Name `InstanceStatusKubernetesV2` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:107:22: Name `InstanceStatusTron` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status.py:107:22: Name `InstanceStatusTron` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_adhoc.py:80:24: Name `AdhocLaunchHistory` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_adhoc.py:80:24: Name `AdhocLaunchHistory` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_adhoc.py:152:30: Name `_path_to_item` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_adhoc.py:152:30: Name `_path_to_item` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:116:36: Name `InstanceStatusKubernetesAutoscalingStatus` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:116:36: Name `InstanceStatusKubernetesAutoscalingStatus` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:124:23: Name `KubernetesPod` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:124:23: Name `KubernetesPod` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:125:30: Name `KubernetesReplicaSet` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:125:30: Name `KubernetesReplicaSet` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:127:28: Name `SmartstackStatus` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:127:28: Name `SmartstackStatus` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:128:23: Name `EnvoyStatus` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes.py:128:23: Name `EnvoyStatus` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_autoscaling_status.py:87:26: Name `HPAMetric` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_autoscaling_status.py:87:26: Name `HPAMetric` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_v2.py:89:36: Name `InstanceStatusKubernetesAutoscalingStatus` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_v2.py:89:36: Name `InstanceStatusKubernetesAutoscalingStatus` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_v2.py:93:27: Name `KubernetesVersion` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_v2.py:93:27: Name `KubernetesVersion` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_v2.py:94:23: Name `EnvoyStatus` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_status_kubernetes_v2.py:94:23: Name `EnvoyStatus` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_tasks.py:147:30: Name `_path_to_item` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/instance_tasks.py:147:30: Name `_path_to_item` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container.py:85:28: Name `TaskTailLines` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container.py:85:28: Name `TaskTailLines` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container_v2.py:96:37: Name `TaskTailLines` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container_v2.py:96:37: Name `TaskTailLines` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container_v2.py:99:33: Name `KubernetesHealthcheck` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container_v2.py:99:33: Name `KubernetesHealthcheck` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container_v2.py:100:28: Name `TaskTailLines` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_container_v2.py:100:28: Name `TaskTailLines` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod.py:86:29: Name `KubernetesContainer` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod.py:86:29: Name `KubernetesContainer` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod.py:94:25: Name `KubernetesPodEvent` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod.py:94:25: Name `KubernetesPodEvent` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod_v2.py:97:25: Name `KubernetesPodEvent` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod_v2.py:97:25: Name `KubernetesPodEvent` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod_v2.py:98:29: Name `KubernetesContainerV2` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_pod_v2.py:98:29: Name `KubernetesContainerV2` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_version.py:90:23: Name `KubernetesPodV2` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/kubernetes_version.py:90:23: Name `KubernetesPodV2` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/resource.py:80:24: Name `ResourceItem` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/resource.py:80:24: Name `ResourceItem` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/resource.py:152:30: Name `_path_to_item` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/resource.py:152:30: Name `_path_to_item` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:84:22: Name `ResourceValue` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:84:22: Name `ResourceValue` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:85:22: Name `ResourceValue` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:85:22: Name `ResourceValue` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:87:21: Name `ResourceValue` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:87:21: Name `ResourceValue` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:88:22: Name `ResourceValue` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/resource_item.py:88:22: Name `ResourceValue` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/smartstack_location.py:84:27: Name `SmartstackBackend` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/smartstack_location.py:84:27: Name `SmartstackBackend` used when not defined
- warning[lint:unresolved-reference] paasta_tools/paastaapi/model/smartstack_status.py:85:28: Name `SmartstackLocation` used when not defined
+ error[lint:unresolved-reference] paasta_tools/paastaapi/model/smartstack_status.py:85:28: Name `SmartstackLocation` used when not defined

pip (https://github.com/pypa/pip)
- warning[lint:unresolved-reference] src/pip/_vendor/certifi/core.py:109:13: Name `os` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/certifi/core.py:109:13: Name `os` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/certifi/core.py:111:16: Name `os` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/certifi/core.py:111:16: Name `os` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/distlib/compat.py:504:37: Name `get_ident` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/distlib/compat.py:504:37: Name `get_ident` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/distlib/compat.py:635:30: Name `__debug__` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/distlib/compat.py:635:30: Name `__debug__` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/requests/status_codes.py:122:9: Name `__doc__` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/requests/status_codes.py:122:9: Name `__doc__` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/requests/status_codes.py:123:12: Name `__doc__` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/requests/status_codes.py:123:12: Name `__doc__` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/rich/traceback.py:179:24: Name `tb_data` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/rich/traceback.py:179:24: Name `tb_data` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/rich/traceback.py:180:25: Name `tb_data` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/rich/traceback.py:180:25: Name `tb_data` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3210:24: Name `original_new` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3210:24: Name `original_new` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3211:32: Name `original_new` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3211:32: Name `original_new` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3216:32: Name `original_new` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3216:32: Name `original_new` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3229:32: Name `original_init_subclass` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3229:32: Name `original_init_subclass` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3238:32: Name `original_init_subclass` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/typing_extensions.py:3238:32: Name `original_init_subclass` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:824:37: Name `basestring` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:824:37: Name `basestring` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:828:32: Name `file` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:828:32: Name `file` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:829:38: Name `unicode` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:829:38: Name `unicode` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:855:36: Name `unicode` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:855:36: Name `unicode` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:859:23: Name `unicode` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:859:23: Name `unicode` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:860:21: Name `unicode` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/urllib3/packages/six.py:860:21: Name `unicode` used when not defined
- warning[lint:unresolved-reference] src/pip/_vendor/urllib3/response.py:152:16: Name `BrotliDecoder` used when not defined
+ error[lint:unresolved-reference] src/pip/_vendor/urllib3/response.py:152:16: Name `BrotliDecoder` used when not defined

ppb-vector (https://github.com/ppb/ppb-vector)
- warning[lint:unresolved-reference] tests/test_typing.py:9:10: Name `vectors` used when not defined
+ error[lint:unresolved-reference] tests/test_typing.py:9:10: Name `vectors` used when not defined
- warning[lint:unresolved-reference] tests/test_typing.py:9:23: Name `units` used when not defined
+ error[lint:unresolved-reference] tests/test_typing.py:9:23: Name `units` used when not defined
- warning[lint:unresolved-reference] tests/test_typing.py:14:19: Name `vector_likes` used when not defined
+ error[lint:unresolved-reference] tests/test_typing.py:14:19: Name `vector_likes` used when not defined

pybind11 (https://github.com/pybind/pybind11)
- warning[lint:unresolved-reference] tests/test_eval_call.py:5:5: Name `call_test2` used when not defined
+ error[lint:unresolved-reference] tests/test_eval_call.py:5:5: Name `call_test2` used when not defined
- warning[lint:unresolved-reference] tests/test_eval_call.py:5:16: Name `y` used when not defined
+ error[lint:unresolved-reference] tests/test_eval_call.py:5:16: Name `y` used when not defined

black (https://github.com/psf/black)
- warning[lint:unresolved-reference] src/black/linegen.py:1284:13: Name `current_line` used when not defined
+ error[lint:unresolved-reference] src/black/linegen.py:1284:13: Name `current_line` used when not defined
- warning[lint:unresolved-reference] src/black/linegen.py:1286:19: Name `current_line` used when not defined
+ error[lint:unresolved-reference] src/black/linegen.py:1286:19: Name `current_line` used when not defined
- warning[lint:unresolved-reference] src/black/linegen.py:1355:13: Name `current_line` used when not defined
+ error[lint:unresolved-reference] src/black/linegen.py:1355:13: Name `current_line` used when not defined
- warning[lint:unresolved-reference] src/black/linegen.py:1357:19: Name `current_line` used when not defined
+ error[lint:unresolved-reference] src/black/linegen.py:1357:19: Name `current_line` used when not defined
- warning[lint:unresolved-reference] src/black/trans.py:2477:16: Name `string_child_idx` used when not defined
+ error[lint:unresolved-reference] src/black/trans.py:2477:16: Name `string_child_idx` used when not defined
- warning[lint:unresolved-reference] src/black/trans.py:2479:36: Name `string_child_idx` used when not defined
+ error[lint:unresolved-reference] src/black/trans.py:2479:36: Name `string_child_idx` used when not defined
- warning[lint:unresolved-reference] src/black/trans.py:2480:9: Name `string_child_idx` used when not defined
+ error[lint:unresolved-reference] src/black/trans.py:2480:9: Name `string_child_idx` used...*[Comment body truncated]*

---

_@AlexWaygood approved on 2025-05-08 07:29_

---

_Merged by @MichaReiser on 2025-05-08 15:54_

---

_Closed by @MichaReiser on 2025-05-08 15:54_

---

_Branch deleted on 2025-05-08 15:54_

---
