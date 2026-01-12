```yaml
number: 8726
title: "Remove `pyproject.toml` from fixtures directory"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pyproject
created_at: 2023-11-16T17:32:14Z
updated_at: 2023-11-16T18:05:01Z
url: https://github.com/astral-sh/ruff/pull/8726
synced_at: 2026-01-10T23:40:55Z
```

# Remove `pyproject.toml` from fixtures directory

---

_Pull request opened by @charliermarsh on 2023-11-16 17:32_

## Summary

This exists to power a test, but it ends up affecting the behavior of all files in the directory. Namely, it means that these files _aren't_ excluded when you format or lint them directly, since in that case, Ruff will fall back to looking at the `pyproject.toml` in `crates/ruff_linter/resources/test/fixtures`, which _doesn't_ exclude these files, unlike our top-level `pyproject.toml`.



---

_Review requested from @zanieb by @charliermarsh on 2023-11-16 17:32_

---

_Label `internal` added by @charliermarsh on 2023-11-16 17:32_

---

_Comment by @github-actions[bot] on 2023-11-16 17:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2023-11-16 17:55_

<3

---

_Merged by @charliermarsh on 2023-11-16 18:04_

---

_Closed by @charliermarsh on 2023-11-16 18:04_

---

_Branch deleted on 2023-11-16 18:04_

---

_Comment by @charliermarsh on 2023-11-16 18:04_

Thanks for finding this!

---
