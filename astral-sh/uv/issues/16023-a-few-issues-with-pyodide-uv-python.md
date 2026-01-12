```yaml
number: 16023
title: A few issues with pyodide + uv python
type: issue
state: open
author: hoodmane
labels: []
assignees: []
created_at: 2025-09-25T10:35:30Z
updated_at: 2025-09-25T12:47:44Z
url: https://github.com/astral-sh/uv/issues/16023
synced_at: 2026-01-12T16:02:21Z
```

# A few issues with pyodide + uv python

---

_@hoodmane_

1. I think `uv python list` should show the Pyodide versions on linux/mac? Currently it's always hidden unless we pass `--all-platforms`.
2. `uv python list --all-platforms` lists e.g., `pyodide-3.12.7-emscripten-wasm32-musl` but trying to install this fails. The install only works with `cpython-3.12.7-emscripten-wasm32-musl`.

---

_Comment by @zanieb on 2025-09-25 12:47_

1. I don't think the Pyodide versions are considered stable enough to show by default, though I think it's reasonable to want to see them without also showing all platforms.
2. That's a bug, we can fix that.

---
