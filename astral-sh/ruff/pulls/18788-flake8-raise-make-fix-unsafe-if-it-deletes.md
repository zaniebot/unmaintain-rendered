```yaml
number: 18788
title: "[`flake8-raise`] Make fix unsafe if it deletes comments (`RSE102`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/rse102
created_at: 2025-06-19T09:17:56Z
updated_at: 2025-06-21T17:11:11Z
url: https://github.com/astral-sh/ruff/pull/18788
synced_at: 2026-01-12T15:56:25Z
```

# [`flake8-raise`] Make fix unsafe if it deletes comments (`RSE102`)

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
/closes #18773
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-19 09:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-06-19 09:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs`:133 on 2025-06-19 09:37_

The fix needs to remain `Unsafe` if `exception_type` is some (which you can see in the changed tests and the many ecosystem changes)

---

_Converted to draft by @chirizxc on 2025-06-19 10:19_

---

_Marked ready for review by @chirizxc on 2025-06-19 20:41_

---

_Comment by @chirizxc on 2025-06-19 20:49_

oh, i deleted `Fix safety`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs`:150 on 2025-06-20 06:18_

Can you tell me why it isn't required to check if the fix would delete comments in the `if` branch? If that's indeed the case, I suggest moving the `applicability` override from line 124 into the else branch here.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs`:40 on 2025-06-20 06:18_

It would be great if we could document the unsafety when `exception_type.is_some` too

---

_@MichaReiser reviewed on 2025-06-20 06:18_

---

_Label `fixes` added by @MichaReiser on 2025-06-21 17:09_

---

_@MichaReiser approved on 2025-06-21 17:09_

Thank you

---

_Merged by @MichaReiser on 2025-06-21 17:09_

---

_Closed by @MichaReiser on 2025-06-21 17:09_

---

_Label `bug` added by @MichaReiser on 2025-06-21 17:09_

---

_Branch deleted on 2025-06-21 17:11_

---
