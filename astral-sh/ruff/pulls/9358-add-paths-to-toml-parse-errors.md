```yaml
number: 9358
title: Add paths to toml parse errors
type: pull_request
state: merged
author: konstin
labels:
  - cli
assignees: []
merged: true
base: main
head: paths-in-toml-path-errors
created_at: 2024-01-02T16:07:53Z
updated_at: 2024-01-02T16:56:56Z
url: https://github.com/astral-sh/ruff/pull/9358
synced_at: 2026-01-12T15:55:28Z
```

# Add paths to toml parse errors

---

_@konstin_

**Summary** Previously, the information which toml file failed to parse was missing in errors.

**Before**
```console
$ ruff check /home/konsti/projects/datasett
ruff failed
  Cause: TOML parse error at line 12, column 8
   |
12 | python "=3.9.2"
   |        ^
expected `.`, `=`
```

**After**
```console
$ ruff check /home/konsti/projects/datasett
ruff failed
  Cause: Failed to parse /home/konsti/projects/datasett/datasett-0.0.1.tar.gz/datasett-0.0.1/pyproject.toml
  Cause: TOML parse error at line 12, column 8
   |
12 | python "=3.9.2"
   |        ^
expected `.`, `=`
```

I avoided pulling in `fs_err` just for this case.

---

_@zanieb approved on 2024-01-02 16:30_

---

_Comment by @github-actions[bot] on 2024-01-02 16:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-01-02 16:56_

---

_Closed by @charliermarsh on 2024-01-02 16:56_

---

_Branch deleted on 2024-01-02 16:56_

---

_Label `cli` added by @charliermarsh on 2024-01-02 16:56_

---
