```yaml
number: 21580
title: "[ty] Fix rendering of unused suppression diagnostic"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: micha/fix-unused-suppression
created_at: 2025-11-22T16:13:35Z
updated_at: 2025-11-22T17:42:58Z
url: https://github.com/astral-sh/ruff/pull/21580
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Fix rendering of unused suppression diagnostic

---

_@MichaReiser_

## Summary

Set the message passed to `context.report_unchecked` as the primary diagnostic message instead of using `""`. 

## Test plan

See changed snapshot

---

_Label `ty` added by @MichaReiser on 2025-11-22 16:13_

---

_Label `diagnostics` added by @MichaReiser on 2025-11-22 16:13_

---

_Review requested from @carljm by @MichaReiser on 2025-11-22 16:13_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-22 16:13_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-22 16:13_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-22 16:13_

---

_Comment by @astral-sh-bot[bot] on 2025-11-22 16:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-22 16:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-11-22 16:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffix-unused-suppression?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21580 will **degrade performances by 4.21%**

<sub>Comparing <code>micha/fix-unused-suppression</code> (5efc4dc) with <code>main</code> (859f9ec)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffix-unused-suppression?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 203.1 s | 212 s | -4.21% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Ffix-unused-suppression?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@AlexWaygood approved on 2025-11-22 16:38_

---

_Comment by @AlexWaygood on 2025-11-22 16:40_

The pydantic benchmark seems to have become flaky recently for some reason (see also https://github.com/astral-sh/ruff/pull/21573#issuecomment-3564780095) 

---

_Merged by @MichaReiser on 2025-11-22 17:42_

---

_Closed by @MichaReiser on 2025-11-22 17:42_

---

_Branch deleted on 2025-11-22 17:42_

---
