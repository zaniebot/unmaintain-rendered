```yaml
number: 18045
title: "[`flake8-simplify`] Fix `SIM905` autofix for `rsplit` creating a reversed list literal"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-SIM905
created_at: 2025-05-12T14:17:15Z
updated_at: 2025-05-12T19:54:23Z
url: https://github.com/astral-sh/ruff/pull/18045
synced_at: 2026-01-12T15:56:11Z
```

# [`flake8-simplify`] Fix `SIM905` autofix for `rsplit` creating a reversed list literal

---

_@LaBatata101_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18042
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Update existing snapshot tests.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-12 14:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM905_SIM905.py.snap`:908 on 2025-05-12 15:08_

This doesn't look right to me:

```pycon
>>> "a,b,c".rsplit(",",1)
['a,b', 'c']
```

---

_@dylwil3 reviewed on 2025-05-12 15:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:194 on 2025-05-12 15:10_

Does it not work to just do `.reverse` after `rsplit` and `rsplitn`?

---

_@dylwil3 requested changes on 2025-05-12 15:11_

Thanks for working on this! I wonder if the fix isn't quite right, and whether the change could be simpler.

---

_Label `bug` added by @dylwil3 on 2025-05-12 15:11_

---

_Label `fixes` added by @dylwil3 on 2025-05-12 15:11_

---

_@LaBatata101 reviewed on 2025-05-12 15:16_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:194 on 2025-05-12 15:16_

I tried using `.rev` but the compiler gave me an error, something about a trait not being implemented for the return type of these functions you mentioned.

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM905_SIM905.py.snap`:908 on 2025-05-12 15:17_

Yeah, you are right that's not correct.

---

_@LaBatata101 reviewed on 2025-05-12 15:17_

---

_@dylwil3 reviewed on 2025-05-12 15:23_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:194 on 2025-05-12 15:23_

Since this isn't a super performance-sensitive spot in the code, you could try collecting first?

---

_Review requested from @dylwil3 by @LaBatata101 on 2025-05-12 19:42_

---

_@dylwil3 approved on 2025-05-12 19:52_

Wonderful, thank you!

---

_Merged by @dylwil3 on 2025-05-12 19:53_

---

_Closed by @dylwil3 on 2025-05-12 19:53_

---

_Branch deleted on 2025-05-12 19:53_

---

_@dylwil3 reviewed on 2025-05-12 19:54_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:194 on 2025-05-12 19:54_

(Actually - we collect _anyway_ and reversing a Vec doesn't re-allocate, so I think it's not even a perf hit really)

---
