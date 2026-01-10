```yaml
number: 11601
title: "docs: Minor formatting typo in F401 example."
type: pull_request
state: merged
author: vitaliyf
labels:
  - internal
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-05-29T14:58:25Z
updated_at: 2024-05-29T15:22:11Z
url: https://github.com/astral-sh/ruff/pull/11601
synced_at: 2026-01-10T21:56:00Z
```

# docs: Minor formatting typo in F401 example.

---

_Pull request opened by @vitaliyf on 2024-05-29 14:58_

## Summary

Removed stray space in sample code snippet that is against ruff's own default formatting rules.

This documentation appears on https://docs.astral.sh/ruff/rules/unused-import/

## Test Plan

This is a trivially obvious change, verifiable with `ruff format --check`

---

_Comment by @charliermarsh on 2024-05-29 15:06_

I think you need to change this in the test file itself, not just the snapshot.

---

_@charliermarsh approved on 2024-05-29 15:14_

Thanks!

---

_Label `internal` added by @charliermarsh on 2024-05-29 15:14_

---

_Merged by @charliermarsh on 2024-05-29 15:14_

---

_Closed by @charliermarsh on 2024-05-29 15:14_

---

_Branch deleted on 2024-05-29 15:15_

---

_Comment by @github-actions[bot] on 2024-05-29 15:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
