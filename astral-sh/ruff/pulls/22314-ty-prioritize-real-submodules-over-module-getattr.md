```yaml
number: 22314
title: "[ty] Prioritize real submodules over module `__getattr__`"
type: pull_request
state: open
author: Adamkadaban
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: resolve-submodules-fix
created_at: 2025-12-31T06:59:57Z
updated_at: 2026-01-05T12:53:57Z
url: https://github.com/astral-sh/ruff/pull/22314
synced_at: 2026-01-10T16:36:19Z
```

# [ty] Prioritize real submodules over module `__getattr__`

---

_Pull request opened by @Adamkadaban on 2025-12-31 06:59_

Related to https://github.com/astral-sh/ty/issues/1053 (PySide6 submodules)
Related to https://github.com/astral-sh/ty/issues/1361 (aiohttp.web)
Related to https://github.com/astral-sh/ty/issues/133 (implicit submodule imports)


## Summary

This PR extends the work from [PR #21260](https://github.com/astral-sh/ruff/pull/21260) by adding:
1. **Improved submodule resolution**: Uses `all_submodules` fallback to discover real submodules on disk before falling back to `__getattr__`, ensuring packages like `anyio`, `aiohttp`, and `PySide6` resolve correctly
2. **Python version gate** for module `__getattr__` (PEP 562 requires Python 3.7+)
3. **Additional test coverage** for the submodule precedence behavior

Fixes an issue where the type checker would incorrectly use a module's `__getattr__` return type instead of recognizing real submodules that exist on disk.

**Example**

Before this change, accessing `anyio.to_thread` would return the type from `anyio.__getattr__()` even though `anyio/to_thread.py` exists as a real submodule:

```python
import anyio

# Before: returned type from __getattr__ (incorrect)
# After: correctly resolves to <module 'anyio.to_thread'>
reveal_type(anyio.to_thread.current_default_thread_limiter())  # revealed: int
```


There are two key decisions to consider before merging this PR:

### Python 3.7+ Version Gate

Module-level `__getattr__` was introduced in PEP 562 (Python 3.7). ty now correctly ignores `__getattr__` when targeting Python < 3.7, while still allowing stub files to use it on older versions. This [aligns with the pyright implementation](https://github.com/microsoft/pyright/blob/1576956c32c8b75f3202203f3cdedd21c8d3f9f7/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L5997C15-L6019C22) and [not with the mypy implementation](https://github.com/python/mypy/blob/c5c12fad9e69525fa5b12058b328d284c5feecc4/mypy/checker.py#L2066C1-L2068C43). 

### Submodule vs `__getattr__` precedence

MyPy, Pyright, and this PR all prioritize real submodules over `__getattr__` return types, even though **at runtime** the precedence is reversed (module attributes win over submodules). This is a deliberate departure from runtime semantics based on the assumption that well-behaved `__getattr__` implementations will raise `AttributeError` for names that are also real submodules. This decision attempts to follow [PR #21260](https://github.com/astral-sh/ruff/pull/21260), which showed ecosystem improvements: ~364 false positives removed vs ~114 new diagnostics added. Major packages like `anyio`, `sklearn`, and `scipy` benefit from this behavior.

## Test Plan

All changes are covered by mdtests in `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:

1. **Updated existing test**: Modified "Precedence: submodules vs `__getattr__`" to expect submodules to win (updated to match [PR #21260](https://github.com/astral-sh/ruff/pull/21260))
2. **Real-world scenario**: Tests anyio-like package with both `__getattr__` and real submodules
3. **Fallback behavior**: Verifies `__getattr__` still works when no matching submodule exists
4. **Complex packages**: Tests PySide-like packages with nested attribute access
5. **Python version**: Confirms `__getattr__` is ignored on Python 3.6
6. **Stub files**: Verifies stub files allow `__getattr__` on older Python versions


---

_Review requested from @carljm by @Adamkadaban on 2025-12-31 06:59_

---

_Review requested from @AlexWaygood by @Adamkadaban on 2025-12-31 06:59_

---

_Review requested from @sharkdp by @Adamkadaban on 2025-12-31 06:59_

---

_Review requested from @dcreager by @Adamkadaban on 2025-12-31 06:59_

---

_Label `ty` added by @MichaReiser on 2025-12-31 07:30_

---

_Comment by @astral-sh-bot[bot] on 2025-12-31 07:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-31 07:33_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/door/_cls/doormeta.py:82:10: error[unresolved-attribute] Object of type `object` has no attribute `TypeHint`
- beartype/door/_cls/doormeta.py:163:19: error[unresolved-attribute] Object of type `object` has no attribute `TypeHint`
+ beartype/door/_cls/doormeta.py:163:45: error[invalid-assignment] Object of type `object` is not assignable to `TypeHint[Unknown]`
- beartype/door/_cls/doormeta.py:179:10: error[unresolved-attribute] Object of type `object` has no attribute `TypeHint`
- Found 493 diagnostics
+ Found 491 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/middleware/wsgi.py:134:13: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run`
- starlette/middleware/wsgi.py:149:13: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run`
- starlette/middleware/wsgi.py:154:9: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run`
- Found 216 diagnostics
+ Found 213 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- tests/test_runserver_main.py:66:32: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- tests/test_runserver_main.py:115:32: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- tests/test_runserver_main.py:171:28: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- tests/test_runserver_main.py:368:32: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- Found 30 diagnostics
+ Found 26 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/results.py:285:50: warning[possibly-missing-attribute] Submodule `task_run` may not be available as an attribute on module `prefect.runtime`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:42:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:48:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:54:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:61:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:73:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:79:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:85:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:92:28: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:95:28: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:100:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:114:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:120:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:126:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:134:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:153:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:159:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:165:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:172:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:186:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:192:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:198:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:205:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:210:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:225:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:231:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:237:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:244:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:254:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:260:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:266:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:272:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:275:30: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:297:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:303:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:309:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:330:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:335:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:340:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:345:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:352:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:359:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:365:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:371:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:378:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:383:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:388:30: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:390:30: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:394:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:397:31: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:456:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:462:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:468:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:487:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:495:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:503:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:507:28: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:530:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:536:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:542:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:563:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:568:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:573:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:578:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:587:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:593:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:601:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:618:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:623:28: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:625:31: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:661:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:667:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:673:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:692:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:700:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:708:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2021_01_20_122127_25f4b90a7a42_initial_migration.py:712:28: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_5f376def75c3_block_data.py:27:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_5f376def75c3_block_data.py:33:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_5f376def75c3_block_data.py:39:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_5f376def75c3_block_data.py:47:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_679e695af6ba_add_configurations.py:27:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_679e695af6ba_add_configurations.py:33:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_679e695af6ba_add_configurations.py:39:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_13_125213_679e695af6ba_add_configurations.py:46:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:26:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:32:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:38:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:45:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:64:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:70:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:76:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:83:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_17_140821_5bff7878e700_add_agents_and_work_queue.py:88:30: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_20_103844_4799f657a6a1_add_block_spec_table.py:27:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_20_103844_4799f657a6a1_add_block_spec_table.py:33:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_20_103844_4799f657a6a1_add_block_spec_table.py:39:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_20_103844_4799f657a6a1_add_block_spec_table.py:48:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_02_21_150017_b68b3cad6b8a_add_block_spec_id_to_blocks.py:27:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_10_145956_1c9390e2f9c6_replace_version_with_checksum_and_.py:28:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_10_145956_1c9390e2f9c6_replace_version_with_checksum_and_.py:34:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_10_145956_1c9390e2f9c6_replace_version_with_checksum_and_.py:40:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_10_145956_1c9390e2f9c6_replace_version_with_checksum_and_.py:56:30: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_10_145956_1c9390e2f9c6_replace_version_with_checksum_and_.py:78:30: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:27:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:33:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:39:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:45:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:50:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:65:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:71:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:77:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:85:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:91:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_12_202952_dc7a3c6fd3e9_add_flow_run_alerts.py:98:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_26_135743_724e6dcc6b5d_add_block_schema_capabilities.py:27:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:40:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:46:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:52:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:59:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:64:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:95:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:101:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:107:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:114:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_05_28_081821_2fe6fe6ca16e_adds_block_schema_references_and_block_.py:119:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_07_11_170700_112c68143fc3_add_infrastructure_document_id_to_.py:26:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_07_11_170700_112c68143fc3_add_infrastructure_document_id_to_.py:42:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_07_25_233637_add97ce1937d_update_deployments_to_include_more_.py:33:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_07_25_233637_add97ce1937d_update_deployments_to_include_more_.py:42:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_08_06_145817_60e428f92a75_expand_deployment_schema_for_improved_ux.py:26:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_10_12_102048_22b7cb02e593_add_state_timestamp.py:26:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_10_12_102048_22b7cb02e593_add_state_timestamp.py:38:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_10_19_093902_6d548701edef_add_created_by.py:27:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_10_19_093902_6d548701edef_add_created_by.py:37:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_10_19_093902_6d548701edef_add_created_by.py:47:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_10_20_101423_3ced59d8806b_add_last_polled.py:26:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:27:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:33:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:39:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:48:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:55:33: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:69:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:75:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:81:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:88:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:93:31: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:126:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:132:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:138:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:148:31: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:195:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2022_11_24_143620_f7587d6c5776_add_worker_tables.py:215:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:37:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:43:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:49:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:58:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:65:33: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:79:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:85:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:91:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:98:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:103:29: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:136:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:142:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:148:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:158:29: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:205:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:225:17: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:259:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:265:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:271:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:280:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:287:33: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:301:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:307:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:313:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:320:13: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/_migrations/versions/postgresql/2023_01_08_180142_d481d5058a19_rename_worker_pools_to_work_pools.py:325:31: warning[possibly-missing-attribute] Submodule `utilities` may not be available as an attribute on module `prefect.server`
+ sr

... (truncated 505 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-12-31 07:33_

---

_Comment by @astral-sh-bot[bot] on 2025-12-31 07:40_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-missing-attribute` | 620 | 0 | 0 |
| `unresolved-attribute` | 4 | 10 | 0 |
| `invalid-argument-type` | 0 | 2 | 2 |
| `invalid-return-type` | 0 | 0 | 2 |
| `invalid-assignment` | 1 | 0 | 0 |
| `parameter-already-assigned` | 1 | 0 | 0 |
| **Total** | **626** | **12** | **4** |


**[Full report with detailed diff](https://9d55be5d.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://9d55be5d.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @MichaReiser on 2025-12-31 07:49_

Thanks for working on this. @Adamkadaban can you take at the ecosystem report? There are many new diagnostics

---

_Comment by @Adamkadaban on 2025-12-31 10:35_

@MichaReiser 
The majority of the new diagnostics are from prefect:
```
Submodule `utilities` may not be available as an attribute on module `prefect.server`
```

I believe this is an accurate warning, since - given `import prefect`, `prefect.server` is only created via the root `__getattr__`, and `prefect.server.__init__` doesn't import or re-exports utilities, and there is no `__getattr__` on `prefect.server` to fill the gap. So `prefect.server.utilities` can legitimately raise at runtime unless a caller imports that subpackage explicitly first.

The changes in this PR tighten attribute resolution to stop auto-materializing subpackages unless they’re discoverable through normal import or explicit export. That means names missing from a package’s namespace (and not covered by a __getattr__) now surface as possibly-missing-attribute.

It seems this implementation is stricter than mypy and pyright. I can remove it if you think that would be more appropriate. Removing this warning seems it would be very simple for prefect, so I think it makes sense to be stricter. But I'm happy to do whatever you think is best. 


I did not check in full, but other changes appear to be from other things merged to main since I made the PR. 

---

_Review requested from @Gankra by @MichaReiser on 2026-01-04 17:31_

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-04 17:32_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-04 17:32_

---

_Comment by @Gankra on 2026-01-05 12:53_

Skimmed the ecosystem results and yeah all of the new positives seem roughly understandable, although the small number of fixes doesn't paint an incredible picture.

The most interesting cases are in scipy, but I think they all relate to corners around `__all__` that are maybe fine/pre-existing:

*  "Module `scipy.linalg.blas` has no member `dnrm2`" seems to be us failing to find things implicitly re-exported by [a star import](https://github.com/scipy/scipy/blob/3afcc475b1d5003bf95e283a7afeb04d094aa6e4/scipy/linalg/blas.py#L239)
* "Module `scipy.special` has no member `multigammaln`" is us failing to resolve the attribute access `scipy.special.multigammaln` which is also "re-exported" via a `*` import but in this case it's [also explicitly stated to exist via `__all__`](https://github.com/scipy/scipy/blob/3afcc475b1d5003bf95e283a7afeb04d094aa6e4/scipy/special/__init__.py#L829)
* "Multiple values provided for parameter `dtype` of bound method `__init__` " this escapes me and I assume is some unrelated issue.

---
