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
updated_at: 2026-01-20T22:17:08Z
url: https://github.com/astral-sh/ruff/pull/22614
synced_at: 2026-01-20T22:52:51Z
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
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/checks.py:390:42: error[invalid-assignment] Object of type `Coroutine[Any, Any, Cooldown | None] | Cooldown | None` is not assignable to `Cooldown | None`
- Found 540 diagnostics
+ Found 539 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
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
- src/prefect/concurrency/_leases.py:89:53: error[invalid-argument-type] Argument to bound method `add_done_callback` is incorrect: Expected `(Future[CoroutineType[Any, Any, None]], /) -> object`, found `def handle_lease_renewal_failure(future: Future[None]) -> Unknown`
- src/prefect/utilities/asyncutils.py:198:16: error[invalid-return-type] Return type does not match returned value: expected `R@run_coro_as_sync | None`, found `CoroutineType[Any, Any, R@run_coro_as_sync | None]`
- src/prefect/utilities/asyncutils.py:207:20: error[invalid-return-type] Return type does not match returned value: expected `R@run_coro_as_sync | None`, found `CoroutineType[Any, Any, R@run_coro_as_sync | None]`
- Found 5412 diagnostics
+ Found 5391 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/formats/pandas.py:115:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Overload[(objs: Iterable[None] | Mapping[HashableT1@concat, None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2@concat] | None = None, levels: Sequence[list[HashableT3@concat] | tuple[HashableT3@concat, ...]] | None = None, names: list[HashableT4@concat] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> Series[S2@concat] | Series[Any], (objs: Iterable[Series[S2@concat] | None] | Mapping[HashableT1@concat, Series[S2@concat] | None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2@concat] | None = None, levels: Sequence[list[HashableT3@concat] | tuple[HashableT3@concat, ...]] | None = None, names: list[HashableT4@concat] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> Series[S2@concat] | Series[Any], (objs: Iterable[Series[Any] | None] | Mapping[HashableT1@concat, Series[Any] | None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2@concat] | None = None, levels: Sequence[list[HashableT3@concat] | tuple[HashableT3@concat, ...]] | None = None, names: list[HashableT4@concat] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> Series[S2@concat] | Series[Any], (objs: Iterable[NDFrame | None] | Mapping[HashableT1@concat, NDFrame | None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2@concat] | None = None, levels: Sequence[list[HashableT3@concat] | tuple[HashableT3@concat, ...]] | None = None, names: list[HashableT4@concat] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> Series[S2@concat] | Series[Any]]`, found `Overload[[HashableT1, HashableT2, HashableT3, HashableT4](objs: Iterable[None] | Mapping[HashableT1, None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2] | None = None, levels: Sequence[list[HashableT3] | tuple[HashableT3, ...]] | None = None, names: list[HashableT4] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> Never, [S2, HashableT1, HashableT2, HashableT3, HashableT4](objs: Iterable[Series[S2] | None] | Mapping[HashableT1, Series[S2] | None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2] | None = None, levels: Sequence[list[HashableT3] | tuple[HashableT3, ...]] | None = None, names: list[HashableT4] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> Series[S2], [HashableT1, HashableT2, HashableT3, HashableT4](objs: Iterable[Series[Any] | None] | Mapping[HashableT1, Series[Any] | None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2] | None = None, levels: Sequence[list[HashableT3] | tuple[HashableT3, ...]] | None = None, names: list[HashableT4] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> Series[Any], [HashableT1, HashableT2, HashableT3, HashableT4](objs: Iterable[NDFrame | None] | Mapping[HashableT1, NDFrame | None], *, axis: Unknown = 0, join: Literal["inner", "outer"] = "outer", ignore_index: bool = False, keys: Iterable[HashableT2] | None = None, levels: Sequence[list[HashableT3] | tuple[HashableT3, ...]] | None = None, names: list[HashableT4] | None = None, verify_integrity: bool = False, sort: bool = False, copy: bool = True) -> DataFrame]`
- Found 4609 diagnostics
+ Found 4610 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2058 diagnostics
+ Found 2059 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14465 diagnostics
+ Found 14464 diagnostics


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

### Merging this PR will **not alter performance**

<sub>Comparing <code>dcreager/distributed-ops</code> (ffd9920) with <code>main</code> (3b5d0d5)</sub>



### Summary

`✅ 23` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  




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
