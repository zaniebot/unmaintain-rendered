```yaml
number: 22580
title: "[ty] Use let-chains more"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/let-chains
created_at: 2026-01-14T18:25:41Z
updated_at: 2026-01-14T19:56:09Z
url: https://github.com/astral-sh/ruff/pull/22580
synced_at: 2026-01-14T20:43:42Z
```

# [ty] Use let-chains more

---

_@AlexWaygood_

## Summary

just a little refactor.

Edit: okay, I removed a period at the end of a diagnostic message, which I guess changes a _lot_ of diagnostic messages.


---

_Label `internal` added by @AlexWaygood on 2026-01-14 18:25_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-14 18:25_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-14 18:25_

---

_Label `ty` added by @AlexWaygood on 2026-01-14 18:25_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-14 18:25_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:27_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-14 18:27:10.983428750 +0000
+++ new-output.txt	2026-01-14 18:27:11.317430379 +0000
@@ -149,7 +149,7 @@
 callables_protocol.py:121:18: error[invalid-assignment] Object of type `def cb6_bad1(*vals: bytes, *, max_len: int | None = None) -> list[bytes]` is not assignable to `NotProto6`
 callables_protocol.py:169:7: error[invalid-assignment] Object of type `def cb8_bad1(x: int) -> Any` is not assignable to `Proto8`
 callables_protocol.py:186:5: error[invalid-assignment] Object of type `Literal["str"]` is not assignable to attribute `other_attribute` of type `int`
