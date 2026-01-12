```yaml
number: 14227
title: "`uv lock` fails when a local index points to a remote wheel"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-06-24T01:58:27Z
updated_at: 2025-06-26T20:17:44Z
url: https://github.com/astral-sh/uv/issues/14227
synced_at: 2026-01-12T16:01:45Z
```

# `uv lock` fails when a local index points to a remote wheel

---

_@charliermarsh_

The error is like: `Failed to convert URL to path`.

You can get this with Pyodide, if you do, e.g., `UV_INDEX=$(pyodide config get package_index) uv lock` and include a package like `sniffio` in your `pyproject.toml` that comes from that "local" Pyodide index.


---

_Label `bug` added by @charliermarsh on 2025-06-24 01:58_

---

_Renamed from "`uv lock` fails when a lock index points to a remote wheel" to "`uv lock` fails when a local index points to a remote wheel" by @charliermarsh on 2025-06-26 15:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-26 19:16_

---

_Closed by @charliermarsh on 2025-06-26 20:17_

---
