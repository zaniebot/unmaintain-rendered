```yaml
number: 22093
title: "[ty] Store un-widened type in `Place`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: charlie/place
created_at: 2025-12-19T18:19:21Z
updated_at: 2025-12-24T04:19:59Z
url: https://github.com/astral-sh/ruff/pull/22093
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Store un-widened type in `Place`

---

_Pull request opened by @charliermarsh on 2025-12-19 18:19_

## Summary

See: https://github.com/astral-sh/ruff/pull/22025#discussion_r2632724156


---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2803 diagnostics
+ Found 2806 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2096 diagnostics
+ Found 2098 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5105 diagnostics
+ Found 5107 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@charliermarsh reviewed on 2025-12-19 18:22_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/enums.rs`:323 on 2025-12-19 18:22_

Success...

---

_Label `ty` added by @AlexWaygood on 2025-12-19 18:24_

---

_Renamed from "Store un-widened type in `Place`" to "[ty] Store un-widened type in `Place`" by @AlexWaygood on 2025-12-19 18:24_

---

_Marked ready for review by @charliermarsh on 2025-12-19 18:48_

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 18:48_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-19 18:48_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-19 18:48_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-19 18:48_

---

_Label `internal` added by @AlexWaygood on 2025-12-19 18:51_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-19 18:51_

---

_@MichaReiser reviewed on 2025-12-19 19:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:960 on 2025-12-19 19:22_

It might be time for named fields... 

---

_@charliermarsh reviewed on 2025-12-19 19:24_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:960 on 2025-12-19 19:24_

Now THAT I am capable of doing.

---

_@MichaReiser reviewed on 2025-12-19 19:28_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:960 on 2025-12-19 19:28_

But maybe wait for a review from someone more knowledgeable on the overall approach or doing it in a separate PR

---

_@carljm approved on 2025-12-24 02:07_

The approach looks good here in general! I do think we need a bit of attention to ergonomics. Probably that's just named fields? We could add method(s) to return narrower views on the struct if most matches don't need all the fields, not sure if that's worth it. We can also do any of that as a separate follow-up PR.

---

_Merged by @charliermarsh on 2025-12-24 04:19_

---

_Closed by @charliermarsh on 2025-12-24 04:19_

---

_Branch deleted on 2025-12-24 04:19_

---
