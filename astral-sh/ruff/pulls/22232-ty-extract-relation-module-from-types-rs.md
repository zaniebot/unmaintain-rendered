```yaml
number: 22232
title: "[ty] Extract `relation` module from `types.rs`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/relation-module
created_at: 2025-12-28T08:51:24Z
updated_at: 2026-01-05T13:16:51Z
url: https://github.com/astral-sh/ruff/pull/22232
synced_at: 2026-01-12T15:57:44Z
```

# [ty] Extract `relation` module from `types.rs`

---

_@MichaReiser_

## Summary

This PR introduces a new `relation.rs` module that implements the type relation (anything `TypeRelation`) methods on `Type`. 

This reduces the size of `types.rs` by about 2.5k lines.

A follow-up change could be to extract all type-var-related code into a `type_var` module.

I started on this because I had a very slim hope that reducing the size of `types.rs` would help compile times... It doesn't do in any meaningful way. 

## Test Plan

cargo 


---

_Label `internal` added by @MichaReiser on 2025-12-28 08:51_

---

_Review requested from @carljm by @MichaReiser on 2025-12-28 08:51_

---

_Label `ty` added by @MichaReiser on 2025-12-28 08:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-28 08:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-28 08:51_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-28 08:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-28 08:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-28 08:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2025-12-29 15:02_

---

_Merged by @MichaReiser on 2026-01-05 13:16_

---

_Closed by @MichaReiser on 2026-01-05 13:16_

---

_Branch deleted on 2026-01-05 13:16_

---
