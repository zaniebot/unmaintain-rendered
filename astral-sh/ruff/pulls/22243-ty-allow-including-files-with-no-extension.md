```yaml
number: 22243
title: "[ty] Allow including files with no extension"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/include-files-without-extension
created_at: 2025-12-29T14:09:16Z
updated_at: 2026-01-07T10:38:03Z
url: https://github.com/astral-sh/ruff/pull/22243
synced_at: 2026-01-12T15:57:44Z
```

# [ty] Allow including files with no extension

---

_@MichaReiser_

## Summary

This PR adds support for including files that have no extension or use a non-standard extension. 
Such files can be included by explicitly listing them in the `include` configuration. ty will assume that those files are Python files

Closes https://github.com/astral-sh/ty/issues/2235

## Test Plan

Added CLI test


---

_Label `configuration` added by @MichaReiser on 2025-12-29 14:09_

---

_Label `ty` added by @MichaReiser on 2025-12-29 14:09_

---

_Review requested from @carljm by @MichaReiser on 2025-12-29 14:09_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-29 14:09_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-29 14:09_

---

_Label `configuration` added by @MichaReiser on 2025-12-29 14:09_

---

_Label `ty` added by @MichaReiser on 2025-12-29 14:09_

---

_Renamed from "[ty] Include files with no extension" to "[ty] Allow including files with no extension" by @MichaReiser on 2025-12-29 14:09_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 14:10_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 14:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- Found 1843 diagnostics
+ Found 1844 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2105 diagnostics
+ Found 2103 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14425 diagnostics
+ Found 14424 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @dcreager removed by @MichaReiser on 2025-12-29 14:15_

---

_Review request for @carljm removed by @MichaReiser on 2025-12-29 14:15_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-29 14:15_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-29 14:15_

---

_@MichaReiser reviewed on 2025-12-29 14:16_

---

_Review comment by @MichaReiser on `crates/ty_project/src/glob/include.rs`:37 on 2025-12-29 14:16_

It's a bit silly that we have to track this, given that `globset` has this information internally but there's no API for it.

---

_Review comment by @Gankra on `crates/ty_project/src/glob.rs`:98 on 2026-01-05 13:03_

nit: docstring should probably discuss this otherwise obscure metadata

---

_Review comment by @Gankra on `crates/ty_project/src/glob/include.rs`:155 on 2026-01-05 13:06_

```suggestion
```

I desperately want this to be loadbearing somehow

---

_Review comment by @Gankra on `crates/ty_project/src/walk.rs`:223 on 2026-01-05 13:13_

Suspicious of the `entry.depth() == 0` check given we're inside `if entry.depth() > 0 || self.force_exclude {`. Don't know the code well enough to say if it still makes sense.

---

_@Gankra approved on 2026-01-05 13:14_

---

_@Gankra reviewed on 2026-01-05 13:17_

---

_Review comment by @Gankra on `crates/ty_project/src/walk.rs`:227 on 2026-01-05 13:17_

Actually wait doesn't this imply `include = ["a.ipynb", "b.pyi"]` will both be forced to be interpretted as `.py`?

---

_@MichaReiser reviewed on 2026-01-05 13:18_

---

_Review comment by @MichaReiser on `crates/ty_project/src/walk.rs`:227 on 2026-01-05 13:18_

The code here only filters out files that are known to be non-Python files. Resolving the source type "for real" happens in `source_text`.

---

_@Gankra reviewed on 2026-01-05 13:25_

---

_Review comment by @Gankra on `crates/ty_project/src/walk.rs`:227 on 2026-01-05 13:25_

Is `entry.path().extension().and_then(PySourceType::try_from_extension)` useless then?

---

_@Gankra reviewed on 2026-01-05 13:27_

---

_Review comment by @Gankra on `crates/ty_project/src/walk.rs`:227 on 2026-01-05 13:27_

Oh I think I understand what you mean -- these `Option<PySourceType>`'s are glorified booleans here.

---

_Review comment by @MichaReiser on `crates/ty_project/src/walk.rs`:227 on 2026-01-05 13:27_

No, it is necessary to filter out files with e.g. a `.txt` extension when including an entire directory. `db.system().source_type` only returns `Some` for files where the source type is overriden.

---

_@MichaReiser reviewed on 2026-01-05 13:27_

---

_Review comment by @BurntSushi on `crates/ty_project/src/glob/include.rs`:203 on 2026-01-06 17:08_

Cute.

---

_Review comment by @BurntSushi on `crates/ty_project/src/glob/include.rs`:37 on 2026-01-06 17:09_

Hah yeah, but I think `globset` only tracks this sort of thing for perf. And that approach could change. So probably not a candidate for a public API unfortunately.

---

_@BurntSushi approved on 2026-01-06 17:12_

---

_@MichaReiser reviewed on 2026-01-07 10:34_

---

_Review comment by @MichaReiser on `crates/ty_project/src/walk.rs`:223 on 2026-01-07 10:34_

Yes, that's completely useless. Thanks for catching it

---

_Merged by @MichaReiser on 2026-01-07 10:38_

---

_Closed by @MichaReiser on 2026-01-07 10:38_

---

_Branch deleted on 2026-01-07 10:38_

---
