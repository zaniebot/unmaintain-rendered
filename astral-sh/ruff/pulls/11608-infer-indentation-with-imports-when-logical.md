```yaml
number: 11608
title: Infer indentation with imports when logical indent is absent
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/ident
created_at: 2024-05-30T03:45:39Z
updated_at: 2024-05-30T04:18:08Z
url: https://github.com/astral-sh/ruff/pull/11608
synced_at: 2026-01-12T15:55:38Z
```

# Infer indentation with imports when logical indent is absent

---

_@charliermarsh_

## Summary

In an `__init__.py` file, it's not uncommon to lack a logical indent (since it may just contain imports). In such cases, we were always falling back to four-space indent. This PR adds detection for indents within import groups.

Closes https://github.com/astral-sh/ruff/issues/11606.


---

_Label `isort` added by @charliermarsh on 2024-05-30 03:59_

---

_Label `bug` added by @charliermarsh on 2024-05-30 03:59_

---

_@charliermarsh reviewed on 2024-05-30 03:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_codegen/src/stylist.rs`:180 on 2024-05-30 03:59_

Ancient TODO!

---

_Comment by @github-actions[bot] on 2024-05-30 04:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-05-30 04:18_

---

_Closed by @charliermarsh on 2024-05-30 04:18_

---

_Branch deleted on 2024-05-30 04:18_

---
