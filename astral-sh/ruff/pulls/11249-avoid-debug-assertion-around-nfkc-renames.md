```yaml
number: 11249
title: Avoid debug assertion around NFKC renames
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/n
created_at: 2024-05-02T17:28:33Z
updated_at: 2024-05-02T18:00:15Z
url: https://github.com/astral-sh/ruff/pull/11249
synced_at: 2026-01-10T22:37:02Z
```

# Avoid debug assertion around NFKC renames

---

_Pull request opened by @charliermarsh on 2024-05-02 17:28_

## Summary

This assertion isn't quite correct, since with NFKC normalization, two identifiers can have different lengths but map to the same binding.

Closes https://github.com/astral-sh/ruff/issues/11238.

Closes https://github.com/astral-sh/ruff/issues/11239.


---

_Comment by @github-actions[bot] on 2024-05-02 17:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-05-02 17:59_

---

_Closed by @charliermarsh on 2024-05-02 17:59_

---

_Branch deleted on 2024-05-02 17:59_

---

_Label `bug` added by @charliermarsh on 2024-05-02 18:00_

---