-callables_protocol.py:187:5: error[unresolved-attribute] Unresolved attribute `xxx` on type `Proto9[P@decorator1, R@decorator1]`.
+callables_protocol.py:187:5: error[unresolved-attribute] Unresolved attribute `xxx` on type `Proto9[P@decorator1, R@decorator1]`
 callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[(x: int), str]` has no attribute `other_attribute2`
 callables_protocol.py:238:8: error[invalid-assignment] Object of type `def cb11_bad1(x: int, y: str, /) -> Any` is not assignable to `Proto11`
 callables_protocol.py:260:8: error[invalid-assignment] Object of type `def cb12_bad1(*args: Any, *, kwarg0: Any) -> None` is not assignable to `Proto12`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/_make.py:1198:9: error[unresolved-attribute] Unresolved attribute `__qualname__` on type `(...) -> Unknown`.
+ src/attr/_make.py:1198:9: error[unresolved-attribute] Unresolved attribute `__qualname__` on type `(...) -> Unknown`
- src/attr/_make.py:1214:13: error[unresolved-attribute] Unresolved attribute `__qualname__` on type `(...) -> Unknown`.
+ src/attr/_make.py:1214:13: error[unresolved-attribute] Unresolved attribute `__qualname__` on type `(...) -> Unknown`
- tests/test_slots.py:96:5: error[unresolved-attribute] Unresolved attribute `t` on type `C1`.
+ tests/test_slots.py:96:5: error[unresolved-attribute] Unresolved attribute `t` on type `C1`
- tests/test_slots.py:98:9: error[unresolved-attribute] Unresolved attribute `t` on type `C1Slots`.
+ tests/test_slots.py:98:9: error[unresolved-attribute] Unresolved attribute `t` on type `C1Slots`
- tests/test_slots.py:161:5: error[unresolved-attribute] Unresolved attribute `t` on type `C2Slots`.
+ tests/test_slots.py:161:5: error[unresolved-attribute] Unresolved attribute `t` on type `C2Slots`
- tests/test_slots.py:222:9: error[unresolved-attribute] Unresolved attribute `t` on type `SimpleOrdinaryClass`.
+ tests/test_slots.py:222:9: error[unresolved-attribute] Unresolved attribute `t` on type `SimpleOrdinaryClass`
- tests/test_slots.py:267:9: error[unresolved-attribute] Unresolved attribute `t` on type `C2Slots`.
+ tests/test_slots.py:267:9: error[unresolved-attribute] Unresolved attribute `t` on type `C2Slots`
- tests/test_slots.py:411:9: error[unresolved-attribute] Unresolved attribute `t` on type `C2Slots`.
+ tests/test_slots.py:411:9: error[unresolved-attribute] Unresolved attribute `t` on type `C2Slots`

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:289:9: error[unresolved-attribute] Unresolved attribute `_executor_shutdown_called` on type `AbstractEventLoop`.
+ src/anyio/_backends/_asyncio.py:289:9: error[unresolved-attribute] Unresolved attribute `_executor_shutdown_called` on type `AbstractEventLoop`

spack (https://github.com/spack/spack)
- lib/spack/spack/config.py:1887:21: error[unresolved-attribute] Unresolved attribute `append` on type `syaml_str`.
+ lib/spack/spack/config.py:1887:21: error[unresolved-attribute] Unresolved attribute `append` on type `syaml_str`
- lib/spack/spack/config.py:1889:21: error[unresolved-attribute] Unresolved attribute `prepend` on type `syaml_str`.
+ lib/spack/spack/config.py:1889:21: error[unresolved-attribute] Unresolved attribute `prepend` on type `syaml_str`
- lib/spack/spack/config.py:1891:21: error[unresolved-attribute] Unresolved attribute `override` on type `syaml_str`.
+ lib/spack/spack/config.py:1891:21: error[unresolved-attribute] Unresolved attribute `override` on type `syaml_str`
- lib/spack/spack/cray_manifest.py:182:5: error[unresolved-attribute] Unresolved attribute `_hashes_final` on type `Spec`.
+ lib/spack/spack/cray_manifest.py:182:5: error[unresolved-attribute] Unresolved attribute `_hashes_final` on type `Spec`
- lib/spack/spack/cray_manifest.py:184:5: error[unresolved-attribute] Unresolved attribute `origin` on type `Spec`.
+ lib/spack/spack/cray_manifest.py:184:5: error[unresolved-attribute] Unresolved attribute `origin` on type `Spec`
- lib/spack/spack/main.py:606:9: error[unresolved-attribute] Unresolved attribute `override` on type `syaml_str`.
+ lib/spack/spack/main.py:606:9: error[unresolved-attribute] Unresolved attribute `override` on type `syaml_str`
- lib/spack/spack/test/cmd/find.py:87:5: error[unresolved-attribute] Unresolved attribute `explicit` on type `Bunch`.
+ lib/spack/spack/test/cmd/find.py:87:5: error[unresolved-attribute] Unresolved attribute `explicit` on type `Bunch`
- lib/spack/spack/test/cmd/find.py:91:5: error[unresolved-attribute] Unresolved attribute `explicit` on type `Bunch`.
+ lib/spack/spack/test/cmd/find.py:91:5: error[unresolved-attribute] Unresolved attribute `explicit` on type `Bunch`
- lib/spack/spack/test/cmd/find.py:92:5: error[unresolved-attribute] Unresolved attribute `implicit` on type `Bunch`.
+ lib/spack/spack/test/cmd/find.py:92:5: error[unresolved-attribute] Unresolved attribute `implicit` on type `Bunch`
- lib/spack/spack/test/concretization/core.py:1903:9: error[unresolved-attribute] Unresolved attribute `reuse` on type `Solver`.
+ lib/spack/spack/test/concretization/core.py:1903:9: error[unresolved-attribute] Unresolved attribute `reuse` on type `Solver`
- lib/spack/spack/test/concretization/core.py:1947:9: error[unresolved-attribute] Unresolved attribute `reuse` on type `Solver`.
+ lib/spack/spack/test/concretization/core.py:1947:9: error[unresolved-attribute] Unresolved attribute `reuse` on type `Solver`
- lib/spack/spack/test/concretization/core.py:1961:9: error[unresolved-attribute] Unresolved attribute `reuse` on type `Solver`.
+ lib/spack/spack/test/concretization/core.py:1961:9: error[unresolved-attribute] Unresolved attribute `reuse` on type `Solver`
- lib/spack/spack/test/spec_semantics.py:1787:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`.
+ lib/spack/spack/test/spec_semantics.py:1787:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`
- lib/spack/spack/test/spec_syntax.py:1365:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`.
+ lib/spack/spack/test/spec_syntax.py:1365:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`
- lib/spack/spack/test/spec_syntax.py:1438:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`.
+ lib/spack/spack/test/spec_syntax.py:1438:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`
- lib/spack/spack/test/spec_syntax.py:1439:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`.
+ lib/spack/spack/test/spec_syntax.py:1439:5: error[unresolved-attribute] Unresolved attribute `_hash` on type `Spec`
- lib/spack/spack/util/spack_yaml.py:171:13: error[unresolved-attribute] Unresolved attribute `override` on type `syaml_str`.
+ lib/spack/spack/util/spack_yaml.py:171:13: error[unresolved-attribute] Unresolved attribute `override` on type `syaml_str`
- lib/spack/spack/vendor/jinja2/async_utils.py:41:9: error[unresolved-attribute] Unresolved attribute `jinja_async_variant` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ lib/spack/spack/vendor/jinja2/async_utils.py:41:9: error[unresolved-attribute] Unresolved attribute `jinja_async_variant` on type `_Wrapped[(...), Unknown, (...), Unknown]`
- lib/spack/spack/vendor/typing_extensions.py:1073:5: error[unresolved-attribute] Unresolved attribute `__text_signature__` on type `def _dict_new(...) -> Unknown`.
+ lib/spack/spack/vendor/typing_extensions.py:1073:5: error[unresolved-attribute] Unresolved attribute `__text_signature__` on type `def _dict_new(...) -> Unknown`
- lib/spack/spack/vendor/typing_extensions.py:1119:5: error[unresolved-attribute] Unresolved attribute `__text_signature__` on type `def _typeddict_new(*args, *, total=True, **kwargs) -> Unknown`.
+ lib/spack/spack/vendor/typing_extensions.py:1119:5: error[unresolved-attribute] Unresolved attribute `__text_signature__` on type `def _typeddict_new(*args, *, total=True, **kwargs) -> Unknown`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/distlib/compat.py:1026:13: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingDict`.
+ src/pip/_vendor/distlib/compat.py:1026:13: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingDict`
- src/pip/_vendor/distlib/compat.py:1099:17: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingDict`.
+ src/pip/_vendor/distlib/compat.py:1099:17: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingDict`
- src/pip/_vendor/distlib/compat.py:1103:17: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingList`.
+ src/pip/_vendor/distlib/compat.py:1103:17: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingList`
- src/pip/_vendor/distlib/compat.py:1106:17: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingTuple`.
+ src/pip/_vendor/distlib/compat.py:1106:17: error[unresolved-attribute] Unresolved attribute `configurator` on type `ConvertingTuple`
- src/pip/_vendor/pygments/token.py:80:1: error[unresolved-attribute] Unresolved attribute `Token` on type `_TokenType`.
+ src/pip/_vendor/pygments/token.py:80:1: error[unresolved-attribute] Unresolved attribute `Token` on type `_TokenType`
- src/pip/_vendor/pygments/token.py:81:1: error[unresolved-attribute] Unresolved attribute `String` on type `_TokenType`.
+ src/pip/_vendor/pygments/token.py:81:1: error[unresolved-attribute] Unresolved attribute `String` on type `_TokenType`
- src/pip/_vendor/pygments/token.py:82:1: error[unresolved-attribute] Unresolved attribute `Number` on type `_TokenType`.
+ src/pip/_vendor/pygments/token.py:82:1: error[unresolved-attribute] Unresolved attribute `Number` on type `_TokenType`
- src/pip/_vendor/requests/adapters.py:369:9: error[unresolved-attribute] Unresolved attribute `connection` on type `Response`.
+ src/pip/_vendor/requests/adapters.py:369:9: error[unresolved-attribute] Unresolved attribute `connection` on type `Response`
- src/pip/_vendor/urllib3/connection.py:554:9: error[unresolved-attribute] Unresolved attribute `_peer_cert` on type `CertificateError`.
+ src/pip/_vendor/urllib3/connection.py:554:9: error[unresolved-attribute] Unresolved attribute `_peer_cert` on type `CertificateError`
- src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:52:9: error[unresolved-attribute] Unresolved attribute `weakref` on type `_Info`.
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:52:9: error[unresolved-attribute] Unresolved attribute `weakref` on type `_Info`
- src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:53:9: error[unresolved-attribute] Unresolved attribute `func` on type `_Info`.
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:53:9: error[unresolved-attribute] Unresolved attribute `func` on type `_Info`
- src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:54:9: error[unresolved-attribute] Unresolved attribute `args` on type `_Info`.
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:54:9: error[unresolved-attribute] Unresolved attribute `args` on type `_Info`
- src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:55:9: error[unresolved-attribute] Unresolved attribute `kwargs` on type `_Info`.
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:55:9: error[unresolved-attribute] Unresolved attribute `kwargs` on type `_Info`
- src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:56:9: error[unresolved-attribute] Unresolved attribute `atexit` on type `_Info`.
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:56:9: error[unresolved-attribute] Unresolved attribute `atexit` on type `_Info`
- src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:57:9: error[unresolved-attribute] Unresolved attribute `index` on type `_Info`.
+ src/pip/_vendor/urllib3/packages/backports/weakref_finalize.py:57:9: error[unresolved-attribute] Unresolved attribute `index` on type `_Info`

beartype (https://github.com/beartype/beartype)
- beartype/_check/forward/reference/fwdrefmake.py:301:5: error[unresolved-attribute] Unresolved attribute `__name_beartype__` on type `type`.
+ beartype/_check/forward/reference/fwdrefmake.py:301:5: error[unresolved-attribute] Unresolved attribute `__name_beartype__` on type `type`
- beartype/_check/forward/reference/fwdrefmake.py:302:5: error[unresolved-attribute] Unresolved attribute `__scope_name_beartype__` on type `type`.
+ beartype/_check/forward/reference/fwdrefmake.py:302:5: error[unresolved-attribute] Unresolved attribute `__scope_name_beartype__` on type `type`

stone (https://github.com/dropbox/stone)
- test/test_python_gen.py:287:9: error[unresolved-attribute] Unresolved attribute `f` on type `S`.
+ test/test_python_gen.py:287:9: error[unresolved-attribute] Unresolved attribute `f` on type `S`
- test/test_python_gen.py:374:9: error[unresolved-attribute] Unresolved attribute `f` on type `S`.
+ test/test_python_gen.py:374:9: error[unresolved-attribute] Unresolved attribute `f` on type `S`
- test/test_python_gen.py:376:9: error[unresolved-attribute] Unresolved attribute `i` on type `S2`.
+ test/test_python_gen.py:376:9: error[unresolved-attribute] Unresolved attribute `i` on type `S2`
- test/test_stone_internal.py:399:9: error[unresolved-attribute] Unresolved attribute `nullable` on type `Struct`.
+ test/test_stone_internal.py:399:9: error[unresolved-attribute] Unresolved attribute `nullable` on type `Struct`

jinja (https://github.com/pallets/jinja)
- tests/test_ext.py:334:9: error[unresolved-attribute] Unresolved attribute `called` on type `def get_user_count() -> Unknown`.
+ tests/test_ext.py:334:9: error[unresolved-attribute] Unresolved attribute `called` on type `def get_user_count() -> Unknown`

asynq (https://github.com/quora/asynq)
- asynq/debug.py:51:1: error[unresolved-attribute] Unresolved attribute `MAX_TASK_STACK_SIZE` on type `DebugOptions`.
+ asynq/debug.py:51:1: error[unresolved-attribute] Unresolved attribute `MAX_TASK_STACK_SIZE` on type `DebugOptions`
- asynq/debug.py:54:1: error[unresolved-attribute] Unresolved attribute `KEEP_DEPENDENCIES` on type `DebugOptions`.
+ asynq/debug.py:54:1: error[unresolved-attribute] Unresolved attribute `KEEP_DEPENDENCIES` on type `DebugOptions`
- asynq/decorators.py:95:9: error[unresolved-attribute] Unresolved attribute `is_pure_async_fn` on type `def sync_to_async_fn_wrapper(...) -> Unknown`.
+ asynq/decorators.py:95:9: error[unresolved-attribute] Unresolved attribute `is_pure_async_fn` on type `def sync_to_async_fn_wrapper(...) -> Unknown`
- asynq/mock_.py:96:1: error[unresolved-attribute] Unresolved attribute `object` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:96:1: error[unresolved-attribute] Unresolved attribute `object` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`
- asynq/mock_.py:98:1: error[unresolved-attribute] Unresolved attribute `dict` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:98:1: error[unresolved-attribute] Unresolved attribute `dict` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`
- asynq/mock_.py:99:1: error[unresolved-attribute] Unresolved attribute `TEST_PREFIX` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:99:1: error[unresolved-attribute] Unresolved attribute `TEST_PREFIX` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`
- asynq/mock_.py:100:1: error[unresolved-attribute] Unresolved attribute `stopall` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`.
+ asynq/mock_.py:100:1: error[unresolved-attribute] Unresolved attribute `stopall` on type `def patch(target, new=..., spec=None, create=False, mocksignature=False, spec_set=None, autospec=False, new_callable=None, **kwargs) -> Unknown`
- asynq/tests/test_debug.py:48:5: error[unresolved-attribute] Unresolved attribute `_task` on type `RuntimeError`.
+ asynq/tests/test_debug.py:48:5: error[unresolved-attribute] Unresolved attribute `_task` on type `RuntimeError`
- asynq/tests/test_debug.py:55:9: error[unresolved-attribute] Unresolved attribute `_traceback` on type `RuntimeError`.
+ asynq/tests/test_debug.py:55:9: error[unresolved-attribute] Unresolved attribute `_traceback` on type `RuntimeError`
- asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, ()].asynq(...) -> AsyncTask[Any]`.
+ asynq/tests/test_mock.py:271:13: error[unresolved-attribute] Unresolved attribute `cant_set_attribute` on type `bound method AsyncDecorator[Any, ()].asynq(...) -> AsyncTask[Any]`
- asynq/tests/test_profiler.py:63:5: error[unresolved-attribute] Unresolved attribute `KEEP_DEPENDENCIES` on type `DebugOptions`.
+ asynq/tests/test_profiler.py:63:5: error[unresolved-attribute] Unresolved attribute `KEEP_DEPENDENCIES` on type `DebugOptions`
- asynq/tests/test_recursive_task.py:28:9: error[unresolved-attribute] Unresolved attribute `MAX_TASK_STACK_SIZE` on type `DebugOptions`.
+ asynq/tests/test_recursive_task.py:28:9: error[unresolved-attribute] Unresolved attribute `MAX_TASK_STACK_SIZE` on type `DebugOptions`
- asynq/tests/test_recursive_task.py:32:9: error[unresolved-attribute] Unresolved attribute `MAX_TASK_STACK_SIZE` on type `DebugOptions`.
+ asynq/tests/test_recursive_task.py:32:9: error[unresolved-attribute] Unresolved attribute `MAX_TASK_STACK_SIZE` on type `DebugOptions`
- asynq/tools.py:206:9: error[unresolved-attribute] Unresolved attribute `__acached_per_instance_cache__` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:206:9: error[unresolved-attribute] Unresolved attribute `__acached_per_instance_cache__` on type `AsyncDecorator[Any, (...)]`
- asynq/tools.py:271:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:271:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, (...)]`
- asynq/tools.py:272:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:272:17: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`
- asynq/tools.py:276:13: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:276:13: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`
- asynq/tools.py:278:9: error[unresolved-attribute] Unresolved attribute `dirty` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:278:9: error[unresolved-attribute] Unresolved attribute `dirty` on type `AsyncDecorator[Any, (...)]`
- asynq/tools.py:279:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:279:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_refresh_time` on type `AsyncDecorator[Any, (...)]`
- asynq/tools.py:280:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:280:9: error[unresolved-attribute] Unresolved attribute `alazy_constant_cached_value` on type `AsyncDecorator[Any, (...)]`
- asynq/tools.py:310:9: error[unresolved-attribute] Unresolved attribute `original_fn` on type `AsyncDecorator[Any, (...)]`.
+ asynq/tools.py:310:9: error[unresolved-attribute] Unresolved attribute `original_fn` on type `AsyncDecorator[Any, (...)]`

