```yaml
number: 9316
title: "Use `Display` for formatter parse errors"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - formatter
assignees: []
merged: true
base: main
head: charlie/display
created_at: 2023-12-29T22:22:07Z
updated_at: 2023-12-29T22:38:08Z
url: https://github.com/astral-sh/ruff/pull/9316
synced_at: 2026-01-10T23:07:18Z
```

# Use `Display` for formatter parse errors

---

_Pull request opened by @charliermarsh on 2023-12-29 22:22_

## Summary

This helps a bit with (but does not close) the issues described in https://github.com/astral-sh/ruff/issues/9311. E.g., now, we at least see: `error: Failed to format main.py: source contains syntax errors: invalid syntax. Got unexpected token '=' at byte offset 20`.


---

_Comment by @charliermarsh on 2023-12-29 22:22_

There will be separate changes to add the row and column information.

---

_Label `formatter` added by @charliermarsh on 2023-12-29 22:22_

---

_Merged by @charliermarsh on 2023-12-29 22:26_

---

_Closed by @charliermarsh on 2023-12-29 22:26_

---

_Label `cli` added by @charliermarsh on 2023-12-29 22:26_

---

_Branch deleted on 2023-12-29 22:26_

---

_Comment by @github-actions[bot] on 2023-12-29 22:38_

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
