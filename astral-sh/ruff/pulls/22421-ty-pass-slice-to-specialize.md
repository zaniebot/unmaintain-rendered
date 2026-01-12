```yaml
number: 22421
title: "[ty] Pass slice to `specialize`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/specialize-slice
created_at: 2026-01-06T17:46:03Z
updated_at: 2026-01-09T08:45:41Z
url: https://github.com/astral-sh/ruff/pull/22421
synced_at: 2026-01-12T15:57:49Z
```

# [ty] Pass slice to `specialize`

---

_@MichaReiser_

## Summary
We call `specialize` or `to_specialized_class` in many places with a fixed number of specializations, and probably often with the very same arguments. 

Today's `specialize` implementation always collectes all specialization
into a `Box<[T]>` to perform the Salsa lookup. However, that `Box` is immediately discarded if the value was interned before.

This PR changes `specialize` to take a `Cow<[T]>`, allowing allocation-free lookups if the number of specialization-items are known at compile time (or we already have a slice). Call-sites where this isn't the case now have to collect to a `Vec`, but that's no different to what we do today, except that the cost is visible at the call site. 

Codspeed shows a 1-3% perf improvement on different benchmarks.

Requires https://github.com/salsa-rs/salsa/pull/1054

## Test Plan

cargo test


---

_Label `internal` added by @MichaReiser on 2026-01-06 17:46_

---

_Label `ty` added by @MichaReiser on 2026-01-06 17:46_

---

_Label `internal` added by @MichaReiser on 2026-01-06 17:46_

---

_Label `ty` added by @MichaReiser on 2026-01-06 17:46_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 17:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-06 17:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- Found 1838 diagnostics
+ Found 1837 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2100 diagnostics
+ Found 2101 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14492 diagnostics
+ Found 14491 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2026-01-06 17:55_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:1137 on 2026-01-06 17:55_

This is one of the few instances where we now collect into a `Box<[Type]>` only to throw the allocation away. 

I think an alternative is to change `to_specialized_instance` to either take a `SmallVec` or have our own struct that implements `Lookup` but can be a collected value or a slice (or implement `Lookup` for `Cow`?))

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 08:34_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2026-01-07 08:50_

---

_Review requested from @carljm by @MichaReiser on 2026-01-07 08:50_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-07 08:50_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-07 08:50_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-07 08:50_

---

_@AlexWaygood approved on 2026-01-07 08:59_

This is great! We could probably also e.g. adjust the signature of `Type::string_literal()` to take a `Cow<CompactString>` instead of `&str`, if that salsa change lands.

---

_Comment by @MichaReiser on 2026-01-07 09:08_

> This is great! We could probably also e.g. adjust the signature of Type::string_literal() to take a Cow<CompactString> instead of &str, if that salsa change lands.

I think that would require one more `Lookup` implementation on the salsa side

---

_Comment by @MichaReiser on 2026-01-09 08:45_

> This is great! We could probably also e.g. adjust the signature of Type::string_literal() to take a Cow<CompactString> instead of &str, if that salsa change lands.

I didn't end up doing this as part of this PR but the necessary changes are in place in the Salsa version that we use now

---

_Merged by @MichaReiser on 2026-01-09 08:45_

---

_Closed by @MichaReiser on 2026-01-09 08:45_

---

_Branch deleted on 2026-01-09 08:45_

---
