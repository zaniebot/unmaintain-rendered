```yaml
number: 22755
title: "[ty] Disallow Self in metaclass and static methods"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/self2
created_at: 2026-01-20T03:50:54Z
updated_at: 2026-01-20T19:29:12Z
url: https://github.com/astral-sh/ruff/pull/22755
synced_at: 2026-01-20T20:43:31Z
```

# [ty] Disallow Self in metaclass and static methods

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/1897.


---

_Label `ty` added by @charliermarsh on 2026-01-20 03:50_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.42% to 77.52%. The percentage of expected errors that received a diagnostic increased from 60.49% to 60.85%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 672 | 676 | +4 | ⏫ (✅) |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 439 | 435 | -4 | ⏬ (✅) |
| Total Diagnostics | 868 | 872 | +4 | ⏫ |
| Precision | 77.42% | 77.52% | +0.10% | ⏫ (✅) |
| Recall | 60.49% | 60.85% | +0.36% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_self_usage.py:113:19](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L113) | invalid-type-form | `Self` cannot be used in a static method |
| [generics_self_usage.py:118:31](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L118) | invalid-type-form | `Self` cannot be used in a static method |
| [generics_self_usage.py:123:37](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L123) | invalid-type-form | `Self` cannot be used in a metaclass |
| [generics_self_usage.py:127:42](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/generics_self_usage.py#L127) | invalid-type-form | `Self` cannot be used in a metaclass |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-20 03:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4454 diagnostics
+ Found 4456 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-20 04:02_

(Needs refinement, not ready for review!)

---

_Comment by @codspeed-hq[bot] on 2026-01-20 14:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **degrade performance by 4.25%**




`❌ 1` regressed benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself2?utm_source=github&utm_medium=comment-v2&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself2?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 4.4 s | 4.6 s | -4.25% |
---

<sub>Comparing <code>charlie/self2</code> (fd312f3) with <code>main</code> (f5c4adf)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself2?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fself2?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Marked ready for review by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @carljm by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-20 15:54_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-20 15:54_

---

_Converted to draft by @charliermarsh on 2026-01-20 16:59_

---

_Marked ready for review by @charliermarsh on 2026-01-20 19:15_

---
