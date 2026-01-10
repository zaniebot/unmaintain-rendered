```yaml
number: 21824
title: "[ty] Support comparisons between variable-length tuples"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/tuple
created_at: 2025-12-06T15:46:24Z
updated_at: 2026-01-06T17:09:42Z
url: https://github.com/astral-sh/ruff/pull/21824
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Support comparisons between variable-length tuples

---

_Pull request opened by @charliermarsh on 2025-12-06 15:46_

## Summary

Closes https://github.com/astral-sh/ty/issues/1741.


---

_Comment by @astral-sh-bot[bot] on 2025-12-06 15:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-06 15:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy (https://github.com/python/mypy)
+ mypy/test/data.py:173:41: error[unsupported-operator] Operator `>=` is not supported between objects of type `_version_info` and `tuple[int, ...]`
- Found 1753 diagnostics
+ Found 1754 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
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
- Found 5533 diagnostics
+ Found 5528 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/externals/_packaging/version.py:104:16: error[unsupported-operator] Operator `<` is not supported between two objects of type `tuple[int, tuple[int, ...], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], NegativeInfinityType | tuple[InfinityType | NegativeInfinityType | int | ... omitted 3 union elements, ...]] | tuple[int, tuple[str, ...]]`
+ sklearn/externals/_packaging/version.py:110:16: error[unsupported-operator] Operator `<=` is not supported between two objects of type `tuple[int, tuple[int, ...], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], NegativeInfinityType | tuple[InfinityType | NegativeInfinityType | int | ... omitted 3 union elements, ...]] | tuple[int, tuple[str, ...]]`
+ sklearn/externals/_packaging/version.py:122:16: error[unsupported-operator] Operator `>=` is not supported between two objects of type `tuple[int, tuple[int, ...], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], NegativeInfinityType | tuple[InfinityType | NegativeInfinityType | int | ... omitted 3 union elements, ...]] | tuple[int, tuple[str, ...]]`
+ sklearn/externals/_packaging/version.py:128:16: error[unsupported-operator] Operator `>` is not supported between two objects of type `tuple[int, tuple[int, ...], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], InfinityType | NegativeInfinityType | tuple[str, int], NegativeInfinityType | tuple[InfinityType | NegativeInfinityType | int | ... omitted 3 union elements, ...]] | tuple[int, tuple[str, ...]]`
- Found 2429 diagnostics
+ Found 2433 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2094 diagnostics
+ Found 2092 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14474 diagnostics
+ Found 14475 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-06 15:53_

The scikit-learn changes seem... correct? Comparing `LegacyCmpKey = Tuple[int, Tuple[str, ...]]` to `CmpKey = Tuple[int, Tuple[int, ...], PrePostDevType, PrePostDevType, PrePostDevType, LocalType]` can indeed be a type error?

---

_Renamed from "Support comparisons between variable-length tuples" to "[ty] Support comparisons between variable-length tuples" by @AlexWaygood on 2025-12-07 13:37_

---

_Label `ty` added by @AlexWaygood on 2025-12-07 13:37_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-07 13:38_

---

_Comment by @astral-sh-bot[bot] on 2025-12-07 13:47_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 1 | 4 |
| `unsupported-operator` | 5 | 0 | 0 |
| `invalid-argument-type` | 0 | 3 | 0 |
| **Total** | **6** | **4** | **4** |


**[Full report with detailed diff](https://c4ca9219.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://c4ca9219.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @charliermarsh on 2026-01-05 21:03_

---

_Review requested from @carljm by @charliermarsh on 2026-01-05 21:03_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-05 21:03_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-05 21:03_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-05 21:03_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:11695 on 2026-01-06 16:09_

All of this screams for a helper method that would do a "smart `zip`" of two `Tuple`s! But I don't think that needs to block this PR.

---

_@dcreager approved on 2026-01-06 16:13_

---

_Merged by @charliermarsh on 2026-01-06 17:09_

---

_Closed by @charliermarsh on 2026-01-06 17:09_

---

_Branch deleted on 2026-01-06 17:09_

---
