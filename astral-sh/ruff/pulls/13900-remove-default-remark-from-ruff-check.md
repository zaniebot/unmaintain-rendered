```yaml
number: 13900
title: "Remove \"default\" remark from `ruff check`"
type: pull_request
state: merged
author: mihaic
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-default
created_at: 2024-10-24T01:05:11Z
updated_at: 2024-10-24T01:25:37Z
url: https://github.com/astral-sh/ruff/pull/13900
synced_at: 2026-01-12T15:55:46Z
```

# Remove "default" remark from `ruff check`

---

_@mihaic_

## Summary

`ruff check` has not been the default in a long time. However, the help message and code comment still designate it as the default. The remark should have been removed in the deprecation PR #10169.

## Test Plan

Not tested.

---

_@charliermarsh approved on 2024-10-24 01:13_

Good call, thank you.

---

_Label `documentation` added by @charliermarsh on 2024-10-24 01:13_

---

_Merged by @charliermarsh on 2024-10-24 01:17_

---

_Closed by @charliermarsh on 2024-10-24 01:17_

---

_Branch deleted on 2024-10-24 01:21_

---

_Comment by @github-actions[bot] on 2024-10-24 01:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
