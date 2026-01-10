```yaml
number: 22096
title: "[ty] Run mypy_primer and typing-conformance workflows on fewer PRs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/less-mypy-primer
created_at: 2025-12-19T18:43:25Z
updated_at: 2025-12-19T19:31:22Z
url: https://github.com/astral-sh/ruff/pull/22096
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Run mypy_primer and typing-conformance workflows on fewer PRs

---

_Pull request opened by @AlexWaygood on 2025-12-19 18:43_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2090. Quoting my rationale from that issue:

> A PR that only touches code in [one of these crates] should never have any impact on memory usage or diagnostics produced. And the comments from the bot just lead to additional notifications which is annoying.

I _think_ I've got the syntax right here. The [docs](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax) say:

>  The order that you define paths patterns matters:
>
>  - A matching negative pattern (prefixed with !) after a positive match will exclude the path.
>  - A matching positive pattern after a negative match will include the path again.

## Test Plan

No idea? Merge it and see?

---

_Label `ci` added by @AlexWaygood on 2025-12-19 18:43_

---

_Label `ty` added by @AlexWaygood on 2025-12-19 18:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 18:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2805 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2093 diagnostics
+ Found 2091 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5083 diagnostics
+ Found 5086 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14425 diagnostics
+ Found 14426 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review requested from @carljm by @AlexWaygood on 2025-12-19 18:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-19 18:47_

---

_@MichaReiser reviewed on 2025-12-19 19:17_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:12 on 2025-12-19 19:17_

```suggestion
```

---

_@MichaReiser reviewed on 2025-12-19 19:19_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:12 on 2025-12-19 19:19_

You also want to exclude `ty_completion_eval`. 

---

_Review comment by @MichaReiser on `.github/workflows/typing_conformance.yaml`:12 on 2025-12-19 19:19_

Same

---

_@MichaReiser approved on 2025-12-19 19:19_

---

_@AlexWaygood reviewed on 2025-12-19 19:22_

---

_Review comment by @AlexWaygood on `.github/workflows/mypy_primer.yaml`:12 on 2025-12-19 19:22_

Why should we run mypy_primer on changes to the `ty_server` crate? Is it possible that changes to that crate will ever change the diagnostics we emit?

---

_@MichaReiser reviewed on 2025-12-19 19:23_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:12 on 2025-12-19 19:23_

It's listed twice :)

---

_@AlexWaygood reviewed on 2025-12-19 19:24_

---

_Review comment by @AlexWaygood on `.github/workflows/mypy_primer.yaml`:12 on 2025-12-19 19:24_

Lol thanks 😆

---

_Review comment by @AlexWaygood on `.github/workflows/mypy_primer.yaml`:12 on 2025-12-19 19:26_

```suggestion
      - "!crates/ty_completion_eval/**"
```

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance.yaml`:12 on 2025-12-19 19:26_

```suggestion
      - "!crates/ty_completion_eval/**"
```

---

_@AlexWaygood reviewed on 2025-12-19 19:27_

---

_Merged by @AlexWaygood on 2025-12-19 19:31_

---

_Closed by @AlexWaygood on 2025-12-19 19:31_

---

_Branch deleted on 2025-12-19 19:31_

---
