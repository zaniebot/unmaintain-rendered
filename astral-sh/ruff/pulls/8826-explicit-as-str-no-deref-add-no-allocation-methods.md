```yaml
number: 8826
title: "Explicit `as_str` (no deref), add no allocation methods"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/custom-methods
created_at: 2023-11-23T14:47:36Z
updated_at: 2023-12-05T20:33:48Z
url: https://github.com/astral-sh/ruff/pull/8826
synced_at: 2026-01-10T23:40:55Z
```

# Explicit `as_str` (no deref), add no allocation methods

---

_Pull request opened by @dhruvmanila on 2023-11-23 14:47_

## Summary

This PR is a follow-up to the AST refactor which does the following:
- Remove `Deref` implementation on `StringLiteralValue` and use explicit `as_str` calls instead. The `Deref` implementation would implicitly perform allocations in case of implicitly concatenated strings. This is to make sure the allocation is explicit.
- Now, certain methods can be implemented to do zero allocations which have been implemented in this PR. They are:
    - `is_empty`
    - `len`
    - `chars`
    - Custom `PartialEq` implementation to compare each character

## Test Plan

Run the linter test suite and make sure all tests pass.


---

_Comment by @dhruvmanila on 2023-11-23 14:47_

Current dependencies on/for this PR:
* main
  * **PR #8062** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8062?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8063** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8063?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8064** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8064?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7927** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7927?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #8826** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8826?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
            * **PR #8828** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8828?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #8835** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8835?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #9016** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9016?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8826?utm_source=stack-comment).

---

_Label `internal` added by @dhruvmanila on 2023-11-23 14:56_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-11-23 14:56_

---

_Comment by @github-actions[bot] on 2023-11-23 15:07_

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

_@charliermarsh approved on 2023-11-24 15:34_

---

_Merged by @dhruvmanila on 2023-11-25 00:03_

---

_Closed by @dhruvmanila on 2023-11-25 00:03_

---

_Branch deleted on 2023-11-25 00:04_

---

_Comment by @MichaReiser on 2023-11-28 02:35_

What has been the main motivation for the change (just asking)? I assume it's for better performance but codespeed isn't loading for me right now.

Could we keep the `Deref` implementation in addition to `as_str` for case where the `Deref` lifetimes work? I'm asking because some of the code seems more complicated now, then when using `Deref`.

---

_Comment by @dhruvmanila on 2023-11-28 14:48_

> What has been the main motivation for the change (just asking)? I assume it's for better performance but codespeed isn't loading for me right now.

These methods are just some low hanging fruits which are simple to implement to work on each character instead of (probably) allocating a String. That said, I haven't seen any regression or improvements.

> Could we keep the `Deref` implementation in addition to `as_str` for case where the `Deref` lifetimes work? I'm asking because some of the code seems more complicated now, then when using `Deref`.

Can you provide an example where you think the code is complicated? That would help me understand your perspective.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:385 on 2023-11-28 23:53_

I think the previous code was easier to read than now where we have the explicit `as_str` call and using `Deref` seemed to work just fine?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:802 on 2023-11-28 23:54_

Same for all these changes. If we could keep the `Deref` implementation in addition to the explicit `as_str` method, then code that doesn't need the more relaxed lifetime could be left as is.

---

_@MichaReiser reviewed on 2023-11-28 23:54_

---

_@charliermarsh reviewed on 2023-11-28 23:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:385 on 2023-11-28 23:59_

The motivation here is that the `Deref` implementation includes an allocation -- the conversion here can allocate if the string is implicitly concatenated. So `Deref` is essentially hiding an allocation. I prefer that we do something explicit over an implicit allocation. It's not about the lifetimes or anything like that. It's intentionally more verbose.

---

_@MichaReiser reviewed on 2023-11-29 00:12_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:385 on 2023-11-29 00:12_

I think we should then call the method `to_str` rather than `as_str` according to https://rust-lang.github.io/api-guidelines/naming.html#ad-hoc-conversions-follow-as_-to_-into_-conventions-c-conv Because I assume that a function called `as` to be free (or extremely cheap), whereas `to` might be cheap, but it depends, and `into_` consumes `self`

---

_@dhruvmanila reviewed on 2023-11-29 00:28_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:385 on 2023-11-29 00:28_

Yeah, I think using `to_str` makes more sense. I was not aware of that. I'll create a follow-up PR to rename that.

---
