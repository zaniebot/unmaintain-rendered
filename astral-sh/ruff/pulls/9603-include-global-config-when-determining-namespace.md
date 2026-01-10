```yaml
number: 9603
title: "Include global `--config` when determining namespace packages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/n
created_at: 2024-01-22T00:00:54Z
updated_at: 2024-01-22T00:14:09Z
url: https://github.com/astral-sh/ruff/pull/9603
synced_at: 2026-01-10T22:57:09Z
```

# Include global `--config` when determining namespace packages

---

_Pull request opened by @charliermarsh on 2024-01-22 00:00_

## Summary

When determining whether _any_ settings have namespace packages, we need to consider the global settings (as would be provided via `--config`). This was a subtle fallout of a refactor.

Closes https://github.com/astral-sh/ruff/issues/9579.

## Test Plan

Tested locally by compiling Ruff and running against this [namespace-test](https://github.com/gokay05/namespace-test) repo.

---

_Label `bug` added by @charliermarsh on 2024-01-22 00:00_

---

_Merged by @charliermarsh on 2024-01-22 00:10_

---

_Closed by @charliermarsh on 2024-01-22 00:10_

---

_Branch deleted on 2024-01-22 00:10_

---

_Comment by @github-actions[bot] on 2024-01-22 00:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
