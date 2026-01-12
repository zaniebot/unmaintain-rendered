```yaml
number: 3218
title: Avoid EXE001 and EXE002 errors from stdin input
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/exe
created_at: 2023-02-24T22:52:45Z
updated_at: 2023-02-24T22:55:34Z
url: https://github.com/astral-sh/ruff/pull/3218
synced_at: 2026-01-12T15:55:12Z
```

# Avoid EXE001 and EXE002 errors from stdin input

---

_@charliermarsh_

Slightly better would be to pass in a `filename` and `filepath` separately, and only run these if we have a `filepath`. As-is, it's a bit strange because we might flag these if a corresponding file exists on disk with the same name as `--stdin-filename` -- which could be a feature, but is arguably a bug.

Closes #3214.

---

_Merged by @charliermarsh on 2023-02-24 22:55_

---

_Closed by @charliermarsh on 2023-02-24 22:55_

---

_Branch deleted on 2023-02-24 22:55_

---
