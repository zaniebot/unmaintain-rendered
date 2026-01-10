```yaml
number: 22227
title: "[ty] fix display of top ParamSpec specialization"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/top-paramspec-display
created_at: 2025-12-27T19:04:41Z
updated_at: 2025-12-27T19:14:24Z
url: https://github.com/astral-sh/ruff/pull/22227
synced_at: 2026-01-10T16:36:18Z
```

# [ty] fix display of top ParamSpec specialization

---

_Pull request opened by @carljm on 2025-12-27 19:04_

## Summary

I only noticed this in the ecosystem report of https://github.com/astral-sh/ruff/pull/22213 after merging it. The change to displaying `Top[]` wrapper around the entire signature instead of just the parameters had the side effect of not showing it at all when displaying a top ParamSpec specialization. This PR fixes that.

Marking internal since this is a fixup of a not-released PR.

## Test Plan

Added mdtest that fails without this PR.


---

_Label `internal` added by @carljm on 2025-12-27 19:04_

---

_Label `ty` added by @carljm on 2025-12-27 19:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-27 19:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-27 19:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-27 19:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]]` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__qualname__`
- src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]]` has no attribute `__name__`
+ src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__name__`
- src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]] & ~MethodType` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__qualname__`
- src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]] & ~MethodType` has no attribute `__name__`
+ src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__name__`

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/commands.py:2477:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__discord_app_commands_checks__` on type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>)`
+ discord/app_commands/commands.py:2477:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__discord_app_commands_checks__` on type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>)`
- discord/app_commands/commands.py:2479:13: error[unresolved-attribute] Object of type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu)` has no attribute `__discord_app_commands_checks__`
+ discord/app_commands/commands.py:2479:13: error[unresolved-attribute] Object of type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu)` has no attribute `__discord_app_commands_checks__`
- discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
+ discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
+ discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
+ discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
+ discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
+ discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
+ discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
+ discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5092 diagnostics
+ Found 5093 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-27 19:12_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 0 | 16 |
| `invalid-return-type` | 0 | 1 | 10 |
| `unresolved-attribute` | 0 | 0 | 8 |
| `invalid-argument-type` | 0 | 0 | 3 |
| **Total** | **0** | **1** | **37** |


**[Full report with detailed diff](https://4a2b9591.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://4a2b9591.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @carljm on 2025-12-27 19:14_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-27 19:14_

---

_Review requested from @sharkdp by @carljm on 2025-12-27 19:14_

---

_Review requested from @dcreager by @carljm on 2025-12-27 19:14_

---

_Merged by @carljm on 2025-12-27 19:14_

---

_Closed by @carljm on 2025-12-27 19:14_

---

_Branch deleted on 2025-12-27 19:14_

---
