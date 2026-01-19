```yaml
number: 22615
title: "[ty] Support overriding `respect-type-ignore-comments`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/analysis-file-level-overrides
created_at: 2026-01-16T11:07:32Z
updated_at: 2026-01-19T08:09:44Z
url: https://github.com/astral-sh/ruff/pull/22615
synced_at: 2026-01-19T08:25:13Z
```

# [ty] Support overriding `respect-type-ignore-comments`

---

_@MichaReiser_

## Summary

Adds support for overriding `respect-type-ignore-comments` in the `overrides` section.

Fixes https://github.com/astral-sh/ty/issues/2530

## Test Plan

Added CLI tests


---

_Review requested from @carljm by @MichaReiser on 2026-01-16 11:07_

---

_Label `configuration` added by @MichaReiser on 2026-01-16 11:07_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-16 11:07_

---

_Label `ty` added by @MichaReiser on 2026-01-16 11:07_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-16 11:07_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-16 11:07_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-16 11:07_

---

_Label `configuration` added by @MichaReiser on 2026-01-16 11:07_

---

_Label `ty` added by @MichaReiser on 2026-01-16 11:07_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 11:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_@MichaReiser reviewed on 2026-01-16 11:09_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:1617 on 2026-01-16 11:09_

When we start adding analysis options that can't be overriden (like a `strict` mode), we then have to warn here that this specific override setting was ignored OR define a separate `OverrideAnalysisOptions` that only defines the subset of options allowed in this section (so that editors don't provide completions for options that aren't allowed). However, this is something that we can figure out once we have non-overridable analysis options.

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 11:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1821 diagnostics
+ Found 1825 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-16 11:12_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 11:30_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Renamed from "[ty] Support overriding `respect-type-ignore-comments" to "[ty] Support overriding `respect-type-ignore-comments`" by @AlexWaygood on 2026-01-16 12:40_

---

_@charliermarsh approved on 2026-01-17 15:34_

---

_Merged by @MichaReiser on 2026-01-19 08:09_

---

_Closed by @MichaReiser on 2026-01-19 08:09_

---

_Branch deleted on 2026-01-19 08:09_

---
