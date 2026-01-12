```yaml
number: 9996
title: Fix SIM113 false positive with async for loops
type: pull_request
state: merged
author: adrienball
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-15T10:48:18Z
updated_at: 2024-02-16T03:40:01Z
url: https://github.com/astral-sh/ruff/pull/9996
synced_at: 2026-01-12T15:55:30Z
```

# Fix SIM113 false positive with async for loops

---

_@adrienball_

## Summary
Ignore `async for` loops when checking the SIM113 rule.

Closes #9995 

## Test Plan
A new test case was added to SIM113.py with an async for loop.


---

_Comment by @github-actions[bot] on 2024-02-15 11:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-02-16 03:39_

---

_Comment by @charliermarsh on 2024-02-16 03:39_

Thank you! Much appreciated.

---

_Label `bug` added by @charliermarsh on 2024-02-16 03:39_

---

_Merged by @charliermarsh on 2024-02-16 03:40_

---

_Closed by @charliermarsh on 2024-02-16 03:40_

---
