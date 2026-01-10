```yaml
number: 13825
title: "[red-knot] Improve chained comparisons handling"
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/narrow-chained-comparison
created_at: 2024-10-19T19:19:06Z
updated_at: 2024-10-21T19:38:08Z
url: https://github.com/astral-sh/ruff/pull/13825
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Improve chained comparisons handling

---

_Pull request opened by @TomerBin on 2024-10-19 19:19_


<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

A small fix for comparisons of multiple comparators.
Instead of comparing each comparator to the leftmost item, we should compare it to the closest item on the left.

While implementing this, I noticed that we don’t yet narrow Yoda comparisons (e.g., `True is x`), so I didn’t change that behavior in this PR.

## Test Plan

Added some mdtests 🎉 


---

_Review requested from @carljm by @TomerBin on 2024-10-19 19:19_

---

_Review requested from @MichaReiser by @TomerBin on 2024-10-19 19:19_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-10-19 19:19_

---

_Comment by @codspeed-hq[bot] on 2024-10-19 19:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/TomerBin:tomer/narrow-chained-comparison)

### Merging #13825 will **not alter performance**

<sub>Comparing <code>TomerBin:tomer/narrow-chained-comparison</code> (9462690) with <code>main</code> (d9ef83b)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Converted to draft by @TomerBin on 2024-10-19 19:31_

---

_Comment by @github-actions[bot] on 2024-10-19 19:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @TomerBin on 2024-10-19 22:24_

Rebased above #13826 to make the build pass on MSRV, so this PR will be ready to review once it is merged.  

---

_Marked ready for review by @TomerBin on 2024-10-20 10:22_

---

_Label `red-knot` added by @MichaReiser on 2024-10-21 08:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:40 on 2024-10-21 19:28_

Technically, given any single transitive relation (and `is` is transitive; `is not`, of course, is not 😆 ) used across all comparisons, any narrowing that applies in one comparison can be applied across all of them; it would be safe to infer `Literal[False]` for `y` here also.

A more general framing would be to say that it's valid to use the type of a name as narrowed in one chained comparison when evaluating another chained comparison involving the same name, so in this case it would be valid to use the type `Literal[False]` for `x` when evaluating narrowing for `y is x`, giving us `Literal[False]` for `y` also. The tricky thing about actually implementing this would be deciding in which order to evaluate the narrowings.

All that said: I don't think it's important that we _do_ this. Perhaps not ever, and certainly not in this PR.

I do think it's important that you also added the "is not" test below, since that one was more clearly wrong with the prior behavior.

---

_@carljm approved on 2024-10-21 19:36_

Thank you!! Great catch, and excellent PR to fix it.

---

_Merged by @carljm on 2024-10-21 19:38_

---

_Closed by @carljm on 2024-10-21 19:38_

---
