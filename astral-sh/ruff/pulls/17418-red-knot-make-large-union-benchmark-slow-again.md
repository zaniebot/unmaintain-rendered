```yaml
number: 17418
title: "[red-knot] make large-union benchmark slow again"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/unionlimit
created_at: 2025-04-16T02:14:16Z
updated_at: 2025-04-16T14:11:55Z
url: https://github.com/astral-sh/ruff/pull/17418
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] make large-union benchmark slow again

---

_@carljm_

## Summary

Now that we've made the large-unions benchmark fast, let's make it slow again!

This adds a following operation (checking `len`) on the large union, which is slow, even though building the large union is now fast. (This is also observed in a real-world code sample.) It's slow because for every element of the union, we fetch its `__len__` method and check it for compatibility with `Sized`.

We can make this fast by extending the grouped-types approach, as discussed in https://github.com/astral-sh/ruff/pull/17403, so that we can do this `__len__` operation (which is identical for every literal string) just once for all literal strings, instead of once per literal string type in the union.

Until we do that, we can make this acceptably fast again for now by setting a lowish limit on union size, which we can increase in the future when we make it fast. This is what I'll do in the next PR.

## Test Plan

`cargo bench --bench red_knot`


---

_Label `red-knot` added by @carljm on 2025-04-16 02:14_

---

_Comment by @github-actions[bot] on 2025-04-16 02:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-04-16 02:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Funionlimit)

### Merging #17418 will **degrade performances by 98.64%**

<sub>Comparing <code>cjm/unionlimit</code> (91df0e5) with <code>main</code> (a1f3619)</sub>



### Summary

`❌ 1` regressions  
`✅ 32` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cjm%2Funionlimit)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_micro[many_string_assignments] `` | 78.9 ms | 5,816.3 ms | -98.64% |


---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/red_knot.rs`:295 on 2025-04-16 07:09_

Should we maybe add parts of your PR description as a (Python or Rust) code comment here? If I came across this benchmark without the explanation, I might have missed that the `len` call is (now) a crucial part of that benchmark.

---

_@sharkdp approved on 2025-04-16 07:09_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-16 13:55_

---

_Review requested from @dcreager by @carljm on 2025-04-16 13:55_

---

_Merged by @carljm on 2025-04-16 14:05_

---

_Closed by @carljm on 2025-04-16 14:05_

---

_Branch deleted on 2025-04-16 14:05_

---
