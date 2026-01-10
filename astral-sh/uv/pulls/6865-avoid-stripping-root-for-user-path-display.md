```yaml
number: 6865
title: Avoid stripping root for user path display
type: pull_request
state: merged
author: charliermarsh
labels:
  - tracing
assignees: []
merged: true
base: main
head: charlie/u
created_at: 2024-08-30T13:11:44Z
updated_at: 2024-08-30T13:50:27Z
url: https://github.com/astral-sh/uv/pull/6865
synced_at: 2026-01-10T12:53:36Z
```

# Avoid stripping root for user path display

---

_Pull request opened by @charliermarsh on 2024-08-30 13:11_

## Summary

Closes https://github.com/astral-sh/uv/issues/6840.

## Test Plan

```
❯ ~/workspace/uv/target/debug/uv pip list --verbose
DEBUG uv 0.4.0
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.3-macos-aarch64-none` at `/Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin/python3` (search path)
DEBUG Using Python 3.12.3 environment at /Users/crmarsh/.local/share/rtx/installs/python/3.12.3/bin/python3
```


---

_Marked ready for review by @charliermarsh on 2024-08-30 13:11_

---

_Label `tracing` added by @charliermarsh on 2024-08-30 13:11_

---

_@zanieb approved on 2024-08-30 13:30_

Taking the easy ones :p

---

_Comment by @charliermarsh on 2024-08-30 13:50_

Always.

---

_Merged by @charliermarsh on 2024-08-30 13:50_

---

_Closed by @charliermarsh on 2024-08-30 13:50_

---

_Branch deleted on 2024-08-30 13:50_

---
