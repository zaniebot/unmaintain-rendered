```yaml
number: 15344
title: "Remove unnecessary `PreviewMode::Enabled` in tests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: ruff-0.9
head: PT006-fix-bugfix
created_at: 2025-01-08T10:11:25Z
updated_at: 2025-01-08T10:19:17Z
url: https://github.com/astral-sh/ruff/pull/15344
synced_at: 2026-01-12T15:55:51Z
```

# Remove unnecessary `PreviewMode::Enabled` in tests

---

_@MichaReiser_


## Summary

https://github.com/astral-sh/ruff/pull/15327 also stabilized the fix introduced by https://github.com/astral-sh/ruff/pull/14699

This PR cleans up the test setup to not use preview mode for the now stabilized behavior.

## Test Plan

<!-- How was it tested? -->


---

_Label `testing` added by @MichaReiser on 2025-01-08 10:13_

---

_Comment by @github-actions[bot] on 2025-01-08 10:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2025-01-08 10:19_

---

_Closed by @MichaReiser on 2025-01-08 10:19_

---

_Branch deleted on 2025-01-08 10:19_

---
