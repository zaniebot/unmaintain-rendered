```yaml
number: 18930
title: "[`pydocstyle`] Fix D413 infinite loop for parenthesized docstring"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - docstring
  - fixes
assignees: []
merged: true
base: main
head: fix-18908
created_at: 2025-06-25T02:33:36Z
updated_at: 2025-06-30T14:56:27Z
url: https://github.com/astral-sh/ruff/pull/18930
synced_at: 2026-01-10T18:39:09Z
```

# [`pydocstyle`] Fix D413 infinite loop for parenthesized docstring

---

_Pull request opened by @danparizher on 2025-06-25 02:33_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #18908



---

_Comment by @github-actions[bot] on 2025-06-25 02:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-06-25 17:56_

---

_Label `docstring` added by @ntBre on 2025-06-25 17:56_

---

_Label `fixes` added by @ntBre on 2025-06-25 17:56_

---

_Comment by @ntBre on 2025-06-25 17:57_

Could you add a test showing that the issue is fixed?

---

_Comment by @danparizher on 2025-06-25 20:09_

> Could you add a test showing that the issue is fixed?

Added the case from the original issue



---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1609 on 2025-06-26 18:20_

I don't think this one has anything to do with D413. If this fixes a problem with another rule, we should either handle that separately, or at least add a test demonstrating the change.

---

_@ntBre reviewed on 2025-06-26 18:20_

Thanks, this looks good. Just one question about the change on line 1609.

---

_@danparizher reviewed on 2025-06-27 00:03_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1609 on 2025-06-27 00:03_

That was an oversight on my part, thanks for pointing it out. Reverted the change.

---

_@ntBre reviewed on 2025-06-30 14:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1609 on 2025-06-30 14:48_

Thank you!

---

_@ntBre approved on 2025-06-30 14:49_

---

_Merged by @ntBre on 2025-06-30 14:49_

---

_Closed by @ntBre on 2025-06-30 14:49_

---

_Branch deleted on 2025-06-30 14:56_

---
