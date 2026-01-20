```yaml
number: 22664
title: "[`ruff`] Make fix unsafe if it deletes comments (`RUF020`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
assignees: []
merged: true
base: main
head: RUF020
created_at: 2026-01-17T20:59:42Z
updated_at: 2026-01-20T15:04:15Z
url: https://github.com/astral-sh/ruff/pull/22664
synced_at: 2026-01-20T15:44:04Z
```

# [`ruff`] Make fix unsafe if it deletes comments (`RUF020`)

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-17 21:09_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `fixes` added by @amyreese on 2026-01-19 18:22_

---

_Review requested from @ntBre by @amyreese on 2026-01-19 18:22_

---

_@amyreese approved on 2026-01-19 18:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/never_union.rs`:103 on 2026-01-19 18:53_

Is `expr` the same in each of these cases? I would be tempted to compute the `applicability` once outside of the `match` and then reuse it in each of these places if so.

---

_@ntBre approved on 2026-01-19 18:54_

Thank you! This looks fine to me, just one suggestion about sharing the applicability check.

(I almost wonder if this could warrant a helper method on `Checker` since this is such a common pattern, but I couldn't think of a great name/API for it earlier)

---

_Comment by @amyreese on 2026-01-19 19:41_

> (I almost wonder if this could warrant a helper method on `Checker` since this is such a common pattern, but I couldn't think of a great name/API for it earlier)

I kind of agree after seeing all these PRs. Perhaps something like `checker.contains_comments(expr)` ?


---

_Comment by @ntBre on 2026-01-19 20:34_

Yeah I just couldn't decide how magical it should be. Something like:

```rust
diagnostic.set_applicable_fix(checker.comment_ranges(), edit);
```

could also be nice. I don't think `checker.contains_comments(ranged)` returning a bool is _that_ much of an improvement over the status quo, so I wanted it to return or even set the applicability too.

---

_Review requested from @amyreese by @chirizxc on 2026-01-20 12:10_

---

_Review requested from @ntBre by @chirizxc on 2026-01-20 12:10_

---

_@ntBre approved on 2026-01-20 15:02_

Thank you!

---

_Merged by @ntBre on 2026-01-20 15:02_

---

_Closed by @ntBre on 2026-01-20 15:02_

---

_Branch deleted on 2026-01-20 15:04_

---
