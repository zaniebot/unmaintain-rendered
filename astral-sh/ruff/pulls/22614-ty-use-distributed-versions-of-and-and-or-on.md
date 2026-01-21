```yaml
number: 22614
title: "[ty] Use distributed versions of AND and OR on constraint sets"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - performance
  - ty
assignees: []
base: main
head: dcreager/distributed-ops
created_at: 2026-01-16T10:01:46Z
updated_at: 2026-01-21T18:51:51Z
url: https://github.com/astral-sh/ruff/pull/22614
synced_at: 2026-01-21T19:05:04Z
```

# [ty] Use distributed versions of AND and OR on constraint sets

---

_@dcreager_

There are some pathological examples where we create a constraint set which is the AND or OR of several smaller constraint sets. For example, when calling a function with many overloads, where the argument is a typevar, we create an OR of the typevar specializing to a type compatible with the respective parameter of each overload.

Most functions have a small number of overloads. But there are some examples of methods with 15-20 overloads (pydantic, numpy, our own auto-generated `__getitem__` for large tuple literals). For those cases, it is helpful to be more clever about how we construct the final result.

Before, we would just step through the `Iterator` of elements and accumulate them into a result constraint set. That results in an `O(n)` number of calls to the underlying `and` or `or` operator — each of which might have to construct a large temporary BDD tree.

AND and OR are both associative, so we can do better! We now invoke the operator in a "tree" shape (described in more detail in the doc comment). We still have to perform the same number of calls, but more of the calls operate on smaller BDDs, resulting in a much smaller amount of overall work.

---

_Label `internal` added by @dcreager on 2026-01-16 10:01_

---

_Label `performance` added by @dcreager on 2026-01-16 10:01_

---

