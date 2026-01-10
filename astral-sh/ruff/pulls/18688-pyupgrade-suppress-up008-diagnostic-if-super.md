```yaml
number: 18688
title: "[`pyupgrade`] Suppress `UP008` diagnostic if `super` symbol is not builtin"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-UP008
created_at: 2025-06-15T21:01:44Z
updated_at: 2025-06-16T19:09:31Z
url: https://github.com/astral-sh/ruff/pull/18688
synced_at: 2026-01-10T18:45:04Z
```

# [`pyupgrade`] Suppress `UP008` diagnostic if `super` symbol is not builtin

---

_Pull request opened by @LaBatata101 on 2025-06-15 21:01_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18684
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-15 21:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@robsdedude reviewed on 2025-06-15 21:38_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:177 on 2025-06-15 21:38_

Having `is_super_call_with_arguments` simply return the following should be equivalent and shorter
```rust
checker.semantic().match_builtin_expr(&call.func, "super") && !call.arguments.args.is_empty()
```

---

_@LaBatata101 reviewed on 2025-06-15 21:41_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:177 on 2025-06-15 21:41_

You're right, thanks!

---

_Label `bug` added by @ntBre on 2025-06-16 19:06_

---

_Label `fixes` added by @ntBre on 2025-06-16 19:06_

---

_@ntBre approved on 2025-06-16 19:09_

Excellent, thanks!

---

_Merged by @ntBre on 2025-06-16 19:09_

---

_Closed by @ntBre on 2025-06-16 19:09_

---
