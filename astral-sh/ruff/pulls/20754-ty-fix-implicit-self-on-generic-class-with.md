```yaml
number: 20754
title: "[ty] fix implicit Self on generic class with typevar default"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/self-lazy-default
created_at: 2025-10-07T20:44:47Z
updated_at: 2025-10-08T01:46:34Z
url: https://github.com/astral-sh/ruff/pull/20754
synced_at: 2026-01-10T17:34:34Z
```

# [ty] fix implicit Self on generic class with typevar default

---

_Pull request opened by @carljm on 2025-10-07 20:44_

## Summary

Typevar attributes (bound/constraints/default) can be either lazily evaluated or eagerly evaluated. Currently they are lazily evaluated for PEP 695 typevars, and eager for legacy and synthetic typevars. https://github.com/astral-sh/ruff/pull/20598 will make them lazy also for legacy typevars, and the ecosystem report on that PR surfaced the issue fixed here (because legacy typevars are much more common in the ecosystem than PEP 695 typevars.)

Applying a transform to a typevar (normalization, materialization, or mark-inferable) will reify all lazy attributes and create a new typevar with eager attributes. In terms of Salsa identity, this transformed typevar will be considered different from the original typevar, whether or not the attributes were actually transformed.

In general, this is not a problem, since all typevars in a given generic context will be transformed, or not, together.

The exception to this was implicit-self vs explicit Self annotations. The typevar we created for implicit self was created initially using inferable typevars, whereas an explicit Self annotation is initially non-inferable, then transformed via mark-inferable when accessed as part of a function signature. If the containing class (which becomes the upper bound of `Self`) is generic, and has e.g. a lazily-evaluated default, then the explicit-Self annotation will reify that default in the upper bound, and the implicit-self would not, leading them to be treated as different typevars, and causing us to fail to solve a call to a method such as `def method(self) -> Self` correctly.

The fix here is to treat implicit-self more like explicit-Self, initially creating it as non-inferable and then using the mark-inferable transform on it. This is less efficient, but restores the invariant that all typevars in a given generic context are transformed together, or not, fixing the bug.

In the improved-constraint-solver work, the separation of typevars into "inferable" and "non-inferable" is expected to disappear, along with the mark-inferable transform, which would render both this bug and the fix moot. So this fix is really just temporary until that lands.

There is a performance regression, but not a huge one: 1-2% on most projects, 5% on one outlier. This seems acceptable, given that it should be fully recovered by removing the mark-inferable transform.

## Test Plan

Added mdtests that failed before this change.


---

_Comment by @github-actions[bot] on 2025-10-07 20:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-07 20:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @carljm on 2025-10-07 20:53_

---

_Comment by @github-actions[bot] on 2025-10-07 20:58_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 3 | 0 | 0 |
| `no-matching-overload` | 2 | 0 | 0 |
| **Total** | **5** | **0** | **0** |

**[Full report with detailed diff](https://cjm-self-lazy-default.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-self-lazy-default.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-07 21:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fself-lazy-default)

### Merging #20754 will **degrade performances by 4.96%**

<sub>Comparing <code>cjm/self-lazy-default</code> (a70eb5e) with <code>main</code> (ff386b4)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fself-lazy-default?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime) | 4.9 s | 5.1 s | -4.96% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fself-lazy-default?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Marked ready for review by @carljm on 2025-10-08 00:00_

---

_Review requested from @AlexWaygood by @carljm on 2025-10-08 00:00_

---

_Review requested from @sharkdp by @carljm on 2025-10-08 00:00_

---

_Review requested from @dcreager by @carljm on 2025-10-08 00:00_

---

_Label `ty` added by @AlexWaygood on 2025-10-08 00:27_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:314 on 2025-10-08 00:39_

Would it be helpful to add tests that fall back on the default specialization?

---

_@dcreager approved on 2025-10-08 00:39_

Agreed that the performance regression is acceptable seeing as how it's temporary. #20677 should land soon; I'm just tracking down some missing pieces in overload resolution.

---

_@carljm reviewed on 2025-10-08 01:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:314 on 2025-10-08 01:32_

It's not really related to this bug at all, but maybe it makes the test a bit more comprehensive for general regression-catching? Sure, I can add that.

---

_Merged by @carljm on 2025-10-08 01:38_

---

_Closed by @carljm on 2025-10-08 01:38_

---

_Branch deleted on 2025-10-08 01:38_

---
