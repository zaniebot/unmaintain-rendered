```yaml
number: 1960
title: Expand scope of archive timestamping
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/setup
created_at: 2024-02-25T00:27:50Z
updated_at: 2024-02-25T00:36:47Z
url: https://github.com/astral-sh/uv/pull/1960
synced_at: 2026-01-10T14:54:43Z
```

# Expand scope of archive timestamping

---

_Pull request opened by @charliermarsh on 2024-02-25 00:27_

## Summary

Instead of looking at _either_ `pyproject.toml` or `setup.py`, we should just be conservative and take the most-recent timestamp out of `pyproject.toml`, `setup.py`, and `setup.cfg`. That will help prevent staleness issues like those described in https://github.com/astral-sh/uv/issues/1913#issuecomment-1961544084.

---

_Label `bug` added by @charliermarsh on 2024-02-25 00:28_

---

_Marked ready for review by @charliermarsh on 2024-02-25 00:28_

---

_Merged by @charliermarsh on 2024-02-25 00:36_

---

_Closed by @charliermarsh on 2024-02-25 00:36_

---

_Branch deleted on 2024-02-25 00:36_

---
