```yaml
number: 22499
title: "[ty] Emit diagnostics for invalid base classes in `type(...)`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/dyn-diag
created_at: 2026-01-10T19:08:39Z
updated_at: 2026-01-14T13:56:06Z
url: https://github.com/astral-sh/ruff/pull/22499
synced_at: 2026-01-14T14:41:25Z
```

# [ty] Emit diagnostics for invalid base classes in `type(...)`

---

_@charliermarsh_

## Summary

Tackles a few TODOs from https://github.com/astral-sh/ruff/pull/22291.


---

_Label `ty` added by @charliermarsh on 2026-01-10 19:08_

---

_Comment by @astral-sh-bot[bot] on 2026-01-10 19:10_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-10 19:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
+ src/async_utils/_graphs.py:49:18: warning[unsupported-dynamic-base] Unsupported base for class created via `type()`: Has type `<special-form 'typing.Protocol'>`
+ src/async_utils/corofunc_cache.py:58:18: warning[unsupported-dynamic-base] Unsupported base for class created via `type()`: Has type `<special-form 'typing.Protocol'>`
+ src/async_utils/task_cache.py:61:18: warning[unsupported-dynamic-base] Unsupported base for class created via `type()`: Has type `<special-form 'typing.Protocol'>`
- Found 9 diagnostics
+ Found 12 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ tests/type/test_scalars.py:541:51: error[subclass-of-final-class] Class `Boolean` cannot inherit from final class `bool`
- Found 640 diagnostics
+ Found 641 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_tests/test_cancelled.py:55:27: error[subclass-of-final-class] Class `Subclass` cannot inherit from final class `Cancelled`
+ src/trio/_core/_tests/test_run.py:2368:27: error[subclass-of-final-class] Class `Subclass` cannot inherit from final class `Nursery`
+ src/trio/_core/_tests/test_run.py:2373:27: error[subclass-of-final-class] Class `Subclass` cannot inherit from final class `CancelScope`
- Found 482 diagnostics
+ Found 485 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
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
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5369 diagnostics
+ Found 5364 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2056 diagnostics
+ Found 2057 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14505 diagnostics
+ Found 14506 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB


```

</details>




---

_Marked ready for review by @charliermarsh on 2026-01-11 14:27_

---

_Review requested from @carljm by @charliermarsh on 2026-01-11 14:27_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-11 14:27_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-11 14:27_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-11 14:27_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-12 04:02_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-12 04:02_

---

_Converted to draft by @charliermarsh on 2026-01-12 20:39_

---

_Marked ready for review by @charliermarsh on 2026-01-13 02:26_

---

_Comment by @AlexWaygood on 2026-01-13 13:46_

> ```diff
> + src/async_utils/_graphs.py:49:18: warning[unsupported-base] Invalid base for class created via `type()`: Has type `<special-form 'typing.Protocol'>`
> ```

Shouldn't that be `unsupported-dynamic-base` rather than `unsupported-base`?

---

_Review request for @Gankra removed by @AlexWaygood on 2026-01-13 13:54_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2026-01-13 13:54_

---

_Comment by @charliermarsh on 2026-01-13 14:26_

Do you mean, exclusively for the protocol case? (As opposed to, e.g., inheriting from enums?) Since that one is a limitation?

---

_Converted to draft by @charliermarsh on 2026-01-13 14:39_

---

_Comment by @charliermarsh on 2026-01-13 16:45_

(Actually not sure if we're supposed to support `final` as a non-decorator, doing some research.)

---

_Marked ready for review by @charliermarsh on 2026-01-13 16:51_

---

_Comment by @charliermarsh on 2026-01-13 16:52_

My read of https://typing.python.org/en/latest/spec/qualifiers.html is that `final` is only intended to work as a decorator, and not via `final(type(...))`. Other type checkers don't enforce that either. So I omitted `final` support.

---

_@AlexWaygood reviewed on 2026-01-13 16:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1082 on 2026-01-13 16:53_

can we therefore emit a diagnostic here warning the user that this has no effect? I don't like silently doing nothing in a case where the user probably expects us to do something 😄

This can be a followup.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:931 on 2026-01-13 16:55_

`unsupported-base` is meant to be for things that probably won't cause runtime errors, but which we can't support. But these actually cause runtime errors! So maybe they should be `invalid-base`?

```pycon
>>> GenericClass = type("GenericClass", (Generic[T],), {})
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    GenericClass = type("GenericClass", (Generic[T],), {})
TypeError: type() doesn't support MRO entry resolution; use types.new_class()
```

---

_@AlexWaygood reviewed on 2026-01-13 16:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1009 on 2026-01-13 16:56_

same here, this actually fails at runtime, so maybe the error code should be `invalid-base`

---

_@AlexWaygood reviewed on 2026-01-13 16:56_

---

_@AlexWaygood reviewed on 2026-01-13 16:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/diagnostics/unsupported_base_dynamic_type.md`:49 on 2026-01-13 16:57_

and here, this should probably be `invalid-base`

```pycon
>>> from typing import TypedDict
... 
... X = type("X", (TypedDict,), {})
... 
Traceback (most recent call last):
  File "<python-input-0>", line 3, in <module>
    X = type("X", (TypedDict,), {})
TypeError: type() doesn't support MRO entry resolution; use types.new_class()
```

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1082 on 2026-01-13 16:58_

Will do. I think that should fire for "static" classes too.

---

_@charliermarsh reviewed on 2026-01-13 16:58_

---

_@AlexWaygood reviewed on 2026-01-13 17:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:1082 on 2026-01-13 17:01_

yeah, that makes sense.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/type.md`:927 on 2026-01-13 17:15_

nit: this one _won't_ fail at runtime...

```suggestion
# error: [unsupported-dynamic-base] "Unsupported base for class created via `type()`"
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/unsupported_base_dyn…_-_Unsupported_base_for…_-_Enum_base_(4873196c8b48364).snap`:36 on 2026-01-13 17:16_

eh... if they're reaching for the dynamic way of creating classes, it's probably more likely here that they want `X = Enum("MyEnum", [])` here, no?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/unsupported_base_dyn…_-_Unsupported_base_for…_-_`TypedDict`_base_(6f76171c88fc8760).snap`:33 on 2026-01-13 17:17_

similarly here, suggesting `X = TypedDict("X", {})` would probably be closer to the mark?

---

_@AlexWaygood approved on 2026-01-13 17:18_

nice, a few more minor nits

---

_@AlexWaygood approved on 2026-01-13 17:33_

---

_Merged by @charliermarsh on 2026-01-14 13:56_

---

_Closed by @charliermarsh on 2026-01-14 13:56_

---

_Branch deleted on 2026-01-14 13:56_

---
