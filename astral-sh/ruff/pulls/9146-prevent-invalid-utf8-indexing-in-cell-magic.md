```yaml
number: 9146
title: Prevent invalid utf8 indexing in cell magic detection
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konstin/fix-9145
created_at: 2023-12-15T09:19:24Z
updated_at: 2023-12-15T14:15:48Z
url: https://github.com/astral-sh/ruff/pull/9146
synced_at: 2026-01-10T23:31:11Z
```

# Prevent invalid utf8 indexing in cell magic detection

---

_Pull request opened by @konstin on 2023-12-15 09:19_

The example below used to panic because we tried to split at 2 bytes in the 4-bytes character `转`.
```python
def sample_func(xx):
    """
    转置 (transpose)
    """
    return xx.T
```

Fixes #9145
Fixes https://github.com/astral-sh/ruff-vscode/issues/362

The second commit is a small test refactoring.

---

_Review requested from @dhruvmanila by @konstin on 2023-12-15 09:19_

---

_Comment by @github-actions[bot] on 2023-12-15 09:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2023-12-15 10:48_

---

_@dhruvmanila approved on 2023-12-15 14:13_

Thanks!

---

_Label `bug` added by @dhruvmanila on 2023-12-15 14:14_

---

_Merged by @dhruvmanila on 2023-12-15 14:15_

---

_Closed by @dhruvmanila on 2023-12-15 14:15_

---

_Branch deleted on 2023-12-15 14:15_

---
