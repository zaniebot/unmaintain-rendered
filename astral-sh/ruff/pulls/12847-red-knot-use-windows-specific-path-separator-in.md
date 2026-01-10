```yaml
number: 12847
title: "[red-knot] Use Windows specific path separator in tests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/windows
created_at: 2024-08-12T16:45:27Z
updated_at: 2024-08-12T16:59:05Z
url: https://github.com/astral-sh/ruff/pull/12847
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Use Windows specific path separator in tests

---

_Pull request opened by @dhruvmanila on 2024-08-12 16:45_

## Summary

fix-up for https://github.com/astral-sh/ruff/pull/12842 to use Windows specific path separator for test output.

---

_Label `red-knot` added by @dhruvmanila on 2024-08-12 16:45_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-12 16:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-12 16:45_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-12 16:45_

---

_@AlexWaygood approved on 2024-08-12 16:46_

---

_@carljm approved on 2024-08-12 16:47_

Longer term we'll probably want a test helper to handle these assertions, but that can wait until after the switch over to type checker; this looks good to fix main for now. Thank you!

---

_Comment by @dhruvmanila on 2024-08-12 16:48_

Yeah, I agree. This is just one test case so thought to use a simple solution.

---

_Merged by @dhruvmanila on 2024-08-12 16:56_

---

_Closed by @dhruvmanila on 2024-08-12 16:56_

---

_Branch deleted on 2024-08-12 16:57_

---

_Comment by @github-actions[bot] on 2024-08-12 16:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
