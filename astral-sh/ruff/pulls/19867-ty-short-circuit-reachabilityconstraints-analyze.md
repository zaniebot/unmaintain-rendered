```yaml
number: 19867
title: "[ty] Short circuit `ReachabilityConstraints::analyze_single` for dynamic types"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/short-circuit-into-callable
created_at: 2025-08-11T16:29:36Z
updated_at: 2025-08-11T19:58:36Z
url: https://github.com/astral-sh/ruff/pull/19867
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Short circuit `ReachabilityConstraints::analyze_single` for dynamic types

---

_Pull request opened by @MichaReiser on 2025-08-11 16:29_

## Summary

Fixes https://github.com/astral-sh/ty/issues/968

This reduces the wall time for a large project from ~24s to ~19s. I plan to do another perf recording to verify that the lock congestion is entirely gone but I first need to walk the dog :)

See inline comment for more details.

## Test Plan

`cargo nextest`


---

_Label `performance` added by @MichaReiser on 2025-08-11 16:29_

---

_Review requested from @carljm by @MichaReiser on 2025-08-11 16:29_

---

_Label `ty` added by @MichaReiser on 2025-08-11 16:29_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-11 16:29_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-11 16:29_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-11 16:29_

---

_Label `performance` added by @MichaReiser on 2025-08-11 16:29_

---

_Label `ty` added by @MichaReiser on 2025-08-11 16:29_

---

_Comment by @github-actions[bot] on 2025-08-11 16:31_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-11 16:39_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fshort-circuit-into-callable?runnerMode=WallTime)

### Merging #19867 will **not alter performance**

<sub>Comparing <code>micha/short-circuit-into-callable</code> (c353acd) with <code>main</code> (f3f4db7)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Comment by @MichaReiser on 2025-08-11 16:40_

Nice, this is even a win for non concurrent runs. All credit goes to @carljm for the much better fix than what I came up with

---

_@dcreager approved on 2025-08-11 16:47_

---

_@AlexWaygood approved on 2025-08-11 16:48_

Very nice!!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:826 on 2025-08-11 16:49_

Sorry, I gave you the wrong info here, I had the sense of this constraint reversed in my head. This needs to return `Truthiness::AlwaysFalse`, not `Truthiness::AlwaysTrue`! I suspect the mypy-primer run is taking so long because there are now tons more bogus diagnostics 😆 

The fact that no test failed on this change also suggests we need an explicit mdtest like this:

```py
from typing import Never, Any

def f(func: Any) -> Never:
    func()
```

Where an `invalid-return-type` is expected (because the call to `func()` is _not_ terminal). That test fails on this PR currently.
```suggestion
                    return Truthiness::AlwaysFalse.negate_if(!predicate.is_positive);
```

---

_@carljm requested changes on 2025-08-11 16:49_

Perf results look amazing! Hopefully they persist once we get the logic right 😆 (I expect they will)

---

_Comment by @github-actions[bot] on 2025-08-11 18:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-08-11 18:25_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:826 on 2025-08-11 18:25_

Don't worry, it does. The most congested lock is now for `IntersectionType`:

<img width="1015" height="442" alt="image" src="https://github.com/user-attachments/assets/3c67927d-8a5e-42eb-ae7d-43e081c04de7" />

And the wall clock time on the large project on my linux machine goes from 42s .. to 16s which is rather ridiculous 

---

_Review requested from @carljm by @MichaReiser on 2025-08-11 18:28_

---

_Comment by @MichaReiser on 2025-08-11 18:33_

Okay, the single threaded benchmarks don't improve by that much anymore. But it's still a huge improvement for multi threaded workloads (with many cores... not what github actions provides)

---

_@carljm approved on 2025-08-11 19:22_

Awesome!

---

_Merged by @MichaReiser on 2025-08-11 19:58_

---

_Closed by @MichaReiser on 2025-08-11 19:58_

---

_Branch deleted on 2025-08-11 19:58_

---
