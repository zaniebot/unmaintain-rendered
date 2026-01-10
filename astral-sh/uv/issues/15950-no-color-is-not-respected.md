```yaml
number: 15950
title: "`--no-color` is not respected"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-09-19T13:54:53Z
updated_at: 2025-09-29T13:38:49Z
url: https://github.com/astral-sh/uv/issues/15950
synced_at: 2026-01-10T03:23:54Z
```

# `--no-color` is not respected

---

_Issue opened by @konstin on 2025-09-19 13:54_

### Summary

Minimal reproduction: `echo boto3 | uv pip compile - --no-color`

<img width="641" height="548" alt="Image" src="https://github.com/user-attachments/assets/139d75df-ca83-48c7-960d-0572a9395098" />

This is unlike `echo boto3 | uv pip compile -p 3.13 - --color never`, which uses non-colored output.

### Platform

Ubuntu 24.04

### Version

uv 0.8.18

### Python version

Python 3.13

---

_Label `bug` added by @konstin on 2025-09-19 13:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-27 18:12_

---

_Closed by @charliermarsh on 2025-09-29 13:38_

---
