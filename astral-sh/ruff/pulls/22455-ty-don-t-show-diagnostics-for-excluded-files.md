```yaml
number: 22455
title: "[ty] Don't show diagnostics for excluded files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-is-directory
created_at: 2026-01-08T11:14:10Z
updated_at: 2026-01-08T17:27:31Z
url: https://github.com/astral-sh/ruff/pull/22455
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Don't show diagnostics for excluded files

---

_Pull request opened by @MichaReiser on 2026-01-08 11:14_

## Summary

Respect the `exclude` patterns in the LSP so that we don't show diagnostics in excluded files. 


## Test Plan

Added e2e test


---

_Label `server` added by @MichaReiser on 2026-01-08 11:14_

---

_Review requested from @carljm by @MichaReiser on 2026-01-08 11:14_

---

_Label `ty` added by @MichaReiser on 2026-01-08 11:14_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-08 11:14_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-08 11:14_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-08 11:14_

---

_@MichaReiser reviewed on 2026-01-08 11:15_

---

_Review comment by @MichaReiser on `crates/ty_project/src/glob/exclude.rs`:53 on 2026-01-08 11:15_

We incorrectly passed the value `directory` here; it should always be true when testing the ancestor paths.

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 11:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-08 11:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2084 diagnostics
+ Found 2083 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14487 diagnostics
+ Found 14486 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@Gankra approved on 2026-01-08 17:16_

Makes sense to me

---

_@Gankra reviewed on 2026-01-08 17:17_

---

_Review comment by @Gankra on `crates/ty_server/src/session.rs`:920 on 2026-01-08 17:17_

(This part feels genuinely kinda suspicious but I'm taking your word for it that this won't do something Really Bad)

---

_@MichaReiser reviewed on 2026-01-08 17:27_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:920 on 2026-01-08 17:27_

Haha yeah. We mainly use `open_files` in `Project` to determine which files we need to type-check. I think the only other use case is that we use it to determine which ASTs we shouldn't GC when checking is done. But, given that we don't check those files, this isn't a code path that we hit.

---

_Merged by @MichaReiser on 2026-01-08 17:27_

---

_Closed by @MichaReiser on 2026-01-08 17:27_

---

_Branch deleted on 2026-01-08 17:27_

---
