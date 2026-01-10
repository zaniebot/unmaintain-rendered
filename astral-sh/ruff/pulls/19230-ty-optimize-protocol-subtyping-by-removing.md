```yaml
number: 19230
title: "[ty] Optimize protocol subtyping by removing expensive and unnecessary equivalence check from the top of `Type::has_relation_to()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/remove-equivalent-check
created_at: 2025-07-09T11:54:03Z
updated_at: 2025-07-10T08:42:29Z
url: https://github.com/astral-sh/ruff/pull/19230
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Optimize protocol subtyping by removing expensive and unnecessary equivalence check from the top of `Type::has_relation_to()`

---

_Pull request opened by @AlexWaygood on 2025-07-09 11:54_

# Summary

A full equivalence check at the top of `Type::has_relation_to()` is unnecessary. All our mdtests, and all our property tests, pass if it is replaced with a much cheaper naive equality check. The reason for this is that equivalence is generally the same as equality for atomic types (types that cannot contain other types); and "non-atomic" types (types that can contain other types within them) all have comprehensive subtyping logic lower down in the `Type::has_relation_to()` method. For every non-atomic type in our model currently, our subtyping logic is sufficiently generalized to also cover equivalent types; there's no need to check for equivalence separately.

Up till now, the inefficiency here hasn't mattered that much, because comparing two nominal types for equivalence is generally pretty cheap. (It is _not_ cheap for large unions, large intersections or large tuples (or combinations of the three), but these are all somewhat rare.) Unfortunately, however, comparing two _structural_ types for equivalence can be extremely expensive: determining whether two protocols are equivalent can necessitate a full structural check between the two underlying interfaces. That means that this PR yields a 7% speedup when checking `DateType`, which has some large protocols in it. We also see 1% speedups on a lot of other micro- and macro-benchmarks for ty, and no slowdowns: https://codspeed.io/astral-sh/ruff/branches/alex%2Fremove-equivalent-check?runnerMode=Instrumentation.

# Test plan
- Existing tests all pass
- `QUICKCHECK_TESTS=100000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable` passes
- Clean mypy_primer report

---

_Label `performance` added by @AlexWaygood on 2025-07-09 11:54_

---

_Label `ty` added by @AlexWaygood on 2025-07-09 11:54_

---

_Comment by @github-actions[bot] on 2025-07-09 11:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-09 12:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fremove-equivalent-check?runnerMode=Instrumentation)

### Merging #19230 will **improve performances by 7.33%**

<sub>Comparing <code>alex/remove-equivalent-check</code> (6c9d9fb) with <code>main</code> (35a33f0)</sub>



### Summary

`⚡ 1` improvements  
`✅ 39` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` DateType `` | 263.6 ms | 245.6 ms | +7.33% |


---

_Marked ready for review by @AlexWaygood on 2025-07-09 12:15_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-09 12:15_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-09 12:15_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-09 12:15_

---

_Renamed from "[ty] Remove expensive and unnecessary equivalence check from the top of `Type::has_relation_to()`" to "[ty] Optimize protocol subtyping by removing expensive and unnecessary equivalence check from the top of `Type::has_relation_to()`" by @AlexWaygood on 2025-07-09 12:17_

---

_@sharkdp approved on 2025-07-10 08:24_

Very nice! Thank you

---

_Merged by @AlexWaygood on 2025-07-10 08:42_

---

_Closed by @AlexWaygood on 2025-07-10 08:42_

---

_Branch deleted on 2025-07-10 08:42_

---
