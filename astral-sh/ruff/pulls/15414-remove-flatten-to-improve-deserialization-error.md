```yaml
number: 15414
title: "Remove `flatten` to improve deserialization error messages"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - configuration
assignees: []
merged: true
base: main
head: dhruv/flatten-options
created_at: 2025-01-11T06:05:48Z
updated_at: 2025-01-11T16:38:23Z
url: https://github.com/astral-sh/ruff/pull/15414
synced_at: 2026-01-10T20:34:00Z
```

# Remove `flatten` to improve deserialization error messages

---

_Pull request opened by @dhruvmanila on 2025-01-11 06:05_

## Summary

Closes: #9719  

## Test Plan

**Before:**

```
ruff failed
  Cause: Failed to parse /Users/dhruv/playground/ruff/pyproject.toml
  Cause: TOML parse error at line 22, column 1
   |
22 | [tool.ruff.lint]
   | ^^^^^^^^^^^^^^^^
invalid type: string "false", expected a boolean
```

**After:**

```
ruff failed
  Cause: Failed to parse /Users/dhruv/playground/ruff/pyproject.toml
  Cause: TOML parse error at line 27, column 20
   |
27 | mypy-init-return = "false"
   |                    ^^^^^^^
invalid type: string "false", expected a boolean
```

---

_Label `configuration` added by @dhruvmanila on 2025-01-11 06:27_

---

_Comment by @github-actions[bot] on 2025-01-11 06:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2025-01-11 08:50_

---

_@charliermarsh approved on 2025-01-11 13:33_

Great, thanks!

---

_Marked ready for review by @dhruvmanila on 2025-01-11 16:37_

---

_Merged by @dhruvmanila on 2025-01-11 16:38_

---

_Closed by @dhruvmanila on 2025-01-11 16:38_

---

_Branch deleted on 2025-01-11 16:38_

---
