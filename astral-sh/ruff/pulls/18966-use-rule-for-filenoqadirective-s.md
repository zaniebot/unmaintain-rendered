```yaml
number: 18966
title: "Use `Rule` for `FileNoqaDirective`s"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: brent/string-noqa-code
head: brent/directive-rule
created_at: 2025-06-26T17:04:45Z
updated_at: 2025-06-27T12:45:28Z
url: https://github.com/astral-sh/ruff/pull/18966
synced_at: 2026-01-12T15:56:28Z
```

# Use `Rule` for `FileNoqaDirective`s

---

_@ntBre_

Summary
--

See https://github.com/astral-sh/ruff/pull/18946#discussion_r2168459251.

Test Plan
--

Existing tests

---

_Label `internal` added by @ntBre on 2025-06-26 17:05_

---

_Label `diagnostics` added by @ntBre on 2025-06-26 17:05_

---

_Comment by @codspeed-hq[bot] on 2025-06-26 17:16_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fdirective-rule?runnerMode=Instrumentation)

### Merging #18966 will **not alter performance**

<sub>Comparing <code>brent/directive-rule</code> (2bbc37e) with <code>brent/string-noqa-code</code> (4eab76b)</sub>



### Summary

`✅ 37` untouched benchmarks  
`⁉️ 1` dropped benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fdirective-rule?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `` ty_micro[many_attribute_assignments] `` | 56.6 ms | N/A | N/A |


---

_Comment by @github-actions[bot] on 2025-06-26 17:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-06-26 17:17_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-26 17:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:54 on 2025-06-26 19:21_

Do we have a short-circuit path somewhere that skips iterating over all diagnostics if there are no `noqa` codes at all?


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:63 on 2025-06-26 19:25_

Should we add (or use) a method `includes_code` that accepts a secondary code instead of the `rule`. That would allow us to defer the rule parsing up to the point where we've found a suppression comment (which should be the exception).

To avoid calling `Noqa::to_string`, we can add a method `Noqa::matches_code(code: &str)` which can match the string without allocationg

---

_@MichaReiser reviewed on 2025-06-26 19:27_

I've a small perf recommendation to reduce the number of `Rule::parse` call

---

_@ntBre reviewed on 2025-06-26 19:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:63 on 2025-06-26 19:46_

Ah yeah, I already added `FileExemption::contains_secondary_code` but I can call it before this to bail out earlier without parsing the rule code.

---

_@ntBre reviewed on 2025-06-26 19:58_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:54 on 2025-06-26 19:58_

I don't think so, but that's a good idea. I added

```rust
    if file_noqa_directives.is_empty() && noqa_directives.is_empty() {
        return Vec::new();
    }
```

to the top of this function, along with those two `is_empty` methods.

---

_@MichaReiser approved on 2025-06-26 20:10_

---

_Merged by @ntBre on 2025-06-26 21:01_

---

_Closed by @ntBre on 2025-06-26 21:01_

---

_Branch deleted on 2025-06-26 21:01_

---

_Comment by @MichaReiser on 2025-06-27 06:24_

Hmm, I think I would have merged this into main or waited with merging to make it easier to review https://github.com/astral-sh/ruff/pull/18946 It's no big deal. It's just a bit confusing and distracting because I now need to decide for each change if I already reviewed it or not.

The other advantage of merging to main is that it would give us better insight in how https://github.com/astral-sh/ruff/pull/18946 changes performance. Now it's all muddled up with this change.

---

_Comment by @ntBre on 2025-06-27 12:45_

Oh, sorry about that! I wasn't thinking about the review or checking the benchmarks again, but it sounds obvious in hindsight.

---
