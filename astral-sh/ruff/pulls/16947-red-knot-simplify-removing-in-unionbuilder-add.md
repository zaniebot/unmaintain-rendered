```yaml
number: 16947
title: "[red-knot] simplify \"removing\" in UnionBuilder::add"
type: pull_request
state: merged
author: alex-700
labels:
  - ty
assignees: []
merged: true
base: main
head: latyshev/builder-simplify-remove
created_at: 2025-03-24T11:33:09Z
updated_at: 2025-06-10T20:46:18Z
url: https://github.com/astral-sh/ruff/pull/16947
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] simplify "removing" in UnionBuilder::add

---

_@alex-700_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Simplify "removing" in UnionBuilder::add
It's now O(m) instead of O(n + m) and easier to read.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
cargo test (incl. mdtest)

---

_Review requested from @carljm by @alex-700 on 2025-03-24 11:33_

---

_Review requested from @AlexWaygood by @alex-700 on 2025-03-24 11:33_

---

_Review requested from @sharkdp by @alex-700 on 2025-03-24 11:33_

---

_Review requested from @dcreager by @alex-700 on 2025-03-24 11:33_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-24 11:36_

---

_Comment by @github-actions[bot] on 2025-03-24 11:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-03-24 15:55_

Thanks for the PR! Codspeed is [neutral](https://codspeed.io/astral-sh/ruff/branches/alex-700%3Alatyshev%2Fbuilder-simplify-remove) on this PR, but I'm hesitant to make lots of changes to this code motivated by performance until we have better benchmarks for code that involves lots of large unions. Certain third-party libraries often do create large unions, which has created lots of pathological performance problems for other type checkers (I wroteup a list of some examples in https://github.com/astral-sh/ty/issues/240). I sort of think we should prioritise adding better benchmarks for code with big unions before making too many changes like this

---

_@MichaReiser reviewed on 2025-03-24 16:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:114 on 2025-03-24 16:18_

I do like the improvement overall as it simplifies the code quiet a bit and it also reduces runtime complexity

I agree that the existing code is somewhat hard to reason about. 

I'm getting the impression that this new code changes behavior because it now always swaps the first element to remove with the added element where, I think, previously, the `to_add` was always appended at the end.

A version of this that's closer to the original is:

```rust
match to_remove[..] {
    [] => self.elements.push(to_add),
    [index] => self.elements[index] = to_add,
    _ => {
        // We iterate in descending order to keep remaining indices valid after `swap_remove`.
        for index in to_remove.into_iter().rev() {
            self.elements.swap_remove(index);
        }

        self.elements.push(to_add);
    }
}
```

---

_Comment by @alex-700 on 2025-03-24 17:18_

@AlexWaygood thanks for the quick response!
This PR was motiviert mostly by simplicity and consistency (the pattern is already used [here](https://github.com/astral-sh/ruff/blob/67cb2e8dbe17da5d3b851af02e567ca292ba53cb/crates/red_knot_python_semantic/src/types/builder.rs#L377-L379), [here](https://github.com/astral-sh/ruff/blob/67cb2e8dbe17da5d3b851af02e567ca292ba53cb/crates/red_knot_python_semantic/src/types/builder.rs#L394-L396), and [here](https://github.com/astral-sh/ruff/blob/67cb2e8dbe17da5d3b851af02e567ca292ba53cb/crates/red_knot_python_semantic/src/types/builder.rs#L466-L468)).

I completely agree with you that performance-wise changes should be measured through better benchmark.

Thoughts about performance:
- it is unlikely that the new code will be worse, than the current one. 
- before the code [from PR] we will spend the time in the [loop](https://github.com/astral-sh/ruff/blob/67cb2e8dbe17da5d3b851af02e567ca292ba53cb/crates/red_knot_python_semantic/src/types/builder.rs#L84). So:
- It is non-trivial to challenge one implementation or another for perf.


---

_@alex-700 reviewed on 2025-03-24 17:25_

---

_Review comment by @alex-700 on `crates/red_knot_python_semantic/src/types/builder.rs`:114 on 2025-03-24 17:25_

> I'm getting the impression that this new code changes behavior because it now always swaps the first element to remove with the added element where, I think, previously, the to_add was always appended at the end.

Yes. It does. 

Also it now changes the order of `elements` by multiple `swap_remove`-s (before it was the order-preserving algorithm of removing). 

But AFAIU there are no order-related requirements for this code (e.g. it could `self.elements[index] = to_add` to the middle). It's not important to preserve previous behavior, so I chose the current version in favor of simplicity.

---

_@AlexWaygood reviewed on 2025-03-24 17:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:114 on 2025-03-24 17:43_

In general we _prefer_ not to change the order of union elements if it's possible _and_ it's cheap to avoid doing so. However, if it leads to significantly worse performance to retain the order of union elements, we're okay with mutating the order (that's the reason why we already use `swap_remove` in several places).

---

_@alex-700 reviewed on 2025-03-24 17:54_

---

_Review comment by @alex-700 on `crates/red_knot_python_semantic/src/types/builder.rs`:114 on 2025-03-24 17:54_

Ok. In this case it is strange to have the optimization (?) `self.elements[index] = to_add` in one instance, and do not have it in another. 

The option, where we append `to_add` always to the end:
```rust
// We iterate in descending order to keep remaining indices valid after `swap_remove`.
for index in to_remove.into_iter().rev() {
    self.elements.swap_remove(index);
}
self.elements.push(to_add);
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:114 on 2025-03-24 18:01_

I think we want to generally preserve the order of user-provided union annotations, but once we are modifying a union, I think it's a pretty weak preference and easily overridden by simplicity or performance.

I also note that no existing test requires changing here, suggesting that the difference in behavior is minimal in practice and doesn't seem to affect common scenarios. And as @alex-700 points out, the new simpler implementation is arguably more consistent in how it handles the ordering.

I would be inclined to accept this change as-is.

---

_@carljm approved on 2025-03-24 18:02_

Thanks for the PR!

---

_Merged by @carljm on 2025-03-24 18:04_

---

_Closed by @carljm on 2025-03-24 18:04_

---

_Branch deleted on 2025-06-10 20:46_

---
