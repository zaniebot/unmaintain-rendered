```yaml
number: 11251
title: Avoid allocations for isort module names
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/cow
created_at: 2024-05-02T18:41:04Z
updated_at: 2024-05-02T19:27:31Z
url: https://github.com/astral-sh/ruff/pull/11251
synced_at: 2026-01-12T15:55:37Z
```

# Avoid allocations for isort module names

---

_@charliermarsh_

## Summary

Random refactor I noticed when investigating the F401 changes. We don't need to allocate in most cases here.

---

_Label `performance` added by @charliermarsh on 2024-05-02 18:42_

---

_@zanieb reviewed on 2024-05-02 18:43_

---

_Review comment by @zanieb on `crates/ruff_python_ast/src/helpers.rs`:724 on 2024-05-02 18:43_

👀 

---

_@zanieb approved on 2024-05-02 18:44_

---

_@charliermarsh reviewed on 2024-05-02 18:45_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/helpers.rs`:724 on 2024-05-02 18:45_

Is it bad

---

_@ibraheemdev reviewed on 2024-05-02 18:45_

---

_Review comment by @ibraheemdev on `crates/ruff_python_ast/src/helpers.rs`:724 on 2024-05-02 18:45_

Does it need the `as_ref` :thinking: 

---

_Merged by @charliermarsh on 2024-05-02 19:17_

---

_Closed by @charliermarsh on 2024-05-02 19:17_

---

_Branch deleted on 2024-05-02 19:17_

---

_Comment by @github-actions[bot] on 2024-05-02 19:27_

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
