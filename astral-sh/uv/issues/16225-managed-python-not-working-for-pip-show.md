---
number: 16225
title: "--managed-python not working for pip show"
type: issue
state: closed
author: NorthLake
labels:
  - bug
assignees: []
created_at: 2025-10-10T09:30:29Z
updated_at: 2025-10-10T09:33:32Z
url: https://github.com/astral-sh/uv/issues/16225
synced_at: 2026-01-10T01:26:04Z
---

# --managed-python not working for pip show

---

_Issue opened by @NorthLake on 2025-10-10 09:30_

### Summary

Running `pip show` with the `--managed-python` flag set still shows packages installed in the global environment.

# Repro
1. Install a package globally using vanilla pip (`pip install pytest`)
2. Initialize a venv (`uv venv`)
3. Run pip show with the `--managed-python` flag without adding any dependencies to the venv (`uv pip show pytest --managed-python`)

# Expected result
`warning: Package(s) not found for: pytest`

# Actual result
Output shows pytest installed in the global environment

### Platform

Windows 11 24H2

### Version

uv 0.8.23 (00d3aa378 2025-10-04)

### Python version

Python 3.13.8

---

_Label `bug` added by @NorthLake on 2025-10-10 09:30_

---

_Comment by @NorthLake on 2025-10-10 09:33_

Just ran through my own repro steps and it worked... Gonna debug some more.

---

_Closed by @NorthLake on 2025-10-10 09:33_

---
