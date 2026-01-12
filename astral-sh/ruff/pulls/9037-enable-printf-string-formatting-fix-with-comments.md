```yaml
number: 9037
title: "Enable `printf-string-formatting` fix with comments on right-hand side"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/up
created_at: 2023-12-07T03:01:28Z
updated_at: 2023-12-07T03:43:22Z
url: https://github.com/astral-sh/ruff/pull/9037
synced_at: 2026-01-10T23:40:55Z
```

# Enable `printf-string-formatting` fix with comments on right-hand side

---

_Pull request opened by @charliermarsh on 2023-12-07 03:01_

## Summary

This was added in https://github.com/astral-sh/ruff/pull/6364 (as a follow-on to https://github.com/astral-sh/ruff/pull/6342), but I don't think it applies in the same way, because we don't _remove_ the right-hand side when converting from `%`-style formatting to `.format` calls.

Closes https://github.com/astral-sh/ruff/issues/8107.


---

_Label `autofix` added by @charliermarsh on 2023-12-07 03:01_

---

_Comment by @github-actions[bot] on 2023-12-07 03:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @charliermarsh on 2023-12-07 03:43_

---

_Merged by @charliermarsh on 2023-12-07 03:43_

---

_Closed by @charliermarsh on 2023-12-07 03:43_

---

_Branch deleted on 2023-12-07 03:43_

---
