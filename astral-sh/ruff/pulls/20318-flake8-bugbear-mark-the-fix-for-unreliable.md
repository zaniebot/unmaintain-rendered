```yaml
number: 20318
title: "[`flake8-bugbear`] Mark the fix for `unreliable-callable-check` as always unsafe (`B004`)"
type: pull_request
state: merged
author: IDrokin117
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix/ruff-20282
created_at: 2025-09-09T16:30:26Z
updated_at: 2025-09-12T19:35:11Z
url: https://github.com/astral-sh/ruff/pull/20318
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-bugbear`] Mark the fix for `unreliable-callable-check` as always unsafe (`B004`)

---

_Pull request opened by @IDrokin117 on 2025-09-09 16:30_

## Summary
Resolves #20282

Makes the rule fix always unsafe, because the replacement may not be semantically equivalent to the original expression, potentially changing the behavior of the code.

Updated docstring with examples.

## Test Plan
- Added two tests from issue and regenerated the snapshot


---

_Renamed from "[`flake8-bugbear`] `unreliable-callable-check` always marked as unsafe (B004)" to "[`flake8-bugbear`] `unreliable-callable-check` always marked as unsafe (`B004`)" by @IDrokin117 on 2025-09-09 16:30_

---

_Comment by @github-actions[bot] on 2025-09-09 16:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/unreliable_callable_check.rs`:53 on 2025-09-12 19:21_

```suggestion
/// Additionally, if there are comments in the `hasattr` call expression, they may be removed:
```

---

_@ntBre approved on 2025-09-12 19:22_

Thank you!

---

_Label `fixes` added by @ntBre on 2025-09-12 19:23_

---

_Renamed from "[`flake8-bugbear`] `unreliable-callable-check` always marked as unsafe (`B004`)" to "[`flake8-bugbear`] Mark the fix for `unreliable-callable-check` as always unsafe (`B004`)" by @ntBre on 2025-09-12 19:23_

---

_Merged by @ntBre on 2025-09-12 19:27_

---

_Closed by @ntBre on 2025-09-12 19:27_

---
