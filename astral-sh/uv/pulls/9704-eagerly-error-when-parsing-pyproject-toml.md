```yaml
number: 9704
title: "Eagerly error when parsing `pyproject.toml` requirements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-12-07T14:02:56Z
updated_at: 2024-12-07T14:14:27Z
url: https://github.com/astral-sh/uv/pull/9704
synced_at: 2026-01-12T16:08:56Z
```

# Eagerly error when parsing `pyproject.toml` requirements

---

_@charliermarsh_

## Summary

Small thing I noticed while working on another change: if we error when extracting `requires-dist`, we go through the full metadata build. We need to distinguish between fatal errors and "the data isn't static".


---

_Label `performance` added by @charliermarsh on 2024-12-07 14:03_

---

_Merged by @charliermarsh on 2024-12-07 14:14_

---

_Closed by @charliermarsh on 2024-12-07 14:14_

---

_Branch deleted on 2024-12-07 14:14_

---
