```yaml
number: 22684
title: "[ty] Narrow on bool and byte subscripts"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: charlie/neg-sub
head: charlie/bool-sub
created_at: 2026-01-18T16:54:00Z
updated_at: 2026-01-18T18:20:05Z
url: https://github.com/astral-sh/ruff/pull/22684
synced_at: 2026-01-18T19:17:01Z
```

# [ty] Narrow on bool and byte subscripts

---

_@charliermarsh_

## Summary

Low-hanging fruit...

---

_Label `ty` added by @charliermarsh on 2026-01-18 16:54_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 16:55_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 16:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1820 diagnostics
+ Found 1817 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2056 diagnostics
+ Found 2055 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4353 diagnostics
+ Found 4351 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14502 diagnostics
+ Found 14501 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @charliermarsh on 2026-01-18 17:24_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 17:29_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 2 | 0 | 6 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 0 | 4 |
| **Total** | **2** | **0** | **15** |


**[Full report with detailed diff](https://7dcdc777.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://7dcdc777.ty-ecosystem-ext.pages.dev/timing))



---

_Marked ready for review by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 17:29_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 17:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:238 on 2026-01-18 18:16_

as you say, `True.__index__()` returns the same thing as `(1).__index__()` and `hash(True)` returns the same thing as `hash(1)`. So maybe we should just treat this exactly the same way as an `int` subscript?

```suggestion
                        ast::Expr::BooleanLiteral(ast::ExprBooleanLiteral { value, .. }) => {
                            let _ = write!(path, "{}", u8::from(*value) });
                            segments
                                .push(SegmentInfo::new(SegmentKind::IntSubscript, start_offset));
                        }
```

with this patch applied, both the `reveal_type` calls here reveal `str` (on your branch currently, only the first one does):

```py
def f(x: tuple[object, object]):
    if isinstance(x[True], str):
        reveal_type(x[True])
        reveal_type(x[1])
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:249 on 2026-01-18 18:19_

Should we add a new `SegmentKind` variant for bytes literals? It feels distinct to string-literal subscriptions. And e.g. I don't think the `Display` for `MemberExpr` is going to be accurate if it's a `StringSubscript` variant that actually represents a bytes subscript:

https://github.com/astral-sh/ruff/blob/bab571c12c46dd1a8b3070df55b52b718832e17c/crates/ty_python_semantic/src/semantic_index/member.rs#L254-L268

---

_@AlexWaygood reviewed on 2026-01-18 18:20_

---
