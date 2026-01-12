```yaml
number: 9376
title: "Avoid infinite loop in constant vs. `None` comparisons"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/eq
created_at: 2024-01-03T02:52:17Z
updated_at: 2024-01-03T03:05:12Z
url: https://github.com/astral-sh/ruff/pull/9376
synced_at: 2026-01-12T15:55:28Z
```

# Avoid infinite loop in constant vs. `None` comparisons

---

_@charliermarsh_

## Summary

We had an early `continue` in this loop, and we weren't setting `comparator = next;` when continuing... This PR removes the early continue altogether for clarity.

Closes https://github.com/astral-sh/ruff/issues/9374.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-01-03 02:52_

---

_Label `fuzzer` added by @charliermarsh on 2024-01-03 02:52_

---

_Marked ready for review by @charliermarsh on 2024-01-03 02:52_

---

_Merged by @charliermarsh on 2024-01-03 03:04_

---

_Closed by @charliermarsh on 2024-01-03 03:04_

---

_Branch deleted on 2024-01-03 03:04_

---

_Comment by @github-actions[bot] on 2024-01-03 03:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
