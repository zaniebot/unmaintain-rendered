```yaml
number: 22113
title: "[ty] Add support for dict(...) calls in typed dict contexts"
type: pull_request
state: merged
author: Hugo-Polloli
labels:
  - ty
assignees: []
merged: true
base: main
head: typed-dict-dict-call
created_at: 2025-12-20T14:37:00Z
updated_at: 2025-12-20T15:59:04Z
url: https://github.com/astral-sh/ruff/pull/22113
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Add support for dict(...) calls in typed dict contexts

---

_Pull request opened by @Hugo-Polloli on 2025-12-20 14:37_

## Summary

fixes https://github.com/astral-sh/ty/issues/2127
- handle `dict(...)` calls in TypedDict context with bidirectional inference
- validate keys/values using the existing TypedDict constructor implem
- mdtest: add 1 positive test, 1 negative test for invalid coverage

## Test Plan

```sh
cargo clippy --workspace --all-targets --all-features -- -D warnings  # Rust linting
cargo test  # Rust testing
uvx pre-commit run --all-files --show-diff-on-failure  # Rust and Python formatting, Markdown and Python linting, etc.
```
fully green

---

_Review requested from @carljm by @Hugo-Polloli on 2025-12-20 14:37_

---

_Review requested from @AlexWaygood by @Hugo-Polloli on 2025-12-20 14:37_

---

_Review requested from @sharkdp by @Hugo-Polloli on 2025-12-20 14:37_

---

_Review requested from @dcreager by @Hugo-Polloli on 2025-12-20 14:37_

---

_Label `ty` added by @AlexWaygood on 2025-12-20 14:38_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 14:39_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 14:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/distro/distro.py:994:16: error[invalid-return-type] Return type does not match returned value: expected `InfoDict`, found `dict[str, str | dict[str, str]]`
- Found 4295 diagnostics
+ Found 4294 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/tests/test_cftimeindex_resample.py:259:37: error[invalid-assignment] Object of type `dict[str, str | int]` is not assignable to `DateRangeKwargs`
- Found 1772 diagnostics
+ Found 1771 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 43 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | Top[Yarn[Any]] | ... omitted 6 union elements, generic[object]]`
- Found 1840 diagnostics
+ Found 1839 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/actions/message_send.py:1137:35: error[invalid-assignment] Object of type `dict[str, Unknown | None]` is not assignable to `UserData`
- zerver/lib/export.py:1974:34: error[invalid-assignment] Object of type `dict[str, list[dict[str, Any] | Unknown] | list[int] | Unknown]` is not assignable to `MessagePartial`
- zerver/lib/message.py:946:52: error[invalid-assignment] Object of type `dict[str, dict[int, RawUnreadDirectMessageDict] | dict[int, RawUnreadStreamDict] | set[int] | dict[int, RawUnreadDirectMessageGroupDict] | bool]` is not assignable to `RawUnreadMessagesResult`
- zerver/lib/message.py:1171:36: error[invalid-assignment] Object of type `dict[str, list[UnreadDirectMessageInfo] | list[UnreadStreamInfo] | list[UnreadDirectMessageGroupInfo] | list[int] | int]` is not assignable to `UnreadMessagesResult`
- zerver/lib/user_groups.py:708:37: error[invalid-assignment] Object of type `dict[str, Unknown | int | None | list[int] | UserGroupMembersDict]` is not assignable to `UserGroupDict`
- zerver/models/realm_emoji.py:88:33: error[invalid-assignment] Object of type `dict[str, str | Unknown | None]` is not assignable to `EmojiInfo`
- zerver/tests/test_import_export.py:3202:23: error[invalid-argument-type] Argument to function `do_update_user_custom_profile_data_if_changed` is incorrect: Expected `list[ProfileDataElementUpdateDict]`, found `list[ProfileDataElementUpdateDict | dict[str, Unknown | str]]`
- zerver/tests/test_message_dict.py:360:57: error[invalid-assignment] Object of type `list[UserDisplayRecipient | dict[str, str | int]]` is not assignable to `list[UserDisplayRecipient]`
- zproject/backends.py:1784:16: error[invalid-return-type] Return type does not match returned value: expected `list[ExternalAuthMethodDictT]`, found `list[ExternalAuthMethodDictT | dict[str, Unknown | str | None]]`
- zproject/backends.py:2649:16: error[invalid-return-type] Return type does not match returned value: expected `list[ExternalAuthMethodDictT]`, found `list[ExternalAuthMethodDictT | dict[str, Unknown | str | None]]`
- zproject/backends.py:3584:50: error[invalid-assignment] Object of type `dict[str, str | Any]` is not assignable to `ExternalAuthMethodDictT`
- zproject/backends.py:3843:50: error[invalid-assignment] Object of type `dict[str, str | Any]` is not assignable to `ExternalAuthMethodDictT`
- Found 3664 diagnostics
+ Found 3652 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2092 diagnostics
+ Found 2094 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@carljm approved on 2025-12-20 15:58_

Looks good to me, thank you!

---

_Merged by @carljm on 2025-12-20 15:59_

---

_Closed by @carljm on 2025-12-20 15:59_

---