_Label `ty` added by @dcreager on 2026-01-16 10:01_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/checks.py:390:42: error[invalid-assignment] Object of type `Coroutine[Any, Any, Cooldown | None] | Cooldown | None` is not assignable to `Cooldown | None`
- Found 539 diagnostics
+ Found 538 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/_internal/concurrency/api.py:83:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@call_soon_in_new_thread]`, found `() -> T@call_soon_in_new_thread | Awaitable[T@call_soon_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:87:16: error[invalid-return-type] Return type does not match returned value: expected `Call[T@call_soon_in_new_thread]`, found `Call[Awaitable[T@call_soon_in_new_thread]]`
- src/prefect/_internal/concurrency/api.py:99:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@call_soon_in_loop_thread]`, found `() -> T@call_soon_in_loop_thread | Awaitable[T@call_soon_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:103:16: error[invalid-return-type] Return type does not match returned value: expected `Call[T@call_soon_in_loop_thread]`, found `Call[Awaitable[T@call_soon_in_loop_thread]]`
- src/prefect/_internal/concurrency/api.py:137:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_loop_thread]`, found `(() -> Awaitable[T@wait_for_call_in_loop_thread]) | Call[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:146:20: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_loop_thread`, found `Awaitable[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:154:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_new_thread]`, found `(() -> T@wait_for_call_in_new_thread) | Call[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:160:16: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_new_thread`, found `Awaitable[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:166:46: error[invalid-argument-type] Argument to function `call_soon_in_new_thread` is incorrect: Expected `() -> Awaitable[T@call_in_new_thread]`, found `(() -> T@call_in_new_thread) | Call[T@call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:174:47: error[invalid-argument-type] Argument to function `call_soon_in_loop_thread` is incorrect: Expected `() -> Awaitable[T@call_in_loop_thread]`, found `(() -> Awaitable[T@call_in_loop_thread]) | Call[T@call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:189:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_loop_thread]`, found `(() -> Awaitable[T@wait_for_call_in_loop_thread]) | Call[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:198:20: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_loop_thread`, found `Awaitable[T@wait_for_call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:206:29: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@wait_for_call_in_new_thread]`, found `(() -> T@wait_for_call_in_new_thread) | Call[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:212:16: error[invalid-return-type] Return type does not match returned value: expected `T@wait_for_call_in_new_thread`, found `Awaitable[T@wait_for_call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:219:46: error[invalid-argument-type] Argument to function `call_soon_in_new_thread` is incorrect: Expected `() -> Awaitable[T@call_in_new_thread]`, found `() -> T@call_in_new_thread | Awaitable[T@call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:220:16: error[invalid-return-type] Return type does not match returned value: expected `T@call_in_new_thread`, found `Awaitable[T@call_in_new_thread]`
- src/prefect/_internal/concurrency/api.py:230:33: error[invalid-argument-type] Argument to function `cast_to_call` is incorrect: Expected `() -> Awaitable[T@call_in_loop_thread]`, found `() -> T@call_in_loop_thread | Awaitable[T@call_in_loop_thread]`
- src/prefect/_internal/concurrency/api.py:233:47: error[invalid-argument-type] Argument to function `call_soon_in_loop_thread` is incorrect: Expected `() -> Awaitable[T@call_in_loop_thread]`, found `() -> T@call_in_loop_thread | Awaitable[T@call_in_loop_thread]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/concurrency/_leases.py:89:53: error[invalid-argument-type] Argument to bound method `add_done_callback` is incorrect: Expected `(Future[CoroutineType[Any, Any, None]], /) -> object`, found `def handle_lease_renewal_failure(future: Future[None]) -> Unknown`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/asyncutils.py:198:16: error[invalid-return-type] Return type does not match returned value: expected `R@run_coro_as_sync | None`, found `CoroutineType[Any, Any, R@run_coro_as_sync | None]`
- src/prefect/utilities/asyncutils.py:207:20: error[invalid-return-type] Return type does not match returned value: expected `R@run_coro_as_sync | None`, found `CoroutineType[Any, Any, R@run_coro_as_sync | None]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any]]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | int | T@resolve_variables | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
- Found 5407 diagnostics
+ Found 5391 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, int | _R@ignore_variance | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14470 diagnostics
+ Found 14469 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:723 on 2026-01-16 10:08_

While we're here, I'm updating the BDD variable ordering to be less clever. For the pathological example from #21902 this has a huge benefit.

---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:4028 on 2026-01-16 10:08_

and also update the tree display to make it more obvious where we're sharing tree structure

---

_Marked ready for review by @dcreager on 2026-01-16 10:09_

---

_Review requested from @carljm by @dcreager on 2026-01-16 10:09_

---

_Review requested from @AlexWaygood by @dcreager on 2026-01-16 10:09_

---

_Review requested from @sharkdp by @dcreager on 2026-01-16 10:09_

---

_Comment by @codspeed-hq[bot] on 2026-01-16 10:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 45.74%**

<sub>Comparing <code>dcreager/distributed-ops</code> (18f5b2a) with <code>main</code> (d4123fc)</sub>



### Summary

`❌ 4` regressed benchmarks  
`✅ 19` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 87.2 s | 160.6 s | -45.74% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 61.8 s | 68.7 s | -10.06% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.8 s | 22.8 s | -8.81% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.7 s | 8.1 s | -4.59% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2026-01-16 10:43_

From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

---

_@MichaReiser reviewed on 2026-01-16 10:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:1828 on 2026-01-16 10:45_

Could this be a `FxIndexSet` or why is it important that `seen` has `Eq` and `PartialEq`?

---

_@MichaReiser reviewed on 2026-01-16 10:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 10:47_

Could it be that the collect calls are expensive? Given that `distributed_or` and `distributed_and` are very small methods, would it make sense to pass the `f` through and apply the mapping in `distributed_or`/and?

Another alternative is to use a `SmallVec` instead. But I wonder if part of the perf regression simply comes from writing all the constraints to a vec


---

_Comment by @dcreager on 2026-01-16 12:37_

> From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

Me too! Maybe the collecting is the culprit, as you suggest? Or maybe because we're not short circuiting anymore? I have an idea that might help with both.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 13:25_

I've pushed up a new version that doesn't collect into a vec first. I want to see how that affects the perf numbers; if it works well I plan to add some better documentation comments describing how it works

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1828 on 2026-01-16 13:25_

It can! Done (My muscle memory is to immediately reach for `FxOrderSet` first)

---

_@dcreager reviewed on 2026-01-16 13:25_

---

_Comment by @AlexWaygood on 2026-01-16 13:40_

Wow, nice job getting this from a 5x perf regression to a 4% perf improvement!!

---

_@dcreager reviewed on 2026-01-16 14:51_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 14:51_

This seems to work well! Pushed up some documentation of the approach.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:150 on 2026-01-19 09:16_

This PR does improve the performance by a fair bit but it also regresses performance by about 1-2 on most other projects. 

It would be lovely if we could use specialization to specialize `when_any` and `when_all` for `ExactSizeIterator` so that we could use the old implementation if there are only very few items. But, that's unlikely an option any time soon unless we migrate to nightly Rust.

I went through some `when_any` usages and:

* We could implement `when_any` for `&[T]` and `&FxOrderSet`
* We could add a `when_any_exact` method (or rename `when_any` to `when_any_iter` to advertise the `ExactSizeIterator` version)


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:723 on 2026-01-19 09:17_

Should we do this in a separate PR so that we better understand where the performance improvements are coming from?

---

_@MichaReiser approved on 2026-01-19 09:22_

Nice. 

It might make sense to specialize `distribute_or` and `distribute_and` (or `when_any`) for `&[T]` and `ExactSizeIterator` as we see a perf regression on many projects (while small). Unless the perf regression is related to the `ordering` change. I suggest splitting that change into its own PR so that we have a better understanding where the regression is coming from.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:723 on 2026-01-20 21:28_

Done: https://github.com/astral-sh/ruff/pull/22777

(I have not addressed the other comments below yet; did this first to see what the performance looks like before considering a fallback for smaller vecs/etc)

---

_@dcreager reviewed on 2026-01-20 21:28_

---

_@dcreager reviewed on 2026-01-20 23:03_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:150 on 2026-01-20 23:03_

An `ExactSizeIterator` is required to return the exact size from `size_hint`, too, so I added a fallback that checks if the max size hint is <= 4, and uses the old implementation if so.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:150 on 2026-01-20 23:14_

(That way I didn't have to worry about specialization or adding a new method for exact-sized things)

---

_@dcreager reviewed on 2026-01-20 23:14_

---

_Comment by @ibraheemdev on 2026-01-21 00:18_

Looks like the last commit regressed performance again? 

---

_@ibraheemdev reviewed on 2026-01-21 01:17_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/constraints.rs`:1174 on 2026-01-21 01:17_

It's unclear to me why this is algorithmically cheaper. If the constraint sets are all disjoint, this is equivalent (modulo node ordering, which we can really control here). The worst case is also equivalent. In any case, both BDD constructions operate on the same set of pairs of nodes — so we end up ORing two medium sized constraint sets instead of a large constraint set with a small one, which should end up being equivalent? I may be missing something here, given that the benchmarks agree this is an improvement.

---

_@ibraheemdev reviewed on 2026-01-21 01:18_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/constraints.rs`:1202 on 2026-01-21 01:18_

Doesn't this assume the input constraint sets are of equal or similar size (though I suppose that is mostly true currently)? Should we try sorting by size here?

---

_Comment by @dcreager on 2026-01-21 18:14_

> Looks like the last commit regressed performance again?

When I try to reproduce the timing numbers, I don't get the same results as codspeed:

||`static_frame`|`colour_science`|
|-|-:|-:|
|main|998.6|3036.|
|always|**982.4**|**2956.**|
|smart2|998.2|3371.|
|smart4|1012.&numsp;|3650.|

"main" is the current `main` branch. "always" is this PR as of commit ffd9920b1e785c1a242c3a2bbff31c1d5821c50e — that is, always applying the new tree-shaped traversal, and not falling back on the old logic for small iterator sizes. "smart2" and "smart4" have the fallback logic for iterators of length <= 4 and <= 2, respectively.

From my testing, "always" seems like a clear winner, at least for these two repos. I'm going to repush the PR to that state to see if maybe there was some weird temporary inconsistency in the codspeed results?

---

_@dcreager reviewed on 2026-01-21 18:34_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1174 on 2026-01-21 18:34_

My analysis is admittedly a bit hand-wavy:

If the constraints are all disjoint, then the size of each constraint set (i.e. the number of internal nodes) grows linearly: `|a| = 1`, `|ab| = 2`, `|abc| = 3`, etc.

Each time we invoke a BDD operator, it costs something like `O(m + n)` time.

So with the linear shape (before this PR), you'd end up with a total cost of

```
(((((a ∨ b) ∨ c) ∨ d) ∨ e) ∨ f) ∨ g

ab        2
ab c      3
abc d     4
abcd e    5
abcde f   6
abcdef g  7
         27
```

and with the tree shape, you get

```
((a ∨ b) ∨ (c ∨ d)) ∨ ((e ∨ f) ∨ g)

ab        2
cd        2
ab cd     4
ef        2
ef g      3
abcd efg  7
         20
```

I think that works out to an overall cost of `O(n^2)` for the linear shape, and `O(n log n)` for the tree shape.

---

_@dcreager reviewed on 2026-01-21 18:35_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1202 on 2026-01-21 18:35_

Assuming my analysis above is correct, I think you're right that sorting would give us a better guarantee of always doing the optimally cheapest ordering of operations. But my worry is that the cost of tracking/calculating the node size, and then doing the sort, would counteract any gain that we'd get.

---
