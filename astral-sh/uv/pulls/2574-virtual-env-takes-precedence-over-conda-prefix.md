```yaml
number: 2574
title: "`VIRTUAL_ENV` takes precedence over `CONDA_PREFIX`"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/venv-before-conda
created_at: 2024-03-20T19:51:58Z
updated_at: 2024-03-20T20:14:45Z
url: https://github.com/astral-sh/uv/pull/2574
synced_at: 2026-01-12T16:05:06Z
```

# `VIRTUAL_ENV` takes precedence over `CONDA_PREFIX`

---

_@konstin_

It is a common pattern to have an active conda base env (that sets `CONDA_PREFIX`) and then create a venv on top of that (setting `VIRTUAL_ENV`).

Previously, we would error when both `VIRTUAL_ENV` and `CONDA_PREFIX` were set, now `VIRTUAL_ENV` takes precedence over `CONDA_PREFIX`.

Fixes #2028

---

_Label `enhancement` added by @konstin on 2024-03-20 19:51_

---

_@charliermarsh approved on 2024-03-20 20:14_

---

_Merged by @charliermarsh on 2024-03-20 20:14_

---

_Closed by @charliermarsh on 2024-03-20 20:14_

---

_Branch deleted on 2024-03-20 20:14_

---
