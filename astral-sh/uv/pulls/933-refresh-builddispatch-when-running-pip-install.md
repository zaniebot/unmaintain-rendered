```yaml
number: 933
title: "Refresh `BuildDispatch` when running pip install with `--reinstall`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/refresh
created_at: 2024-01-15T18:14:59Z
updated_at: 2024-01-15T18:56:19Z
url: https://github.com/astral-sh/uv/pull/933
synced_at: 2026-01-10T15:39:02Z
```

# Refresh `BuildDispatch` when running pip install with `--reinstall`

---

_Pull request opened by @charliermarsh on 2024-01-15 18:14_

## Summary

This fixes an extremely subtle bug in `pip install --reinstall`, whereby if you depend on `setuptools` at the top level, we end up uninstalling it after resolving, which breaks some cached state. If we have `--reinstall`, we need to reset that cached state between resolving and installing.

## Test Plan

Running `pip install --reinstall` with:

```txt
setuptools
devpi @ https://files.pythonhosted.org/packages/e4/f3/e334eb4dc930ccb677363e1305a3730003d203efbb023329e4b610e4515b/devpi-2.2.0.tar.gz
```

Fails on `main`, but passes.

---

_Label `bug` added by @charliermarsh on 2024-01-15 18:15_

---

_Merged by @charliermarsh on 2024-01-15 18:56_

---

_Closed by @charliermarsh on 2024-01-15 18:56_

---

_Branch deleted on 2024-01-15 18:56_

---
