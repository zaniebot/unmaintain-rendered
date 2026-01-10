```yaml
number: 14029
title: "[red-knot] use fixpoint iteration for all cycles"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/fixpoint
created_at: 2024-11-01T00:23:35Z
updated_at: 2025-03-12T12:46:55Z
url: https://github.com/astral-sh/ruff/pull/14029
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] use fixpoint iteration for all cycles

---

_Pull request opened by @carljm on 2024-11-01 00:23_

Pulls in the latest Salsa main branch, which supports fixpoint iteration, and uses it to handle all query cycles.

With this, we no longer need to skip any corpus files to avoid panics.

Latest perf results show a 6% incremental and 1% cold-check regression. This is not a "no cycles" regression, as tomllib and typeshed do trigger some definition cycles (previously handled by our old `infer_definition_types` fallback to `Unknown`). We don't currently have a benchmark we can use to measure the pure no-cycles regression, though I expect there would still be some regression; the fixpoint iteration feature in Salsa does add some overhead even for non-cyclic queries.

I think this regression is within the reasonable range for this feature. We can do further optimization work later, but I don't think it's the top priority right now. So going ahead and acknowledging the regression on CodSpeed.

Mypy primer is happy, so this doesn't regress anything on our currently-checked projects. I expect it probably unlocks adding a number of new projects to our ecosystem check that previously would have panicked.

Fixes #13792
Fixes #14672

---

_Label `red-knot` added by @carljm on 2024-11-01 00:23_

---

_Comment by @codspeed-hq[bot] on 2024-11-01 00:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Ffixpoint)

### Merging #14029 will **degrade performances by 5.98%**

<sub>Comparing <code>cjm/fixpoint</code> (328e31e) with <code>main</code> (a6572a5)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.2 ms | -5.98% |


---

_Comment by @github-actions[bot] on 2024-11-01 00:43_

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

_Comment by @MichaReiser on 2024-11-15 06:29_

Nice on bringing down the regression. I'm still somewhat surprised that we see an 11% perf regression and a 4 perf regression in the salsa benchmarks (that don't have any cycles).

---

_Comment by @carljm on 2024-11-15 14:59_

Yeah I planned to do some more looking at profiles; thanks for doing that!

---

_Renamed from "[WIP] initial fixpoint iteration" to "[red-knot] use fixpoint iteration for all cycles" by @carljm on 2025-03-12 05:20_

---

_Comment by @github-actions[bot] on 2025-03-12 05:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @carljm on 2025-03-12 05:35_

---

_Review requested from @MichaReiser by @carljm on 2025-03-12 05:35_

---

_Review requested from @AlexWaygood by @carljm on 2025-03-12 05:35_

---

_Review requested from @sharkdp by @carljm on 2025-03-12 05:35_

---

_Review comment by @MichaReiser on `crates/red_knot_project/tests/check.rs`:282 on 2025-03-12 07:21_

Should we delete this functionality now that all known failures are fixed? Do we forsee situations where it is okay for a PR to introduce a new panic case over fixing the panic?

---

_@MichaReiser approved on 2025-03-12 07:22_

This looks good to me. Do you think it would be useful to have any documentation on when/where to add cycle fallbacks or documentation on the cycle fallback function itself? 

And very impressive that this results in only a 1% perf regression in the cold case. Nice work!

---

_@carljm reviewed on 2025-03-12 12:04_

---

_Review comment by @carljm on `crates/red_knot_project/tests/check.rs`:282 on 2025-03-12 12:04_

It's a good question. I thought we could keep it for now. I _think_ any new cycle cases we run into should be easy to fix by adding cycle handling for a new query, but it's possible we could run into such a case where it's difficult to get the cycle handling correct and it makes more sense to split it into a separate PR, in which scenario we might still use this? I don't feel strongly.

---

_@MichaReiser reviewed on 2025-03-12 12:06_

---

_Review comment by @MichaReiser on `crates/red_knot_project/tests/check.rs`:282 on 2025-03-12 12:06_

That makes sense. Although that also likely means that we would start regressing on projects that Red Knot supported before. So maybe the correct order would be to fix the cycle first, and then land the behavior change?

I don't feel strongly either and am happy to go either way. It just feels nice if regressing panics is *not easy* 

---

_@carljm reviewed on 2025-03-12 12:11_

---

_Review comment by @carljm on `crates/red_knot_project/tests/check.rs`:282 on 2025-03-12 12:11_

> we would start regressing on projects that Red Knot supported before

Maybe; it depends whether the PR is actually regressing on existing files (in this case I would be more hesitant to allow using `KNOWN_FAILURES`), or if it is e.g. an unrelated Ruff linter PR adding a new test file, which reveals an existing-but-not-previously-tested cycle. This latter case is I think the strongest reason to keep `KNOWN_FAILURES` around for now.

---

_Comment by @carljm on 2025-03-12 12:17_

> Do you think it would be useful to have any documentation on when/where to add cycle fallbacks or documentation on the cycle fallback function itself?

Yes, I can add that. I think the latter is probably the most useful and will serve as the former by example, too? Not sure where to put the former that it would be discoverable when needed. Also, Salsa's panic message if you do hit a cycle on a query without cycle handling is pretty informative and would be enough to find existing examples to copy.

---

_Comment by @carljm on 2025-03-12 12:37_

Changed my mind -- too many cases of cycle handling, felt too redundant to repeat the same docs on each. So I added a paragraph to the doc comment at the top of `infer.rs`, where all but one of our queries with cycle handling live.

---

_Merged by @carljm on 2025-03-12 12:41_

---

_Closed by @carljm on 2025-03-12 12:41_

---

_Branch deleted on 2025-03-12 12:41_

---
