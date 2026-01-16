```yaml
number: 22586
title: "[ty] Add support for dynamic dataclasses via `make_dataclass`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: charlie/functional-dict
created_at: 2026-01-14T22:20:58Z
updated_at: 2026-01-16T11:49:59Z
url: https://github.com/astral-sh/ruff/pull/22586
synced_at: 2026-01-16T11:56:11Z
```

# [ty] Add support for dynamic dataclasses via `make_dataclass`

---

_@charliermarsh_

## Summary

Like #22327, but for dataclasses.


---

_Label `ty` added by @charliermarsh on 2026-01-14 22:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/experimental/pydantic/error_type.py:149:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ strawberry/experimental/pydantic/object_type.py:255:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 348 diagnostics
+ Found 350 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5410 diagnostics
+ Found 5405 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1823 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2054 diagnostics
+ Found 2055 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14508 diagnostics
+ Found 14509 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @carljm by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-15 02:58_

---

_Converted to draft by @charliermarsh on 2026-01-15 02:59_

---

_Marked ready for review by @charliermarsh on 2026-01-15 03:22_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-15 03:59_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-15 09:18_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 09:23_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 5 | 4 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 0 | 3 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| **Total** | **2** | **5** | **12** |


**[Full report with detailed diff](https://fb720bd3.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://fb720bd3.ty-ecosystem-ext.pages.dev/timing))



---

_Closed by @AlexWaygood on 2026-01-15 13:33_

---

_Reopened by @AlexWaygood on 2026-01-15 13:34_

---

_Comment by @AlexWaygood on 2026-01-15 13:46_

I'm a _bit_ hesitant about this one. The typing spec doesn't say anything about this function AFAIK, and I don't remember seeing any user requests on the issue tracker for us to add special-cased support. Mypy_primer only shows two diagnostics going away in the ecosystem, on `strawberry` -- and it looks like those lines already have `type: ignore`s on them for mypy.

Meanwhile, this is _quite_ a bit of new code, and the manual parsing of arguments in `infer/builder.rs` is quite complicated and error-prone.

Do any other type checkers have special-cased support for this function? We do have to draw the line _somewhere_.

I think I'd feel differently if we could share more of the call-expression-parsing logic with what we've already added for namedtuples (which definitely _was_ necessary), but the schema `make_dataclass()` expects is _just_ different enough that sharing more of the code seems like it could be pretty difficult?

Another -- much simpler -- way to avoid false positives with the objects returned by this function would be to just special-case the return type so that we infer `type[Unknown]` rather than `type` -- that wouldn't involve any custom call-expression parsing.

---

_Comment by @charliermarsh on 2026-01-15 13:59_

AFAICT, Mypy and Pyright do not support it. But... I would still advocate for us to support it? We have all the infrastructure to do so! And almost all of the new code is in argument parsing -- which is very verbose, but at least fairly mechanical and testable? (E.g., it's much easier to know when we have that right, as opposed to something deep in type inference.) I think I don't agree that _this_ is where we should draw the line, unless it's a feature that we can't support for reasons that I don't yet understand or would be uncovered by your review.


---

_Comment by @AlexWaygood on 2026-01-15 14:01_

(I'm curious for @carljm's and @dcreager's opinions!)

---

_Comment by @charliermarsh on 2026-01-15 14:08_

Ditto and am totally fine to be overruled on it of course. I took it as a given that we'd support this given that it's in the [type system overview](https://github.com/astral-sh/ty/issues/1889); that may have been a mistake!

---

_Comment by @AlexWaygood on 2026-01-15 14:10_

> Ditto and am totally fine to be overruled on it of course. I took it as a given that we'd support this given that it's in the [type system overview](https://github.com/astral-sh/ty/issues/1889); that may have been a mistake!

Well, that issue just says that we don't have any tests for the function right now. But this PR adds a lot more than just test coverage 😆

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:7735 on 2026-01-15 14:12_

Can we match on the elements here. The same in other positions
```suggestion
                    [name_expr, type_expr] => {
```

---

_@MichaReiser reviewed on 2026-01-15 14:12_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:5833 on 2026-01-15 14:13_

It seems unfortunate that we have to repeat all those methods for every dynamic class literal. Can't we share more infrastructure?

---

_@MichaReiser reviewed on 2026-01-15 14:13_

---

_Comment by @carljm on 2026-01-16 00:53_

I think it's pretty natural to support this (along with similar forms for namedtuple and TypedDict) and it seems pretty easy to do. I'm not really concerned by the argument parsing. It's a chunk of code, sure, but it's not a chunk of code that interacts with other things in a way that multiplies complexity; it's pretty much standalone, we'll only ever touch it if literally `make_dataclass` support is broken, and then we'll know right where to look.

The fact that other type checkers don't support it means I wouldn't have considered it a priority for stable -- but if it's done and working, I don't see any significant downside to merging it.

---

_Comment by @AlexWaygood on 2026-01-16 11:49_

Okiedokie

---
