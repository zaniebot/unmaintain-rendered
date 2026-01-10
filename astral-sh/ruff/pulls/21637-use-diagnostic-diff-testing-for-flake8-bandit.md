```yaml
number: 21637
title: "Use `diagnostic_diff` testing for flake8-bandit preview tests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: micha/flake8-bandit-preview-tests
created_at: 2025-11-26T09:09:32Z
updated_at: 2025-11-26T09:13:46Z
url: https://github.com/astral-sh/ruff/pull/21637
synced_at: 2026-01-10T16:48:02Z
```

# Use `diagnostic_diff` testing for flake8-bandit preview tests

---

_Pull request opened by @MichaReiser on 2025-11-26 09:09_

## Summary

Only snapshot diagnostics that are new in preview mode or have been removed in preview mode
for the flake8-bandit tests instead of snapshoting all diagnostics. This makes reviewing changes like
https://github.dev/astral-sh/ruff/pull/21469 easier



---

_Label `testing` added by @MichaReiser on 2025-11-26 09:09_

---

_Merged by @MichaReiser on 2025-11-26 09:13_

---

_Closed by @MichaReiser on 2025-11-26 09:13_

---

_Branch deleted on 2025-11-26 09:13_

---
