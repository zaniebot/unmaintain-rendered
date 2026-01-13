```yaml
number: 22499
title: "[ty] Emit diagnostics for invalid base classes in `type(...)`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: charlie/dyn-expression
head: charlie/dyn-diag
created_at: 2026-01-10T19:08:39Z
updated_at: 2026-01-13T14:39:34Z
url: https://github.com/astral-sh/ruff/pull/22499
synced_at: 2026-01-13T15:29:24Z
```

# [ty] Emit diagnostics for invalid base classes in `type(...)`

---

_@charliermarsh_

## Summary

Tackles a few TODOs from https://github.com/astral-sh/ruff/pull/22291. In pursuit of those TODOs, we also add support for tracking `final`, since we now enforce it for dynamic classes via diagnostics.


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
+ src/async_utils/_graphs.py:49:18: warning[unsupported-dynamic-base] Invalid base for class created via `type()`: Has type `<special-form 'typing.Protocol'>`
+ src/async_utils/corofunc_cache.py:58:18: warning[unsupported-dynamic-base] Invalid base for class created via `type()`: Has type `<special-form 'typing.Protocol'>`
+ src/async_utils/task_cache.py:61:18: warning[unsupported-dynamic-base] Invalid base for class created via `type()`: Has type `<special-form 'typing.Protocol'>`
- Found 9 diagnostics
+ Found 12 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ tests/type/test_scalars.py:541:51: error[subclass-of-final-class] Class `Boolean` cannot inherit from final class `bool`
- Found 640 diagnostics
+ Found 641 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_tests/test_cancelled.py:55:27: error[subclass-of-final-class] Class `Subclass` cannot inherit from final class `Cancelled`
+ src/trio/_core/_tests/test_run.py:2368:27: error[subclass-of-final-class] Class `Subclass` cannot inherit from final class `Nursery`
+ src/trio/_core/_tests/test_run.py:2373:27: error[subclass-of-final-class] Class `Subclass` cannot inherit from final class `CancelScope`
- Found 482 diagnostics
+ Found 485 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5168 diagnostics
+ Found 5170 diagnostics


```

</details>


No memory usage changes detected ✅



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