black (https://github.com/psf/black)
- src/black/trans.py:493:13: error[unresolved-attribute] Unresolved attribute `__cause__` on type `object`.
+ src/black/trans.py:493:13: error[unresolved-attribute] Unresolved attribute `__cause__` on type `object`
- src/blib2to3/pytree.py:149:13: error[unresolved-attribute] Unresolved attribute `parent` on type `object`.
+ src/blib2to3/pytree.py:149:13: error[unresolved-attribute] Unresolved attribute `parent` on type `object`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_addons.py:80:13: error[unresolved-attribute] Unresolved attribute `number` on type `type`.
+ tests/test_addons.py:80:13: error[unresolved-attribute] Unresolved attribute `number` on type `type`
- tests/test_dupefilters.py:33:9: error[unresolved-attribute] Unresolved attribute `method` on type `Self@from_crawler`.
+ tests/test_dupefilters.py:33:9: error[unresolved-attribute] Unresolved attribute `method` on type `Self@from_crawler`
- tests/test_pipeline_media.py:455:17: error[unresolved-attribute] Unresolved attribute `store_uri` on type `Self@from_crawler`.
+ tests/test_pipeline_media.py:455:17: error[unresolved-attribute] Unresolved attribute `store_uri` on type `Self@from_crawler`

rich (https://github.com/Textualize/rich)
- tests/test_inspect.py:395:5: error[unresolved-attribute] Unresolved attribute `SomeClass` on type `ModuleType`.
+ tests/test_inspect.py:395:5: error[unresolved-attribute] Unresolved attribute `SomeClass` on type `ModuleType`
- tests/test_inspect.py:396:5: error[unresolved-attribute] Unresolved attribute `function` on type `ModuleType`.
+ tests/test_inspect.py:396:5: error[unresolved-attribute] Unresolved attribute `function` on type `ModuleType`
- tests/test_repr.py:57:5: error[unresolved-attribute] Unresolved attribute `angular` on type `def __rich_repr__(self) -> Unknown`.
+ tests/test_repr.py:57:5: error[unresolved-attribute] Unresolved attribute `angular` on type `def __rich_repr__(self) -> Unknown`

ignite (https://github.com/pytorch/ignite)
- examples/mnist/mnist_with_tqdm_logger.py:94:9: error[unresolved-attribute] Unresolved attribute `n` on type `ProgressBar`.
+ examples/mnist/mnist_with_tqdm_logger.py:94:9: error[unresolved-attribute] Unresolved attribute `n` on type `ProgressBar`
- examples/mnist/mnist_with_tqdm_logger.py:94:18: error[unresolved-attribute] Unresolved attribute `last_print_n` on type `ProgressBar`.
+ examples/mnist/mnist_with_tqdm_logger.py:94:18: error[unresolved-attribute] Unresolved attribute `last_print_n` on type `ProgressBar`
- examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:cell 14:25:5: error[unresolved-attribute] Unresolved attribute `n` on type `ProgressBar`.
+ examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:cell 14:25:5: error[unresolved-attribute] Unresolved attribute `n` on type `ProgressBar`
- examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:cell 14:25:14: error[unresolved-attribute] Unresolved attribute `last_print_n` on type `ProgressBar`.
+ examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:cell 14:25:14: error[unresolved-attribute] Unresolved attribute `last_print_n` on type `ProgressBar`
- examples/notebooks/TextCNN.ipynb:cell 61:19:5: error[unresolved-attribute] Unresolved attribute `n` on type `ProgressBar`.
+ examples/notebooks/TextCNN.ipynb:cell 61:19:5: error[unresolved-attribute] Unresolved attribute `n` on type `ProgressBar`
- examples/notebooks/TextCNN.ipynb:cell 61:19:14: error[unresolved-attribute] Unresolved attribute `last_print_n` on type `ProgressBar`.
+ examples/notebooks/TextCNN.ipynb:cell 61:19:14: error[unresolved-attribute] Unresolved attribute `last_print_n` on type `ProgressBar`
- tests/ignite/handlers/test_base_logger.py:149:5: error[unresolved-attribute] Unresolved attribute `alpha` on type `State`.
+ tests/ignite/handlers/test_base_logger.py:149:5: error[unresolved-attribute] Unresolved attribute `alpha` on type `State`
- tests/ignite/handlers/test_base_logger.py:150:5: error[unresolved-attribute] Unresolved attribute `beta` on type `State`.
+ tests/ignite/handlers/test_base_logger.py:150:5: error[unresolved-attribute] Unresolved attribute `beta` on type `State`
- tests/ignite/handlers/test_base_logger.py:151:5: error[unresolved-attribute] Unresolved attribute `gamma` on type `State`.
+ tests/ignite/handlers/test_base_logger.py:151:5: error[unresolved-attribute] Unresolved attribute `gamma` on type `State`
- tests/ignite/handlers/test_checkpoint.py:285:5: error[unresolved-attribute] Unresolved attribute `score` on type `State`.
+ tests/ignite/handlers/test_checkpoint.py:285:5: error[unresolved-attribute] Unresolved attribute `score` on type `State`
- tests/ignite/handlers/test_checkpoint.py:362:5: error[unresolved-attribute] Unresolved attribute `score` on type `State`.
+ tests/ignite/handlers/test_checkpoint.py:362:5: error[unresolved-attribute] Unresolved attribute `score` on type `State`
- tests/ignite/metrics/test_metric.py:1214:9: error[unresolved-attribute] Unresolved attribute `_num_correct` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1214:9: error[unresolved-attribute] Unresolved attribute `_num_correct` on type `Metric`
- tests/ignite/metrics/test_metric.py:1215:9: error[unresolved-attribute] Unresolved attribute `_num_examples` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1215:9: error[unresolved-attribute] Unresolved attribute `_num_examples` on type `Metric`
- tests/ignite/metrics/test_metric.py:1216:9: error[unresolved-attribute] Unresolved attribute `_numerator` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1216:9: error[unresolved-attribute] Unresolved attribute `_numerator` on type `Metric`
- tests/ignite/metrics/test_metric.py:1217:9: error[unresolved-attribute] Unresolved attribute `_denominator` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1217:9: error[unresolved-attribute] Unresolved attribute `_denominator` on type `Metric`
- tests/ignite/metrics/test_metric.py:1218:9: error[unresolved-attribute] Unresolved attribute `_weight` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1218:9: error[unresolved-attribute] Unresolved attribute `_weight` on type `Metric`
- tests/ignite/metrics/test_metric.py:1219:9: error[unresolved-attribute] Unresolved attribute `_updated` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1219:9: error[unresolved-attribute] Unresolved attribute `_updated` on type `Metric`
- tests/ignite/metrics/test_metric.py:1226:9: error[unresolved-attribute] Unresolved attribute `_numerator` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1226:9: error[unresolved-attribute] Unresolved attribute `_numerator` on type `Metric`
- tests/ignite/metrics/test_metric.py:1227:9: error[unresolved-attribute] Unresolved attribute `_denominator` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1227:9: error[unresolved-attribute] Unresolved attribute `_denominator` on type `Metric`
- tests/ignite/metrics/test_metric.py:1228:9: error[unresolved-attribute] Unresolved attribute `_weight` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1228:9: error[unresolved-attribute] Unresolved attribute `_weight` on type `Metric`
- tests/ignite/metrics/test_metric.py:1229:9: error[unresolved-attribute] Unresolved attribute `_updated` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1229:9: error[unresolved-attribute] Unresolved attribute `_updated` on type `Metric`
- tests/ignite/metrics/test_metric.py:1231:9: error[unresolved-attribute] Unresolved attribute `_numerator` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1231:9: error[unresolved-attribute] Unresolved attribute `_numerator` on type `Metric`
- tests/ignite/metrics/test_metric.py:1232:9: error[unresolved-attribute] Unresolved attribute `_denominator` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1232:9: error[unresolved-attribute] Unresolved attribute `_denominator` on type `Metric`
- tests/ignite/metrics/test_metric.py:1233:9: error[unresolved-attribute] Unresolved attribute `_weight` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1233:9: error[unresolved-attribute] Unresolved attribute `_weight` on type `Metric`
- tests/ignite/metrics/test_metric.py:1234:9: error[unresolved-attribute] Unresolved attribute `_updated` on type `Metric`.
+ tests/ignite/metrics/test_metric.py:1234:9: error[unresolved-attribute] Unresolved attribute `_updated` on type `Metric`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/knn.py:171:9: error[unresolved-attribute] Unresolved attribute `index_type` on type `Config`.
+ sockeye/knn.py:171:9: error[unresolved-attribute] Unresolved attribute `index_type` on type `Config`
- sockeye/knn.py:173:9: error[unresolved-attribute] Unresolved attribute `train_data_size` on type `Config`.
+ sockeye/knn.py:173:9: error[unresolved-attribute] Unresolved attribute `train_data_size` on type `Config`
- test/unit/test_inference.py:70:9: error[unresolved-attribute] Unresolved attribute `zeros_array` on type `Translator`.
+ test/unit/test_inference.py:70:9: error[unresolved-attribute] Unresolved attribute `zeros_array` on type `Translator`
- test/unit/test_inference.py:71:9: error[unresolved-attribute] Unresolved attribute `inf_array` on type `Translator`.
+ test/unit/test_inference.py:71:9: error[unresolved-attribute] Unresolved attribute `inf_array` on type `Translator`
- test/unit/test_inference.py:72:9: error[unresolved-attribute] Unresolved attribute `inf_array` on type `Translator`.
+ test/unit/test_inference.py:72:9: error[unresolved-attribute] Unresolved attribute `inf_array` on type `Translator`

nox (https://github.com/wntrblm/nox)
- nox/_decorators.py:61:5: error[unresolved-attribute] Unresolved attribute `__kwdefaults__` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ nox/_decorators.py:61:5: error[unresolved-attribute] Unresolved attribute `__kwdefaults__` on type `_Wrapped[(...), Unknown, (...), Unknown]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/gen_test.py:583:13: error[unresolved-attribute] Unresolved attribute `local_ref` on type `Self@test_coroutine_refcounting`.
+ tornado/test/gen_test.py:583:13: error[unresolved-attribute] Unresolved attribute `local_ref` on type `Self@test_coroutine_refcounting`
- tornado/test/http1connection_test.py:25:13: error[unresolved-attribute] Unresolved attribute `server_stream` on type `Self@asyncSetUp`.
+ tornado/test/http1connection_test.py:25:13: error[unresolved-attribute] Unresolved attribute `server_stream` on type `Self@asyncSetUp`
- tornado/test/httpclient_test.py:813:19: error[unresolved-attribute] Unresolved attribute `port` on type `Self@setUp`.
+ tornado/test/httpclient_test.py:813:19: error[unresolved-attribute] Unresolved attribute `port` on type `Self@setUp`
- tornado/test/httpclient_test.py:815:13: error[unresolved-attribute] Unresolved attribute `server` on type `Self@setUp`.
+ tornado/test/httpclient_test.py:815:13: error[unresolved-attribute] Unresolved attribute `server` on type `Self@setUp`
- tornado/test/httpserver_test.py:1513:13: error[unresolved-attribute] Unresolved attribute `http1` on type `Self@get_app`.
+ tornado/test/httpserver_test.py:1513:13: error[unresolved-attribute] Unresolved attribute `http1` on type `Self@get_app`
- tornado/test/ioloop_test.py:64:13: error[unresolved-attribute] Unresolved attribute `called` on type `Self@test_add_callback_wakeup`.
+ tornado/test/ioloop_test.py:64:13: error[unresolved-attribute] Unresolved attribute `called` on type `Self@test_add_callback_wakeup`
- tornado/test/ioloop_test.py:68:13: error[unresolved-attribute] Unresolved attribute `called` on type `Self@test_add_callback_wakeup`.
+ tornado/test/ioloop_test.py:68:13: error[unresolved-attribute] Unresolved attribute `called` on type `Self@test_add_callback_wakeup`
- tornado/test/ioloop_test.py:71:13: error[unresolved-attribute] Unresolved attribute `start_time` on type `Self@test_add_callback_wakeup`.
+ tornado/test/ioloop_test.py:71:13: error[unresolved-attribute] Unresolved attribute `start_time` on type `Self@test_add_callback_wakeup`
- tornado/test/ioloop_test.py:82:13: error[unresolved-attribute] Unresolved attribute `stop_time` on type `Self@test_add_callback_wakeup_other_thread`.
+ tornado/test/ioloop_test.py:82:13: error[unresolved-attribute] Unresolved attribute `stop_time` on type `Self@test_add_callback_wakeup_other_thread`
- tornado/test/ioloop_test.py:466:17: error[unresolved-attribute] Unresolved attribute `current_io_loop` on type `Self@test_non_current`.
+ tornado/test/ioloop_test.py:466:17: error[unresolved-attribute] Unresolved attribute `current_io_loop` on type `Self@test_non_current`

PyGithub (https://github.com/PyGithub/PyGithub)
- tests/Authentication.py:349:13: error[unresolved-attribute] Unresolved attribute `modifiedHeaders` on type `Self@testAddingCustomHeaders`.
+ tests/Authentication.py:349:13: error[unresolved-attribute] Unresolved attribute `modifiedHeaders` on type `Self@testAddingCustomHeaders`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/retrying.py:25:13: error[unresolved-attribute] Unresolved attribute `failed_count` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ cki_lib/retrying.py:25:13: error[unresolved-attribute] Unresolved attribute `failed_count` on type `_Wrapped[(...), Unknown, (...), Unknown]`
- cki_lib/retrying.py:26:13: error[unresolved-attribute] Unresolved attribute `retries` on type `_Wrapped[(...), Unknown, (...), Unknown]`.
+ cki_lib/retrying.py:26:13: error[unresolved-attribute] Unresolved attribute `retries` on type `_Wrapped[(...), Unknown, (...), Unknown]`
- tests/test_krb_ticket_refresher.py:23:9: error[unresolved-attribute] Unresolved attribute `LOGGER` on type `RefreshKerberosTicket`.
+ tests/test_krb_ticket_refresher.py:23:9: error[unresolved-attribute] Unresolved attribute `LOGGER` on type `RefreshKerberosTicket`
- tests/test_misc.py:190:13: error[unresolved-attribute] Unresolved attribute `communicate` on type `def fake_popen(...) -> Unknown`.
+ tests/test_misc.py:190:13: error[unresolved-attribute] Unresolved attribute `communicate` on type `def fake_popen(...) -> Unknown`
- tests/test_misc.py:192:13: error[unresolved-attribute] Unresolved attribute `returncode` on type `def fake_popen(...) -> Unknown`.
+ tests/test_misc.py:192:13: error[unresolved-attribute] Unresolved attribute `returncode` on type `def fake_popen(...) -> Unknown`
- tests/test_misc.py:193:13: error[unresolved-attribute] Unresolved attribute `called` on type `def fake_popen(...) -> Unknown`.
+ tests/test_misc.py:193:13: error[unresolved-attribute] Unresolved attribute `called` on type `def fake_popen(...) -> Unknown`
- tests/test_psql.py:99:9: error[unresolved-attribute] Unresolved attribute `side_effect` on type `bound method PSQLHandler._execute(query, *args) -> Unknown`.
+ tests/test_psql.py:99:9: error[unresolved-attribute] Unresolved attribute `side_effect` on type `bound method PSQLHandler._execute(query, *args) -> Unknown`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/webscanner_helper/test_proxyauth_selenium.py:57:9: error[unresolved-attribute] Unresolved attribute `is_replay` on type `Request`.
+ examples/contrib/webscanner_helper/test_proxyauth_selenium.py:57:9: error[unresolved-attribute] Unresolved attribute `is_replay` on type `Request`
- mitmproxy/tools/web/app.py:591:29: error[unresolved-attribute] Unresolved attribute `port` on type `object`.
+ mitmproxy/tools/web/app.py:591:29: error[unresolved-attribute] Unresolved attribute `port` on type `object`
- mitmproxy/tools/web/app.py:600:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`.
+ mitmproxy/tools/web/app.py:600:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`
- mitmproxy/tools/web/app.py:604:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`.
+ mitmproxy/tools/web/app.py:604:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`
- mitmproxy/tools/web/app.py:614:29: error[unresolved-attribute] Unresolved attribute `status_code` on type `object`.
+ mitmproxy/tools/web/app.py:614:29: error[unresolved-attribute] Unresolved attribute `status_code` on type `object`
- mitmproxy/tools/web/app.py:623:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`.
+ mitmproxy/tools/web/app.py:623:33: error[unresolved-attribute] Unresolved attribute `trailers` on type `object`
- mitmproxy/tools/web/app.py:627:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`.
+ mitmproxy/tools/web/app.py:627:29: error[unresolved-attribute] Unresolved attribute `text` on type `object`
- test/mitmproxy/net/http/test_cookies.py:233:5: error[unresolved-attribute] Unresolved attribute `return_value` on type `def time() -> int | float`.
+ test/mitmproxy/net/http/test_cookies.py:233:5: error[unresolved-attribute] Unresolved attribute `return_value` on type `def time() -> int | float`
- test/mitmproxy/proxy/test_tutils.py:143:5: error[unresolved-attribute] Unresolved attribute `foo` on type `TCommand`.
+ test/mitmproxy/proxy/test_tutils.py:143:5: error[unresolved-attribute] Unresolved attribute `foo` on type `TCommand`
- test/mitmproxy/proxy/test_tutils.py:144:5: error[unresolved-attribute] Unresolved attribute `bar` on type `TCommand`.
+ test/mitmproxy/proxy/test_tutils.py:144:5: error[unresolved-attribute] Unresolved attribute `bar` on type `TCommand`
- test/mitmproxy/proxy/test_tutils.py:146:5: error[unresolved-attribute] Unresolved attribute `foo` on type `TCommand`.
+ test/mitmproxy/proxy/test_tutils.py:146:5: error[unresolved-attribute] Unresolved attribute `foo` on type `TCommand`
- test/mitmproxy/proxy/test_tutils.py:147:5: error[unresolved-attribute] Unresolved attribute `bar` on type `TCommand`.
+ test/mitmproxy/proxy/test_tutils.py:147:5: error[unresolved-attribute] Unresolved attribute `bar` on type `TCommand`
- web/gen/state_js.py:30:5: error[unresolved-attribute] Unresolved attribute `_servers` on type `ServerInstance[Unknown]`.
+ web/gen/state_js.py:30:5: error[unresolved-attribute] Unresolved attribute `_servers` on type `ServerInstance[Unknown]`
- web/gen/state_js.py:35:5: error[unresolved-attribute] Unresolved attribute `_server` on type `ServerInstance[Unknown]`.
+ web/gen/state_js.py:35:5: error[unresolved-attribute] Unresolved attribute `_server` on type `ServerInstance[Unknown]`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/generation/hypothesis/examples.py:51:5: error[unresolved-attribute] Unresolved attribute `_hypothesis_internal_database_key` on type `(...) -> None`.
+ src/schemathesis/generation/hypothesis/examples.py:51:5: error[unresolved-attribute] Unresolved attribute `_hypothesis_internal_database_key` on type `(...) -> None`

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/transaction_processor/level_5_actions_utest.py:256:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`.
+ dragonchain/transaction_processor/level_5_actions_utest.py:256:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`
- dragonchain/transaction_processor/level_5_actions_utest.py:275:9: error[unresolved-attribute] Unresolved attribute `side_effect` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`.
+ dragonchain/transaction_processor/level_5_actions_utest.py:275:9: error[unresolved-attribute] Unresolved attribute `side_effect` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`
- dragonchain/transaction_processor/level_5_actions_utest.py:294:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`.
+ dragonchain/transaction_processor/level_5_actions_utest.py:294:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`
- dragonchain/transaction_processor/level_5_actions_utest.py:312:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`.
+ dragonchain/transaction_processor/level_5_actions_utest.py:312:9: error[unresolved-attribute] Unresolved attribute `return_value` on type `bound method InterchainModel.is_transaction_confirmed(transaction_hash: str) -> bool`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/config.py:1282:9: error[unresolved-attribute] Unresolved attribute `__pydantic_config__` on type `_TypeT@with_config`.
+ pydantic/config.py:1282:9: error[unresolved-attribute] Unresolved attribute `__pydantic_config__` on type `_TypeT@with_config`
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
- pydantic/main.py:267:5: error[unresolved-attribute] Unresolved attribute `__pydantic_base_init__` on type `def __init__(self, /, **data: Any) -> None`.
+ pydantic/main.py:267:5: error[unresolved-attribute] Unresolved attribute `__pydantic_base_init__` on type `def __init__(self, /, **data: Any) -> None`
- pydantic/root_model.py:70:5: error[unresolved-attribute] Unresolved attribute `__pydantic_base_init__` on type `def __init__(self, /, root: RootModelRootType@RootModel = ..., **data) -> None`.
+ pydantic/root_model.py:70:5: error[unresolved-attribute] Unresolved attribute `__pydantic_base_init__` on type `def __init__(self, /, root: RootModelRootType@RootModel = ..., **data) -> None`

comtypes (https://github.com/enthought/comtypes)
- comtypes/_memberspec.py:483:9: error[unresolved-attribute] Unresolved attribute `__name__` on type `(...) -> Any`.
+ comtypes/_memberspec.py:483:9: error[unresolved-attribute] Unresolved attribute `__name__` on type `(...) -> Any`
- comtypes/_memberspec.py:521:13: error[unresolved-attribute] Unresolved attribute `__name__` on type `(...) -> Any`.
+ comtypes/_memberspec.py:521:13: error[unresolved-attribute] Unresolved attribute `__name__` on type `(...) -> Any`
- comtypes/_vtbl.py:113:5: error[unresolved-attribute] Unresolved attribute `has_outargs` on type `def call_with_this(...) -> Unknown`.
+ comtypes/_vtbl.py:113:5: error[unresolved-attribute] Unresolved attribute `has_outargs` on type `def call_with_this(...) -> Unknown`
- comtypes/_vtbl.py:217:9: error[unresolved-attribute] Unresolved attribute `has_outargs` on type `def call_without_this(this, *args) -> Unknown`.
+ comtypes/_vtbl.py:217:9: error[unresolved-attribute] Unresolved attribute `has_outargs` on type `def call_without_this(this, *args) -> Unknown`

mypy (https://github.com/python/mypy)
- mypyc/irbuild/for_helpers.py:793:13: error[unresolved-attribute] Unresolved attribute `next_reg` on type `Self@gen_condition`.
+ mypyc/irbuild/for_helpers.py:793:13: error[unresolved-attribute] Unresolved attribute `next_reg` on type `Self@gen_condition`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/directives/other.py:331:9: error[unresolved-attribute] Unresolved attribute `title_styles` on type `Struct`.
+ sphinx/directives/other.py:331:9: error[unresolved-attribute] Unresolved attribute `title_styles` on type `Struct`
- sphinx/directives/other.py:332:9: error[unresolved-attribute] Unresolved attribute `section_level` on type `Struct`.
+ sphinx/directives/other.py:332:9: error[unresolved-attribute] Unresolved attribute `section_level` on type `Struct`
- sphinx/util/parsing.py:82:5: error[unresolved-attribute] Unresolved attribute `title_styles` on type `Struct`.
+ sphinx/util/parsing.py:82:5: error[unresolved-attribute] Unresolved attribute `title_styles` on type `Struct`
- sphinx/util/parsing.py:83:5: error[unresolved-attribute] Unresolved attribute `section_level` on type `Struct`.
+ sphinx/util/parsing.py:83:5: error[unresolved-attribute] Unresolved attribute `section_level` on type `Struct`
- sphinx/util/parsing.py:88:9: error[unresolved-attribute] Unresolved attribute `title_styles` on type `Struct`.
+ sphinx/util/parsing.py:88:9: error[unresolved-attribute] Unresolved attribute `title_styles` on type `Struct`
- sphinx/util/parsing.py:89:9: error[unresolved-attribute] Unresolved attribute `section_level` on type `Struct`.
+ sphinx/util/parsing.py:89:9: error[unresolved-attribute] Unresolved attribute `section_level` on type `Struct`

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/tree.py:1216:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT@CommandTree]`.
+ discord/app_commands/tree.py:1216:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT@CommandTree]`
- discord/app_commands/tree.py:1273:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT@CommandTree]`.
+ discord/app_commands/tree.py:1273:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT@CommandTree]`
- discord/app_commands/tree.py:1280:9: error[unresolved-attribute] Unresolved attribute `_cs_namespace` on type `Interaction[ClientT@CommandTree]`.
+ discord/app_commands/tree.py:1280:9: error[unresolved-attribute] Unresolved attribute `_cs_namespace` on type `Interaction[ClientT@CommandTree]`
- discord/ext/commands/core.py:1949:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Any, (...), Any] | ((...) -> Coroutine[Any, Any, Any])) -> Command[Any, (...), Any] | ((...) -> Coroutine[Any, Any, Any])`.
+ discord/ext/commands/core.py:1949:9: error[unresolved-attribute] Unresolved attribute `predicate` on type `def decorator(func: Command[Any, (...), Any] | ((...) -> Coroutine[Any, Any, Any])) -> Command[Any, (...), Any] | ((...) -> Coroutine[Any, Any, Any])`
- discord/ext/comman

... (truncated 2212 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-14 18:30_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:36_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 0 | 1,221 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-return-type` | 1 | 1 | 2 |
| `invalid-argument-type` | 0 | 0 | 1 |
| **Total** | **1** | **1** | **1,231** |


**[Full report with detailed diff](https://0b43d6a3.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://0b43d6a3.ty-ecosystem-ext.pages.dev/timing))



---

_@MichaReiser approved on 2026-01-14 19:09_

---

_Merged by @AlexWaygood on 2026-01-14 19:56_

---

_Closed by @AlexWaygood on 2026-01-14 19:56_

---

_Branch deleted on 2026-01-14 19:56_

---
