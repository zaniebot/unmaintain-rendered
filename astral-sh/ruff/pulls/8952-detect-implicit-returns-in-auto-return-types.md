```yaml
number: 8952
title: Detect implicit returns in auto-return-types
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/autotyping
created_at: 2023-12-01T17:16:39Z
updated_at: 2023-12-01T17:35:03Z
url: https://github.com/astral-sh/ruff/pull/8952
synced_at: 2026-01-10T23:40:55Z
```

# Detect implicit returns in auto-return-types

---

_Pull request opened by @charliermarsh on 2023-12-01 17:16_

## Summary

Adds detection for branches without a `return` or `raise`, so that we can properly `Optional` the return types. I'd like to remove this and replace it with our code graph analysis from the `unreachable.rs` rule, but it at least fixes the worst offenders.

Closes #8942.


---

_Label `bug` added by @charliermarsh on 2023-12-01 17:16_

---

_Comment by @github-actions[bot] on 2023-12-01 17:32_

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

_Marked ready for review by @charliermarsh on 2023-12-01 17:34_

---

_Merged by @charliermarsh on 2023-12-01 17:35_

---

_Closed by @charliermarsh on 2023-12-01 17:35_

---

_Branch deleted on 2023-12-01 17:35_

---
