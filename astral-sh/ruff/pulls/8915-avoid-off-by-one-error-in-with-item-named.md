```yaml
number: 8915
title: Avoid off-by-one error in with-item named expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: charlie/with
created_at: 2023-11-30T00:01:08Z
updated_at: 2023-11-30T00:16:53Z
url: https://github.com/astral-sh/ruff/pull/8915
synced_at: 2026-01-10T23:40:55Z
```

# Avoid off-by-one error in with-item named expressions

---

_Pull request opened by @charliermarsh on 2023-11-30 00:01_

## Summary

Given `with (a := b): pass`, we truncate the `WithItem` range by one on both sides such that the parentheses are part of the statement, rather than the item. However, for `with (a := b) as x: pass`, we want to avoid this trick.

Closes https://github.com/astral-sh/ruff/issues/8913.


---

_Label `bug` added by @charliermarsh on 2023-11-30 00:01_

---

_Label `parser` added by @charliermarsh on 2023-11-30 00:01_

---

_@MichaReiser approved on 2023-11-30 00:08_

---

_Merged by @charliermarsh on 2023-11-30 00:11_

---

_Closed by @charliermarsh on 2023-11-30 00:11_

---

_Branch deleted on 2023-11-30 00:11_

---

_Comment by @github-actions[bot] on 2023-11-30 00:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
