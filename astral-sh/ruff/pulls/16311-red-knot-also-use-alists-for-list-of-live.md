```yaml
number: 16311
title: "[red-knot] Also use alists for list of live bindings/declarations"
type: pull_request
state: closed
author: dcreager
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: dcreager/alist-for-live-bds
created_at: 2025-02-21T22:40:26Z
updated_at: 2025-02-28T19:58:26Z
url: https://github.com/astral-sh/ruff/pull/16311
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Also use alists for list of live bindings/declarations

---

_Pull request opened by @dcreager on 2025-02-21 22:40_

This PR extends #16306 to also use association lists to store the list of live bindings and declarations for each symbol in the use-def map.

---

_Label `performance` added by @dcreager on 2025-02-21 22:40_

---

_Label `red-knot` added by @dcreager on 2025-02-21 22:40_

---

_@dcreager reviewed on 2025-02-21 22:44_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:1 on 2025-02-21 22:44_

Much of the churn is this file is from the new parameters to thread through the `SymbolStates` arenas

---

_Marked ready for review by @dcreager on 2025-02-21 22:45_

---

_Review requested from @carljm by @dcreager on 2025-02-21 22:45_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-21 22:45_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-21 22:45_

---

_Review requested from @sharkdp by @dcreager on 2025-02-21 22:45_

---

_Comment by @github-actions[bot] on 2025-02-21 22:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @carljm on 2025-02-21 23:31_

(Haven't reviewed this one yet, just wanted to note that [CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Falist-for-live-bds) seems to think this one is a 1% cold-check regression. So if that's consistent, we'll need to decide if there are sufficient code simplicity arguments to do it anyway, or if there's something we can do to eliminate the regression.)

---

_@MichaReiser reviewed on 2025-02-23 15:29_

The code changes look good to me (they seem mostly mechanical). 

What would help me review this PR conceptually is a short explanation on how live bindings and declarations are used (what are the creation and access patterns) and why using alists is a good fit.

---

_@MichaReiser reviewed on 2025-02-23 15:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:726 on 2025-02-23 15:31_

I prefer accepting own instances for functions that unconditionally clone the parameter to make the cost more visible to callers. It can also help avoiding unnecessary calls in the cases where doesn't use the owned instance besides calling the function

---

_@MichaReiser reviewed on 2025-02-23 15:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:314 on 2025-02-23 15:34_

Is it possible to keep the `len` comparison to avoid comparing iterators of different lengths?

---

_@MichaReiser reviewed on 2025-02-23 15:35_

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:237 on 2025-02-23 15:35_

Nit: I'd probably move the `Clone` constraint closer to `map` to avoid that we accidentally "inherit" it for other methods adding to this `ListBuilder` impl block

---

_Converted to draft by @dcreager on 2025-02-25 19:56_

---

_Comment by @dcreager on 2025-02-25 19:56_

Putting this back to draft while I fix up some merge conflicts

---

_Comment by @codspeed-hq[bot] on 2025-02-27 01:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Falist-for-live-bds)

### Merging #16311 will **degrade performances by 4.18%**

<sub>Comparing <code>dcreager/alist-for-live-bds</code> (3fb1653) with <code>main</code> (7dad0c4)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Falist-for-live-bds)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 84.2 ms | 87.8 ms | -4.18% |


---

_Comment by @dcreager on 2025-02-28 19:57_

I've tried a few variations on this, but have not been able to get a performance increase.  I get things down to performance-neutral with the reusable alist cells from #16408, but that's not worth the extra code complexity.

---

_Closed by @dcreager on 2025-02-28 19:57_

---

_Branch deleted on 2025-02-28 19:58_

---
