```yaml
number: 18035
title: "[`refurb`] Add coverage of `set` and `frozenset` calls (`FURB171`)"
type: pull_request
state: merged
author: naslundx
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: single-item-membership-test-missing-set-constructor
created_at: 2025-05-12T09:39:49Z
updated_at: 2025-05-29T18:59:49Z
url: https://github.com/astral-sh/ruff/pull/18035
synced_at: 2026-01-10T18:45:04Z
```

# [`refurb`] Add coverage of `set` and `frozenset` calls (`FURB171`)

---

_Pull request opened by @naslundx on 2025-05-12 09:39_

…ItemMembershipTest

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds coverage of using set(...) in addition to `{...} in SingleItemMembershipTest.

Fixes #15792
(and replaces the old PR #15793)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Updated unit test and snapshot.

Steps to reproduce are in the issue linked above.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-12 09:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-05-13 01:39_

---

_Label `rule` added by @ntBre on 2025-05-14 18:49_

---

_Label `preview` added by @ntBre on 2025-05-14 18:49_

---

_Renamed from "(FURB171) Add coverage of using set(...) and frozenset(...) in Single…" to "[`refurb`] Add coverage of `set` and `frozenset` calls (`FURB171`)" by @ntBre on 2025-05-14 18:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:141 on 2025-05-14 19:11_

```suggestion
           arguments.find_positional(0).and_then(|arg| single_item(arg, semantic))
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:84 on 2025-05-14 19:21_

Nit: I'd just leave it like this to minimize the diff. `semantic` isn't used anywhere else anyway.

```suggestion
    // Check if the right-hand side is a single-item object.
     let Some(item) = single_item(right, checker.semantic()) else {
```

---

_@ntBre reviewed on 2025-05-14 19:26_

Thanks! The implementation looks reasonable to me. Would you mind moving the new test cases to the end of the test file? That will make it a lot easier to review that part.

Alternatively you could move `FURB171.py` to `FURB171_0.py` and add your new tests in `FURB171_1.py`.

cc @dylwil3 and @AlexWaygood since y'all reviewed the previous iteration of this PR (https://github.com/astral-sh/ruff/pull/15793).

---

_Comment by @naslundx on 2025-05-16 12:36_

Fixed the suggested tweaks, will reorg the test cases as well

---

_Comment by @naslundx on 2025-05-23 08:27_

There we go. Back to you @ntBre !

---

_@ntBre approved on 2025-05-29 18:58_

Looks great, thank you!

---

_Merged by @ntBre on 2025-05-29 18:59_

---

_Closed by @ntBre on 2025-05-29 18:59_

---
