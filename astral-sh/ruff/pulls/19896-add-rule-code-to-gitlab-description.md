```yaml
number: 19896
title: Add rule code to GitLab description
type: pull_request
state: merged
author: ntBre
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: brent/gitlab-description
created_at: 2025-08-13T14:28:41Z
updated_at: 2025-08-13T15:19:28Z
url: https://github.com/astral-sh/ruff/pull/19896
synced_at: 2026-01-10T17:52:17Z
```

# Add rule code to GitLab description

---

_Pull request opened by @ntBre on 2025-08-13 14:28_

## Summary

Fixes #19881. While I was here, I also made a couple of related tweaks to the output format. First, we don't need to strip the `SyntaxError: ` prefix anymore since that's not added directly to the diagnostic message after #19644. Second, we can use `secondary_code_or_id` to fall back on the lint ID for syntax errors, which changes the `check_name` from `syntax-error` to `invalid-syntax`. And then the main change requested in the issue, prepending the `check_name` to the description.

## Test Plan

Existing tests and a new screenshot from GitLab:

<img width="362" height="113" alt="image" src="https://github.com/user-attachments/assets/97654ad4-a639-4489-8c90-8661c7355097" />


---

_Label `diagnostics` added by @ntBre on 2025-08-13 14:28_

---

_Comment by @github-actions[bot] on 2025-08-13 14:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-08-13 15:10_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-13 15:10_

---

_@MichaReiser approved on 2025-08-13 15:13_

Nice, thank you

---

_Merged by @ntBre on 2025-08-13 15:19_

---

_Closed by @ntBre on 2025-08-13 15:19_

---

_Branch deleted on 2025-08-13 15:19_

---
