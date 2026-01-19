```yaml
number: 22676
title: "[ty] Fix docstring rendering for literal blocks after doctests"
type: pull_request
state: open
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
base: main
head: claude/fix-issue-2497-j5GU2
created_at: 2026-01-18T11:58:49Z
updated_at: 2026-01-19T15:21:51Z
url: https://github.com/astral-sh/ruff/pull/22676
synced_at: 2026-01-19T15:25:06Z
```

# [ty] Fix docstring rendering for literal blocks after doctests

---

_@MichaReiser_

## Summary

When a doctest ended on a blank line, `in_doctest` was not reset to `false`. This could cause issues when a docstring contained a doctest followed by a literal block (::) with blank lines inside it.

In that scenario, a blank line inside the literal block would incorrectly trigger the doctest-ending logic (because `in_doctest` was still `true` from the earlier doctest), prematurely closing the literal block and causing subsequent code content to be rendered as regular text with `&nbsp;` indentation instead of being inside the code block.

Fixes https://github.com/astral-sh/ty/issues/2497

## Test Plan

Added test

---

_Marked ready for review by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @carljm by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-18 12:32_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-18 12:32_

---

_Converted to draft by @MichaReiser on 2026-01-18 12:32_

---

_Label `bug` added by @MichaReiser on 2026-01-18 12:32_

---

_Label `server` added by @MichaReiser on 2026-01-18 12:32_

---

_Label `ty` added by @MichaReiser on 2026-01-18 12:32_

---

_Renamed from "Fix issue 2497" to "[ty] Fix issue 2497" by @MichaReiser on 2026-01-18 12:32_

---

_Renamed from "[ty] Fix issue 2497" to "[ty] Correctly detect end of doctests" by @MichaReiser on 2026-01-19 14:28_

---

_Renamed from "[ty] Correctly detect end of doctests" to "[ty] Fix docstring rendering for literal blocks after doctests" by @MichaReiser on 2026-01-19 14:29_

---

_@MichaReiser reviewed on 2026-01-19 14:31_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:484 on 2026-01-19 14:31_

I'm not a 100% sure about this change but opening a doctest only sets `in_any_code` and `in_doctest` to `true`. Ending a doctest should, therefore, set `in_doctest` and `in_any_code` to false.

It also seems that `in_literal` and `in_doctest` are exclusive to each other. It could make sense to use an enum over `Text`, `Doctest`, `Literal` with an `is_code()` as state variable instead, to make this clear.

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-19 14:31_

---

_Review request for @carljm removed by @MichaReiser on 2026-01-19 14:31_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-19 14:31_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2026-01-19 14:31_

---

_Marked ready for review by @MichaReiser on 2026-01-19 14:31_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 14:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 14:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1821 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~690MB
+ TOTAL MEMORY USAGE: ~725MB
-     memo metadata = ~167MB
+     memo metadata = ~194MB


```

</details>




---

_Comment by @MichaReiser on 2026-01-19 15:03_

Uff, thanks RustRover for making all the formatting changes. Love it

---
