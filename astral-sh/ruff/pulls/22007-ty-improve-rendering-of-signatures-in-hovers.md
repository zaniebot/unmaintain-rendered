```yaml
number: 22007
title: "[ty] improve rendering of signatures in hovers"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/signature-blegh
created_at: 2025-12-16T14:43:58Z
updated_at: 2025-12-17T20:09:32Z
url: https://github.com/astral-sh/ruff/pull/22007
synced_at: 2026-01-12T15:57:38Z
```

# [ty] improve rendering of signatures in hovers

---

_@Gankra_

This is the return of #21438 because we never found anything better and I think it would be good to have this for the beta.

---

_Review requested from @carljm by @Gankra on 2025-12-16 14:43_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-16 14:43_

---

_Review requested from @sharkdp by @Gankra on 2025-12-16 14:43_

---

_Review requested from @dcreager by @Gankra on 2025-12-16 14:43_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-16 14:43_

---

_Label `server` added by @Gankra on 2025-12-16 14:43_

---

_Label `ty` added by @Gankra on 2025-12-16 14:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 14:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 14:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_groupby.py:439:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
- tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
+ tests/test_resampler.py:402:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2080 diagnostics
+ Found 2082 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14392 diagnostics
+ Found 14391 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_ide/src/hover.rs`:2154 on 2025-12-16 16:12_

it's changes like this that make me very unsure, because a `Callable` type has many differences in our model to function-literal types.

I've been using ty locally for a few weeks now in my editor and I haven't _noticed_ the highlighting of `Callable` types sticking out to me as something gross in on-hover information or anywhere else

---

_@AlexWaygood reviewed on 2025-12-16 16:12_

---

_@Gankra reviewed on 2025-12-16 16:22_

---

_Review comment by @Gankra on `crates/ty_ide/src/hover.rs`:2154 on 2025-12-16 16:22_

I could remove this fallback, I care less about it.

---

_Comment by @Gankra on 2025-12-16 16:52_

I've removed the underscore fallback, so this should really only trigger in cases where it's ultimately an actual function that we got confused about for whatever reason (usually overloads).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:1678 on 2025-12-16 20:05_

"make one up" doesn't seem to really describe what we're doing here anymore?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:1685 on 2025-12-16 20:08_

This seems reasonable, if we're tracking a definition for the callable, that name should be a reasonable one to use.

That said, if this is all we're doing it makes me wonder if/why we even need `self.settings.disallow_signature_name` and threading down the definition, for this. Presumably we already had access to the definition in whatever top-level type we were initially rendering that contains a signature; why not always render it there?

---

_@carljm reviewed on 2025-12-16 20:09_

---

_@Gankra reviewed on 2025-12-16 22:30_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:1685 on 2025-12-16 22:30_

Although it's heavily used as a sub-routine, Signature is in fact a top-level Display entry-point (bind, ide_support, signature_help). It's the thing we often end up with as an output of trying to get the "best" overload in LSP features.

---

_@carljm reviewed on 2025-12-17 00:03_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:1685 on 2025-12-17 00:03_

I see, makes sense.

---

_@carljm approved on 2025-12-17 00:04_

Without the `def _` for no-definition callable types, this looks fine to me!

---

_@AlexWaygood approved on 2025-12-17 11:53_

ah nice, this is a good solution. Thank you!

---

_Merged by @Gankra on 2025-12-17 20:09_

---

_Closed by @Gankra on 2025-12-17 20:09_

---

_Branch deleted on 2025-12-17 20:09_

---
