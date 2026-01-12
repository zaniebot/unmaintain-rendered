```yaml
number: 10673
title: Log memory stats on timeout
type: pull_request
state: closed
author: konstin
labels:
  - error messages
assignees: []
draft: true
base: main
head: konsti/log-memory-on-timeout
created_at: 2025-01-16T10:56:13Z
updated_at: 2025-03-10T23:03:11Z
url: https://github.com/astral-sh/uv/pull/10673
synced_at: 2026-01-12T16:09:25Z
```

# Log memory stats on timeout

---

_@konstin_

Counter to #10672, i'm trying to log memory stats on a bytecode compilation timeout for #6105.

Not polished since this is debug output.

Example:

```
error: Failed to bytecode-compile Python file in: .venv/lib/python3.13/site-packages
  Caused by: Bytecode timed out (0.02s) compiling file: `/home/konsti/projects/uv/.venv/lib/python3.13/site-packages/plotly/graph_objs/_figurewidget.py`. Total memory: 67172139008 bytes, used memory: 20386217984 bytes, total swap: 0 bytes, used swap: 0 bytes.
```


---

_Label `error messages` added by @konstin on 2025-01-16 10:56_

---

_Comment by @konstin on 2025-03-10 23:03_

#6105 was diagnosed as a QEMU bug

---

_Closed by @konstin on 2025-03-10 23:03_

---
